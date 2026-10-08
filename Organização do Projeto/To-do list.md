# Roteiro de trabalho

<<<<<<< HEAD
- [x] Baixar os dados da Dengue do SINAN 📅 2026-08-13 ✅ 2026-08-13
- [x] Baixar os dados do IBGE 📅 2026-08-13 . ✅ 2026-08-27
- [x] Baixar os dados de Clima do Earth Engine 📅 2026-08-13 ✅ 2026-08-20
- [x] Análise Exploratória apenas dos dados do SINAN 📅 2026-08-27 ✅ 2026-08-27
- [x] Explorar os schemas das tabelas do SIDRA 📅 2026-08-27 ✅ 2026-08-27
- [x] Selecionar Agregações de cada tabela do SIDRA. 📅 2026-09-03 ✅ 2026-08-27
- [x] Criar a primeira agregação: Delineamento 2010 (forma de abastecimento de água, destino do lixo, clima em são paulo, demográfica) 📅 2026-09-03
- [x] Criar a segunda agregação: Delineamento 2022 (forma de abastecimento de água, destino do lixo, clima em são paulo, demográfico) 📅 2026-09-03
- [x] Deixar o código comentado e o Obsidian organizado para a apresentação 📅 2026-09-03 ✅ 2026-09-03
=======
## Primeira etapa: agregação e exploração das fontes

### Coleta e exploração concluídas

- [x] Baixar os dados de dengue do SINAN 📅 2026-08-13 ✅ 2026-08-13
- [x] Baixar os dados do IBGE 📅 2026-08-13 ✅ 2026-08-27
- [x] Realizar a coleta climática inicial 📅 2026-08-13 ✅ 2026-08-20
  - O clima passou a ser tratado em repositório separado e está fora do contrato SINAN + IBGE atual.
- [x] Fazer a análise exploratória inicial do SINAN 📅 2026-08-27 ✅ 2026-08-27
- [x] Explorar os esquemas das tabelas do SIDRA 📅 2026-08-27 ✅ 2026-08-27
- [x] Selecionar as agregações das tabelas SIDRA 3218, 6803, 6805 e 6892 📅 2026-09-03 ✅ 2026-08-27
- [x] Construir a primeira versão da agregação SINAN + saneamento de 2010 📅 2026-09-03
- [x] Construir a primeira versão da agregação SINAN + saneamento de 2022 📅 2026-09-03
- [x] Documentar fontes, grão, chaves, taxas, ausências e limitações no Obsidian 📅 2026-09-03 ✅ 2026-09-02

### Fechamento do contrato municipal

- [x] Usar município de residência (`ID_MN_RESI`) como local analítico e preservar município de notificação (`ID_MUNICIP`) para auditoria 📅 2026-09-03 ✅ 2026-09-02
- [x] Integrar a população residente de 2010 pela Tabela SIDRA 136, variável 93, categoria total `c86/0` 📅 2026-09-03 ✅ 2026-09-02
- [x] Construir os contratos de 2010 e 2022 a partir da grade completa de municípios do IBGE 📅 2026-09-03 ✅ 2026-09-02
- [x] Preencher com zero somente as contagens dos municípios sem notificação agregada e preservar as flags `tem_notificacao_registrada` e `situacao_registro_sinan` 📅 2026-09-03 ✅ 2026-09-02
- [x] Calcular `taxa_notificacoes_100k` e `taxa_confirmados_100k` em 2010 e 2022 📅 2026-09-03 ✅ 2026-09-02
- [x] Gerar auditoria de códigos inválidos e divergências entre município de residência e de notificação 📅 2026-09-03 ✅ 2026-09-02
- [x] Automatizar e validar testes de unicidade, cobertura, percentuais, preservação de totais e cardinalidade das junções 📅 2026-09-03 ✅ 2026-09-02
- [x] Reexecutar o pipeline com as fontes atualizadas e confirmar todas as auditorias 📅 2026-09-03 ✅ 2026-09-02
- [ ] Reexecutar `sinan.ipynb`, `seneamento.ipynb`, `integracao_sinan_saneamento.ipynb` e `analise_espacial_pysal.ipynb` de cima a baixo 📅 2026-09-03
- [ ] Remover erros e resultados obsoletos dos notebooks antes da apresentação 📅 2026-09-03
>>>>>>> d015875 (oi)

---

## Segunda etapa: interpretação visual da informação integrada

- [x] Implementar um protótipo da análise espacial de 2022 com mapas, Moran global/bivariado e LISA
- [ ] Reexecutar e validar o protótipo de 2022 com o contrato baseado no município de residência
- [ ] Criar mapas de cobertura que diferenciem zero de notificações, dado ausente e código inválido
- [ ] Mapear as taxas de notificações e casos confirmados por 100 mil habitantes em 2010 e 2022
- [ ] Definir indicadores de saneamento comparáveis entre 2010 e 2022 e registrar o mapeamento entre categorias
- [ ] Definir o tratamento dos cinco municípios criados entre as malhas de 2010 e 2022
- [ ] Comparar distribuições e padrões espaciais de 2010 e 2022 sem interpretar categorias não equivalentes como variação temporal
- [ ] Recalcular Moran global e LISA para os dois anos e avaliar a estabilidade dos agrupamentos
- [ ] Registrar interpretações, cobertura e limitações junto de cada visualização

---

## Terceira etapa: construção e validação de modelos

- [ ] Definir a variável resposta, o horizonte temporal e a unidade de previsão antes da modelagem
- [ ] Selecionar apenas preditores disponíveis antes do desfecho e documentar possível vazamento de informação
- [ ] Construir um modelo-base de contagem, avaliando Poisson e binomial negativa com população como exposição
- [ ] Verificar sobredispersão, colinearidade, influência de extremos e estabilidade dos coeficientes
- [ ] Definir validação temporal e espacial, evitando partições aleatórias que misturem municípios vizinhos ou períodos
- [ ] Avaliar autocorrelação espacial dos resíduos
- [ ] Comparar o modelo-base com modelos de aprendizado de máquina usando as mesmas partições e métricas
- [ ] Reportar incerteza, desempenho fora da amostra e limitações ecológicas
- [ ] Planejar a incorporação futura do clima somente depois da formalização do contrato no repositório externo
