# Documento de Visão

## 1. Contexto

Atualmente, nos processos de clínicas e consultórios de Psicologia, os cadastros de profissionais e pacientes encontram-se frequentemente distribuídos em diferentes ferramentas manuais, planilhas ou sistemas desconectados. O gerenciamento de permissões de acesso não é centralizado, as agendas profissionais não operam de forma integrada e as informações operacionais e administrativas permanecem fragmentadas. 

Esse cenário gera relevante dificuldade no controle de quem pode visualizar e manipular cada dado, comprometendo o sigilo e a eficiência operacional. Diante dessa realidade, a organização necessita de uma modelagem orientada a objetos completa, robusta e rastreável, capaz de mapear integralmente o domínio antes de uma futura implementação em software.

## 2. Situação-problema

A ausência de integração e centralização gera diversos gargalos organizacionais e técnicos:
- **Ausência de integração:** dispersão de dados cadastrais, horários e histórico entre canais distintos;
- **Dificuldade no controle de permissões:** ausência de políticas claras e centralizadas de segurança e privacidade da informação;
- **Falta de visão clara do domínio:** carência de representação formal e consistente dos relacionamentos entre usuários, perfis, psicólogos, pacientes, horários e atendimentos;
- **Insegurança da informação:** risco de vazamento de dados sensíveis e acessos não autorizados por falta de barreiras granulares;
- **Carência documental:** necessidade de uma especificação técnica formal e fundamentada em padrões de engenharia de software para subsidiar de maneira confiável a futura fase de implementação.

## 3. Objetivo geral

Modelar uma plataforma integrada, flexível e segura destinada à gestão administrativa e profissional de psicólogos, permitindo representar usuários, permissões, profissionais, clientes/pacientes, agenda, atendimentos e auditoria de forma consistente e rastreável.

## 4. Objetivos específicos

- Centralizar e organizar os cadastros de usuários, profissionais e clientes/pacientes;
- Controlar o acesso à plataforma por meio de usuários, perfis e permissões granulares;
- Organizar os dados dos psicólogos, incluindo a associação a uma ou mais áreas de atuação;
- Administrar clientes/pacientes e representar de forma consistente os vínculos autorizados entre psicólogos e pacientes;
- Gerenciar a disponibilidade de horários dos profissionais e todo o ciclo de agendamentos;
- Permitir o autoatendimento limitado e autenticado do cliente/paciente;
- Representar conceitualmente os atendimentos e seus históricos de forma segura e contextualizada;
- Prover rastreabilidade e controle operacional por meio de um módulo de auditoria;
- Produzir um conjunto completo de artefatos de modelagem orientada a objetos, persistência e qualidade para orientar a futura implementação da plataforma.

## 5. Público e usuários da plataforma

A plataforma é direcionada a:
- **Administradores:** responsáveis pela gestão de acessos, cadastros centrais, parametrizações e auditoria da clínica;
- **Psicólogos:** profissionais que gerenciam sua disponibilidade, consultam suas agendas, atendem pacientes vinculados e registram conceitualmente as sessões;
- **Clientes/Pacientes:** usuários que utilizam o sistema com escopo restrito para consulta de disponibilidade, agendamentos e acompanhamento de suas próprias solicitações;
- **Gestores e Organização:** interessados na governança operacional e conformidade dos processos;
- **Equipe Técnica e de Desenvolvimento:** engenheiros de software e modeladores que utilizarão este dossiê como especificação técnica formal para implementação.

## 6. Visão geral da solução

A solução concebida consiste em uma plataforma estruturada sob o paradigma de Orientação a Objetos, ancorada em arquitetura modular e persistência em banco de dados relacional. 

A plataforma contemplará módulos essenciais integrados:
1. **Identidade e Acesso:** autenticação segura, controle de perfis e permissões granulares;
2. **Administração:** gestão de entidades, vínculos e parâmetros do sistema;
3. **Psicólogos:** gerenciamento de perfil profissional, áreas de atuação e disponibilidade de horários;
4. **Clientes/Pacientes:** cadastros, autoatendimento focado em agendamentos e controle rigoroso de privacidade;
5. **Agenda:** gestão de faixas de disponibilidade, reservas, confirmações e cancelamentos;
6. **Atendimento:** registro conceitual das sessões vinculadas estritamente a profissional e paciente autorizados;
7. **Auditoria:** rastreamento contínuo de operações sensíveis para garantia de segurança e compliance.

Toda a concepção prioriza o isolamento de dados sensíveis (sem exposição de credenciais e sem acesso cruzado entre pacientes), além de manter estrita rastreabilidade entre requisitos e modelos.
