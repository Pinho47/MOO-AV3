# Especificação de objetos — Plataforma para Psicólogos

**Responsável:** Integrante 3 — Modelagem Orientada a Objetos e domínio.  
**Status:** exemplo estrutural fictício e parcial, subordinado ao modelo existente. Não aprova requisitos, assinaturas ou cardinalidades pendentes.

## 1. Objetivo e base documental

Exemplificar o diagrama de classes por meio de objetos identificados e ligações concretas em uma fotografia parcial. O recorte mostra conta de acesso distinta do cadastro, vínculo entre profissional e paciente e organização de agenda/disponibilidade. Não acrescenta classes, atributos, operações, associações ou multiplicidades.

Bases inspecionadas antes da criação:

- [Modelo de domínio](../Dominio/modelo-dominio.md).
- [Diagrama de domínio](../Dominio/diagrama-dominio.mmd).
- [Diagrama de classes](../Classes/diagrama-classes.mmd).
- [Especificação de classes](../Classes/especificacao-classes.md).
- [Decisões de modelagem](../Classes/decisoes-modelagem.md).

Fontes originais: [RF](../../02-requisitos/requisitos-funcionais.md), [RN](../../02-requisitos/regras-negocio.md), [glossário](../../02-requisitos/glossario.md), [decisões oficiais](../../01-planejamento/decisoes.md) e [escopo](../../02-requisitos/escopo.md).

Os nomes diagrama-objetos.mmd e especificacao-objetos.md seguem o padrão diagrama-classes.mmd/especificacao-classes.md. A pasta Objetos não existia na inspeção inicial; não houve conflito com arquivos anteriores. O README de UML prevê diagramas e fontes editáveis, sem impor outra nomenclatura ou ferramenta.

## 2. Cenário fictício

Uma conta fictícia está associada a um cadastro profissional e outra a um cadastro de paciente. Os dois cadastros participam de um vínculo formalizado. O profissional é titular da agenda representada, que contém um período de disponibilidade ilustrativo.

É uma fotografia de objetos já existentes, não uma sequência de execução dos cadastros ou de formalização. Não há chamada de métodos, nova alteração de situação, demonstração de autenticação ou concessão de autorização.

Não são exibidos agendamento nem atendimento: o cenário não depende de estado inicial da solicitação, confirmação, cancelamento ou realização de sessão. Também não instancia RegistroAuditoria. O recorte não afirma ausência de registros de auditoria dos eventos anteriores; RF-032 e RN-009/RN-014 continuam aplicáveis às operações auditáveis fora da vista.

Perfil, Permissao e AreaAtuacao também não são projetados. Isso não significa que as contas não tenham perfis/permissões ou que o profissional não tenha áreas. O cenário é uma vista parcial, não um inventário completo de todo o sistema.

## 3. Notação e limites do Mermaid

Fonte editável: [diagrama-objetos.mmd](diagrama-objetos.mmd).

Utiliza-se a gramática classDiagram já empregada no repositório como **representação adaptada de objetos**: o identificador técnico da caixa é o nome da instância, e seu rótulo exibe nomeInstancia : Classe. Os membros exibidos são slots atributo = valor, não declarações de novos atributos ou métodos.

A palavra class na fonte é uma construção da gramática Mermaid; ela não introduz novas classes no modelo do projeto. As seis classes usadas permanecem exatamente as do modelo de classes.

Esta representação não deve ser confundida com notação UML integral de InstanceSpecification: não reproduz o sublinhado convencional do cabeçalho e não declara conformidade UML estrita. O nome/classe no rótulo, os slots e as ligações tornam explícita a leitura como instâncias. Se a avaliação exigir notação integral, essa limitação deve ser considerada antes da entrega final, sem alterar o modelo de classes.

- Cada ligação rotulada Axx é uma ocorrência da associação de mesmo ID, não uma nova associação.
- Não se desenham multiplicidades nos links de objetos. Quantidade de objetos exibidos não cria cardinalidade de classe.
- A ausência de slot/link nesta vista não significa null, inexistência ou multiplicidade zero.
- Os valores privados exibidos em documentação fictícia não mudam sua visibilidade no modelo nem autorizam exposição por uma interface.
- Todos os nomes de instância são identificadores do desenho, não atributos nome/id introduzidos nos objetos.

## 4. Objetos e slots

São sete objetos de seis classes existentes, com três slots exibidos: duas ocorrências de Usuario.identificacao e uma de Disponibilidade.periodo. Nenhum método é representado.

| Objeto | Classe | Valor exibido |
|---|---|---|
| contaProfissionalCenario | Usuario | identificacao = CONTA-FICTICIA-P01 |
| profissionalCenario | Psicologo | Nenhum atributo escalar representado. |
| contaPacienteCenario | Usuario | identificacao = CONTA-FICTICIA-C01 |
| pacienteCenario | Paciente | Nenhum atributo escalar representado. |
| vinculoCenario | Vinculo | Nenhum atributo escalar representado. |
| agendaCenario | Agenda | Nenhum atributo escalar representado. |
| disponibilidadeCenario | Disponibilidade | periodo = FAIXA-FICTICIA 2040-08-17 09h00 a 10h00 |

### contaProfissionalCenario

**Objeto:** contaProfissionalCenario  
**Classe:** Usuario  
**Papel no cenário:** Conta associada ao cadastro profissional fictício.  
**Atributos representados:** identificacao = CONTA-FICTICIA-P01  
**Relacionamentos:** A03 com profissionalCenario.  
**Fundamentação:** RF-003 e RF-014; PRE-001; classe Usuario e A03.

**Limitação:** credencialProtegida e situacaoConta não são exibidos. Perfis e permissões não são projetados nesta vista; isso não significa conta sem perfil ou sem credencial.

### profissionalCenario

**Objeto:** profissionalCenario  
**Classe:** Psicologo  
**Papel no cenário:** Cadastro do profissional titular da agenda e participante do vínculo.  
**Atributos representados:** Nenhum atributo escalar representado.  
**Relacionamentos:** A03 com contaProfissionalCenario; A06 com vinculoCenario; A08 com agendaCenario.  
**Fundamentação:** RF-014, RF-021, RF-023, RF-029; RN-011; A03/A06/A08.

**Limitação:** O modelo de classes não fixou atributos escalares para Psicologo. Não se inventam nome ou CRP. Áreas de atuação não são exibidas; sua ausência na vista não significa quantidade zero.

### contaPacienteCenario

**Objeto:** contaPacienteCenario  
**Classe:** Usuario  
**Papel no cenário:** Conta associada ao cadastro do paciente fictício.  
**Atributos representados:** identificacao = CONTA-FICTICIA-C01  
**Relacionamentos:** A04 com pacienteCenario.  
**Fundamentação:** RF-003 e RF-018; DEC-006; PRE-001; A04.

**Limitação:** Não é conta autenticada em operação. Credencial, situação e associações de perfil/permissão foram omitidas como recorte, não como inexistentes.

### pacienteCenario

**Objeto:** pacienteCenario  
**Classe:** Paciente  
**Papel no cenário:** Cadastro do paciente participante do vínculo.  
**Atributos representados:** Nenhum atributo escalar representado.  
**Relacionamentos:** A04 com contaPacienteCenario; A07 com vinculoCenario.  
**Fundamentação:** RF-018 e RF-021; DEC-006; A04/A07.

**Limitação:** situacaoCadastral existe no modelo, mas não é exibida porque o vocabulário de estados está pendente. Não se inventam nome, documento, contato ou histórico clínico.

### vinculoCenario

**Objeto:** vinculoCenario  
**Classe:** Vinculo  
**Papel no cenário:** Relação formal já estabelecida entre os dois cadastros fictícios.  
**Atributos representados:** Nenhum atributo escalar representado.  
**Relacionamentos:** A06 com profissionalCenario; A07 com pacienteCenario.  
**Fundamentação:** RF-021; glossário de Vínculo; RN-003; A06/A07.

**Limitação:** Não há slot ativo, autorizado ou data de início. Não se executa estaAutorizado() nem se afirma resultado de verificação. A fotografia não descreve formalização, encerramento ou autorização de consulta.

### agendaCenario

**Objeto:** agendaCenario  
**Classe:** Agenda  
**Papel no cenário:** Agenda profissional cujo titular é profissionalCenario.  
**Atributos representados:** Nenhum atributo escalar representado.  
**Relacionamentos:** A08 com profissionalCenario; A09 com disponibilidadeCenario.  
**Fundamentação:** RF-023 e RF-029; RN-011; A08/A09.

**Limitação:** Exibir uma agenda não fixa o número de agendas por psicólogo. Compromissos não são exibidos, sem afirmar que a agenda inteira está vazia.

### disponibilidadeCenario

**Objeto:** disponibilidadeCenario  
**Classe:** Disponibilidade  
**Papel no cenário:** Período oferecido na agenda do profissional fictício.  
**Atributos representados:** periodo = FAIXA-FICTICIA 2040-08-17 09h00 a 10h00  
**Relacionamentos:** A09 com agendaCenario.  
**Fundamentação:** RF-023; RN-011; glossário de Disponibilidade; atributo periodo e A09.

**Limitação:** A faixa é apenas uma apresentação textual fictícia do slot. Não é tipo Texto aprovado, formato de armazenamento, duração de sessão ou regra de recorrência. Não equivale a reserva nem comprova horário livre.

## 5. Valores ilustrativos e ausência de dados reais

| Slot | Valor ilustrativo | Limitação |
|---|---|---|
| contaProfissionalCenario.identificacao | CONTA-FICTICIA-P01 | Identificador arbitrário criado apenas para este desenho; não é login, e-mail, chave técnica ou formato aprovado. |
| contaPacienteCenario.identificacao | CONTA-FICTICIA-C01 | Identificador arbitrário distinto do anterior, sem correspondência com pessoa real. |
| disponibilidadeCenario.periodo | FAIXA-FICTICIA 2040-08-17 09h00 a 10h00 | Data e faixa totalmente ilustrativas, sem vínculo com agenda real. Uma faixa de uma hora neste exemplo não define duração de atendimento, granularidade, formato, fuso, recorrência ou intervalo padrão. |

PC-01 e PC-07 continuam abertas: apresentação textual de um valor não resolve seu tipo ou formato. O marcador FAIXA-FICTICIA integra somente a ilustração do slot, não é um novo campo ou valor de enumeração.

Não são usados nomes de pessoas, nomes de integrantes, CPF, e-mail, CRP, credenciais ou informações pessoais reais. Nenhuma conta é apresentada como ativa ou bloqueada; nenhum estado cadastral do paciente ou do vínculo foi criado.

## 6. Ligações concretas e rastreabilidade

| Associação existente | Ligação nesta fotografia | Fonte e interpretação |
|---|---|---|
| A03 — Usuario–Psicologo | contaProfissionalCenario — profissionalCenario | RF-014 distingue conta de cadastro profissional. Não estabelece limite de contas/cadastros. |
| A04 — Usuario–Paciente | contaPacienteCenario — pacienteCenario | RF-018 depende de cadastro de usuário; DEC-006 confirma acesso do paciente. Não define obrigatoriedade temporal ou relação global 1:1. |
| A06 — Psicologo–Vinculo | profissionalCenario — vinculoCenario | RF-021; glossário. Este vínculo tem o profissional indicado. |
| A07 — Paciente–Vinculo | pacienteCenario — vinculoCenario | RF-021; glossário. O mesmo vínculo tem o paciente indicado. |
| A08 — Psicologo–Agenda | profissionalCenario — agendaCenario | RF-023, RF-029 e RN-011; titularidade conforme interpretação já presente no domínio. |
| A09 — Agenda–Disponibilidade | agendaCenario — disponibilidadeCenario | RF-023 e RN-011; o período pertence à agenda do profissional representado. |

Nenhuma ligação direta Psicologo–Paciente ou Paciente–Agenda foi acrescentada. O par profissional/paciente é expresso pelas duas ligações ao objeto Vinculo, conforme o modelo existente.

Os extremos 1 já existentes são respeitados neste recorte: um profissional e um paciente para vinculoCenario, um titular para agendaCenario e uma agenda para disponibilidadeCenario. As quantidades nos extremos PENDENTE não são determinadas pela fotografia.

A03 e A04 apresentam uma ocorrência cada, sem declarar que somente uma seja permitida. A08/A09 continuam com a necessidade de validação explícita do grupo registrada em PC-09. A fotografia apenas exemplifica a interpretação atual; não a aprova definitivamente.

## 7. Regras consideradas e limites de demonstração

- **RN-002 / RNF-001:** não há senha em texto puro, nem credencial ilustrada.
- **RN-003 / RNF-006:** o vínculo entre os cadastros aparece estruturalmente. Não se executa consulta nem se afirma que a presença do link basta para autorizar acesso.
- **RN-004 / RNF-005 / DEC-006:** cadastro e conta do paciente são distintos; a vista acadêmica não é uma tela de autoatendimento nem concede acesso aos dados de terceiros.
- **RN-011:** disponibilidade liga-se à agenda do profissional titular representado.
- **RN-005 e RN-010:** sem agendamento ou solicitação no recorte, não se testa conflito nem se conclui que o período está livre. A disponibilidade deve ser confrontada com compromissos fora desta vista.
- **RF-024 / CA-RF-024:** não se demonstra consulta de horários ou reserva.
- **RN-006, RN-007, RN-012 e RN-013:** não se exercitam cancelamento, atendimento ou confirmação; seus contratos e pendências permanecem intactos.
- **RN-009 e RN-014:** a omissão de eventos da vista não dispensa auditoria ou permite editar registros; não há transição sendo representada.
- **RNF-007 e RNF-012:** cada tipo e link do recorte corresponde aos modelos e à terminologia existentes.
- **DEC-005:** todos os identificadores e valores são fictícios.

Este diagrama demonstra compatibilidade estrutural de um recorte com o conhecimento atual; não comprova regras executáveis, autorização, cronologia de eventos ou ausência de conflitos no sistema inteiro.

## 8. Pendências preservadas

| Pendência existente em Classes | Tratamento no exemplo |
|---|---|
| PC-01 | identificacao tem apenas valor ilustrativo; tipo, geração e formato não fixados. Credencial omitida. |
| PC-02 e PC-05 | Situação de conta e situação cadastral não exibidas; nenhuma enumeração ou transição criada. |
| PC-03 e PC-04 | Perfis, permissões, áreas e campos profissionais não detalhados; não se conclui que estejam ausentes do sistema. |
| PC-06 | Nenhum estado de vínculo, algoritmo de autorização, resultado booleano ou encerramento. Formalização é contexto da fotografia, não novo fluxo aprovado. |
| PC-07 | periodo ilustrativo, sem regra temporal aprovada. Agendamento, solicitação, confirmação e cancelamento fora do recorte. |
| PC-08 | Atendimento e conteúdo clínico não projetados. |
| PC-09 | Nenhuma cardinalidade nova. Uma ocorrência desenhada não fecha multiplicidade pendente; A08/A09 continuam sujeitas à validação. |
| PC-10 | Sem evento de auditoria ilustrado; vocabulário de operações, precisão e eventos adicionais não definidos. |
| PC-11 | Inovação e serviço externo não incluídos. |
| PC-12 | Sem operações, assinaturas, colaboradores ou mecanismos novos. |

## 9. Como a fotografia exemplifica o modelo de classes

Os rótulos separam identidade da instância e classe. As duas contas demonstram que Usuario não é o próprio cadastro profissional/paciente. O objeto Vinculo concretiza as associações A06 e A07, sem criar uma associação direta entre seus participantes. Agenda e Disponibilidade concretizam a organização prevista em A08/A09.

Atribuir valores fictícios a identificacao e periodo ilustra slots dos atributos já modelados. A ausência de slots nos objetos cujas classes não possuem atributos escalares definidos evita inventar dados. Omitir situacaoCadastral e situacaoConta mantém os estados pendentes.

O recorte não exige instanciar as 12 classes ou fornecer todos os valores. Sua compatibilidade final continua condicionada às definições futuras dos modelos de origem.

## 10. Revisão e validação

Critérios de revisão deste artefato:

1. Sete objetos tipados somente por seis classes existentes: Usuario, Psicologo, Paciente, Vinculo, Agenda e Disponibilidade.
2. Três slots, usando exclusivamente identificacao e periodo das respectivas classes.
3. Seis links, correspondentes a A03, A04, A06, A07, A08 e A09, sem novas associações.
4. Nenhuma operação, estado ou multiplicidade acrescentada.
5. Somente dados ilustrativos, com limites de formato/tipo explícitos.
6. Fontes de Dominio e Classes mantidas em leitura.
7. Gramática Mermaid verificável pelo parser local sem instalar dependências. Validação gramatical não equivale a renderização visual nem conformidade UML integral.

A revisão do grupo permanece necessária, inclusive quanto à aceitação da notação adaptada de objetos.
