# Matriz de Rastreabilidade Inicial

Esta matriz conecta cada requisito funcional às regras de negócio aplicáveis, ator, caso de uso futuro e critério de aceite. Os nomes `UC-FUT-*` são identificadores de trabalho para o Integrante 2 confirmar ou substituir; não representam casos de uso ou diagramas já elaborados. Onde não há regra específica, usa-se RNF aplicável ou indica-se “—”.

| Requisito | Regra relacionada | Ator | Caso de uso futuro | Critério de aceite | Status |
|---|---|---|---|---|---|
| RF-001 | RN-001, RNF-002 | Administrador, Psicólogo, Cliente/Paciente | UC-FUT-001 — Autenticar usuário | CA-RF-001 | Para revisão |
| RF-002 | RNF-002 | Administrador, Psicólogo, Cliente/Paciente | UC-FUT-002 — Encerrar sessão | CA-RF-002 | Para revisão |
| RF-003 | RN-002 | Administrador | UC-FUT-003 — Cadastrar usuário | CA-RF-003 | Para revisão |
| RF-004 | RNF-002 | Administrador, Psicólogo, Cliente/Paciente conforme permissão | UC-FUT-004 — Alterar dados permitidos | CA-RF-004 | Para revisão |
| RF-005 | RNF-002 | Administrador | UC-FUT-005 — Ativar usuário | CA-RF-005 | Para revisão |
| RF-006 | RN-001 | Administrador | UC-FUT-006 — Bloquear usuário | CA-RF-006 | Para revisão |
| RF-007 | RNF-002 | Administrador | UC-FUT-007 — Gerenciar perfis | CA-RF-007 | Para revisão |
| RF-008 | RNF-002 | Administrador | UC-FUT-008 — Gerenciar permissões | CA-RF-008 | Para revisão |
| RF-009 | RNF-002 | Administrador | UC-FUT-009 — Consultar painel administrativo | CA-RF-009 | Para revisão |
| RF-010 | RN-002 | Administrador | UC-FUT-010 — Consultar usuários | CA-RF-010 | Para revisão |
| RF-011 | — | Administrador | UC-FUT-011 — Consultar psicólogos | CA-RF-011 | Para revisão |
| RF-012 | RNF-002 | Administrador | UC-FUT-012 — Consultar pacientes | CA-RF-012 | Para revisão |
| RF-013 | RN-008, RN-014 | Administrador autorizado | UC-FUT-013 — Consultar auditoria | CA-RF-013 | Para revisão |
| RF-014 | — | Administrador | UC-FUT-014 — Cadastrar psicólogo | CA-RF-014 | Para revisão |
| RF-015 | RNF-002 | Psicólogo | UC-FUT-015 — Atualizar perfil profissional | CA-RF-015 | Para revisão |
| RF-016 | RNF-002 | Psicólogo, Administrador autorizado | UC-FUT-016 — Associar área de atuação | CA-RF-016 | Para revisão |
| RF-017 | RN-003, RNF-006 | Psicólogo | UC-FUT-017 — Consultar pacientes vinculados | CA-RF-017 | Para revisão |
| RF-018 | — | Administrador | UC-FUT-018 — Cadastrar cliente/paciente | CA-RF-018 | Para revisão |
| RF-019 | RNF-002 | Administrador | UC-FUT-019 — Atualizar cadastro de paciente | CA-RF-019 | Para revisão |
| RF-020 | RN-009 | Administrador | UC-FUT-020 — Alterar situação cadastral | CA-RF-020 | Para revisão |
| RF-021 | RN-003, RN-007 | Administrador | UC-FUT-021 — Formalizar vínculo | CA-RF-021 | Para revisão |
| RF-022 | RN-004, RNF-005 | Cliente/Paciente | UC-FUT-022 — Consultar próprio cadastro | CA-RF-022 | Para revisão |
| RF-023 | RN-011 | Psicólogo | UC-FUT-023 — Cadastrar disponibilidade | CA-RF-023 | Para revisão |
| RF-024 | RN-010, RN-004 | Cliente/Paciente | UC-FUT-024 — Consultar horários disponíveis | CA-RF-024 | Para revisão |
| RF-025 | RN-005, RN-010 | Cliente/Paciente | UC-FUT-025 — Solicitar agendamento | CA-RF-025 | Para revisão |
| RF-026 | RN-013 | Ator pendente de decisão | UC-FUT-026 — Confirmar agendamento | CA-RF-026 | Pendente de decisão do grupo |
| RF-027 | RN-006, RN-012 | Cliente/Paciente; demais atores pendentes | UC-FUT-027 — Cancelar agendamento | CA-RF-027 | Pendente de decisão do grupo |
| RF-028 | RN-004, RNF-005 | Cliente/Paciente | UC-FUT-028 — Consultar próprios agendamentos | CA-RF-028 | Para revisão |
| RF-029 | RN-003, RN-011 | Psicólogo | UC-FUT-029 — Consultar agenda própria | CA-RF-029 | Para revisão |
| RF-030 | RN-006, RN-007 | Psicólogo | UC-FUT-030 — Registrar atendimento realizado | CA-RF-030 | Para revisão |
| RF-031 | RN-003, RN-007 | Psicólogo | UC-FUT-031 — Consultar histórico permitido | CA-RF-031 | Para revisão |
| RF-032 | RN-009, RN-014, RNF-004 | Sistema, decorrente de ação dos atores | UC-FUT-032 — Registrar evento de auditoria | CA-RF-032 | Para revisão |

## Pendências para fechar a rastreabilidade

- Integrante 2 deve confirmar nomes, fronteiras e atores dos casos de uso futuros; nenhum diagrama comportamental foi criado nesta fase.
- Grupo deve definir ator e fluxo de confirmação de agendamento (RF-026/RN-013).
- Grupo deve definir prazo, estados e atores autorizados para cancelamento (RF-027/RN-012).
- Grupo deve definir situação inicial da solicitação e efeito na disponibilidade (RF-025).
- Grupo deve selecionar quais eventos não administrativos também serão auditáveis (RF-032/RN-009).
- Inovação e Serviço Externo permanecem sem requisito até decisão formal.
