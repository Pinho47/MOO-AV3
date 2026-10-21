# Fase 4 — Casos de Uso: Revisão de Dados por Caso de Uso

**Responsável:** Integrante 4 — Caio Cunha (Persistência e arquitetura)
**Fase do plano:** Fase 4 — Casos de uso e especificações
**Tarefas do plano cobertas:** Identificar em cada caso de uso quais informações precisam ser consultadas, criadas ou alteradas; marcar integrações externas a considerar em componentes e implantação; registrar regras de persistência implícitas que precisem ser formalizadas como RN ou RNF; revisar os casos que envolvam auditoria, autenticação, autorização e dados sensíveis.
**Insumos:** RF-001 a RF-032, RN-001 a RN-014, RNF-001 a RNF-012, CA-RF-001 a CA-RF-032, `02-requisitos/rastreabilidade-inicial.md`, `02-requisitos/glossario.md`, `03-uml/Contexto/Interações_Processos.md`.

## 1. Objetivo e limitação de insumo

Este documento estabelece, para cada caso de uso previsto, **quais dados são lidos, criados e alterados**, e por consequência **quais tabelas, chaves e restrições o modelo relacional terá de suportar**. É o insumo direto do DER (Fase 7).

**Limitação declarada:** as especificações textuais dos casos de uso são responsabilidade do Integrante 2 e ainda não foram produzidas (`03-uml/Casos de uso/` contém apenas o arquivo de guarda). A análise abaixo usa os identificadores de trabalho `UC-FUT-*` já existentes em `02-requisitos/rastreabilidade-inicial.md`, que são explicitamente "identificadores de trabalho para o Integrante 2 confirmar ou substituir". **Esta análise é preliminar e deve ser revisada quando as especificações forem aprovadas.**

## 2. Método

Para cada caso de uso foram determinadas quatro coisas:

1. **Dados consultados** — entidades lidas, que geram as tabelas de consulta e os índices de leitura.
2. **Dados criados** — entidades inseridas, que geram as tabelas de inserção e as chaves primárias.
3. **Dados alterados** — entidades atualizadas, que geram as restrições de transição de estado.
4. **Integridade, auditoria e atomicidade** — se o caso exige integridade referencial entre tabelas, se produz trilha de auditoria (RN-009) e se as escritas devem ocorrer em uma única transação.

A coluna *Transação atômica* é a mais relevante para a Fase 7: casos que escrevem em mais de uma tabela e precisam ser indivisíveis determinam a fronteira transacional da aplicação.

## 3. Matriz de dados por caso de uso

| Caso de uso | RF | Dados consultados | Dados criados | Dados alterados | Integridade referencial | Auditoria | Transação atômica |
|---|---|---|---|---|---|---|---|
| UC-FUT-001 Autenticar usuário | RF-001 | usuario, perfil, permissao (via usuario_perfil e perfil_permissao) | sessao (condicional — ver P-07) | usuario (data de último acesso, condicional) | usuario ativo (RN-001); perfil e permissão existentes | Sim — sucesso e falha | Sim: leitura + auditoria indivisíveis |
| UC-FUT-002 Encerrar sessão | RF-002 | sessao | — | sessao (situação, expiração) | sessão pertence a usuário existente | Sim | Não |
| UC-FUT-003 Cadastrar usuário | RF-003 | perfil, permissao, e-mail (unicidade) | usuario, usuario_perfil | — | perfil e permissões existentes; e-mail único | Sim | Sim: usuário + vínculo de perfil + auditoria |
| UC-FUT-004 Alterar dados permitidos | RF-004 | usuario, perfil, permissao | — | usuario (somente campos liberados) | — | Sim | Não |
| UC-FUT-005 Ativar usuário | RF-005 | usuario | — | usuario.situacao | transição de situação válida | Sim | Sim: situação + auditoria |
| UC-FUT-006 Bloquear usuário | RF-006 | usuario, sessao (sessões abertas) | — | usuario.situacao; sessao (revogação, condicional) | transição de situação válida | Sim | Sim: situação + auditoria |
| UC-FUT-007 Gerenciar perfis | RF-007 | perfil, permissao | perfil, perfil_permissao | perfil; perfil_permissao | permissao existente | Sim | Sim |
| UC-FUT-008 Gerenciar permissões | RF-008 | permissao, perfil, perfil_permissao | permissao | permissao; perfil_permissao | perfil existente | Sim | Sim |
| UC-FUT-009 Consultar painel administrativo | RF-009 | usuario, perfil, permissao; contagensderived de paciente, agendamento, auditoria | — | — | — | Não (leitura) | Não |
| UC-FUT-010 Consultar usuários | RF-010 | usuario | — | — | — | Não (leitura) | Não |
| UC-FUT-011 Consultar psicólogos | RF-011 | psicologo, area_atuacao | — | — | — | Não (leitura) | Não |
| UC-FUT-012 Consultar pacientes | RF-012 | paciente | — | — | — | Não (leitura) | Não |
| UC-FUT-013 Consultar auditoria | RF-013 | auditoria, usuario | — | — | auditoria pertence a usuário existente | Não (leitura; RN-014 proíbe alteração) | Não |
| UC-FUT-014 Cadastrar psicólogo | RF-014 | usuario (ver INC-12) | psicologo | — | vínculo opcional com usuário | Sim | Sim |
| UC-FUT-015 Atualizar perfil profissional | RF-015 | psicologo | — | psicologo | — | Sim | Não |
| UC-FUT-016 Associar área de atuação | RF-016 | psicologo, area_atuacao | psicologo_area_atuacao | — | par único (psicologo, área) | Sim | Sim |
| UC-FUT-017 Consultar pacientes vinculados | RF-017 | vinculo, paciente, usuario | — | — | paciente vinculado ao psicólogo autenticado (RN-003) | Não (leitura) | Não |
| UC-FUT-018 Cadastrar paciente | RF-018 | usuario (ver INC-12), paciente | paciente | — | — | Sim | Sim |
| UC-FUT-019 Atualizar cadastro de paciente | RF-019 | paciente | — | paciente (dados administrativos) | — | Sim | Não |
| UC-FUT-020 Alterar situação do paciente | RF-020 | paciente | — | paciente.situacao | domínio de valores válido | Sim | Sim: situação + auditoria |
| UC-FUT-021 Formalizar vínculo | RF-021 | psicologo, paciente | vinculo | — | par (psicólogo, paciente) único; ambos existentes | Sim | Sim |
| UC-FUT-022 Consultar próprio cadastro | RF-022 | usuario, paciente | — | — | paciente da sessão autenticada (RN-004, RNF-005) | Não (leitura) | Não |
| UC-FUT-023 Cadastrar disponibilidade | RF-023 | psicologo, disponibilidade, agendamento (conflitos) | disponibilidade | — | disponibilidade pertence ao psicólogo autenticado (RN-011) | Sim | Sim: disponibilidade + auditoria |
| UC-FUT-024 Consultar horários disponíveis | RF-024 | disponibilidade, agendamento, vinculo, paciente | — | — | paciente vinculado ao psicólogo consultado (RN-010) | Não (leitura; não reserva) | Não |
| UC-FUT-025 Solicitar agendamento | RF-025 | paciente, vinculo, disponibilidade, agendamento | agendamento, auditoria | disponibilidade (marcação de ocupação, conforme decisão) | horário livre (RN-005, RN-010); par paciente/psicólogo vinculado (RN-003) | Sim | **Sim — transação crítica** |
| UC-FUT-026 Confirmar agendamento | RF-026 | agendamento | — | agendamento.situacao | transição válida; não gerar conflito (RN-005) | Sim | Sim: situação + auditoria |
| UC-FUT-027 Cancelar agendamento | RF-027 | agendamento, sessao, vinculo | — | agendamento.situacao; disponibilidade (liberação) | situação anterior cancelável (RN-012); impede atendimento (RN-006) | Sim | Sim: situação + auditoria |
| UC-FUT-028 Consultar próprios agendamentos | RF-028 | agendamento, paciente | — | — | paciente da sessão autenticada | Não (leitura) | Não |
| UC-FUT-029 Consultar agenda própria | RF-029 | agendamento, disponibilidade, paciente, vinculo | — | — | agenda do psicólogo autenticado (RN-011); vínculo para exibir paciente (RN-003) | Não (leitura) | Não |
| UC-FUT-030 Registrar atendimento | RF-030 | agendamento, vinculo, paciente, psicologo | atendimento | agendamento (situação para realizado, conforme máquina de estados) | par (psicólogo, paciente) deve existir como vínculo (RN-007); não referencia agendamento cancelado (RN-006) | Sim | **Sim — transação crítica** |
| UC-FUT-031 Consultar histórico permitido | RF-031 | atendimento, vinculo, paciente, psicologo | — | — | vínculo autorizado vigente (RN-003) | Não (leitura) | Não |
| UC-FUT-032 Registrar evento de auditoria | RF-032 | usuario | auditoria | — | auditoria pertence a usuário existente; imutável (RN-014) | Sim (é o próprio registro) | Sim |

**Leitura da matriz.** Quatro casos exigem atomicidade de mehrere tabelas com semântica de negócio forte: UC-FUT-003, UC-FUT-025, UC-FUT-030 e UC-FUT-021. Os casos UC-FUT-025 e UC-FUT-030 são os que determinam a fronteira transacional da aplicação, pois alteram simultaneamente o estado do agendamento, a ocupação da disponibilidade e a trilha de auditoria.

## 4. Integrações externas marcadas

A task do plano pede marcar integrações externas a considerar **posteriormente em componentes e implantação** (Fase 7). O resultado desta marcação é o insumo do diagrama de componentes e do diagrama de implantação.

| Integração | Situação no escopo | Efeito na persistência | Efeito na arquitetura | Referência |
|---|---|---|---|---|
| Serviço Externo de E-mail/Notificação | **PENDENTE DE DECISÃO DO GRUPO** | Se houver gravação interna de avisos, exige tabela `notificacao` com destinatário, tipo de evento, canal, situação de envio e data. Se for puramente externo, **não há tabela** e o componente consome eventos do domínio | Interface dedicada no diagrama de componentes (DIP — plano §15); nó externo no diagrama de implantação | DEC-009, `03-uml/Contexto/Interações_Processos.md` |
| Fonte de data e hora do servidor | Necessidade técnica, não ator | Auditoria, agendamento e disponibilidade dependem de uma **fonte única de tempo**; o cliente não deve fornecer o carimbo de tempo | Componente de infraestrutura no diagrama de implantação | RN-009, RNF-004, CA-RF-032 |
| Cálculo de fuso horário e recorrência | **PENDENTE** (RF-023 obs.: "Regras detalhadas de recorrência e fuso horário não foram definidas") | Afeta a forma de armazenar e comparar os intervalos de `disponibilidade`; é a origem da inconsistência INC-05 | Deve ficar isolado em um componente de tempo, sem dependência do SGBD | RF-023, RF-025 |
| Navegador e dispositivos dos usuários | Externo por definição | Nenhuma | Nós externos no diagrama de implantação | `02-requisitos/escopo.md` §5.2 |

Nenhuma outra integração externa está confirmada no escopo. O Atendente foi excluído (DEC-007) e a funcionalidade de inovação permanece pendente (DEC-010).

## 5. Regras de persistência implícitas a formalizar

As regras abaixo **não estão** formalizadas em `02-requisitos/regras-negocio.md`, mas decorrem diretamente dos fluxos e das pós-condições dos requisitos. São propostas deste Integrante 4 para incorporação pelo Integrante 1.

| Proposta | Regra proposta | Origem | Justificativa de impacto na persistência |
|---|---|---|---|
| **RN-015** | A alteração de situação de uma entidade de estado e o registro de auditoria correspondente devem ocorrer na mesma transação; se a auditoria falhar, a alteração é revertida. | RF-005, RF-006, RF-020, RF-025, RF-026, RF-027, RF-032, RN-009 | Sem atomicidade, um estado alterado pode ficar sem rastro de auditoria, quebrando RNF-004 e RN-009. |
| **RN-016** | Não pode existir mais de um vínculo vigente entre o mesmo psicólogo e o mesmo cliente/paciente. | RF-021, RN-003, RN-007, CA-RF-021 | Sem unicidade, vínculos duplicados produzem ambiguidade na autorização e multiplicam indevidamente os atendimentos derivados. |
| **RN-017** | Todo atendimento deve referenciar o agendamento que lhe deu origem e não pode referenciar agendamento cancelado. | RF-030, RF-006, CA-RF-027, CA-RF-030 | Torna a FK `atendimento → agendamento` obrigatória e exige guarda de transição entre as tabelas. |
| **RN-018** | Um agendamento ativo deve referenciar a disponibilidade efetivamente ocupada, e essa disponibilidade deve pertencer ao psicólogo do agendamento. | RF-023, RF-025, RN-005, RN-010, RN-011 | Impede que um agendamento aponte para a agenda de outro profissional; justifica a chave única composta em `disponibilidade`. |
| **RN-019** | O par (psicólogo, cliente/paciente) registrado em um atendimento deve existir como vínculo autorizado. | RF-030, RN-007, RN-003, CA-RF-030 | Sem essa regra, o modelo permite gravar um atendimento entre um profissional e um paciente sem vínculo. |

## 6. Revisão dos casos com auditoria, autenticação, autorização e dados sensíveis

| Caso de uso | Dado sensível ou restrito envolvido | Risco de persistência indevida | Exigência de modelagem | RN / RNF |
|---|---|---|---|---|
| UC-FUT-001 Autenticar usuário | Credencial e identidade | Gravar senha em texto puro; registrar a credencial na trilha de auditoria | Coluna dedicada de hash, nunca texto puro; nunca incluir credencial no registro de auditoria | RN-002, RNF-001 |
| UC-FUT-003 Cadastrar usuário | Credencial | Expor a credencial em respostas de leitura | Hash com algoritmo de custo; projeção de leitura que exclui a credencial | RN-002, RNF-001 |
| UC-FUT-010 Consultar usuários | Credenciais de todos os usuários | Vazamento em massa por listagem | A consulta nunca deve projetar a coluna de credencial | RN-002, RES-002 |
| UC-FUT-013 Consultar auditoria | Trilha operacional completa | Consulta como forma de alterar; exposição de conteúdo sensível colateral | Auditoria somente leitura para o ator autorizado; registro imutável; conteúdo restrito ao necessário | RN-008, RN-014 |
| UC-FUT-017 Consultar pacientes vinculados | Dados de pacientes vinculados a um profissional | Listagem global de pacientes disfarçada de vínculo | Toda consulta de paciente por profissional passa obrigatoriamente pelo vínculo | RN-003, RNF-006 |
| UC-FUT-022 Consultar próprio cadastro | Dados pessoais do paciente | Exposição de dados de terceiros | Filtro pelo identificador da sessão, sem exceção | RN-004, RNF-005, RES-006 |
| UC-FUT-028 Consultar próprios agendamentos | Agendamentos de pacientes | Leitura cruzada entre pacientes | Filtro pelo paciente da sessão | RN-004, RNF-005 |
| UC-FUT-031 Consultar histórico permitido | Histórico de sessões profissionais | Acesso a histórico após encerramento do vínculo | Regra de temporalidade do vínculo (INC-04) | RN-003, RN-007 |
| UC-FUT-030 Registrar atendimento | Sessão profissional conceitual | Conteúdo clínico aprofundado, expressamente vedado | Apenas registro conceitual com os quatro elementos exigidos; sem conteúdo clínico | RF-030 obs., DEC-008 |
| UC-FUT-032 Registrar evento de auditoria | Sequência de operações, com responsável | Captura acidental de senha ou de dado sensível no registro | Registro restrito aos quatro elementos de CA-RF-032; nenhum segredo | RN-014, RNF-004 |

## 7. Itens a confirmar com o Integrante 2

1. Confirmação dos nomes e das fronteiras dos 32 casos de uso, hoje provisionalmente `UC-FUT-*`.
2. Definição de quais operações, além das administrativas, são auditáveis (RF-032 obs. e `rastreabilidade-inicial.md`).
3. Efeito persistido de UC-FUT-002 (encerrar sessão): a sessão é uma entidade persistida ou um estado volátil (ver P-07).
4. Existência de fluxos alternativos que envolvam notificação em UC-FUT-016, UC-FUT-026 e UC-FUT-027.
5. Confirmação dos três casos críticos e do caso opcional previstos no plano §5.3: SEQ-001 (autenticar), SEQ-002 (agendar), SEQ-003 (cancelar/confirmar) e SEQ-004 (preencher encaixe). O caso SEQ-004 depende da decisão de inovação (DEC-010).
6. Delimitação do conteúdo conceitual de `atendimento`, necessária para o dicionário de dados da Fase 7.

## 8. Rastreabilidade deste documento

| Item | Origem |
|---|---|
| Matriz de dados por caso de uso | RF-001 a RF-032, `02-requisitos/rastreabilidade-inicial.md` |
| Regras de persistência implícitas | RN-015 a RN-019 (propostas), derivadas de RN-003, RN-005, RN-006, RN-007, RN-009, RN-010, RN-011 |
| Integrações externas | DEC-007, DEC-009, DEC-010, RF-023 obs., `02-requisitos/escopo.md` §5.2 |
| Revisão de dados sensíveis | RNF-001, RNF-004, RNF-005, RNF-006, RN-002, RN-003, RN-004, RN-008, RN-014, RES-002, RES-006 |
| Pendências P-01 a P-12 | `04-persistencia/analises/fase-05-viabilidade-persistencia-dominio.md` §7 |
