# Especificação da máquina de estados EST-001 — Fase 6

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 22/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Natureza:** **PROPOSTA.** Não substitui artefato oficial do grupo.

| Artefato | Arquivo |
|---|---|
| Fonte editável (verdade do conteúdo) | [`proposta-I2-estados-est-001.mmd`](proposta-I2-estados-est-001.mmd) |
| Editável (draw.io) | `proposta-I2-estados-est-001.drawio.xml` |
| Dossiê (PDF) | `proposta-I2-estados-est-001.pdf` |
| Entidade modelada | `Agendamento` (CLS-001 v2) · atributo `-situacao` : `SituacaoAgendamento` |
| Base de requisitos | RF-025, RF-026, RF-027, RF-030 · RN-012, RN-013, RN-006 · plano §5.4 · INC-13 (Integrante 4) |

---

## 1. Objetivo

O plano da AV3 pede "a máquina de estados de Agendamento ou outra entidade relevante, incluindo eventos, condições e ações nas transições" (§6.1, Integrante 2). O EST-001 fixa o **ciclo de vida da situação do agendamento**, usando exatamente as **situações nomeadas** propostas na revisão FASE5-REV-I2 §6:

`SOLICITADO` · `CONFIRMADO` · `REALIZADO` · `CANCELADO` · `NAO_COMPARECEU`

Essas situações são o atributo `-situacao` do `Agendamento` e o enum `SituacaoAgendamento` do CLS-001 v2 — o que faz desta máquina **persistível** (o ponto de corte do plano: "estados x atributo/status persistível").

## 2. Estados

| Estado | Significado | Criado por | Pode sair? |
|---|---|---|---|
| `SOLICITADO` | Agendamento criado, ainda não confirmado. | `Agendamento.solicitar` (RF-025) | Sim (2 transições) |
| `CONFIRMADO` | Horário reservado e confirmado. | `Agendamento.confirmar` (OP-13, RN-013) | Sim (3 transições) |
| `REALIZADO` | Sessão realizada e registrada. | `Agendamento.concluir` (OP-17, RF-030) | **Não** (terminal) |
| `CANCELADO` | Agendamento cancelado. | `Agendamento.cancelar` (OP-14, RF-027) | **Não** (terminal) |
| `NAO_COMPARECEU` | O paciente não compareceu. | `Agendamento.registrarFalta` (OP-18) | **Não** (terminal) |

> A transição inicial `[*] → SOLICITADO` marca que **todo agendamento nasce solicitado** — não existe agendamento já confirmado, porque a confirmação é automática (RN-013) e ocorre no mesmo ato.

## 3. Transições: evento, condição e ação

| # | De → Para | Evento | Condição (guarda) | Efeito / ação | Origem |
|---|---|---|---|---|---|
| T1 | `[*]` → `SOLICITADO` | `solicitar` | vínculo ATIVO e slot livre | cria o agendamento | RF-025 / RN-005 / RN-010 |
| T2 | `SOLICITADO` → `CONFIRMADO` | `confirmar` | `[slot livre]` — a reserva do slot foi obtida | agenda o horário, marca CONFIRMADO | OP-13 / RN-013 |
| T3 | `SOLICITADO` → `CANCELADO` | `cancelar` | `[prazo]` — dentro do prazo de cancelamento | libera o horário, audita, notifica | OP-14/OP-15 / RN-012 |
| T4 | `CONFIRMADO` → `CANCELADO` | `cancelar` | `[prazo]` | libera o horário, audita, notifica | OP-14/OP-15 / RN-012 |
| T5 | `CONFIRMADO` → `REALIZADO` | `concluir` | `[atendimento]` — a sessão foi registrada | registra o atendimento, audita | OP-17 / OP-20 / RF-030 |
| T6 | `CONFIRMADO` → `NAO_COMPARECEU` | `registrarFalta` | — | audita | OP-18 / INC-13 |

### 3.1 Estados terminais

`REALIZADO`, `CANCELADO` e `NAO_COMPARECEU` **não têm saída**. Um agendamento não é "des-realizado" nem "des-cancelado": a correção é um novo agendamento. Isso preserva a **imutabilidade do histórico auditável** (RN-009/RNF-004) e evita que a trilha de auditoria precise de "desfazer" (INC-07: situacao anterior/nova).

### 3.2 A regra que a máquina deixa explícita

> **`NAO_COMPARECEU` só é alcançável a partir de `CONFIRMADO`** (T6), e **nunca a partir de `SOLICITADO`**.
>
> Justificativa: se o agendamento não foi confirmado, o horário não estava reservado de fato; registrar falta sobre um agendamento não confirmado registraria um fato falso no histórico. Esta é a restriction **INC-13** do Integrante 4, adotada aqui.

### 3.3 Cancelamento a partir de dois estados

O cancelamento é possível de `SOLICITADO` (T3) **e** de `CONFIRMADO` (T4). A guarda é a mesma em ambos (`[prazo]`, OP-15), e o efeito na agenda é o mesmo (devolver o slot). A duplicação existe porque **as duas transições têm origem diferente**: em T3 o slot ainda não estava reservado; em T4 ele estava. O plano §5.4 prevê exatamente este par.

## 4. Correspondência com os demais diagramas

| Transição | SEQ-002 | SEQ-003 | ATV-001 | COM-001 (msg.) |
|---|---|---|---|---|
| T1 | `Agendamento.solicitar` | — | `Criar agendamento` | 7 |
| T2 | `confirmar` [OP-13] | `confirmar` (`opt`) | `Confirmar (OP-13)` | 8 |
| T3 | — | `cancelar` (`opt`) | `Cancelar (OP-14)` | 12 |
| T4 | — | `cancelar` (`opt`) | `Cancelar (OP-14)` | 12 |
| T5 | — | `concluir` (`opt`) | `Concluir (OP-17)` | 13/14 |
| T6 | — | `registrarFalta` (`opt`) | `Registrar falta (OP-18)` | — |

**Nenhuma transição sem origem e nenhuma mensagem sem transição.** As quatro colunas da tabela acima são a evidência da comparação cruzada pedida pelo plano.

## 5. Decisões de modelagem

1. **As guardas são mostradas no rótulo da transição** entre colchetes (`[slot livre]`, `[prazo]`, `[atendimento]`) para que a máquina seja autossuficiente, e detalhadas nas notas do diagrama. O padrão é proposital: colchete = **condição**, sem colchete = **evento**.
2. **A transição inicial é sempre `solicitar`**: o plano §5.4 exige que toda entidade com situação declare o estado inicial, e `SOLICITADO` é o único ponto de entrada possível.
3. **`SOLICITADO` é um estado real, e não um detalhe de implementação**, porque a confirmação automática (RN-013) depende de P-01, ainda não decidido. Se o grupo decidir que a confirmação é síncrona e inseparável, T1 e T2 podem colapsar em uma só transição — **a máquina está pronta para essa mudança**.
4. **Não há estado "pendente de pagamento"**, "lista de espera" nem "sessão": estão fora do domínio (plano §5.2, P-07).

## 6. Lacunas registradas

| Lacuna | Causa | Efeito na máquina |
|---|---|---|
| Valor do prazo de cancelamento | DEC-012 / P-02 | A guarda `[prazo]` existe sem valor; T3/T4 são desenháveis, a condição não é. |
| Confirmação automática síncrona x assíncrona | P-01 | `SOLICITADO` pode ser transitório. |
| "Preencher encaixe" | DEC-010 | Não existe transição de encaixe. |
| Notificação | DEC-009 | A notificação é **efeito** da transição (T3/T4), não estado. |

## 7. Como ler e editar

- A **fonte editável** é o `.mmd` (Mermaid, `stateDiagram-v2`).
- As **hints de layout** `%% pos: <id> <x> <y>` posicionam o centro de cada estado (o Mermaid as ignora; o renderizador offline do repositório as usa — ver `03-uml/README.md`).
- As **notas** (`note left of` / `note right of`) explicam a situação inicial e as guardas.
- O `.drawio.xml` é a versão editável no draw.io; o `.pdf` é a versão do dossiê.
