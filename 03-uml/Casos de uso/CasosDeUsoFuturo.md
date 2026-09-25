Revisão de Casos de Uso Futuros
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Objetivo: confirmar nomes/fronteiras/atores antes da Fase 4, e sinalizar o que precisa de ajuste.

(1)Confirmados sem alteração 
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
A maioria dos UC-FUT está com nome, ator e fronteira coerentes com o que mapeamos nas Fases 1 e 2. Confirmo os seguintes como estão: UC-FUT-001, 002, 003, 005, 007, 008, 010, 011, 012, 013, 014, 015, 017, 018, 019, 020, 022, 023, 024, 025, 026, 027, 028, 029, 030, 031.

(2)Pontos que precisam de ajuste ou decisão antes da Fase 4
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
UC-FUT-004 e UC-FUT-016 - possível redundância

RF-004 ("alterar dados permitidos", genérico) se sobrepõe parcialmente com RF-015 (Psicólogo atualiza próprio perfil) e RF-019 (Administrador atualiza cadastro de paciente). Da mesma forma, RF-016 (associar área de atuação) parece ser só um campo dentro do perfil profissional já coberto por RF-015.

O ideal seria manter RF-004 só para o Administrador alterando dados de identidade (login, e-mail, status da conta), não dados de perfil profissional/cadastro, que já têm RF próprio. E avaliar se RF-016 vira parte do fluxo de UC-FUT-015 em vez de caso de uso isolado.

UC-FUT-006 - falta o par "desbloquear"

RF-005 (Ativar) e RF-006 (Bloquear) existem, mas não achei um RF explícito de desbloquear uma conta já bloqueada, só ativar uma conta nova. Se "ativar" não cobrir reabilitação de conta bloqueada, falta um RF. Vale confirmar com o Integrante 1.

UC-FUT-009 - não deveria ser um caso de uso isolado

"Consultar painel administrativo" não tem um objetivo fechado próprio - é uma tela que agrega outros casos de uso (gerenciar usuários, perfis, auditoria etc.). Recomendo não modelar como UC independente; ele vira só o ponto de entrada visual, sem diagrama de sequência próprio.

UC-FUT-021 - alinhar nome

Sugiro renomear de "Formalizar vínculo" para "Vincular psicólogo a paciente", pra bater com o nome que já usamos desde a Fase 1 (é um dos 4 processos críticos).

UC-FUT-032 - não é caso de uso, é efeito colateral

"Registrar evento de auditoria" não tem ator humano que o inicia - o próprio Integrante 1 já anotou "Sistema, decorrente de ação dos atores". Isso não deveria ser um caso de uso com diagrama próprio, e sim uma relação <<include>> a partir de cada caso de uso auditável (ex: "Bloquear usuário" inclui "Registrar evento de auditoria"). Vou modelar assim no diagrama de casos de uso, salvo objeção do grupo.

Pendência: quem pode cancelar um agendamento?

RF-027 já está marcado como pendente quanto a prazo/estados, mas o texto também deixa em aberto quais atores podem cancelar - hoje só cita Cliente/Paciente confirmado, com "demais atores pendentes". Sugiro registrar isso formalmente como uma nova decisão pra não se perder:

DEC-011 (sugestão) - Além do Paciente, o Psicólogo e/ou o Administrador também podem cancelar um agendamento? Se sim, em quais condições?

(3)Confirmação dos processos críticos (candidatos a diagrama de sequência)
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Batendo a matriz com o que a gente já tinha das Fases 1 e 2, os 3 processos críticos confirmados continuam os mesmos, agora com RF oficial:

SEQ-001 - Autenticar usuário -> RF-001 (+ RF-002 encerrar sessão, RN-001 bloqueio)
SEQ-002 - Vincular psicólogo a paciente → RF-021 (RN-003, RN-007)
SEQ-003 - Agendar -> confirmar/cancelar → RF-025, RF-026 (pendente DEC ator), RF-027 (pendente DEC-011)
