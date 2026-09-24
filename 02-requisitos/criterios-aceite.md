# Critérios de Aceite

Os critérios usam condições observáveis nos fluxos e artefatos de modelagem. Os comportamentos dependentes de decisão do grupo permanecem condicionados à resolução das pendências descritas nos próprios requisitos.

### CA-RF-001 — Autenticação válida

**Dado** um usuário cadastrado, ativo e com credenciais válidas,  
**quando** solicitar autenticação,  
**então** o sistema deverá permitir acesso somente às funções compatíveis com seu perfil e permissões.

### CA-RF-002 — Encerramento de sessão

**Dado** um usuário com sessão autenticada,  
**quando** solicitar encerramento,  
**então** a sessão deverá deixar de autorizar acesso às funções restritas.

### CA-RF-003 — Cadastro de usuário

**Dado** um Administrador autenticado e autorizado,  
**quando** cadastrar um usuário e associar perfil permitido,  
**então** o usuário deverá ser registrado com identificação própria sem exposição de senha em texto puro.

### CA-RF-004 — Alteração de dados de usuário

**Dado** um solicitante autenticado e autorizado para determinado conjunto de dados,  
**quando** alterar esses dados,  
**então** somente os campos permitidos deverão ser atualizados.

### CA-RF-005 — Ativação de usuário

**Dado** um usuário cadastrado e um Administrador autorizado,  
**quando** o Administrador ativar a conta,  
**então** a conta deverá ficar habilitada para autenticação, sujeita às permissões atribuídas.

### CA-RF-006 — Bloqueio de usuário

**Dado** um usuário cadastrado e um Administrador autorizado,  
**quando** o Administrador bloquear a conta,  
**então** o usuário não deverá conseguir iniciar autenticação.

### CA-RF-007 — Gerenciamento de perfis

**Dado** um Administrador autorizado,  
**quando** consultar ou manter um perfil de acesso,  
**então** a operação deverá respeitar as permissões administrativas e disponibilizar o perfil para associação autorizada.

### CA-RF-008 — Gerenciamento de permissões

**Dado** um perfil existente e um Administrador autorizado,  
**quando** atualizar as permissões do perfil,  
**então** autorizações subsequentes deverão considerar as permissões atualizadas sem ampliar os limites dos atores.

### CA-RF-009 — Painel administrativo

**Dado** um Administrador autenticado,  
**quando** acessar o painel administrativo,  
**então** deverá visualizar somente funções administrativas compatíveis com suas permissões.

### CA-RF-010 — Consulta de usuários

**Dado** um Administrador autorizado,  
**quando** consultar usuários cadastrados,  
**então** os dados cadastrais permitidos deverão ser apresentados sem revelar senhas em texto puro.

### CA-RF-011 — Consulta de psicólogos

**Dado** um Administrador autorizado,  
**quando** consultar psicólogos cadastrados,  
**então** o sistema deverá apresentar os dados profissionais permitidos.

### CA-RF-012 — Consulta de pacientes

**Dado** um Administrador autorizado,  
**quando** consultar clientes/pacientes cadastrados,  
**então** o sistema deverá apresentar dados cadastrais conforme suas permissões, sem conceder acesso a prontuário completo.

### CA-RF-013 — Consulta de auditoria

**Dado** um Administrador com permissão de auditoria,  
**quando** consultar registros,  
**então** os registros autorizados deverão ser apresentados sem alteração pela operação de consulta.

### CA-RF-014 — Cadastro de psicólogo

**Dado** um Administrador autorizado,  
**quando** cadastrar um Psicólogo,  
**então** deverá ser criado o registro profissional correspondente sem duplicar desnecessariamente a identidade de usuário.

### CA-RF-015 — Atualização de perfil profissional

**Dado** um Psicólogo autenticado com perfil profissional,  
**quando** atualizar dados profissionais liberados,  
**então** somente os dados de seu próprio perfil autorizados deverão ser alterados.

### CA-RF-016 — Associação de área de atuação

**Dado** um Psicólogo e uma área de atuação válidos,  
**quando** o Psicólogo ou Administrador autorizado efetuar a associação,  
**então** a área deverá constar no perfil profissional correspondente.

### CA-RF-017 — Consulta de pacientes vinculados

**Dado** um Psicólogo autenticado com vínculos autorizados,  
**quando** consultar clientes/pacientes,  
**então** o resultado deverá conter somente pacientes vinculados a esse Psicólogo.

### CA-RF-018 — Cadastro de cliente/paciente

**Dado** um Administrador autorizado,  
**quando** cadastrar um cliente/paciente,  
**então** o cadastro deverá ser registrado sem uso de dados reais nos artefatos do projeto.

### CA-RF-019 — Atualização administrativa de paciente

**Dado** um cadastro de paciente existente e um Administrador autorizado,  
**quando** atualizar dados cadastrais administrativos,  
**então** somente dados administrativos permitidos deverão ser alterados.

### CA-RF-020 — Alteração de situação cadastral

**Dado** um cliente/paciente cadastrado e um Administrador autorizado,  
**quando** alterar sua situação,  
**então** a nova situação deverá ser registrada e a operação relevante deverá ser rastreável.

### CA-RF-021 — Vínculo entre paciente e psicólogo

**Dado** um Psicólogo e um Cliente/Paciente cadastrados,  
**quando** um Administrador autorizado formalizar o vínculo,  
**então** o vínculo deverá ser registrado e considerado nas autorizações profissionais.

### CA-RF-022 — Consulta do próprio cadastro

**Dado** um Cliente/Paciente autenticado,  
**quando** consultar seu cadastro,  
**então** deverá receber somente informações próprias liberadas para visualização.

### CA-RF-023 — Cadastro de disponibilidade

**Dado** um Psicólogo autenticado,  
**quando** cadastrar disponibilidade,  
**então** o período deverá ser associado à própria agenda e considerado junto a conflitos existentes.

### CA-RF-024 — Consulta de horários disponíveis

**Dado** um Cliente/Paciente autenticado e vinculado ao Psicólogo,  
**quando** consultar horários,  
**então** serão exibidos horários disponíveis para solicitação e a consulta não deverá reservar nenhum deles.

### CA-RF-025 — Solicitação de agendamento

**Dado** um Cliente/Paciente autenticado, um Psicólogo vinculado e um horário disponível,  
**quando** solicitar o agendamento,  
**então** a solicitação deverá associar paciente, Psicólogo e horário, sem aceitar horário indisponível.

### CA-RF-026 — Confirmação de agendamento

**Dado** uma solicitação passível de confirmação e o fluxo de confirmação aprovado pelo grupo,  
**quando** o ator autorizado confirmar,  
**então** a situação deverá passar a confirmada sem criar conflito de horário.

**Pendência:** O ator e o fluxo ainda precisam de decisão do grupo.

### CA-RF-027 — Cancelamento de agendamento

**Dado** um agendamento passível de cancelamento e um ator autorizado segundo regras aprovadas,  
**quando** solicitar o cancelamento,  
**então** a situação deverá ser alterada para cancelado e o agendamento não poderá ser registrado como atendimento realizado.

**Pendência:** Antecedência, estados canceláveis e atores adicionais ainda precisam de decisão do grupo.

### CA-RF-028 — Consulta dos próprios agendamentos

**Dado** um Cliente/Paciente autenticado,  
**quando** consultar seus agendamentos,  
**então** deverá visualizar somente os agendamentos associados ao próprio cadastro e suas situações.

### CA-RF-029 — Consulta da agenda do psicólogo

**Dado** um Psicólogo autenticado,  
**quando** consultar sua agenda,  
**então** deverá visualizar somente compromissos da própria agenda e informações compatíveis com seus vínculos autorizados.

### CA-RF-030 — Registro conceitual de atendimento

**Dado** um Psicólogo autenticado, vínculo autorizado e sessão válida,  
**quando** registrar a realização do atendimento,  
**então** o registro deverá associar Psicólogo e Cliente/Paciente válidos e não deverá representar prontuário completo.

### CA-RF-031 — Consulta de histórico permitido

**Dado** um Psicólogo autenticado com vínculo autorizado,  
**quando** consultar histórico de atendimentos,  
**então** deverá visualizar apenas registros permitidos relacionados ao vínculo.

### CA-RF-032 — Registro de evento de auditoria

**Dado** uma operação classificada como relevante para auditoria,  
**quando** ela ocorrer,  
**então** deverá ser registrado usuário responsável, data, hora e operação em trilha cronológica e rastreável, sem incluir senhas em texto puro.
