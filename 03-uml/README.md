# UML

Esta pasta armazena os diagramas UML e seus arquivos editáveis.

## Convenção de entrega dos diagramas (Fase 5)

Todo diagrama entregue vem em **dois formatos**, com o **mesmo nome-base**:

| Formato | Arquivo | Papel | Quem abre |
|---|---|---|---|
| Mermaid (`.mmd`) | `nome.mmd` | **fonte da verdade do conteúdo**; versionável e revisável em texto | editor de texto, GitHub |
| draw.io (`.drawio.xml`) | `nome.drawio.xml` | **fonte editável gráfica** (formato oficial do repositório) | diagrams.net (web ou desktop) |
| PDF | `nome.pdf` | **exportação para leitura e impressão** (dossiê) | leitor de PDF |

Regras:

1. o `.mmd` manda no **conteúdo**; o `.drawio.xml` e o `.pdf` são o mesmo
   diagrama em formato de edição e de leitura;
2. mudou o conteúdo, mudou o `.mmd` **e** os outros dois (no diagrams.net:
   *Arquivo > Exportar como > PDF* e *Arquivo > Salvar como* `.drawio.xml`);
3. arquivo de **proposta** do Integrante 2 começa com `proposta-I2-` e **não
   substitui** o artefato oficial do Integrante 3 — convém os dois;
4. os PDF exportados usam fonte **Arial** e página com o tamanho do desenho
   (sem margem), para que o diagrama entre legível no dossiê.

## Inventário

| Diagrama | ID | Fonte (`.mmd`) | Editável (`.drawio.xml`) | PDF | Situação |
|---|---|---|---|---|---|
| Contexto | CTX-001 | — (Fase 2) | `Contexto/diagrama-contexto.drawio.xml` | `Contexto/diagrama-contexto.pdf` | reconstrução editável; o PDF original da Fase 2 (`Contexto/Diagrama de contexto fase 2.pdf`) foi preservado |
| Casos de uso | UC-001 | — (Fase 3) | `Casos de uso/Diagrama casos de uso uc001/Diagrama de casos de uso UC-001.drawio.xml` | `Casos de uso/Diagrama casos de uso uc001/Diagrama de casos de uso UC-001.pdf` | oficial do grupo |
| Domínio | CLS-DOM | `Dominio/diagrama-dominio.mmd` | `Dominio/diagrama-dominio.drawio.xml` | `Dominio/diagrama-dominio.pdf` | oficial do Integrante 3 |
| Classes | CLS-001 | `Classes/diagrama-classes.mmd` | `Classes/diagrama-classes.drawio.xml` | `Classes/diagrama-classes.pdf` | oficial do Integrante 3 (multiplicidades `PENDENTE`) |
| Classes (proposta) | CLS-001 v2 | `Classes/proposta-I2-diagrama-classes-v2.mmd` | `Classes/proposta-I2-diagrama-classes-v2.drawio.xml` | `Classes/proposta-I2-diagrama-classes-v2.pdf` | **proposta** do Integrante 2 (Fase 5) |
| Objetos | OBJ-001 | `Objetos/diagrama-objetos.mmd` | `Objetos/diagrama-objetos.drawio.xml` | `Objetos/diagrama-objetos.pdf` | oficial do Integrante 3 |
| Objetos (proposta) | OBJ-002 | `Objetos/proposta-I2-diagrama-objetos-v2.mmd` | `Objetos/proposta-I2-diagrama-objetos-v2.drawio.xml` | `Objetos/proposta-I2-diagrama-objetos-v2.pdf` | **proposta** do Integrante 2 (Fase 5) |

## Convenções de leitura

- **Multiplicidades (padrão UML):** a multiplicidade escrita **ao lado de uma
  classe** indica **quantas instâncias dessa classe** se relacionam com **uma
  instância da outra**. Exemplo: `Usuario "0..*" -- "1" Perfil` = *cada conta tem
  exatamente 1 perfil* e *cada perfil atende 0..* contas*.
- **`PENDENTE` não é multiplicidade:** é anotação editorial do Integrante 3
  sobre cardinalidade ainda não fechada. O fechamento proposto na Fase 5 está
  em `01-planejamento/revisoes/FASE5-REV-I2-revisao-modelo-dominio.md` §7.
- **Stereótipos `<<enumeration>>`:** situação de `Usuario`, `Paciente`,
  `Vinculo` e `Agendamento` — os mesmos valores do dicionário de dados.
- **Notas:** caixas amarelas à direita do diagrama, com a referência "Nota para
  `<classe>`"; elas trazem ressalvas de modelagem e não alteram o modelo.
- **Valores fictícios:** todo valor de objeto é inventado
  (`CONTA-FICTICIA-*`, `FAIXA-FICTICIA-*`), conforme DEC-005 — nenhum dado
  real de paciente.

## Pastas ainda vazias (Fases 5 e 6)

`Atividade`, `Comunicação`, `Estados` e `Sequências` aguardam os diagramas
correspondentes. `Estados` e `Sequências` dependem do fechamento das
multiplicidades e das operações propostas na Fase 5.
