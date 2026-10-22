# Especificação de objetos OBJ-002 — Cenário do ciclo de agendamento

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 21/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Arquivo de diagrama:** [`proposta-I2-diagrama-objetos-v2.mmd`](proposta-I2-diagrama-objetos-v2.mmd).
**Complementa (não substitui):** [`diagrama-objetos.mmd`](diagrama-objetos.mmd) e [`especificacao-objetos.md`](especificacao-objetos.md) — OBJ-001, do Integrante 3.

## 1. Objetivo

O OBJ-001 é uma fotografia parcial do **cadastro**: conta de acesso, vínculo e agenda com uma disponibilidade. Ele não mostra o processo central do projeto (agendar → confirmar → realizar/cancelar), nem nenhuma entidade que tenha **situação**.

O OBJ-002 completa a cobertura de objetos com o **ciclo de vida do agendamento**, incluindo as entidades com estado (`Agendamento`, `Vinculo`), o `Atendimento` e o `RegistroAuditoria`, de modo que:

1. a documentação mostre como os objetos do modelo se relacionam em um cenário real de uso;
2. os **nomes de situação** propostos na revisão FASE5-REV-I2 §6 fiquem visíveis e, portanto, revisáveis;
3. as regras RN-003, RN-005, RN-006, RN-007 e RN-009 possam ser conferidas sobre a fotografia.

## 2. Cenário

> Uma paciente fictícia, vinculada a um psicólogo fictício, agenda um horário liberado pelo profissional (**SOLICITADO → CONFIRMADO**). O atendimento é registrado (**REALIZADO**), com o registro de auditoria do responsável. Em outro horário, um agendamento é cancelado (**CANCELADO**) e **não** gera atendimento, conforme RN-006.

Recorte: **parcial** — a fotografia mostra apenas os objetos relevantes do cenário. A ausência de um objeto pode ser significativa (o agendamento cancelado **não** possui `Atendimento`).

## 3. Objetos do cenário

| Instância | Classe | Slots preenchidos | Papel no cenário |
|---|---|---|---|
| `contaPacienteCenario` | Usuario | `identificacao = CONTA-FICTICIA-C01`, `situacaoConta = ATIVO` | Conta de acesso do paciente (A04) |
| `contaProfissionalCenario` | Usuario | `identificacao = CONTA-FICTICIA-P01`, `situacaoConta = ATIVO` | Conta de acesso do profissional (A03) |
| `perfilPacienteCenario` | Perfil | — | Papel de acesso do paciente (A01) |
| `permissaoConsultaCenario` | Permissao | — | Permissão agregada ao perfil (A02) |
| `pacienteCenario` | Paciente | `situacaoCadastral = ATIVO` | Cadastro do paciente (A04) |
| `profissionalCenario` | Psicologo | — | Cadastro do profissional (A03) |
| `areaAtuacaoCenario` | AreaAtuacao | — | Área de atuação do profissional (A05) |
| `vinculoCenario` | Vinculo | `situacao = ATIVO`, `dataInicio = 2026-10-01` | Autorização de acesso entre os dois (A06/A07) |
| `agendaCenario` | Agenda | — | Agenda titular do profissional (A08) |
| `disponibilidadeCenario` | Disponibilidade | `periodo = FAIXA-FICTICIA 2026-11-03 14h00 a 15h00` | Período liberado pelo profissional (A09) |
| `agendamentoRealizadoCenario` | Agendamento | `situacao = REALIZADO`, `horario = 2026-11-03 14h00` | Sessão realizada (A10/A11/A12) |
| `agendamentoCanceladoCenario` | Agendamento | `situacao = CANCELADO`, `horario = 2026-11-05 09h00` | Sessão cancelada (A10/A11/A12) |
| `atendimentoCenario` | Atendimento | — | Registro conceitual do atendimento (A13/A14/A15) |
| `auditoriaConclusaoCenario` | RegistroAuditoria | `data = 2026-11-03`, `hora = 15h05`, `operacao = AGENDAMENTO_CONCLUIDO` | Trilha de auditoria da conclusão (A16) |
| `auditoriaCancelamentoCenario` | RegistroAuditoria | `data = 2026-11-02`, `hora = 10h20`, `operacao = AGENDAMENTO_CANCELADO` | Trilha de auditoria do cancelamento (A16) |
| `notificacaoEnviadaCenario` | **Mensagem (objeto de aplicação, não classe de domínio)** | `canal = EMAIL`, `referencia = AGENDAMENTO-REALIZADO-CENARIO` | Efeito do `<<include>>` de notificação (DEC-009 pendente) |

## 4. Ligações e o que cada uma demonstra

| Ligação | Associação | O que demonstra |
|---|---|---|
| `contaPacienteCenario` → `perfilPacienteCenario` | A01 | Conta e papel de acesso são objetos distintos (identidade ≠ autorização). |
| `perfilPacienteCenario` → `permissaoConsultaCenario` | A02 | Autorização por permissão (RNF-002). |
| `contaPacienteCenario` → `pacienteCenario` | A04 | A conta de acesso é distinta do cadastro do paciente (RF-014/RF-018). |
| `contaProfissionalCenario` → `profissionalCenario` | A03 | Idem para o profissional. |
| `profissionalCenario` → `areaAtuacaoCenario` | A05 | Área de atuação declarada (RF-016). |
| `profissionalCenario` / `pacienteCenario` → `vinculoCenario` | A06/A07 | O vínculo ATIVO sustenta RN-003 e RN-007. |
| `profissionalCenario` → `agendaCenario` | A08 | Agenda própria do profissional (RN-011). |
| `agendaCenario` → `disponibilidadeCenario` | A09 | Período pertence à agenda titular. |
| `agendaCenario` → os dois `agendamento` | A10 | Compromissos são apresentados na agenda do titular (RF-029). |
| `profissionalCenario` / `pacienteCenario` → os dois `agendamento` | A11/A12 | Cada agendamento tem exatamente um profissional e um paciente. |
| `profissionalCenario` / `pacienteCenario` → `atendimentoCenario` | A13/A14 | O atendimento referencia os participantes, não apenas o profissional. |
| `agendamentoRealizadoCenario` → `atendimentoCenario` | A15 | A sessão realizada corresponde a **um** atendimento (RF-030); sem isso, RN-006 ficaria invérido. |
| `disponibilidadeCenario` → `agendamentoRealizadoCenario` | A17 (condicional) | O slot ofertado reservado — só é desenhável se o grupo aprovar o modelo de **slot discreto** (P-05). |
| `contaProfissionalCenario` → `auditoriaConclusaoCenario`; `contaPacienteCenario` → `auditoriaCancelamentoCenario` | A16 | O responsável pela operação auditada é o usuário que a executou (RN-009, RN-014). |
| `notificacaoEnviadaCenario` → `agendamentoRealizadoCenario` | (aplicação) | A notificação referencia o agendamento; **não** é entidade de domínio (DEC-009 pendente). |

## 5. Regras verificáveis na fotografia

| Regra | Como aparece no OBJ-002 |
|---|---|
| RN-003 — acesso profissional condicionado a vínculo | O `Atendimento` só existe para o par ligado por `vinculoCenario` ATIVO. |
| RN-005 — exclusividade de horário ativo | Os dois agendamentos ocupam horários distintos; nenhum slot está reservado por dois agendamentos. |
| RN-006 — agendamento cancelado não gera atendimento | `agendamentoCanceladoCenario` **não** tem `Atendimento`; a ausência está anotada no diagrama. |
| RN-007 — vínculos obrigatórios do atendimento | `atendimentoCenario` referencia paciente e profissional ligados por `vinculoCenario`. |
| RN-009 — rastreabilidade de alterações relevantes | Duas trilhas de auditoria, uma por operação relevante, com responsável, data e hora. |
| RN-014 — imutabilidade lógica de auditoria | Os registros de auditoria não têm operação de alteração no modelo. |
| DEC-005 — nenhum dado real | Todos os identificadores são fictícios (`CONTA-FICTICIA-*`, `FAIXA-FICTICIA`); não há nome, CPF, e-mail, CRP ou conteúdo clínico. |
| DEC-008 — sem prontuário | O `Atendimento` é um registro conceitual, sem atributos de conteúdo clínico. |

## 6. O que **não** aparece (e por quê)

| Ausência | Motivo |
|---|---|
| Sessão autenticada | Não é entidade de domínio (P-07); pertence à camada de aplicação. |
| Classe/classe `Notificacao` do domínio | DEC-009 pendente e nenhum RF correspondente; a notificação aparece como objeto de aplicação. |
| Lista de espera / encaixe | DEC-010 pendente; sem RF e sem caso de uso (R5-14). |
| Multiplicidades nas ligações | A fotografia exemplifica; cardinalidade é definida no diagrama de classes (CLS-001) e na [matriz de rastreabilidade](../../06-rastreabilidade/matriz-rastreabilidade-fase5.md). |
| Atributos de identificação dos cadastros (nome, e-mail, documento) | DEC-005 — identificação por valores fictícios. |

## 7. Checklist de revisão do OBJ-002

- [ ] O grupo confirma os nomes das situações (`SOLICITADO`, `CONFIRMADO`, `REALIZADO`, `CANCELADO`, `NAO_COMPARECEU`) ou informa os substitutos.
- [ ] O grupo decide o modelo de tempo (P-05): com slot discreto, a ligação A17 entra oficialmente; com período contínuo, ela sai.
- [ ] O grupo decide DEC-009: se o serviço externo de e-mail for aprovado, o objeto de aplicação vira uma fronteira nomeada no diagrama de componentes (Fase 7).
- [ ] O Integrante 3 confirma se o `Atendimento` deve referenciar o `Agendamento` (OP-20) — a ligação A15 do OBJ-002 depende disso.
- [ ] Nenhum valor do diagrama pode ser rastreado a dados reais (DEC-005) — conferido na revisão.
