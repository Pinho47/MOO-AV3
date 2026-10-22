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

---

# PROPOSTAS DE DECISÃO — FASE 5 (Integrante 2) · AGUARDANDO APROVAÇÃO DO GRUPO

> **Atenção:** o bloco abaixo **não é decisão vigente**. São propostas submetidas pelo Integrante 2 na Fase 5 (revisão do modelo de domínio), com o objetivo de destravar a Fase 6 (sequências) e a Fase 7 (persistência). Cada proposta só passa a valer quando o grupo a aprovar e quando o Integrante 1 mover a linha correspondente para o registro oficial de decisões.
>
> Numeração: as propostas **DEC-011** e **DEC-012** já são citadas nas especificações de caso de uso do Integrante 2 (`UC-027`) e por isso mantêm esses números. As demais são novas.
>
> Base completa: [`01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md`](revisoes/FASE5-REV-I2-revisao-modelo-dominio.md) e [`06-rastreabilidade/matriz-rastreabilidade-fase5.md`](../06-rastreabilidade/matriz-rastreabilidade-fase5.md).

## DEC-011 — Atores autorizados para cancelar um agendamento (proposta)

**Proposta:** O cancelamento (RF-027, RN-012) pode ser solicitado pelo **paciente** titular do agendamento e pelo **psicólogo** vinculado. O administrador só cancela em operação corretiva, e sempre com motivo registrado em auditoria.

**Justificativa:** Coerente com DEC-006 (acesso do paciente limitado, mas com autoatendimento de agenda) e com RN-003 (o profissional só atua sobre pacientes vinculados). Evita que a operação fique sem ator definido, como está hoje em `UC-027`.

**Efeito no modelo:** assinatura `Agendamento.cancelar(solicitante: Usuario, motivo: Texto)` com guarda de autorização; a autorização é verificável na própria operação.

**Situação:** aguardando aprovação do grupo.

## DEC-012 — Prazo de antecedência para cancelamento (proposta)

**Proposta:** O cancelamento só é aceito quando o agendamento está em situação `SOLICITADO` ou `CONFIRMADO` **e** faltam no mínimo **2 horas** para o início do horário. Após o início, o compromisso só pode ser encerrado como `REALIZADO` ou `NAO_COMPARECEU`.

**Justificativa:** Fecha a pendência citada em `UC-027` ("prazo de antecedência mínimo para cancelar — ainda sem definição do grupo") e materializa a guarda `podeSerCancelado()`.

**Efeito no modelo:** a regra fica em uma única operação consultável, reutilizada pelo caso de uso e pelo teste conceitual CT-027.

**Alternativa a discutir:** 4 horas ou 24 horas, ou valor definido por faixa de horário.

**Situação:** aguardando aprovação do grupo.

## DEC-013 — Situações da conta de usuário (proposta)

**Proposta:** `Usuario.situacaoConta` assume os valores `ATIVO`, `BLOQUEADO` e `INATIVO`. Apenas `ATIVO` permite autenticação (RN-001); `BLOQUEADO` recusa mesmo com credencial correta; `INATIVO` representa conta desativada sem penalidade.

**Justificativa:** Alinha o modelo ao dicionário de dados do plano §10.2 (`status` = ATIVO, BLOQUEADO, INATIVO) e dá significado operacional a RF-005/RF-006.

**Situação:** aguardando aprovação do grupo.

## DEC-014 — Situações cadastrais do paciente (proposta)

**Proposta:** `Paciente.situacaoCadastral` assume `ATIVO`, `SUSPENSO` e `INATIVO`, com transição livre entre si, sempre por administrador autorizado e com registro em auditoria (RN-009).

**Justificativa:** RF-020 exige "alterar a situação do cliente/paciente", mas o modelo não tinha valores definidos (pendência PC-05).

**Situação:** aguardando aprovação do grupo.

## DEC-015 — Situação e temporalidade do vínculo (proposta)

**Proposta:** `Vinculo` passa a ter `situacao` (`ATIVO`, `ENCERRADO`), `dataInicio` e `dataFim`. Ao encerrar, o vínculo deixa de autorizar o acesso do profissional ao paciente (RN-003, RN-007). Agendamentos futuros já marcados **não** são cancelados automaticamente: o sistema **alerta** o profissional para reagendar.

**Justificativa:** Fecha parcialmente a DUV-003 e a pendência P-12 do Integrante 4, e torna verificável a restrição de acesso ao longo do tempo.

**Ponto de atenção:** a alternativa "cancelar automaticamente os agendamentos futuros" é mais simples de implementar, mas retira decisão do profissional — o grupo precisa escolher.

**Situação:** aguardando aprovação do grupo.

## DEC-016 — Situações e transições do agendamento (proposta)

**Proposta:** `Agendamento.situacao` assume `SOLICITADO`, `CONFIRMADO`, `REALIZADO`, `CANCELADO` e `NAO_COMPARECEU`, com as transições:

| De | Evento | Para |
|---|---|---|
| — | `solicitar` | `SOLICITADO` |
| `SOLICITADO` | `confirmar` | `CONFIRMADO` |
| `CONFIRMADO` | `concluir` | `REALIZADO` |
| `CONFIRMADO` | `registrarFalta` | `NAO_COMPARECEU` |
| `SOLICITADO` / `CONFIRMADO` | `cancelar` | `CANCELADO` |

`solicitar`, `confirmar` e `cancelar` ocupam o horário (RN-005, RN-010). `CANCELADO`, `REALIZADO` e `NAO_COMPARECEU` são terminais, e `CANCELADO` não admite atendimento (RN-006).

**Justificativa:** Usa exatamente a nomenclatura do plano §5.4 ("Solicitado → Confirmado → Realizado; saídas para Cancelado e Não compareceu"), de modo que domínio, casos de uso, máquina de estados (EST-001) e banco (`agendamento.status`) usem os mesmos nomes.

**Situação:** aguardando aprovação do grupo.

## DEC-017 — Mecanismo da confirmação do agendamento (proposta)

**Proposta:** A confirmação do agendamento é **automática**, disparada pelo sistema no mesmo fluxo da solicitação, sem aprovação manual de outro ator — exatamente como já escrito em `UC-025`. A regra RN-013 é satisfeita por **mecanismo autorizado**, e a assinatura `confirmar(solicitante: Usuario)` fica completa, com o solicitante identificando quem originou a solicitação.

**Justificativa:** O horário só aparece para o paciente porque o profissional já o liberou; um passo de aprovação manual duplicaria a regra RN-011. Fecha a pendência P-01 do Integrante 4 e a pendência PC-07 do Integrante 3.

**Situação:** aguardando aprovação do grupo.

## DEC-018 — Modelo de tempo da disponibilidade (proposta)

**Proposta:** O horário liberado pelo profissional é um **slot discreto** (data, hora de início e duração fixa), e não um intervalo contínuo livre. Cada `Disponibilidade` corresponde a um horário ofertado e consultável.

**Justificativa:** Torna a consulta de horários (RF-024) uma lista de slots reais, simplifica a exclusividade de RN-005 (um slot, no máximo um agendamento ativo) e permite vincular o agendamento ao slot reservado (associação A17).

**Efeito no modelo:** habilita a associação condicional `Disponibilidade` → `Agendamento` e o índice de exclusividade no banco.

**Situação:** aguardando aprovação do grupo (pendência P-05 do Integrante 4).

## DEC-019 — Cardinalidade entre usuário e perfil (proposta)

**Proposta:** Cada conta de acesso tem **no máximo um** perfil (`Usuario "0..1" — Perfil "1"`); toda conta que autentica possui exatamente um perfil. Um perfil pode ser compartilhado por várias contas.

**Justificativa:** Resolve a INC-01 do Integrante 4. O caminho de verificação de autorização (RF-001, RNF-002) fica simples e o N:N já previsto pelo plano §10.1 permanece apenas entre `Perfil` e `Permissao`.

**Situação:** aguardando aprovação do grupo (pendência P-06 do Integrante 4).

## DEC-020 — Sessão fora do modelo de domínio (proposta)

**Proposta:** A sessão autenticada (RF-002) **não** é entidade do modelo de domínio. É estado volátil de aplicação, tratado por um serviço de autenticação; no diagrama de classes aparece apenas como serviço, e nas sequências como objeto de fronteira.

**Justificativa:** Coerente com a análise do Integrante 4 (P-07) e com a decisão de modelagem OO-16 do Integrante 3. Evita inflar o domínio com conceito técnico e sem regra de negócio própria.

**Situação:** aguardando aprovação do grupo.

## DEC-021 — Conta de acesso opcional para os cadastros (proposta)

**Proposta:** A relação entre `Usuario` e os cadastros `Psicologo` e `Paciente` é de `0..1` para `0..1`: o cadastro pode existir sem conta (registros importados, cadastros em_regularização) e uma conta atende a no máximo um cadastro de cada tipo.

**Justificativa:** Fecha a INC-12/P-11 do Integrante 4 e preserva a distinção entre conta de acesso e cadastro (RF-014, RF-018, DEC-006).

**Situação:** aguardando aprovação do grupo.

## DEC-022 — Notificação por e-mail como fronteira de aplicação (proposta)

**Proposta:** Enquanto o grupo não decidir a DEC-009, a notificação é modelada como **fronteira de aplicação** (interface `ServicoEnvioMensagem`), sem classe de domínio e sem tabela. Se a DEC-009 for aprovada, o Integrante 1 deve numerar o requisito correspondente (RF-033) e a notificação passa a aparecer no diagrama de componentes da Fase 7.

**Justificativa:** O plano §6.1 penaliza elementos órfãos; criar a classe `Notificacao` sem requisito seria um órfão. A include UC "Notificar paciente por e-mail" já existe e hoje não tem responsável modelada (achado R5-12).

**Situação:** aguardando aprovação do grupo (vinculada à DEC-009).

## DEC-023 — Registro de atendimento referencia o agendamento (proposta)

**Proposta:** O `Atendimento` referencia o `Agendamento` do qual decorre (A15, `1` para `0..1`). O registro só é permitido para agendamento em situação `CONFIRMADO`, e o atendimento é criado quando o agendamento passa a `REALIZADO`.

**Justificativa:** Torna verificáveis "sessão válida da agenda" (RF-030) e "agendamento cancelado não pode ser registrado como realizado" (RN-006), fechando a pendência PC-08 do Integrante 3 e a INC-09 do Integrante 4.

**Situação:** aguardando aprovação do grupo.

## DEC-024 — Colaboração de auditoria e conteúdo mínimo (proposta)

**Proposta:** O registro de auditoria é disparado por um **serviço de aplicação** após cada operação auditável, e não por dependência estrutural das entidades. O conteúdo mínimo do registro é: responsável, data, hora, operação, **situação anterior** e **nova situação** quando a operação alterou uma situação.

**Justificativa:** Fecha a pendência PC-12 do Integrante 3 (colaborações) e a INC-07 do Integrante 4 (situação anterior/nova), e mantém RN-009 e RN-014 verificáveis sem acoplar todas as classes ao `RegistroAuditoria`.

**Situação:** aguardando aprovação do grupo.

## DEC-025 — "Não compareceu" é registrado manualmente (proposta)

**Proposta:** A situação `NAO_COMPARECEU` é registrada manualmente pelo **psicólogo**, após o término do horário. Não há expiração automática.

**Justificativa:** Fecha a pendência "DEC-5" citada em `UC-027`. A alternativa automática exigiria um mecanismo de temporização, que traria complexidade (e risco) sem regra aprovada.

**Situação:** aguardando aprovação do grupo.

## DEC-026 — Convenção de numeração de casos de uso e de testes (proposta)

**Proposta:** A numeração de casos de uso segue a convenção já adotada nos casos críticos, `UC-0RF` (o número do caso é o número do requisito), e a de teste conceitual idem, `CT-0RF`. Os casos críticos recebem a linha "RFs cobertos", como já existe nos demais casos.

**Justificativa:** Torna a [matriz de rastreabilidade](../06-rastreabilidade/matriz-rastreabilidade-fase5.md) auditável linha a linha e elimina a interpretação manual dos vínculos (achados R5-16 e R5-17).

**Situação:** aguardando aprovação do grupo.

