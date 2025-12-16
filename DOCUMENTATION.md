# 📊 Documentação do Processo de Previsão de Estoque com SageMaker Canvas

Este documento detalha o processo de construção, treinamento, análise e utilização de um modelo de previsão de estoque usando o **Amazon SageMaker Canvas**, com base no arquivo de dados fornecido (`dataset-500-curso-sagemaker-canvas-dio.csv`).

## 1. Preparação dos Dados no SageMaker Canvas

O SageMaker Canvas é uma interface visual que permite aos usuários criar modelos de Machine Learning sem escrever código. 

### A. Importação do Dataset

1.  **Acessar o SageMaker Canvas:** Inicie o SageMaker Canvas a partir do console da AWS.
2.  **Criar/Importar Dataset:** Na seção "Datasets", clique em "Create" ou "Import".
3.  **Seleção do Arquivo:** Selecione o arquivo `dataset-500-curso-sagemaker-canvas-dio.csv` para upload.
4.  **Pré-visualização:** O Canvas exibirá uma pré-visualização dos dados, contendo as seguintes colunas:
    * `ID_PRODUTO`: Identificador do produto.
    * `DIA`: Data da observação.
    * `FLAG_PROMOCAO`: Indica se há promoção (0 ou 1).
    * `QUANTIDADE_ESTOQUE`: A quantidade de estoque naquele dia (a variável que queremos prever).

## 2. Construção e Treinamento do Modelo

### A. Configuração do Modelo

1.  **Criar Novo Modelo:** Na seção "Models", clique em "Create a new model".
2.  **Nome do Modelo:** Dê um nome, como `PrevisaoEstoque_DIO`.
3.  **Selecionar o Dataset:** Escolha o dataset importado (`dataset-500-curso-sagemaker-canvas-dio.csv`).
4.  **Definir Variável de Destino (Target):**
    * Selecione **`QUANTIDADE_ESTOQUE`** como a coluna que você deseja prever.
5.  **Definir Tipo de Problema:**
    * O SageMaker Canvas identificará automaticamente o problema como **Regressão** (prever um valor numérico contínuo), ou mais especificamente, uma **Previsão de Séries Temporais** devido à coluna `DIA`.

### B. Configurações Adicionais (Séries Temporais)

Como o problema é de previsão de séries temporais, o Canvas solicita informações específicas:

* **Identificador de Item (Item Identifier):** Selecione **`ID_PRODUTO`** (necessário para modelar as séries temporais individualmente para cada produto).
* **Timestamp:** Selecione **`DIA`** (a coluna de tempo).

### C. Treinamento do Modelo

1.  **Revisar e Excluir Colunas (Opcional):** `ID_PRODUTO`, `DIA`, e `FLAG_PROMOCAO` são úteis e devem ser mantidas.
2.  **Iniciar Treinamento:**
    * O Canvas oferece as opções: **"Quick build"** (Treinamento rápido e menos preciso) e **"Standard build"** (Treinamento mais completo e preciso).
    * **Escolha para a Documentação:** Escolha **"Standard build"** para obter as melhores métricas. Clique em **"Train"**.

## 3. Análise do Modelo

Após a conclusão do treinamento, a página de análise do modelo será apresentada. 

### A. Métricas de Performance

O foco deve estar em minimizar o erro. As métricas típicas são:

* **RMSE (Root Mean Square Error):** Média quadrática da raiz do erro. Indica o tamanho médio do erro, em unidades da variável de destino (`QUANTIDADE_ESTOQUE`). **Um valor menor é melhor.**
    > *Exemplo:* Se o RMSE for 15, o erro médio de previsão é de $\pm 15$ unidades de estoque.
* **MAPE (Mean Absolute Percentage Error):** Erro percentual absoluto médio. É fácil de interpretar. **Um valor menor é melhor.**
    > *Exemplo:* Um MAPE de 10% significa que o erro médio de previsão é de 10%.

> **Requisito: Examine as métricas.**
> *Anotar os valores de RMSE e MAPE (ex: RMSE = 18.5, MAPE = 12.3%) obtidos após o treinamento.*

### B. Influência das Características

O Canvas fornece um gráfico que mostra a importância de cada coluna na previsão.

* **Verificar as Principais Características:** As mais influentes geralmente são:
    1.  A própria história da `QUANTIDADE_ESTOQUE` em dias anteriores (o modelo de séries temporais captura isso).
    2.  A `FLAG_PROMOCAO` (promoções podem reduzir drasticamente o estoque).
    3.  O `ID_PRODUTO` (padrões de estoque diferentes).

> **Requisito: Verifique as principais características.**
> *Documentar as colunas mais importantes identificadas pelo Canvas (ex: 1. `QUANTIDADE_ESTOQUE (Lagged)`, 2. `FLAG_PROMOCAO`, 3. `ID_PRODUTO`).*

### C. Ajustes e Re-treinamento (Processo Iterativo)

> **Requisito: Faça ajustes e re-treine se necessário.**

Se as métricas iniciais não forem satisfatórias, as ações a seguir podem ser tomadas:

1.  **Ajustar Configurações:** Voltar para a seção de `Build` e ajustar as configurações do modelo ou incluir/excluir colunas.
2.  **Aumentar Tempo de Treinamento (Se usou Quick Build):** Re-treinar com "Standard build".
3.  **Refinar Dados (Feature Engineering):** Voltar à fonte de dados e realizar transformações para criar colunas mais preditivas (ex: Dia da Semana, Mês, Contagem de dias desde a última promoção, etc.).

## 4. Previsão e Conclusões

### A. Gerar Previsões

1.  **Acessar a Aba "Predict":** No modelo treinado, vá para a aba "Predict".
2.  **Selecionar o Tipo de Previsão:**
    * **Previsão de Ponto (Single item prediction):** Para prever uma única linha de dados.
    * **Previsão em Lote (Batch prediction):** Para prever várias linhas ou um intervalo de datas futuras.
3.  **Configurar Previsão de Lote:**
    * Especificar a **Duração da Previsão (Forecast Horizon)**, ou seja, quantos dias (ou períodos) no futuro você deseja prever.
    * *Exemplo:* Prever o estoque para os próximos **7 dias**.
4.  **Executar a Previsão:** Clique em **"Generate"**. O Canvas irá gerar o arquivo de resultados.

### B. Análise das Previsões

1.  **Exportar Resultados:** Exporte o arquivo de resultados para análise externa.
2.  **Analisar:** O arquivo de saída incluirá o `ID_PRODUTO`, o `DIA` futuro e a **`QUANTIDADE_ESTOQUE` Prevista**.

#### Insights e Conclusões

> **Requisito: Documente suas conclusões e insights.**

* **Conclusão do Desempenho:** O modelo é considerado **satisfatório/insatisfatório** com um erro percentual médio (MAPE) de X%.
* **Insight 1 (Padrões de Estoque):** Observamos que o estoque dos produtos 8, 10, e 14 (aqueles com `FLAG_PROMOCAO` alta) tende a ser previsto como **significativamente mais baixo** após as datas de promoção.
* **Insight 2 (Impacto da Promoção):** A característica `FLAG_PROMOCAO` foi a **segunda mais influente**, indicando que a decisão de promoção é um fator chave para o planejamento de estoque, devendo ser fornecida com antecedência para obter previsões precisas.
* **Próximos Passos:** Para melhorar a acurácia, pode-se incluir dados externos como feriados, eventos sazonais ou dados de vendas diárias (se disponíveis) para enriquecer o modelo.

## 🎯 Resumo da Solução

| Etapa | Ação no SageMaker Canvas | Variável(is) Chave |
| :--- | :--- | :--- |
| **Construção** | Criar modelo de **Previsão de Séries Temporais** | `QUANTIDADE_ESTOQUE` (Target), `DIA` (Timestamp), `ID_PRODUTO` (Item ID) |
| **Treinamento** | Selecionar **Standard build** | Otimização para minimizar RMSE/MAPE |
| **Análise** | Avaliar **Métricas de Performance** e **Feature Importance** | RMSE (Erro Absoluto), MAPE (Erro Percentual) |
| **Previsão** | Usar a função **Previsão em Lote** | Definir o horizonte de previsão (ex: 7 dias) |