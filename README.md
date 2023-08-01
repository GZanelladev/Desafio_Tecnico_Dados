# Desafio Técnico - Dados - FieldPRO

## Objetivo: O objetivo deste desafio é construir um modelo de calibração de um sensor de chuva baseado em impactos mecânicos

### Objetivos Específicos:
1. **Análise Exploratória dos Dados:** Explorar os dados dos arquivos **Sensor_FieldPRO.csv** e **Estacao_Convencional.csv** para entender suas estruturas e verificar se há dados faltantes ou inconsistentes. Identificar correlações entre as variáveis e quais delas podem ser mais relevantes para a calibração.

2. **Pré-processamento dos Dados:** Realizar a limpeza dos dados, preencher valores faltantes, removendo duplicatas e convertendo as variáveis necessárias em formatos adequados para os modelos (quando aplicável).

3. **Feature Engineering:** Criar novas variáveis ou transformar as existentes com base no conhecimento do domínio e nas correlações identificadas.

4. **Seleção do Modelo:** Escolher um modelo adequado para calibração (Regressão linear, Regressão Polinomial, Random Forest ou Redes Neurais)

5. **Treinamento do Modelo:** Separar os dados em conjuntos de treinamento e teste. Treinar o modelo com os dados de treinamento e ajustando seus hiperparâmetros para obter o melhor desempenho possível.

6. **Avaliação do Modelo:** Utilizar o conjunto de testes para avaliar o desempenho do modelo e verificar sua capacidade de calibrar o sensor de chuva com precisão.

7. **Implementação do Modelo:** Após a escolha do melhor modelo, implantar para uso em produção.

8. **Monitoramento e Manutenção:** Manter o modelo em execução, monitorando seu desempenho ao longo do tempo e fazendo atualizações conforme necessário.


# Dicionário das Variáveis (Sensor_FieldPro.csv)

 0.   Datetime – utc       (object)  - Data e Hora da realização da medida
 1.   air_humidity_100     (float64) - Umidade do ar relativo externo
 2.   air_temperature_100  (float64) - Temperatura do ar externo
 3.   atm_pressure_main    (int64)   - Pressão atmosférica
 4.   num_of_resets        (int64)   - Número total de resets da placa desde que foi ligada pela primeira vez
 5.   piezo_charge         (int64)   - Carga do acumulador (medido de hora em hora)
 6.   piezo_temperature    (int64)   - Temperatura da placa

# Dicionário das Variáveis (Estação_Convencional.csv)

 0.   data             (object) - Data da medição
 1.   Hora (Brasília)  (object) - Hora da medição
 2.   chuva            (float64) - Chuva

## Introdução do Desafio

O sistema de medição de chuva funciona por meio de uma placa eletrônica com
um piezoelétrico, um acumulador de carga e um sensor de temperatura. Os dados são transmitidos de hora em hora.

O impacto das gotas de chuva gera vibrações no piezoelétrico, que induzem uma corrente elétrica.
A corrente elétrica não é medida diretamente, mas é acumulada ao longo do tempo e gera uma queda na carga do acumulador.

A carga do acumulador é medida de hora em hora e transmitida com o nome de **piezo_charge**.
A temperatura da placa é transmitida sob o nome **piezo_temperature** e pode ser importante na calibração.

Um evento de reset na placa pode afetar o comportamento do acumulador de carga, e o número total de resets da placa desde que foi ligada pela primeira vez
é transmitido com o nome **num_of_resets**.

As medidas realizadas pelo sensor estão no arquivo **Sensor_FieldPRO.csv**, para comparação, foram utilizadas medidas de uma estação metereológica próxima,
que estão no arquivo **Estacao_Convencional.csv**.

Outras medidas que podem ser úteis na modelagem são: a temperatura do ar externo **air_temperature_100**, umidade relativa do ar externo **air_humidity_100**
e a pressão atmosférica **atm_pressure_main**.

As medidas do sensor incluem a carga medida no acumulador, a temperatura da placa, o número de resets da placa e as condições atmosféricas do ambiente.

## Metodologia

**1. Análise Exploratória**

A analise exploratório foi realizada para os dois banco de dados **Sensor_FieldPRO.csv** e **Estacao_Convencional**, foi observado os tipos de dados,
a existência ou não de dados faltantes, e assim foi construído o dicionário de variáveis dos Datasets.
Foi realizada o estudo das correlações entre as features do sensor.

**2. Pré-processamento**

- Foi feita a exclusão dos dados faltantes, uma vez que estes dados representavam apenas 0,39% em relação ao todo.
- Foram mantidos os dados númericos para construção dos modelos de estudo.
- Foi excluída a features **air_temperature_100** devido a alta correlação com o valor da temperatura do piezoelétrico.

**3. Feature Engineering**

- Foi realizada a separação da data e horário para concatenar os dataset e realizar o ***join*** por intersecção.

**4. Seleção do Modelo**

- Foram realizados 5 modelos de Machine Learning para identificar qual teria a melhor performace para prever os valores de chuva.
- Os modelos escolhidos foram Random Forest Regressor, Support Vector Regression, Regressão Linear, Stochastic Gradient Descent e Regressão Polinomial.
- Para o modelo de Regressão Polinomial foi realizado o estudo do hiperparâmetro de grau das features.

**5. Treinamento do Modelo**

- O banco de dados foi dividido em base de treino e teste, sendo que o teste correspondia a 20% do dataset total.

**6. Avaliação do Modelo**

- Foram calculadas as métricas MSE, RMSE, MAE e R2 para avaliação dos modelos de Machine Learning adotados.
- A métrica R2 varia de 0 a 1, e representa o percentual da variância dos dados que é explicado pelo modelo,
portanto quanto maior seu valor mais explicativo é o modelo em relação aos dados previstos.
- A métrica MAE mede a média da diferença entre o valor real com o valor predito
- A métrica MSE calcula a média da diferença entre o valor predito com o real ao quadrado. Esta métrica é penalizada pelos Outliers.
- A métrica RMSE é calculada a partir da raiz quadrada da métrica MSE.

**7. Implementação do Modelo:**

- O melhor modelo (Random Forest Regressor) foi salvo para posterior deploy em alguma Nuvem (GCP, AWS ou Azzure).
- Também foi implementada uma aplicação onde é possível entrar com os dados do sensor e obter a previsão do valor para a chuva.

## Conclusão

O Modelo de Machine Learning Random Forest Regressor aprensentou o menor valor para a métrica RMSE e maior valor de R2, assim definindo
o melhor modelo quando comparado com o demais.

## Referências Bibliográficas

1. [Artigo Medium](https://medium.com/@lucas.lyon96/qual-modelo-de-machine-learning-escolher-para-o-meu-problema-8874c2bc8517")
2. [Artigo - Machine Learning Calibration Model](https://amt.copernicus.org/articles/11/291/2018/")
3. [Artigo - Uncontrolled Enviroments Calibration Sensors](https://github.com/marcelcases/calibration-sensors-machine-learning#data-observation")
4. [Aritgo - Machine Learning for Light Sensor](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8473444/")
5. [Métricas para Regressão](https://medium.com/data-hackers/prevendo-n%C3%BAmeros-entendendo-m%C3%A9tricas-de-regress%C3%A3o-35545e011e70")
