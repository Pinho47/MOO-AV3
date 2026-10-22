# Fase 5 — Revisão do modelo de domínio, classes e objetos (Integrante 2)

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 21/10/2026 · **Versão:** v1.0 (rascunho para walkthrough do grupo).
**Tarefa do plano atendida:** Fase 5, bloco do Integrante 2 (plano §8):
revisar se as operações propostas nas classes permitem executar os fluxos dos casos de uso; apontar responsabilidades ausentes necessárias aos diagramas de sequência; validar se estados e eventos citados nos casos possuem representação coerente no domínio; não criar métodos apenas para satisfazer diagramas futuros.

## 1. Objetivo, escopo e limites desta revisão

Esta é uma **revisão de cobertura de comportamento**. Não redesenha o modelo de domínio: a estrutura é do Integrante 3 e a responsabilidade por ela permanece com ele. O que este documento faz é responder a três perguntas que o plano atribui ao Integrante 2:

1. **As operações hoje modeladas conseguem executar os fluxos dos casos de uso?**
2. **Que responsabilidades estão faltando para que os diagramas de sequência da Fase 6 sejam possíveis sem inventar mensagens?**
3. **Os estados e eventos citados nos casos de uso existem, de forma coerente, no domínio?**

Limites declarados:

- Não se aprova requisito, regra ou caso de uso; não se alteram artefatos do Integrante 3 sem pedido dele.
- Não se criam classes de conveniência (`Sistema`, `GerenciadorGeral`, `Plataforma`) nem operações sem regra que as sustente (plano §5.2 e §19).
- Toda proposta de operação nova neste documento carrega a base (RF/RN/CA/UC) que a sustenta. Propostas sem base documental estão listadas como **não propostas** (§9).
- Anumeração de pendências: **R5-xx** = achado desta revisão (local). **PC-xx** = pendência do Integrante 3 em `03-uml/Classes/`. **P-xx** = pendência do Integrante 4 em `04-persistencia/analises/fase-05-viabilidade-persistencia-dominio.md`. **DEC-xxx** = registro de decisões do grupo.

## 2. Fontes inspecionadas

| Artefato | Caminho | Uso nesta revisão |
|---|---|---|
| Requisitos funcionais (RF-001–032) | `02-requisitos/requisitos-funcionais.md` | Fonte primária das responsabilidades de cada fluxo |
| Regras de negócio (RN-001–014) | `02-requisitos/regras-negocio.md` | Restrições que as operações devem preservar |
| Critérios de aceite (CA-RF-xxx) | `02-requisitos/criterios-aceite.md` | Verificabilidade das propostas |
| Especificações de casos críticos | `03-uml/Casos de uso/especificacoes-casos-de-uso-criticos.md` | UC-001, UC-021, UC-025, UC-027 |
| Especificações dos demais casos | `03-uml/Casos de uso/especificacoes-casos-de-uso-demais.md` | Fluxos administrativos e de consulta |
| Modelo de domínio | `03-uml/Dominio/modelo-dominio.md` | Conceitos, restrições e pendências PD-01–PD-12 |
| Especificação de classes | `03-uml/Classes/especificacao-classes.md` | Operações, contratos e cobertura por RF |
| Diagrama de classes | `03-uml/Classes/diagrama-classes.mmd` | Assocciações A01–A16 e assinaturas |
| Decisões de modelagem OO | `03-uml/Classes/decisoes-modelagem.md` | OO-01–OO-16 e pendências PC-01–PC-12 |
| Diagrama e especificação de objetos | `03-uml/Objetos/` | OBJ-001 e sua suficiência como cenário |
| Decisões do grupo | `01-planejamento/decisoes.md` | DEC-001–DEC-010 |
| Análise de viabilidade de persistência | `04-persistencia/analises/fase-05-viabilidade-persistencia-dominio.md` | INC-01–INC-12 e P-01–P-12 |
| Plano de execução | Plano AV3 (PDF) | Fase 5, §5.3, §5.4, §13, §15, §19 |

## 3. Método e critérios

Para cada caso de uso, o fluxo principal foi decomposto em **passos observáveis** e cada passo foi confrontado com as operações existentes no diagrama de classes. O veredicto é:

- **COBERTO** — existe operação cujo nome, base e contrato executam o passo.
- **PARCIAL** — existe operação, mas assinatura, guarda ou estado não permitem executar o passo.
- **LACUNA** — nenhum operação sustenta o passo; o fluxo não é executável com o modelo atual.
- **FORA DO DOMÍNIO** — o passo exige responsabilidades de aplicação/fronteira (sessão, e-mail externo, painel), que o plano §5.2 proíbe colocar em entidade de domínio.

Critérios de qualidade aplicados: plano §13 (regras de consistência), §15 (evidências OO/SOLID) e §19 (penalidades do enunciado — em especial *"ausência de multiplicidades, responsabilidades ou fluxos essenciais caracteriza modelagem incompleta"*).

## 4. Resultado 1 — Cobertura dos casos de uso × operações do domínio

### 4.1 UC-001 — Autenticar usuário (Administrador, Psicólogo, Paciente)

| Passo do fluxo (fonte: UC-001) | Operação de domínio disponível | Veredicto | Observação |
|---|---|---|---|
| Usuário informa credenciais (e-mail e senha) | — | **LACUNA** | Nenhuma operação trata credencial. `Usuario` só tem `ativar`, `bloquear`, `associarPerfil`, `atualizarDadosPermitidos` |
| Sistema valida se a conta existe e está ativa | `Usuario.situacaoConta` (atributo, tipo PENDENTE — PC-02) | **PARCIAL** | Não há predicado de estado; o valor de `situacaoConta` nem está definido (R5-04) |
| Sistema valida se a senha confere | — | **LACUNA** | RN-002 exige representação protegida da credencial; nenhum método compara a credencial (R5-01) |
| Sistema cria a sessão autenticada, vinculada ao perfil e permissões | — | **FORA DO DOMÍNIO** | Sessão é estado volátil de aplicação (P-07 do I4); cabe a um serviço de aplicação, não a entidade (OO-16) |
| A1 — Encerrar sessão invalida a sessão ativa | — | **FORA DO DOMÍNIO** | Idem; a sessão não é entidade do domínio |
| E1 — Credenciais inválidas: recusa o acesso e não cria sessão | — | **LACUNA** | Depende das duas lacunas acima |
| E2 — Conta bloqueada: recusa mesmo com credenciais corretas (RN-001) | `Usuario.bloquear(solicitante)` | **PARCIAL** | Existe o bloqueio (RF-006), mas não existe a condição que impede a autenticação (RN-001) |

**Conclusão:** UC-001 **não é executável** com o modelo atual. Faltam duas responsabilidades de domínio e uma de aplicação.

### 4.2 UC-021 — Vincular psicólogo a paciente (Administrador)

| Passo do fluxo | Operação disponível | Veredicto | Observação |
|---|---|---|---|
| Administrador seleciona um Psicólogo cadastrado | — (consulta administrativa) | **LACUNA** | RF-011 não tem operação (R5-06) |
| Administrador seleciona um Paciente cadastrado | — | **LACUNA** | RF-012 não tem operação (R5-06) |
| Sistema formaliza o vínculo entre os dois | `Vinculo.formalizar(psicologo, paciente, solicitante)` | **COBERTO** | Boa operação de criação; guarda de duplicidade ainda PC-06 |
| Sistema inclui o evento na trilha de auditoria | `RegistroAuditoria.registrar(...)` | **PARCIAL** | Existe a criação do registro, mas nenhuma colaboração define *quando* e *com o que* o registro é feito (PC-12, R5-08) |

**Conclusão:** o núcleo do caso está coberto. Falta a materialização das consultas administrativas e a colaboração de auditoria.

### 4.3 UC-025 — Solicitar agendamento com confirmação automática (Paciente)

| Passo do fluxo | Operação disponível | Veredicto | Observação |
|---|---|---|---|
| Paciente consulta horários disponíveis do Psicólogo vinculado (UC-024 / RF-024) | `Agenda.verificarDisponibilidade(horario): Booleano` | **LACUNA** | A operação é booleana e pontual: não devolve **a lista** de horários disponíveis. O próprio OO-11 reconhece que ela *"não substitui a consulta que apresenta horários disponíveis de RF-024"* (R5-03) |
| Paciente seleciona um horário e solicita o agendamento | `Agendamento.solicitar(psicologo, paciente, horario, solicitante)` | **PARCIAL** | Existe; mas o tipo de `horario` está PENDENTE (PC-07) e o vínculo com o período de disponibilidade não está modelado (R5-10) |
| Sistema valida que o horário ainda está disponível (RN-005, RN-010) | `Agenda.verificarDisponibilidade(horario)` | **PARCIAL** | A verificação existe, mas o modelo não mostra onde o horário é **reservado**, nem qual situação "ocupa" o horário (INC-06 do I4, P-03) |
| Sistema cria o agendamento com **situação confirmada automaticamente** | `Agendamento.confirmar()` | **PARCIAL** | `confirmar()` tem **assinatura incompleta** e nenhum ator; o caso de uso define confirmação automática, o que contradiz a pendência PC-07 (R5-02) |
| Sistema envia notificação de confirmação por e-mail (<<include>>) | — | **FORA DO DOMÍNIO** | DEC-009 está pendente; o domínio excluiu e-mail (OO-16, R5-12) |

**Conclusão:** o caso depende de três decisões ainda abertas (situação inicial, mecanismo de confirmação, modelo de tempo do horário) e de uma operação ausente (listagem de horários).

### 4.4 UC-027 — Cancelar agendamento (Paciente, Psicólogo)

| Passo do fluxo | Operação disponível | Veredicto | Observação |
|---|---|---|---|
| Ator seleciona um agendamento próprio/vinculado | — | **LACUNA** | RF-028/RF-029 (consultar os próprios agendamentos / a agenda própria) não têm operação (R5-06) |
| Ator solicita o cancelamento | `Agendamento.cancelar(solicitante)` | **PARCIAL** | Existe, mas a assinatura é provisória: sem ator autorizado (RN-012), sem prazo (DEC-012) e sem motivo (R5-02) |
| Sistema valida se o agendamento está em situação cancelável | — | **LACUNA** | Não existe predicado de transição; o domínio de `situacao` está PENDENTE (R5-04) |
| Sistema altera a situação para cancelado | `Agendamento.cancelar(...)` | **PARCIAL** | Transição existe, destino não nomeado |
| Sistema registra a operação na auditoria (RN-009) | `RegistroAuditoria.registrar(...)` | **PARCIAL** | Colaboração não definida (R5-08) |
| E1 — Agendamento já realizado ou já cancelado: recusa novo cancelamento | — | **LACUNA** | Depende do predicado de guardas |

**Conclusão:** a transição existe, as guardas e a assinatura não.

### 4.5 Demais casos de uso

| Caso de uso | RF cobertos | Operações existentes | Veredicto | Lacuna principal |
|---|---|---|---|---|
| Gerenciar usuários e permissões | RF-003–010 | `Usuario.ativar/bloquear/associarPerfil/atualizarDadosPermitidos`, `Perfil.adicionarPermissao/removerPermissao` | **PARCIAL** | Não existe **criação** de conta (RF-003), de perfil (RF-007) nem de permissão (RF-008); consultas RF-009/RF-010 sem operação (R5-07, R5-06) |
| Gerenciar psicólogos e pacientes | RF-011–012, RF-014, RF-018–020 | `Psicologo.atualizarDadosProfissionais/associarAreaAtuacao`, `Paciente.atualizarCadastro/alterarSituacao` | **PARCIAL** | Não existe **cadastro** de Psicólogo (RF-014) nem de Paciente (RF-018); consultas RF-011/RF-012 sem operação (R5-07, R5-06) |
| Consultar auditoria | RF-013 | `RegistroAuditoria.registrar(...)` | **LACUNA** | Existe a escrita; a **leitura** com filtro e ordem cronológica não tem responsabilidade (R5-06) |
| Gerenciar perfil e agenda | RF-015, RF-016, RF-023, RF-029 | `Psicologo.atualizarDadosProfissionais/associarAreaAtuacao`, `Agenda.adicionarDisponibilidade/verificarDisponibilidade` | **PARCIAL** | A **criação** da instância de `Disponibilidade` não existe (PC-07) — `adicionarDisponibilidade` recebe uma instância que ninguém produz (R5-07); consulta da agenda própria (RF-029) sem operação |
| Atender pacientes vinculados | RF-017, RF-030, RF-031 | `Psicologo.consultarPacientesVinculados/consultarHistoricoPermitido`, `Atendimento.registrar(psicologo, paciente, contextoSessao, solicitante)` | **PARCIAL** | `registrar` recebe `contextoSessao` sem tipo definido; **não referencia o Agendamento**, então "sessão válida da agenda" (RF-030) e RN-006 não são verificáveis no modelo (R5-05) |
| Consultar meu cadastro | RF-022 | `Paciente.consultarCadastroPermitido(solicitante)` | **COBERTO** | Apenas projetar os campos liberados (PC-05) |
| Registrar evento de auditoria (<<include>>) | RF-032 | `RegistroAuditoria.registrar(...)` | **COBERTO** | Colaboração disparada por outras operações (R5-08) |
| Notificar paciente por e-mail (<<include>>) | — (DEC-009 pendente) | — | **FORA DO DOMÍNIO** | Decisão pendente e já desenhada no diagrama de casos de uso (R5-12) |

### 4.6 Consolidado

- **COBERTO:** 2 de 14 casos/UCs avaliados diretamente; **PARCIAL:** 8; **LACUNA:** 3; **FORA DO DOMÍNIO:** 1 (parcialmente).
- **Cobertura por RF:** 21 dos 32 RFs têm objetivo representado em alguma operação (apenas "parcialmente" ou "colaboração posterior" na classificação do Integrante 3). **11 RFs não têm responsabilidade modelada**: RF-001, RF-002, RF-003, RF-009, RF-010, RF-011, RF-012, RF-013, RF-024, RF-028, RF-029.
- **Nenhum RF está sem classe.** O problema é de operação/colaboração, não de estrutura de classes. Isso é uma boa notícia: a **estrutura do domínio está correta** e não precisa ser redesenhada; o que falta é fechar contratos.

## 5. Resultado 2 — Responsabilidades ausentes para os diagramas de sequência

Propostas abaixo. Todas têm base documental; nenhuma existe apenas para "preencher" uma mensagem de sequência (plano §8, Integrante 2).

### 5.1 Operações de domínio propostas (para o Integrante 3 incorporar)

| ID | Classe | Operação proposta | Base | Passo do fluxo que executa |
|---|---|---|---|---|
| OP-01 | `Usuario` | `podeAutenticar(): Booleano` | RF-001, RN-001, PC-02 | UC-001 E2 |
| OP-02 | `Usuario` | `confereCredencial(credencial: Credencial): Booleano` | RF-001, RN-002, RNF-001 | UC-001 passo 3 |
| OP-03 | `Usuario` | `cadastrar(dadosPermitidos, perfil: Perfil, solicitante: Usuario): Usuario` | RF-003, RN-002 | "Gerenciar usuários" (cadastrar) |
| OP-04 | `Usuario` | `consultarPermitidos(solicitante: Usuario): List<Usuario>` | RF-010 | "Gerenciar usuários" (consultar) |
| OP-05 | `Psicologo` | `cadastrar(dadosPermitidos, solicitante: Usuario): Psicologo` | RF-014, CA-RF-014 | "Gerenciar psicólogos" |
| OP-06 | `Psicologo` | `consultarCadastros(solicitante: Usuario): List<Psicologo>` | RF-011 | "Gerenciar psicólogos e pacientes" (consultar) |
| OP-07 | `Paciente` | `cadastrar(dadosPermitidos, solicitante: Usuario): Paciente` | RF-018 | "Gerenciar pacientes" |
| OP-08 | `Paciente` | `consultarCadastros(solicitante: Usuario): List<Paciente>` | RF-012 | "Gerenciar pacientes" (consultar) |
| OP-09 | `RegistroAuditoria` | `consultar(filtro, solicitante: Usuario): List<RegistroAuditoria>` | RF-013, RN-008, RN-014 | "Consultar auditoria" |
| OP-10 | `Agenda` | `cadastrarDisponibilidade(periodo: Periodo, solicitante: Usuario): Disponibilidade` | RF-023, RN-011 | "Gerenciar perfil e agenda" |
| OP-11 | `Agenda` | `consultarHorariosDisponiveis(paciente: Paciente, periodo: Periodo): List<Disponibilidade>` | RF-024, RN-010, CA-RF-024 | UC-025 passo 1 |
| OP-12 | `Agenda` | `consultarCompromissos(solicitante: Usuario): List<Agendamento>` | RF-029, RN-011 | "Gerenciar perfil e agenda" (RF-029) |
| OP-13 | `Agendamento` | `confirmar(solicitante: Usuario)` — assinatura completada | RF-026, RN-013, CA-RF-026 | UC-025 passo 4 |
| OP-14 | `Agendamento` | `cancelar(solicitante: Usuario, motivo: Texto)` — assinatura completada | RF-027, RN-012, RN-009 | UC-027 |
| OP-15 | `Agendamento` | `podeSerCancelado(): Booleano` | RF-027, RN-012 | UC-027 passo 3 (guarda) |
| OP-16 | `Agendamento` | `estaCancelada(): Booleano` | RN-006 | RN-006 (impedir atendimento) |
| OP-17 | `Agendamento` | `concluir(solicitante: Usuario)` | RF-030, RN-006, plano §5.4 | Transição para REALIZADO (EST-001) |
| OP-18 | `Agendamento` | `registrarFalta(solicitante: Usuario)` | plano §5.4 (estado "Não compareceu"), UC-027/DEC-5 | Estado terminal do ciclo |
| OP-19 | `Agendamento` | `consultarMeus(solicitante: Usuario): List<Agendamento>` | RF-028, RN-004 | UC-027 passo 1 |
| OP-20 | `Atendimento` | `registrar(agendamento: Agendamento, contextoSessao, solicitante: Usuario): Atendimento` — troca o par de participantes pelo agendamento | RF-030, RN-006, RN-007, CA-RF-030 | "Atender pacientes vinculados" |
| OP-21 | `Vinculo` | `encerrar(solicitante: Usuario, data: Data): Vinculo` | RF-021 (obs.), RN-003, RN-007, DUV-003 | Encerramento do vínculo |
| OP-22 | `Vinculo` | `estaAtivo(): Booleano` | RN-003, RN-007 | Consulta de autorização |

Observações de projeto para OP-11 e OP-20:

- **OP-11 devolve `Disponibilidade`, e não um novo tipo "HorárioDisponível".** Com o modelo de **slot discreto** (decisão P-05 do I4, apoiada nesta revisão), cada `Disponibilidade` já é um horário ofertado; a lista devolvida é simply o conjunto de slots da agenda ainda não ocupados. Isso evita criar uma classe nova e mantém a coerência com RN-005/RN-010.
- **OP-20 troca participants por agendamento.** O `Atendimento` passa a se referenciar ao `Agendamento` (A15), o que torna verificáveis "sessão válida da agenda" (RF-030) e "cancelado não pode ser registrado como realizado" (RN-006), além de fechar a cardinalidade de A15.

### 5.2 Responsabilidades de aplicação/fronteira (não vão para o diagrama de classes de domínio)

O plano §5.2 proíbe "classes onipotentes" e o OO-16 do Integrante 3 proíbe atribuir login/sessão/consulta global a entidades. As responsabilidades abaixo **não são entidades do domínio** e devem aparecer como objetos de aplicação/borda nas sequências e, depois, como componentes (Fase 7).

| ID | Responsabilidade | Base | Onde entra |
|---|---|---|---|
| AP-01 | `ServicoAutenticacao.autenticar(email, senha)` → cria/invalida sessão | RF-001, RF-002 | SEQ-001; componente de aplicação |
| AP-02 | `ServicoAuditoria.registrar(operacao, responsavel, detalhes)` — dispara o registro após operações auditáveis | RN-009, RF-032, RNF-004 | SEQ-001/002/003; fecha PC-12 |
| AP-03 | `ServicoAgenda.solicitar/confirmar/cancelar` — coordena regra, auditoria e notificação | RF-025–027, RN-005, RN-009 | SEQ-003 |
| AP-04 | `ServicoNotificacao` + interface `ServicoEnvioMensagem` (fronteira externa) | DEC-009, UC-025 (<<include>>) | SEQ-003; componente externo (I4, INC-10) |
| AP-05 | `ServicoConsulta`/projeções para as listagens administrativas | RF-010–013, RF-028–029 | SEQ e componentes; resolve as consultas por projeto de leitura, sem poluir as entidades |

**Observação de método:** AP-05 existe porque as listagens (RF-010–013, RF-028–029) devolvem coleções com filtros e projeções. Duas alternativas foram avaliadas: (a) operação de consulta em cada entidade (OP-04, OP-06, OP-08, OP-09, OP-12, OP-19) ou (b) uma camada de consulta. **Proposta hybrid:** manter as operações OP-xx nas entidades, porque elas expressam a **regra de acesso** (quem pode ver o quê — RN-003, RN-004, RN-008), e deixar a camada AP-05 para paginação/filtro. Isto preserva a rastreabilidade RF→classe→operação e não cria classe onipotente.

### 5.3 Mensagens das sequências da Fase 6 e a operação que as executa

Preparação para a Fase 6. Cada mensagem proposta já tem operação correspondente; as marcadas como **(lacuna)** são as que a Fase 6 **não pode** desenhar sem decisão do grupo.

| Sequência | Mensagem | Operação | Situação |
|---|---|---|---|
| SEQ-001 (autenticar) | `autenticar(email, senha)` | AP-01 | depende de OP-01/OP-02 |
| | `confereCredencial(credencial)` | OP-02 | proposta |
| | `podeAutenticar()` | OP-01 | proposta |
| | `criarSessao(usuario)` | AP-01 | fora do domínio (P-07) |
| | `registrar(operacao, responsavel, detalhes)` | AP-02 / `RegistroAuditoria.registrar` | parcial |
| SEQ-002 (vincular) | `formalizar(psicologo, paciente, solicitante)` | `Vinculo.formalizar` | coberto |
| | `consultarCadastros(solicitante)` | OP-05/OP-08 | proposta |
| | `registrar(...)` | AP-02 | parcial |
| SEQ-003 (agendar) | `consultarHorariosDisponiveis(paciente, periodo)` | OP-11 | proposta |
| | `solicitar(psicologo, paciente, horario, solicitante)` | `Agendamento.solicitar` | parcial (tipo de `horario`, PC-07) |
| | `verificarDisponibilidade(horario)` | `Agenda.verificarDisponibilidade` | coberto |
| | `confirmar(solicitante)` | OP-13 | assinatura incompleta hoje |
| | `cancelar(solicitante, motivo)` | OP-14 | assinatura provisória hoje |
| | `podeSerCancelado()` | OP-15 | proposta |
| | `notificar(confirmacao)` | AP-04 | DEC-009 pendente |
| SEQ-004 (listagem de espera — opcional) | — | — | **inexistente**: DEC-010 pendente, sem RF, sem caso de uso (R5-14) |

## 6. Resultado 3 — Validação de estados e eventos citados nos casos

O plano §5.4 exige que **os mesmos estados apareçam no domínio, nos casos e no banco** ("Solicitado → Confirmado → Realizado; saídas para Cancelado e Não compareceu"). Hoje os três atributos de situação existem, mas **sem domínio de valores**.

| Situação citada nos casos / RF | Onde aparece | Atributo no domínio | Coerente? | Situação recomendada |
|---|---|---|---|---|
| "conta cadastrada e ativa"; "conta bloqueada" | UC-001 (pré-condição, E2); RF-005, RF-006; RN-001 | `Usuario.situacaoConta` (PC-02) | **Não** — tipo PENDENTE, sem valores nem transições | **ATIVO, BLOQUEADO, INATIVO** — exatamente os valores do dicionário de dados do plano §10.2. Transições: ATIVO→BLOQUEADO (`bloquear`), BLOQUEADO→ATIVO (`ativar`), ATIVO→INATIVO (inativação administrativa). **Só ATIVO autentica** (RN-001) |
| "situação cadastral" do paciente | RF-020, CA-RF-020, RN-009 | `Paciente.situacaoCadastral` (PC-05) | **Não** — valores e transições PENDENTES | **ATIVO, SUSPENSO, INATIVO**. Transições livremente entre si, sempre por Administrador autorizado e com auditoria (RN-009) |
| "solicitada" / "confirmada" / "cancelada" | UC-025 (confirmada automática), UC-027 (cancelável), RF-025/026/027; RN-005, RN-006, RN-012, RN-013 | `Agendamento.situacao` (PC-07) | **Não** — nenhum valor nomeado | **SOLICITADO, CONFIRMADO, REALIZADO, CANCELADO, NAO_COMPARECEU** — nomenclatura do plano §5.4, para casar com EST-001 e com a coluna `agendamento.status` (§10.2) |
| "vínculo formalizado" | UC-021, RF-021, RN-003, RN-007 | `Vinculo` — **sem atributo de situação** | **Não** — o vínculo não tem estado | **ATIVO, ENCERRADO**, com `dataInicio` e `dataFim` (fecha DUV-003 e I4/INC-04, P-12) |
| "horário disponível / ocupado" | UC-025, RF-024, RN-005, RN-010 | `Disponibilidade.periodo` + `Agenda.verificarDisponibilidade` | **Parcial** — não há estado de ocupação | Sem estado novo: a ocupação é **derivada** dos agendamentos da agenda (RN-005). Se o grupo adotar slot discreto, a reserva é um vínculo explícito `Disponibilidade ← Agendamento` (A17, condicional) |
| "atendimento realizado" | RF-030, RN-006, RN-007 | `Atendimento` — sem situação | **Parcial** | A **existência** do `Atendimento` significa sessão registrada (RF-030 é "registro conceitual"); não é preciso estado adicional. Só é aceitável se o `Atendimento` passar a referenciar o `Agendamento` (OP-20) |
| "não compareceu" | UC-027 (DEC-5 em aberto); plano §5.4 | — | **Não** | Estado terminal do `Agendamento`, conforme plano §5.4. **Pendente de decisão**: manual (Psicólogo) ou automático por prazo |

### 6.1 Transições propostas para `Agendamento` (base para EST-001 da Fase 6)

| De | Evento | Para | Guarda | Autor | Base |
|---|---|---|---|---|---|
| — | `solicitar` | SOLICITADO | vínculo autorizado + slot livre | Paciente | RF-025, RN-010 |
| SOLICITADO | `confirmar` | CONFIRMADO | slot ainda livre | Sistema (automático, na regra) ou Administrador | RF-026, RN-013, UC-025 |
| CONFIRMADO | `concluir` | REALIZADO | sessão realizada | Psicólogo | RF-030, plano §5.4 |
| CONFIRMADO | `registrarFalta` | NAO_COMPARECEU | — | Psicólogo | plano §5.4, DEC-5 |
| SOLICITADO / CONFIRMADO | `cancelar` | CANCELADO | `podeSerCancelado()` | Paciente dono / Psicólogo vinculado | RF-027, RN-012, DEC-011 |
| CANCELADO / REALIZADO / NAO_COMPARECEU | — | — | estado terminal | — | RN-006 |

**Consequências que precisam ser aceitas pelo grupo (não são detalhe de modelagem):**

1. A confirmação automática de UC-025 só é coerente se o **mecanismo** for o ator autorizado (RN-013 permite "ator **ou mecanismo** autorizado"). A assinatura `confirmar(solicitante)` fica completa e a regra de quem dispara fica no caso de uso.
2. Se **SOLICITADO e CONFIRMADO** ocupam o horário, o índice de exclusividade de RN-005 precisa dos dois (INC-06 do I4) — é por isso que P-03 é decisão obrigatória.
3. "Cancelar" a partir de SOLICITADO faz sentido; "cancelar" um atendimento REALIZADO é proibido (RN-006).

### 6.2 Eventos citados nos casos e sua representação no domínio

| Evento | Origem | Representação | Situação |
|---|---|---|---|
| `informar credenciais` / `validar credencial` | UC-001 | OP-02 + AP-01 | proposta |
| `criar sessão` / `encerrar sessão` | UC-001 A1, RF-002 | AP-01 (fora do domínio) | P-07 |
| `ativar` / `bloquear` | RF-005, RF-006 | `Usuario.ativar/bloquear` | coberto |
| `formalizar vínculo` | UC-021 | `Vinculo.formalizar` | coberto |
| `encerrar vínculo` | DUV-003 | OP-21 + `Vinculo.situacao` | proposta |
| `cadastrar disponibilidade` | RF-023 | OP-10 | proposta |
| `consultar horários` | RF-024, UC-025 | OP-11 | proposta |
| `solicitar` | RF-025 | `Agendamento.solicitar` | coberto |
| `confirmar` | RF-026, UC-025 | OP-13 | assinatura a completar |
| `cancelar` | RF-027, UC-027 | OP-14 + OP-15 | assinatura a completar |
| `concluir` / `registrar falta` | RF-030, plano §5.4 | OP-17, OP-18 | proposta |
| `registrar atendimento` | RF-030 | OP-20 | proposta (redesenho do parâmetro) |
| `alterar situação cadastral` | RF-020 | `Paciente.alterarSituacao` | coberto (estados a definir) |
| `registrar auditoria` | RF-032, RN-009 | `RegistroAuditoria.registrar` + AP-02 | parcial |
| `notificar por e-mail` | UC-025 (<<include>>), DEC-009 | AP-04 | decisão pendente |
| `entrar na lista de espera` | plano §3.2, DEC-010 | **inexistente** | decisão pendente |

## 7. Resultado 4 — Multiplicidades: proposta de fechamento (PC-09 / P-06 / P-11)

O plano §19 penaliza explicitamente modelagem sem multiplicidades, e o Integrante 4 bloqueou o DER (P-01…P-05) por pendências de cardinalidade. As 16 associações seguem com `PENDENTE` em ambos os extremos. **Proposta de fechamento para aprovação do grupo** (nenhuma foi aplicada nos arquivos do Integrante 3 por esta revisão):

| ID | Associação | Multiplicidade proposta | Justificativa | Alinhamento |
|---|---|---|---|---|
| A01 | Usuario — Perfil | `Usuario "0..1" — "1" Perfil` | Toda conta que autentica possui exatamente um perfil (RF-003, RF-001); perfis são reutilizados por várias contas | Fecha INC-01 (recomendação do I4: 1:1 com UNIQUE) |
| A02 | Perfil — Permissao | `Perfil "1" — "0..*" Permissao` | Um perfil agrupa zero ou mais permissões; uma permissão pode servir a vários perfis (N:N) | Plano §10.1 (`perfil_permissao`) |
| A03 | Usuario — Psicologo | `Usuario "0..1" — "0..1" Psicologo` | Conta de acesso é distinta do cadastro profissional (RF-014); um profissional pode existir sem conta e uma conta atende a no máximo um cadastro profissional | Fecha INC-12 / P-11 |
| A04 | Usuario — Paciente | `Usuario "0..1" — "0..1" Paciente` | Mesma lógica para o paciente (RF-018, DEC-006) | Fecha INC-12 / P-11 |
| A05 | Psicologo — AreaAtuacao | `Psicologo "0..*" — "1" AreaAtuacao` | Várias áreas por profissional (RF-016); uma área pode atender vários profissionais. Invariante: perfil ativo exige ao menos uma área | Plano §10.1 (`psicologo_area_atuacao`); ver INC-09 |
| A06 | Psicologo — Vinculo | `Psicologo "1" — "0..*" Vinculo` | Um vínculo identifica **um** profissional (glossário, RF-021) | — |
| A07 | Paciente — Vinculo | `Paciente "1" — "0..*" Vinculo` | Um vínculo identifica **um** paciente | — |
| A08 | Psicologo — Agenda | `Psicologo "1" — "1" Agenda` | Agenda **própria** (RN-011, RF-023, RF-029): um profissional, uma agenda | Resolve a validação pedida em PC-09 |
| A09 | Agenda — Disponibilidade | `Agenda "1" — "0..*" Disponibilidade` | Períodos pertencem à agenda titular (RN-011) | Resolve a validação pedida em PC-09 |
| A10 | Agenda — Agendamento | `Agenda "1" — "0..*" Agendamento` | Compromissos são apresentados na agenda do titular (RF-029) | — |
| A11 | Psicologo — Agendamento | `Psicologo "1" — "0..*" Agendamento` | Cada agendamento tem um profissional | — |
| A12 | Paciente — Agendamento | `Paciente "1" — "0..*" Agendamento` | Cada agendamento tem um paciente | — |
| A13 | Psicologo — Atendimento | `Psicologo "1" — "0..*" Atendimento` | Cada atendimento tem um profissional responsável (RF-030, RN-007) | — |
| A14 | Paciente — Atendimento | `Paciente "1" — "0..*" Atendimento` | Cada atendimento tem um paciente (RF-030, RN-007) | — |
| A15 | Agendamento — Atendimento | `Agendamento "1" — "0..1" Atendimento` | Uma sessão realizada corresponde a no máximo um registro conceitual; o agendamento cancelado não o tem (RN-006) | Fecha PD-09 com OP-20 |
| A16 | Usuario — RegistroAuditoria | `Usuario "0..1" — "0..*" RegistroAuditoria` | Eventos gerados pelo sistema (confirmação automática, expiração) não têm usuário responsável; eventos administrativos sempre têm | Alinha com RNF-004 e com OP-02 |

**Associação nova proposta (condicional):**

| ID | Associação | Multiplicidade proposta | Condição | Justificativa |
|---|---|---|---|---|
| A17 | Disponibilidade — Agendamento | `Disponibilidade "0..1" — "0..*" Agendamento` | **Somente se** o grupo aprovar o modelo de slot discreto (P-05) | Liga o agendamento ao período ofertado reservado; torna verificável RN-005/RN-010 e o índice de exclusividade |

> Observação: com A17 opcional e o modelo de slot discreto, a regra RN-005 fica: *um `Disponibilidade` não pode ter dois agendamentos em situação SOLICITADO/CONFIRMADO*. É a formulação mais simples e portátil para o banco (inciso INC-05 do I4).

## 8. Achados da revisão (R5-01 … R5-15)

Severidade: **C** = crítico (bloqueia a Fase 6), **A** = alto (bloqueia DER/dossiê), **M** = médio, **B** = baixo.

| ID | Sev. | Achado | Evidência | Ação recomendada | Responsável |
|---|---|---|---|---|---|
| R5-01 | C | Nenhuma operação executa a autenticação | UC-001 vs `especificacao-classes.md` §4 | Incorporar OP-01, OP-02 + AP-01 | I3 + I2 |
| R5-02 | C | `confirmar()` sem assinatura e `cancelar()` provisória contradizem UC-025 (confirmação automática) e UC-027 | PC-07, RN-012, RN-013 | Completar assinaturas (OP-13, OP-14) + decidir DEC de mecanismo | I1 + I3 |
| R5-03 | C | Não há operação que devolva a **lista** de horários disponíveis (RF-024) | OO-11 admite; RF-024 exige | Incorporar OP-11 | I3 |
| R5-04 | C | Três atributos de situação sem domínio de valores; EST-001 e `agendamento.status` dependem disso | PC-02, PC-05, PC-07, P-01–P-04 | Aprovar os conjuntos da §6 | I1 + grupo |
| R5-05 | A | `Atendimento.registrar` não referencia o `Agendamento`: "sessão válida" (RF-030) e RN-006 ficam invéridos | PC-08, PD-09 | Adotar OP-20 e fechar A15 | I3 |
| R5-06 | A | 11 RFs sem responsabilidade de operação (consultas e criação) | Cobertura por RF do I3 ("colaboração posterior") | Incorporar OP-03…OP-12, OP-19 | I3 |
| R5-07 | A | Não existe criação de conta, de cadastro profissional/paciente nem de `Disponibilidade` | RF-003, RF-014, RF-018, RF-023 | Incorporar OP-03, OP-05, OP-07, OP-10 | I3 |
| R5-08 | A | Auditoria exigida por RN-009 não tem colaboração definida | PC-12, OO-15 | Definir AP-02 e o conteúdo (situação anterior/nova — INC-07) | I2 + I3 |
| R5-09 | M | Vínculo sem temporalidade nem estado; DUV-003 sem resposta | DUV-003, INC-04, P-12 | OP-21 + `Vinculo.situacao` (ATIVO/ENCERRADO) | I1 + I3 |
| R5-10 | M | 14 das 16 associações com multiplicidade `PENDENTE`; sem isso o diagrama não é UML válido | `diagrama-classes.mmd`, PC-09, plano §19 | Aprovar a §7 | Grupo + I3 |
| R5-11 | M | OBJ-001 não exemplifica o cenário mais relevante (agendamento) nem entidades com estado | `especificacao-objetos.md` §2 | Produzir o cenário OBJ-002 (§10) | I2 |
| R5-12 | M | Inconsistência entre artefatos e decisões: (a) o caso "Notificar paciente por e-mail" afirma que **DEC-009 está resolvido (sim)**, enquanto o registro oficial diz `PENDENTE DE DECISÃO DO GRUPO`; (b) UC-027 cita **DEC-011 e DEC-012 que não existem** no registro (DEC-001–010); (c) UC-025/UC-027 citam "DEC-4" e "DEC-5", que colidem com DEC-004 (Equipe) e DEC-005 (Dados) | `decisoes.md` (DEC-001–010) vs `especificacoes-casos-de-uso-*.md` | Formalizar DEC-011+ e alinhar os casos de uso e o diagrama de casos | I1 + I2 |
| R5-13 | M | A análise de viabilidade de persistência da Fase 5 (I4) foi feita sobre a lista do plano §5.1, **não** sobre o modelo de classes real (o próprio documento declara que as pastas `Classes` e `Objetos` ainda não existiam) | `fase-05-viabilidade-persistencia-dominio.md` §1 | Refazer a análise contra o modelo aprovado antes do DER | I4 |
| R5-14 | M | Innoção (DEC-010) e serviço externo (DEC-009) pendentes, embora o plano §3.2 exija o módulo de inovação no escopo | DEC-009/010, plano §3.2 | Decidir em reunião: aprovar a lista de espera inteligente | Grupo |
| R5-15 | B | Nenhuma composição modelada; a relação Agendamento–Atendimento tem ciclo de vida dependente justificável | OO-03 (conservador) | Avaliar composição em A15 (opcional) | I3 + I5 |
| R5-16 | M | Os casos de uso críticos (UC-001, UC-021, UC-025, UC-027) **não têm a linha "RFs cobertos"**, presente nos demais casos; a rastreabilidade depende de inferência do leitor | `especificacoes-casos-de-uso-criticos.md` vs `especificacoes-casos-de-uso-demais.md` | Acrescentar "RFs cobertos" e "Regras de negócio aplicadas" nos quatro casos críticos | I2 |
| R5-17 | M | RF-024, RF-028 e RF-029 estão absorvidos em outros casos (passo 1 de UC-025, fluxo A1 de UC-015) e **não têm caso de uso próprio**, o que dificulta a sequência e o teste isolado | `especificacoes-casos-de-uso-*.md` | Propor UC-024, UC-028 e UC-029 (matriz §1) | I2 + I1 |

## 9. O que **não** foi proposto (e por quê)

Regra do plano §8: *"Não criar métodos apenas para satisfazer diagramas futuros sem responsabilidade clara."* Estas sugestões foram **descartadas** deliberadamente:

| Descartado | Motivo |
|---|---|
| `Usuario.autenticar(email, senha)` na entidade | Envolve sessão, armazenamento de credencial e infraestrutura (RF-002, P-07); cabe a AP-01. A entidade responde apenas *se pode* e *se a credencial confere* (OP-01/OP-02) |
| `Agendamento.remarcar()` / `reagendar()` | Nenhum RF ou RN fala em remarcação (RF-027 trata cancelamento) |
| `Agendamento.setSituacao()` / `atualizarSituacao()` | Destruiria o encapsulamento e a máquina de estados (EST-001) |
| `Agenda.removerDisponibilidade()` | RF-023 cobre cadastro; remoção não está em nenhum RF. Reavaliar se o grupo quiser |
| `Vinculo.estaAtivo()` antes de definir o estado | Proposto **apenas** junto com a decisão de estado (§6) e a temporalidade (R5-09) |
| `Paciente.consultarHistorico()` | Expõe registro profissional restrito ao paciente (RN-004, RNF-005) |
| Getters/setters para todos os atributos | OO-04; quebram encapsulamento e poluem a especificação |
| Classe `Sistema`, `GerenciadorGeral`, `Plataforma` | Plano §5.2: classes onipotentes. As responsabilidades de aplicação estão em AP-01…AP-05 |
| Classe de domínio `Notificacao` | DEC-009 pendente e nenhum RF correspondente (RNF de órfãos: plano §6.1). A fronteira vira interface em AP-04 |
| Classes `ListaEspera` / `EntradaListaEspera` | DEC-010 pendente; sem RF e sem caso de uso (R5-14). Modelo só depois da decisão |
| `Sessao` como classe de domínio | RF-002 é volátil e técnico; decisão P-07 do I4 |
| Herdar `Psicologo`/`Paciente` de `Usuario` | Plano §5.2 e OO-01: identidade ≠ autorização; herança por permissão é enganosa |

## 10. Revisão do diagrama de objetos (OBJ-001) e proposta de cenário adicional

**Constatação (R5-11):** o OBJ-001 é uma "fotografia parcial" com 7 objetos de 6 classes — conta profissional, conta paciente, vínculo, agenda e disponibilidade. Ele **não** mostra `Agendamento`, `Atendimento`, `RegistroAuditoria`, `Perfil`/`Permissao` nem `AreaAtuacao`, ou seja, não exemplifica o processo mais relevante do projeto (agendar → confirmar → cancelar → registrar) nem nenhuma entidade com situação. Um diagrama de objetos que só mostra cadastro não demonstra o modelo.

**Proposta:** o arquivo `03-uml/Objetos/proposta-I2-diagrama-objetos-v2.mmd` (especificado em `03-uml/Objetos/proposta-I2-especificacao-objetos-v2.md`) descreve o cenário OBJ-002 — *solicitação de agendamento confirmada e depois cancelada, com notificação e auditoria* — com:

- objetos: `contaPacienteCenario`, `pacienteCenario`, `contaProfissionalCenario`, `profissionalCenario`, `areaAtuacaoCenario`, `vinculoCenario`, `agendaCenario`, `disponibilidadeCenario`, `agendamentoRealizadoCenario`, `agendamentoCanceladoCenario`, `atendimentoCenario`, `notificacaoEnviadaCenario` (objeto de aplicação), `auditoriaConclusaoCenario`, `auditoriaCancelamentoCenario`, `perfilPacienteCenario`, `permissaoConsultaCenario`;
- valores **inteiramente fictícios** (`AGENDAMENTO-FICTICIO-01`, `CONTA-FICTICIA-C01`, `2026-11-03 14:00`), sem nome de pessoa, sem CPF/e-mail/CRP real (DEC-005);
- slots apenas de atributos **já existentes** no modelo, mais o slot `situacao` (proposta de OP/§6) e `operacao` da auditoria.

Observação de rastreabilidade: o OBJ-002 usa a nomenclatura de situação proposta na §6 (`SOLICITADO`, `CONFIRMADO`, `CANCELADO`). Se o grupo aprovar nomes diferentes, o arquivo precisa ser atualizado em conjunto — é o mesmo mecanismo de revisão cruzada do plano §13.

## 11. Checklist do plano §13 aplicado à Fase 5

| Regra | Verificação | Resultado |
|---|---|---|
| Todo ator do diagrama de casos de uso está definido na documentação | Administrador, Psicólogo, Paciente | **OK** |
| Todo caso de uso possui requisito/objetivo rastreável | 13 UCs com RF ou DEC | **OK**, exceto "Notificar por e-mail" (sem RF) → R5-12 |
| Conceitos relevantes dos casos de uso aparecem no modelo de domínio | 12 classes | **OK**; falta a noção de *notificação* e *lista de espera* (decisões pendentes) |
| Cada classe possui responsabilidade clara; sem classes onipotentes | 12 classes | **OK** — nenhuma classe onipotente identificada |
| Mensagens de sequência correspondem a operações conhecidas | §5.3 | **Parcial** — 2 dependem de OP-01/OP-02, 4 de assinaturas a completar |
| Estados e transições têm eventos, condições ou ações coerentes | §6 | **Não** — nenhum estado nomeado hoje (R5-04) |
| Entidades persistentes têm correspondência com classes | §7 | **Parcial** — A15 e A17 em aberto |
| FKs e cardinalidades respeitam as multiplicidades do domínio | §7 | **Não** — multiplicidades PENDENTE (R5-10) |
| Mudanças em um artefato provocam revisão dos relacionados | §12 | **Aplicado** — esta revisão-propõe e o registro de decisões |

## 12. Pendências que permanecem após esta revisão

| ID | Pendência | Fecha com |
|---|---|---|
| PC-02 | Valores de `situacaoConta` | Proposta §6 (ATIVO/BLOQUEADO/INATIVO) |
| PC-05 | Valores de `situacaoCadastral` e transições | Proposta §6 (ATIVO/SUSPENSO/INATIVO) |
| PC-06 | Estado e temporalidade do vínculo | Proposta §6 + OP-21/OP-22 |
| PC-07 | Tipos temporais, situação inicial, confirmação e cancelamento | Propostas §6.1, §7 e OP-13/OP-14 |
| PC-08 | Conteúdo do atendimento e relação com o agendamento | OP-20 |
| PC-09 | Cardinalidades A01–A16 | Proposta §7 |
| PC-10 | Vocabulário da operação auditada e ordenação | AP-02 + INC-07 (situação anterior/nova) |
| PC-11 | Inovação e serviço externo | DEC-009/DEC-010 (grupo) |
| PC-12 | Colaborações, retornos e erros | §5.1, §5.2, §5.3 |
| P-01 | Ator/mecanismo da confirmação | §6.1 (mecanismo automático) |
| P-02 | Prazo, situações canceláveis e atores | UC-027 + DEC-011/DEC-012 (propostos) |
| P-03 | Situação inicial e ocupação do horário | §6.1 (SOLICITADO ocupa o slot) |
| P-04 | Situações cadastrais do paciente | §6 (ATIVO/SUSPENSO/INATIVO) |
| P-05 | Modelo de tempo da disponibilidade | §5.1 (slot discreto) |
| P-06 | Cardinalidade Usuario–Perfil | §7 (0..1/1) |
| P-07 | Sessão: entidade ou volátil | §5.2 (fora do domínio) |
| P-08 | Catálogo de áreas | §7 (manter `AreaAtuacao`) |
| P-09 | Serviço externo de notificação | §5.2 (interface de fronteira) |
| P-10 | Funcionalidade de inovação | Grupo |
| P-11 | Identidade do profissional/paciente | §7 (0..1 opcional e única) |
| P-12 | Escopo temporal do vínculo | §6 (dataInicio/dataFim) |
| INC-01…INC-12 | Inconsistências multiplicidade × regra | §7 e §6 tratam INC-01, INC-03, INC-04, INC-05, INC-06, INC-07, INC-09, INC-10, INC-11, INC-12 |

## 13. Checklist de aprovação (para a ata do walkthrough)

- [ ] Grupo aprova os conjuntos de situações da §6 ( Usuario, Paciente, Agendamento, Vínculo ).
- [ ] Grupo decide o mecanismo de confirmação (P-01) e o prazo/actors do cancelamento (P-02).
- [ ] Grupo decide o modelo de tempo da disponibilidade: slot discreto ou intervalo contínuo (P-05).
- [ ] Grupo decide DEC-009 (notificação) e DEC-010 (inovação) — travam o modelo e o pitch.
- [ ] Integrante 3 incorpora OP-01…OP-22 e as multiplicidades da §7, atualizando `especificacao-classes.md` e `diagrama-classes.mmd`.
- [ ] Integrante 1 atualiza a [matriz de rastreabilidade da Fase 5](../06-rastreabilidade/matriz-rastreabilidade-fase5.md) com o resultado das decisões.
- [ ] Integrante 4 refaz a viabilidade de persistência contra o modelo aprovado (R5-13).
- [ ] Integrante 2 parte para os diagramas de sequência da Fase 6 com a §5.3.

**Registro de participação desta revisão:** Integrante 2 (Aquiles Angelo) — análise dos 13 casos de uso, das 12 classes, das 16 associações, dos 32 RFs e das 14 RNs; conferido contra o modelo do Integrante 3 e a análise do Integrante 4. Revisão cruzada por outro integrante ainda **pendente** (Definition of Done do plano §13.1).

## 14. Entregáveis desta Fase 5 (Integrante 2)

| Arquivo | Conteúdo | Situação |
|---|---|---|
| `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md` | Esta revisão: cobertura UC × classes, responsabilidades ausentes, validação de estados/eventos, multiplicidades propostas e achados R5-01…R5-17 | Pronto para walkthrough |
| `03-uml/Classes/proposta-I2-diagrama-classes-v2.mmd` | CLS-001 v2 **proposto**: multiplicidades fechadas (A01–A17), quatro enumerações de situação e as operações OP-01…OP-22 | Proposta para o Integrante 3 incorporar |
| `03-uml/Objetos/proposta-I2-diagrama-objetos-v2.mmd` | OBJ-002 **proposto**: fotografia do ciclo de agendamento (realizado e cancelado), com atendimento, auditoria e notificação | Proposta para o Integrante 3/Integrante 6 |
| `03-uml/Objetos/proposta-I2-especificacao-objetos-v2.md` | Especificação do OBJ-002: objetos, ligações, regras verificáveis e ausências significativas | Pronto para walkthrough |
| `06-rastreabilidade/matriz-rastreabilidade-fase5.md` | Matriz RF → RN/RNF → UC → classe → **operação** → sequência → tabela → teste, verificação de órfãos e tabelas propostas | Proposta para o Integrante 1 homologar |
| `07-dossie/secoes/fase5-secao-18-20-27.md` | Texto pronto para as seções 18, 19, 20 e 27 do dossiê final | Texto para o Integrante 6 consolidar |
| `01-planejamento/decisoes.md` (bloco final) | Propostas DEC-011…DEC-026, com justificativa e efeito no modelo | Aguardando aprovação do grupo |

**Sequência sugerida ao grupo:** (1) walkthrough desta revisão; (2) decisão das DEC-011…DEC-026; (3) incorporação pelo Integrante 3 das operações e multiplicidades aprovadas; (4) Fase 6 (sequências, atividades, estados) pelo Integrante 2, usando a §5.3; (5) Fase 7 (pacotes, componentes, DER) pelo Integrante 4, com a §4 da matriz de rastreabilidade.
