# 📈 Forecast de Vendas – Hackathon Big Data 2025

Este repositório contém a solução desenvolvida para o desafio de previsão de demanda semanal por **PDV / SKU** no Hackathon Big Data 2025.  
O objetivo é prever as vendas das **5 semanas de janeiro/2023** utilizando o histórico completo de **2022**.

---

## 🚀 Executar no Google Colab

Clique abaixo para abrir o notebook diretamente no Colab:

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/andreiaspi/bigdataforecast-2025/blob/main/previsao_lgbm.ipynb)

---

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


---

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

## 📊 Resultados

### 🔎 Validação Interna (2022 / Semanas 45–52)
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

## 🏆 Conclusão

- Implementei uma solução **robusta, escalável e interpretável** para previsão de vendas semanais.  
- Exploramos atributos de negócio relevantes (**PDV, Categoria, Premise, Zipcode**), além de variáveis temporais (lags, médias móveis, sazonalidade).  
- Acompanhamos a evolução das submissões, melhorando a performance em relação ao baseline.  
- O modelo final (LGBM tunado) obteve **WMAPE competitivo** e está pronto para ser replicado em outros períodos.  

---

Os proóximos seriam:
Incluir informações externas (feriados, clima, eventos regionais).  
- Explorar ensembles de modelos (LightGBM + Prophet + ARIMA).  
- Ajustes mais finos de hiperparâmetros com **Optuna**.  
- Avaliação por família de produtos e clusters de PDVs.
