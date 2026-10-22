# Matriz de Rastreabilidade — Fase 6 (diagramas comportamentais)

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 22/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Natureza:** **PROPOSTA.** Não substitui artefato oficial do grupo.
**Complementa (não altera):** [`matriz-rastreabilidade-fase5.md`](matriz-rastreabilidade-fase5.md) — a matriz da Fase 5 continua sendo a **matriz mestra** do projeto (RF → RN/RNF → UC → Classe → Operação → Sequência → Tabela → CT).

---

## 0. Por que uma matriz nova

O plano §6.1 (Integrante 2) pede que, após a modelagem comportamental, a matriz de rastreabilidade seja **atualizada com sequências, atividade, comunicação e estados**. A matriz da Fase 5 já tinha a coluna "Sequência", mas **não tinha** as colunas de diagrama de atividades, de comunicação e de máquina de estados, e **não rastreava** as mensagens dos diagramas.

Para **não mexer em artefato de fase anterior** (regra da AV3), esta matriz é um **documento novo e complementar**, com identificador próprio (`MT6`), referenciando a matriz da Fase 5 sempre que a coluna é herdada. Quando o grupo aprovar, as duas matrizes podem ser fundidas em uma só, na revisão do dossiê.

**Convenções:**

| Sigla | Significado |
|---|---|
| **SEQ-001/002/003** | Diagramas de sequência (Fase 6). |
| **ATV-001** | Diagrama de atividades (Fase 6). |
| **COM-001** | Diagrama de comunicação (Fase 6). |
| **EST-001** | Máquina de estados do `Agendamento` (Fase 6). |
| **OP-xx** | Operação proposta na Fase 5 (CLS-001 v2). |
| **AP-xx** | Responsabilidade de aplicação (FASE5-REV-I2 §5.2). |
| **IR-xx** | Interface de infraestrutura (Fase 7). |
| **INC-xx / TRS-xx / ARQ-xx** | Restrições e decisões da análise de persistência do Integrante 4. |

---

## 1. Matriz principal — requisito → regra → caso de uso → diagrama comportamental

| RF | RN / RNF | UC | Operação / Responsabilidade | SEQ | ATV | COM (msg.) | EST (transição) | Status |
|---|---|---|---|---|---|---|---|---|
| RF-001 Autenticar usuário | RN-001, RN-002, RNF-001, RNF-002 | UC-001 | OP-01 `podeAutenticar`; OP-02 `confereCredencial`; AP-01 | **SEQ-001** | — | — | — | Proposto (Fase 5) |
| RF-002 Encerrar sessão | RNF-002 | UC-001 (A1) | AP-01 `invalidarSessaoAtiva` | **SEQ-001** (`opt`) | — | — | — | Fora do domínio (P-07) |
| RF-003 Cadastrar usuário | RN-002, RNF-001 | UC-003 | OP-03 `cadastrar` | — | — | — | — | Proposto (Fase 5) |
| RF-004 Alterar dados de usuário | RN-002 | UC-003 | `atualizarDadosPermitidos` | — | — | — | — | Coberto |
| RF-005 Ativar usuário | RNF-003 | UC-003 | `ativar` | — | — | — | — | Coberto |
| RF-006 Bloquear usuário | RN-001, RNF-003 | UC-003 | `bloquear` | — | — | — | — | Coberto |
| RF-007 Gerenciar perfis | RNF-002 | UC-003 | `adicionarPermissao`, `removerPermissao` | — | — | — | — | Parcial (PC-10) |
| RF-008 Gerenciar permissões | RNF-002 | UC-003 | — | — | — | — | — | Parcial |
| RF-009 Consultar painel | RNF-004 | UC-003 | AP-05 | — | — | — | — | Fora do domínio |
| RF-010 Consultar usuários | RNF-002 | UC-003 | OP-04 `consultarPermitidos` | — | — | **2** | — | Proposto (Fase 5) |
| RF-011 Consultar psicólogos | RNF-002 | UC-003 | OP-06 `consultarCadastros` | — | — | — | — | Proposto (Fase 5) |
| RF-012 Consultar pacientes | RNF-005 | UC-003 | OP-08 `consultarCadastros` | — | — | — | — | Proposto (Fase 5) |
| RF-013 Consultar auditoria | RN-008, RN-014 | UC-013 | OP-09 `consultar` | — | — | — | — | Proposto (Fase 5) |
| RF-014 Cadastrar psicólogo | RN-002, RNF-002 | UC-003 | OP-05 `cadastrar` | — | — | — | — | Proposto (Fase 5) |
| RF-015 Atualizar perfil profissional | RN-002 | UC-015 | `atualizarDadosProfissionais` | — | — | — | — | Coberto |
| RF-016 Associar área de atuação | RN-002 | UC-015 | `associarAreaAtuacao` | — | — | — | — | Coberto |
| RF-017 Consultar pacientes vinculados | RN-003, RNF-006 | UC-016 | `consultarPacientesVinculados` | — | — | — | — | Coberto |
| RF-018 Cadastrar paciente | RN-005 | UC-003 | OP-07 `cadastrar` | — | — | — | — | Proposto (Fase 5) |
| RF-019 Atualizar cadastro | RN-009 | UC-003 | `atualizarCadastro` | — | — | — | — | Coberto |
| RF-020 Alterar situação do paciente | RN-009 | UC-003 | `alterarSituacao` | — | — | — | — | Parcial (estados §6) |
| RF-021 Vincular paciente a psicólogo | RN-003, RN-007 | UC-021 | `formalizar`; OP-21 `encerrar` | — | — | — | — | Coberto (estado pendente) |
| RF-022 Consultar o próprio cadastro | RN-004, RNF-005 | UC-022 | `consultarCadastroPermitido` | — | — | — | — | Coberto |
| RF-023 Cadastrar disponibilidade | RN-005, RN-011 | UC-015 | OP-10 `cadastrarDisponibilidade` | — | — | — | — | Proposto (Fase 5) |
| RF-024 Consultar horários disponíveis | RN-010, RNF-010 | UC-024 | OP-11 `consultarHorariosDisponiveis` | **SEQ-002** | **ATV-001** (`Consultar horários`) | **4, 5** | — | Proposto (Fase 5) |
| RF-025 Solicitar agendamento | RN-005, RN-010 | UC-025 | `Agendamento.solicitar` | **SEQ-002** | **ATV-001** (`Criar agendamento`) | **1, 7** | **T1** (`[*] → SOLICITADO`) | Parcial (PC-07) |
| RF-026 Confirmar agendamento | RN-013 | UC-025 | OP-13 `confirmar` | **SEQ-002**, **SEQ-003** (`opt`) | **ATV-001** (`Confirmar (OP-13)`) | **8** | **T2** (`SOLICITADO → CONFIRMADO`) | Parcial (PC-07) |
| RF-027 Cancelar agendamento | RN-012, RN-006 | UC-027 | OP-14 `cancelar`; OP-15 `podeSerCancelado` | **SEQ-003** | **ATV-001** (`Cancelar (OP-14)`) | **12** | **T3**, **T4** (→ `CANCELADO`) | Parcial (PC-07) |
| RF-028 Consultar os próprios agendamentos | RN-004 | UC-028 | OP-19 `consultarMeus` | **SEQ-003** | — | — | — | Proposto (Fase 5) |
| RF-029 Consultar a agenda do psicólogo | RN-011 | UC-029 | OP-12 `consultarCompromissos` | — | — | — | — | Proposto (Fase 5) |
| RF-030 Registrar atendimento | RN-006, RN-007, DEC-008 | UC-016 / UC-027 | OP-20 `registrar`; OP-17 `concluir` | **SEQ-003** (`opt`) | **ATV-001** (`Registrar atendimento`, `Concluir`) | **13, 14** | **T5** (`CONFIRMADO → REALIZADO`) | Proposto (Fase 5) |
| RF-031 Consultar histórico de atendimentos | RN-003, RN-007, RNF-006 | UC-016 | `consultarHistoricoPermitido` | — | — | — | — | Coberto |
| RF-032 Registrar evento de auditoria | RN-009, RN-014, RNF-004 | UC-032 (`<<include>>`) | `RegistroAuditoria.registrar` + AP-02 | **SEQ-001/002/003** | **ATV-001** (4 auditorias) | **9, 10** | **T2, T3, T4, T5, T6** (efeito) | Coberto (colaboração parcial) |
| — (plano §5.4 / INC-13) | — | UC-027 | OP-18 `registrarFalta` | **SEQ-003** (`opt`) | **ATV-001** (`Registrar falta`) | — | **T6** (`CONFIRMADO → NAO_COMPARECEU`) | Proposto (Fase 6) |
| — (DEC-009, sem RF) | — | UC-033 | AP-04 `ServicoNotificacao.notificar` | **SEQ-002/003** | **ATV-001** (2 nós: confirmação e cancelamento) | **11** | efeito de T2/T3/T4 | Fora do domínio (condicional) |

**Leitura:** 10 dos 32 RFs (RF-024 a RF-030, RF-032) têm **cobertura comportamental de Fase 6**; os demais 22 são de **cadastro, consulta e administration** — declaradamente fora do escopo comportamental desta fase (plano §6.1: "casos de uso críticos aprovados"), e todos já estão cobertos pela matriz da Fase 5. Nenhum RF **crítico** ficou sem diagrama.

---

## 2. Mensagens dos diagramas → operação ou responsabilidade

### 2.1 SEQ-001 — Autenticar (UC-001)

| # | Mensagem | Origem | RN/RF |
|---|---|---|---|
| 1 | `autenticar(email, senha)` | **AP-01** `ServicoAutenticacao` | RF-001, RN-001/002 |
| 2 | `obterPorIdentificacao(identificacao)` | **IR-01** (infraestrutura, Fase 7) | RF-001 |
| 3 | `podeAutenticar()` | **OP-01** | RN-001 |
| 4 | `confereCredencial(senha)` | **OP-02** | RN-002, RNF-001 |
| 5 | `obterPermissoes(idUsuario)` | **IR-02** (infraestrutura, Fase 7) | RF-007/008 |
| 6 | `criarSessao(usuario)` / `invalidarSessaoAtiva()` | **AP-01** | RF-002 |
| 7 | `registrar("LOGIN_CONCEDIDO" / "LOGIN_NEGADO", …)` | **AP-02** + `RegistroAuditoria.registrar` | RF-032, RN-009 |

### 2.2 SEQ-002 — Agendar (UC-025 + UC-024)

| # | Mensagem | Origem | RN/RF |
|---|---|---|---|
| 1 | `solicitar(horario)` | ator → **AP-03** | RF-025 |
| 2 | `estaAtivo()` | **OP-22** `Vinculo` | RN-003, RN-005 |
| 3 | `consultarHorariosDisponiveis(paciente, periodo)` | **OP-11** `Agenda` | RF-024 |
| 4 | `verificarDisponibilidade(horario)` | `Agenda.verificarDisponibilidade` (modelo do I3) | RN-010 |
| 5 | `Agendamento.solicitar(psicologo, paciente, horario, solicitante)` | entidade de domínio | RF-025, RN-005, RN-010 |
| 6 | `confirmar(solicitante)` | **OP-13** | RF-026, RN-013 |
| 7 | `registrar("AGENDAMENTO_CONFIRMADO", …)` | **AP-02** | RF-032, RN-009 |
| 8 | `notificar(confirmacao)` | **AP-04** (fronteira externa) | DEC-009 |

### 2.3 SEQ-003 — Cancelar / confirmar / concluir / registrar falta (UC-027)

| # | Mensagem | Origem | RN/RF |
|---|---|---|---|
| 1 | `consultarMeus(solicitante)` | **OP-19** | RF-028 |
| 2 | `podeSerCancelado()` | **OP-15** | RN-012, RF-027 |
| 3 | `cancelar(solicitante, motivo)` | **OP-14** | RF-027, RN-012 |
| 4 | `registrar("AGENDAMENTO_CANCELADO", …)` | **AP-02** | RF-032 |
| 5 | `notificar(cancelamento)` | **AP-04** | DEC-009 |
| 6 | `confirmar(solicitante)` | **OP-13** | RF-026, RN-013 |
| 7 | `estaCancelada()` | **OP-16** | RN-006 |
| 8 | `concluir(solicitante)` | **OP-17** | RF-030 |
| 9 | `registrar(agendamento, contextoSessao, solicitante)` | **OP-20** `Atendimento` | RF-030, RN-006/007 |
| 10 | `registrarFalta(solicitante)` | **OP-18** | plano §5.4, INC-13 |
| 11 | `registrar("AGENDAMENTO_CONCLUIDO" / "AGENDAMENTO_NAO_COMPARECEU", …)` | **AP-02** | RF-032 |

### 2.4 COM-001 — Comunicação (14 mensagens)

| # | Mensagem | Origem | Diagrama correspondente |
|---|---|---|---|
| 1 | `solicitar(horario)` | AP-03 | SEQ-002 (1) |
| 2 | `consultarPermitidos(solicitante)` | **OP-04** | SEQ-001 (autorização por operação) |
| 3 | `estaAtivo()` | **OP-22** | SEQ-002 (2) |
| 4 | `consultarHorarios(paciente, periodo)` | **OP-11** | SEQ-002 (3) |
| 5 | `slots livres` | A09 (`Agenda`—`Disponibilidade`) | SEQ-002 (3) |
| 6 | `verificarDisponibilidade(horario)` | `Agenda.verificarDisponibilidade` | SEQ-002 (4) |
| 7 | `solicitar(psicologo, paciente, horario)` | `Agendamento.solicitar` | SEQ-002 (5) |
| 8 | `confirmar(solicitante)` | **OP-13** | SEQ-002 (6) |
| 9 | `registrar(AGENDAMENTO_CONFIRMADO, …)` | **AP-02** | SEQ-002 (7) |
| 10 | `registrar(responsavel, data, hora, operacao)` | `RegistroAuditoria.registrar` | SEQ-002 (7) |
| 11 | `notificar(confirmacao)` | **AP-04** | SEQ-002 (8) |
| 12 | `cancelar(solicitante, motivo)` | **OP-14** | SEQ-003 (3) |
| 13 | `concluir(agendamento)` | RF-030 (fluxo do ator) | SEQ-003 (`opt` conclusão) |
| 14 | `registrar(agendamento, contextoSessao)` | **OP-20** | SEQ-003 (9) |

### 2.5 ATV-001 — Atividades

| Nó do diagrama | Origem | Diagrama correspondente |
|---|---|---|
| Solicitar agendamento | RF-025 (ator) | SEQ-002 (1) |
| Consultar horários (OP-11) | **OP-11** | SEQ-002 (3), COM-001 (4) |
| Validar vínculo e horário | **OP-22** + `Agenda.verificarDisponibilidade` | SEQ-002 (2, 4), COM-001 (3, 6) |
| Criar agendamento | `Agendamento.solicitar` | SEQ-002 (5), COM-01 (7) |
| Confirmar (OP-13) | **OP-13** | SEQ-002 (6), COM-001 (8), EST T2 |
| Auditar confirmação (RN-009) | **AP-02** | SEQ-002 (7), COM-001 (9/10) |
| Notificar confirmação (AP-04) | **AP-04** | SEQ-002 (8), COM-001 (11) |
| Cancelar (OP-14) | **OP-14** | SEQ-003 (3), COM-001 (12), EST T3/T4 |
| Liberar horário | RN-012/RN-010 (efeito no catálogo) | SEQ-003 (implícito), EST T3/T4 |
| Auditar cancelamento | **AP-02** | SEQ-003 (4) |
| Notificar cancelamento (AP-04) | **AP-04** | SEQ-003 (5) |
| Registrar atendimento (OP-20) | **OP-20** | SEQ-003 (9), COM-001 (14) |
| Concluir (OP-17) | **OP-17** | SEQ-003 (8), EST T5 |
| Registrar falta (OP-18) | **OP-18** | SEQ-003 (10), EST T6 |
| Auditar falta / conclusão | **AP-02** | SEQ-003 (11) |
| Decisões `d1`, `d2`, `d3`, `d5` | RN-010, RN-005/RN-010, RN-012, RN-006 | SEQ-002/003 (fragamentos `alt`) |

### 2.6 EST-001 — Transições

| # | Transição | Evento | Guarda | Operação | Sequência |
|---|---|---|---|---|---|
| T1 | `[*] → SOLICITADO` | `solicitar` | vínculo ATIVO e slot livre | `Agendamento.solicitar` | SEQ-002 (5) |
| T2 | `SOLICITADO → CONFIRMADO` | `confirmar` | `[slot livre]` | **OP-13** | SEQ-002 (6), SEQ-003 (`opt`) |
| T3 | `SOLICITADO → CANCELADO` | `cancelar` | `[prazo]` | **OP-14** / **OP-15** | SEQ-003 (3) |
| T4 | `CONFIRMADO → CANCELADO` | `cancelar` | `[prazo]` | **OP-14** / **OP-15** | SEQ-003 (3) |
| T5 | `CONFIRMADO → REALIZADO` | `concluir` | `[atendimento]` | **OP-17** / **OP-20** | SEQ-003 (8, 9) |
| T6 | `CONFIRMADO → NAO_COMPARECEU` | `registrarFalta` | — | **OP-18** | SEQ-003 (10) |

**Nenhuma mensagem de sequência, nenhuma mensagem de comunicação e nenhuma atividade está sem origem em operação ou responsabilidade.** Este é o critério de elegibilidade adotado no plano §6.1 ("mensagens, retornos, alternativas e exceções coerentes com os fluxos textuais") e na revisão FASE5-REV-I2 §5.2.

---

## 3. Matriz de consistência cruzada (exigida pelo plano §6.1)

Cada linha cruza os quatro diagramas comportamentais e as três fontes textuais (casos de uso, modelo de classes, requisitos).

| Comportamento | Caso de uso | Classe / operação | SEQ | ATV | COM | EST | RF | RN |
|---|---|---|---|---|---|---|---|---|
| Autenticar | UC-001 | `Usuario.podeAutenticar` (OP-01), `confereCredencial` (OP-02) | SEQ-001 | — | — | — | RF-001 | RN-001, RN-002 |
| Encerrar sessão | UC-001 (A1) | AP-01 | SEQ-001 (`opt`) | — | — | — | RF-002 | RNF-002 |
| Consultar horários | UC-024 | `Agenda.consultarHorariosDisponiveis` (OP-11) | SEQ-002 | ATV-001 | COM-001 (4, 5) | — | RF-024 | RN-010 |
| Solicitar agendamento | UC-025 | `Agendamento.solicitar` | SEQ-002 | ATV-001 | COM-001 (1, 7) | EST-001 (T1) | RF-025 | RN-005, RN-010 |
| Confirmar | UC-025 | `Agendamento.confirmar` (OP-13) | SEQ-002, SEQ-003 | ATV-001 | COM-001 (8) | EST-001 (T2) | RF-026 | RN-013 |
| Cancelar | UC-027 | `Agendamento.cancelar` (OP-14), `podeSerCancelado` (OP-15) | SEQ-003 | ATV-001 | COM-001 (12) | EST-001 (T3, T4) | RF-027 | RN-012, RN-006 |
| Consultar meus agendamentos | UC-028 | `Agendamento.consultarMeus` (OP-19) | SEQ-003 | — | — | — | RF-028 | RN-004 |
| Registrar atendimento | UC-016 / UC-027 | `Atendimento.registrar` (OP-20), `Agendamento.concluir` (OP-17) | SEQ-003 (`opt`) | ATV-001 | COM-001 (13, 14) | EST-001 (T5) | RF-030 | RN-006, RN-007 |
| Registrar falta | UC-027 (plano §5.4) | `Agendamento.registrarFalta` (OP-18) | SEQ-003 (`opt`) | ATV-001 | — | EST-001 (T6) | — | INC-13 |
| Auditar | UC-032 (`<<include>>`) | `RegistroAuditoria.registrar` + AP-02 | SEQ-001/002/003 | ATV-001 (4 nós) | COM-001 (9, 10) | efeito de T2–T6 | RF-032 | RN-009, RN-014 |
| Notificar | UC-033 | AP-04 (fronteira externa) | SEQ-002, SEQ-003 | ATV-001 (2 nós) | COM-001 (11) | efeito de T2–T4 | — (DEC-009) | — |
| Autorização | UC-001, UC-016 | `Usuario.consultarPermitidos` (OP-04), `Vinculo.estaAtivo` (OP-22) | SEQ-002 | ATV-001 (`d2`) | COM-001 (2, 3) | guarda de T1 | RF-010, RF-017, RF-021 | RN-003, RN-005 |

**Divergências detectadas: 0.** A verificação detalhada (incluindo o cotejo com a análise de persistência do Integrante 4) está em `01-planejamento/revisoes/FASE6-REV-I2-revisao-comportamental.md`.

---

## 4. Verificação de órfãos da Fase 6

| Verificação exigida pelo plano | Resultado | Observação |
|---|---|---|
| RF sem diagrama comportamental | **Sim — 22** | RF-003 a RF-023 (exceto RF-024) e RF-028/RF-029/RF-031 são de cadastro/consulta/administração: **fora do escopo comportamental** desta fase (plano §6.1 — "casos de uso críticos"). Cobertos pela matriz da Fase 5 e pelos diagramas de classes/objetos. |
| RF crítico sem diagrama comportamental | **Não** | RF-001, RF-002, RF-024, RF-025, RF-026, RF-027, RF-030, RF-032 têm pelo menos um diagrama. |
| Caso de uso crítico sem diagrama | **Não** | UC-001 → SEQ-001 · UC-021 → coberto indiretamente por SEQ-002 (`estaAtivo`) e pelo modelo · UC-025 → SEQ-002 + ATV-001 + COM-001 · UC-027 → SEQ-003 + ATV-001 + COM-001 + EST-001. |
| Mensagem de sequência sem operação/responsabilidade | **Não** | Ver §2.1–2.3 (todas as mensagens remeteram a OP/AP/IR). |
| Atividade sem origem | **Não** | Ver §2.5. |
| Mensagem de comunicação sem origem | **Não** | Ver §2.4. |
| Transição de estado sem operação | **Não** | Ver §2.6. |
| Situação de estado sem atributo persistível | **Não** | `SOLICITADO`, `CONFIRMADO`, `REALIZADO`, `CANCELADO`, `NAO_COMPARECEU` = exatamente o enum `SituacaoAgendamento` do CLS-001 v2 e a coluna `agendamento.status` proposta na Fase 5 (`IMP-DER-02` do I4). |
| Transição sem caso de uso | **Não** | T1–T6 mapeadas na §3. |
| Diagrama sem requisito | **Não** | Todos os seis diagramas declaram RF/RN no cabeçalho do `.mmd` e na especificação correspondente. |
| Diagrama comportamental sem classe | **Não** | Todos os participantes/atividade/colaboradores pertencem a uma classe do CLS-001 v2 ou a uma responsabilidade AP/IR declarada. |
| Sequência sem par Aufruf/Resposta | **Não** | Toda mensagem de solicitação tem o retorno correspondente (`-->>`), com exceção das chamadas internas de auto-mensagem, que são rotuladas como tal. |

---

## 5. Cobertura por requisito (contagem de artefatos comportamentais)

| # | RF | SEQ | ATV | COM | EST | Total de artefatos |
|---|---|---|---|---|---|---|
| 1 | RF-001 | 1 | 0 | 0 | 0 | 1 |
| 2 | RF-002 | 1 | 0 | 0 | 0 | 1 |
| 24 | RF-024 | 1 | 1 | 1 | 0 | 3 |
| 25 | RF-025 | 1 | 1 | 1 | 1 | 4 |
| 26 | RF-026 | 2 | 1 | 1 | 1 | 5 |
| 27 | RF-027 | 1 | 1 | 1 | 1 | 4 |
| 28 | RF-028 | 1 | 0 | 0 | 0 | 1 |
| 30 | RF-030 | 1 | 1 | 1 | 1 | 4 |
| 32 | RF-032 | 3 | 1 | 1 | 0 | 5 |
| — | INC-13 (falta) | 1 | 1 | 0 | 1 | 3 |
| — | DEC-009 (notificar) | 2 | 1 | 1 | 0 | 4 |

---

## 6. Testes conceituais seccionados (complementam CT-001…CT-032)

A matriz da Fase 5 (§6) define os testes conceituais por RF. A Fase 6 **não cria** testes novos: ela indica **qual artefato comportamental materializa** cada cenário de teste.

| CT | RF | Artefato que evidencia o cenário |
|---|---|---|
| CT-001 | RF-001 | SEQ-001 (`alt` E2 — conta bloqueada) |
| CT-002 | RF-002 | SEQ-001 (`opt` A1) |
| CT-024 | RF-024 | SEQ-002 (`alt` "Há horários disponíveis" / A1), ATV-001 (`d1`), COM-001 (4, 5) |
| CT-025 | RF-025 | SEQ-002, ATV-001 (`d2`, `Criar agendamento`), COM-001 (1, 7), EST-001 (T1) |
| CT-026 | RF-026 | SEQ-002, SEQ-003 (`opt`), ATV-001 (`Confirmar (OP-13)`), COM-001 (8), EST-001 (T2) |
| CT-027 | RF-027 | SEQ-003 (`alt` "Pode ser cancelado" / E1), ATV-001 (`d3`), COM-001 (12), EST-001 (T3, T4) |
| CT-028 | RF-028 | SEQ-003 (`consultarMeus`) |
| CT-030 | RF-030 | SEQ-003 (`opt` conclusão), ATV-001 (`d5` [Sim]), COM-001 (13, 14), EST-001 (T5) |
| CT-032 | RF-032 | SEQ-001/002/003 (7 + 1 + 4 mensagens de auditoria), ATV-001 (4 nós de auditoria), COM-001 (9, 10) |
| — | INC-13 | SEQ-003 (`opt` "Não compareceu"), ATV-001 (`d5` [Não]), EST-001 (T6) |

---

## 7. Regras de manutenção desta matriz

1. Toda mudança em RF, RN, UC, operação, diagrama comportamental ou situação **atualiza esta matriz e a matriz da Fase 5 no mesmo commit** (plano §6.1 e §13).
2. Nenhuma mensagem pode ser adicionada a um diagrama comportamental sem uma linha nesta matriz (§2) — a regra é o mecanismo que impede "mensagem inventada para satisfazer um diagrama futuro".
3. Ao aprovar o grupo as propostas, as colunas `Sequência` da matriz da Fase 5 devem ser completadas com ATV/COM/EST e esta matriz deve ser **fundida** na matriz mestra (ou mantida como apêndice, o que é decisão do Integrante 5).
4. Mudança nas **situações** (`SOLICITADO`…`NAO_COMPARECEU`) altera simultaneamente: o enum do CLS-001 v2, o `CHECK` de `agendamento.status` (`IMP-DER-02` do I4) e o EST-001.
5. Mudança em **P-01** (confirmação automática síncrona × assíncrona) pode colapsar T1+T2 do EST-001 — a máquina já está desenhada para admitir esse colapso sem redesenhar o diagrama.

---

## 8. Índice dos artefatos rastreados

| Artefato | Arquivo | Especificação |
|---|---|---|
| SEQ-001 | `03-uml/Sequências/proposta-I2-sequencia-seq-001-autenticar.mmd` (+`.drawio.xml`, `.pdf`) | `03-uml/Sequências/proposta-I2-especificacao-sequencias.md` |
| SEQ-002 | `03-uml/Sequências/proposta-I2-sequencia-seq-002-agendar.mmd` (+`.drawio.xml`, `.pdf`) | idem |
| SEQ-003 | `03-uml/Sequências/proposta-I2-sequencia-seq-003-cancelar-confirmar.mmd` (+`.drawio.xml`, `.pdf`) | idem |
| ATV-001 | `03-uml/Atividade/proposta-I2-atividade-atv-001.mmd` (+`.drawio.xml`, `.pdf`) | `03-uml/Atividade/proposta-I2-especificacao-atividade.md` |
| COM-001 | `03-uml/Comunicação/proposta-I2-comunicacao-com-001.mmd` (+`.drawio.xml`, `.pdf`) | `03-uml/Comunicação/proposta-I2-especificacao-comunicacao.md` |
| EST-001 | `03-uml/Estados/proposta-I2-estados-est-001.mmd` (+`.drawio.xml`, `.pdf`) | `03-uml/Estados/proposta-I2-especificacao-estados.md` |
| Revisão | `01-planejamento/revisoes/FASE6-REV-I2-revisao-comportamental.md` | — |
| Dossiê | `07-dossie/secoes/fase6-secao-21-24.md` | — |
