# 📈 Forecast de Vendas – Hackathon Big Data 2025

Este repositório contém a solução desenvolvida para o desafio de previsão de demanda semanal por **PDV / SKU** no Hackathon Big Data 2025.  
O objetivo é prever as vendas das **5 semanas de janeiro/2023** utilizando o histórico completo de **2022**.

---

##  Executar no Google Colab

Clique abaixo para abrir o notebook diretamente no Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/andreiaspi/bigdataforecast-2025/blob/main/forecast_lgbm_.ipynb)

## 📂 Estrutura do Repositório

bigdataforecast-2025/
│
├── notebooks/
│ └── previsao_lgbm.ipynb # notebook principal (Colab)
│
├── submissões/
│ ├── submissao1.parquet
│ ├── submissao2.parquet
│ └── submissao3.parquet
│
├── README.md

# Descrição rápida
- **notebooks/previsao_lgbm.ipynb** → pipeline completo: EDA, features, modelagem (LightGBM), validação, submissão.  
- **submissões/** → arquivos no formato oficial (`semana;pdv;produto;quantidade`, UTF-8).  
- **data/** → arquivos originais (não versionados). 

---## Como Executar

### 1. Requisitos
- Python 3.10+  
- Instale as bibliotecas necessárias:
  ```bash
  pip install pandas numpy lightgbm scikit-learn pyarrow

### 2.Dados

Os arquivos originais não estão versionados no GitHub (por tamanho).
Coloque-os dentro da pasta data/:

produtos.parquet

pdvs.parquet

transacoes_2022.parquet

Caso utilize Google Drive + Colab, ajuste os caminhos de leitura no notebook.

### 3. Notebook

Abra no Google Colab clicando no ícone


#Ou rode localmente:

### 4. Saída

Após executar todo o pipeline, os arquivos de submissão serão gerados na pasta submissões/:

submissao3.csv

submissao3.parquet

Formato exigido:

semana;pdv;produto;quantidade
1;1023;123;120
2;1045;234;85
3;1023;456;110


semana: 1 a 5 (janeiro/2023)

pdv, produto: inteiros

quantidade: inteiro, arredondado, não negativo
jupyter notebook notebooks/previsao_lgbm.ipynb

## ⚙️ Metodologia

1. **Exploração e Limpeza de Dados (EDA)**  
   - Identificação de outliers, vendas negativas e dados duplicados.  
   - Criação de base semanal agregada por `(ano, semana, pdv, produto)`.  

2. **Feature Engineering**  
   - Lags de 1 a 4 semanas.  
   - Médias móveis (3 e 8 semanas).  
   - Tendência de curto prazo (`roll3 - roll8`).  
   - Recência (semanas desde a última venda).  
   - Atributos estáveis de produto e PDV (categoria, subcategoria, premise, categoria_pdv, zipcode).  
   - Flag sazonal de dezembro (`is_december`).  

3. **Modelagem**  
   - Modelo principal: **LightGBM Regressor**.  
   - Treino em 2022 (semanas 1–44), validação em 2022 (semanas 45–52).  
   - Tunagem de hiperparâmetros (`num_leaves`, `min_data_in_leaf`, `learning_rate`, etc.).  

4. **Validação**  
   - Métrica oficial: **WMAPE**.  
   - Métricas adicionais: **MAE, RMSE, sMAPE** para maior robustez.  

5. **Previsão para Janeiro/2023**  
   - Estratégia iterativa semana a semana (1–5).  
   - Estado inicial baseado na semana 52 de 2022.  
   - Controle do limite de 1.500.000 linhas na submissão.  

---

##  Resultados

###  Validação Interna (2022 / Semanas 45–52)
- **MAE**   : 2.50  
- **RMSE**  : 10.79  
- **WMAPE** : 0.49  
- **sMAPE** : 0.47%  

### 📈 Evolução das Submissões
| Submissão | Modelo / Estratégia                 | WMAPE (Leaderboard) |
|-----------|--------------------------------------|----------------------|
| 1         | LGBM simples (baseline)             | ~1.00               |
| 2         | Blend LGBM + Naive (lag-1)          | ~0.90               |
| 3         | LGBM Tunado + Features adicionais   | ~0.90               |

---

### Resultados e Visualizações

Durante a exploração e validação do modelo, foram gerados alguns gráficos de apoio:

1. **Série temporal de vendas (2022)**  
   Mostra a evolução semanal das quantidades vendidas, evidenciando tendência e sazonalidade.  

2. **Comparação de Previsões vs Valores Reais**  
   Gráfico de barras/linhas para as semanas de validação (sem. 45–52 de 2022), avaliando a performance do LightGBM.  

Esses gráficos auxiliaram na interpretação do modelo e confirmaram a consistência da previsão.  

##  Conclusão

- Implementei uma solução **robusta, escalável e interpretável** para previsão de vendas semanais.  
- Explorando  atributos de negócio relevantes (**PDV, Categoria, Premise, Zipcode**), além de variáveis temporais (lags, médias móveis, sazonalidade).  
- Acompanhando  a evolução das submissões, melhorando a performance em relação ao baseline.  
- O modelo final (LGBM tunado) obteve **WMAPE competitivo** e está pronto para ser replicado em outros períodos.  

---

Os proóximos seriam:
##  Próximos Passos

- Explorar **modelos alternativos** (XGBoost, CatBoost, Prophet híbrido) para comparação de performance.  
- Incluir **variáveis externas** relevantes (feriados, sazonalidade especial, promoções, eventos regionais).  
- Realizar **feature engineering avançado** (variáveis de tendência, sazonalidade mensal e anual, interações entre categorias de produtos e tipos de loja).  
- Implementar **técnicas de hiperparâmetros automáticas** (Optuna, GridSearchCV) para refinar ainda mais o modelo.  
- Criar **painéis visuais** (Dash, Streamlit ou Power BI) para facilitar a análise das previsões.  
- Ajustar a estratégia de previsão para suportar **horizontes mais longos** (além de 5 semanas).  

##  Integrante Individual

**Andreia Spinella**  
Iniciante e entusiasta em tecnologia, atualmente em transição de carreira e negócios.  

🔗 [LinkedIn](https://shre.ink/SolH) *(a atualizar)*  

Atualmente estudando fundamentos de negócios (**processos, pessoas e tecnologia**).  
Acredito que antes de implementar Inteligência Artificial é essencial compreender profundamente o funcionamento das empresas, sua **cultura organizacional** e estratégias de negócio, para então aplicar tecnologia de forma eficaz e com impacto real.


