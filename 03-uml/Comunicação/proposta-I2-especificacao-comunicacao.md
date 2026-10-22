# Especificação do diagrama de comunicação COM-001 — Fase 6

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 22/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Natureza:** **PROPOSTA.** Não substitui artefato oficial do grupo.

| Artefato | Arquivo |
|---|---|
| Fonte editável (verdade do conteúdo) | [`proposta-I2-comunicacao-com-001.mmd`](proposta-I2-comunicacao-com-001.mmd) |
| Editável (draw.io) | `proposta-I2-comunicacao-com-001.drawio.xml` |
| Dossiê (PDF) | `proposta-I2-comunicacao-com-001.pdf` |
| Cenário | Agendar e concluir atendimento (UC-024 + UC-025 + UC-027) |
| Base de requisitos | RF-024, RF-025, RF-026, RF-027, RF-030, RF-032 · RN-003, RN-005, RN-009, RN-010, RN-013 |
| Diagrama correlato | SEQ-002, SEQ-003 e ATV-001 (mesmo cenário) |

---

## 1. Objetivo

O plano da AV3 pede "o diagrama de comunicação para um cenário relevante" (§6.1, Integrante 2). O COM-001 mostra **quais colaboradores trocam mensagens entre si** no cenário de agendamento, e **em que ordem**. É o diagrama que responde à pergunta "quem fala com quem", enquanto a sequência responde "o que acontece ao longo do tempo".

Por ser o mesmo cenário de ATV-001/SEQ-002/SEQ-003, o COM-001 funciona como **visão estrutural** (topologia de vínculos) do mesmo comportamento.

## 2. Cenário

> Um paciente solicita um horário a um psicólogo vinculado. O serviço de agenda consulta as permissões, o vínculo, a agenda e a disponibilidade; cria e confirma o agendamento; audita e notifica. Mais tarde o mesmo agendamento pode ser cancelado (com auditoria e notificação) ou concluído pelo psicólogo, com registro do atendimento.

## 3. Colaboradores (12 objetos)

| Objeto | Forma no desenho | Natureza | Papel no cenário |
|---|---|---|---|
| `Paciente` | estádio | fronteira (ator) | Solicita e cancela. |
| `Psicólogo` | estádio | fronteira (ator) | Conclui a sessão realizada. |
| `ServicoNotificacao` | estádio | **fronteira externa** (serviço de mensagens) | Envia a confirmação/cancelamento (AP-04). |
| `ServicoAgenda` | hexágono | **serviço de aplicação — hub** | Coordena regra, auditoria e notificação (AP-03). |
| `ServicoAuditoria` | hexágono | **serviço de aplicação** | Dispara o registro auditável (AP-02). |
| `Usuario` | retângulo | entidade de domínio | Autorização: o que o solicitante pode consultar (OP-04). |
| `Vinculo` | retângulo | entidade de domínio | Autorização do acesso ao paciente (OP-22). |
| `Agenda` | retângulo | entidade de domínio | Horários livres e verificação de slot (OP-11, RN-005/RN-010). |
| `Disponibilidade` | retângulo | entidade de domínio | Períodos liberados pelo profissional (A09). |
| `Agendamento` | retângulo | entidade de domínio | Criação, confirmação e cancelamento (OP-13, OP-14). |
| `Atendimento` | retângulo | entidade de domínio | Registro da sessão realizada (OP-20). |
| `RegistroAuditoria` | retângulo | entidade de domínio | Trilha auditável (RF-032). |

**Leitura das formas:** `[ ]` entidade de domínio · `{ }` serviço de aplicação · `( )` fronteira (ator ou serviço externo).

## 4. Vínculos e mensagens numeradas (1 a 14)

| # | Emissor → Receptor | Mensagem | Origem da autorização |
|---|---|---|---|
| 1 | `Paciente` → `ServicoAgenda` | `solicitar(horario)` | RF-025 / UC-024 |
| 2 | `ServicoAgenda` → `Usuario` | `consultarPermitidos(solicitante)` | OP-04 / RN-003 |
| 3 | `ServicoAgenda` → `Vinculo` | `estaAtivo()` | OP-22 / RN-005 |
| 4 | `ServicoAgenda` → `Agenda` | `consultarHorarios(paciente, periodo)` | OP-11 / RF-024 |
| 5 | `Agenda` → `Disponibilidade` | `slots livres` | A09 |
| 6 | `ServicoAgenda` → `Agenda` | `verificarDisponibilidade(horario)` | RN-005 / RN-010 |
| 7 | `ServicoAgenda` → `Agendamento` | `solicitar(psicologo, paciente, horario)` | `Agendamento.solicitar` / RF-025 |
| 8 | `Agendamento` → `ServicoAgenda` | `confirmar(solicitante)` | OP-13 / RN-013 |
| 9 | `ServicoAgenda` → `ServicoAuditoria` | `registrar(AGENDAMENTO_CONFIRMADO, ...)` | AP-02 / RN-009 |
| 10 | `ServicoAuditoria` → `RegistroAuditoria` | `registrar(responsavel, data, hora, operacao)` | RF-032 |
| 11 | `ServicoAgenda` → `ServicoNotificacao` | `notificar(confirmacao)` | AP-04 / DEC-009 |
| 12 | `ServicoAgenda` → `Agendamento` | `cancelar(solicitante, motivo)` | OP-14 / RN-012 |
| 13 | `Psicólogo` → `ServicoAgenda` | `concluir(agendamento)` | RF-030 / UC-027 |
| 14 | `ServicoAgenda` → `Atendimento` | `registrar(agendamento, contextoSessao)` | OP-20 / RF-030 |

> **Mensagens 4 e 6** percorrem **o mesmo vínculo** `ServicoAgenda`—`Agenda`: no diagrama foram desenhadas como duas setas numeradas para que a ordem (consultar → verificar) fique explícita. **Os retornos** (valores devolvidos pelos colaboradores) **não** geram seta própria: percorrem o vínculo no sentido inverso, como é a notação de comunicação.

## 5. Por que `ServicoAgenda` é o hub

O COM-001 mostra de forma gráfica a regra do plano §5.2: **não existe uma classe "Sistema" ou "GerenciadorGeral"**. O papel de coordinación é do **serviço de aplicação** `ServicoAgenda` (AP-03), que:

- **consulta** as entidades que possuem a regra (permissões, vínculo, agenda, disponibilidade);
- **aciona** as entidades que possuem o estado (`Agendamento`);
- **dispara** os efeitos transversais (auditoria, notificação) chamando os serviços que os executam (`ServicoAuditoria`, `ServicoNotificacao`) — em vez de auditar "de dentro" da entidade.

A mesma topologia vale para `ServicoAuditoria`, que **chama** `RegistroAuditoria` (mensagem 10) em vez de gravar a trilha por conta própria.

## 6. Vínculos com as sequências

| Mensagem do COM-001 | Sequência correspondente |
|---|---|
| 1, 4, 5, 6, 7, 9, 10, 11 | SEQ-002 (`solicitar`, `consultarHorariosDisponiveis`, `verificarDisponibilidade`, `Agendamento.solicitar`, `confirmar`, auditoria, notificação) |
| 12, 13, 14 | SEQ-003 (`cancelar`, `concluir`, `Atendimento.registrar`) |
| 2, 3 | SEQ-002 (`Vinculo.estaAtivo`); a autorização por permissões aparece aqui como OP-04, o que explicita RN-003 |

**Divergência registrada:** SEQ-001 usa a infraestrutura `RepositorioAutorizacao.obterPermissoes` (IR-02) para carregar as permissões da sessão, enquanto o COM-001 usa `Usuario.consultarPermitidos` (OP-04) para **autorização por operação**. São coisas diferentes: IR-02 monta a sessão (infraestrutura), OP-04 verifica, a cada operação, o que o solicitante pode acessar (regra de domínio). O cotejo está na revisão FASE6-REV-I2.

## 7. Decisões de modelagem

1. **`ServicoNotificacao` é fronteira, não entidade.** Com DEC-009 pendente, ela aparece como serviço externo (API de envio), exatamente como pede o plano §5.2 para componentes externos.
2. **A notificação sai do hub (mensagem 11) e não da entidade `Agendamento`.** Notificar é efeito de aplicação, não regra do domínio.
3. **`Usuario` aparece no COM-001** (e não nas sequências) porque o diagrama de comunicação destaca **vínculos**: mostrar que a autorização é uma *consulta* a uma entidade, e não uma condição implícita no ator, é um ganho de legibilidade que o diagrama de atividades não tem.
4. **Não há `Perfil`/`Permissao` como objetos.** O COM-001 é do cenário de agendamento; a estrutura de acesso (A01/A02) aparece no diagrama de classes e em SEQ-001.
5. **O objeto `Disponibilidade` é alcançado só por `Agenda`** (A09), coerente com a associação do CLS-001 v2 (a reserva A17 é condicional a P-05 e não é desenhável ainda).

## 8. Lacunas registradas

| Lacuna | Causa | Tratamento |
|---|---|---|
| Reserva do slot (A17) | P-05 | Não há mensagem entre `Disponibilidade` e `Agendamento`. |
| Canal de notificação | DEC-009 | `ServicoNotificacao` aparece como fronteira, sem canal. |
| Prazo de cancelamento | DEC-012 / P-02 | A mensagem 12 não carrega prazo. |
| Lista de espera / sessão | P-07 | Fora do domínio. |

## 9. Como ler e editar

- A **fonte editável** é o `.mmd` (Mermaid, `flowchart LR`).
- As **hints de layout** `%% pos: <id> <x> <y>` posicionam o centro de cada colaborador (o Mermaid as ignora; o renderizador offline do repositório as usa — ver `03-uml/README.md`).
- O `.drawio.xml` é a versão editável no draw.io; o `.pdf` é a versão do dossiê.
