
.ATORES
Confirmados:
-----------------------------------------------------------------------------------------------------------------
Administrador — gerencia usuários, perfis, permissões, profissionais, pacientes e auditoria (módulo Administração).

Psicólogo — gerencia perfil profissional, área de atuação, agenda e atendimento aos pacientes vinculados (módulo Psicólogos + lado profissional de Agenda).

Opcionais — ainda em aberto:
-----------------------------------------------------------------------------------------------------------------
Paciente — vai ter login próprio (ator) ou fica só como cadastro administrativo (entidade de domínio)? Ver DEC-1.

Atendente — entra como ator separado com permissão própria, ou o Administrador acumula essa função no MVP? Ver DEC-2.

Serviço externo — só entra se o grupo decidir notificar por e-mail de verdade; senão a notificação fica interna ao sistema. Ver DEC-3.

.PROCESSOS CRÍTICOS
-----------------------------------------------------------------------------------------------------------------
Processos que cruzam mais de um módulo e/ou têm mais de um desfecho possível:

Autenticar e autorizar acesso — cruza Identidade/acesso com todos os outros módulos; é pré-condição de tudo.

Vincular psicólogo a paciente — cruza Administração, Psicólogos e Pacientes; define quem pode ver quem.

Agendar → confirmar/cancelar — cruza Agenda, Psicólogo, Paciente (e Lista de espera, se a inovação entrar); é o processo com mais estados.

Encaixe via lista de espera (inovação) — depende do processo 3 já estar fechado, pois reage a um cancelamento.

.DECISÕES QUE TRAVAM O INICIO DOS CASOS DE USO
-----------------------------------------------------------------------------------------------------------------
DEC-1 — Paciente com login próprio? Se sim: +1 ator no diagrama de contexto, novos casos de uso ("paciente solicita/cancela agendamento", "paciente visualiza histórico"), precisa de perfil/permissão próprio. Se não: sistema mais simples, só 2 perfis de acesso; alguém sempre agenda pelo paciente.

DEC-2 — Atendente como ator separado? Se sim: +1 faixa de permissão em Perfil/Permissao, distinta de Administrador. Se não: Administrador acumula cadastro de paciente — risco de virar gargalo.

DEC-3 — Notificação por serviço externo (e-mail) de verdade? Se sim: precisa aparecer no diagrama de componentes/implantação como integração externa. Se não: notificação fica só como classe interna (Notificação), sem diagrama de implantação extra.

DEC-4 — Conflito de horário: recusa em silêncio ou sugere outro horário? Muda o fluxo alternativo do caso de uso de agendamento (SEQ-002). Decisão obrigatória, não tem opção "de fora".

DEC-5 — "Não compareceu": manual (psicólogo marca) ou automático (sistema muda o estado sozinho)? Muda a máquina de estados de Agendamento (EST-001). Decisão obrigatória, não tem opção "de fora".

.DÚVIDAS
-----------------------------------------------------------------------------------------------------------------
DUV-003 — O que acontece com agendamentos futuros já marcados se o vínculo Psicólogo-Paciente for desfeito depois?

DUV-005 — Na lista de espera, o candidato tem prazo para aceitar o encaixe antes de passar pro próximo da fila?

DUV-006 — Auditoria registra só ações administrativas críticas, ou também login/logout de psicólogo e paciente? RNF-003 fala em "operações administrativas críticas", mas não define a fronteira.

DUV-007 — Se Atendente ficar de fora (DEC-2 = não), quem faz o cadastro administrativo de paciente no dia a dia?

