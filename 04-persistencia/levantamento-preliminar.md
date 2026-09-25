# Levantamento Preliminar de Persistência (Integrante 4 — Fase 2)

Período de referência: 08/09/2026 a 14/09/2026. Recuperação posterior, sem tabelas definitivas.

## 1. Objetivo
Revisar visão, escopo, atores e glossário sob a ótica de persistência e arquitetura, sem criar DER, tabelas, PK/FK ou SQL.

## 2. Conceitos potencialmente persistentes
- Usuário (estado da conta: ATIVO, BLOQUEADO, INATIVO)
- Perfil, Permissão
- Psicólogo (CRP, áreas de atuação)
- Área de Atuação
- Cliente/Paciente (cadastro administrativo)
- Vínculo Psicólogo-Paciente
- Disponibilidade
- Agendamento e status
- Atendimento conceitual (sem dados reais)
- Auditoria
- Notificação (provável, se serviço externo confirmado)
- Lista de espera (provável, se inovação adotada)

Nomes preliminares. Alinhamento definitivo com o diagrama de classes do Integrante 3.

## 3. Relações relevantes observadas no escopo
- Usuário ↔ Perfil
- Perfil ↔ Permissão
- Psicólogo ↔ Área de Atuação
- Psicólogo ↔ Cliente/Paciente (via Vínculo)
- Psicólogo ↔ Disponibilidade
- Psicólogo ↔ Agendamento
- Cliente/Paciente ↔ Agendamento
- Agendamento ↔ Atendimento
- Usuário ↔ Auditoria

Sem cardinalidades finais nesta fase.

## 4. Restrições que impactam persistência
- Senha nunca em texto puro (hash seguro: Argon2id, bcrypt ou PBKDF2 — OWASP)
- Usuário bloqueado precisa ter situação identificável
- Psicólogo só acessa pacientes com vínculo autorizado
- Paciente não acessa dados de outro paciente
- Agendamento precisa preservar estado/situação
- Um horário não pode ter dois agendamentos ativos simultaneamente
- Auditoria deve identificar operação, usuário e data/hora
- Dados reais de pacientes proibidos (LGPD: dados de saúde são sensíveis)
- Exclusão física versus inativação a decidir posteriormente

## 5. Impactos arquiteturais preliminares
Módulos prováveis: Identidade e Acesso, Administração, Psicólogos, Clientes/Pacientes, Agenda, Atendimento, Auditoria, Notificações (se confirmada).
- Dependências entre módulos devem ser controladas
- Segurança com responsabilidade definida
- Persistência respeitando fronteiras do domínio
- Componentes e implantação em fase posterior

## 6. Glossário técnico mínimo
- Persistência, PK, FK, integridade referencial, auditoria, restrição, integração externa, cardinalidade

## 7. Dependências com a modelagem de domínio
Aguardar Integrante 3 para multiplicidades, composição/agregação, herança e classes persistentes versus serviços de apoio.

## 8. Itens ainda pendentes
- Confirmação de serviço externo de e-mail/notificação
- Confirmação da inovação (lista de espera)
- Diagrama de contexto oficial aprovado pela equipe
- Alinhamento terminológico com Integrantes 1 e 3
