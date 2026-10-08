# Dados do Censo do IBGE

## Fonte e acesso

Os dados demográficos e domiciliares são obtidos do [SIDRA/IBGE](https://sidra.ibge.gov.br/) por meio da API de agregados. A consulta é feita no nível territorial **Município** (`n6`), e os códigos retornados pelo IBGE são normalizados para seis dígitos para a integração com o SINAN.

O pipeline preserva o nome da fonte populacional e mantém versões em formato longo dos indicadores de saneamento, com dimensão, indicador e valor; em 2022, o identificador da tabela também acompanha as linhas. Isso permite rastrear cada coluna do contrato até a seleção original do SIDRA.

## População residente

### Censo 2010 — Tabela 136

A [Tabela SIDRA 136](https://sidra.ibge.gov.br/tabela/136) é denominada **População residente, por cor ou raça**. Para obter um único denominador municipal, o pipeline seleciona:

- período: 2010;
- variável 93: `População residente`, em pessoas;
- classificação 86, categoria 0: `Total` de cor ou raça;
- território: todos os municípios.

Forma conceitual da consulta:

```text
/t/136/n6/all/v/93/p/2010/c86/0
```

Essa seleção evita somar manualmente categorias de cor ou raça e fornece uma linha de população por município. O metadado da tabela a identifica como parte de **Amostra — Primeiros Resultados** e inclui a nota `Dados da Amostra`; essa procedência deve acompanhar a interpretação.

#### Resultado da carga validada

A execução de 2 de setembro de 2026 gravou a seleção da Tabela 136 em
`sinan/processados/ibge_populacao_municipio_ano.parquet`, junto das demais
fontes populacionais. O recorte `ano == 2010` apresentou:

| Controle | Resultado |
| --- | ---: |
| Linhas municipais | 5.565 |
| Chaves únicas `ano + codigo_municipio` | 5.565 |
| Códigos municipais nulos | 0 |
| Populações nulas ou não positivas | 0 |
| População residente total | 190.755.799 |

Todas as linhas recebem a fonte
`SIDRA 136 v93 c86=0 - Censo 2010`. Após a integração, o contrato
`sinan_saneamento_2010.parquet` também contém 5.565 municípios, todos com
população e saneamento correspondentes: 4.572 têm notificação agregada no
SINAN e 993 permanecem na grade com contagens e taxas iguais a zero.

A reconciliação do SINAN em 2010 parte de 1.381.254 registros brutos. Destes,
127 têm código de residência inválido e 67 notificações pertencem a 31 códigos
municipais bem formatados que não correspondem à grade censitária de 2010. Os
1.381.060 registros restantes são atribuídos ao contrato municipal; as
exclusões ficam preservadas nos arquivos de auditoria, sem associação
artificial a outro município.

### Censo 2022 — Tabela 4709

A [Tabela SIDRA 4709](https://sidra.ibge.gov.br/tabela/4709) fornece a população residente do Censo 2022. O pipeline usa a variável 93 no nível municipal como denominador dos contratos de 2022.

### Anos intercensitários — Tabela 6579

A [Tabela SIDRA 6579](https://sidra.ibge.gov.br/tabela/6579) fornece a população residente estimada, variável 9324, para os anos disponíveis do painel histórico. Estimativa e contagem censitária não são rotuladas como a mesma fonte.

## Condições domiciliares de 2010 — Tabela 3218

A [Tabela SIDRA 3218](https://sidra.ibge.gov.br/tabela/3218) é denominada **Domicílios particulares permanentes, por forma de abastecimento de água, segundo a existência de banheiro ou sanitário e esgotamento sanitário, o destino do lixo e a existência de energia elétrica**.

O pipeline usa a variável derivada 1000096, `Domicílios particulares permanentes — percentual do total geral`, e consulta uma dimensão por vez, mantendo as demais no total. Isso evita cruzamentos desnecessários entre todas as classificações.

| Dimensão | Classificação SIDRA | Indicadores preservados |
| --- | ---: | --- |
| Abastecimento de água | 61 | Rede geral; poço/nascente dentro ou fora da propriedade; rio/açude/lago/igarapé; categorias específicas de aldeia; outra forma |
| Banheiro e esgotamento | 299 | Banheiro ou sanitário com rede/fossa séptica; outros escoadouros; sem banheiro nem sanitário |
| Destino do lixo | 67 | Serviço de limpeza; caçamba de serviço de limpeza; outro destino |
| Energia elétrica | 309 | Com energia; sem energia |

O resultado é preservado em dois formatos: longo, adequado à auditoria de categorias, e largo, com uma linha por município para a integração.

## Condições domiciliares de 2022

Em 2022, as dimensões são distribuídas em três tabelas e usam a variável derivada 1000381, `Domicílios particulares permanentes ocupados — percentual do total geral`.

| Tabela | Conteúdo usado | Classificação |
| ---: | --- | ---: |
| [6803](https://sidra.ibge.gov.br/tabela/6803) | Existência de ligação à rede geral e principal forma de abastecimento de água | 1821 |
| [6805](https://sidra.ibge.gov.br/tabela/6805) | Tipo de esgotamento sanitário | 11558 |
| [6892](https://sidra.ibge.gov.br/tabela/6892) | Destino do lixo | 67 |

As categorias selecionadas representam, entre outras, rede geral e fontes alternativas de água; rede/fossas/outros destinos de esgoto e ausência de banheiro; coleta, caçamba, queima, enterramento, descarte em terreno/área pública e outros destinos do lixo.

## Grão, chave e semântica de ausência

- Grão das tabelas largas: uma linha por `ano + codigo_municipio`.
- Chave de integração: ano e código municipal normalizado.
- `0` em um indicador percentual é um valor divulgado pelo SIDRA e deve ser preservado.
- Célula nula indica ausência ou falha de correspondência; não deve ser preenchida automaticamente com zero.
- A grade completa do Censo é usada para representar municípios sem notificação registrada no SINAN, sem eliminar municípios do denominador territorial.

## Comparabilidade entre 2010 e 2022

Os contratos dos dois censos são adequados para análises paralelas, mas as colunas originais não são automaticamente equivalentes.

1. Em 2010, a Tabela 3218 se refere a **domicílios particulares permanentes**; em 2022, as tabelas selecionadas se referem a **domicílios particulares permanentes ocupados**.
2. As categorias de água, esgotamento e lixo foram alteradas e detalhadas entre os censos.
3. A divisão territorial passou de 5.565 municípios em 2010 para 5.570 em 2022.
4. A população de 2010 e a de 2022 vêm de tabelas e planos de divulgação distintos.

Antes de calcular variações 2010–2022, é necessário construir indicadores harmonizados mais amplos, registrar o mapeamento de categorias e definir como tratar as mudanças territoriais. Uma comparação direta de colunas apenas porque seus nomes se parecem pode gerar uma conclusão incorreta.

## Saídas relacionadas

- `ibge_populacao_municipio_ano.parquet`;
- `ibge_saneamento_2010.parquet` e `ibge_saneamento_2010_longo.parquet`;
- `ibge_saneamento_2022.parquet` e `ibge_saneamento_2022_longo.parquet`;
- `sinan_saneamento_2010.parquet` e `sinan_saneamento_2022.parquet`, após a integração.

O tratamento e as regras do contrato estão detalhados em [[Tratamento dos Dados]].
