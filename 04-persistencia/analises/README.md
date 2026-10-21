# Análises de Persistência e Arquitetura — Integrante 4

**Responsável:** Integrante 4 — Caio Cunha (Persistência e arquitetura)
**Escopo:** revisões e registros atribuídos ao Integrante 4 no Plano de Execução da AV3 (Fases 4 a 9).
**SGBD de referência:** relacional (DEC-003).

## Objetivo

Esta pasta reúne as análises produzidas pelo Integrante 4 **antes** da elaboração do DER, do modelo relacional, do dicionário de dados e do mapeamento objeto-relacional, que correspondem à Fase 7 do plano.

Cada documento responde às tarefas atribuídas ao Integrante 4 em uma fase específica e funciona como insumo direto para a Fase 7. Nenhum deles é o DER: são revisões, classificações e registros de decisão que antecedem a modelação.

## Documentos

| Documento | Fase do plano | Tarefas do plano cobertas | Situação |
|---|---|---|---|
| [Revisão de dados por caso de uso](fase-04-revisao-dados-por-caso-de-uso.md) | Fase 4 — Casos de uso e especificações | Identificar em cada caso de uso quais informações precisam ser consultadas, criadas ou alteradas; marcar integrações externas a considerar em componentes e implantação; registrar regras de persistência implícitas a formalizar como RN ou RNF; revisar os casos que envolvem auditoria, autenticação, autorização e dados sensíveis. | **Provisória** — depende das especificações textuais dos casos de uso (Integrante 2) |
| [Viabilidade de persistência do modelo de domínio](fase-05-viabilidade-persistencia-dominio.md) | Fase 5 — Modelo de domínio, classes e objetos | Revisar multiplicidades e associações pensando na viabilidade de persistência; identificar quais classes serão persistentes e quais são apenas serviços/estruturas de apoio; marcar N:N, composição e herança que exigirão estratégia de mapeamento; apontar inconsistências entre multiplicidade de domínio e regras de negócio antes do DER. | **Provisória** — depende do modelo de classes aprovado (Integrante 3) |
| [Impacto comportamental sobre a persistência](fase-06-impacto-comportamental-persistencia.md) | Fase 6 — Diagramas comportamentais | Revisar chamadas a repositórios, serviços externos e persistência para evitar dependências incoerentes; identificar quais mensagens exigirão suporte arquitetural ou transacional posteriormente; verificar se alterações de estado precisam de campos, histórico ou auditoria na persistência; registrar impactos do comportamento no DER e nos componentes planejados. | **Provisória** — depende dos diagramas comportamentais (Integrante 2) |

### Fases ainda não executadas

| Fase | Tarefas do Integrante 4 | Artefato previsto |
|---|---|---|
| Fase 7 — Arquitetura e persistência | Diagrama de pacotes, de componentes e de implantação; DER lógico; modelo relacional com PK/FK/UNIQUE/NOT NULL/domínios; resolução de N:N; integridade referencial e estratégia de exclusão/inativação; dicionário de dados; mapeamento objeto-relacional; justificativa do nível de normalização; DDL MySQL opcional | `04-persistencia/` e `03-uml/` (Pacotes, Componentes, Implantação) |
| Fase 8 — Qualidade e rastreabilidade | Revisar PK, FK, UNIQUE, NOT NULL, N:N, normalização, integridade referencial e exclusão/inativação; verificar pacotes, componentes, interfaces e implantação; confirmar dicionário e mapeamento; fornecer evidências técnicas para segurança, compatibilidade, confiabilidade e manutenibilidade | `fase-08-verificacao-qualidade-persistencia.md` |
| Fase 9 — Consolidação e pitch | Versões finais de DER e modelo relacional; conferência de dicionário, mapeamento e DDL; pontos técnicos de arquitetura e persistência para o pitch | `fase-09-encerramento-e-pitch.md` |

## Convenções

- Identificadores de RF, RNF, RN, CA, UC, SEQ, EST e DEC seguem o padrão definido no Plano de Execução.
- `RNF-013` a `RNF-020` e `RN-015` a `RN-019` são **propostas técnicas deste Integrante 4**. A incorporação ao conjunto oficial de requisitos cabe ao Integrante 1 (coordenação e requisitos); até lá, valem como proposta rastreável.
- `UC-FUT-*` são identificadores de trabalho herdados de `02-requisitos/rastreabilidade-inicial.md`. Os casos de uso definitivos pertencem ao Integrante 2 e ainda não foram produzidos.
- `INC-01` a `INC-13` são inconsistências detectadas entre multiplicidade de domínio, regras de negócio e comportamento; `P-01` a `P-19` são pendências que exigem decisão do grupo.
- `DEP-01` a `DEP-06` são dependências incoerentes entre comportamento e persistência; `TRS-01` a `TRS-06` são exigências transacionais; `ARQ-01` a `ARQ-08` são exigências arquiteturais; `IMP-DER-01` a `IMP-DER-10` e `IMP-CMP-01` a `IMP-CMP-09` são os impactos registrados sobre o DER e sobre os componentes planejados.
- Documentos de outras áreas **não são alterados** por este Integrante. Qualquer alteração proposta é registrada como proposta explícita e endereçada ao responsável.
- Nenhum exemplo deste conjunto utiliza dados reais de pacientes (DEC-005).

## Situação dos insumos

| Insumo | Responsável | Situação |
|---|---|---|
| RF-001..RF-032, RNF-001..RNF-012, RN-001..RN-014 | Integrante 1 | Concluído (Fase 3) |
| Glossário, visão, escopo, critérios de aceite | Integrante 1 | Concluído (Fases 2 e 3) |
| Diagrama de contexto e interações por ator | Integrante 2 | Concluído (Fase 2) |
| Casos de uso e especificações textuais | Integrante 2 | **Em produção** — a análise da Fase 4 é preliminar e revisável |
| Modelo de classes e de objetos | Integrante 3 | **Em produção** — a análise da Fase 5 é preliminar e revisável |
| Diagramas comportamentais (sequências, estados, atividades, comunicação) | Integrante 2 | **Em produção** — a análise da Fase 6 foi conduzida sobre o plano §5.3/§5.4 e deve ser reexecutada contra os diagramas aprovados |

As análises das Fases 4, 5 e 6 serão revisadas assim que os artefatos dos Integrantes 2 e 3 forem aprovados, conforme a regra de sincronização do plano. O walkthrough da Fase 6 ("comparar sequência × operações de classes e estados × atributo/status persistível") deve usar a lista `DEP-*` e `IMP-DER-*` deste documento como roteiro.
