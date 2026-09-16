# Stakeholders e Atores

## 1. Stakeholders

Os principais stakeholders (partes interessadas) envolvidos direta ou indiretamente com a plataforma são:

- **Organização responsável pela plataforma:** entidade ou consultório que define os direcionamentos estratégicos e gerencia a operação do sistema;
- **Administradores:** profissionais responsáveis pela parametrização, governança dos acessos, cadastros e conformidade regulatória;
- **Psicólogos:** profissionais especializados que utilizam o sistema para gerir suas agendas, disponibilidade e o registro conceitual de seus atendimentos;
- **Clientes/Pacientes:** usuários que buscam serviços de psicologia e utilizam o autoatendimento da plataforma de forma prática e segura;
- **Equipe de Engenharia / Futura Implementação:** desenvolvedores, arquitetos e engenheiros de software que utilizarão as especificações de modelagem da AV3 como guia de construção;
- **Equipe de Suporte e Manutenção Futura:** responsáveis técnicos pela estabilidade, monitoramento e continuidade operacional do ambiente de software.

## 2. Atores confirmados

O sistema possui três atores humanos primários confirmados:

1. **Administrador**
2. **Psicólogo**
3. **Cliente/Paciente**

## 3. Responsabilidades por ator

### 3.1 Administrador
- Gerenciar usuários do sistema (criação, edição, inativação, redefinição de acesso);
- Gerenciar perfis e matriz de permissões;
- Gerenciar os cadastros centrais de psicólogos;
- Gerenciar os cadastros centrais de clientes/pacientes;
- Consultar registros e relatórios de auditoria;
- Executar operações administrativas autorizadas e parametrizar o sistema.

### 3.2 Psicólogo
- Gerenciar as informações de seu próprio perfil profissional;
- Associar-se e manter suas áreas de atuação profissional;
- Consultar a relação de clientes/pacientes aos quais possui vínculo formalizado;
- Cadastrar e gerenciar seus períodos e horários de disponibilidade na agenda;
- Consultar sua agenda de sessões e horários marcados;
- Executar funções operacionais e profissionais autorizadas;
- Registrar conceitualmente as sessões de atendimento realizadas;
- Consultar o histórico permitido dos atendimentos executados.

### 3.3 Cliente/Paciente
- Realizar login com credenciais próprias em interface dedicada de autoatendimento;
- Consultar faixas de horários disponíveis do psicólogo vinculado;
- Solicitar agendamentos de sessões;
- Consultar o status e o histórico dos próprios agendamentos;
- Cancelar agendamentos próprios, em conformidade com as regras de antecedência permitidas;
- Consultar unicamente as informações e dados disponibilizados ao seu próprio usuário.

## 4. Limitações de acesso

### 4.1 Administrador
- **Privacidade de credenciais:** não possui acesso à visualização de senhas de usuários em texto puro;
- **Conformidade compulsória:** todas as operações administrativas devem passar por verificação de autorização e são compulsoriamente registradas no módulo de auditoria.

### 4.2 Psicólogo
- **Isolamento de vínculos:** não possui acesso irrestrito ao cadastro global de pacientes, podendo visualizar exclusivamente dados dos clientes/pacientes com quem mantém vínculo autorizado;
- **Segregação administrativa:** não possui permissão para acessar funções administrativas, gerenciar outros usuários ou alterar permissões do sistema.

### 4.3 Cliente/Paciente
- **Vedação administrativa:** não pode administrar outros pacientes, psicólogos ou usuários da plataforma;
- **Restrição de segurança:** não pode acessar nem manipular perfis, permissões ou logs de auditoria;
- **Privacidade estrita:** não possui acesso a dados de outros pacientes sob nenhuma circunstância;
- **Sigilo profissional:** não possui acesso a notas técnicas ou registros profissionais restritos dos psicólogos.

## 5. Atores não incluídos

### 5.1 Atendente
- **Situação:** `NÃO INCLUÍDO NO ESCOPO ATUAL`
- **Justificativa:** Trata-se de um papel opcional. O grupo avaliou que a inclusão do atendente neste momento aumentaria desnecessariamente o volume de requisitos, casos de uso, permissões e matriz de rastreabilidade, sem ganho representativo para o escopo pedagógico da AV3. Caso surja necessidade futura, uma decisão formal deverá ser registrada antes de sua incorporação.

### 5.2 Elementos que NÃO são atores (Diretriz de Modelagem)
Para fins de consistência e apoio ao **Integrante 2 (Casos de Uso e Comportamento)**, destaca-se que os seguintes elementos **não devem** ser modelados como atores:
- Banco de dados relacional;
- Módulos internos da plataforma;
- Tabelas, entidades ou classes;
- Componentes de infraestrutura ou serviços internos.

## 6. Decisões pendentes

### 6.1 Ator Externo: Serviço Externo de E-mail/Notificação
- **Situação:** `PENDENTE DE DECISÃO DO GRUPO`
- **Descrição:** Caso o grupo decida que a plataforma emitirá confirmações e avisos externos de agendamento por meio de terceiros, o *Serviço Externo de Notificação* atuará como um ator externo de suporte. Caso contrário, as notificações serão restritas ao ambiente interno da aplicação.

---

### Nota de Integração — Alinhamento com o Integrante 2

Para a elaboração do **Diagrama de Contexto**, o Integrante 2 deve considerar a seguinte fronteira:

```text
       Administrador
             |
             |
Psicólogo --- [ Plataforma para Psicólogos ] --- Cliente/Paciente
             |
             |
      Serviço Externo
(E-mail/Notificação - se confirmado)
```
