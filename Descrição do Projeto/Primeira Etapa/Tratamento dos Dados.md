# Tratamento dos dados

## Escopo desta etapa

Nesta etapa, o contrato analítico integra as notificações de dengue do SINAN com população residente e indicadores domiciliares do Censo do IBGE. O grão final é **uma linha por município e ano** (`codigo_municipio`, `ano`).

Os dados climáticos não fazem parte deste contrato no momento. Sua coleta e seu tratamento estão sendo conduzidos em outro repositório; por isso, nenhuma variável climática deve ser presumida nas bases descritas aqui. Ver [[Dados do Clima no Earth Engine]].

## SINAN

### Leitura e harmonização

- Foram carregados 24 arquivos anuais de dengue, de 2000 a 2023, em formato Parquet.
- A leitura é feita com Polars em modo lazy/streaming, selecionando apenas as colunas necessárias antes das agregações.
- Diferenças de esquema entre períodos são harmonizadas, principalmente nas variáveis de classificação final e evolução do caso.
- Datas e códigos municipais inválidos são contabilizados na auditoria, em vez de desaparecerem silenciosamente.
- Campos categóricos vazios são representados por `nao_preenchido`, separadamente de respostas originalmente codificadas como `ignorado`.

### Município de residência

A agregação analítica usa `ID_MN_RESI`, isto é, o **município de residência** da pessoa notificada. Essa escolha é coerente com o objetivo de relacionar os casos à população e às condições domiciliares às quais ela está exposta.

O campo `ID_MUNICIP`, município em que ocorreu a notificação, não é descartado metodologicamente: ele é tratado como dimensão de auditoria. A comparação entre residência e notificação permite quantificar registros sem código válido e casos em que os dois municípios diferem. Assim, uma diferença de local não é confundida com erro de integração.

Os códigos territoriais são normalizados para os seis primeiros dígitos do código municipal do IBGE, removendo o dígito verificador quando a fonte traz sete dígitos. Valores fora dos formatos esperados ficam nulos e entram nos relatórios de qualidade.

### Medidas agregadas

Para cada município-ano são calculados, no mínimo:

- total de notificações;
- total de casos classificados como confirmados;
- total de registros com data de notificação inválida;
- `tem_notificacao_registrada`, indicador de existência de linha agregada do SINAN para o município-ano;
- `situacao_registro_sinan`, rótulo que mantém explícita a distinção entre município com e sem notificação registrada.

As tabelas auxiliares da exploração preservam distribuições anuais de classificação, raça/cor e evolução, além da série mensal de notificações e casos confirmados.

## IBGE/SIDRA

As fontes e seleções completas estão em [[Dados do Censo do IBGE]]. Para a integração principal foram utilizadas:

- **Tabela 136, Censo 2010:** população residente total por município, variável 93 e categoria total da classificação de cor ou raça (`c86/0`);
- **Tabela 3218, Censo 2010:** percentuais de domicílios particulares permanentes segundo abastecimento de água, banheiro/esgotamento, destino do lixo e energia elétrica;
- **Tabela 4709, Censo 2022:** população residente total por município;
- **Tabelas 6803, 6805 e 6892, Censo 2022:** percentuais de domicílios particulares permanentes ocupados referentes, respectivamente, a abastecimento de água, esgotamento sanitário e destino do lixo;
- **Tabela 6579:** estimativas populacionais para anos intercensitários disponíveis no painel histórico.

As respostas da API SIDRA são normalizadas para as colunas `ano`, `codigo_municipio`, `municipio` e `valor`. As categorias selecionadas são renomeadas para indicadores descritivos e também preservadas em formato longo para rastreabilidade.

## Construção do contrato município–ano

### Grade territorial e ausência de notificações

A base do contrato censitário é a grade completa de municípios fornecida pelo IBGE em cada ano, e não apenas os municípios encontrados no SINAN. Em seguida, as contagens agregadas do SINAN são associadas por `ano` e `codigo_municipio`.

Esta regra permite distinguir três situações:

1. **Município válido sem linha agregada no SINAN:** as contagens de notificações e confirmados recebem zero, `tem_notificacao_registrada` é falso e `situacao_registro_sinan` registra a ausência de notificação.
2. **Município com uma ou mais notificações:** as contagens observadas são mantidas, `tem_notificacao_registrada` é verdadeiro e a situação registra a presença de notificação.
3. **Notificação com código de residência ausente ou inválido:** o registro entra na auditoria do SINAN, mas não é atribuído artificialmente a nenhum município.

Portanto, **zero significa ausência de notificação registrada na base consultada**, e não comprova ausência de transmissão da dengue. Dados ausentes de população ou saneamento continuam nulos; eles não são convertidos em zero.

### Taxas epidemiológicas

Quando a população residente é válida e maior que zero, são calculadas:

$$
\text{taxa de notificações por 100 mil} =
\frac{\text{notificações}}{\text{população residente}} \times 100\,000
$$

$$
\text{taxa de confirmados por 100 mil} =
\frac{\text{casos confirmados}}{\text{população residente}} \times 100\,000
$$

Se o denominador estiver ausente ou não for positivo, a taxa permanece nula. A coluna de fonte da população permite distinguir Censo 2010, Censo 2022 e estimativas intercensitárias.

## Auditoria e testes

O fechamento da agregação inclui verificações automáticas para:

- garantir unicidade de `ano + codigo_municipio` nos contratos;
- garantir população válida para 2010 e 2022;
- separar ausência de registro SINAN de ausência de dados das demais fontes;
- preservar os totais do SINAN antes e depois da agregação dos registros com município válido;
- detectar multiplicação de linhas durante as junções;
- verificar percentuais do SIDRA no intervalo de 0 a 100;
- medir a cobertura das correspondências com população e saneamento;
- contabilizar códigos de residência/notificação inválidos e divergências entre os dois campos.

Os relatórios de qualidade devem acompanhar as bases analíticas; uma execução que falhe nas invariantes do contrato não deve ser usada nos mapas ou modelos.

### Validação da carga de 2010

Na execução validada em 2 de setembro de 2026, a Tabela 136 forneceu 5.565
linhas e 5.565 chaves municipais únicas, sem código nulo e sem população nula
ou não positiva. A soma da população residente foi 190.755.799 pessoas. O
contrato integrado manteve a grade completa e obteve 100% de correspondência
com população e saneamento.

Dos 5.565 municípios, 4.572 apresentaram notificação agregada e 993 foram
representados explicitamente sem notificação registrada. Entre os 1.381.254
registros SINAN brutos de 2010, 127 tinham residência inválida e 67 pertenciam
a 31 códigos sem correspondência na grade censitária; por isso, 1.381.060
notificações foram atribuídas aos municípios válidos. Essa reconciliação é
registrada em `auditoria_integracao.csv` e nas auditorias específicas de
códigos municipais.

## Arquivos produzidos

Os artefatos processados ficam em `sinan/processados/`. Os principais contratos são:

- `sinan_municipio_ano.parquet`: agregação observada do SINAN;
- `sinan_populacao_municipio_ano.parquet`: painel histórico integrado à população;
- `sinan_saneamento_2010.parquet`: contrato censitário de 2010;
- `sinan_saneamento_2022.parquet`: contrato censitário de 2022;
- `qualidade_sinan_por_arquivo.csv`: auditoria dos arquivos anuais do SINAN;
- `auditoria_sinan_codigos_municipio_invalidos.csv`: registros com código de residência ou notificação inválido;
- `auditoria_sinan_fluxo_residencia_notificacao.parquet`: contagens por par município de residência–município de notificação;
- `auditoria_sinan_sem_correspondencia_populacao_censo.csv`: códigos bem formados do SINAN sem correspondência na grade censitária do mesmo ano;
- `auditoria_integracao.csv`: cobertura e invariantes das junções.

## Reexecução reproduzível

Na raiz do repositório:

```bash
uv sync
uv run python analysis_pipeline.py --refresh-ibge
uv run python -m unittest discover -s tests -v
```

Depois de gerar os contratos e confirmar os testes, os notebooks devem ser executados de cima a baixo, sem depender de estado anterior, na ordem:

1. `sinan.ipynb`;
2. `seneamento.ipynb`;
3. `integracao_sinan_saneamento.ipynb`;
4. `analise_espacial_pysal.ipynb`.

Os resultados gravados nos notebooks precisam corresponder aos arquivos processados da mesma execução. O uso de `--refresh-ibge` exige conexão com a API SIDRA; sem essa opção, o pipeline reutiliza os arquivos processados existentes quando o cache está completo.

## Limitações

- O SINAN registra notificações e está sujeito a subnotificação, atraso, duplicidade e mudanças de preenchimento ao longo do tempo.
- Município de residência e município de notificação podem divergir; essa diferença é esperada em parte dos atendimentos e deve ser auditada.
- Os recortes municipais de 2010 e 2022 não são idênticos: havia 5.565 municípios em 2010 e 5.570 em 2022. Comparações temporais devem explicitar como tratam os cinco municípios criados nesse intervalo.
- A Tabela 136 traz dados da amostra do Censo 2010, enquanto a Tabela 4709 é a fonte adotada para a população do Censo 2022; a origem deve permanecer identificada nos resultados.
- O painel histórico preserva linhas do SINAN mesmo em anos sem denominador disponível nas fontes populacionais consultadas; nesses casos, população e taxas permanecem nulas.
- A Tabela 3218 usa domicílios particulares permanentes em 2010, enquanto as tabelas de 2022 usam domicílios particulares permanentes **ocupados**. Além disso, algumas categorias mudaram. Percentuais com nomes semelhantes não devem ser tratados como perfeitamente equivalentes sem recodificação metodológica.
- A associação entre dengue e saneamento é ecológica e exploratória; não demonstra causalidade individual.
- Como clima está fora do contrato atual, resultados desta etapa não controlam temperatura, precipitação ou defasagens climáticas.
