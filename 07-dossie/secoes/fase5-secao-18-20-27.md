# Dossiê — Seções 18, 19, 20 e 27 (texto pronto)

**Responsável:** Integrante 2 (Fase 5) · **Data:** 21/10/2026 · **Versão:** v1.0.
**Uso:** texto para as seções 18 (Modelo de domínio), 19 (Diagrama de classes), 20 (Diagrama de objetos) e 27 (Princípios OO e SOLID) do dossiê final (estrutura do plano §16).
**Regra de inserção de diagramas (plano §16.1):** cada diagrama entra no dossiê com título + identificador, objetivo, elementos principais, como interpretar, decisões de modelagem, relação com requisitos/classes/banco e **referência ao arquivo editável** no repositório. As imagens (PNG/SVG) são geradas a partir dos `.mmd` pelo Integrante 6; aqui fica o texto e o caminho do fonte editável.

> **Estado dos modelos na data deste texto:** o diagrama de classes e o diagrama de objetos estão em **fase de aprovação**. As multiplicidades e os nomes de situação propostos nesta seção foram submetidos ao grupo e ainda não são decisão formal (ver `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md`). Enquanto o grupo não aprovar, o dossiê deve usar a formulação "proposta em revisão" e não "decidido".

---

## 18. Modelo de domínio

### 18.1 Objetivo

O modelo de domínio representa os conceitos essenciais da plataforma para psicólogos, suas associações e as restrições sustentadas pelos requisitos (RF-001 a RF-032), pelas regras de negócio (RN-001 a RN-014) e pelas decisões do grupo (DEC-001 a DEC-010). É um modelo **conceitual de domínio**: define o que existe, como se relaciona e sob que regras, sem fixar tecnologia de persistência, interface ou arquitetura. Sua função no dossiê é dar uma base comum e rastreável para os diagramas de classes, objetos, sequência, estados e para o modelo relacional.

### 18.2 Elementos principais

O modelo reconheceu **doze conceitos**:

| # | Conceito | Papel no domínio | Requisitos que responde |
|---|---|---|---|
| 1 | `Usuario` | Identidade e conta de acesso, com situação da conta | RF-001 a RF-006, RF-010 |
| 2 | `Perfil` | Papel de acesso do usuário | RF-001, RF-003, RF-007 |
| 3 | `Permissao` | Permissão individual agregada ao perfil | RF-001, RF-007, RF-008 |
| 4 | `Psicologo` | Cadastro profissional e suas áreas de atuação | RF-014 a RF-017, RF-030, RF-031 |
| 5 | `AreaAtuacao` | Catálogo de áreas de atuação profissional | RF-016 |
| 6 | `Paciente` | Cadastro do cliente/paciente e sua situação cadastral | RF-018 a RF-022, RF-028 |
| 7 | `Vinculo` | Autorização que habilita o profissional a atender o paciente | RF-021, RF-017, RF-030, RF-031 |
| 8 | `Agenda` | Agenda titular do profissional | RF-023, RF-024, RF-029 |
| 9 | `Disponibilidade` | Período/horário liberado pelo profissional | RF-023, RF-024 |
| 10 | `Agendamento` | Compromisso entre profissional e paciente, com situação | RF-025 a RF-030 |
| 11 | `Atendimento` | Registro conceitual da sessão realizada | RF-030, RF-031 |
| 12 | `RegistroAuditoria` | Trilha cronológica das operações relevantes | RF-013, RF-032 |

O modelo separa deliberadamente **identidade** (`Usuario`) de **autorização** (`Perfil` → `Permissao`) e de **cadastro** (`Psicologo`, `Paciente`): uma conta de acesso não é o cadastro profissional nem o cadastro do paciente. O `Atendente` foi excluído do escopo (DEC-007) e, por isso, não existe como ator nem classe.

### 18.3 Como interpretar

O diagrama do domínio (`03-uml/Dominio/diagrama-dominio.mmd`) apresenta as associações com identificadores `A01` a `A16`. Cada associação corresponde a uma relação do modelo de classes:

| ID | Associação | Leitura |
|---|---|---|
| A01 | `Usuario` — `Perfil` | Todo usuário tem um papel de acesso; o papel pode ser compartilhado por várias contas. |
| A02 | `Perfil` — `Permissao` | Um perfil agrupa permissões; a mesma permissão pode servir a vários perfis. |
| A03 | `Usuario` — `Psicologo` | A conta de acesso do profissional é distinta do cadastro profissional. |
| A04 | `Usuario` — `Paciente` | A conta de acesso do paciente é distinta do cadastro do paciente. |
| A05 | `Psicologo` — `AreaAtuacao` | O profissional atua em uma ou mais áreas catalogadas. |
| A06 / A07 | `Psicologo` / `Paciente` — `Vinculo` | O vínculo identifica **um** profissional e **um** paciente, e é a base do acesso autorizado. |
| A08 | `Psicologo` — `Agenda` | Cada profissional tem agenda própria (RN-011). |
| A09 | `Agenda` — `Disponibilidade` | Os períodos ofertados pertencem à agenda do titular. |
| A10 | `Agenda` — `Agendamento` | Os compromissos são apresentados na agenda do titular. |
| A11 / A12 | `Psicologo` / `Paciente` — `Agendamento` | Cada agendamento tem exatamente um profissional e um paciente. |
| A13 / A14 | `Psicologo` / `Paciente` — `Atendimento` | Cada atendimento tem um profissional responsável e um paciente. |
| A15 | `Agendamento` — `Atendimento` | A sessão realizada corresponde a, no máximo, um registro conceitual de atendimento. |
| A16 | `Usuario` — `RegistroAuditoria` | O registro de auditoria identifica o responsável pela operação. |

As **multiplicidades** dessas associações foram fechadas na Fase 5 (proposta do Integrante 2, aprovação do grupo pendente) e estão detalhadas na seção 19. Em todos os casos valem três invariantes: (i) um agendamento sempre pertence a uma agenda e a um vínculo válido; (ii) um agendamento cancelado não pode gerar atendimento (RN-006); (iii) toda operação administrativa relevante gera registro de auditoria com responsável, data, hora e operação (RN-009, RN-014).

### 18.4 Decisões de modelagem

| Decisão | Efeito no modelo | Base |
|---|---|---|
| Identidade separada de autorização | `Usuario`, `Perfil` e `Permissao` são três conceitos distintos, com duas associações 1:N | plano §5.2, DEC-002 (autorização por perfil e permissão) |
| Cadastros sem herança de `Usuario` | `Psicologo` e `Paciente` **não** herdam de `Usuario`; a relação é por associação | OO-01, plano §5.2 (evitar herança por permissão) |
| Vínculo como conceito de primeira classe | O acesso do profissional ao paciente depende do vínculo, não do cadastro | RN-003, RN-007, RF-017 |
| Disponibilidade como conceito da agenda | O profissional libera períodos na **própria** agenda | RN-011, RF-023 |
| Agendamento com situação | O compromisso tem estado explícito, o que sustenta a exclusividade de horário (RN-005) e a proibição de atender cancelado (RN-006) | RN-005, RN-006, RN-010 |
| Atendimento conceitual | O atendimento é um registro de ocorrência, sem conteúdo clínico | DEC-008, RF-030 |
| Auditoria como registro imutável | A trilha admite apenas acréscimos (append-only) e é identificada por responsável, data, hora e operação | RN-009, RN-014, RF-032 |
| Notificação e sessão fora do domínio | Notificação por e-mail (DEC-009) e sessão (RF-002) são responsabilidades de aplicação/fronteira, não entidades de domínio | DEC-009 (pendente), OO-16 |

### 18.5 Relação com requisitos, classes e banco

Cada um dos 32 requisitos funcionais tem ao menos um conceito do modelo associado, e cada conceito tem requisito associado — não há elemento órfão. O detalhamento completo (RF → RN/RNF → caso de uso → classe → operação → sequência → tabela → teste conceitual) está na **seção 31 (Matriz de rastreabilidade)**, em `06-rastreabilidade/matriz-rastreabilidade-fase5.md`. As tabelas correspondentes (`usuario`, `vinculo`, `agendamento`, entre outras) e as chaves estrangeiras são detalhadas na seção 26.

### 18.6 Arquivos

- Fonte editável do diagrama: `03-uml/Dominio/diagrama-dominio.mmd`
- Especificação do modelo: `03-uml/Dominio/modelo-dominio.md`
- Revisão da Fase 5: `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md`

---

## 19. Diagrama de classes

### 19.1 Identificação e objetivo

**CLS-001 — Diagrama de classes do domínio da Plataforma para Psicólogos.**
O diagrama detalha as doze classes do modelo de domínio com seus atributos, operações, visibilidades, tipos e multiplicidades, de modo a tornar o modelo executável em termos de comportamento: é ele que mostra **quem** responde por cada regra de negócio e **quem** participa de cada fluxo de caso de uso.

### 19.2 Elementos principais

- **Classes com estado:** `Usuario` (situação da conta), `Paciente` (situação cadastral), `Vinculo` (situação e temporalidade) e `Agendamento` (situação do compromisso) — cada uma com um conjunto de valores nomeados, alinhado ao dicionário de dados do banco.
- **Classes de comportamento:** `Agendamento` (ciclo de vida da sessão), `Vinculo` (autorização), `Agenda`/`Disponibilidade` (oferta de horários), `RegistroAuditoria` (trilha), `Perfil`/`Permissao` (autorização).
- **Multiplicidades fechadas** nas dezesseis associações A01–A16, mais a associação condicional entre `Disponibilidade` e `Agendamento` (reserva de slot), incluída apenas se o grupo aprovar o modelo de horário discreto.
- **Visibilidade:** atributos privados (`-`) e operações públicas (`+`), sem exposição direta de estado interno.
- **Assinaturas completas** nas operações que manejam situação e autorização, com o ator solicitante explicitado (ex.: `cancelar(solicitante: Usuario, motivo: Texto)`), para que a autorização por papel/permissão seja auditável na própria operação.

### 19.3 Como interpretar

Cada operação corresponde a um passo verificável de um caso de uso:

| Família de operações | Passos de caso de uso que executa |
|---|---|
| `Usuario.podeAutenticar`, `confereCredencial` | UC-001 — validação de conta ativa (RN-001) e de credencial protegida (RN-002) |
| `Usuario.cadastrar`, `ativar`, `bloquear`, `atualizarDadosPermitidos`, `associarPerfil` | Gerenciar usuários e permissões (RF-003 a RF-008) |
| `Psicologo.cadastrar`, `atualizarDadosProfissionais`, `associarAreaAtuacao` | Gerenciar perfil profissional (RF-014 a RF-016) |
| `Paciente.cadastrar`, `atualizarCadastro`, `alterarSituacao`, `consultarCadastroPermitido` | Gerenciar pacientes e consultar meu cadastro (RF-018 a RF-022) |
| `Vinculo.formalizar`, `encerrar`, `estaAtivo` | UC-021 — vincular e encerrar vínculo (RF-021, RN-003) |
| `Agenda.cadastrarDisponibilidade`, `consultarHorariosDisponiveis`, `consultarCompromissos` | Gerenciar agenda e consultar horários (RF-023, RF-024, RF-029) |
| `Agendamento.solicitar`, `confirmar`, `cancelar`, `podeSerCancelado`, `concluir`, `registrarFalta`, `consultarMeus` | UC-025 e UC-027 — solicitar, confirmar, cancelar e listar (RF-025 a RF-028) |
| `Atendimento.registrar` (referenciando o `Agendamento`) | Atender pacientes vinculados (RF-030, RN-006, RN-007) |
| `RegistroAuditoria.registrar`, `consultar` | Registrar e consultar auditoria (RF-013, RF-032) |

A leitura de uma operação deve responder sempre a três perguntas: **qual regra implementa** (RN), **qual requisito satisfaz** (RF) e **quem pode invocá-la** (ator solicitante + permissão). Operação sem regra correspondente não foi incluída — ver a seção de revisões do dossiê, em que as sugestões descartadas estão registradas com justificativa.

### 19.4 Decisões de modelagem

- **Sem classes onipotentes:** nenhuma classe concentra operações de domínios distintos; responsabilidades de aplicação (sessão, notificação externa, consultas agregadas de painel) ficam em serviços, e não em entidades.
- **Herança apenas quando há relação "é um" real:** nenhuma herança foi usada para representar perfis de acesso; `Administrador`, `Psicólogo` e `Paciente` são papéis de `Perfil`, não subclasses de `Usuario`.
- **Encapsulamento da situação:** a situação de `Agendamento` só muda por operações de transição controladas (`confirmar`, `cancelar`, `concluir`, `registrarFalta`); não existe operação genérica de alteração de estado.
- **Guardas explícitas:** `podeSerCancelado()` materializa a condição de cancelamento (situação atual + prazo + autorização), usada tanto pelo caso de uso quanto pela regra RN-012.
- **Auditoria por colaboração, não por dependência estrutural:** `RegistroAuditoria` não é dependência de todas as classes; o registro é disparado pelo serviço de aplicação responsável pela operação (RN-009), o que evita acoplamento estrutural indevido.

### 19.5 Relação com requisitos, classes e banco

O diagrama é a origem do mapeamento classe ↔ tabela: cada uma das doze classes corresponde a uma tabela, e as duas tabelas associativas (`perfil_permissao` e `psicologo_area_atuacao`) resolvem os N:N. As colunas de situação (`usuario.status`, `paciente.situacao_cadastral`, `vinculo.situacao`, `agendamento.status`) recebem exatamente os mesmos valores nomeados do diagrama — o que atende à exigência de que **os mesmos estados apareçam no domínio, nos casos de uso e no banco**.

### 19.6 Arquivos

- Fonte editável vigente: `03-uml/Classes/diagrama-classes.mmd` (Integrante 3)
- Especificação de classes: `03-uml/Classes/especificacao-classes.md`
- Decisões de modelagem OO: `03-uml/Classes/decisoes-modelagem.md`
- Proposta de fechamento das multiplicidades e das situações (Fase 5): `03-uml/Classes/proposta-I2-diagrama-classes-v2.mmd`

---

## 20. Diagrama de objetos

### 20.1 Identificação e objetivo

**OBJ-001 / OBJ-002 — Fotografias do modelo em uso.**
O diagrama de objetos materializa o diagrama de classes: mostra instâncias identificadas, seus valores de atributo e as ligações entre elas, em uma fotografia parcial e coerente com um cenário real. É a evidência de que o modelo funciona com dados concretos e é a referência para o teste conceitual e para o mapeamento com o banco.

### 20.2 Elementos principais

- **OBJ-001 (cadastro):** conta de acesso do profissional e do paciente, os cadastros correspondentes, o vínculo entre eles, a agenda do profissional e um período de disponibilidade liberado.
- **OBJ-002 (ciclo de agendamento):** o mesmo par profissional–paciente com o vínculo `ATIVO`, a agenda, o período ofertado, **dois agendamentos** (um `REALIZADO` e um `CANCELADO`), o `Atendimento` do primeiro, os **registros de auditoria** das duas operações e o objeto de aplicação que representa a notificação enviada.

### 20.3 Como interpretar

Cada caixa é uma **instância** (não uma classe nova) e a notação `nomeInstancia : Classe` explicita isso. As ligações reproduzem as associações A01–A16 com instâncias concretas. Três ausências são significativas e estão anotadas no diagrama:

1. o agendamento `CANCELADO` **não** possui `Atendimento` — é a verificação visual de RN-006;
2. o agendamento `REALIZADO` possui **um único** `Atendimento` — verificação de A15 e de RF-030;
3. o `Vinculo` aparece com `situacao = ATIVO` e `dataInicio`, base da temporalidade que impede acesso a paciente após o encerramento do vínculo.

Todos os valores são **fictícios** (`CONTA-FICTICIA-C01`, `FAIXA-FICTICIA 2026-11-03 14h00 a 15h00`), conforme DEC-005: nenhum nome, documento, e-mail, registro profissional ou conteúdo clínico real aparece nos artefatos.

### 20.4 Decisões de modelagem

- O diagrama **não** declara multiplicidades: a fotografia exemplifica, a cardinalidade é do diagrama de classes.
- O objeto de notificação é marcado como **objeto de aplicação**, não como classe de domínio, enquanto DEC-009 não for decidida.
- As situações mostradas são as mesmas do diagrama de classes e do dicionário de dados; se o grupo alterar os nomes, os três artefatos mudam juntos (revisão cruzada, plano §13).

### 20.5 Relação com requisitos, classes e banco

Cada objeto corresponde a uma linha (ou a um conjunto de linhas) do modelo relacional: `contaPacienteCenario` ↔ `usuario`, `vinculoCenario` ↔ `vinculo`, `agendamentoRealizadoCenario` ↔ `agendamento` com `status = 'REALIZADO'`, e assim por diante. O OBJ-002 é, na prática, o **teste conceitual** do ciclo de agendamento (CT-025 a CT-030 da matriz de rastreabilidade).

### 20.6 Arquivos

- Fontes editáveis: `03-uml/Objetos/diagrama-objetos.mmd` (OBJ-001) e `03-uml/Objetos/proposta-I2-diagrama-objetos-v2.mmd` (OBJ-002)
- Especificações: `03-uml/Objetos/especificacao-objetos.md` e `03-uml/Objetos/proposta-I2-especificacao-objetos-v2.md`

---

## 27. Princípios OO e SOLID

### 27.1 Objetivo

Esta seção evidencia, com exemplos concretos do modelo entregue, como cada princípio Orientado a Objetos e cada princípio SOLID aparece no artefato. Não é uma lista genérica: cada linha aponta classe, operação ou regra que sustenta a afirmação.

### 27.2 Princípios OO

| Princípio | Evidência no modelo |
|---|---|
| **Abstração** | As doze classes representam conceitos do domínio (agenda, vínculo, disponibilidade, agendamento), não detalhes de implementação de banco ou de interface. `Disponibilidade` existe porque o profissional **libera períodos** (RF-023); `Vinculo` existe porque o acesso profissional é **autorizado** (RN-003), e não por conveniência de armazenamento. |
| **Encapsulamento** | Atributos são privados e as situações mudam apenas por operações controladas: `Agendamento` não expõe alteração genérica de situação, só `confirmar`, `cancelar`, `concluir` e `registrarFalta`, cada uma com guarda própria. A credencial aparece apenas como `credencialProtegida`, nunca em texto puro (RN-002, RNF-001). |
| **Coesão** | Cada classe tem uma responsabilidade focada: `Agenda` organiza a oferta de horários; `Agendamento` conduz o ciclo de vida da sessão; `RegistroAuditoria` mantém a trilha; `Perfil`/`Permissao` tratam autorização. Não existe classe que agregasse cadastro, agenda e auditoria. |
| **Baixo acoplamento** | As entidades não dependem umas das outras por referência a implementações: o registro de auditoria é solicitado por um serviço de aplicação, e não por dependência estrutural de todas as classes; a notificação externa é uma interface de fronteira (DIP), não uma chamada direta a um serviço de e-mail. |
| **Herança** | Usada apenas para relação "é um" real. Nenhuma herança representa perfis de acesso: `Administrador`, `Psicólogo` e `Paciente` são instâncias de `Perfil` com permissões distintas. `Psicologo` e `Paciente` são **associações** a `Usuario`, não subclasses, porque conta de acesso e cadastro têm responsabilidades diferentes (RF-014, DEC-006). |
| **Polimorfismo** | A interface de notificação externa (`ServicoEnvioMensagem`) admite implementações distintas — serviço de e-mail, registro interno — sem alterar o domínio. Onde a variação ainda não existe no escopo, o modelo **não** a antecipa: não há polimorfismo artificial. |
| **Composição** | `Atendimento` corresponde a uma sessão agendada (A15, `1` Agendamento para `0..1` Atendimento): o registro conceitual tem ciclo de vida dependente do agendamento, e um agendamento cancelado nunca o possui (RN-006). |

### 27.3 Princípios SOLID

| Princípio | Evidência no modelo |
|---|---|
| **S — Responsabilidade Única (SRP)** | `Agendamento` controla o ciclo de vida do compromisso (solicitado → confirmado → realizado/cancelado); `RegistroAuditoria` apenas registra; `Vinculo` apenas autoriza o acesso. O envio de avisos não é responsabilidade de entidade: cabe ao serviço de notificação, o que evita a classe onipotente vedada pelo plano §5.2. |
| **O — Princípio Aberto/Fechado (OCP)** | Novos papéis de acesso entram como novos `Perfil`/`Permissao`, sem alterar `Usuario` nem as entidades de domínio. Novas áreas de atuação entram no catálogo `AreaAtuacao`. O modelo não foi projetado para "adicionar psicólogo" ou "adicionar paciente" como casos especiais dentro das classes existentes. |
| **L — Princípio de Substituição de Liskov (LSP)** | As operações que recebem o mesmo parâmetro respeitam o mesmo contrato: toda operação de estado de `Agendamento` recebe o ator solicitante e só altera a situação quando a guarda é satisfeita; qualquer implementação futura de notificação ou persistência deve obedecer ao contrato da interface correspondente. |
| **I — Princípio de Segregação de Interfaces (ISP)** | As permissões são granulares (`Permissao` por operação) e o perfil agrega apenas as que o ator efetivamente possui; a restrição de ampliar permissões além do que o administrador tem é exatamente a materialização desse princípio (RN-002, RNF-002). |
| **D — Princípio de Inversão de Dependência (DIP)** | O domínio não depende de tecnologia: o `Agendamento` não conhece e-mail, banco ou interface; a auditoria é acionada por serviço; o serviço de notificação depende de uma interface de fronteira (`ServicoEnvioMensagem`). O modelo persiste por mapeamento objeto-relacional, sem regra de negócio dentro de gatilhos de banco. |

### 27.4 Observações e limites reconhecidos

- **Composição e herança são ponto de partida, não dogma:** a associação `Agendamento`–`Atendimento` foi modelada como composição pelo ciclo de vida dependente; não se estendeu herança artificial para representar papéis de usuário.
- **Polimorfismo com escopo honesto:** onde não há variação real no escopo atual (notificação, persistência), o modelo declara **interfaces** e deixa a implementação para a arquitetura (seção 25), em vez de criar abstrações sem uso.
- **Responsabilidades de aplicação:** as operações de consulta agregada, de sessão e de notificação foram propositalmente mantidas **fora** das entidades, o que é uma decisão de coesão e de baixo acoplamento — e uma resposta direta à penalidade do plano §19 sobre modelagem sem responsabilidades claras.

### 27.5 Arquivos

- `03-uml/Classes/decisoes-modelagem.md` — decisões OO-01 a OO-16 do Integrante 3.
- `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md` — seção 9: o que **não** foi proposto e por quê (evidência de disciplina de modelagem).
- `05-qualidade/` — plano de qualidade ISO/IEC 25010 e critérios mensuráveis (seção 28 do dossiê).
