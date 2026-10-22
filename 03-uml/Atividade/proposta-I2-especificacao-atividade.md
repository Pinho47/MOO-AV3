# Especificação do diagrama de atividades ATV-001 — Fase 6

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 22/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Natureza:** **PROPOSTA.** Não substitui artefato oficial do grupo.

| Artefato | Arquivo |
|---|---|
| Fonte editável (verdade do conteúdo) | [`proposta-I2-atividade-atv-001.mmd`](proposta-I2-atividade-atv-001.mmd) |
| Editável (draw.io) | `proposta-I2-atividade-atv-001.drawio.xml` |
| Dossiê (PDF) | `proposta-I2-atividade-atv-001.pdf` |
| Base de requisitos | RF-024, RF-025, RF-026, RF-027, RF-030, RF-032 · RN-003, RN-005, RN-009, RN-010, RN-012, RN-013 |
| Casos de uso | UC-024, UC-025, UC-027 |
| Diagrama correlato | SEQ-002, SEQ-003 (mesmo cenário) |

---

## 1. Objetivo

O plano da AV3 pede "o diagrama de atividades com decisões, responsabilidades e paralelismo quando aplicável" (§6.1, Integrante 2). O ATV-001 mostra **o ciclo completo do agendamento** — do pedido de horário até a conclusão, o cancelamento ou a falta — em **quatro pistas de responsabilidade**, com **quatro decisões**, **três blocos paralelos** e uma etapa **sequencial** de notificação do cancelamento (ver §5).

O cenário é o **mesmo** coberto por SEQ-002 e SEQ-003, justamente para permitir a comparação cruzada exigida pelo plano ("comparar todos os diagramas comportamentais entre si e corrigir divergências").

## 2. Cenário

> O paciente solicita um agendamento. O sistema consulta os horários disponíveis e, se houver, o paciente escolhe um. O sistema valida o vínculo e o horário, cria o agendamento e o confirma automaticamente, auditando e notificando em paralelo. Mais tarde, o agendamento pode ser **cancelado** (com liberação do horário e auditoria em paralelo, seguidas da notificação do cancelamento) ou **mantido**; no caso de mantido, o paciente comparece e a sessão é **concluída** (registro de atendimento e auditoria em paralelo) ou registrada como **falta** (com auditoria).

## 3. Pistas (swimlanes) e responsabilidades

| Pista | Elementos | Responsabilidade |
|---|---|---|
| **Paciente** | Início, Solicitar agendamento, Selecionar horário, Comparecer à sessão | Ações do usuário (fronteira externa). |
| **Sistema (aplicação e domínio)** | Consultar horários, Validar vínculo e horário, Criar agendamento, Confirmar, Cancelar, Manter, Liberar horário, Registrar falta, e as 4 decisões | Regras de negócio e orquestração (AP-03 + entidades de domínio). |
| **Psicólogo** | Registrar atendimento, Concluir | Ações do profissional (fronteira externa). |
| **Auditoria e notificação** | Auditar confirmação, Notificar confirmação, Auditar cancelamento, Notificar cancelamento, Auditar falta, Auditar conclusão | Efeitos transversais (AP-02 e AP-04). |

> A separação entre "Sistema" e "Auditoria e notificação" existe porque **auditoria e notificação não são regra de negócio**: são efeitos transversais disparados pela aplicação (AP-02, AP-04). Misturá-los à pista de regra é o que produz a "classe onipotente" vedada pelo plano §5.2.

## 4. Decisões

| Nó | Texto | Condição detalhada | Regra |
|---|---|---|---|
| `d1` | Há horários? | A agenda do psicólogo tem slots livres no período pedido | RF-024 / UC-024 |
| `d2` | Vínculo OK? | Vínculo ATIVO entre paciente e psicólogo **e** slot ainda livre | RN-005, RN-010 (A17 condicional a P-05) |
| `d3` | Cancelamento? | O pedido de cancelamento é aceito: agendamento não realizado, prazo em aberto | RF-027, RN-012, OP-15 (DEC-012 pendente) |
| `d5` | Sessão realizada? | O atendimento chegou a acontecer | RF-030, RN-006 |

As condições foram mantidas **curtas** no desenho (rótulos de losango precisam caber); o detalhe completo está na tabela acima e nas especificações dos casos de uso.

## 5. Blocos paralelos (fork/join)

| Bloco | Origem | Ramos paralelos | Join | Por que é paralelo |
|---|---|---|---|---|
| `fork1`/`join1` | após `Confirmar (OP-13)` | **par 1** auditar confirmação (RN-009) · **par 2** notificar confirmação (AP-04) | `join1` | A auditoria e o envio de mensagem são independentes entre si; a notificação é pós-commit (TRS-02 do I4). |
| `fork2`/`join2` | após `Cancelar (OP-14)` | **par 1** liberar horário · **par 2** auditar cancelamento (RN-009) | `join2` | Devolver o slot ao catálogo e registrar a trilha não se bloqueiam. |
| `fork3`/`join3` | após `Registrar atendimento (OP-20)` | **par 1** concluir (OP-17) · **par 2** auditar conclusão (RN-009) | `join3` | Concluir o agendamento e escrever a trilha são operações distintas. |

Além dos três blocos paralelos, há **uma etapa sequencial** depois do `join2`:

| Etapa | Posição | Origem | Por que é sequencial |
|---|---|---|---|
| `Notificar cancelamento (AP-04)` | entre `join2` e `Fim` | AP-04 / DEC-009 | A notificação do cancelamento só pode ser disparada **depois** que o cancelamento está gravado e auditado (pós-commit, TRS-02 do I4) e **antes** da resposta ao ator — é exatamente a ordem do SEQ-003 (`registrar` → `notificar` → resposta). Por isso ela **não** é um terceiro ramo do `fork2`. |

> **Ressalva técnica (importante para a persistência):** a notificação é *pos-commit* (TRS-02 do Integrante 4) e, por isso, **não** pode estar na mesma transação que grava o agendamento. O paralelismo do ATV-001 é **lógico** (independência de regra), não uma transação única.

## 6. Caminhos (rotas de leitura)

### 6.1 Caminho feliz

`Início → Solicitar agendamento → Consultar horários (OP-11) → d1 [Sim] → Selecionar horário → Validar vínculo e horário → d2 [Sim] → Criar agendamento → Confirmar (OP-13) → fork1 → (auditar ‖ notificar) → join1 → d3`

### 6.2 Sem horários (A1 de UC-025)

`d1 [Não] → Sem horários (A1) → Fim`

### 6.3 Conflito de horário (E1 de UC-025)

`d2 [Não] → Recusar (E1) → Fim`

### 6.4 Cancelamento

`d3 [Sim] → Cancelar (OP-14) → fork2 → (liberar horário ‖ auditar cancelamento) → join2 → Notificar cancelamento (AP-04) → Fim`

### 6.5 Sessão realizada

`d3 [Não] → Manter agendamento → Comparecer à sessão → d5 [Sim] → Registrar atendimento (OP-20) → fork3 → (concluir ‖ auditar conclusão) → join3 → Fim`

### 6.6 Não compareceu

`d5 [Não] → Registrar falta (OP-18) → Auditar falta (RN-009) → Fim`

## 7. Correspondência com as sequências (verificação cruzada)

| Nó do ATV-001 | SEQ-002 | SEQ-003 | Operação/responsabilidade |
|---|---|---|---|
| Consultar horários (OP-11) | `consultarHorariosDisponiveis` | — | OP-11 |
| Validar vínculo e horário | `estaAtivo` + `verificarDisponibilidade` | — | OP-22 + `Agenda.verificarDisponibilidade` |
| Criar agendamento | `Agendamento.solicitar` | — | `Agendamento.solicitar` |
| Confirmar (OP-13) | `confirmar` | `confirmar` (`opt`) | OP-13 / RN-013 |
| Auditar confirmação | `registrar("AGENDAMENTO_CONFIRMADO")` | `registrar("AGENDAMENTO_CONFIRMADO")` | AP-02 / RN-009 |
| Notificar confirmação (AP-04) | `notificar(confirmacao)` | — | AP-04 / DEC-009 |
| Cancelar (OP-14) | — | `podeSerCancelado` + `cancelar` | OP-15 + OP-14 |
| Auditar cancelamento | — | `registrar("AGENDAMENTO_CANCELADO")` | AP-02 |
| Notificar cancelamento (AP-04) | — | `notificar(cancelamento)` | AP-04 / DEC-009 |
| Liberar horário | — | *(implícito em RN-012)* | regra do catálogo de disponibilidade |
| Registrar atendimento (OP-20) | — | `Atendimento.registrar` | OP-20 / RF-030 |
| Concluir (OP-17) | — | `concluir` | OP-17 |
| Registrar falta (OP-18) | — | `registrarFalta` | OP-18 / INC-13 |
| Auditar falta | — | `registrar("AGENDAMENTO_NAO_COMPARECEU")` | AP-02 |

**Resultado:** nenhuma atividade do ATV-001 existe sem origem em uma operação ou responsabilidade conhecida; nenhuma mensagem de SEQ-002/SEQ-003 está sem atividade correspondente.

## 8. Decisões de modelagem e ressalvas

1. **Cancelamento e sessão realizada são alternativas**, não etapas em sequência: `d3` separa "o paciente cancela" de "o paciente mantém". O `Fim` existe em cada rota terminal, como manda a notação de atividades.
2. **"Liberar horário"** aparece apenas no cancelamento. O catálogo de disponibilidade é do domínio; a liberação é consequência da regra (RN-012/RN-010), não uma mensagem para um serviço de aplicação — por isso não há operação correspondente no CLS-001 v2, e isso está anotado na revisão.
3. **A notificação é condicional** (DEC-009): se o grupo decidir que não haverá notificação, o ramo *par 2* do `fork1` e a etapa `Notificar cancelamento` simplesmente saem do diagrama.
4. **A notificação do cancelamento é sequencial, não paralela.** O SEQ-003 mostra a ordem *auditar → notificar → responder ao ator*; o ATV-001 reproduz essa ordem com `Notificar cancelamento` entre o `join2` e o `Fim`. A correção é o achado **R6-05** da revisão da Fase 6 (ver `01-planejamento/revisoes/FASE6-REV-I2-revisao-comportamental.md` §6.2).
5. **Não há lista de espera nem sessão como atividade**: ambos estão fora do domínio (plano §5.2, P-07).
6. **`Fim` é um nó genérico** repetido nas rotas terminais — é a notação de atividade, não uma entidade.

## 9. Lacunas registradas

| Lacuna | Causa | Tratamento no desenho |
|---|---|---|
| "Preencher encaixe" | DEC-010 não decidido | Não aparece no ATV-001 (nenhuma atividade de encaixe). |
| Reserva do slot (A17) | P-05 não decidido | "Validar vínculo e horário" cobre a verificação; a reserva é interna a `Agendamento.solicitar`. |
| Prazo de cancelamento | DEC-012 / P-02 | `d3` registra a *decisão*, não o prazo; a nota fica na especificação. |
| Canal de notificação | DEC-009 | `Notificar confirmação (AP-04)` e `Notificar cancelamento (AP-04)` são fronteira externa (serviço de notificação), não entidade do domínio. |

## 10. Como ler e editar

- A **fonte editável** é o `.mmd` (Mermaid, `flowchart TD` com `subgraph` por pista).
- As **hints de layout** `%% pos: <id> <x> <y>` posicionam o centro de cada nó; o Mermaid as ignora como comentário, e o renderizador offline do repositório as usa (ver `03-uml/README.md`).
- As barras de paralelismo usam a forma `[||]`, desenhada como barra vertical.
- O `.drawio.xml` é a versão editável no draw.io; o `.pdf` é a versão do dossiê.
