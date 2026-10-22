# Especificação das sequências SEQ-001, SEQ-002 e SEQ-003 — Fase 6

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 22/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Natureza:** **PROPOSTA.** Não substitui artefato oficial do grupo.

| Diagrama | Fonte editável | Editável (draw.io) | Dossiê (PDF) | Caso de uso |
|---|---|---|---|---|
| SEQ-001 | [`proposta-I2-sequencia-seq-001-autenticar.mmd`](proposta-I2-sequencia-seq-001-autenticar.mmd) | `proposta-I2-sequencia-seq-001-autenticar.drawio.xml` | `proposta-I2-sequencia-seq-001-autenticar.pdf` | UC-001 |
| SEQ-002 | [`proposta-I2-sequencia-seq-002-agendar.mmd`](proposta-I2-sequencia-seq-002-agendar.mmd) | `proposta-I2-sequencia-seq-002-agendar.drawio.xml` | `proposta-I2-sequencia-seq-002-agendar.pdf` | UC-025 (+UC-024/RF-024) |
| SEQ-003 | [`proposta-I2-sequencia-seq-003-cancelar-confirmar.mmd`](proposta-I2-sequencia-seq-003-cancelar-confirmar.mmd) | `proposta-I2-sequencia-seq-003-cancelar-confirmar.drawio.xml` | `proposta-I2-sequencia-seq-003-cancelar-confirmar.pdf` | UC-027 |

> **Convenção de entrega:** o `.mmd` é a **fonte da verdade do conteúdo**; o `.drawio.xml` é a versão editável; o `.pdf` é a versão do dossiê. Os três arquivos têm o **mesmo nome-base**.

---

## 1. Objetivo

O plano da AV3 exige "pelo menos três diagramas de sequência para os casos de uso críticos aprovados" (§6.1, Integrante 2). Os três diagramas aqui propostos cobrem os três caminhos críticos do domínio:

1. **SEQ-001 — entrar no sistema** (UC-001): é a precondição de tudo; inclui os desvios de erro E1/E2 e o caso de sucesso.
2. **SEQ-002 — obter e reservar um horário** (UC-025 + UC-024): é o caminho feliz de marcação, com a alternativa de conflito de horário e a alternativa de ausência de horários (A1).
3. **SEQ-003 — cancelar, confirmar, concluir e registrar falta** (UC-027): é o ciclo de vida do `Agendamento` depois de criado, com as guardas de prazo e de situação.

Os três diagramas são **entrelaçados**: quem lê o conjunto entende o ciclo completo sem precisar decifrar um único diagrama isolado.

## 2. Regras de elegibilidade das mensagens (plano §6.1)

Toda mensagem de um diagrama de sequência deve corresponder a **uma operação conhecida** (OP-01…OP-22, do diagrama de classes CLS-001 v2) ou a **uma responsabilidade de aplicação** (AP-01…AP-05, da revisão FASE5-REV-I2 §5.2). Não há mensagem "criada para satisfazer um diagrama futuro".

| Elemento | Origem da autorização | Onde aparece |
|---|---|---|
| `autenticar`, `criarSessao`, `invalidarSessaoAtiva` | AP-01 (`ServicoAutenticacao`) | SEQ-001 |
| `registrar(...)` (auditoria) | AP-02 (`ServicoAuditoria`) + `RegistroAuditoria.registrar` (RF-032) | SEQ-001/002/003 |
| `solicitar`/`confirmar`/`cancelar`/`concluir`/`registrarFalta` | AP-03 (`ServicoAgenda`) + OP-13…OP-18 | SEQ-002/003 |
| `notificar(...)` | AP-04 (`ServicoNotificacao`, fronteira externa) | SEQ-002/003 |
| `podeAutenticar`, `confereCredencial` | OP-01, OP-02 | SEQ-001 |
| `estaAtivo` (vínculo) | OP-22 | SEQ-002 |
| `consultarHorariosDisponiveis` | OP-11 | SEQ-002 |
| `verificarDisponibilidade` | `Agenda.verificarDisponibilidade` (modelo do I3) | SEQ-002 |
| `consultarMeus`, `podeSerCancelado`, `estaCancelada` | OP-19, OP-15, OP-16 | SEQ-003 |
| `Atendimento.registrar` | OP-20 | SEQ-003 |
| `obterPorIdentificacao`, `obterPermissoes` | IR-01, IR-02 (infraestrutura, Fase 7) | SEQ-001 |

**Limite proposital:** *sessão*, *notificação* e *lista de espera* **não** são entidades de domínio neste repositório (P-07, DEC-009 e plano §5.2). Aparecem como **comportamento de aplicação** (`AP-01`) ou como **fronteira externa** (`AP-04`), nunca como classe do modelo.

## 3. SEQ-001 — Autenticar usuário (UC-001)

### 3.1 Cenário

> Um usuário (paciente, psicólogo ou administrador) informa identificação e senha. O sistema localiza a conta, verifica se pode autenticar, confere a credencial, carrega as permissões, cria a sessão e audita o evento. Em qualquer falha, o acesso é recusado **e** o evento é auditado.

### 3.2 Participantes

| Participante | Natureza | Papel |
|---|---|---|
| `Ator` | ator humano | Quem se autentica. |
| `ServicoAutenticacao` | serviço de aplicação (AP-01) | Coordena o caso de uso; fora do domínio. |
| `RepositorioUsuario` | infraestrutura (IR-01) | Obtém a conta por identificação (Fase 7). |
| `Usuario` | entidade de domínio | Decide **se pode** (OP-01) e **se a credencial confere** (OP-02). |
| `RepositorioAutorizacao` | infraestrutura (IR-02) | Devolve perfil e permissões (Fase 7). |
| `ServicoAuditoria` | serviço de aplicação (AP-02) | Registra o evento auditável. |
| `RegistroAuditoria` | entidade de dominio | Persiste a trilha (data, hora, operação, responsável). |

### 3.3 Fragmentos

| Fragmento | Tipo | Condição / efeito |
|---|---|---|
| `E1 - Conta inexistente` | `alt` | `RepositorioUsuario` devolve "conta inexistente" → auditoria de `LOGIN_NEGADO` e recusa (E1). |
| `E2 - Conta bloqueada ou inativa` | `alt` | `podeAutenticar()` = **false** (RN-001) → auditoria e recusa (E2). |
| `E1 - Credencial inválida` | `alt` | `podeAutenticar()` = true e `confereCredencial()` = **false** (RN-002) → auditoria e recusa (E1). |
| `Sucesso - credenciais válidas` | `alt` | Permissões carregadas, sessão criada, `LOGIN_CONCEDIDO` auditado. |
| `A1 (RF-002) - Encerramento de sessão` | `opt` | O usuário pode encerrar a sessão a qualquer momento; invalida a sessão ativa. |

### 3.4 Decisões de modelagem

- A **entidade `Usuario` não autentica**. Ela responde apenas "se pode" e "se a credencial confere" (plano §5.2, revisão FASE5-REV-I2 §7). Criar sessão, invalidar sessão e deal com credencial protegida é responsabilidade da aplicação (AP-01) e da infraestrutura (IR-01).
- A auditoria é feita **nos quatro desfechos**, incluindo os negativos: é o que fecha RN-009/RNF-004 (PC-12).
- `criarSessao` e `invalidarSessaoAtiva` aparecem como mensagens **internas ao próprio `ServicoAutenticacao`**, porque sessão não é entidade e não pode ser mensagem para um colaborador de domínio.

### 3.5 Divergência a registrar

A análise de persistência do Integrante 4 (`04-persistencia/analises/fase-06-impacto-comportamental-persistencia.md`) menciona `consultarPorLogin` e `compararHash`, enquanto o diagrama usa `obterPorIdentificacao` (IR-01) e `confereCredencial` (OP-02). **A divergência é de nome de operação de infraestrutura, não de comportamento**: a verificação do hash é responsabilidade da entidade `Usuario` (RN-002), e a busca da conta é do repositório. O cotejo está na revisão FASE6-REV-I2.

## 4. SEQ-002 — Agendar atendimento (UC-025 + UC-024/RF-024)

### 4.1 Cenário

> O paciente consulta os horários livres do seu psicólogo; escolhe um; o sistema valida o vínculo e a disponibilidade do slot; cria o agendamento, confirma-o de forma automática (RN-013), audita e notifica.

### 4.2 Participantes

`Ator (Paciente)`, `ServicoAgenda` (AP-03), `Vinculo`, `Agenda`, `Agendamento`, `ServicoAuditoria` (AP-02), `RegistroAuditoria`, `ServicoNotificacao` (AP-04).

### 4.3 Fragmentos

| Fragmento | Tipo | Condição / efeito |
|---|---|---|
| `Há horários disponíveis` | `alt` (1º ramo) | Caminho feliz: devolve a lista, segue para `solicitar`. |
| `Horário ainda disponível` | `alt` aninhado | `verificarDisponibilidade` = true → cria, confirma, audita, notifica. |
| `E1 - Conflito de horário` | `alt` aninhado | O slot foi tomado entre a consulta e a reserva → recusa e sugere outro horário (RF-025). |
| `Nenhum horário disponível (A1 de UC-025)` | `alt` (2º ramo) | Informa que não há horários livres (RF-024). |

### 4.4 Decisões de modelagem

- **A consulta vai ao `ServicoAgenda`, não ao `Agendamento`.** O `ServicoAgenda` (AP-03) é o único coordinator do caso: ele consulta, decide, cria, audita e notifica. É o que evita a classe onipotente vedada pelo plano §5.2.
- **O vínculo é consultado antes da agenda** (`estaAtivo`, OP-22): sem vínculo ATIVO o paciente não tem agenda alheia — é a materialização de RN-003/RN-005.
- **A reserva do slot não aparece como mensagem.** Com o modelo de slot discreto (P-05, associação A17 `Disponibilidade`—`Agendamento`), a reserva é interna a `Agendamento.solicitar`. Sem a decisão do grupo, ela **não é desenhavel** — está registrado como lacuna em §7.
- **A notificação é pós-commit** (TRS-02 do Integrante 4) e **condicional** (DEC-009 pendente): aparece como fronteira externa, e o retorno "enviado | falha de entrega" deixa explícito que ela não desfaz o agendamento.

## 5. SEQ-003 — Cancelar / confirmar / concluir / registrar falta (UC-027)

### 5.1 Cenário

> O paciente ou o psicólogo consulta seus agendamentos e cancela um deles; em outro agendamento, a confirmação automática é registrada; em um terceiro, a sessão é concluída com registro de atendimento; em um quarto, registra-se a falta.

### 5.2 Participantes

`Ator (Paciente ou Psicólogo)`, `ServicoAgenda` (AP-03), `Agendamento`, `Atendimento`, `ServicoAuditoria` (AP-02), `RegistroAuditoria`, `ServicoNotificacao` (AP-04).

### 5.3 Fragmentos

| Fragmento | Tipo | Condição / efeito |
|---|---|---|
| `Pode ser cancelado` | `alt` (1º ramo) | `podeSerCancelado()` = true → cancela, audita, notifica (RF-027/RN-012). |
| `E1 - Agendamento já realizado ou já cancelado` | `alt` (2º ramo) | Recusa o cancelamento (E1 de UC-027). |
| `RF-026 - Confirmação do agendamento` | `opt` | Confirmação automática (RN-013) + auditoria. |
| `RF-030 - Conclusão da sessão realizada` | `opt` | `estaCancelada()` = false → conclui, registra atendimento (OP-20), audita. |
| `INC-13 - Não compareceu` | `opt` | `registrarFalta` (OP-18) a partir de `CONFIRMADO`, com auditoria. |

> Os três blocos `opt` são **fluxos alternativos** do mesmo agendamento, não etapas obrigatórias em sequência. Isso está escrito na nota do próprio diagrama para evitar leitura errada.

### 5.4 Decisões de modelagem

- **`podeSerCancelado()` é uma operação do `Agendamento` (OP-15)**, não uma condição no serviço: quem conhece o prazo e a situação é a entidade. O prazo em si depende de **DEC-012/P-02**, ainda não decidido.
- **`estaCancelada()` (OP-16) é consultado antes de concluir**, para materializar **RN-006** (não há atendimento de sessão cancelada) sem duplicar a regra no serviço.
- **`NAO_COMPARECEU` só é alcançável a partir de `CONFIRMADO`** (INC-13 do Integrante 4). A nota do diagrama de estados (EST-001) registra essa restrição.
- **A ausência de atendimento na falta é intencional**: falta não é atendimento.

## 6. Cobertura de requisitos e regras

| RF / RN | SEQ-001 | SEQ-002 | SEQ-003 |
|---|---|---|---|
| RF-001 (autenticar) | ✔ | | |
| RF-002 (sessão) | ✔ (`opt` A1) | | |
| RF-024 (horários livres) | | ✔ | |
| RF-025 (agendar) | | ✔ | |
| RF-026 (confirmar) | | ✔ | ✔ (`opt`) |
| RF-027 (cancelar) | | | ✔ |
| RF-028 (meus agendamentos) | | | ✔ (`consultarMeus`, OP-19) |
| RF-030 (registrar atendimento) | | | ✔ (`opt`) |
| RF-032 (auditoria) | ✔ | ✔ | ✔ |
| RN-001 (conta bloqueada) | ✔ (E2) | | |
| RN-002 (credencial/senha) | ✔ (E1) | | |
| RN-003 (vínculo autoriza acesso) | | ✔ | |
| RN-005 (vínculo ATIVO) | | ✔ | |
| RN-006 (cancelado ≠ atendido) | | | ✔ |
| RN-009 (auditoria) | ✔ | ✔ | ✔ |
| RN-010 (slot único) | | ✔ (E1) | |
| RN-012 (prazo de cancelamento) | | | ✔ |
| RN-013 (confirmação automática) | | ✔ | ✔ (`opt`) |

## 7. Lacunas registradas (não desenháveis nesta fase)

| Lacuna | Causa | Efeito no diagrama |
|---|---|---|
| **SEQ-004 — "Preencher encaixe"** | DEC-010 (inovação) não decidido | Sequência **não produzida**. O caso de uso não pode ser desenhado sem definir o que é "encaixe". |
| **Reserva do slot (A17)** | P-05 (slot discreto) não decidido | A reserva aparece como interna a `Agendamento.solicitar`; não há mensagem própria. |
| **Prazo de cancelamento** | DEC-012 / P-02 não decidido | A guarda `[prazo]` aparece como condição de OP-15, sem valor. |
| **Canal de notificação** | DEC-009 não decidido | `ServicoNotificacao` é fronteira externa (interface), não entidade. |
| **Lista de espera / sessão** | P-07 e plano §5.2 | Fora do domínio; não aparece. |

## 8. Como ler e editar

1. A **fonte editável** é o `.mmd` (Mermaid). Ele pode ser aberto em qualquer editor de texto ou renderizado por Mermaid Live.
2. As **hints de layout** `%% pos: <id> <x> <y>` são comentários Mermaid válidos: o Mermaid as ignora, e o renderizador offline do repositório as usa para posicionar os participantes. Ver `03-uml/README.md`.
3. O `.drawio.xml` é gerado a partir do `.mmd` e é o arquivo a ser editado no **draw.io** quando for preciso ajustar o desenho sem mexer no conteúdo.
4. O `.pdf` é a versão congelada para o dossiê.

## 9. Revisão de qualidade (cota do Integrante 5)

- **Confiabilidade:** todos os caminhos de erro das sequências terminam em recusa explícita ao ator e em registro de auditoria — não há caminho de erro silencioso.
- **Exceções:** as exceções de infraestrutura (repositório, envio de mensagem) aparecem como retorno alternativo ("conta inexistente", "falha de entrega"), sem criar entidade de exceção no domínio.
- **Segurança/autorização:** a autorização é consultada no serviço (`OP-04`, `OP-22`) e não é assumida pelo ator.
- **Auditoria:** toda operação auditável do plano (RN-009) tem uma mensagem correspondente nos três diagramas.
