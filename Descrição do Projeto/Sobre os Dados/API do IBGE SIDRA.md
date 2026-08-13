## SIDRA
API do IBGE
## Fazendo uma requisiçao por meio do site.
https://sidra.ibge.gov.br/acervo


## Como a requisiçao e feita na API
### Campos:
* Pesquisa: representa a fonte dos dados, ou seja, de onde provêm as diversas tabelas armazenadas no Banco de Tabelas Estatísticas. O conjunto das tabelas de uma pesquisa compõe o seu Plano Tabular, que pode ser subdividido por Plano de Divulgação.
* Plano de Divulgação: cada uma das diferentes fases de divulgação dos dados de uma pesquisa ao longo do tempo. Ex: O Censo Demográfico inicia divulgando a sua Sinopse, depois seus Resultados Preliminares, depois os dados do Universo de Informantes etc.
* Assunto: cada um dos diferentes assuntos pesquisados. Ex: As Estatísticas do Registro Civil pesquisam dados sobre casamentos, nascimentos, divórcios etc.
* Tabela: é a unidade de consulta do Banco de Tabelas Estatísticas. Cada tabela é identificada por um número único.
* Variável: descreve o tipo de dado que a tabela possui. Uma mesma tabela pode possuir mais de uma variável. Ex: área colhida, área plantada, valor da produção etc.
* Classificação: utilizada para qualificar as variáveis de uma tabela, tornando o seu significado mais específico. Ex: a variável Quantidade produzida na lavoura temporária pode ser classificada por produto da lavoura temporária e grupo de área de lavoura.
* Categoria: Cada um dos possíveis valores que uma classificação pode assumir. Ex: para a classificação produto da lavoura temporária temos abacaxi, alho, mandioca etc e, para a classificação grupo de área de lavoura, temos menos de 1 hectare, 1 até menos de 2 hectares etc.
* Tipo de Período: tipo de unidade de tempo a qual os dados se referem. Ex: um mês, ano, trimestre etc.
* Nível Territorial: cada um dos diferentes tipos de divisão territorial espacialmente associados aos dados de uma tabela. Ex: Unidade da Federação, Grande Região, Município etc
* Unidade Territorial: cada um dos componentes de um nível territorial. Ex: para o nível territorial Unidade da Federação, temos Pernambuco, Rio de Janeiro, São Paulo etc.

### https://servicodados.ibge.gov.br/api/v3/agregados/{agregado}/periodos/{periodos}/variaveis/{variavel}

