# Registro de Decisões do Projeto

Este arquivo será utilizado para registrar decisões relevantes
tomadas durante o desenvolvimento da AV3.

## DEC-001 — Tipo de projeto

O trabalho consiste na modelagem orientada a objetos de uma
plataforma destinada a profissionais de Psicologia.

## DEC-002 — Repositório

O GitHub será utilizado como repositório oficial do projeto,
mantendo histórico de versões e organização dos artefatos.

## DEC-003 — Persistência

O sistema será modelado considerando persistência em banco de
dados relacional.

## DEC-004 — Equipe

O grupo será composto por seis integrantes com responsabilidades
principais distintas e revisão cruzada dos artefatos.

## DEC-005 — Dados

Nenhum dado real de pacientes será utilizado nos artefatos,
diagramas ou exemplos do projeto.

## DEC-006 — Acesso do Cliente/Paciente

**Decisão:**  
O Cliente/Paciente será considerado ator do sistema e possuirá acesso autenticado limitado.

**Responsabilidades permitidas:**
- realizar login;
- consultar horários disponíveis;
- solicitar agendamento;
- consultar os próprios agendamentos;
- cancelar agendamento quando permitido;
- consultar somente informações disponibilizadas ao próprio usuário.

**Restrições:**
O Cliente/Paciente não terá acesso às funções administrativas, informações de outros usuários, auditoria ou informações profissionais restritas.

**Justificativa:**  
Permitir autoatendimento básico relacionado à agenda sem conceder privilégios administrativos ou profissionais.

## DEC-007 — Exclusão do Atendente no Escopo Atual

**Decisão:**  
O perfil/ator Atendente não será incluído no escopo da plataforma nesta fase do projeto.

**Justificativa:**  
Trata-se de um papel opcional. A sua exclusão neste momento evita acréscimo desnecessário de requisitos, regras, casos de uso e permissões, mantendo o foco nos objetivos pedagógicos centrais da AV3. Caso o grupo decida incluí-lo posteriormente, nova decisão formal será registrada.

## DEC-008 — Delimitação de Escopo Negativo Inicial

**Decisão:**  
Permanecerão explicitamente fora do escopo do projeto: pagamentos, faturamento, convênios, emissão fiscal, teleconsulta, prescrição de medicamentos, prontuário clínico aprofundado e cobrança.

**Justificativa:**  
Limitar a amplitude do projeto para assegurar profundidade e rigor metodológico na modelagem orientada a objetos dos módulos essenciais.

## DEC-009 — Status do Serviço Externo de Notificação

**Decisão:**  
O uso de um Serviço Externo de E-mail/Notificação como ator externo permanece registrado como `PENDENTE DE DECISÃO DO GRUPO`.

**Justificativa:**  
A definição do canal de disparo externo ou tratamento interno de avisos será deliberada em conjunto pelo grupo antes da consolidação dos casos de uso.

## DEC-010 — Status da Funcionalidade de Inovação

**Decisão:**  
A funcionalidade de inovação permanece registrada como `PENDENTE DE DECISÃO DO GRUPO`.

**Justificativa:**  
A funcionalidade inovadora será proposta e escolhida coletivamente pela equipe para posterior integração nos requisitos, modelos UML, persistência e pitch.

