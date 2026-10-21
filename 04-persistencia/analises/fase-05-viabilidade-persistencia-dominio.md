# Fase 5 — Modelo de Domínio: Viabilidade de Persistência

**Responsável:** Integrante 4 — Caio Cunha (Persistência e arquitetura)
**Fase do plano:** Fase 5 — Modelo de domínio, classes e objetos
**Tarefas do plano cobertas:** Revisar multiplicidades e associações pensando na viabilidade de persistência; identificar quais classes deverão ser persistentes e quais são apenas serviços/estruturas de apoio; marcar relacionamentos N:N, composição e herança que exigirão estratégia de mapeamento; apontar inconsistências entre multiplicidade de domínio e regras de negócio antes do DER.
**Insumos:** glossário do domínio, RF-001 a RF-032, RN-001 a RN-014, RNF-001 a RNF-012, plano §5.1 (modelo de domínio inicial), plano §5.2 (decisões arquiteturais), plano §10 (persistência e mapeamento).

## 1. Objetivo e limitação de insumo

O plano é explícito: *"O banco não deve ser criado de forma independente. O DER deve ser derivado do domínio e as cardinalidades precisam refletir as multiplicidades das associações."* Esta revisão existe para **verificar a viabilidade de persistência do modelo de domínio antes de o DER ser desenhado**, e para registrar onde multiplicidade e regra de negócio ainda divergem.

**Limitação declarada:** o diagrama de classes e o de objetos são responsabilidade do Integrante 3 e ainda não foram produzidos (`03-uml/` não possui as pastas `Classes` e `Objetos` da estrutura recomendada no plano §12). A análise abaixo foi conduzida sobre a **lista de classes do plano §5.1**, sobre o glossário e sobre os requisitos, conforme o próprio plano recomenda para o §5.1: *"A lista abaixo é uma proposta de ponto de partida. Ela deve ser revisada pelo grupo com base nos requisitos finais; não deve ser copiada mecanicamente."*

**Consequência:** a análise é **provisória** e precisa ser reconciliada com o CLS-001 aprovado. As inconsistências INC-01 a INC-12 são o mecanismo concreto dessa reconciliação: cada uma aponta uma decisão que precisa ser fechada e que muda o DER.

## 2. Classificação das classes: persistentes × estruturas de apoio

| Classe | Origem | Papel | Persistente? | Justificativa | Tabela candidata |
|---|---|---|---|---|---|
| Usuario | §5.1, glossário | Identidade, autenticação e estado da conta | **Sim** | Entidade central; PRE-001 exige identificação própria | `usuario` |
| Perfil | §5.1, glossário | Agrupamento de responsabilidades e permissões | **Sim** | Entidade referenciada por usuários | `perfil` |
| Permissao | §5.1, glossário | Autorização granular | **Sim** | Catálogo referenciado por perfis | `permissao` |
| Psicologo | §5.1, glossário | Dados profissionais, CRP, áreas de atuação e agenda | **Sim** | Entidade de domínio com ciclo de vida próprio | `psicologo` |
| AreaAtuacao | §5.1, glossário | Área de especialidade do profissional | **Condicional** | Depende da decisão sobre catálogo de áreas (INC-09) | `area_atuacao` |
| Paciente | §5.1, glossário | Cadastro administrativo do paciente | **Sim** | Entidade central do módulo de pacientes | `paciente` |
| VinculoProfissional | §5.1, glossário | Relação autorizada entre psicólogo e paciente | **Sim** | Associação N:N com atributos próprios (temporalidade, situação) | `vinculo_profissional` |
| Disponibilidade | §5.1, glossário | Intervalo oferecido pelo psicólogo | **Sim** | Entidade que ocupa a agenda | `disponibilidade` |
| Agendamento | §5.1, glossário | Reserva de horário, com situação controlada | **Sim** | Entidade de estado central; foco da máquina de estados EST-001 | `agendamento` |
| Atendimento | §5.1, glossário | Registro conceitual da sessão realizada | **Sim** | Entidade referenciada pelo histórico | `atendimento` |
| Auditoria | §5.1, glossário | Trilha cronológica e imutável | **Sim** | Entidade exigida por RN-009, RN-014, RNF-004 | `auditoria` |
| Notificacao | §5.1, glossário | Aviso decorrente de evento do sistema | **Condicional** | Depende de DEC-009 (INC-10) | `notificacao` |
| ListaEspera | §5.1 | Candidatos a encaixe | **Não — fora do escopo** | DEC-010 pendente; nenhum RF de inovação existe (INC-11) | — |
| EntradaListaEspera | §5.1 | Solicitação individual de preferência | **Não — fora do escopo** | DEC-010 pendente (INC-11) | — |
| Sessao | não listada no §5.1 | Estado de autenticação entre requisições | **Condicional** | Depende de RF-002 obs. (P-07) | `sessao` |
| Serviços de domínio (autenticação, autorização, auditoria, notificação, agenda, atendimento) | §5.2, §15 | Regras de negócio; exemplo do plano §15: *"Agendamento controla ciclo de vida; NotificacaoService envia avisos"* | **Não** | Comportamento sem estado próprio; o estado é das entidades | — |
| Repositórios e gateways | §5.2, §15 (DIP) | Acesso a dados e a serviços externos | **Não** | Interfaces; a implementação concreta fica na camada de infraestrutura | — |

**Conclusão da classificação.** O domínio produz **11 entidades persistentes**, **3 condicionais** e um conjunto de serviços e repositórios que não geram tabela. A separação entre entidade e serviço segue o princípio SRP aplicado pelo plano §15: `Agendamento` controla o ciclo de vida; `ServicoNotificacao` envia avisos; `ServicoAuditoria` registra.

## 3. Multiplicidades × cardinalidades

Multiplicidade de domínio (esquerda) e cardinalidade relacional resultante (direita), com a viabilidade avaliada.

| Associação | Multiplicidade de domínio | Cardinalidade relacional | Viabilidade | Observação |
|---|---|---|---|---|
| Usuario → Perfil | 1..1 (plano §6.1) | FK `usuario.id_perfil` | **Instável** | O plural de RF-003 ("associe os perfis autorizados") sugere N:N. Ver INC-01 |
| Perfil ↔ Permissao | N:N | Tabela associativa `perfil_permissao` | Viável | Exatamente o previsto no plano §10.1 |
| Psicologo ↔ AreaAtuacao | N:N | Tabela associativa `psicologo_area_atuacao` | Viável | Previsto no plano §10.1. Catálogo pendente (INC-09) |
| Psicologo ↔ Paciente | N:N, via VinculoProfissional | Tabela associativa `vinculo_profissional` | Viável | Previsto no plano §5.1. Temporalidade pendente (INC-04) |
| Psicologo → Disponibilidade | 1..* | FK `disponibilidade.id_psicologo` | Viável | RN-011 determina a posse |
| Disponibilidade → Agendamento | 0..1 **ativo** | FK `agendamento.id_disponibilidade` | Viável com ressalva | Depende da definição de "ativo" (INC-06) e do modelo de tempo (INC-05) |
| Agendamento → Psicologo | 1 | FK `agendamento.id_psicologo` | **Instável** | Redundante com o caminho via disponibilidade (INC-02) |
| Agendamento → Paciente | 1 | FK `agendamento.id_paciente` | Viável | Sem alternativa no modelo |
| Agendamento → Disponibilidade | 0..1 | FK `agendamento.id_disponibilidade` | Viável | Necessária para a RN-018 proposta |
| Agendamento → Atendimento | 0..1 | FK `atendimento.id_agendamento` | Viável | Composição parcial; protege o cancelamento (RN-006, RN-017) |
| Atendimento → Psicologo | 1 | FK `atendimento.id_psicologo` | Viável | Em par com o paciente (INC-03) |
| Atendimento → Paciente | 1 | FK `atendimento.id_paciente` | Viável | Em par com o psicólogo (INC-03) |
| Paciente → Usuario | 0..1 | FK `paciente.id_usuario` UNIQUE | **Instável** | Depende de INC-12 |
| Psicologo → Usuario | 0..1 | FK `psicologo.id_usuario` UNIQUE | **Instável** | Depende de INC-12 |
| Usuario → Auditoria | 1..* | FK `auditoria.id_usuario` | Viável | Com `ON DELETE RESTRICT` (INC-08) |
| Agendamento → Situação | atributo | Coluna `situacao` com domínio fechado | Viável | Valores devem coincidir com EST-001 |

## 4. Relações N:N, composição e herança

### 4.1 Relações muitos-para-muitos

O plano §10.1 é taxativo: *"Qualquer N:N deve ser explicitamente resolvida no modelo relacional."* As três relações N:N do modelo e suas resoluções:

| N:N | Tabela associativa | Atributos próprios da associação | Justificativa |
|---|---|---|---|
| Perfil ↔ Permissao | `perfil_permissao` | — | Repo §10.1 |
| Psicologo ↔ AreaAtuacao | `psicologo_area_atuacao` | — | Repo §10.1 |
| Psicologo ↔ Paciente | `vinculo_profissional` | `data_inicio`, `data_fim`, `situacao` | O vínculo não é um par simples: é a **autorização** que RN-003 impõe e que precisa ser datada (INC-04) |

A terceira merece destaque: `vinculo_profissional` é a **única** tabela associativa do modelo que carrega atributos, e é a que materializa RN-003. Ao registrar histórico de atendimento, ela precisa ser consultável por tempo — por isso a temporalidade proposta em RNF-020.

### 4.2 Composições

| Relação | Tipo | Consequência para a persistência |
|---|---|---|
| Agendamento → Disponibilidade | **Associação simples**, não composição | A disponibilidade é recurso do psicólogo e sobrevive a seus agendamentos; quando o agendamento é cancelado, a disponibilidade é liberada, não destruída. FK sem cascata de exclusão. |
| Agendamento → Atendimento | **Composição parcial** (0..1) | O atendimento é criado e destruído junto ao ciclo do agendamento, mas o modelo exige que o cancelamento impeça a criação do atendimento (RN-006). FK `atendimento.id_agendamento` com `ON DELETE RESTRICT`: a exclusão do agendamento não pode apagar a trilha de atendimento. |
| Usuario → Perfil / Permissao | **Agregação fraca** | O perfil é um catálogo compartilhado; a associação se resolve por tabela associativa, sem ciclo de vida dependente. |

### 4.3 Herança

O plano §5.2 adverte: *"Evitar criar Administrador, Psicólogo e Atendente como subclasses de Usuario apenas para representar permissões. Herança só deve existir quando houver relação 'é um' real e comportamento especializado."* E o §5.1 reforça: *"Manter Psicologo como conceito próprio de domínio, pois possui CRP, área de atuação, agenda e vínculos profissionais."*

**Recomendação de estratégia de mapeamento:**

| Cenário | Estratégia | Justificativa |
|---|---|---|
| **Recomendado:** `Psicologo` e `Paciente` como entidades independentes, com FK opcional para `Usuario` | Sem herança; associação 1..1 opcional | Coerente com §5.2 e com CA-RF-014 ("sem duplicar desnecessariamente a identidade de usuário"). O psicólogo existe no domínio mesmo sem conta de acesso. Ver INC-12 |
| Alternativo: subclasses de `Usuario` com herança | **TPT (Table Per Type)** — tabela `usuario` com PK compartilhada e tabelas filhas com FK para a PK do pai | Aceitável **somente** se o grupo registrar DEC confirmando que existe relação "é um" real e comportamento especializado. Nesse caso, `psicologo` e `paciente` herdariam `id_usuario` como PK e FK |

**Interação entre herança e INC-12.** A escolha tem consequência prática: se `Psicologo` herdar de `Usuario`, a FK opcional deixa de existir e o Professional é obrigatoriamente um usuário. Se `Psicologo` for entidade independente com FK opcional, o profissional pode existir no cadastro sem conta — o que é coerente com RF-014 e RF-015, onde "perfil profissional" e "conta de acesso" aparecem como conceitos relacionados mas não idênticos.

**Conclusão da classificação.** Não há, no modelo atual, nenhuma relação "é um" que justifique herança. A herança de `Usuario` seria usada apenas para representar permissões, exatamente o que o plano §5.2 desaconselha.

## 5. Inconsistências entre multiplicidade de domínio e regras de negócio

São os pontos que precisam ser fechados **antes** do DER. Cada um altera cardinalidade, chave ou restrição do modelo relacional.

### INC-01 — Plural de perfis em RF-003 contra o modelo linear Usuario→Perfil→Permissao

- **Evidência:** RF-003 diz *"associe os perfis autorizados"* (plural). O glossário define Perfil como agrupamento *"atribuíveis aos usuários"*. O plano §6.1 exemplifica `Usuario → Perfil → Permissao` (1:N). O plano §10.1 resolve o N:N `Perfil ↔ Permissao` e **não** menciona `Usuario ↔ Perfil`.
- **Impacto:** 1:1 (`usuario.id_perfil` com UNIQUE) ou N:N (`usuario_perfil`). Muda a cardinalidade, a FK, o caminho de verificação de autorização do RF-001 e a própria quantidade de tabelas.
- **Proposta:** decidir. Se o modelo for 1 perfil por usuário, fica alinhado ao §5.2 (separar identidade de autorização) e mais simples. O plural de RF-003 pode ser lido como plural genérico.

### INC-02 — Redundância do psicólogo dentro do agendamento

- **Evidência:** RN-011 — a disponibilidade pertence à agenda do próprio psicólogo. RN-005 — exclusividade de horário **de um psicólogo**. RN-010 — solicitação apenas em horário disponível. Logo o psicólogo do agendamento é **derivável** pelo caminho `agendamento → disponibilidade → psicólogo`. Mesmo assim, o plano §10 exemplifica `Agendamento.psicologo → agendamento.id_psicologo - FK`, e RF-025 exige registrar *"Cliente/Paciente, Psicólogo, horário e situação"*.
- **Impacto:** duas FKs que apontam para o mesmo psicólogo, com risco real de divergência — o agendamento apontaria para um profissional e a disponibilidade para outro.
- **Proposta:** duas saídas. (a) Manter ambas e impor FK composta `(id_disponibilidade, id_psicologo)` referenciando uma chave única de `disponibilidade`, o que torna a divergência impossível. (b) Remover `id_psicologo` do agendamento e resolver por junção. A (a) preserva a leitura direta exigida por RF-025; a (b) elimina a redundância. **Decisão do grupo.**

### INC-03 — Par (psicólogo, paciente) no atendimento contra a exigência de vínculo

- **Evidência:** RN-007 — atendimento associado a psicólogo e paciente *"válidos, com vínculo autorizado"*. RN-003 — o psicólogo só acessa paciente vinculado. RF-030 e CA-RF-030 repetem a exigência.
- **Impacto:** se o atendimento referencia psicólogo e paciente por FKs independentes, **nada impede gravar um atendimento de um profissional a um paciente sem vínculo**. As duas FKs ficam sem autoridade.
- **Proposta:** FK composta `(id_psicologo, id_paciente)` referenciando a chave única de `vinculo_profissional`. Isso exige que a tabela associativa tenha PK própria ou chave única no par. Com a RN-019 proposta, a inconsistência passa a ser impedida pelo próprio banco.

### INC-04 — Vínculo sem temporalidade contra o histórico de atendimento

- **Evidência:** RF-021 obs. — *"As condições de encerramento do vínculo precisam ser definidas pelo grupo antes de detalhar transições."* RN-003 condiciona o acesso ao vínculo. RF-031 — histórico de atendimentos de pacientes vinculados.
- **Impacto:** se o vínculo é encerrado e o atendimento histórico permanece, a consulta de histórico depende do estado temporal do vínculo. Sem `data_inicio`/`data_fim`, o modelo não distingue "atendimento sob vínculo vigente" de "atendimento sob vínculo encerrado", e RN-003 pode barrar indevidamente o acesso ao histórico próprio do profissional.
- **Proposta:** RNF-020 (integridade temporal dos vínculos) com colunas `data_inicio`, `data_fim` e `situacao` em `vinculo_profissional`. O encerramento passa a ser um **encerramento lógico datado**, não a remoção do registro.

### INC-05 — "Horário" atômico contra "disponibilidade" como intervalo

- **Evidência:** o glossário define Disponibilidade como *"Intervalo de tempo e dias oferecido pelo psicólogo"*. RF-023 fala em *"períodos de disponibilidade"*. RN-005 fala em *"um mesmo horário"*; RN-010, em *"horário que esteja disponível"*. RF-023 obs.: *"Regras detalhadas de recorrência e fuso horário não foram definidas na Fase 2."*
- **Impacto:** se a disponibilidade é um **intervalo contínuo**, dois agendamentos em intervalos parcialmente sobrepostos violam RN-005, mas uma restrição `UNIQUE (id_disponibilidade)` **não expressa** isso — ela só impede o par exato. Seria necessária restrição de exclusão por sobreposição, que não é portátil entre SGBDs. Se a disponibilidade é uma **grade de slots discretos** de duração fixa, a restrição `UNIQUE` é suficiente, simples e portável.
- **Proposta:** decidir o modelo de tempo. **Recomenda-se slot discreto com duração fixa**, o que mantém a restrição de unicidade simples e portátil, e é coerente com a linguagem de "horário" usada em RN-005 e RN-010. Registrar DEC.

### INC-06 — "Ativos incompatíveis" contra a situação inicial da solicitação

- **Evidência:** RN-005 — *"não pode possuir dois agendamentos ativos incompatíveis"*. RF-025 obs. — *"A situação inicial da solicitação e se ela bloqueia imediatamente o horário precisam de decisão do grupo."* `02-requisitos/rastreabilidade-inicial.md` — pendência: *"Grupo deve definir situação inicial da solicitação e efeito na disponibilidade."*
- **Impacto:** a restrição de unicidade depende de **quais situações são "ativas"**. Se `SOLICITADO` não bloquear o horário, o mesmo horário pode acumular várias solicitações pendentes — aceitável se assim decidido, mas a regra precisa declarar quais situações competem. Sem essa definição **não é possível escrever o índice único parcial** que materializa RN-005.
- **Proposta:** definir o conjunto de situações que ocupam o horário e registrar DEC. O índice único parcial só pode ser escrito depois disso.

### INC-07 — Auditoria sem o valor anterior da situação

- **Evidência:** RN-009 — alterações relevantes de situação devem preservar rastreabilidade por auditoria. RNF-004 — usuário responsável, data, hora e operação. CA-RF-020 — *"a nova situação deverá ser registrada e a operação relevante deverá ser rastreável"*. CA-RF-032 — usuário, data, hora e operação. O plano §10.2 exemplifica `agendamento.status` como coluna única.
- **Impacto:** o conjunto `{usuário, data, hora, operação}` **não captura o valor anterior nem o posterior**. A trilha não responde "o que mudou, de qual estado para qual estado". O exemplo do plano §10.2 atende o mínimo literal do critério de aceite, mas esvazia o substantivo de RN-009.
- **Proposta:** RNF-019 (rastreabilidade de alteração de situação), com `situacao_anterior` e `situacao_nova` no registro de auditoria, ou tabela de histórico `historico_situacao`.

### INC-08 — Auditoria contra exclusão de usuário

- **Evidência:** RN-014 — registros não alterados por operações comuns. RF-032 — o registro deve *"permanecer rastreável"*. RF-005 e RF-006 — ativar e bloquear, sem exclusão. O plano §10.2 prevê `usuario.status` com valores ATIVO, BLOQUEADO e INATIVO.
- **Impacto:** se o usuário for excluído fisicamente, a auditoria perde o responsável e quebra RN-014. Nenhuma exclusão física de usuário é prevista nos requisitos, o que indica **exclusão lógica** como estratégia.
- **Proposta:** RNF-013 (integridade referencial com estratégia de exclusão controlada). `ON DELETE RESTRICT` em `auditoria.id_usuario`, situação em `usuario` como mecanismo de inativação, e ausência de qualquer FK com `ON DELETE CASCADE` sobre `auditoria` ou `atendimento`.

### INC-09 — Catálogo de áreas de atuação

- **Evidência:** RF-016 obs. — *"A manutenção de um catálogo de áreas não está detalhada nas decisões atuais e requer definição se for necessária."* O glossário define Área de atuação como conceito do domínio. O plano §5.1 lista `AreaAtuacao` como classe. O plano §10.1 exige resolver o N:N `Psicologo ↔ AreaAtuacao`.
- **Impacto:** **com** catálogo, `area_atuacao` é entidade com PK e o N:N é resolvido por tabela associativa. **Sem** catálogo, a área é atributo controlado e a classe `AreaAtuacao` do §5.1 deixa de se sustentar — o plano §5.1 é expressamente "proposta de ponto de partida".
- **Proposta:** decidir. O DER só deve conter `area_atuacao` se o catálogo for aprovado.

### INC-10 — Notificação fora do modelo até a decisão do grupo

- **Evidência:** DEC-009 pendente. `03-uml/Contexto/Interações_Processos.md` — ator secundário *"possível, não confirmado"*. Não existe RF para notificação; RF-032 é auditoria. O glossário lista Notificação.
- **Impacto:** se houver gravação interna, exige a tabela `notificacao`; se o aviso for puramente externo, não há tabela e a fronteira vira componente. A decisão altera o **diagrama de componentes** e o **diagrama de implantação**, não só o banco.
- **Proposta:** manter a tabela fora do DER até DEC-009, e modelar a fronteira como **interface** (DIP, plano §15), de modo que a decisão posterior não force redesenho da arquitetura.

### INC-11 — Funcionalidade de inovação fora do DER até a decisão do grupo

- **Evidência:** DEC-010 pendente. O plano §5.1 lista `ListaEspera` e `EntradaListaEspera`. A seção 8 do documento de requisitos registra: *"Nenhum requisito funcional de inovação foi criado."* O plano §3.2 apenas **recomenda** a lista de espera inteligente; recomendação do plano não é decisão do grupo.
- **Impacto:** incluir essas tabelas antes da decisão violaria o congelamento de escopo (DEC-008 e DEC-010) e criaria **tabelas órfãs** — vedado pela regra do plano §6.1: *"Nenhum requisito crítico, caso de uso, classe persistente, tabela ou diagrama deve ficar órfão."*
- **Proposta:** manter fora do DER. Se o grupo aprovar a lista de espera, o acréscimo será de duas tabelas e uma FK, sem impacto nas demais.

### INC-12 — Profissional e paciente sem conta de acesso

- **Evidência:** RF-014 obs. — *"O cadastro profissional e a conta de acesso são conceitos relacionados, mas suas informações não devem ser duplicadas sem necessidade."* CA-RF-014 — *"sem duplicar desnecessariamente a identidade de usuário."* RF-015 e RF-023 pressupõem *"perfil profissional cadastrado"*, não *"usuário"*. DEC-006 — o Cliente/Paciente é ator com acesso autenticado limitado, o que sugere conta.
- **Impacto:** `Psicologo` pode existir sem conta (0..1) ou obrigatoriamente ter uma (1..1). A escolha muda a cardinalidade, a presença ou não de FK em `psicologo` e `paciente`, e a obrigatoriedade de `usuario` no cadastro.
- **Proposta:** decidir. **Recomendação:** FK opcional e única em `psicologo` e `paciente`, sem herança e sem coluna discriminadora `tipo` — coerente com §5.2, que desaconselha modelar tipo de usuário como herança. O `tipo` pode ser derivado do perfil associado.

## 6. Achados positivos

Registrar o que já está coerente é parte da revisão:

- **Separação identidade/autorização** — `Usuario → Perfil → Permissao` com N:N resolvido em `perfil_permissao` cobre RNF-002 sem duplicação, e é a decisão de DEC-002/§5.2.
- **Vínculo como entidade de primeira classe** — tratar `VinculoProfissional` como entidade associativa com atributos (e não como simples par de chaves) é o que torna RN-003 implementável e auditável.
- **Auditoria como entidade append-only** — coerente com RN-014 e com a ausência de qualquer RF de alteração ou exclusão de auditoria.
- **Atendimento como registro conceitual** — a separação entre registro de sessão e prontuário clínico respeita RF-030 obs. e DEC-008.
- **Ausência de dados reais** — nenhum artefato do repositório contém dado real de paciente, conforme verificado na Fase 3.

## 7. Pendências que bloqueiam o DER

Nenhuma destas pendências é resolvível apenas pelo Integrante 4: todas exigem decisão do grupo.

| ID | Pendência | Origem | Impacto no DER | Decisão de |
|---|---|---|---|---|
| P-01 | Ator e fluxo de confirmação do agendamento | RN-013, RF-026, CA-RF-026 | Domínio de valores de `agendamento.situacao` | Grupo |
| P-02 | Prazo, situações canceláveis e atores do cancelamento | RN-012, RF-027, CA-RF-027 | Domínio de valores e guardas de transição | Grupo |
| P-03 | Situação inicial da solicitação e seu efeito sobre a disponibilidade | RF-025 obs. | Definição de "ativo" para o índice único de RN-005 | Grupo |
| P-04 | Situações cadastrais do paciente | RF-020 obs. | Domínio de valores e restrição de verificação | Grupo |
| P-05 | Modelo de tempo da disponibilidade: slot discreto ou intervalo contínuo | RF-023 obs., RN-005, RN-010 | Forma da restrição de exclusividade de horário | Grupo |
| P-06 | Cardinalidade entre Usuário e Perfil | RF-003, plano §6.1 | Presença de `usuario_perfil` | Grupo |
| P-07 | Sessão é entidade persistida ou estado volátil | RF-002 obs. | Presença de `sessao` | Grupo |
| P-08 | Catálogo de áreas de atuação | RF-016 obs. | Presença de `area_atuacao` | Grupo |
| P-09 | Serviço Externo de Notificação | DEC-009 | Tabela `notificacao` e componente externo | Grupo |
| P-10 | Funcionalidade de inovação | DEC-010 | Tabelas `lista_espera` e `entrada_lista_espera` | Grupo |
| P-11 | Modelo de identidade do profissional e do paciente | RF-014 obs., CA-RF-014, DEC-006 | FK opcional, herança ou coluna discriminadora | Grupo |
| P-12 | Escopo temporal do vínculo profissional | RF-021 obs., RN-003, RF-031 | Colunas de data em `vinculo_profissional` | Grupo |

**Regra de suspensão do DER.** Enquanto P-01 a P-05 não estiverem fechadas, o DER pode ser desenhado em nível de **estrutura e cardinalidades**, mas **não** pode receber o modelo relacional definitivo, o dicionário de dados e o DDL, porque o domínio de valores das situações e a forma da restrição de exclusividade de horário dependem dessas decisões.

## 8. Rastreabilidade deste documento

| Seção | Origem |
|---|---|
| Classificação de classes | plano §5.1, §5.2, glossário, RF-014, RF-015, RF-023, RF-032, DEC-009, DEC-010 |
| Multiplicidades × cardinalidades | plano §6.1, §10, RF-003, RF-021, RF-025, RF-030, RN-003, RN-005, RN-011 |
| N:N, composição e herança | plano §10.1, §15 (SOLID), §5.2, RF-014 obs., RF-030 |
| INC-01 a INC-12 | RF-003, RF-014, RF-016, RF-020, RF-021, RF-023, RF-025, RF-026, RF-027, RF-030, RN-003, RN-005, RN-006, RN-007, RN-009, RN-010, RN-011, RN-014, RNF-004, CA-RF-014, CA-RF-020, CA-RF-032, DEC-009, DEC-010 |
| Pendências P-01 a P-12 | `02-requisitos/rastreabilidade-inicial.md` §"Pendências para fechar a rastreabilidade", RF-002 obs., RF-016 obs., RF-020 obs., RF-021 obs., RF-023 obs., RF-025 obs., RF-026 obs., RF-027 obs., RN-012, RN-013, DEC-009, DEC-010 |
