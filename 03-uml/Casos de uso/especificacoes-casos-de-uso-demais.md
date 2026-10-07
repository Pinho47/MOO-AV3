Gerenciar usuários e permissões
-
Ator: Administrador

RFs cobertos: RF-003, RF-004, RF-005, RF-006, RF-007, RF-008, RF-009, RF-010

Pré-condição: Administrador autenticado.

Pós-condição (sucesso): Usuário, perfil ou permissão criado/alterado conforme a operação solicitada.

Fluxo principal:

Administrador acessa a área de gestão de usuários.
Administrador escolhe a operação: cadastrar, consultar, alterar dados, ativar, bloquear usuário, ou gerenciar perfis/permissões.
Sistema executa a operação e confirma o resultado.
Sistema inclui o evento na auditoria (<<include>> "Registrar evento de auditoria").

Fluxos alternativos: nenhum identificado até o momento.

Exceções:

E1: dados inválidos no cadastro — sistema recusa e informa o erro.
E2: tentativa de ampliar permissões além do que o próprio Administrador possui — sistema recusa (RN-002, RNF-002).

Regras de negócio aplicadas: RN-002 (proteção de senhas — nunca em texto puro), RNF-002 (autorização por perfil e permissão).

Gerenciar psicólogos e pacientes
-
Ator: Administrador

RFs cobertos: RF-011, RF-012, RF-014, RF-018, RF-019, RF-020

Pré-condição: Administrador autenticado.

Pós-condição (sucesso): Cadastro de psicólogo ou paciente criado, consultado ou atualizado.

Fluxo principal:

Administrador escolhe gerenciar Psicólogo ou Cliente/Paciente.
Administrador cadastra, consulta, atualiza dados ou altera a situação cadastral.
Sistema executa e confirma a operação.
Sistema inclui o evento na auditoria quando a operação altera dados (<<include>> "Registrar evento de auditoria").

Fluxos alternativos: nenhum identificado até o momento.

Exceções:

E1: dados inválidos — sistema recusa e informa o erro.

Regras de negócio aplicadas: RN-009 (rastreabilidade de alterações relevantes).

Observação: por não usar dados reais de pacientes (restrição do enunciado), os cadastros de teste devem usar sempre informação fictícia.

Consultar auditoria
-
Ator: Administrador

RFs cobertos: RF-013

Pré-condição: Administrador autenticado e com permissão de auditoria.

Pós-condição (sucesso): Registros de auditoria exibidos, sem alteração pela própria consulta.

Fluxo principal:

Administrador acessa a área de auditoria.
Administrador aplica filtro (ex: por usuário, por período, por tipo de operação).
Sistema apresenta os registros autorizados, em ordem cronológica.

Fluxos alternativos: nenhum identificado até o momento.

Exceções:

E1: Administrador sem permissão de auditoria — sistema recusa o acesso (RN-008).

Regras de negócio aplicadas: RN-008 (consulta restrita à auditoria), RN-014 (imutabilidade lógica de auditoria).

Gerenciar perfil e agenda
-
Ator: Psicólogo

RFs cobertos: RF-015, RF-016, RF-023, RF-029

Pré-condição: Psicólogo autenticado.

Pós-condição (sucesso): Perfil profissional, área de atuação ou disponibilidade de agenda atualizados.

Fluxo principal:

Psicólogo acessa a área do próprio perfil ou agenda.
Psicólogo atualiza dados profissionais, associa área de atuação, ou cadastra período de disponibilidade.
Sistema valida que a disponibilidade pertence à própria agenda do Psicólogo (RN-011) e não conflita com horário já ocupado (RN-005).
Sistema confirma a atualização.

Fluxos alternativos:

A1: Psicólogo consulta sua agenda consolidada (RF-029), sem alterar nada.

Exceções:

E1: tentativa de disponibilizar horário já comprometido — sistema recusa (RN-005).

Regras de negócio aplicadas: RN-005 (exclusividade de horário ativo), RN-011 (disponibilidade na própria agenda).

Atender pacientes vinculados
-
Ator: Psicólogo

RFs cobertos: RF-017, RF-030, RF-031

Pré-condição: Psicólogo autenticado; vínculo autorizado com o(s) paciente(s) consultado(s) (RN-003).

Pós-condição (sucesso): Consulta de pacientes vinculados exibida, ou atendimento conceitual registrado, ou histórico permitido consultado.

Fluxo principal:

Psicólogo consulta a lista de pacientes vinculados a ele.
Psicólogo seleciona um paciente e registra o atendimento realizado (sem dados clínicos reais — DEC-008), ou consulta o histórico de atendimentos permitido.
Sistema valida o vínculo autorizado antes de qualquer exibição ou registro.
Sistema confirma a operação.

Fluxos alternativos: nenhum identificado até o momento.

Exceções:

E1: Psicólogo tenta acessar paciente sem vínculo — sistema recusa (RN-003).

Regras de negócio aplicadas: RN-003 (acesso profissional condicionado a vínculo), RN-007 (vínculos obrigatórios do atendimento).

Consultar meu cadastro
-
Ator: Paciente

RFs cobertos: RF-022

Pré-condição: Paciente autenticado.

Pós-condição (sucesso): Dados do próprio cadastro exibidos, e somente eles.

Fluxo principal:

Paciente acessa a área "meu cadastro".
Sistema exibe somente as informações liberadas associadas ao próprio Paciente.

Fluxos alternativos: nenhum identificado até o momento.

Exceções: nenhuma identificada além da autenticação (UC-001).

Regras de negócio aplicadas: RN-004 (acesso do paciente ao próprio cadastro), RNF-005 (isolamento dos dados de pacientes).

Registrar evento de auditoria — <<include>>, sem ator próprio

RFs cobertos: RF-032

Observação: não é um caso de uso iniciado por ator — é incluído automaticamente por outros casos de uso quando uma operação relevante ocorre (hoje: "Gerenciar usuários e permissões" e "Vincular psicólogo a paciente"; pode ampliar conforme o grupo definir outros eventos auditáveis — ver DUV-006).

Fluxo: ao ser incluído, o sistema registra usuário responsável, data, hora e operação, em trilha cronológica, sem incluir senha em texto puro.

Regras de negócio aplicadas: RN-009 (rastreabilidade de alterações relevantes), RN-014 (imutabilidade lógica de auditoria).

Notificar paciente por e-mail — <<include>>, sem ator próprio

RFs cobertos: nenhum RF oficial ainda — decorre do DEC-009 (resolvido: sim, para confirmação de agendamento).

Observação: incluído automaticamente pelo caso de uso "Agendar consulta" quando um agendamento é criado e confirmado.

Fluxo: ao ser incluído, o sistema aciona o serviço externo de e-mail, que entrega a mensagem de confirmação ao endereço cadastrado do Paciente.

Pendência: ainda não formalizado como RF numerado — recomendo que o Integrante 1 numere assim que o grupo confirmar o alcance (só confirmação, ou também cancelamento/lembrete).
