# Requisitos Não Funcionais

Os critérios abaixo descrevem qualidades e restrições da solução modelada. Quando a Fase 2 não oferece métrica numérica, ela não é inventada; o valor fica para definição na etapa de qualidade.

## 1. Segurança

### RNF-001 — Proteção de credenciais

**Categoria:** Segurança

**Descrição:** Senhas não devem ser armazenadas nem disponibilizadas em texto puro.

**Critério de verificação:** O modelo deverá representar armazenamento seguro de credenciais e não deverá conter atributo conceitual de senha em texto puro.

### RNF-002 — Autorização por perfil e permissão

**Categoria:** Segurança

**Descrição:** O acesso às funcionalidades deverá respeitar os perfis e as permissões atribuídos ao usuário.

**Critério de verificação:** Para cada RF com acesso restrito, a rastreabilidade deverá indicar ator autorizado e a revisão do Integrante 2 deverá confirmar que os casos de uso preservam essa restrição.

### RNF-003 — Bloqueio de acesso de conta

**Categoria:** Segurança

**Descrição:** Usuários bloqueados não deverão conseguir autenticar-se.

**Critério de verificação:** O fluxo de autenticação modelado deverá negar acesso a uma conta bloqueada.

### RNF-004 — Auditoria de operações relevantes

**Categoria:** Segurança

**Descrição:** Operações administrativas relevantes deverão ser auditáveis, com usuário responsável, data, hora e operação identificáveis.

**Critério de verificação:** O requisito e o modelo futuro de auditoria deverão prever os quatro elementos de identificação e manter os registros rastreáveis.

### RNF-005 — Isolamento dos dados de pacientes

**Categoria:** Segurança

**Descrição:** O sistema deverá impedir que um Cliente/Paciente acesse informações pertencentes a outros pacientes.

**Critério de verificação:** Casos de uso e critérios de aceite deverão limitar cada consulta ao identificador do próprio Cliente/Paciente.

### RNF-006 — Acesso profissional por vínculo

**Categoria:** Segurança

**Descrição:** O Psicólogo deverá acessar somente clientes/pacientes com vínculo autorizado.

**Critério de verificação:** A autorização das consultas profissionais deverá exigir vínculo entre o Psicólogo autenticado e o Cliente/Paciente consultado.

## 2. Confiabilidade

### RNF-007 — Integridade entre elementos relacionados

**Categoria:** Confiabilidade

**Descrição:** A solução modelada deverá preservar a integridade das relações entre usuários, perfis, psicólogos, clientes/pacientes, vínculos, agenda, agendamentos, atendimentos e auditoria.

**Critério de verificação:** Os modelos de domínio e persistência futuros deverão explicitar as associações e restrições necessárias para impedir referências inválidas.

## 3. Manutenibilidade

### RNF-008 — Coesão e baixo acoplamento

**Categoria:** Manutenibilidade

**Descrição:** A solução deverá favorecer alta coesão entre responsabilidades relacionadas e baixo acoplamento entre módulos.

**Critério de verificação:** Na revisão dos modelos, cada responsabilidade deverá estar atribuída a elemento do domínio coerente e dependências entre módulos deverão estar justificadas.

## 4. Flexibilidade

### RNF-009 — Evolução modular

**Categoria:** Flexibilidade

**Descrição:** A organização dos módulos deverá permitir evolução futura sem exigir reestruturação completa da solução modelada.

**Critério de verificação:** A revisão arquitetural futura deverá identificar módulos e suas dependências, explicando como mudanças localizadas podem ser acomodadas.

## 5. Desempenho

### RNF-010 — Consulta interativa de agenda

**Categoria:** Desempenho

**Descrição:** Consultas de agenda deverão possuir tempo de resposta compatível com uso interativo normal.

**Critério de verificação:** **VALOR A DEFINIR NA ETAPA DE QUALIDADE**; não há métrica de tempo ou carga definida nos documentos da Fase 2.

## 6. Rastreabilidade e Qualidade da Modelagem

### RNF-011 — Rastreabilidade entre artefatos

**Categoria:** Rastreabilidade e Qualidade da Modelagem

**Descrição:** Os artefatos deverão manter rastreabilidade entre requisitos, regras de negócio, atores, casos de uso futuros e critérios de aceite.

**Critério de verificação:** Cada RF deverá constar na matriz inicial com ator, regra relacionada quando aplicável, caso de uso futuro e critério de aceite.

### RNF-012 — Consistência terminológica e semântica

**Categoria:** Rastreabilidade e Qualidade da Modelagem

**Descrição:** Os requisitos e modelos deverão manter termos consistentes com o glossário e respeitar as decisões e fronteiras de escopo aprovadas.

**Critério de verificação:** A revisão cruzada deverá verificar nomenclatura, atores, escopo negativo, privacidade e ausência de contradição entre artefatos.
