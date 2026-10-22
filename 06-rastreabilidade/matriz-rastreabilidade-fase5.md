# Matriz de Rastreabilidade — Fase 5 (modelo de domínio, classes e objetos)

**Responsável:** Integrante 2 — Casos de uso e diagramas comportamentais.
**Data:** 21/10/2026 · **Versão:** v1.0 (proposta para o grupo).
**Substitui/complementa:** `02-requisitos/rastreabilidade-inicial.md` (matriz RF → RN → UC → CA), que **não** cobre classes, sequências e tabelas.

Formato de colunas conforme o plano §6.1: `RF | RN/RNF | UC | Classe | Sequência | Tabela | Qualidade/Teste`, acrescido de **Operação** (a operação que executa o requisito — a lacuna que a matriz inicial não permitia ver) e **Status**.

Legenda de status:

| Status | Significado |
|---|---|
| **Coberto** | Uma operação existente do modelo do Integrante 3 executa o requisito. |
| **Parcial** | Existe operação, mas falta assinatura, guarda ou estado (pendência PC-xx). |
| **Proposto** | Operação **nova** proposta por esta revisão (OP-xx da revisão FASE5-REV-I2). |
| **Fora do domínio** | Responsabilidade de aplicação/fronteira (AP-xx); não vira classe de domínio. |

## 1. Matriz principal (RF → RN/RNF → UC → Classe → Operação → Sequência → Tabela → Qualidade/Teste)

| RF | RN / RNF | UC | Classe | Operação | Sequência | Tabela (proposta Fase 7) | Qualidade / Teste | Status |
|---|---|---|---|---|---|---|---|---|
| RF-001 Autenticar usuário | RN-001, RN-002, RNF-001, RNF-002 | UC-001 | Usuario (+ aplicação `ServicoAutenticacao`) | OP-01 `podeAutenticar`; OP-02 `confereCredencial` | SEQ-001 | `usuario` | Segurança / CT-001 | **Proposto** |
| RF-002 Encerrar sessão | RNF-002 | UC-001 (A1) | — (aplicação) | AP-01 `ServicoAutenticacao.encerrar` | SEQ-001 | `sessao` (volátil, não persistente) | Usabilidade / CT-002 | Fora do domínio (P-07) |
| RF-003 Cadastrar usuário | RN-002, RNF-001 | UC-003 | Usuario, Perfil | OP-03 `cadastrar` | SEQ-001 | `usuario`, `perfil` | Segurança / CT-003 | **Proposto** |
| RF-004 Alterar dados de usuário | RN-002 | UC-003 | Usuario | `atualizarDadosPermitidos` | SEQ-001 | `usuario` | Segurança / CT-004 | Coberto |
| RF-005 Ativar usuário | RNF-003 | UC-003 | Usuario | `ativar` | SEQ-001 | `usuario` | Disponibilidade / CT-005 | Coberto |
| RF-006 Bloquear usuário | RN-001, RNF-003 | UC-003 | Usuario | `bloquear` | SEQ-001 | `usuario` | Segurança / CT-006 | Coberto |
| RF-007 Gerenciar perfis | RNF-002 | UC-003 | Perfil | `adicionarPermissao`, `removerPermissao` (+ criação de perfil a definir) | SEQ-001 | `perfil` | Manutenibilidade / CT-007 | Parcial (PC-10) |
| RF-008 Gerenciar permissões | RNF-002 | UC-003 | Permissao | — (criação/atualização da permissão sem operação) | SEQ-001 | `permissao` | Manutenibilidade / CT-008 | Parcial |
| RF-009 Consultar painel administrativo | RNF-004 | UC-003 | (leitura agregada) | AP-05 camada de consulta | SEQ-001 | — (projeção de leitura) | Usabilidade / CT-009 | Fora do domínio |
| RF-010 Consultar usuários cadastrados | RNF-002 | UC-003 | Usuario | OP-04 `consultarPermitidos` | SEQ-001 | `usuario` | Segurança / CT-010 | **Proposto** |
| RF-011 Consultar psychologists cadastrados | RNF-002 | UC-003 | Psicologo | OP-06 `consultarCadastros` | SEQ-001 | `psicologo` | Segurança / CT-011 | **Proposto** |
| RF-012 Consultar clientes/pacientes cadastrados | RNF-005 | UC-003 | Paciente | OP-08 `consultarCadastros` | SEQ-001 | `paciente` | Segurança / CT-012 | **Proposto** |
| RF-013 Consultar registros de auditoria | RN-008, RN-014 | UC-013 | RegistroAuditoria | OP-09 `consultar` | SEQ-001 | `registro_auditoria` | Rastreabilidade / CT-013 | **Proposto** |
| RF-014 Cadastrar psicólogo | RN-002, RNF-002 | UC-003 | Psicologo (+ Usuario, AreaAtuacao) | OP-05 `cadastrar` | SEQ-001 | `psicologo` | Manutenibilidade / CT-014 | **Proposto** |
| RF-015 Atualizar perfil profissional | RNF-002 | UC-015 | Psicologo | `atualizarDadosProfissionais` | SEQ-002 | `psicologo` | Manutenibilidade / CT-015 | Coberto |
| RF-016 Associar área de atuação | RNF-002 | UC-015 | Psicologo, AreaAtuacao | `associarAreaAtuacao` | SEQ-002 | `psicologo_area_atuacao` | Manutenibilidade / CT-016 | Coberto |
| RF-017 Consultar pacientes vinculados | RN-003, RNF-006 | UC-016 | Psicologo, Vinculo | `consultarPacientesVinculados` | SEQ-002 | `vinculo` | Confiabilidade / CT-017 | Coberto |
| RF-018 Cadastrar cliente/paciente | RN-005 | UC-003 | Paciente (+ Usuario) | OP-07 `cadastrar` | SEQ-001 | `paciente` | Segurança / CT-018 | **Proposto** |
| RF-019 Atualizar cadastro administrativo | RN-009 | UC-003 | Paciente | `atualizarCadastro` | SEQ-001 | `paciente` | Rastreabilidade / CT-019 | Coberto |
| RF-020 Alterar situação do cliente/paciente | RN-009 | UC-003 | Paciente | `alterarSituacao` | SEQ-001 | `paciente` | Rastreabilidade / CT-020 | Parcial (estados §6 da revisão) |
| RF-021 Vincular cliente/paciente a psicólogo | RN-003, RN-007 | UC-021 | Vinculo | `formalizar`; OP-21 `encerrar` | SEQ-002 | `vinculo` | Confiabilidade / CT-021 | Coberto (estado pendente) |
| RF-022 Consultar informações do próprio cadastro | RN-004, RNF-005 | UC-022 | Paciente | `consultarCadastroPermitido` | SEQ-001 | `paciente` | Segurança / CT-022 | Coberto |
| RF-023 Cadastrar disponibilidade | RN-005, RN-011 | UC-015 | Agenda, Disponibilidade | OP-10 `cadastrarDisponibilidade` | SEQ-002 | `disponibilidade` | Confiabilidade / CT-023 | **Proposto** |
| RF-024 Consultar horários disponíveis | RN-010, RNF-010 | UC-024 (subsumido em UC-025) | Agenda, Disponibilidade, Agendamento | OP-11 `consultarHorariosDisponiveis` | SEQ-003 | `disponibilidade`, `agendamento` | Usabilidade / CT-024 | **Proposto** |
| RF-025 Solicitar agendamento | RN-005, RN-010 | UC-025 | Agendamento | `solicitar` | SEQ-003 | `agendamento` | Confiabilidade / CT-025 | Parcial (PC-07) |
| RF-026 Confirmar agendamento | RN-013 | UC-025 | Agendamento | OP-13 `confirmar` | SEQ-003 | `agendamento` | Confiabilidade / CT-026 | Parcial (PC-07) |
| RF-027 Cancelar agendamento | RN-012, RN-006 | UC-027 | Agendamento | OP-14 `cancelar`; OP-15 `podeSerCancelado` | SEQ-003 | `agendamento` | Confiabilidade / CT-027 | Parcial (PC-07) |
| RF-028 Consultar os próprios agendamentos | RN-004 | UC-028 | Agendamento | OP-19 `consultarMeus` | SEQ-003 | `agendamento` | Usabilidade / CT-028 | **Proposto** |
| RF-029 Consultar agenda do psicólogo | RN-011 | UC-029 (fluxo A1 de UC-015) | Agenda, Agendamento | OP-12 `consultarCompromissos` | SEQ-002 | `agenda`, `agendamento` | Usabilidade / CT-029 | **Proposto** |
| RF-030 Registrar conceitualmente um atendimento | RN-006, RN-007, DEC-008 | UC-016 | Atendimento, Agendamento | OP-20 `registrar(agendamento, …)` | SEQ-003 | `atendimento` | Confiabilidade / CT-030 | **Proposto** |
| RF-031 Consultar histórico permitido de atendimentos | RN-003, RN-007, RNF-006 | UC-016 | Psicologo, Atendimento | `consultarHistoricoPermitido` | SEQ-002 | `atendimento` | Segurança / CT-031 | Coberto |
| RF-032 Registrar evento de auditoria | RN-009, RN-014, RNF-004 | UC-032 (`<<include>>`) | RegistroAuditoria | `registrar` + AP-02 | SEQ-001/002/003 | `registro_auditoria` | Rastreabilidade / CT-032 | Coberto (colaboração parcial) |

Legenda de casos de uso adotada nesta matriz (convenção já usada pelo Integrante 2 nos casos críticos: `UC-0RF`):

| UC | Nome | Situação |
|---|---|---|
| UC-001 | Autenticar usuário | Especificado (crítico) |
| UC-003 | Gerenciar usuários e permissões | Especificado (demais) — **proposta de ID** |
| UC-013 | Consultar auditoria | Especificado (demais) — **proposta de ID** |
| UC-015 | Gerenciar perfil e agenda | Especificado (demais) — **proposta de ID** |
| UC-016 | Atender pacientes vinculados | Especificado (demais) — **proposta de ID** |
| UC-021 | Vincular psicólogo a paciente | Especificado (crítico) |
| UC-022 | Consultar meu cadastro | Especificado (demais) — **proposta de ID** |
| UC-024 | Consultar horários disponíveis | **Proposto**: hoje está subsumido no passo 1 de UC-025; o plano §3.1 e o RF-024 pedem caso próprio |
| UC-025 | Solicitar agendamento (com confirmação automática) | Especificado (crítico) |
| UC-027 | Cancelar agendamento | Especificado (crítico) |
| UC-028 | Consultar os próprios agendamentos | **Proposto**: RF-028 não tem caso de uso especificado |
| UC-029 | Consultar a agenda do psicólogo | **Proposto**: hoje é apenas o fluxo A1 de UC-015 |
| UC-032 | Registrar evento de auditoria (`<<include>>`) | Especificado (demais) — **proposta de ID** |
| UC-033 | Notificar paciente por e-mail (`<<include>>`) | Especificado (demais) — **proposta de ID**; sem RF numerado (DEC-009) |
| UC-034 | Encaixe em lista de espera | **Proposto** (Fase 3 / DEC-010); hoje inexistente |

> Pendência de padronização (ação do Integrante 2, Fase 5/6): os casos críticos (UC-001, UC-021, UC-025, UC-027) **não têm a linha "RFs cobertos"**, presente nos demais. A linha deve ser acrescentada para que a matriz seja auditável sem inferência.

## 2. Cobertura por classe (verificação de órfãos)

| Classe | RFs que a classe responde | Qtde | Sem RF? |
|---|---|---|---|
| Usuario | RF-001, RF-002, RF-003, RF-004, RF-005, RF-006, RF-010, RF-032 | 8 | Não |
| Perfil | RF-001, RF-003, RF-007, RF-008 | 4 | Não |
| Permissao | RF-001, RF-007, RF-008 | 3 | Não |
| Psicologo | RF-014, RF-015, RF-016, RF-017, RF-023, RF-029, RF-030, RF-031 | 8 | Não |
| AreaAtuacao | RF-016 | 1 | Não |
| Paciente | RF-018, RF-019, RF-020, RF-022, RF-024, RF-025, RF-026, RF-027, RF-028 | 9 | Não |
| Vinculo | RF-017, RF-021, RF-030, RF-031 | 4 | Não |
| Agenda | RF-023, RF-024, RF-029 | 3 | Não |
| Disponibilidade | RF-023, RF-024, RF-025 | 3 | Não |
| Agendamento | RF-025, RF-026, RF-027, RF-028, RF-029, RF-030 | 6 | Não |
| Atendimento | RF-030, RF-031 | 2 | Não |
| RegistroAuditoria | RF-013, RF-032 | 2 | Não |

**Resultado:** 12/12 classes têm RF associado; 32/32 RFs têm classe associada. **Não há classe órfã nem RF órfão.** A carência do modelo é de operação, não de estrutura — o que confirma que o modelo do Integrante 3 não precisa ser redesenhado.

## 3. Verificação de órfãos exigida pelo plano §6.1

| Verificação | Resultado | Observação |
|---|---|---|
| Requisito funcional sem caso de uso | **Parcial** | RF-024, RF-028, RF-029 sem caso de uso próprio (subsumidos). Proposta de UC-024/028/029 acima. |
| Caso de uso sem requisito | **Sim — 1** | UC-033 "Notificar paciente por e-mail" (DEC-009) sem RF numerado. Depende de decisão do grupo (R5-12). |
| Caso de uso sem classe | Não | Todos os casos mapeiam para ao menos uma classe. |
| Classe persistente sem tabela | Não (proposta) | 14 tabelas propostas na §4; `sessao` é volátil e fora do domínio. |
| Classe sem RF | Não | Ver §2. |
| Tabela sem classe | Não | Ver §4. |
| Diagrama sem classe/requisito | **Sim — 1** | OBJ-001 (objetos) não inclui `Agendamento`, `Atendimento` nem `RegistroAuditoria`; OBJ-002 proposto resolve. |
| Sequência sem operação | **Parcial** | SEQ-001 depende de OP-01/OP-02; SEQ-003 depende de OP-13/OP-14 completadas. Ver §5 da revisão. |
| Estado sem atributo | **Sim — 4** | Situação de conta, situação cadastral, situação de vínculo e situação de agendamento sem valores (R5-04). |

## 4. Tabelas propostas (entrada para a Fase 7 — DER e dicionário de dados)

| Tabela | Classe | PK / FK relevante | Observação |
|---|---|---|---|
| `usuario` | Usuario | `id_usuario` PK; `id_perfil` FK | `status` = ATIVO \| BLOQUEADO \| INATIVO (proposta §6 da revisão, alinhada ao plano §10.2) |
| `perfil` | Perfil | `id_perfil` PK | 1 perfil por conta (A01) |
| `permissao` | Permissao | `id_permissao` PK | Catálogo de permissões |
| `perfil_permissao` | Perfil ↔ Permissao | PK composta | N:N resolvido (plano §10.1) |
| `psicologo` | Psicologo | `id_psicologo` PK; `id_usuario` FK UNIQUE | Conta opcional (A03, P-11) |
| `area_atuacao` | AreaAtuacao | `id_area` PK | Catálogo (P-08) |
| `psicologo_area_atuacao` | Psicologo ↔ AreaAtuacao | PK composta | N:N resolvido |
| `paciente` | Paciente | `id_paciente` PK; `id_usuario` FK UNIQUE | `situacao_cadastral` = ATIVO \| SUSPENSO \| INATIVO |
| `vinculo` | Vinculo | `id_vinculo` PK; `id_psicologo` FK; `id_paciente` FK | `situacao` = ATIVO \| ENCERRADO; `data_inicio`, `data_fim`; unicidade do par ativo (PC-06) |
| `agenda` | Agenda | `id_agenda` PK; `id_psicologo` FK UNIQUE | 1:1 com o profissional (A08) |
| `disponibilidade` | Disponibilidade | `id_disponibilidade` PK; `id_agenda` FK | Slot ou período conforme P-05 |
| `agendamento` | Agendamento | `id_agendamento` PK; `id_psicologo` FK; `id_paciente` FK; `id_agenda` FK; `id_disponibilidade` FK (A17, condicional) | `status` = SOLICITADO \| CONFIRMADO \| REALIZADO \| CANCELADO \| NAO_COMPARECEU; índice de exclusividade (RN-005, INC-05/INC-06 do I4) |
| `atendimento` | Atendimento | `id_atendimento` PK; `id_agendamento` FK UNIQUE; `id_psicologo` FK; `id_paciente` FK | Registro conceitual, sem conteúdo clínico (DEC-008); A15 = 1:1 opcional |
| `registro_auditoria` | RegistroAuditoria | `id_registro` PK; `id_usuario` FK | `operacao`, `data`, `hora`, detalhes; RN-014 (imutabilidade lógica) |
| `sessao` (opcional) | — | — | **Não é entidade de domínio** (P-07); se implementada, é técnica, não modelada em CLS-001 |

Observação: as tabelas acima são **proposta do Integrante 2** para acelerar a Fase 7. A authoritative é a análise do Integrante 4 (`04-persistencia/analises/`), que precisa ser refeita contra o modelo aprovado (R5-13).

## 5. Sequências previstas na Fase 6 e a operação que cada mensagem executa

| Sequência | UC | Atores | Operações chamadas | Dependências |
|---|---|---|---|---|
| SEQ-001 | UC-001 | Paciente/Psicólogo/Administrador → Sistema | AP-01 `autenticar`; OP-01 `podeAutenticar`; OP-02 `confereCredencial`; AP-01 `criarSessao`; AP-02 `registrar` | OP-01/OP-02 (propostas) |
| SEQ-002 | UC-021, UC-015, UC-016 | Administrador/Psicólogo → Sistema | OP-05/OP-08 `consultarCadastros`; `formalizar`; `atualizarDadosProfissionais`; `associarAreaAtuacao`; OP-10 `cadastrarDisponibilidade`; `consultarPacientesVinculados`; `consultarHistoricoPermitido`; AP-02 `registrar` | OP-05/OP-08/OP-10 (propostas) |
| SEQ-003 | UC-025, UC-027, UC-024, UC-028, UC-029 | Paciente/Psicólogo → Sistema | OP-11 `consultarHorariosDisponiveis`; `solicitar`; `verificarDisponibilidade`; OP-13 `confirmar`; OP-14 `cancelar`; OP-15 `podeSerCancelado`; OP-16 `estaCancelada`; OP-17 `concluir`; OP-18 `registrarFalta`; OP-20 `registrar`; AP-02 `registrar`; AP-04 `notificar` | OP-11 (proposta), assinaturas PC-07, DEC-009 |
| SEQ-004 | UC-034 (lista de espera) | Paciente/Psicólogo → Sistema | — | **Bloqueada por DEC-010** (R5-14) |

## 6. Testes conceituais previstos (identificadores CT-xxx)

O plano §5 adota `CT-001, CT-002, …` como identificadores de teste conceitual. Proposta de convenção: **CT coincide com o RF** (`CT-0RF`), o que permite auditoria imediata contra `CA-RF-xxx`.

| CT | RF | Cenário conceitual | Critério de aceite |
|---|---|---|---|
| CT-001 | RF-001 | Conta bloqueada com senha correta é recusada | CA-RF-001, RN-001 |
| CT-002 | RF-002 | Sessão encerrada invalida o acesso corrente | CA-RF-002 |
| CT-003 | RF-003 | Conta criada recebe perfil e não expõe senha | CA-RF-003, RN-002 |
| CT-004…CT-006 | RF-004…006 | Alteração permitida, ativação e bloqueio | CA-RF-004…006 |
| CT-007/008 | RF-007/008 | Perfil só recebe permissões que o administrador possui | CA-RF-007/008, RNF-002 |
| CT-009 | RF-009 | Painel exibe somente indicadores autorizados | CA-RF-009 |
| CT-010…012 | RF-010…012 | Listagens administrativas não expõem credencial nem dados além do permitido | CA-RF-010…012 |
| CT-013 | RF-013 | Auditoria filtrada e em ordem cronológica | CA-RF-013, RN-008 |
| CT-014…016 | RF-014…016 | Cadastro de profissional e área de atuação | CA-RF-014…016 |
| CT-017 | RF-017 | Profissional não vinculado não acessa o paciente | CA-RF-017, RN-003 |
| CT-018…020 | RF-018…020 | Cadastro/atualização/situação do paciente com auditoria | CA-RF-018…020, RN-009 |
| CT-021 | RF-021 | Vínculo duplicado é recusado; encerramento bloqueia acessos futuros | CA-RF-021, RN-003/RN-007 |
| CT-022 | RF-022 | Paciente só vê o próprio cadastro | CA-RF-022, RN-004 |
| CT-023 | RF-024 | Disponibilidade na agenda própria e sem conflito | CA-RF-023, RN-005/RN-011 |
| CT-024 | RF-024 | Horários disponíveis excluem slots já reservados | CA-RF-024, RN-010 |
| CT-025…027 | RF-025…027 | Solicitação, confirmação automática e cancelamento dentro do prazo | CA-RF-025…027, RN-013/RN-012 |
| CT-028 | RF-028 | Paciente só lista os próprios agendamentos | CA-RF-028, RN-004 |
| CT-029 | RF-029 | Agenda consolidada do profissional | CA-RF-029, RN-011 |
| CT-030/031 | RF-030/031 | Atendimento exige agendamento confirmado; histórico respeita o vínculo | CA-RF-030/031, RN-006/RN-007 |
| CT-032 | RF-032 | Toda alteração auditável gera registro com responsável, data, hora e operação | CA-RF-032, RN-009/RN-014 |

## 7. Regras de manutenção desta matriz

1. Toda mudança em RF, RN, UC, classe, operação, sequência ou tabela **atualiza esta matriz** no mesmo commit (plano §6.1 e §13).
2. Nenhum RF, UC, classe, tabela, sequência ou estado pode ficar órfão; a verificação está na §3 e deve ser repetida a cada atualização.
3. A coluna **Operação** é a que evidencia o modelo: RF sem operação é lacuna concreta — foi ela que revelou as 11 lacunas de responsabilidade listadas na §4.6 da revisão FASE5-REV-I2.
4. Mudança de cardinalidade (multiplicidade) ou de estado altera a coluna **Tabela** e o índice de exclusividade de RN-005 — deve ser comunicada ao Integrante 4 antes do DER.
5. A numeração de UC (`UC-0RF`) e de CT (`CT-0RF`) é proposta do Integrante 2 e precisa de homologação do grupo para virar padrão do dossiê.
