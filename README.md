# 📈 Projeto de Previsão de Estoque com Amazon SageMaker Canvas

Este repositório documenta a criação e análise de um modelo de **Previsão de Estoque de Séries Temporais** utilizando o Amazon SageMaker Canvas, uma ferramenta de Machine Learning No-Code/Low-Code da AWS.

O objetivo do projeto é prever a `QUANTIDADE_ESTOQUE` futura de diferentes produtos, auxiliando o planejamento de compras e a mitigação do risco de ruptura de estoque (stockout).

## 🚀 Tecnologias e Ferramentas Envolvidas

| Categoria | Tecnologia | Função no Projeto |
| :--- | :--- | :--- |
| **Plataforma ML** | **Amazon SageMaker Canvas** | Ambiente visual (No-Code) para construção, treinamento, análise e deploy do modelo de ML. |
| **Cloud Computing** | **Amazon Web Services (AWS)** | Infraestrutura para hospedar o SageMaker e processar os dados. |
| **Dataset** | `dataset-500-curso-sagemaker-canvas-dio.csv` | Dados históricos contendo IDs de produtos, datas, flags de promoção e quantidade de estoque. |
| **Algoritmo** | **Time Series Forecasting** | Modelo de previsão de séries temporais (escolha automática otimizada pelo Canvas). |

## ⚙️ Configuração do Modelo

O Canvas foi configurado para resolver um problema de **Previsão de Séries Temporais** com base nos dados fornecidos:

| Variável | Coluna no Dataset | Função no Modelo |
| :--- | :--- | :--- |
| **Target (Variável a Prever)** | `QUANTIDADE_ESTOQUE` | Variável dependente. |
| **Timestamp (Eixo Temporal)** | `DIA` | Determina a ordem da série temporal. |
| **Item Identifier** | `ID_PRODUTO` | Permite que o modelo treine uma série temporal para cada produto. |
| **Exógena (Feature)** | `FLAG_PROMOCAO` | Variável externa que influencia a previsão. |
| **Treinamento** | Standard Build | Maior acurácia e análise detalhada. |


### Como Rodar o Projeto

* Este projeto foi desenvolvido integralmente no **Amazon SageMaker Canvas**.
* Para replicar, basta importar o dataset `dataset-500-curso-sagemaker-canvas-dio.csv` no Canvas e seguir as configurações de Séries Temporais descritas acima.

### 🔗 Links
[![github](https://img.shields.io/badge/github-000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/PEDROPAULONETO/k8s-projeto1-app-base/)
[![linkedin](https://img.shields.io/badge/linkedin-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/pedro-paulo-da-silva-neto-8b8a20368/)
