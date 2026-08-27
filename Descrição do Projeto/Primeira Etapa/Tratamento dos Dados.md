## SINAN
* Ao todo coletamos os dados entre 2000-2026 e havia um total de mais de 22 milhões de notificações nessa base de dados. Para ser possível trabalhar com essa quantidade imensa de dados nós optamos por utilizar a biblioteca **polars** e coletar apenas as colunas necessárias nas análises que fizemos.
* Nós coletamos os seguintes dados: Notificações por dia, Sazonalidade mensal, Notificações por raça, Notificações por estado, Notificações por município, Notificações por classificação final da doença (cura, morte, etc).
* Parte dos dados do SINAN vieram com os dados faltando, mas nós percebemos que não haviam notificações com todos os dados faltando. Logo, decidimos não descartá-los e no lugar disso críamos uma nova categoria "não preenchidos", para diferenciar dos "ignorados".

## IBGE

## Clima
