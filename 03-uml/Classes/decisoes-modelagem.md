# Decisões de modelagem OO

**Responsável:** Integrante 3.  
**Status:** decisões de representação OO para revisão; não substituem DEC oficiais nem aprovam pendências de requisitos.

Referências: [especificação de classes](especificacao-classes.md), [diagrama de classes](diagrama-classes.mmd), [modelo de domínio](../Dominio/modelo-dominio.md), [RF](../../02-requisitos/requisitos-funcionais.md), [RN](../../02-requisitos/regras-negocio.md), [RNF](../../02-requisitos/requisitos-nao-funcionais.md), [glossário](../../02-requisitos/glossario.md), [decisões oficiais](../../01-planejamento/decisoes.md) e [escopo](../../02-requisitos/escopo.md).

## 1. Decisões tomadas nesta etapa

| ID local | Decisão e justificativa OO | Base e limite |
|---|---|---|
| OO-01 | Preservar Usuario, Psicologo e Paciente como classes distintas associadas. Conta de acesso e cadastros têm responsabilidades diferentes. | RF-014 distingue conta/cadastro; RF-018, DEC-006. Não há generalização automática. |
| OO-02 | Administrador é papel/perfil, sem classe independente. Perfil significa acesso, não perfil profissional. | Glossário; RF-007–008, RF-015. Nenhum cadastro próprio de Administrador foi exigido. |
| OO-03 | Preservar as 16 associações do domínio sem alterar cardinalidades. Ausência conservadora de herança, composição e agregação, pois as fontes atuais não sustentam suficientemente hierarquia de substituição ou propriedade forte de ciclo de vida. | Domínio A01–A16; RNF-007–008. Não é proibição definitiva dessas relações; nenhuma exclusão em cascata foi presumida. |
| OO-04 | Declarar atributos privados e operações de intenção explícita; não gerar getters/setters. Associação também é propriedade encapsulada. | RNF-008; RN-003–007, RN-014. Encapsulamento estrutural não substitui autorização. |
| OO-05 | Separar situação da conta, situação cadastral e situação do agendamento. Não fechar enumerações. | RF-005–006, RF-020, RF-025–027. Ativo/bloqueado e confirmado/cancelado não provam conjuntos completos de estados. |
| OO-06 | Usar apenas tipos justificados: Data/Hora conceituais, Booleano para verificações e referências às classes existentes. Omitir tipo não especificado e declarar sua pendência. | RF-032; RF-024–025; RN-003/RN-007. Sem UUID, Inteiro, Texto ou datas técnicas inventados. |
| OO-07 | Métodos públicos OO continuam sujeitos a autenticação e autorização. Quando ator é conhecido, solicitante: Usuario explicita o contexto a validar. | RF e RNF-002. Instância passada por argumento não é prova de autorização; origem confiável pendente PC-12. |
| OO-08 | adicionarPermissao/removerPermissao são refinamentos OO derivados de manter/atualizar associações, não operações literais isoladas em RF-008. removerPermissao é proposta de possível revogação Perfil–Permissao, nunca exclusão da Permissao. | RF-008 e CA-RF-008. Natureza, fluxo e limites devem ser validados com Integrantes 1 e 2; PC-03/PC-09/PC-12 preservadas. |
| OO-09 | Paciente.alterarSituacao tem contrato condicionado e exige transição autorizada e auditoria. Não há setter irrestrito. | RF-020; RN-009. Sem estados aprovados, não há implementação completa desse contrato. |
| OO-10 | formalizar expressa RF-021; estaAutorizado é apenas proposta de refinamento derivado, não função booleana definida nos requisitos. Não se presume ATIVO nem encerramento. | RN-003, RN-007 e RF-021. Representação e critério completo de autorização dependem de definição/validação, PC-06/PC-12. |
| OO-11 | adicionarDisponibilidade representa parte do cadastro de período de RF-023; criação da instância, validação completa e representação temporal continuam abertas. verificarDisponibilidade é refinamento derivado de conflito/disponibilidade e não substitui a consulta de horários de RF-024. | RF-023–026; RN-005/RN-010; CA-RF-024. Contratos preveem revalidação sem reserva pela consulta, mas mecanismos não foram demonstrados; PC-07/PC-12. |
| OO-12 | Agendamento controla confirmar/cancelar; Atendimento registra sessão válida como objeto distinto. Contratos conhecidos não aprovam parâmetros ausentes. | RF-025–030; RN-006/RN-007/RN-012/RN-013. Sem confirmação automática ou limite 1:1 inventado. |
| OO-13 | formalizar, solicitar e os dois registrar têm objetivo funcional sustentado. Representá-los como operações estáticas de criação com retorno é escolha OO proposta, não exigência literal dos RFs. | RF-021, RF-025, RF-030, RF-032. Validar colaboração com Integrante 2 (PC-12); não define cardinalidade histórica ou idempotência. |
| OO-14 | Exigência documental: imutabilidade da auditoria. Concretização OO: campos e responsável imutáveis após criação e ausência de métodos comuns de edição/exclusão/setters. | Escopo 3.7, RF-032, RN-014. Não define retenção, descarte ou administração extraordinária. Cronologia da trilha ainda exige colaboração não detalhada. |
| OO-15 | Registrar rastreabilidade como pós-condição obrigatória dos casos auditáveis; não criar dependência estrutural de toda classe para RegistroAuditoria. | RN-009, RF-032. Integração das mudanças com auditoria será detalhada nas sequências; não se presume gravação em banco pela entidade. |
| OO-16 | Não atribuir automaticamente operações de sistema (painel, login, sessão, busca global) a uma entidade ou criar classes técnicas para preencher cobertura. | RF-001–002, RF-009–013; RNF-008. Registrar colaboração ainda necessária com Integrante 2. |

### Precisão da classificação

As 22 operações preservadas são classificadas individualmente na especificação: 18 objetivos diretamente sustentados e quatro refinamentos OO derivados (adicionarPermissao, removerPermissao, estaAutorizado e verificarDisponibilidade). Contrato condicionado é uma classificação adicional: a existência do objetivo não fecha assinatura, regra, tipo ou colaboração.

Usuario.associarPerfil tem suporte direto em RF-003 durante o cadastro. RF-007 apenas reforça a disponibilidade de perfis para associação; não detalha fluxo completo de alteração posterior. Manutenção posterior permanece condicionada à definição/validação em PC-09/PC-12.

Assinaturas incompletas ou provisórias, incluindo confirmar e cancelar, não são decisões definitivas. Os valores 1 nas associações refletem interpretação semântica documentada no domínio. A08 e A09 merecem validação explícita do grupo, sem alteração nesta etapa. PENDENTE é anotação de modelagem, não multiplicidade UML válida.

A ausência de herança, composição e agregação vale para a evidência atual. Administrador continua papel; Usuario, Psicologo e Paciente mantêm responsabilidades separadas. Nada disso autoriza subclasses apenas por permissões.

## 2. Hipóteses evitadas

- Não foram criados nome, CPF, telefone, e-mail, login, identificadores técnicos ou descrições em todas as classes por costume.
- O CRP aparece como processo candidato nas interações do Integrante 2; não foram definidos formato, validação ou obrigatoriedade sem confirmação dos campos profissionais.
- Não se presume que cada conta pertença obrigatoriamente a um único cadastro, nem que uma conta possa acumular quaisquer papéis.
- Não se define catálogo de áreas, remoção de área, desativação de profissional ou exclusão de registros.
- Não se criam atributos de coleção para transformar PENDENTE em 0..* implicitamente.
- Não se adicionam datas de início/fim de vínculo ou estados ATIVO/ENCERRADO sem regras.
- Não se equipara ativar a desbloquear automaticamente.
- Não se modelam falta, expiração, remarcação, duração fixa ou confirmação automática.
- Não se aceita um booleano enviado pelo cliente como prova de vínculo autorizado, sessão válida ou permissão.
- Não se presume que todo agendamento gere exatamente um atendimento, ou que uma operação repetida de registro seja idempotente.
- Não se fecha o conteúdo clínico do atendimento ou se expõem registros profissionais ao paciente.
- Não se incluem Pagamento, ProntuarioCompleto, ListaEspera, integração de e-mail ou outras funcionalidades não aprovadas (DEC-008–010).
- Não se acrescentam repositórios, controladores, serviços, mecanismos criptográficos ou modelo relacional nesta etapa.

## 3. Pendências de atributos, tipos e comportamento

Os códigos PC são locais. “Integrante 1” indica validação de requisito/regra; “Integrante 2” indica alinhamento com fluxos, mensagens e estados. Não se pede a esses integrantes aprovar uma funcionalidade excluída implicitamente.

| ID | Pendente | Base | Validação necessária |
|---|---|---|---|
| PC-01 | Tipo/formato de identificacao, representação da credencial protegida, campos de atualização da conta e tipo de dadosPermitidos. Não pressupor login/e-mail ou hash específico. | RF-003–004; RN-002; RNF-001 | Integrante 1 define informação permitida; Integrante 2 confirma fluxo. |
| PC-02 | Valores de situacaoConta; ativar como desbloqueio ou não; efeito de bloqueio em sessões abertas. | RF-005–006; revisão de casos futuros | Integrantes 1 e 2. |
| PC-03 | Atributos de Perfil, Permissao e AreaAtuacao; representação da autorização; catálogo; validação dos refinamentos adicionarPermissao e removerPermissao, incluindo possível revogação da associação sem exclusão de Permissao. | RF-007–008, RF-016; glossário | Integrante 1 valida limites; Integrante 2 valida manutenção e fluxos derivados. |
| PC-04 | Campos profissionais liberados, detalhamento do CRP citado como candidato, tipo de dadosPermitidos e projeção da consulta de pacientes. | RF-015–017; interações/processos | Integrantes 1 e 2. |
| PC-05 | Campos administrativos do paciente, tipo/valores de situacaoCadastral e novaSituacao, transições e informações liberadas ao próprio paciente. | RF-019–022; RN-004/RN-009 | Integrantes 1 e 2. Sem isso alterarSituacao é contrato condicionado. |
| PC-06 | Representação e critério completo de autorização de vínculo; validar estaAutorizado como proposta derivada, não função booleana especificada. Duplicidade/simultaneidade, encerramento, agenda futura e histórico continuam pendentes. | RF-021, RF-031; RN-003/RN-007; PD-03 do domínio | Integrantes 1 e 2. encerrar/estaAtivo não incluídos. |
| PC-07 | Tipos de periodo/horario; recorrência, fuso, duração e sobreposição; estados ativos; situação inicial/bloqueio do horário; confirmação (ator/mecanismo); cancelamento (atores, prazo, estados). | RF-023–027; RN-005, RN-010, RN-012–013 | Integrante 1 define regras; Integrante 2 alinha estados e mensagens, inclusive assinatura de confirmar. |
| PC-08 | Conteúdo mínimo do atendimento, tipo/estrutura de contextoSessao, dados temporais próprios, histórico permitido e tratamento de registro repetido. | RF-030–031; RN-006–007 | Integrantes 1 e 2. Não inclui prontuário aprofundado. |
| PC-09 | Cardinalidades pendentes A01–A16; validação explícita das interpretações 1 em A08/A09; alcance da associação/manutenção posterior de perfis. Nenhum extremo do domínio foi alterado. | Domínio; RF relacionados na especificação | Integrante 1 confirma regra; Integrante 2 verifica cenários; Integrante 3 refina após validação. |
| PC-10 | Tipo/vocabulário de operacao da auditoria, precisão/fuso de Data/Hora, ordenação de eventos simultâneos e eventos não administrativos auditáveis. | RF-032; RN-009/RN-014; RNF-004 | Integrante 1 define escopo; Integrante 2 modela registro decorrente da ação. |
| PC-11 | Inovação e serviço externo ainda sem aprovação. Ausentes do diagrama. | DEC-009–010 | Grupo, com formalização pelo Integrante 1 antes de modelar. |
| PC-12 | Retornos/erros, consultas e assinaturas provisórias; identidade confiável, autorização e auditoria; criação de cadastros e de Disponibilidade, validação desta e colaboração dos refinamentos; manutenção posterior de perfil; validação da escolha de operações estáticas. Cobertura individual dos RF-001–032 na especificação. | RF correspondentes; RNF-002, RNF-007–008; RN-003–009, RN-014 | Integrante 2 detalha colaborações; Integrante 1 valida ambiguidades ou mudanças de regra. Nenhum mecanismo é considerado já demonstrado. |

Os tipos omitidos no Mermaid correspondem às pendências acima, não a tipos provisórios aprovados. Data e Hora representam exatamente as informações documentadas para auditoria; não estabelecem formato ou precisão. Booleano representa resultado de uma pergunta, não um atributo adicional de estado.

## 4. Cardinalidades pendentes

| Associações | O que não está definido |
|---|---|
| A01 | Perfis por usuário e usuários por perfil. |
| A02 | Permissões por perfil e perfis por permissão. |
| A03–A04 | Contas por cadastro e cadastros por conta. |
| A05 | Multiplicidade global de áreas por profissional e profissionais por área; RF-016 admite uma ou mais áreas, mas ciclo inicial/limites não estão fechados. |
| A06–A07 | Vínculos por profissional e por paciente. Cada vínculo mantém 1 profissional e 1 paciente. |
| A08 | Agendas por psicólogo pendentes; 1 titular por agenda é interpretação herdada que merece validação explícita. |
| A09 | Períodos por agenda pendentes; 1 agenda por disponibilidade é interpretação herdada que merece validação explícita. |
| A10 | Compromissos por agenda e agendas por agendamento. |
| A11–A12 | Agendamentos por profissional/paciente; cada agendamento mantém 1 de cada participante. |
| A13–A14 | Atendimentos por profissional/paciente; cada atendimento mantém 1 de cada participante. |
| A15 | Quantidades nos dois extremos Agendamento–Atendimento. |
| A16 | Registros por usuário; cada evento mantém 1 responsável pela operação. |

Nenhuma dessas lacunas foi preenchida com 0..*, 0..1 ou 1:1. Referências tipadas em parâmetros não aprovam cardinalidades de associação.

## 5. Avaliação das sugestões de métodos

| Sugestão | Resultado |
|---|---|
| Usuario.ativar/bloquear | Incluídos por RF-005/RF-006, com autorização e limites explícitos. |
| Perfil.adicionarPermissao/removerPermissao | Incluídos como refinamento da manutenção das associações em RF-008. |
| Psicologo.atualizarDadosProfissionais | Incluído por RF-015; campos e tipo pendentes. |
| Paciente.atualizarCadastro/alterarSituacao | Incluídos por RF-019/RF-020; alteração de situação condicionada às transições e auditoria. |
| Vinculo.encerrar | Não incluído: RF-021 declara condições de encerramento pendentes. |
| Vinculo.estaAtivo | Não incluído: estado ativo de vínculo não está formalizado. A consulta estaAutorizado representa a condição exigida por RN-003/RN-007, ainda com critério completo pendente. |
| Agenda.adicionarDisponibilidade/verificarDisponibilidade | Cadastro de período é diretamente sustentado, mas criação/validação estão incompletas; verificação é refinamento derivado e não substitui RF-024. |
| Agendamento.confirmar/cancelar | Incluídos como contratos condicionados por RF-026/RF-027 e RN-012/RN-013. |
| Atendimento.registrar | Incluído como criação válida por RF-030, com RN-006/RN-007. |

## 6. Revisão cruzada e critério de conclusão

| Comparação | Resultado conceitual |
|---|---|
| Diagrama × especificação | 12 classes, dez atributos, 22 operações e 16 associações descritos; nenhuma classe adicional criada para preencher lacunas. |
| Classes × requisitos | Cada classe possui fundamentação; classes sem atributos documentados permanecem sem campos inventados. |
| Métodos × regras | Contratos contemplam as regras e registram a preservação pretendida. Não demonstram mecanismos de autorização, ausência de conflitos ou integração de auditoria ainda não modelados. |
| Relacionamentos × domínio | A01–A16 e multiplicidades preservadas, incluindo PENDENTE. |
| Tipos e estados | Não há enumeração fechada ou tipo técnico inventado; tipos omitidos possuem PC correspondente. |
| Escopo | Somente modelagem OO; nenhuma mudança no domínio ou nos documentos dos outros integrantes é necessária nesta etapa. |

A cobertura funcional foi desdobrada por RF na especificação, sem acrescentar classes ou métodos. A conclusão desta etapa é um modelo documental parcial com contratos declarados, não demonstração dos mecanismos nem fechamento das pendências do grupo. A revisão estática não certifica renderização Mermaid nem execução dos contratos. A conformidade final requer resolver PC pertinentes e validar os fluxos com os Integrantes 1 e 2.
