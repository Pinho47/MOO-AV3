# Escopo do Sistema

## 1. Escopo geral

O escopo deste projeto compreende a modelagem orientada a objetos de uma plataforma integrada para gestão administrativa e profissional voltada a psicólogos. O sistema visa estruturar as entidades, relações e fluxos operacionais necessários para centralizar cadastros, controlar permissões, gerenciar agendas, registrar conceitualmente atendimentos e assegurar trilhas de auditoria, garantindo segurança e integridade de dados.

## 2. Módulos contemplados

A plataforma é composta pelos seguintes módulos principais:
1. **Identidade e Acesso**
2. **Administração**
3. **Psicólogos**
4. **Clientes/Pacientes**
5. **Agenda**
6. **Atendimento**
7. **Auditoria**
8. **Inovação** (Pendente de definição pelo grupo)

## 3. Funcionalidades gerais por módulo

### 3.1 Identidade e Acesso
- Login e autenticação de usuários;
- Gestão de credenciais com armazenamento seguro;
- Gerenciamento de perfis de acesso;
- Atribuição e controle de permissões por perfil;
- Ativação e bloqueio de contas de usuários.

### 3.2 Administração
- Painel administrativo central;
- Gestão de usuários (criação, edição, bloqueio, vinculação de perfis);
- Gestão de psicólogos (cadastro, vinculação de áreas de atuação);
- Gestão de clientes/pacientes (cadastro administrativo, situação);
- Gestão de perfis e matriz de permissões;
- Consulta e monitoramento de logs de auditoria.

### 3.3 Psicólogos
- Gerenciamento do próprio perfil profissional;
- Associação a uma ou mais áreas de atuação;
- Consulta de clientes/pacientes autorizados mediante vínculo;
- Definição e gerenciamento de horários de disponibilidade;
- Consulta e acompanhamento de sua agenda de atendimentos;
- Execução de funções profissionais autorizadas;
- Registro conceitual de sessões de atendimento realizadas;
- Consulta ao histórico permitido de atendimentos.

### 3.4 Clientes/Pacientes
- Cadastro administrativo e acompanhamento de situação cadastral;
- Representação formal do vínculo com o psicólogo responsável;
- Acesso autenticado exclusivo com permissões restritas (autoatendimento);
- Consulta de horários e faixas de disponibilidade do psicólogo vinculado;
- Solicitação de agendamentos;
- Consulta aos próprios agendamentos;
- Cancelamento de agendamentos próprios (respeitando regras estabelecidas).

### 3.5 Agenda
- Configuração de faixas e regras de disponibilidade dos profissionais;
- Criação e manutenção de agendamentos;
- Confirmação de agendamentos;
- Cancelamento de agendamentos;
- Controle dos estados/situação do agendamento (ex.: agendado, confirmado, cancelado, realizado).

### 3.6 Atendimento
- Registro conceitual de sessões de atendimento;
- Acesso ao histórico permitido da sessão;
- Vínculo obrigatório com o psicólogo responsável;
- Vínculo obrigatório com o cliente/paciente atendido.

### 3.7 Auditoria
- Registro cronológico e imutável de operações relevantes realizadas no sistema;
- Identificação do usuário responsável pela ação;
- Registro de data, hora e identificação da operação realizada;
- Suporte a consultas administrativas para fins de conformidade e segurança.

### 3.8 Inovação
```text
PENDENTE DE DECISÃO DO GRUPO
```
A funcionalidade de inovação será definida em momento oportuno pelo grupo e posteriormente incorporada aos requisitos, modelos, persistência e apresentação do projeto.

## 4. Fora do escopo

Para manter a viabilidade e controlar a complexidade da modelagem dentro dos limites da AV3, os seguintes itens estão explicitamente **fora do escopo inicial**:
- Módulo de pagamentos e processamento financeiro;
- Faturamento e controle de caixa;
- Gestão e integração com operadoras de planos de saúde / convênios;
- Emissão de notas fiscais e documentos fiscais eletrônicos;
- Módulo de teleconsulta / videoconferência em tempo real;
- Prescrição de medicamentos ou emissão de receitas;
- Prontuário clínico eletrônico aprofundado/completo;
- Cobrança automatizada e régua de cobrança;
- Integrações com gateways financeiros e bancários.

## 5. Fronteiras do sistema

### 5.1 O que faz parte do sistema (Interno)
- Entidades de domínio, serviços de negócio e regras operacionais da plataforma;
- Interfaces e pontos de interação com os atores autenticados (Administrador, Psicólogo e Cliente/Paciente);
- Mecanismo de autenticação, autorização e controle de sessão;
- Camada de persistência relacional para armazenamento de dados da aplicação e auditoria.

### 5.2 O que é externo ao sistema (Fronteiras externas)
- Navegadores e dispositivos de acesso utilizados pelos usuários finais;
- Serviço Externo de E-mail/Notificação (caso seja confirmada a sua utilização);
- Sistemas de cobrança, bancos ou operadoras (descartados pelo escopo negativo).

## 6. Premissas

- **PRE-001** — Cada usuário possuirá identificação própria.
- **PRE-002** — Psicólogos poderão possuir uma ou mais áreas de atuação.
- **PRE-003** — Clientes/Pacientes poderão possuir acesso autenticado limitado.
- **PRE-004** — O acesso às funcionalidades dependerá de perfil e permissões.
- **PRE-005** — Psicólogos somente acessarão pacientes vinculados.
- **PRE-006** — A persistência futura será modelada utilizando banco de dados relacional.

## 7. Restrições

- **RES-001** — Dados reais de pacientes não poderão ser utilizados.
- **RES-002** — Senhas não poderão ser armazenadas ou disponibilizadas em texto puro.
- **RES-003** — O projeto possui foco em modelagem e não exige implementação completa.
- **RES-004** — Os diagramas deverão utilizar UML 2.x quando aplicável.
- **RES-005** — A persistência será modelada considerando banco de dados relacional.
- **RES-006** — O Cliente/Paciente não poderá acessar informações pertencentes a outros pacientes.

## 8. Pendências de decisão

1. **Funcionalidade de Inovação:**
   - Status: `PENDENTE DE DECISÃO DO GRUPO`
   - Descrição: O grupo definirá a funcionalidade inovadora a ser modelada antes do fechamento dos requisitos funcionais.

2. **Serviço Externo de E-mail/Notificação:**
   - Status: `PENDENTE DE DECISÃO DO GRUPO`
   - Descrição: Avaliação se a plataforma contará com ator externo para disparo de mensagens ou se a notificação será tratada apenas internamente.
