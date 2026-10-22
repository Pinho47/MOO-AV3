# Fase 6 — Revisão dos diagramas comportamentais (Integrante 2)

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 22/10/2026 · **Versão:** v1.0 (rascunho para walkthrough do grupo).
**Tarefa do plano atendida:** Fase 6, bloco do Integrante 2 (plano §6.1):
criar pelo menos três diagramas de sequência para os casos de uso críticos aprovados; garantir que mensagens, retornos, alternativas e exceções sejam coerentes com os fluxos textuais; criar o diagrama de atividades com decisões, responsabilidades e paralelismo; criar o diagrama de comunicação para um cenário relevante; criar a máquina de estados de `Agendamento`; **comparar todos os diagramas comportamentais entre si e corrigir divergências**; atualizar a matriz de rastreabilidade com os diagramas.

---

## 1. Objetivo, escopo e limites desta revisão

Esta é uma **revisão de consistência comportamental**. Ela não redesenha o modelo de domínio (Fase 5) nem a persistência (Integrante 4): ela verifica se os **seis diagramas da Fase 6 contam a mesma história**, se toda mensagem tem origem em operação ou responsabilidade conhecida, e se os diagramas respeitam as decisões ainda pendentes do grupo.

Limites declarados:

- Não se altera nenhum artefato de fase anterior: o modelo de classes, os casos de uso, o diagrama de casos e as matrizes existentes permanecem como estão.
- Não se aprovam requisitos, regras ou decisões: as divergências são **registradas e propostas**, com a decisão do grupo explicitamente pendente.
- Não se criam mensagens "para satisfazer um diagrama futuro" (plano §8): toda mensagem nova desta fase remete a OP-xx, AP-xx ou IR-xx já Propostas na Fase 5.
- Não há dados reais (DEC-005) em nenhum artefato: os diagramas usam rótulos de operação e identificadores de regra, nunca nomes, documentos ou e-mails.

Anumeração de achados: **R6-xx** = achado desta revisão (local). Os demais prefixos são herdados da Fase 5 (**R5-xx**), do Integrante 3 (**PC-xx**), do Integrante 4 (**P-xx**, **INC-xx**, **TRS-xx**, **ARQ-xx**) e do grupo (**DEC-xxx**).

---

## 2. Fontes inspecionadas

| Artefato | Caminho | Uso nesta revisão |
|---|---|---|
| Plano de execução | `00-plano/` (PDF do plano) | Fase 6, §5.3, §5.4, §6.1, §13, §16, §16.1, §19 |
| Especificações dos casos críticos | `03-uml/Casos de uso/especificacoes-casos-de-uso-criticos.md` | UC-001, UC-021, UC-025, UC-027 — fluxos textuais |
| Especificações dos demais casos | `03-uml/Casos de uso/especificacoes-casos-de-uso-demais.md` | UC-024, UC-028, UC-029 (propostos), UC-032, UC-033 |
| Requisitos funcionais | `02-requisitos/requisitos-funcionais.md` | RF-001 a RF-032 |
| Regras de negócio | `02-requisitos/regras-negocio.md` | RN-001 a RN-014 |
| Critérios de aceite | `02-requisitos/criterios-aceite.md` | CA-RF-xxx |
| Modelo de classes (oficial) | `03-uml/Classes/diagrama-classes.mmd` | Operações existentes, multiplicidades `PENDENTE` |
| Modelo de classes (proposta I2) | `03-uml/Classes/proposta-I2-diagrama-classes-v2.mmd` | OP-01…OP-22, enum `SituacaoAgendamento` |
| Revisão da Fase 5 | `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md` | §5.2 (AP-01…AP-05), §5.3 (mensagens previstas), §6.1 (transições), §9 (não proposto) |
| Análise de impacto comportamental (I4) | `04-persistencia/analises/fase-06-impacto-comportamental-persistencia.md` | INC-13, TRS-02, ARQ-07, operações de infraestrutura |
| Matriz da Fase 5 | `06-rastreabilidade/matriz-rastreabilidade-fase5.md` | Colunas herdadas (classe, operação, tabela, CT) |
| Decisões do grupo | `01-planejamento/decisoes.md` | DEC-001…DEC-010 (DEC-009 e DEC-010 pendentes) |

---

## 3. Método e critérios

O método foi o de **confrontar cada artefato comportamental contra os outros**, na ordem que o plano §6.1 pede:

1. **Teste de elegibilidade da mensagem** — toda mensagem (sequência, comunicação) e toda atividade devem remeter a uma operação OP-xx, a uma responsabilidade de aplicação AP-xx ou a uma interface de infraestrutura IR-xx já Propostas na Fase 5. Mensagem sem origem = **defeito**.
2. **Teste de cobertura do caso crítico** — para cada caso de uso crítico (UC-001, UC-021, UC-025, UC-027), o conjunto {SEQ, ATV, COM, EST} deve cobrir **todos os passos observáveis** do fluxo textual, incluindo **alternativas** e **exceções**.
3. **Teste de coerência cruzada** — um mesmo comportamento (ex.: "confirmar o agendamento") deve aparecer com **o mesmo nome de operação**, **a mesma regra** e **o mesmo efeito** nos quatro diagramas.
4. **Teste de correspondência com a persistência** — as operações de infraestrutura citadas nos diagramas do Integrante 4 (`consultarPorLogin`, `compararHash`, `verificarSituacao`, `obterPermissoes`, `notificar`) devem ter um papel inequívoco frente às operações de domínio OP-xx.
5. **Teste de não invenção** — nenhum diagrama pode conter mensagem, estado ou atividade que não esteja ancorada em RF/RN/UC/DEC.

Veredictos usados: **OK** · **DIVERGENTE** (corrigido nesta fase) · **PENDENTE** (decisão do grupo) · **NÃO DESSENHÁVEL** (bloqueado por decisão).

---

## 4. Resultado 1 — Cobertura dos casos de uso críticos pelos diagramas

### 4.1 UC-001 — Autenticar usuário

| Passo do fluxo (fonte: UC-001) | Artefato | Veredicto | Observação |
|---|---|---|---|
| Usuário informa identificação e senha | SEQ-001, msg. 1 (`autenticar`) | **OK** | AP-01 — a entidade não autentica (plano §5.2). |
| Sistema busca a conta cadastrada | SEQ-001, msg. 2 (`obterPorIdentificacao`) | **OK** | IR-01 — infraestrutura, Fase 7. Ver R6-02. |
| Sistema valida se a conta está ativa/bloqueada | SEQ-001, msg. 3 (`podeAutenticar`) | **OK** | OP-01 — RN-001 materializado na entidade. Ver R6-03. |
| Sistema confere a credencial | SEQ-001, msg. 4 (`confereCredencial`) | **OK** | OP-02 — RN-002. Ver R6-01. |
| Sistema carrega perfil e permissões | SEQ-001, msg. 5 (`obterPermissoes`) | **OK** | IR-02 — sem divergência com o I4. |
| Sistema cria a sessão | SEQ-001, msg. 6 (`criarSessao`) | **OK** | AP-01 — sessão fora do domínio (P-07). |
| E1 — credencial inválida → recusa + auditoria | SEQ-001, `alt` "E1 - Credencial inválida" | **OK** | Recusa explícita ao ator **e** auditoria (`LOGIN_NEGADO`). |
| E2 — conta bloqueada → recusa + auditoria | SEQ-001, `alt` "E2 - Conta bloqueada ou inativa" | **OK** | Idem, com guarda RN-001. |
| A1 — encerrar sessão | SEQ-001, `opt` A1 | **OK** | `invalidarSessaoAtiva` (AP-01). |

**Conclusão:** UC-001 está integralmente coberto por SEQ-001. **Não há** diagrama de atividades, de comunicação ou de estados para a autenticação — e **não deve haver**: o objeto de interesse da Fase 6 é o ciclo de agendamento.

### 4.2 UC-021 — Vincular psicólogo a paciente

| Passo do fluxo | Artefato | Veredicto | Observação |
|---|---|---|---|
| Administrador seleciona o profissional e o paciente | — | **PENDENTE** | Depende de OP-05/OP-08 (consultas administrativas), fora do escopo comportamental crítico. |
| Sistema formaliza o vínculo | SEQ-002 (uso de `estaAtivo`), ATV-001 (guarda de `d2`) | **OK (parcial)** | O vínculo aparece como **autorização consultada**, não como fluxo de cadastro — coerente com RN-003/RN-005. |
| Sistema encerra o vínculo | — | **PENDENTE** | OP-21 (`encerrar`) — fora do escopo desta fase; sem situação no modelo oficial (PC-06). |
| Auditoria do ato | COM-001 (msg. 9/10, por analogia) | **OK (modelo)** | O padrão "serviço → auditoria → registro" é o mesmo em todos os atos auditáveis. |

**Conclusão:** UC-021 é **cuberto transversalmente** (o vínculo é condição de autorização no ciclo de agendamento) e **não é coberto longitudinalmente**. Registrado como lacuna de escopo, não como defeito — o plano §6.1 pede sequências "para os casos de uso **críticos aprovados**", e aivialização do vínculo está coberta pelo modelo de classes e pelo diagrama de objetos.

### 4.3 UC-025 — Solicitar agendamento (com confirmação automática)

| Passo do fluxo | SEQ-002 | ATV-001 | COM-001 | EST-001 | Veredicto |
|---|---|---|---|---|---|
| Passo 1 — paciente consulta horários livres | `consultarHorariosDisponiveis` (OP-11) | `Consultar horários (OP-11)` + `d1` | msg. 4, 5 | — | **OK** |
| Passo 2 — paciente escolhe o horário | — | `Selecionar horário` | — | — | **OK** |
| Passo 3 — sistema valida vínculo e disponibilidade | `estaAtivo` (OP-22) + `verificarDisponibilidade` | `Validar vínculo e horário` + `d2` | msg. 3, 6 | guarda de T1 | **OK** |
| Passo 4 — sistema cria o agendamento | `Agendamento.solicitar` | `Criar agendamento` | msg. 7 | T1 (`[*] → SOLICITADO`) | **OK** |
| Passo 5 — confirmação automática | `confirmar` (OP-13) | `Confirmar (OP-13)` | msg. 8 | T2 | **OK** |
| Passo 6 — auditoria | `registrar("AGENDAMENTO_CONFIRMADO")` | `Auditar confirmação (RN-009)` | msg. 9, 10 | efeito de T2 | **OK** |
| Passo 7 — notificação | `notificar(confirmacao)` | `Notificar confirmação (AP-04)` (ramo *par 2* do `fork1`) | msg. 11 | efeito de T2 | **OK (condicional a DEC-009)** |
| A1 — não há horário disponível | `alt` "Nenhum horário disponível" | `d1` [Não] → `Sem horários (A1)` | — | — | **OK** |
| E1 — conflito de horário | `alt` "E1 - Conflito de horário" | `d2` [Não] → `Recusar (E1)` | msg. 6 (retorno falso) | — | **OK** |

**Conclusão:** UC-025 está coberto nos **quatro** diagramas, e os quatro contam exatamente a mesma história. A confirmação automática (RN-013) é desenhada como **operação do `ServicoAgenda`**, e não como passo do ator — coerente com RN-013 ("ator **ou mecanismo** autorizado").

### 4.4 UC-027 — Cancelar agendamento

| Passo do fluxo | SEQ-003 | ATV-001 | COM-001 | EST-001 | Veredicto |
|---|---|---|---|---|---|
| Ator consulta seus agendamentos | `consultarMeus` (OP-19) | — | — | — | **OK** |
| Ator solicita o cancelamento | `cancelar(agendamento, motivo)` | `Cancelar (OP-14)` | msg. 12 | evento `cancelar` | **OK** |
| Sistema verifica se pode cancelar | `podeSerCancelado` (OP-15) | `d3` | — | guardas `[prazo]` de T3/T4 | **OK** |
| Sistema cancela | `cancelar(solicitante, motivo)` (OP-14) | — | msg. 12 | T3, T4 | **OK** |
| Auditoria do cancelamento | `registrar("AGENDAMENTO_CANCELADO")` | `Auditar cancelamento (RN-009)` | msg. 9, 10 | efeito de T3/T4 | **OK** |
| Notificação do cancelamento | `notificar(cancelamento)` | `Notificar cancelamento (AP-04)` (etapa sequencial após o `join2` — ver R6-05) | msg. 11 | efeito de T3/T4 | **OK** |
| E1 — já realizado ou já cancelado | `alt` "E1 - Agendamento já realizado ou já cancelado" | `d3` [Não] → `Manter agendamento` | — | estados terminais | **OK** |

**Conclusão:** UC-027 está coberto. A recusa (E1) aparece **nos dois diagramas** (sequência: `alt`; atividade: ramo "Não" do `d3`), com o mesmo significado.

---

## 5. Resultado 2 — Comparação cruzada entre os seis diagramas

### 5.1 Matriz de coerência por comportamento

| Comportamento | Nome da operação (em todos os diagramas) | Regra | Seq | ATV | COM | EST | Coerente? |
|---|---|---|---|---|---|---|---|
| Buscar a conta | `obterPorIdentificacao` | — | SEQ-001 (IR-01) | — | — | — | **OK** (só SEQ-001) |
| Verificar se a conta pode autenticar | `podeAutenticar` (OP-01) | RN-001 | SEQ-001 | — | — | — | **OK** |
| Conferir a credencial | `confereCredencial` (OP-02) | RN-002 | SEQ-001 | — | — | — | **OK** |
| Carregar permissões da sessão | `obterPermissoes` (IR-02) | RF-007/008 | SEQ-001 | — | — | — | **OK** |
| Auditar | `registrar(...)` | RN-009 | SEQ-001/002/003 | 4 nós de auditoria | msg. 9, 10 | efeito das transições | **OK** |
| Consultar horários disponíveis | `consultarHorariosDisponiveis` (OP-11) / `consultarHorarios` (COM) | RN-010 | SEQ-002 | `Consultar horários (OP-11)` | msg. 4 | — | **OK** (abreviação coerente) |
| Verificar o slot | `verificarDisponibilidade` | RN-010 | SEQ-002 | `Validar vínculo e horário` | msg. 6 | guarda de T1 | **OK** |
| Verificar o vínculo | `estaAtivo` (OP-22) | RN-003/RN-005 | SEQ-002 | `d2` | msg. 3 | guarda de T1 | **OK** |
| Criar o agendamento | `Agendamento.solicitar` | RF-025 | SEQ-002 | `Criar agendamento` | msg. 7 | T1 | **OK** |
| Confirmar | `confirmar(solicitante)` (OP-13) | RN-013 | SEQ-002, SEQ-003 | `Confirmar (OP-13)` | msg. 8 | T2 | **OK** |
| Cancelar | `cancelar(solicitante, motivo)` (OP-14) | RN-012 | SEQ-003 | `Cancelar (OP-14)` | msg. 12 | T3, T4 | **OK** |
| Pode cancelar | `podeSerCancelado()` (OP-15) | RN-012 | SEQ-003 | `d3` | — | guarda `[prazo]` | **OK** |
| Concluir | `concluir(solicitante)` (OP-17) | RF-030, RN-006 | SEQ-003 | `Concluir (OP-17)` | msg. 13 | T5 | **OK** |
| Verificar se foi cancelado | `estaCancelada()` (OP-16) | RN-006 | SEQ-003 | — | — | — | **OK** (só SEQ-003) |
| Registrar atendimento | `registrar(agendamento, contextoSessao, solicitante)` (OP-20) | RF-030 | SEQ-003 | `Registrar atendimento (OP-20)` | msg. 14 | efeito de T5 | **OK** |
| Registrar falta | `registrarFalta(solicitante)` (OP-18) | INC-13 | SEQ-003 | `Registrar falta (OP-18)` | — | T6 | **OK** |
| Notificar | `notificar(...)` (AP-04) | DEC-009 | SEQ-002, SEQ-003 | `Notificar confirmação (AP-04)`, `Notificar cancelamento (AP-04)` | msg. 11 | efeito das transições | **OK** |

**Resultado: 17 comportamentos comparados, 0 divergências de nome, 0 divergências de regra, 0 divergências de efeito.**

### 5.2 Matriz de cobertura dos quatro diagramas por requisito

| RF | SEQ | ATV | COM | EST | Total |
|---|---|---|---|---|---|
| RF-001, RF-002 | ✔ | — | — | — | 1 |
| RF-024 | ✔ | ✔ | ✔ | — | 3 |
| RF-025 | ✔ | ✔ | ✔ | ✔ | 4 |
| RF-026 | ✔ (2) | ✔ | ✔ | ✔ | 5 |
| RF-027 | ✔ | ✔ | ✔ | ✔ (2) | 4 |
| RF-028 | ✔ | — | — | — | 1 |
| RF-030 | ✔ | ✔ | ✔ | ✔ | 4 |
| RF-032 | ✔ (3) | ✔ | ✔ | ✔ (efeito) | 5 |

### 5.3 Verificação de não invenção (teste do plano §8)

| Verificação | Resultado |
|---|---|
| Mensagem de sequência sem OP/AP/IR | **0** (82 setas auditadas: 49 solicitações e 33 retornos; 35 mensagens de solicitação distintas) |
| Atividade sem origem em operação | **0** (26 atividades em pista + 5 nós `Fim`, de 37 caixas do diagrama) |
| Mensagem de comunicação sem origem | **0** (14 mensagens auditadas) |
| Transição de estado sem operação | **0** (6 transições auditadas) |
| Estado sem atributo persistível | **0** (`SituacaoAgendamento` = `agendamento.status`) |
| Operação nova criada nesta fase | **0** — a Fase 6 **não** propôs operação alguma; ela só *usou* OP-01…OP-22, AP-01…AP-05 e IR-01/IR-02 |
| Entidade nova criada nesta fase | **0** — nenhum diagrama introduziu classe |
| Dado real exposto | **0** (DEC-005 respeitado: só rótulos de operação, regra e papel) |

---

## 6. Resultado 3 — Cotejo com a análise de persistência do Integrante 4

O Integrante 4 صفحة uma análise de impacto comportamental (`04-persistencia/analises/fase-06-impacto-comportamental-persistencia.md`) com operações de infraestrutura que precisam de um papel inequívoco frente às operações de domínio OP-xx. Este é o cotejo.

| # | Operação do I4 | Onde entra na Fase 6 | Veredicto | Encaminhamento |
|---|---|---|---|---|
| 1.1 | `consultarPorLogin(login)` — repositório de usuário | SEQ-001, msg. 2 — `obterPorIdentificacao(identificacao)` (IR-01) | **DIVERGENTE (nomenclatura)** | **R6-02**: mesma responsabilidade (carregar a conta). O nome do I4 usa "login"; o modelo (CLS-001 v2) tem o atributo `Usuario.identificacao` e RF-001 fala em "credenciais". **Proposta:** unificar em `obterPorIdentificacao(identificacao)` — ou, se o grupo preferir "login", alterar o atributo e o RF. Enquanto não houver decisão, os dois nomes coexistem com o mesmo significado, e isso está anotado na especificação de SEQ-001. |
| 1.2 | `compararHash(hashArmazenado, senha)` — serviço de autenticação (domínio) | SEQ-001, msg. 4 — `confereCredencial(senha)` (OP-02, entidade `Usuario`) | **DIVERGENTE (camada)** | **R6-01**: os dois concordam que a comparação **não** acontece no repositório (RN-002) e sim no domínio. A diferença é *quem* compara: serviço de autenticação × entidade `Usuario`. A Fase 5 já fixou o princípio de que **a entidade decide** ("se pode" e "se a credencial confere"), e o atributo `credencialProtegida` é privado. **Proposta:** manter OP-02 na entidade e atualizar a análise do I4 para `Usuario.confereCredencial` + coluna `usuario.hash_senha` (que o I4 já prevê). |
| 1.3 | `verificarSituacao(usuario)` — repositório de usuário | SEQ-001, msg. 3 — `podeAutenticar()` (OP-01, entidade `Usuario`) | **DIVERGENTE (camada)** | **R6-03**: regra de negócio no repositório é o padrão que o plano §5.2 recusa (repositório carrega, entidade decide). **Proposta:** o repositório carrega `situacaoConta`; a entidade decide, com RN-001 materializado em `podeAutenticar()`. Esta é a leitura que também sustenta `Vinculo.estaAtivo` (OP-22). |
| 1.4 | `obterPermissoes(idUsuario)` — repositório de autorização | SEQ-001, msg. 5 — `obterPermissoes(idUsuario)` (IR-02) | **OK — sem divergência** | Mantido exatamente. *Atenção:* é **diferente** do `consultarPermitidos` (OP-04) que aparece na mensagem 2 do COM-001 — ver R6-04. |
| 2.6 | `notificar(destinatario, evento)` — serviço externo | SEQ-002/SEQ-003 — `notificar(confirmacao)` / `notificar(cancelamento)` (AP-04); COM-001 msg. 11; ATV-001 `Notificar confirmação (AP-04)` e `Notificar cancelamento (AP-04)` | **OK — nome do argumento a harmonizar** | **R6-06**: o parâmetro do I4 é `evento`; o diagrama é mais específico (`confirmacao` / `cancelamento`). A especificação do I4 descreve o mesmo envio. **Proposta:** nomear o parâmetro `evento` na interface `ServicoEnvioMensagem` e manter `confirmacao`/`cancelamento` como **valores** do evento. |
| — | **TRS-02** — notificação fora da transação de escrita (pós-commit) | SEQ-002/003 (a notificação aparece **depois** da auditoria e da resposta do `Agendamento`); ATV-001 (fork1, ramo *par 2*) | **OK — alinhado** | **R6-07**: os diagramas **não desenham fronteira de transação** (não há marcações de commit). Isso é uma escolha consciente: a fronteira transacional é responsabilidade da camada de persistência (Fase 7). O que os diagramas garantem é a **ordem**: a notificação é disparada **após** a gravação e a auditoria. |
| — | **INC-13** — "Não compareceu" alcançável apenas a partir de "Confirmado" | EST-001, transição **T6** (`CONFIRMADO → NAO_COMPARECEU`); nota do diagrama; SEQ-003 (`opt` "Não compareceu"); ATV-001 (`d5` [Não]) | **OK — INC-13 resolvido** | A INC-13 estava **aberta** ("Indefinido"). A Fase 6 a **fecha por desenho**: não existe transição de `SOLICITADO` para `NAO_COMPARECEU`. Isso responde também à pergunta **P-14** do I4. |
| — | **ARQ-07** — *outbox* para eventos pós-commit | Não aparece nos diagramas | **PENDENTE / fora de escopo** | A *outbox* é infraestrutura de Fase 7. **R6-08**: se o grupo decidir pela *outbox*, o ATV-001 ganha uma atividade "registrar evento de saída" na pista de auditoria; o desenho atual já separa o ramo *par 2* do `fork1`, o que facilita a inclusão sem redesenhar. |

### 6.1 Achados desta revisão (R6-01 … R6-08)

| ID | Sev. | Achado | Evidência | Ação recomendada | Responsável |
|---|---|---|---|---|---|
| R6-01 | M | `compararHash` (serviço) × `confereCredencial` (entidade `Usuario`): a comparação de credencial aparece em duas camadas | I4 §1.2 × CLS-001 v2 (OP-02) × SEQ-001 msg. 4 | Unificar em OP-02 na entidade; atualizar a análise do I4 | I4 + I3 |
| R6-02 | B | `consultarPorLogin` (I4) × `obterPorIdentificacao` (IR-01): mesmo papel, nomes diferentes | I4 §1.1 × SEQ-001 msg. 2 | Unificar o nome no glossário de infraestrutura | I4 + I2 |
| R6-03 | M | `verificarSituacao` no repositório (I4) × `podeAutenticar` na entidade (OP-01): regra de negócio no repositório | I4 §1.3 × CLS-001 v2 (OP-01) × SEQ-001 msg. 3 | Repositório carrega, entidade decide | I4 + I3 |
| R6-04 | M | `obterPermissoes` (IR-02, montar a sessão) × `consultarPermitidos` (OP-04, autorizar uma operação): nomes parecidos, papéis diferentes | SEQ-001 msg. 5 × COM-001 msg. 2 | Manter os dois nomes; documentar a distinção (o glossário do dossiê deve dizer "permissões da sessão" × "acesso permitido a esta operação") | I2 + I3 |
| R6-05 | B | No ATV-001, o ramo *par 2* do `fork2` (cancelamento) é "auditar cancelamento", mas **não** há "notificar cancelamento" — enquanto o SEQ-003 notifica o cancelamento | ATV-001 `fork2` × SEQ-003 msg. 5 | **Corrigido nesta fase**: ver §6.2 | I2 |
| R6-06 | B | Nome do parâmetro da notificação: `evento` (I4) × `confirmacao`/`cancelamento` (diagramas) | I4 §2.6 × SEQ-002/003 | Harmonizar: parâmetro `evento`, valor `confirmacao`/`cancelamento` | I4 + I2 |
| R6-07 | B | Os diagramas não marcam fronteira de transação, o que pode ser lido como "notificação na mesma transação" | SEQ-002/003, ATV-001 fork1 | Manter; registrar em nota que a fronteira transacional é da Fase 7 (TRS-02) | I2 + I4 |
| R6-08 | M | **R6-08 — pendência herdada:** a análise do I4 da Fase 5 (R5-13) foi feita sobre a lista do plano §5.1, não sobre o modelo de classes real; os diagramas da Fase 6 assume OP-01…OP-22, que ainda **não** estão no diagrama oficial | I4 §1 e R5-13 | Refazer a análise contra o modelo aprovado antes do DER | I4 |

> **Nota de numeração:** R6-08 reativa o achado R5-13, porque ele **bloqueia a Fase 7** e agora tem consequência direta nos diagramas: enquanto o Integrante 3 não incorporar OP-01…OP-22, os diagramas da Fase 6 estão apoiados em um modelo que ainda não é o oficial.

### 6.2 Correção aplicada nesta fase (R6-05)

O SEQ-003 mostra, no caminho de cancelamento, a sequência **auditar → notificar → responder ao ator**. O ATV-001, no `fork2`, mostrou **liberar horário ‖ auditar cancelamento**, sem nenhum passo de notificação. Isso é uma **divergência real de comportamento** entre dois diagramas do mesmo conjunto.

**Correção:** o ATV-001 passou a ter uma etapa **sequencial** `Notificar cancelamento (AP-04)` entre o `join2` e o `Fim` do bloco de cancelamento:

| Bloco | Origem | Ramos paralelos / etapa seguinte |
|---|---|---|
| `fork1`/`join1` | após `Confirmar (OP-13)` | par 1 auditar confirmação ‖ **par 2** notificar confirmação (AP-04) |
| `fork2`/`join2` | após `Cancelar (OP-14)` | par 1 liberar horário ‖ par 2 auditar cancelamento → **join2 → Notificar cancelamento (AP-04)** |
| `fork3`/`join3` | após `Registrar atendimento (OP-20)` | par 1 concluir ‖ par 2 auditar conclusão |

A notificação do cancelamento **não** é um terceiro ramo do `fork2`, e a escolha é deliberada: no SEQ-003 a ordem é *auditar → notificar → responder*, e o envio de mensagem só pode acontecer depois que o cancelamento está gravado e auditado (pós-commit, TRS-02 do Integrante 4). Modelá-la como ramo paralelo do `fork2` admitiria a leitura de que a notificação corre antes da auditoria — o que contradiz o SEQ-003. Por isso ela é **sequencial depois do `join2`**, e o paralelismo do bloco de cancelamento continua sendo apenas "devolver o horário ‖ registrar a trilha".

Com isso, os **dois** eventos notificáveis da Fase 6 (confirmação e cancelamento) têm lugar nos dois diagramas, e a contagem final do ATV-001 é: **4 decisões, 3 blocos paralelos, 1 etapa sequencial de notificação, 4 pistas**.

---

## 7. Resultado 4 — Lacunas e decisões que bloqueiam o fechamento da Fase 6

| ID | Lacuna | Causa | Efeito nos diagramas | Quem fecha |
|---|---|---|---|---|
| **LAC-01** | **SEQ-004 — "Preencher encaixe"** não foi produzido | DEC-010 (inovação) pendente; "encaixe" não está definido em nenhum RF/UC | A sequência **não existe**. O caso de uso do módulo de inovação (UC-034) **não tem** representação comportamental | Grupo (DEC-010) |
| **LAC-02** | Reserva do slot (A17 `Disponibilidade`—`Agendamento`) | P-05 (slot discreto × intervalo) pendente | Não há mensagem de reserva; a reserva é interna a `Agendamento.solicitar` e a verificação aparece como `verificarDisponibilidade` | Grupo (P-05) |
| **LAC-03** | Prazo e atores do cancelamento | DEC-012 / P-02 pendentes | A guarda `[prazo]` existe sem valor em T3/T4 e em `d3`; o ator é "Paciente ou Psicólogo" | Grupo (DEC-012) |
| **LAC-04** | Canal de notificação (e-mail?) | DEC-009 pendente | `ServicoNotificacao` é fronteira externa (interface); sem canal, sem tabela | Grupo (DEC-009) |
| **LAC-05** | Mecanismo da confirmação (síncrono × assíncrono) | P-01 pendente | `SOLICITADO` pode ser transitório; T1+T2 podem colapsar sem redesenhar o EST-001 | Grupo (P-01) |
| **LAC-06** | "Não compareceu": manual ou automático por prazo? | DEC-5 citado nos casos × INC-13 | O EST-001 tem o **estado**; o **gatilho** (quem dispara `registrarFalta`) não está fixado | Grupo (DEC-005/011) |
| **LAC-07** | UC-027 cita **DEC-011 e DEC-012**, que não existem no registro oficial (R5-12) | Inconsistência herdada da Fase 3 | Os diagramas referenciam "DEC-012" como proposta; se o grupo numerar diferente, é renomeação | I1 + grupo |
| **LAC-08** | Multiplicidades ainda `PENDENTE` no diagrama oficial | R5-10 / PC-09 | Os diagramas da Fase 6 **não dependem** de multiplicidades (a regra de exclusividade RN-005 é citada por RN/RF, não por cardinalidade) | Grupo + I3 |
| **LAC-09** | "`consultarMeus` (OP-19) e o RF-028 sem caso de uso próprio" | R5-17 | O SEQ-003 inclui a consulta como **passo preparatório** do cancelamento, marcado como UC-028 (proposto) | I1 + I2 |

---

## 8. Checklist do plano §13 aplicado à Fase 6

| Regra | Verificação | Resultado |
|---|---|---|
| Todo ator do diagrama de casos está definido na documentação | Ator, Paciente, Psicólogo, Administrador | **OK** — só `Ator` (genérico) aparece nas sequências; o papel concreto vem do caso de uso |
| Todo caso de uso possui requisito/objetivo rastreável | UC-001, UC-024, UC-025, UC-027 | **OK** — UC-024 e UC-028 são propostas do I2 (matriz da Fase 5 §1) |
| Conceitos relevantes dos casos aparecem nos diagramas | 12 classes do modelo | **OK** — 7 das 12 classes aparecem como participante ou colaborador: `Usuario` (SEQ-001, COM-001), `Vinculo` (SEQ-002, COM-001, ATV-001), `Agenda` (SEQ-002, COM-001, ATV-001), `Agendamento` (SEQ-002/003, COM-001, ATV-001, EST-001), `Atendimento` (SEQ-003, COM-001, ATV-001) e `RegistroAuditoria` (SEQ-001/002/003, COM-001, ATV-001); `Disponibilidade` é retornada em SEQ-002 e é colaborador no COM-001. `Perfil` e `Permissao` entram pela permissão carregada (IR-02); `Psicologo`, `Paciente` e `AreaAtuacao` são de cadastros, cobertos na Fase 5 |
| Cada entidade/objeto tem responsabilidade clara; sem classes onipotentes | 4 diagramas | **OK** — a coordenação está no `ServicoAgenda` (AP-03); nenhuma entidade "sistema" |
| **Mensagens de sequência correspondem a operações conhecidas** | §5 | **OK** — 82/82 setas (49 solicitações e 33 retornos) com origem em OP/AP/IR (era "Parcial" na Fase 5) |
| **Mensagens de comunicação correspondem a operações conhecidas** | §5 | **OK** — 14/14 |
| Estados e transições têm eventos, condições ou ações coerentes | EST-001 | **OK** — 5 estados, 6 transições, todas com evento e 3 com guarda; encerra a INC-13 |
| Atividades têm decisões, responsabilidades e paralelismo | ATV-001 | **OK** — 4 decisões, 4 pistas, 3 blocos paralelos (fork/join) e 1 etapa sequencial de notificação |
| Entidades persistentes têm correspondência com classes | 5Diagramas | **OK** — `SOLICITADO`…`NAO_COMPARECEU` = enum `SituacaoAgendamento` = `agendamento.status` (`IMP-DER-02` do I4) |
| FKs e cardinalidades respeitam as multiplicidades do domínio | — | **Não se aplica** — as multiplicidades seguem `PENDENTE` (LAC-08); os diagramas não dependem delas |
| Mudanças em um artefato provocam revisão dos relacionados | esta revisão + matriz da Fase 6 | **Aplicado** — a §6 desta revisão e `06-rastreabilidade/matriz-rastreabilidade-fase6.md` |
| Nenhum dado real nos artefatos | DEC-005 | **OK** — verificado por inspeção do texto extraído dos 6 PDFs |
| Diagramas têm título + identificador, objetivo, elementos, interpretação, decisões, relação com requisitos/classes/banco e referência ao arquivo editável | plano §16.1 | **OK** — as 4 especificações em `03-uml/*/proposta-I2-especificacao-*.md` seguem a ordem do plano |

---

## 9. Pendências que permanecem após esta revisão

| ID | Pendência | Fecha com |
|---|---|---|
| R6-01, R6-03 | Camada da comparação de credencial e da verificação de situação | I4 + I3 (OP-02/OP-01) |
| R6-02 | Nome da operação de busca da conta | I4 + I2 (glossário) |
| R6-04 | Distinção `obterPermissoes` × `consultarPermitidos` | I2 (glossário do dossiê) |
| R6-06 | Nome do parâmetro de notificação | I4 + I2 |
| R6-08 / R5-13 | Reanálise de persistência contra o modelo aprovado | I4 |
| LAC-01…LAC-09 | DEC-009, DEC-010, DEC-012, P-01, P-02, P-05, P-14 | Grupo |
| PC-02, PC-05, PC-06, PC-07, PC-08, PC-09, PC-10, PC-12 | Incorporação pelo Integrante 3 das OP-xx e das situações | I3 |
| R5-16 | Linha "RFs cobertos" nos quatro casos críticos | I2 (Fase 3) |
| Revisão cruzada por outro integrante | Definition of Done do plano §13.1 | Grupo |

---

## 10. Checklist de aprovação (para a ata do walkthrough)

- [ ] Grupo aprova os **cinco diagramas** (SEQ-001/002/003, ATV-001, COM-001) e a **máquina de estados** (EST-001) como artefatos comportamentais da Fase 6.
- [ ] Grupo decide **DEC-010** (inovação) — decide se a **SEQ-004** entra no conjunto; sem isso, a lacuna LAC-01 permanece.
- [ ] Grupo decide **DEC-009** (notificação) e **DEC-012** (prazo/ator do cancelamento) — fecham LAC-03 e LAC-04.
- [ ] Grupo decide **P-01** (mecanismo da confirmação) e **P-05** (modelo do horário) — fecham LAC-02 e LAC-05.
- [ ] Grupo confirma a **INC-13** (não compareceu apenas a partir de confirmado) e a resposta à **P-14**.
- [ ] Integrante 3 incorpora **OP-01…OP-22** e as situações §6 da revisão da Fase 5, atualizando `diagrama-classes.mmd` e `especificacao-classes.md`.
- [ ] Integrante 4 atualiza a análise de impacto comportamental com **R6-01, R6-02, R6-03, R6-06** e refaz a viabilidade (R5-13/R6-08).
- [ ] Integrante 1 homologa `06-rastreabilidade/matriz-rastreabilidade-fase6.md` (ou a funde na matriz mestra).
- [ ] Integrante 6 insere no dossiê as seções **21 a 24** a partir de `07-dossie/secoes/fase6-secao-21-24.md`.

**Registro de participação desta revisão:** Integrante 2 (Aquiles Angelo) — confronto dos 6 diagramas comportamentais contra 32 RFs, 14 RNs, 13 casos de uso, 12 classes, 16 associações, as 22 operações propostas (OP-01…OP-22), as 5 responsabilidades de aplicação (AP-01…AP-05) e a análise de impacto comportamental do Integrante 4. Revisão cruzada por outro integrante ainda **pendente** (Definition of Done do plano §13.1).

---

## 11. Entregáveis desta Fase 6 (Integrante 2)

| Arquivo | Conteúdo | Situação |
|---|---|---|
| `03-uml/Sequências/proposta-I2-sequencia-seq-001-autenticar.mmd` (+ `.drawio.xml`, `.pdf`) | **SEQ-001** — autenticar usuário (UC-001), com E1/E2 e A1 | Pronto para walkthrough |
| `03-uml/Sequências/proposta-I2-sequencia-seq-002-agendar.mmd` (+ `.drawio.xml`, `.pdf`) | **SEQ-002** — agendar (UC-025 + UC-024), com A1 e E1 | Pronto para walkthrough |
| `03-uml/Sequências/proposta-I2-sequencia-seq-003-cancelar-confirmar.mmd` (+ `.drawio.xml`, `.pdf`) | **SEQ-003** — cancelar / confirmar / concluir / registrar falta (UC-027) | Pronto para walkthrough |
| `03-uml/Atividade/proposta-I2-atividade-atv-001.mmd` (+ `.drawio.xml`, `.pdf`) | **ATV-001** — ciclo do agendamento em 4 pistas, 4 decisões, 3 blocos paralelos e a etapa sequencial de notificação do cancelamento | Pronto para walkthrough |
| `03-uml/Comunicação/proposta-I2-comunicacao-com-001.mmd` (+ `.drawio.xml`, `.pdf`) | **COM-001** — 12 colaboradores, 14 mensagens, hub `ServicoAgenda` (AP-03) | Pronto para walkthrough |
| `03-uml/Estados/proposta-I2-estados-est-001.mmd` (+ `.drawio.xml`, `.pdf`) | **EST-001** — máquina de estados do `Agendamento` (5 estados, 6 transições) | Pronto para walkthrough |
| `03-uml/Sequências/proposta-I2-especificacao-sequencias.md` | Especificação das três sequências: participantes, fragmentos, decisões, lacunas | Pronto para walkthrough |
| `03-uml/Atividade/proposta-I2-especificacao-atividade.md` | Especificação do ATV-001: pistas, decisões, paralelismo, rotas | Pronto para walkthrough |
| `03-uml/Comunicação/proposta-I2-especificacao-comunicacao.md` | Especificação do COM-001: colaboradores, mensagens, hub | Pronto para walkthrough |
| `03-uml/Estados/proposta-I2-especificacao-estados.md` | Especificação do EST-001: estados, transições, guardas | Pronto para walkthrough |
| `06-rastreabilidade/matriz-rastreabilidade-fase6.md` | Matriz RF/RN/UC × SEQ/ATV/COM/EST, mensagens × OP/AP, verificação de órfãos | Pronto para homologação do I1 |
| `07-dossie/secoes/fase6-secao-21-24.md` | Texto pronto para as seções 21, 22, 23 e 24 do dossiê final | Texto para o I6 consolidar |
| `01-planejamento/revisoes/FASE6-REV-I2-revisao-comportamental.md` | Esta revisão: cobertura, comparação cruzada, cotejo com o I4, achados R6-01…R6-08, checklist | Pronto para walkthrough |

**Sequência sugerida ao grupo:** (1) walkthrough desta revisão e dos seis diagramas; (2) decisão de DEC-009, DEC-010, DEC-012, P-01, P-02 e P-05; (3) incorporação pelo I3 das OP-xx e das situações; (4) atualização das análises do I4 com R6-01…R6-06; (5) Fase 7 (pacotes, componentes, DER) com a matriz da Fase 6 como insumo.
