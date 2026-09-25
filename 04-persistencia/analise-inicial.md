# Análise Inicial — Persistência e Arquitetura (Integrante 4 — Fase 1)

## 1. Papel do Integrante 4
Responsável por Persistência e Arquitetura na Plataforma para Psicólogos (AV3 — MOO).
Recuperação da Fase 1 (01/09/2026 a 07/09/2026) realizada posteriormente à entrada no grupo. Sem registro retroativo de participação.

## 2. Artefatos futuros sob sua responsabilidade
- DER lógico (DER-001)
- Modelo relacional com PK, FK, UNIQUE, NOT NULL
- Dicionário de dados
- Mapeamento classe <-> tabela e atributo <-> coluna
- Tratamento de N:N, herança, composição
- Integridade referencial e estratégia de exclusão/inativação
- Normalização justificada
- Diagrama de pacotes (PAC-001), componentes (CMP-001), implantação (IMP-001)
- DDL MySQL opcional

Nenhum artefato definitivo criado nesta fase, conforme plano.

## 3. Dependências com os outros integrantes
- Integrante 1: requisitos, escopo, regras, decisões, baseline RF/RNF/RN
- Integrante 2: casos de uso e fluxos
- Integrante 3: classes, associações, multiplicidades
- Integrante 5: critérios de qualidade ISO/IEC 25010
- Integrante 6: integração documental e repositório

## 4. Pastas e documentos que servirão como entrada
- `README.md`
- `01-planejamento/cronograma.md`, `equipe.md`, `decisoes.md`
- `02-requisitos/` (visão, escopo, glossário, RF/RNF/RN futuros)
- `04-persistencia/` — área própria do Integrante 4
- Estrutura de pastas proposta na Fase 1:
  - `01_DER`, `02_Modelo_Relacional`, `03_Dicionario_Dados`, `04_Objeto_Relacional`
  - `05_Arquitetura_Pacotes`, `06_Arquitetura_Componentes`, `07_Implantacao`
  - `08_Decisoes_Tecnicas`, `09_Exportacoes`

## 5. Ferramentas e formatos
- DER, pacotes, componentes, implantação: diagrams.net (draw.io), `.drawio`, exportação PDF/PNG
- Dicionário e modelo relacional preliminar: planilha `.xlsx`, exportação PDF

## 6. Decisões técnicas preliminares
- DEC-001 (Proposta): usar banco relacional como referência — exigência de DER, PK, FK e integridade.
- DEC-002 (Adotada): não usar dados reais de pacientes.
- DEC-003 (Proposta): separar persistência das regras de negócio.
- DEC-004 (Proposta): registrar auditoria para operações críticas.
- DEC-005 (Adotada): não definir tabelas definitivas na Fase 1.

## 7. Riscos iniciais de consistência
- DER divergente das classes futuras
- Cardinalidades sem validação do domínio
- Dependências arquiteturais não controladas
- Elementos órfãos sem rastreabilidade

## 8. Situação atual
- Acesso ao repositório `Pinho47/MOO-AV3` (branch `master`) a confirmar pelo grupo
- Planejamento lido, função compreendida, estrutura localizada
- Pronto para Fase 2 (visão/escopo) e Fase 3 (requisitos)
