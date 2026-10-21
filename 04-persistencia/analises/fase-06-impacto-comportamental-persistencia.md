# Fase 6 — Diagramas Comportamentais: Impacto sobre a Persistência e a Arquitetura

**Responsável:** Integrante 4 — Caio Cunha (Persistência e arquitetura)
**Fase do plano:** Fase 6 — Diagramas comportamentais (sequências, atividade, comunicação e máquina de estados)
**Tarefas do plano cobertas:** Revisar chamadas a repositórios, serviços externos e persistência para evitar dependências incoerentes; identificar quais mensagens exigirão suporte arquitetural ou transacional posteriormente; verificar se alterações de estado precisam de campos, histórico ou auditoria na persistência; registrar impactos do comportamento no DER e nos componentes planejados.
**Insumos:** plano §5.2 (decisões arquiteturais), §5.3 (casos críticos SEQ-001 a SEQ-004), §5.4 (máquina de estados EST-001), §6.1 (matriz de rastreabilidade), RF-001 a RF-032, RN-001 a RN-014, RNF-001 a RNF-012, `03-uml/Contexto/Interações_Processos.md`, `04-persistencia/analises/fase-04-revisao-dados-por-caso-de-uso.md`, `04-persistencia/analises/fase-05-viabilidade-persistencia-dominio.md`.

## 1. Objetivo e limitação de insumo

O comportamento é a **fonte mais reveladora de exigências de persistência**: é no diagrama de sequência que aparecem as mensagens de escrita, as transições de estado e os pontos decommit — e é aí que se descobre se a modelagem de dados suporta o fluxo ou se vai obrigar a redesenhar o banco.

**Limitação declarada:** os diagramas comportamentais são responsabilidade do Integrante 2 e ainda não foram produzidos. As pastas `03-uml/Sequências/`, `03-uml/Estados/`, `03-uml/Atividade/` e `03-uml/Comunicação/` contêm apenas o arquivo de guarda (`arquivo.md`, 2 bytes). O diagrama de contexto da Fase 2 é o único artefato comportamental existente.

Por isso, esta análise foi conduzida sobre as **fontes primárias do comportamento já definidas pelo grupo** — os casos críticos SEQ-001 a SEQ-004 do plano §5.3, a máquina de estados EST-001 do plano §5.4, as interações por ator já aprovadas na Fase 2 e os requisitos —, e **não** sobre diagramas concluídos. Consequência: o documento é **provisório e reexecutável**, e a lista de dependências incoerentes (§3.2) é exatamente o roteiro do walkthrough que o plano exige como atividade conjunta da fase ("comparar sequência × operações de classes e estados × atributo/status persistível").

## 2. Método

Quatro passes, na ordem em que o plano §8 lista as tarefas do Integrante 4:

1. **Varredura de chamadas** — inventariar, em cada sequência, toda mensagem que chega a um repositório, a um serviço externo ou à persistência, com a tabela afetada.
2. **Detecção de dependências incoerentes** — marcar as chamadas que criam acoplamento indevido, que deleguem à camada de dados a decisão de regras de negócio, ou que ameacem a atomicidade.
3. **Classificação de suporte** — separar as mensagens que exigirão **suporte transacional** (unidade de trabalho, fronteira de commit) das que exigirão **suporte arquitetural** (componentes, interfaces, gateways).
4. **Mapeamento estado ↔ persistência** — confrontar as transições de EST-001 com as colunas que o DER precisará ter, e com o que a trilha de auditoria precisa carregar.

Cada achado recebe identificador estável: `DEP-nn` (dependência incoerente), `TRS-nn` (exigência transacional), `ARQ-nn` (exigência arquitetural), `IMP-DER-nn` (impacto no DER), `IMP-CMP-nn` (impacto nos componentes), `INC-nn` (inconsistência nova) e `P-nn` (pendência que exige decisão do grupo).

## 3. Revisão de chamadas a repositórios, serviços externos e persistência

### 3.1 Inventário de chamadas por sequência

**SEQ-001 — Autenticar usuário** (plano §5.3: "segurança, bloqueio, credenciais, autorização e auditoria")

| # | Mensagem | Destinatário | Tabela | Veredito |
|---|---|---|---|---|
| 1.1 | `consultarPorLogin(login)` | Repositório de usuário | `usuario` | Coerente |
| 1.2 | `compararHash(hashArmazenado, senha)` | Serviço de autenticação (domínio) | `usuario.hash_senha` | Coerente — a comparação ocorre no serviço, não no repositório (RN-002) |
| 1.3 | `verificarSituacao(usuario)` | Repositório de usuário | `usuario.situacao` | Coerente — RN-001 exige bloqueio efetivo |
| 1.4 | `obterPermissoes(idUsuario)` | Repositório de autorização | `usuario`→`perfil`→`perfil_permissao`→`permissao` | Coerente — mas o caminho depende da cardinalidade de INC-01 |
| 1.5 | `registrarAuditoria(operacao, resultado, alvo)` | Repositório de auditoria | `auditoria` | Coerente — RN-009, CA-RF-032 |
| 1.6 | `atualizarUltimoAcesso(idUsuario)` | Repositório de usuário | `usuario` | **Ajustar** — ver TRS-01 |

**SEQ-002 — Agendar atendimento** (plano §5.3: "disponibilidade, psicólogo, paciente, regras de conflito e notificação")

| # | Mensagem | Destinatário | Tabela | Veredito |
|---|---|---|---|---|
| 2.1 | `consultarDisponibilidade(psicologo, intervalo)` | Repositório de disponibilidade | `disponibilidade` | Coerente — RN-010 |
| 2.2 | `verificarVinculo(psicologo, paciente)` | Repositório de vínculo | `vinculo_profissional` | Coerente — RN-003 |
| 2.3 | `verificarConflito(psicologo, intervalo)` | Repositório de agendamento | `agendamento` | **Incoerente** — DEP-01 |
| 2.4 | `salvar(agendamento)` | Repositório de agendamento | `agendamento` | Coerente |
| 2.5 | `marcarOcupacao(disponibilidade)` | Repositório de disponibilidade | `disponibilidade` | Coerente — mas exige coluna de ocupação (IMP-DER-03) |
| 2.6 | `notificar(destinatario, evento)` | **Serviço externo** de notificação | `notificacao` *(condicional)* | **Condicional** — DEC-009, ARQ-02, TRS-02 |
| 2.7 | `registrarAuditoria(...)` | Repositório de auditoria | `auditoria` | Coerente |

**SEQ-003 — Cancelar/confirmar agendamento** (plano §5.3: "mudança de estado, regras e notificação")

| # | Mensagem | Destinatário | Tabela | Veredito |
|---|---|---|---|---|
| 3.1 | `confirmar(agendamento)` | Entidade `Agendamento` (operação controlada) | — | Coerente — plano §5.2: `confirmar()`, `cancelar()`, `concluir()` |
| 3.2 | `cancelar(agendamento)` | Entidade `Agendamento` (operação controlada) | — | Coerente |
| 3.3 | `salvar(agendamento)` | Repositório de agendamento | `agendamento` | Coerente |
| 3.4 | `liberarOcupacao(disponibilidade)` | Repositório de disponibilidade | `disponibilidade` | Coerente — liberação, não exclusão |
| 3.5 | `notificar(...)` | **Serviço externo** de notificação | `notificacao` *(condicional)* | **Condicional** — DEC-009 |
| 3.6 | `registrarAuditoria(...)` | Repositório de auditoria | `auditoria` | Coerente — RN-015 proposta |

**SEQ-004 — Preencher encaixe** (opcional, plano §5.3): **fora do escopo desta análise** — DEC-010 pendente e INC-11 já classificou `ListaEspera`/`EntradaListaEspera` como fora do DER. Reabrir apenas se o grupo aprovar a inovação.

### 3.2 Dependências incoerentes detectadas

| ID | Dependência incoerente | Onde aparece | Por que é incoerente | Correção proposta |
|---|---|---|---|---|
| **DEP-01** | **Verificação de conflito fora da transação da reserva.** `verificarConflito` (2.3) e `salvar` (2.4) são mensagens separadas, com estado intermediário entre elas. | SEQ-002 | Duas requisições simultâneas podem validar o mesmo horário e passar ambas; a reserva real passa a depender exclusivamente do índice único do banco, e o erro de violação de unicidade do SGBD vaza para a camada de domínio — o usuário receberia uma mensagem técnica em vez de "horário indisponível" (CA-RF-025). | Disponibilizar no repositório uma operação **atômica** `reservarSeLivre(agendamento)` (IMP-CMP-03). O índice único do banco vira **última linha de defesa**, e a regra de negócio (RN-005) é decidida no domínio, com erro de domínio coerente. |
| **DEP-02** | **Serviço externo chamado dentro da transação que grava o agendamento.** `notificar` (2.6) pode ser emitida entre 2.4 e o commit. | SEQ-002, SEQ-003 | Se o commit falhar depois do envio, o paciente recebe aviso de um agendamento que não existe. Se o serviço externo estiver lento ou indisponível, ele segura a transação e derruba a disponibilidade da agenda (RNF-006). | Notificação **pós-commit** (ARQ-02), com mecanismo de saída (*outbox*) para não perder o evento. Decisão de projeto pendente em P-15. |
| **DEP-03** | **Auditoria acoplada ao repositório de auditoria.** `registrarAuditoria` (1.5, 2.7, 3.6) implica que o repositório de auditoria resolva o ator. | Todas | O repositório de auditoria passa a depender do repositório de usuário, criando acoplamento desnecessário e permitindo que a auditoria carregue dado além do mínimo de CA-RF-032. | O serviço de auditoria deve receber **o identificador do ator como parâmetro**, e o repositório grava a FK. A validação da existência do usuário fica no banco (FK com `ON DELETE RESTRICT`). |
| **DEP-04** | **Leitura de paciente a partir do agendamento.** O fluxo de consulta da agenda do psicólogo (UC-FUT-029, RF-029) precisa do paciente associado ao agendamento. | SEQ-002/SEQ-003, RF-029 | Se o acesso ao paciente for obtido por `agendamento → paciente`, o vínculo profissional-paciente (RN-003) deixa de ser o **filtro** e passa a ser apenas um **dado**. Isso abre brecha de listagem indevida de pacientes, exatamente o que a Fase 4 (§6) marcou como risco. | Toda consulta de paciente por profissional deve passar **obrigatoriamente** por `vinculo_profissional`. Em arquitetura, isso significa um componente de consulta que só resolve paciente via vínculo — nunca via agendamento. |
| **DEP-05** | **Leitura direta da coluna `situacao` por fora do domínio.** O estado do agendamento é lido por telas e relatórios. | SEQ-003, RF-028, RF-029 | O plano §5.2 exige encapsular a mudança de estado por operações controladas. Se a situação é lida direto do banco, consumidores externos passam a conhecer o **domínio de valores** (INC-01, P-01, P-02) e a lógica de transição se duplica fora da entidade. | Leitura por método da entidade ou por visão/projeção read-only que expõe o estado já no vocabulário de negócio. A coluna continua existindo — o que muda é quem pode interpretá-la. |
| **DEP-06** | **Ciclo de dependência entre domínio e auditoria.** O serviço de auditoria precisa do nome do usuário para descrever a operação, e o repositório de usuário chama o serviço de auditoria. | SEQ-001 | Dependência circular clássica: `usuario → auditoria → usuario`. | O registro de auditoria recebe **identificadores**, nunca objetos de entidade. A composição do texto legível (quem foi, em que formato) fica na camada de aplicação, acima dos dois. |

### 3.3 Conclusão da revisão

Das **19 chamadas inventariadas**, 13 são coerentes com o modelo, 2 exigem ajuste estrutural (DEP-01, DEP-04), 3 dependem de decisão pendente do grupo (notificação, auditoria de falha) e 1 viola encapsulamento (DEP-05). Nenhuma chamada exige **tabela nova** por si só — mas três exigem **coluna nova** em tabela já existente (IMP-DER-01, IMP-DER-03 e, condicionalmente, IMP-DER-07). Essa é a informação que a Fase 7 vai consumir.

## 4. Mensagens que exigirão suporte arquitetural ou transacional

### 4.1 Exigências transacionais

| ID | Sequência / mensagem | Fronteira transacional | Atomicidade exigida | Risco se ignorada |
|---|---|---|---|---|
| **TRS-01** | SEQ-001 · 1.5 e 1.6 — `registrarAuditoria` + `atualizarUltimoAcesso` | **Transação A (sucesso):** leitura do usuário + auditoria + carimbo de último acesso indivisíveis | A auditoria da tentativa bem-sucedida e a alteração do usuário precisam ser indivisíveis (RN-015 proposta) | Sessão registrada sem rastro de auditoria, ou carimbo atualizado sem registro — quebra RNF-004 |
| **TRS-02** | SEQ-002 · 2.6 — `notificar` | **FORA** da transação de escrita (pós-commit) | A notificação não pode participar da transação que grava o agendamento | Aviso falso ao paciente e indisponibilidade da agenda por dependência de serviço externo (DEP-02) |
| **TRS-03** | SEQ-002 · 2.4 + 2.5 + 2.7 — `salvar` + `marcarOcupacao` + `registrarAuditoria` | **Transação B: uma única transação** para as três escritas | Agendamento, ocupação e auditoria indivisíveis (RN-015) | Horário ocupado sem agendamento, ou agendamento sem rastro; em ambos os casos, estado inconsistente que nenhum componente consegue explicar |
| **TRS-04** | SEQ-003 · 3.3 + 3.4 + 3.6 — `salvar` + `liberarOcupacao` + `registrarAuditoria` | **Transação C: uma única transação** | Situação, liberação e auditoria indivisíveis (RN-015) | Horário liberado enquanto o agendamento segue "Confirmado", permitindo **duplo agendamento** — a violação mais grave de RN-005 |
| **TRS-05** | SEQ-001 · falha de autenticação (caminho de exceção) | **Transação D: independente** da transação de negócio | A auditoria da **falha** precisa ser gravada mesmo quando a transação de negócio é revertida | O registro de tentativa falha some junto com o rollback — e é justamente a falha que mais interessa à RNF-004 (tentativa de acesso indevido) |
| **TRS-06** | RF-024 / UC-FUT-024 — consulta de horários disponíveis | Somente leitura | Sem transação explícita, mas a leitura deve ser **consistente** (snapshot) no momento em que a reserva é tentada | O paciente vê um horário que, ao agendar, já foi ocupado — o efeito oposto de RN-010 |

**Padrão transacional resultante:** quatro fronteiras bem definidas — **A** (autenticação bem-sucedida), **B** (reserva), **C** (confirmar/cancelar), **D** (auditoria de falha, autocommit). Nenhuma delas deve ser decidida ad hoc dentro de um repositório: a fronteira precisa ser um **componente de unidade de trabalho** (IMP-CMP-02), porque as três escritas de TRS-03 e TRS-04 atravessam repositórios diferentes.

### 4.2 Exigências arquiteturais

| ID | Necessidade | Origem | Componente/interface requerida | Decisão do grupo |
|---|---|---|---|---|
| **ARQ-01** | Repositórios como **interfaces** no domínio, implementações na infraestrutura | DEP-01, DEP-03, DEP-06; plano §15 (DIP) | `IRepositorioUsuario`, `IRepositorioAgendamento`, `IRepositorioDisponibilidade`, `IRepositorioVinculo`, `IRepositorioAuditoria`, `IRepositorioAutorizacao` | — |
| **ARQ-02** | **Gateway** de notificação, com implementação externa | SEQ-002/003 · 2.6, 3.5; DEC-009 pendente | `IGatewayNotificacao` (interface) + adaptadores: e-mail, interno | **Pendente** — DEC-009. A modelagem como interface permite decidir depois sem redesenhar |
| **ARQ-03** | Unidade de trabalho / gerenciador transacional | TRS-01, TRS-03, TRS-04 | `IUnidadeTrabalho` na camada de aplicação | — |
| **ARQ-04** | Serviço de auditoria isolado, acionado por todos os casos de escrita | RN-009, CA-RF-032, DEP-03, DEP-06 | `IServicoAuditoria` | — |
| **ARQ-05** | Serviço de autorização, reavaliado por operação | RN-003, RN-004, RNF-002 | `IServicoAutorizacao` | Depende da cardinalidade **INC-01** (1:1 ou N:N entre usuário e perfil) |
| **ARQ-06** | **Serviço de tempo e fuso horário** | RN-012 (prazo de cancelamento), RN-005/RN-010 (comparação de intervalos), INC-05 | `IServicoTempo` | Depende da definição do modelo de tempo (**P-05**) |
| **ARQ-07** | Mecanismo de saída (*outbox*) para eventos pós-commit | TRS-02, DEP-02 | `IRepositorioOutbox` | — |
| **ARQ-08** | Componente de consulta que resolve paciente **somente** via vínculo | DEP-04, RN-003 | `IConsultaPacientesVinculados` | — |

**Observação de projeto:** ARQ-02, ARQ-05 e ARQ-06 estão bloqueados por decisões do grupo (DEC-009, INC-01, P-05). A análise mostra que **none** deles é reason for blocking the Fase 7: todos são modelados como interfaces com implementação pendente, o que preserva o princípio da inversão de dependência e permite fechar o DER sem esperar as decisões.

## 5. Alterações de estado: campos, histórico e auditoria

### 5.1 Máquina de estados EST-001 × persistência

O plano §5.4 define o ciclo: `Solicitado → Confirmado → Realizado`, com saídas para `Cancelado` e `Não compareceu`, e exige que *"os mesmos estados devem aparecer no domínio, nos casos e no banco"*. Esta é a atividade conjunta da fase (estado × atributo/status persistível), e é o ponto de maior risco de divergência do projeto.

| Transição | Evento | Condição | Ação | Coluna nova? | Histórico? | Auditoria? |
|---|---|---|---|---|---|---|
| (inicial) → **Solicitado** | `solicitar()` | RN-010 (horário disponível), RN-003 (vínculo), RN-005 (sem conflito) | Reserva o horário | `situacao` (default `SOLICITADO`) | Não — é o estado inicial | **Sim** (RN-009) |
| Solicitado → **Confirmado** | `confirmar()` | RN-013 (ator e prazo de confirmação) | Mantém a ocupação | **`data_confirmacao`** | **Sim** | **Sim** |
| Solicitado → **Cancelado** | `cancelar()` | RN-012 (prazo, situação cancelável, ator) | Libera a disponibilidade | **`data_cancelamento`**, **`id_usuario_cancelamento`** | **Sim** | **Sim** |
| Confirmado → **Cancelado** | `cancelar()` | RN-012 | Libera a disponibilidade | **`data_cancelamento`**, **`id_usuario_cancelamento`** | **Sim** | **Sim** |
| Confirmado → **Realizado** | `concluir()` | RN-007 (vínculo válido); cria o atendimento | Cria `atendimento` | **`data_realizacao`** | **Sim** | **Sim** |
| Confirmado → **Não compareceu** | `registrarFalta()` | Prazo (a definir) | Libera a disponibilidade | **`data_nao_comparecimento`** | **Sim** | **Sim** |
| Solicitado → **Não compareceu** | — | — | — | — | — | **Indefinido — INC-13** |

### 5.2 Achado central: a coluna `situacao` sozinha não responde "quando" e "quem"

Este é o achado de maior consequência da Fase 6 para o DER, e **confirma e amplia a INC-07** levantada na Fase 5.

A auditoria, conforme RNF-004 e CA-RF-032, carrega `{usuário responsável, data, hora, operação}`. Taken isoladamente, esse conjunto responde *"quem fez o quê, quando"* — mas **não** responde *"quando este agendamento foi confirmado"*, *"quando foi cancelado"*, *"há quanto tempo está pendente"*, *"quem cancelou e com que justificativa operacional"*.

Isso importa porque três regras do escopo **dependem do tempo**, e nenhuma delas pode ser avaliada olhando apenas a auditoria:

- **RN-012** — cancelamento só é permitido dentro de prazo. Precisa da data do cancelamento **e** do estado anterior.
- **RN-013** — confirmação depende de prazo. Precisa da data da solicitação **e** da data da confirmação.
- **RN-005 / RN-010** — conflito e disponibilidade dependem da situação atual **e** do tempo já decorrido.

Se a única fonte temporal fosse a auditoria, cada consulta dessas regras exigiria varredura da trilha — o que é lento, frágil (a trilha é append-only e cresce sem limite, RN-014) e semanticamente errado (a auditoria registra *operações*, não *estado corrente*).

**Conclusão e proposta:** o agendamento precisa de **colunas de data por transição**, e a auditoria precisa carregar o **valor anterior e o novo** da situação.

| Elemento | Onde | Origem |
|---|---|---|
| `data_confirmacao`, `data_cancelamento`, `data_realizacao`, `data_nao_comparecimento` | `agendamento` | IMP-DER-01, desta seção |
| `id_usuario_cancelamento` | `agendamento` | RN-012 (o cancelamento precisa de responsável identificável, além do usuário da auditoria) |
| `situacao_anterior`, `situacao_nova` | `auditoria` | RNF-019 proposta na Fase 5, reforçada aqui |

### 5.3 Histórico: histórico de situação ou não?

Duas alternativas foram avaliadas contra as restrições do projeto:

| Alternativa | Descrição | Avaliação |
|---|---|---|
| **A — Colunas de data + auditoria com valor anterior/novo** (recomendada) | `agendamento` guarda a data de cada transição; `auditoria` guarda o par situação anterior/situação nova | Atende RN-009, RN-012 e RN-013 com **consultas O(1)** sobre a entidade; a trilha permanece append-only; **não exige tabela nova**; o DER fica menor |
| **B — Tabela `historico_situacao`** | Uma linha por transição, com data, de, para, responsável | Mais fiel a um "histórico" completo, e útil se o grupo quiser reconstruir a linha do tempo; porém **cria uma tabela nova** (IMP-DER-08) e duplica o que a auditoria já registra, com risco de divergência entre as duas fontes |

**Recomendação: alternativa A**, com a B reservada para o caso de o grupo exigir reconstrução de linha do tempo por atendimento. A decisão cabe ao Integrante 1 e ao Integrante 3 (modelo de classes), e está registrada em **P-18**.

### 5.4 Auditoria por transição

Todas as seis transições da tabela 5.1 geram auditoria (RN-009). A gravação segue RN-015 (proposta da Fase 4): **mudança de situação e registro de auditoria na mesma transação**. Um caso especial merece registro: a transição `Confirmado → Realizado` cria, além da auditoria, a entidade `atendimento` — portanto a transação crítica do sistema (TRS-04) grava **três** artefatos: `agendamento.situacao`, `auditoria` e `atendimento`.

## 6. Impactos do comportamento no DER e nos componentes planejados

### 6.1 Impactos no DER (DER-001)

| ID | Impacto | Tabela | Efeito | Origem |
|---|---|---|---|---|
| **IMP-DER-01** | Colunas de data por transição + responsável pelo cancelamento | `agendamento` | `+4 colunas` de data/hora, `+1 FK` para `usuario` | §5.1, §5.2 |
| **IMP-DER-02** | Domínio de valores de `situacao` deve coincidir **exatamente** com EST-001 | `agendamento` | CHECK com 5 a 6 valores | §5.1; P-01, P-02, INC-13 |
| **IMP-DER-03** | Necessidade de representar a **ocupação** da disponibilidade | `disponibilidade` | `+data_ocupacao` **ou** derivação por `agendamento.id_disponibilidade` + situação | 2.5, 3.4; INC-06 |
| **IMP-DER-04** | FK de `atendimento → agendamento` deve ser **NOT NULL** e `ON DELETE RESTRICT` | `atendimento` | Restrição de integridade | RN-017 proposta; criação em `concluir()` |
| **IMP-DER-05** | `auditoria` ganha `situacao_anterior` e `situacao_nova` | `auditoria` | `+2 colunas` | RNF-019; §5.2 |
| **IMP-DER-06** | `vinculo_profissional` precisa ser consultável **por tempo** | `vinculo_profissional` | `+data_inicio`, `+data_fim`, `+situacao` | RNF-020; INC-04, P-12 |
| **IMP-DER-07** | Se a sessão for entidade persistida, precisa de situação e revogação | `sessao` *(condicional)* | `+situacao`, `+data_expiracao` | 1.6, RF-002, RF-006; P-07, P-17 |
| **IMP-DER-08** | Alternativa B de §5.3 criaria `historico_situacao` | *(nova tabela)* | Não recomendado — ver §5.3 | P-18 |
| **IMP-DER-09** | Nenhuma tabela nova para notificação enquanto DEC-009 não for decidido | — | Fronteira modelada como **interface** | §4.2, ARQ-02 |
| **IMP-DER-10** | Nenhuma tabela de inovação enquanto DEC-010 não for decidido | — | `lista_espera` e `entrada_lista_espera` fora do DER | SEQ-004; INC-11 |

### 6.2 Impactos nos componentes planejados

| ID | Componente/interface requerida | Tipo | Origem |
|---|---|---|---|
| **IMP-CMP-01** | `IGatewayNotificacao` (domínio) + adaptadores externos (infraestrutura) | Interface | ARQ-02, IMP-DER-09 |
| **IMP-CMP-02** | `IUnidadeTrabalho` — gerenciador transacional na camada de aplicação | Componente | ARQ-03, TRS-01/03/04 |
| **IMP-CMP-03** | `IRepositorioAgendamento.reservarSeLivre(...)` — reserva atômica | Interface | DEP-01, RN-005 |
| **IMP-CMP-04** | `IServicoAuditoria` — isolado, recebe **ids**, nunca entidades | Interface | ARQ-04, DEP-03, DEP-06 |
| **IMP-CMP-05** | `IServicoTempo` — tempo e fuso, fora do SGBD | Interface | ARQ-06, INC-05, P-05 |
| **IMP-CMP-06** | `IServicoAutorizacao` — permissão reavaliada por operação | Interface | ARQ-05, INC-01 |
| **IMP-CMP-07** | `IConsultaPacientesVinculados` — paciente só via vínculo | Componente | ARQ-08, DEP-04, RN-003 |
| **IMP-CMP-08** | `IRepositorioOutbox` — eventos para notificação pós-commit | Interface | ARQ-07, TRS-02, DEP-02 |
| **IMP-CMP-09** | `IServicoAgendamento` — dono das transições `confirmar()`, `cancelar()`, `concluir()` | Componente | plano §5.2; §5.1 |

### 6.3 Comportamento que **não** impacta a persistência

Registrado para evitar retrabalho: o diagrama de atividades (ATV-001) e o diagrama de comunicação (COM-001) descrevem **fluxo de controle entre objetos**, não estado persistente. Nenhum dos dois, por si só, cria tabela, coluna ou restrição. Eles impactam a persistência apenas se revelarem uma transição de estado não prevista — o que restará como verificação no walkthrough.

## 7. Pendências geradas pela Fase 6

| ID | Pendência | Origem | Impacto no DER | Decisão de |
|---|---|---|---|---|
| **P-13** | Quem pode registrar "Não compareceu" e em que prazo | §5.1 | Coluna `data_nao_comparecimento`, domínio de `situacao` | Grupo |
| **P-14** | Se "Não compareceu" é alcançável a partir de "Solicitado" | INC-13 | Domínio de `situacao`, diagrama de estados | Grupo |
| **P-15** | Notificação síncrona (na transação) ou assíncrona (pós-commit) | TRS-02, DEP-02 | Presença de `outbox`; `notificacao` | Grupo |
| **P-16** | Auditoria de falha de autenticação em transação independente | TRS-05 | Nenhum (comportamento), mas afeta RNF-004 | Grupo |
| **P-17** | Sessão é entidade persistida — coerente com o revogamento de sessão em RF-006 | IMP-DER-07 | Presença de `sessao` (coerente com P-07) | Grupo |
| **P-18** | Histórico de situação: colunas na entidade + auditoria enriquecida, ou tabela `historico_situacao` | §5.3 | Alternativa A (recomendada) ou B | Grupo / Integrante 3 |
| **P-19** | O cliente deve fornecer o carimbo de tempo das operações de escrita, ou o servidor? | TRS-01..TRS-04, RNF-004 | Colunas de data com origem definida | Grupo |

## 8. Inconsistência nova (INC-13)

**INC-13 — Caminho `Solicitado → Não compareceu` não definido.** O plano §5.4 lista como "saídas possíveis" `Cancelado` e `Não compareceu`, mas não diz de qual estado a não comparecimento é alcançável. Semanticamente, "não compareceu" pressupõe que a sessão **existiu e foi reservada** — o que corresponde a `Confirmado`. Permitir a partir de `Solicitado` admitiria que um pedido nunca confirmado vire ausência, o que é semanticamente distinto (seria "não solicitou"). Impacto: define o domínio de valores de `situacao` e as guardas de transição. **Proposta:** permitir `Não compareceu` **apenas** a partir de `Confirmado`, e registrar o cancelamento do `Solicitado` como `Cancelado`.

## 9. Rastreabilidade deste documento

| Seção | Origem |
|---|---|
| Inventário de chamadas | plano §5.3 (SEQ-001 a SEQ-004), RF-001, RF-002, RF-025 a RF-027, RF-032, RN-001 a RN-003, RN-005, RN-009 a RN-013, `03-uml/Contexto/Interações_Processos.md` |
| DEP-01 a DEP-06 | Tarefas do plano §8 (Fase 6, Integrante 4), plano §5.2, §15, DEP-01/03/04/05/06 em INC-01, INC-03, INC-05, INC-06, RN-003, RN-005 |
| TRS-01 a TRS-06 | RN-015 (proposta, Fase 4), RNF-004, RN-009, CA-RF-025, CA-RF-032, INC-02, INC-03 |
| ARQ-01 a ARQ-08 | plano §5.2, §15 (SOLID/DIP), DEC-009, INC-01, INC-05, RN-003, RN-004, RN-009 |
| §5.1 e §5.2 | plano §5.4 (EST-001), RN-009, RN-012, RN-013, RN-014, RNF-004, CA-RF-020, CA-RF-032, INC-07, RNF-019 (proposta) |
| §5.3 e §5.4 | INC-07, RNF-019, RN-014, P-01, P-02 |
| IMP-DER-01 a IMP-DER-10 | §3.1, §4.2, §5.1, §5.2, INC-04, INC-06, INC-11, RNF-020, RN-017 (proposta) |
| IMP-CMP-01 a IMP-CMP-09 | §3.2, §4.1, §4.2, §5.1 |
| INC-13 e P-13 a P-19 | plano §5.4, RF-002, RF-006, RF-023 obs., RF-026, RF-027, RN-012, RN-013 |
