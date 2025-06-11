# Relatório de Análise Preditiva: Aluguel de Bicicletas em Seul

---

## Introdução

Este relatório apresenta uma análise preditiva detalhada do dataset de aluguel de bicicletas na cidade de Seul, Coreia do Sul, durante os anos de 2017 e 2018. O **objetivo principal** foi desenvolver modelos capazes de prever a quantidade de bicicletas alugadas com base em fatores temporais e climáticos, além de projetar o volume de aluguéis para janeiro de 2019.

A análise foi estruturada em etapas progressivas, começando pela **exploração dos dados**, seguida pela identificação de **padrões e relações** entre variáveis, **preparação dos dados** para modelagem, **construção e avaliação de modelos preditivos** e, finalmente, a **projeção de previsões futuras**. Cada etapa foi conduzida com rigor metodológico, garantindo resultados confiáveis e interpretáveis.

---

## Análise Exploratória dos Dados

### Estrutura do Dataset

O dataset contém **8.760 registros**, correspondendo a observações horárias ao longo de dois anos (2017-2018), com **13 variáveis originais**. A variável alvo, `Rented Bike Count`, representa a quantidade de bicicletas alugadas em cada hora registrada. Um ponto positivo é que **não foram identificados valores nulos** no conjunto de dados, facilitando a análise.

As variáveis incluem informações temporais (data/hora, dia, dia da semana, hora) e condições climáticas (temperatura, umidade, vento, etc.). A variável alvo `Rented Bike Count` apresenta uma ampla variação, com valores entre **0 e 3.556**, média de **704,60** e mediana de **504,5**, indicando uma distribuição assimétrica positiva.

### Análise da Variável Alvo

A distribuição da quantidade de bicicletas alugadas mostra um padrão **assimétrico à direita**, com maior concentração em valores mais baixos. Isso sugere períodos específicos de demanda excepcionalmente alta. A diferença entre média (704,60) e mediana (504,5) confirma essa assimetria.

A análise temporal revelou **padrões cíclicos importantes**: variações consistentes por hora, dia da semana e mês do ano, refletindo os padrões de mobilidade urbana.

### Influência das Variáveis Climáticas

* **Temperatura:** Mostrou a correlação mais forte com os aluguéis (**0,5386**). A demanda aumenta em temperaturas moderadas (10-30°C) e cai em extremos de frio (<0°C) ou calor (>30°C).
* **Umidade, Chuva e Neve:** Apresentaram correlações negativas, confirmando que condições climáticas adversas reduzem a demanda.
* **Visibilidade e Radiação Solar:** Mostraram correlações positivas, indicando que dias claros e ensolarados favorecem o uso de bicicletas.

### Padrões Temporais

* **Hora do Dia:** Forte influência (correlação de **0,4103**), com picos às **8h e 18h**.
* **Dias da Semana:** Padrões distintos entre dias úteis (picos de deslocamento) e fins de semana (demanda mais distribuída à tarde).
* **Sazonalidade:** Verão e primavera apresentam demanda significativamente maior que o inverno, com junho sendo o pico e janeiro o vale.

### Interações entre Variáveis

A combinação de fatores como **temperatura moderada e horários de pico** resultou nas maiores médias de aluguéis. A presença de chuva ou neve reduziu a demanda mesmo em temperaturas favoráveis. A **Análise de Componentes Principais (PCA)** sugeriu que as variáveis climáticas poderiam ser reduzidas a aproximadamente 3 componentes, explicando cerca de 80% da variância.

---

## Preparação dos Dados para Modelagem

Para potencializar a capacidade preditiva, realizamos diversas transformações:
1.  **Extração de características temporais** adicionais (ano, mês, dia do ano, estação).
2.  **Criação de variáveis cíclicas** (seno e cosseno para hora, dia da semana, mês).
3.  **Criação de variáveis de interação** (indicadores de fim de semana, chuva, neve).
4.  **Categorização da temperatura** em cinco níveis.

Os dados foram divididos em conjuntos de **treino (80%)** e **teste (20%)**, com estratificação por estação. As variáveis numéricas foram padronizadas. Foram definidos um conjunto básico e um expandido de variáveis para avaliar o ganho com a engenharia de features.

---

## Construção e Avaliação de Modelos Preditivos

### Abordagem de Modelagem

Avaliamos diversos algoritmos de regressão, desde modelos lineares simples até ensembles como Random Forest e Gradient Boosting, além de SVR e KNN. Os modelos foram treinados com o conjunto expandido de variáveis e avaliados com métricas de erro (RMSE, MAE) e R². O melhor modelo (`Random Forest`) foi otimizado com `GridSearchCV`.

### Resultados dos Modelos

* Os modelos baseados em árvores (`Random Forest` e `Gradient Boosting`) superaram significativamente os modelos lineares, indicando relações não-lineares nos dados.
* O **`Random Forest` otimizado** obteve o melhor desempenho, com **RMSE de aproximadamente 268,56** e **R² superior a 0,80** no teste.
* A análise de importância de variáveis destacou a **hora do dia, mês e temperatura** como os preditores mais relevantes.

### Modelo Temporal para Previsão Futura

Desenvolvemos um modelo específico (`Random Forest`) utilizando apenas variáveis temporais para a projeção de janeiro de 2019. Este modelo manteve boa capacidade preditiva, com **R² em torno de 0,70**, confirmando a forte influência dos padrões temporais.

---

## Projeção de Previsões para Janeiro de 2019

### Metodologia

As previsões para janeiro de 2019 foram geradas com o modelo temporal. Ressaltamos que esta abordagem tem limitações, como a não consideração de dados climáticos futuros ou eventos especiais.

### Resultados Principais

* **Total projetado:** aproximadamente **289.739** bicicletas alugadas.
* **Média diária projetada:** em torno de **9.346** bicicletas.
* **Picos de demanda:** dias úteis às **18h** (aprox. 2.829 bicicletas).
* **Mínimos de demanda:** madrugada (2h-4h, < 11 bicicletas).
* O heatmap de previsão confirmou os padrões de pico nos horários de deslocamento e baixa demanda noturna.

### Recomendações Operacionais

1.  Garantir disponibilidade de bicicletas nos horários de pico (**8h e 18h**).
2.  Planejar manutenção em períodos de menor demanda (madrugadas, domingos).
3.  Para maior precisão, incorporar previsões climáticas no modelo completo.
4.  Monitorar o desempenho real para calibrar o modelo.
5.  Implementar estratégias de incentivo em períodos de baixa demanda.

---

## Conclusões e Considerações Finais

A análise revelou padrões temporais e climáticos consistentes que influenciam a demanda por bicicletas. O modelo **`Random Forest` otimizado** apresentou o melhor desempenho. As projeções para janeiro de 2019 fornecem insights valiosos para o planejamento operacional.

**Principais fatores de influência identificados:**
1.  Hora do dia
2.  Temperatura
3.  Condições climáticas (chuva, neve, umidade)
4.  Dia da semana
5.  Sazonalidade

**Para trabalhos futuros, recomendamos:**
1.  Incorporar dados de feriados e eventos especiais.
2.  Desenvolver modelos específicos por estação.
3.  Explorar técnicas de séries temporais (ex: SARIMA, Prophet).
4.  Integrar previsões meteorológicas.
5.  Analisar dados de mais anos para identificar tendências.

Esta análise demonstra o potencial da modelagem preditiva para otimizar sistemas de mobilidade urbana.
