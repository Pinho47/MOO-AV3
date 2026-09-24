# Regras de Negócio

As regras formalizam restrições operacionais identificadas no escopo e nas decisões. Regras sem parâmetros definidos estão marcadas como pendentes e não devem ser tratadas como decisão já aprovada.

### RN-001 — Bloqueio de autenticação

**Regra:** Usuários bloqueados não podem autenticar-se.

**Atores/entidades envolvidos:** Usuário.

**Requisitos relacionados:** RF-001, RF-006.

**Justificativa:** Impedir acesso de contas bloqueadas.

### RN-002 — Proteção de senhas

**Regra:** O Administrador não pode visualizar senhas em texto puro; senhas não podem ser armazenadas ou disponibilizadas dessa forma.

**Atores/entidades envolvidos:** Administrador, usuário, credenciais.

**Requisitos relacionados:** RF-003, RF-010, RNF-001.

**Justificativa:** Preservar a confidencialidade das credenciais.

### RN-003 — Acesso profissional condicionado a vínculo

**Regra:** O Psicólogo somente pode acessar clientes/pacientes vinculados a ele por relação autorizada.

**Atores/entidades envolvidos:** Psicólogo, Cliente/Paciente, vínculo.

**Requisitos relacionados:** RF-017, RF-021, RF-031.

**Justificativa:** Restringir o acesso aos dados necessários à atuação autorizada.

### RN-004 — Acesso do paciente ao próprio cadastro

**Regra:** O Cliente/Paciente somente pode consultar informações relacionadas ao próprio cadastro e liberadas para seu acesso.

**Atores/entidades envolvidos:** Cliente/Paciente.

**Requisitos relacionados:** RF-022, RF-028.

**Justificativa:** Preservar a privacidade entre pacientes e limitar o autoatendimento.

### RN-005 — Exclusividade de horário ativo

**Regra:** Um mesmo horário de um Psicólogo não pode possuir dois agendamentos ativos incompatíveis.

**Atores/entidades envolvidos:** Psicólogo, disponibilidade, agendamento.

**Requisitos relacionados:** RF-023, RF-025, RF-026.

**Justificativa:** Evitar conflitos na agenda.

### RN-006 — Agendamento cancelado

**Regra:** Um agendamento cancelado não pode posteriormente ser registrado como atendimento realizado.

**Atores/entidades envolvidos:** Agendamento, atendimento.

**Requisitos relacionados:** RF-027, RF-030.

**Justificativa:** Preservar consistência entre situação do agendamento e registro de atendimento.

### RN-007 — Vínculos obrigatórios do atendimento

**Regra:** Um atendimento registrado deve estar associado a um Psicólogo e a um Cliente/Paciente válidos, com vínculo autorizado.

**Atores/entidades envolvidos:** Psicólogo, Cliente/Paciente, atendimento, vínculo.

**Requisitos relacionados:** RF-021, RF-030, RF-031.

**Justificativa:** Manter contexto e autorização do registro conceitual de sessão.

### RN-008 — Consulta restrita à auditoria

**Regra:** Apenas usuários autorizados podem consultar registros de auditoria; no escopo atual, essa consulta cabe ao Administrador autorizado.

**Atores/entidades envolvidos:** Administrador, usuário, auditoria.

**Requisitos relacionados:** RF-013, RF-032.

**Justificativa:** Preservar a confidencialidade dos registros operacionais.

### RN-009 — Rastreabilidade de alterações relevantes

**Regra:** Alterações relevantes de situação devem preservar rastreabilidade por registro de auditoria.

**Atores/entidades envolvidos:** Usuário, situação cadastral, agendamento, auditoria.

**Requisitos relacionados:** RF-020, RF-027, RF-032.

**Justificativa:** Permitir identificar operações relevantes e seus responsáveis.

### RN-010 — Solicitação em horário disponível

**Regra:** Um Cliente/Paciente somente pode solicitar agendamento em horário que esteja disponível no momento da solicitação.

**Atores/entidades envolvidos:** Cliente/Paciente, Psicólogo, disponibilidade, agendamento.

**Requisitos relacionados:** RF-024, RF-025.

**Justificativa:** Evitar solicitações sobre horários indisponíveis.

### RN-011 — Disponibilidade na própria agenda

**Regra:** Um Psicólogo somente pode disponibilizar horários vinculados à própria agenda.

**Atores/entidades envolvidos:** Psicólogo, disponibilidade, agenda.

**Requisitos relacionados:** RF-023, RF-029.

**Justificativa:** Evitar alteração de disponibilidade pertencente a outro profissional.

### RN-012 — Permissão para cancelamento

**Regra:** O cancelamento deve respeitar as permissões do ator responsável e as condições de cancelamento aprovadas pelo grupo.

**Atores/entidades envolvidos:** Cliente/Paciente, Psicólogo, Administrador, agendamento.

**Requisitos relacionados:** RF-027.

**Justificativa:** Garantir que apenas atores autorizados alterem a situação do agendamento.

**Pendência:** Definir prazo de antecedência, situações canceláveis e se Psicólogo e/ou Administrador podem cancelar.

### RN-013 — Confirmação de agendamento

**Regra:** A confirmação deve ocorrer por ator ou mecanismo autorizado, segundo fluxo aprovado pelo grupo.

**Atores/entidades envolvidos:** Agendamento; ator responsável pendente.

**Requisitos relacionados:** RF-025, RF-026.

**Justificativa:** A Fase 2 prevê confirmação, mas não identifica o responsável nem define confirmação automática.

**Pendência:** Definir ator responsável, estados anteriores/posteriores e efeito sobre disponibilidade.

### RN-014 — Imutabilidade lógica de auditoria

**Regra:** Registros de auditoria devem ser cronológicos, rastreáveis e não devem ser alterados por operações comuns de consulta ou gestão.

**Atores/entidades envolvidos:** Auditoria, usuários autorizados.

**Requisitos relacionados:** RF-013, RF-032.

**Justificativa:** Preservar confiabilidade da trilha operacional, conforme escopo.
