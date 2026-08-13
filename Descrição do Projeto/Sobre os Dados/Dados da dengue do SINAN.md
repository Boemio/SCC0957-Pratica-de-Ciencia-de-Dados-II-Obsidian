## Sobre o SINAN
O [SINAN](https://portalsinan.saude.gov.br/) (Sistema de Informação de Agravos de Notificação) é a base de dados oficial do Ministério da Saúde no Brasil usada para registrar, transmitir e investigar doenças e problemas de saúde que exigem aviso obrigatório das autoridades. Sua utilização efetiva permite a realização do diagnóstico dinâmico da ocorrência de um evento na população, podendo fornecer subsídios para explicações causais dos agravos de notificação compulsória, além de vir a indicar riscos aos quais as pessoas estão sujeitas, contribuindo assim, para a identificação da realidade epidemiológica de determinada área geográfica. O seu uso sistemático, de forma descentralizada, contribui para a democratização da informação, permitindo que todos os profissionais de saúde tenham acesso à informação e as tornem disponíveis para a comunidade. É, portanto, um instrumento relevante para auxiliar o planejamento da saúde, definir prioridades de intervenção, além de permitir que seja avaliado o impacto das intervenções.
### O que ele faz
* Guarda dados sobre surtos e epidemias;
* Ajuda a rastrear riscos locais;
* Guia ações de saúde pública.
### Principais Funções
* Registra agravos como dengue, febre amarela e tuberculose.
- Ajuda estados e municípios a planejar respostas rápidas.
- Permite consultas de dados epidemiológicos pelo SUS.

## Sobre os dados da Dengue
Transmitida pelo mosquito Aedes aegypti, a dengue é uma doença viral que se espalha rapidamente no mundo. Nos últimos 50 anos, a incidência aumentou 30 vezes, com ampliação da expansão geográfica para novos países e, na presente década, para pequenas cidades e áreas rurais. É estimado que 50 milhões de infecções por dengue ocorram anualmente e que aproximadamente 2,5 bilhões de pessoas morem em países onde a dengue é endêmica.

Na região das Américas, a doença tem se disseminado com surtos cíclicos ocorrendo a cada 3/5 anos. No Brasil, a transmissão vem ocorrendo de forma continuada desde 1986, intercalando-se com a ocorrência de epidemias, geralmente associadas com a introdução de novos sorotipos em áreas anteriormente indenes ou alteração do sorotipo predominante. O maior surto no Brasil ocorreu em 2013, com aproximadamente 2 milhões de casos notificados. Atualmente, circulam no país os quatro sorotipos da doença.

### Ficha de Notificação
A ficha de notificação da dengue no SINAN é o documento oficial obrigatório utilizado por profissionais de saúde para registrar todo caso suspeito da doença no Brasil. Ela serve para avisar as autoridades de vigilância epidemiológica sobre a ocorrência do caso, permitindo ações rápidas de controle do mosquito Aedes aegypti e o monitoramento de epidemias. Atualmente, ela é integrada na Ficha de Investigação de Dengue e Chikungunya.

![[Ficha_DENGCHIK_FINAL.pdf]]

## Carregamento dos dados pelo PySUS
O [PySUS](https://pysus.readthedocs.io/en/latest/index.html) é um pacote desenvolvido em Python que facilita o acesso, o download, a organização e a análise de dados públicos de saúde disponibilizados pelo Sistema Único de Saúde (SUS). A ferramenta simplifica o processo de obtenção e tratamento dessas informações diretamente a partir das bases do DATASUS, reduzindo a necessidade de procedimentos manuais e complexos.

Por meio do PySUS, foram coletados e processados os dados referentes às fichas de notificação registradas ao longo dos últimos 23 anos, permitindo a construção de uma base histórica para a análise dos eventos notificados no período.

### Dicionário dos Dados
![[DIC_DADOS_ONLINE.pdf]]