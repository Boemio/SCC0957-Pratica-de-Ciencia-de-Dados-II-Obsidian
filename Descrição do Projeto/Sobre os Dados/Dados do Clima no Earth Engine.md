# Dados do clima no Earth Engine

## Situação no projeto

Os dados climáticos estão sendo coletados e tratados em **um repositório separado**. Eles não fazem parte do pipeline SINAN + IBGE deste repositório e não estão presentes nos contratos `sinan_saneamento_2010.parquet` e `sinan_saneamento_2022.parquet`.

Esta separação é intencional nesta fase. A documentação do repositório atual não atribui fonte, produto, resolução espacial, variáveis ou regras de agregação aos dados climáticos, pois essas definições pertencem ao fluxo externo e ainda não foram incorporadas aqui.

## Limite de interpretação

As análises atuais relacionam notificações de dengue, população residente e saneamento domiciliar. Elas **não controlam fatores climáticos** e não devem ser apresentadas como se controlassem temperatura, precipitação ou qualquer outra medida meteorológica.

Uma integração futura somente deverá ocorrer depois que o repositório climático documentar sua fonte, período, unidade, granularidade espacial e temporal, tratamento de ausências e chave de associação. Até lá, clima permanece explicitamente fora do escopo deste contrato.
