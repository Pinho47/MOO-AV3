# Dossiê — Seções 21, 22, 23 e 24 (texto pronto)

**Responsável:** Integrante 2 (Fase 6) · **Data:** 22/10/2026 · **Versão:** v1.0.
**Uso:** texto para as seções 21 (Diagramas de sequência), 22 (Diagrama de comunicação), 23 (Diagrama de atividades) e 24 (Máquina de estados) do dossiê final (estrutura do plano §16).
**Regra de inserção de diagramas (plano §16.1):** cada diagrama entra no dossiê com título + identificador, objetivo, elementos principais, como interpretar, decisões de modelagem, relação com requisitos/classes/banco e **referência ao arquivo editável** no repositório. Os seis diagramas da Fase 6 já têm **PDF e fonte editável (`.drawio.xml`)** versionados em `03-uml/` — ver a convenção de entrega em `03-uml/README.md`; se o grupo quiser PNG/SVG adicional, eles são gerados a partir do `.drawio.xml` no diagrams.net.

> **Estado dos diagramas na data deste texto:** os seis diagramas comportamentais estão em **fase de aprovação**. As operações `OP-01…OP-22` e as situações usadas aqui foram propostas na Fase 5 e ainda **não** incorporadas ao diagrama de classes oficial (ver `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md`); as decisões **DEC-009** (canal de notificação), **DEC-010** (inovação/"encaixe"), **DEC-012** e **P-01**, **P-02**, **P-05** continuam pendentes do grupo. Enquanto o grupo não aprovar, o dossiê deve usar a formulação "proposta em revisão" e não "decidido".
>
> **Regra de ausência de dados reais (DEC-005):** nenhum diagrama desta seção contém nome, documento, e-mail ou qualquer identificador de pessoa real — apenas papéis, identificadores de operação e de regra.

---

## 21. Diagramas de sequência

### 21.1 Identificação e objetivo

O plano da AV3 pede "pelo menos três diagramas de sequência para os casos de uso críticos aprovados, garantindo que mensagens, retornos, alternativas e exceções sejam coerentes com os fluxos textuais" (§6.1, Integrante 2). A Fase 6 entregou **três** diagramas de sequência, um para cada caso de uso crítico com comportamento observável:

| ID | Título | Caso de uso | Requisitos |
|---|---|---|---|
| **SEQ-001** | Autenticar usuário | UC-001 | RF-001, RF-002 |
| **SEQ-002** | Agendar atendimento | UC-025 (+ UC-024) | RF-024, RF-025, RF-026, RF-032 |
| **SEQ-003** | Cancelar / confirmar / concluir agendamento | UC-027 | RF-026, RF-027, RF-028, RF-030, RF-032 |

O objetivo comum é mostrar **quem fala com quem, em que ordem e sob qual condição**, de modo que cada passo do texto do caso de uso possa ser lido como uma mensagem entre participantes identificados.

### 21.2 SEQ-001 — Autenticar usuário (UC-001)

**Participantes (7):** Ator (paciente, psicólogo ou administrador) · `ServicoAutenticacao` (AP-01) · `RepositorioUsuario` (IR-01) · `Usuario` · `RepositorioAutorizacao` (IR-02) · `ServicoAuditoria` (AP-02) · `RegistroAuditoria`.

**Fluxo:** `autenticar(email, senha)` → `obterPorIdentificacao(identificacao)` → `podeAutenticar()` (OP-01, RN-001) → `confereCredencial(senha)` (OP-02, RN-002) → `obterPermissoes(idUsuario)` → `criarSessao(usuario)` → `registrar("LOGIN_CONCEDIDO" | "LOGIN_NEGADO", …)`.

**Alternativas e exceções:** `alt` **E1 — credencial inválida** (recusa + auditoria `LOGIN_NEGADO`); `alt` **E2 — conta bloqueada ou inativa** (recusa por RN-001 + auditoria); `opt` **A1 — encerrar sessão** (`invalidarSessaoAtiva`, AP-01).

### 21.3 SEQ-002 — Agendar atendimento (UC-025 + UC-024)

**Participantes (8):** Ator (paciente) · `ServicoAgenda` (AP-03) · `Vinculo` · `Agenda` · `Agendamento` · `ServicoAuditoria` (AP-02) · `RegistroAuditoria` · `ServicoNotificacao` (AP-04).

**Fluxo:** consulta de horários (`consultarHorariosDisponiveis`, OP-11) → validação do vínculo (`estaAtivo`, OP-22) → escolha do horário → `verificarDisponibilidade(horario)` (RN-010) → `Agendamento.solicitar(...)` → `confirmar(solicitante)` (OP-13, RN-013) → auditoria → notificação → resposta ao ator.

**Alternativas e exceções:** `alt` **A1 — nenhum horário disponível** (informar e encerrar); `alt` **E1 — conflito de horário** (recusa com sugestão de outro horário).

### 21.4 SEQ-003 — Cancelar / confirmar / concluir / registrar falta (UC-027)

**Participantes (7):** Ator (paciente ou psicólogo) · `ServicoAgenda` (AP-03) · `Agendamento` · `Atendimento` · `ServicoAuditoria` (AP-02) · `RegistroAuditoria` · `ServicoNotificacao` (AP-04).

**Fluxo do cancelamento:** `consultarMeus(solicitante)` (OP-19, RF-028) → `cancelar(agendamento, motivo)` → `podeSerCancelado()` (OP-15, RN-012) → `cancelar(solicitante, motivo)` (OP-14) → auditoria (`AGENDAMENTO_CANCELADO`) → `notificar(cancelamento)` → resposta ao ator.

**Blocos `opt` alternativos (não são etapas obrigatórias em sequência):** confirmação (`RF-026`), conclusão da sessão realizada (`RF-030`, com `estaCancelada()` — OP-16/RN-006 —, `concluir` — OP-17 — e `Atendimento.registrar` — OP-20) e registro de falta (`plano §5.4` / INC-13, com `registrarFalta` — OP-18).

**Alternativa:** `alt` **E1 — agendamento já realizado ou já cancelado** (recusa).

### 21.5 Como interpretar

1. **A linha de vida é a unidade de responsabilidade.** `ServicoAgenda`, `ServicoAuditoria`, `ServicoAutenticacao` e `ServicoNotificacao` são **responsabilidades de aplicação** (AP-01 a AP-04): concentram a orquestração e ficam **fora** do diagrama de classes de domínio, conforme o plano §5.2. As entidades (`Usuario`, `Vinculo`, `Agenda`, `Agendamento`, `Atendimento`, `RegistroAuditoria`) carregam as regras.
2. **Toda mensagem remete a uma operação ou responsabilidade conhecida.** As 37 mensagens dos três diagramas foram conferidas uma a uma contra as operações `OP-01…OP-22` da Fase 5, as responsabilidades `AP-01…AP-05` e as interfaces de infraestrutura `IR-01`/`IR-02`. Não existe mensagem "inventada para satisfazer um diagrama futuro" (plano §8).
3. **A seta tracejada é o retorno** (`-->>`) e não conta como mensagem nova na contagem.
4. **`alt` é alternativa de fluxo, `opt` é bloco condicional**, e ambos refletem o texto do caso de uso — nenhuma alternativa existe só no diagrama.
5. **As guardas aparecem entre colchetes** na própria mensagem (`[OP-15 / RN-012]`), de modo que a regra fica legível sem consultar a matriz.
6. **Repositórios são infraestrutura**, não domínio: aparecem como `RepositorioUsuario` e `RepositorioAutorizacao`, e apenas carregam dados.

### 21.6 Decisões de modelagem

| Decisão | Efeito | Base |
|---|---|---|
| A entidade decide, o repositório carrega | `podeAutenticar()` (OP-01) e `confereCredencial()` (OP-02) estão na entidade `Usuario`, não no repositório | plano §5.2, RN-001/RN-002 (achado **R6-03** da revisão da Fase 6) |
| Confirmação automática como operação do serviço | `confirmar(solicitante)` é executado por `ServicoAgenda`, não pelo ator | RN-013 ("ator **ou mecanismo** autorizado"), P-01 |
| Notificação é fronteira externa, condicional | `ServicoNotificacao` é interface; a mensagem está marcada como dependente de decisão do grupo | DEC-009 (pendente), TRS-02 do Integrante 4 |
| Auditoria como efeito transversal | `registrar(...)` é sempre delegado a `ServicoAuditoria` e nunca executado "de dentro" da entidade | RN-009, RN-014 |
| Sessão fora do domínio | `criarSessao` / `invalidarSessaoAtiva` são responsabilidade de aplicação | RF-002, P-07 |
| Cancelamento exige verificação prévia | `podeSerCancelado()` (OP-15) é consultada antes de `cancelar` (OP-14) | RN-012, DEC-012 (pendente) |
| "Não compareceu" só a partir de confirmado | o bloco `opt` de falta existe apenas para agendamento `CONFIRMADO` | INC-13 do Integrante 4, plano §5.4 |

### 21.7 Relação com requisitos, classes e banco

Cada mensagem dos três diagramas corresponde a uma **operação do modelo de classes** (`Usuario`, `Vinculo`, `Agenda`, `Agendamento`, `Atendimento`, `RegistroAuditoria`), a uma responsabilidade de aplicação ou a uma interface de infraestrutura — a matriz completa está em `06-rastreabilidade/matriz-rastreabilidade-fase6.md` (§2.1 a §2.3) e §3. No banco, os mesmos passos correspondem às tabelas `usuario`, `vinculo`, `agendamento`, `atendimento` e `registro_auditoria` (seção 26 do dossiê); a notificação **não** tem tabela, por ser serviço externo (DEC-009).

### 21.8 Arquivos

| Artefato | Arquivo |
|---|---|
| SEQ-001 (fonte, editável e PDF) | `03-uml/Sequências/proposta-I2-sequencia-seq-001-autenticar.mmd` · `.drawio.xml` · `.pdf` |
| SEQ-002 | `03-uml/Sequências/proposta-I2-sequencia-seq-002-agendar.mmd` · `.drawio.xml` · `.pdf` |
| SEQ-003 | `03-uml/Sequências/proposta-I2-sequencia-seq-003-cancelar-confirmar.mmd` · `.drawio.xml` · `.pdf` |
| Especificação dos três | `03-uml/Sequências/proposta-I2-especificacao-sequencias.md` |
| Revisão da fase | `01-planejamento/revisoes/FASE6-REV-I2-revisao-comportamental.md` |

---

## 22. Diagrama de comunicação

### 22.1 Identificação e objetivo

**COM-001 — Diagrama de comunicação: ciclo do agendamento.**
O diagrama de comunicação representa **a mesma informação** do diagrama de sequência, mas com o foco invertido: em vez de mostrar a ordem no tempo, mostra **quais objetos colaboram** e **qual objeto coordena a colaboração**. Ele atende ao item do plano §6.1 ("diagrama de comunicação para um cenário relevante") e cumpre a função de **checagem cruzada**: o cenário é o mesmo do SEQ-002, do SEQ-003 e do ATV-001, de modo que qualquer objeto ou mensagem que apareça em um e não em outro fica evidente.

**Cenário:** "Agendar e concluir atendimento" (UC-024 + UC-025 + UC-027).

### 22.2 Elementos principais

**Doze colaboradores**, com natureza indicada pela forma:

| Collaborador | Natureza | Papel no cenário |
|---|---|---|
| `Paciente` | fronteira externa | solicita e cancela |
| `Psicólogo` | fronteira externa | conclui o atendimento |
| `Usuario` | entidade de domínio | sobre o qual recai a autorização por operação (OP-04) |
| `ServicoAgenda` | **serviço de aplicação (hub)** | coordena o cenário (AP-03) |
| `Vinculo` | entidade de domínio | autorização do vínculo (OP-22) |
| `Agenda` | entidade de domínio | catálogo de horários (OP-11) |
| `Disponibilidade` | entidade de domínio | slots ofertados (A09) |
| `Agendamento` | entidade de domínio | compromisso e seu ciclo (OP-13, OP-14) |
| `Atendimento` | entidade de domínio | registro da sessão (OP-20) |
| `ServicoAuditoria` | serviço de aplicação | trilha (AP-02) |
| `RegistroAuditoria` | entidade de domínio | registro imutável (RF-032) |
| `ServicoNotificacao` | fronteira externa | envio de mensagem (AP-04) |

### 22.3 Como interpretar

As **14 mensagens** estão numeradas na ordem de ocorrência, e cada seta carrega a identificação da operação e a regra/requisimento que a origina:

| # | Mensagem | Origem |
|---|---|---|
| 1 | `solicitar(horario)` | RF-025 |
| 2 | `consultarPermitidos(solicitante)` | OP-04 |
| 3 | `estaAtivo()` | OP-22 / RN-003 |
| 4 | `consultarHorarios(paciente, periodo)` | OP-11 |
| 5 | `slots livres` | A09 |
| 6 | `verificarDisponibilidade(horario)` | RN-005 |
| 7 | `solicitar(psicologo, paciente, horario)` | RF-025 |
| 8 | `confirmar(solicitante)` | OP-13 / RN-013 |
| 9 | `registrar(AGENDAMENTO_CONFIRMADO)` | AP-02 |
| 10 | `registrar(responsavel, data, hora, operacao)` | RF-032 |
| 11 | `notificar(confirmacao)` | AP-04 (DEC-009) |
| 12 | `cancelar(solicitante, motivo)` | OP-14 |
| 13 | `concluir(agendamento)` | RF-030 |
| 14 | `registrar(agendamento, contextoSessao)` | OP-20 |

**O `ServicoAgenda` é o hub** do diagrama: todas as mensagens que disparam regra passam por ele, e é ele que aciona auditoria e notificação. Isso é proposital — é a materialização da regra do plano §5.2 contra a **classe onipotente**: o poder de decisão fica dividido entre `ServicoAgenda` (orquestração), as entidades (regras) e `ServicoAuditoria`/`ServicoNotificacao` (efeitos).

### 22.4 Decisões de modelagem

| Decisão | Efeito | Base |
|---|---|---|
| Um único diagrama de comunicação | o plano pede "um cenário relevante"; o cenário do agendamento é o de maior densidade de objetos | plano §6.1 |
| `ServicoAgenda` como hub | concentração da coordenação sem concentration de regra | plano §5.2 (AP-03) |
| Retornos não geram seta | o grafo tem 14 arestas, não 14 diálogo com resposta | notação de diagrama de comunicação |
| Notificação como fronteira externa | `ServicoNotificacao` é interface, não entidade | DEC-009 (pendente), TRS-02 |
| Autorização por operação (OP-04) separada das permissões da sessão | `consultarPermitidos` ≠ `obterPermissoes` | achado **R6-04** da revisão da Fase 6 |

### 22.5 Relação com requisitos, classes e banco

Os doze colaboradores são **onze classes do modelo de domínio** (`Usuario`, `Vinculo`, `Agenda`, `Disponibilidade`, `Agendamento`, `Atendimento`, `RegistroAuditoria` e demais) e **responsabilidades de aplicação/fronteira** (`ServicoAgenda`, `ServicoAuditoria`, `ServicoNotificacao`) — nenhuma classe nova foi criada. As mensagens 1 a 14 correspondem, uma a uma, aos passos do SEQ-002, do SEQ-003 e às atividades do ATV-001; a correspondência está em `06-rastreabilidade/matriz-rastreabilidade-fase6.md` §2.4.

### 22.6 Arquivos

| Artefato | Arquivo |
|---|---|
| COM-001 (fonte, editável e PDF) | `03-uml/Comunicação/proposta-I2-comunicacao-com-001.mmd` · `.drawio.xml` · `.pdf` |
| Especificação | `03-uml/Comunicação/proposta-I2-especificacao-comunicacao.md` |

---

## 23. Diagrama de atividades

### 23.1 Identificação e objetivo

**ATV-001 — Diagrama de atividades: ciclo do agendamento.**
O plano §6.1 pede "o diagrama de atividades com decisões, responsabilidades e paralelismo quando aplicável". O ATV-001 representa o **fluxo de negócio** do agendamento — do pedido do horário até a conclusão, o cancelamento ou a falta — destacando **quem** executa cada passo (**responsabilidade**) e **onde** a regra decide (**decisões**).

### 23.2 Elementos principais

- **Quatro pistas (swimlanes):** *Paciente* · *Sistema (aplicação e domínio)* · *Psicólogo* · *Auditoria e notificação*. A separação entre "Sistema" e "Auditoria e notificação" existe porque auditoria e notificação **não são regra de negócio**: são efeitos transversais (AP-02 e AP-04).
- **Quatro decisões:** `d1` (Há horários?), `d2` (Vínculo OK?), `d3` (Cancelamento?), `d5` (Sessão realizada?).
- **Três blocos paralelos (fork/join):**

| Bloco | Gatilho | Ramos paralelos | Join |
|---|---|---|---|
| `fork1`/`join1` | Confirmar (OP-13) | auditar confirmação ‖ notificar confirmação | `join1` |
| `fork2`/`join2` | Cancelar (OP-14) | liberar horário ‖ auditar cancelamento | `join2` |
| `fork3`/`join3` | Registrar atendimento (OP-20) | concluir (OP-17) ‖ auditar conclusão | `join3` |

- **Uma etapa sequencial de notificação** entre o `join2` e o fim do bloco de cancelamento: `Notificar cancelamento (AP-04)` — ela só pode ocorrer depois que o cancelamento está gravado e auditado (pós-commit, TRS-02) e antes da resposta ao ator, exatamente como no SEQ-003 (achado **R6-05**, corrigido na Fase 6).

### 23.3 Como interpretar

1. **A pista mostra a responsabilidade, não o objeto.** Uma atividade pode ser executada por uma entidade de domínio (`Confirmar (OP-13)`) ou por um serviço de aplicação (`Auditar confirmação`).
2. **O losango é uma decisão da regra**, e o texto no losango é curto por limitação de espaço; a condição completa está na especificação do diagrama e no caso de uso.
3. **A barra de paralelismo indica independência de regra**, não transação única: o paralelismo é lógico. A fronteira transacional é responsabilidade da camada de persistência (Fase 7) e é explicitamente o que o TRS-02 do Integrante 4 restringe.
4. **Cada rota tem um `Fim`** — a notação de atividade exige fim explícito em cada rota terminal (sem horários, recusa, cancelamento, sessão realizada e falta).
5. **O cancelamento e a sessão realizada são alternativas**, não etapas em sequência: `d3` separa "o paciente cancela" de "o paciente mantém".

### 23.4 Decisões de modelagem

| Decisão | Efeito | Base |
|---|---|---|
| Quatro pistas, e não duas | separa a regra de negócio dos efeitos transversais | plano §5.2 |
| "Liberar horário" aparece só no cancelamento | é consequência da regra (RN-012/RN-010) sobre o catálogo de disponibilidade, não uma mensagem a um serviço de aplicação | RN-010, RN-012 |
| Notificação do cancelamento sequencial | mantém a ordem *auditar → notificar → responder* do SEQ-003 | TRS-02, achado **R6-05** |
| Sem lista de espera e sem sessão como atividade | ambos fora do domínio | plano §5.2, P-07 |
| Decisões `d1`, `d2`, `d3`, `d5` | uma decisão por desvio de rota do caso de uso | UC-024, UC-025, UC-027 |

### 23.5 Relação com requisitos, classes e banco

Cada atividade do ATV-001 corresponde a uma operação de domínio, a uma responsabilidade de aplicação ou a um efeito de regra (matriz `06-rastreabilidade/matriz-rastreabilidade-fase6.md` §2.5). Nenhuma atividade existe sem origem: as 18 atividades auditadas estão todas ancoradas. As atividades de consulta de horário e de registro de atendimento correspondem, no banco, a leituras e escritas em `disponibilidade`, `agendamento` e `atendimento` (seção 26).

### 23.6 Arquivos

| Artefato | Arquivo |
|---|---|
| ATV-001 (fonte, editável e PDF) | `03-uml/Atividade/proposta-I2-atividade-atv-001.mmd` · `.drawio.xml` · `.pdf` |
| Especificação | `03-uml/Atividade/proposta-I2-especificacao-atividade.md` |

---

## 24. Máquina de estados

### 24.1 Identificação e objetivo

**EST-001 — Máquina de estados do `Agendamento`.**
O plano §5.4 fixa o ciclo de vida do compromisso — "Solicitado → Confirmado → Realizado; saídas para Cancelado e Não compareceu" — e o §6.1 pede a máquina de estados correspondente. A máquina registra **quais situações o agendamento pode ter, por qual evento sai de cada uma e o que é permitido a partir delas**.

### 24.2 Elementos principais

**Cinco situações**, exatamente os valores do enumerado `SituacaoAgendamento` do modelo de classes (Fase 5) e da coluna `agendamento.status` do banco:

| Situação | Significado |
|---|---|
| `SOLICITADO` | compromisso criado, aguardando confirmação (situação inicial) |
| `CONFIRMADO` | compromisso confirmado |
| `REALIZADO` | sessão realizada — **terminal** |
| `CANCELADO` | compromisso cancelado — **terminal** |
| `NAO_COMPARECEU` | falta registrada — **terminal** |

**Seis transições:**

| # | Transição | Evento | Guarda | Operação |
|---|---|---|---|---|
| T1 | `[*] → SOLICITADO` | `solicitar` | vínculo ATIVO e slot livre | `Agendamento.solicitar` (RF-025) |
| T2 | `SOLICITADO → CONFIRMADO` | `confirmar` | `[slot livre]` | OP-13 (RN-013) |
| T3 | `SOLICITADO → CANCELADO` | `cancelar` | `[prazo]` | OP-14 / OP-15 (RN-012) |
| T4 | `CONFIRMADO → CANCELADO` | `cancelar` | `[prazo]` | OP-14 / OP-15 (RN-012) |
| T5 | `CONFIRMADO → REALIZADO` | `concluir` | `[atendimento]` | OP-17 / OP-20 (RF-030) |
| T6 | `CONFIRMADO → NAO_COMPARECEU` | `registrarFalta` | — | OP-18 (plano §5.4) |

### 24.3 Como interpretar

1. **`SOLICITADO` é o estado inicial** e `REALIZADO`, `CANCELADO` e `NAO_COMPARECEU` são **terminais**: não há retorno de um compromisso encerrado.
2. **O evento é a causa; a guarda é a condição.** `cancelar` só se aplica a `SOLICITADO` ou `CONFIRMADO` e dentro do prazo (`[prazo]`, valor pendente de DEC-012/P-02); `concluir` pressupõe que existe atendimento.
3. **A ausência de transição é uma informação**: não existe `SOLICITADO → NAO_COMPARECEU`. Um agendamento que o paciente não compareceu só pode ter sido confirmado antes — é a INC-13 do Integrante 4, respondida por desenho.
4. **O cancelamento é possível em dois pontos** porque o paciente pode desistir antes da confirmação ou depois dela.
5. **A máquina é compatível com a confirmação automática**: se o grupo decidir (P-01) que a confirmação é assíncrona, `SOLICITADO` fica ocupado por um instante e T1/T2 apenas se separam no tempo — nenhum redesenho é necessário.

### 24.4 Decisões de modelagem

| Decisão | Efeito | Base |
|---|---|---|
| Cinco situações, três terminais | o compromisso encerrado não volta | plano §5.4 |
| `NAO_COMPARECEU` só a partir de `CONFIRMADO` | fecha a INC-13 do Integrante 4 e a pergunta P-14 | INC-13, plano §5.4 |
| `SOLICITADO` como situação inicial explícita | o diagrama de objetos e o banco precisam de um valor inicial | PC-07 (Fase 5) |
| Guardas `[prazo]` e `[slot livre]` no desenho | as condições dependem de decisões ainda abertas e ficam visíveis como pendência | RN-012, RN-010, DEC-012, P-05 |
| Sem situação "aguardando aprovação" | não há aprovação manual no escopo aprovado | RN-013 |

### 24.5 Relação com requisitos, classes e banco

As cinco situações são os valores do enumerado `SituacaoAgendamento` (modelo de classes, seção 19) e correspondem à coluna `agendamento.status` do modelo relacional (seção 26, com a restrição de domínio correspondente). Cada transição tem operação de origem (Tabela §2.6 da matriz da Fase 6) e caso de uso associado (UC-025 ou UC-027). Nenhuma situação existe sem atributo persistível e nenhuma transição existe sem operação.

### 24.6 Arquivos

| Artefato | Arquivo |
|---|---|
| EST-001 (fonte, editável e PDF) | `03-uml/Estados/proposta-I2-estados-est-001.mmd` · `.drawio.xml` · `.pdf` |
| Especificação | `03-uml/Estados/proposta-I2-especificacao-estados.md` |

---

## Nota de consolidação para o Integrante 6

Ao montar o dossiê final:

1. **Inserir as seis figuras** nas seções 21 a 24, usando o `.pdf` correspondente e **mantendo a identificação** (`SEQ-001`, `SEQ-002`, `SEQ-003`, `COM-001`, `ATV-001`, `EST-001`) nas legendas, para que a matriz de rastreabilidade (seção 31) possa referenciá-las.
2. **Conservar as ressalvas de estado**: as seções devem dizer "proposta em revisão" enquanto o grupo não aprovar as decisões DEC-009, DEC-010, DEC-012, P-01, P-02 e P-05 e a incorporação das operações `OP-01…OP-22` pelo Integrante 3.
3. **Remeter as lacunas** (sequência do "encaixe" bloqueada por DEC-010; prazo do cancelamento; canal de notificação; reserva de slot) à seção de revisões/decisões do dossiê, e não ao corpo dos diagramas.
4. **Não insertar PNG/SVG separados**: o repositório entrega `.mmd` (fonte do conteúdo), `.drawio.xml` (editável) e `.pdf` (dossiê), com o **mesmo nome-base**. Se uma versão vetorial for necessária, gerar a partir do `.drawio.xml` no diagrams.net.
5. ** conferências automáticas feitas na Fase 6**: XML bem formado nos 6 arquivos, 1 página por PDF, ausência de sobreposição de caixas e de arestas atravessando caixas, e conferência do texto extraído de cada PDF contra o conteúdo do `.mmd`.
