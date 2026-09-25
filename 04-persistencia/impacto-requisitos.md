# Impacto dos Requisitos em Persistência e Arquitetura (Integrante 4 — Fase 3)

Período de referência: 15/09/2026 a 28/09/2026. Sem DER, SQL ou mapeamento definitivo.

## 1. Objetivo
Analisar RF, RNF, RN e critérios de aceite sob a ótica de persistência, integridade e arquitetura.

Nota: RFs abaixo são contribuição técnica do Integrante 4. IDs finais devem ser conferidos com a baseline oficial do Integrante 1 antes de congelar.

## 2. Requisitos com impacto em persistência

| Requisito | Impacto |
|---|---|
| RF-001 — Autenticação de usuários ativos | Médio — usuário, credencial protegida, estado da conta |
| RF-002 — Bloquear/reativar contas | Alto — estado da conta e histórico |
| RF-003 — Perfil profissional do psicólogo | Alto — perfil e vínculo com usuário |
| RF-004 — Pacientes e vínculo com psicólogo | Alto — relação persistente |
| RF-005 — Disponibilidades do psicólogo | Alto — intervalos/horários |
| RF-006 — Agendamentos sem conflito | Alto — integridade e concorrência |
| RF-007 — Confirmar/cancelar agendamentos | Alto — estado e histórico |
| RF-008 — Auditoria de operações relevantes | Alto — histórico controlado |
| RF-009 — Consulta conforme permissões | Alto — autorização e filtros |
| RF-010 — Lista de espera (se adotada) | Médio — persistência adicional |

## 3. Regras de negócio com impacto em integridade

| ID | Regra | Restrição futura |
|---|---|---|
| RN-001 | Psicólogo só acessa pacientes com vínculo autorizado | FK + validação de acesso |
| RN-002 | Um horário sem dois agendamentos ativos | UNIQUE / controle transacional |
| RN-003 | Usuário bloqueado não autentica | Estado persistido + CHECK |
| RN-004 | Sem visualização de senha em texto puro | Hash seguro, sem coluna de senha original |
| RN-005 | Agendamento cancelado não pode ser realizado | Status controlado |
| RN-006 | Atendimento associado a paciente e psicólogo válidos | FKs + integridade referencial |

## 4. RNFs sugeridos

| ID | Requisito | Verificação |
|---|---|---|
| RNF-001 | Sem senha em texto puro | Inspeção do modelo |
| RNF-002 | Respeito a permissões | Teste de acesso negado |
| RNF-003 | Auditoria de operações críticas | Verificar registro criado |
| RNF-004 | Integridade referencial | PK/FK + inserção inválida recusada |
| RNF-005 | Sem dois agendamentos ativos no mesmo horário | Teste de conflito |
| RNF-006 | Separação apresentação/aplicação/domínio/persistência | Revisão de pacotes/componentes |
| RNF-007 | Manutenção sem alterações espalhadas | Revisão de dependências |
| RNF-008 | Controle de acesso a dados sensíveis | Cenários por perfil |

## 5. Critérios de aceite (Dado/Quando/Então)
- CA-001: usuário ativo + credencial válida, ao informar credenciais, autentica com funções autorizadas
- CA-002: usuário bloqueado, ao tentar entrar, autenticação negada
- CA-003: horário ocupado, ao solicitar outro agendamento, operação recusada
- CA-004: agendamento cancelado, ao tentar marcar como realizado, operação recusada
- CA-005: operação crítica concluída, gera auditoria com data, usuário e operação
- CA-006: psicólogo sem vínculo, ao consultar cadastro restrito, acesso negado
- CA-007: relação com entidade inexistente, ao gravar, operação recusada por integridade

## 6. Rastreabilidade inicial
- RF-001 + RN-003 → Usuário + estado → CA-001/CA-002
- RF-004 + RN-001 → Paciente + vínculo → CA-006
- RF-006 + RN-002 → Agendamento + disponibilidade → CA-003
- RF-007 + RN-005 → Status do agendamento → CA-004
- RF-008 → Auditoria → CA-005
- RF-009 + RN-001 → Autorização + vínculo → CA-006
- RNF-001 + RN-004 → Hash seguro → CA-001 + inspeção
- RNF-004 + RN-006 → PK/FK → CA-007

## 7. Relações a validar com o Integrante 3
Usuário-Perfil, Perfil-Permissão, Psicólogo-Área, Vínculo Psicólogo-Paciente, Disponibilidade-Agendamento, Agendamento-Atendimento, Usuário-Auditoria, necessidade de entidade de Notificação.

## 8. Pendências antes da modelagem definitiva
- Alinhar IDs com Integrante 1
- Confirmar integrações externas
- Aguardar classes e multiplicidades do Integrante 3
- Não criar DER, modelo relacional, DDL ou componentes definitivos nesta fase
