UC-001 — Autenticar usuário
--------------------------------------------------------------------------------------------------------
Atores: Administrador, Psicólogo, Paciente

Pré-condição: Usuário possui conta cadastrada e ativa.

Pós-condição (sucesso): Sessão autenticada é criada, com acesso restrito às funções compatíveis com o perfil do usuário.

Fluxo principal:

Usuário informa credenciais (e-mail e senha).
Sistema valida se a conta existe e está ativa.
Sistema valida se a senha confere.
Sistema cria a sessão autenticada, vinculada ao perfil e permissões do usuário.

Fluxos alternativos:

A1 (RF-002): usuário solicita encerramento de sessão a qualquer momento — sistema invalida a sessão ativa.

Exceções:

E1: credenciais inválidas — sistema recusa o acesso e não cria sessão (RF-001).
E2: conta bloqueada — sistema recusa o acesso mesmo com credenciais corretas (RN-001).

Regras de negócio aplicadas: RN-001 (bloqueio de autenticação), RN-002 (proteção de senhas — nunca exibidas em texto puro).

UC-021 — Vincular psicólogo a paciente
--------------------------------------------------------------------------------------------------------
Ator: Administrador

Pré-condição: Psicólogo e Paciente já cadastrados no sistema.

Pós-condição (sucesso): Vínculo registrado; Psicólogo passa a ter acesso autorizado aos dados do Paciente.

Fluxo principal:

Administrador seleciona um Psicólogo cadastrado.
Administrador seleciona um Paciente cadastrado.
Sistema formaliza o vínculo entre os dois.
Sistema inclui o evento na trilha de auditoria (<<include>> UC "Registrar evento de auditoria").

Fluxos alternativos: nenhum identificado até o momento.

Exceções:

E1: psicólogo ou paciente não encontrado/inválido — sistema recusa a operação.

Regras de negócio aplicadas: RN-003 (acesso profissional condicionado a vínculo), RN-007 (vínculos obrigatórios do atendimento).

Pendência aberta: DUV-003 — o que acontece com agendamentos futuros se esse vínculo for desfeito depois. Ainda sem regra definida.

UC-025 — Solicitar agendamento (com confirmação automática)
--------------------------------------------------------------------------------------------------------
Ator: Paciente

Pré-condição: Paciente autenticado e vinculado a um Psicólogo; Psicólogo já cadastrou disponibilidade de horários (UC-023).

Pós-condição (sucesso): Agendamento criado e automaticamente confirmado; notificação de confirmação enviada por e-mail.

Fluxo principal:

Paciente consulta horários disponíveis do Psicólogo vinculado (UC-024).
Paciente seleciona um horário disponível e solicita o agendamento.
Sistema valida que o horário ainda está disponível (RN-005, RN-010).
Sistema cria o agendamento com situação confirmada automaticamente — não há etapa de aprovação manual por outro ator, já que o horário só está visível por já ter sido liberado pelo Psicólogo.
Sistema inclui o envio de notificação por e-mail ao Paciente (<<include>> UC "Notificar paciente por e-mail" — DEC-009).

Fluxos alternativos:

A1: nenhum horário disponível para o Psicólogo vinculado — sistema informa e não permite prosseguir.

Exceções:

E1 — conflito de horário: paciente tenta solicitar um horário que deixou de estar disponível entre a consulta (passo 1) e a solicitação (passo 2). Pendente (DEC-4): o sistema deve recusar em silêncio ou sugerir outro horário? Fluxo de exceção a ser fechado antes do diagrama de sequência.

Regras de negócio aplicadas: RN-005 (exclusividade de horário ativo), RN-010 (solicitação em horário disponível).

UC-027 — Cancelar agendamento
--------------------------------------------------------------------------------------------------------
Atores: Paciente, Psicólogo (DEC-011)

Pré-condição: Agendamento existente, em situação cancelável.

Pós-condição (sucesso): Situação do agendamento alterada para cancelado; agendamento cancelado não pode depois ser registrado como atendimento realizado (RN-006).

Fluxo principal:

Ator (Paciente ou Psicólogo) seleciona um agendamento próprio/vinculado.
Ator solicita o cancelamento.
Sistema valida se o agendamento está em situação cancelável.
Sistema altera a situação para cancelado.
Sistema registra a operação na auditoria (RN-009).

Justificativa do Psicólogo como ator (registrada pelo grupo): o psicólogo precisa poder cancelar para os casos em que algo impeça o atendimento da parte dele.

Fluxos alternativos: nenhum identificado até o momento.

Exceções:

E1: agendamento já realizado ou já cancelado — sistema recusa novo cancelamento.

Regras de negócio aplicadas: RN-006 (agendamento cancelado), RN-012 (permissão para cancelamento).

Pendências abertas:

DEC-012 (sugestão de numeração): prazo de antecedência mínimo para cancelar — ainda sem definição do grupo.
DEC-5: "não compareceu" é manual ou automático — afeta os estados possíveis de onde um cancelamento pode partir
