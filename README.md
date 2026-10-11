# Detecção de Fraudes em Cartões de Crédito com Machine Learning

## Sobre o projeto

Este projeto tem como objetivo desenvolver e avaliar modelos de *Machine Learning* para a detecção de fraudes em transações de cartões de crédito. O trabalho aborda um problema comum em aplicações reais de classificação: o **desbalanceamento de classes**, em que as transações fraudulentas representam uma parcela muito pequena do conjunto de dados.

A proposta é construir um pipeline de aprendizado de máquina, desde a exploração e preparação dos dados até o treinamento, a avaliação e a interpretação dos modelos, utilizando métricas adequadas para esse tipo de problema.

## Problema abordado

A base de dados utilizada contém aproximadamente 284 mil transações, das quais apenas cerca de 0,17% são fraudulentas. Esse cenário torna a acurácia uma métrica insuficiente para avaliar a qualidade de um modelo.

Por exemplo, um classificador que considere todas as transações como legítimas pode apresentar uma acurácia próxima de 99,8%, mas será incapaz de identificar qualquer fraude.

Por esse motivo, o projeto prioriza métricas como **recall, precisão e F1-score para a classe fraudulenta**, buscando compreender o equilíbrio entre a identificação de fraudes e a quantidade de falsos positivos.

## Base de dados

O conjunto de dados utilizado é o *Credit Card Fraud Detection*, disponibilizado publicamente no Kaggle.

A base contém transações realizadas por cartões de crédito em setembro de 2013, durante dois dias de operações. Suas principais características são:

- `Time`: tempo transcorrido desde a primeira transação registrada na base.
- `Amount`: valor da transação.
- `V1` a `V28`: variáveis numéricas transformadas por Análise de Componentes Principais (PCA), utilizadas para preservar a confidencialidade das informações originais.
- `Class`: variável-alvo, em que `0` representa uma transação legítima e `1` representa uma transação fraudulenta.

O conjunto de dados é carregado diretamente da fonte, sem armazenar o arquivo original no repositório.

**Fonte dos dados:** [Credit Card Fraud Detection — Kaggle](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

## Tecnologias utilizadas

- **Python:** desenvolvimento do projeto.
- **Pandas:** manipulação e exploração dos dados.
- **NumPy:** operações numéricas e transformações.
- **Matplotlib e Seaborn:** visualização dos dados e das métricas.
- **Scikit-learn:** preparação dos dados, treinamento e avaliação dos modelos.
- **XGBoost:** treinamento de um modelo baseado em gradient boosting.
- **SHAP:** interpretação das previsões dos modelos.
- **Imbalanced-learn:** técnicas de balanceamento de classes, quando aplicadas.
- **Jupyter Notebook / Google Colab:** ambiente de desenvolvimento e experimentação.

## Etapas do projeto

### 1. Análise exploratória

Inicialmente, os dados são carregados e analisados para compreender sua estrutura, identificar as variáveis disponíveis e verificar a distribuição das classes.

São investigadas a quantidade de transações legítimas e fraudulentas, a proporção de cada classe e a distribuição dos valores das transações.

### 2. Preparação dos dados

A etapa de preparação contempla transformações necessárias para o treinamento dos modelos, incluindo:

- Criação de variáveis derivadas, como o logaritmo do valor da transação.
- Padronização das variáveis numéricas com `StandardScaler`, quando apropriado.
- Separação dos dados em conjuntos de treinamento e teste utilizando estratificação (`stratify`), preservando a proporção entre as classes.
- Tratamento do desbalanceamento por meio de estratégias como ajuste de pesos das classes, undersampling e oversampling, conforme os experimentos realizados.

As transformações que precisam aprender parâmetros a partir dos dados são ajustadas somente no conjunto de treinamento, evitando vazamento de informações (*data leakage*).

### 3. Treinamento dos modelos

São utilizados diferentes algoritmos de classificação para comparar suas capacidades de identificar transações fraudulentas:

- **Regressão Logística:** modelo de referência (*baseline*).
- **Random Forest:** conjunto de árvores de decisão capaz de representar relações não lineares.
- **XGBoost:** algoritmo de gradient boosting que constrói árvores de decisão sequencialmente.

Além da comparação entre os algoritmos, são investigados o tratamento do desbalanceamento, os hiperparâmetros e os limiares de decisão.

### 4. Avaliação dos resultados

A avaliação utiliza métricas que permitem analisar o desempenho dos modelos especificamente na identificação de fraudes.

- **Recall:** proporção de fraudes reais que foram identificadas corretamente.
- **Precisão:** proporção das transações classificadas como fraude que realmente eram fraudulentas.
- **F1-score:** média harmônica entre precisão e recall.
- **Curva ROC e AUC:** avaliam a capacidade de discriminação entre as classes em diferentes limiares.
- **Curva Precision-Recall:** permite analisar a relação entre precisão e recall, sendo especialmente útil em problemas com classes muito desbalanceadas.

O recall da classe fraudulenta recebe atenção especial, pois deixar de identificar uma fraude pode representar um prejuízo financeiro. Entretanto, a precisão também é importante, uma vez que um excesso de falsos positivos pode gerar bloqueios indevidos e prejudicar a experiência dos clientes.

O limiar padrão de 0,5 não é considerado obrigatório. Diferentes limiares
