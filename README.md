README.md

# 🏆 Hackathon Big Data 2025 – Previsão de Vendas  

Este repositório contém a solução desenvolvida para o desafio de previsão de vendas no **Hackathon Big Data 2025**.  

O objetivo foi prever a **quantidade semanal de vendas por PDV (ponto de venda) e SKU (produto)** para as cinco primeiras semanas de **janeiro/2023**, utilizando como base o histórico de vendas do ano de 2022.  

---

## 📂 Estrutura do Projeto  

- `notebooks/` → Contém os experimentos no Google Colab.  
- `dados/` → Pasta de entrada (não versionada aqui).  
- `submissoes/` → Arquivos gerados para submissão (`CSV` e `Parquet`).  
- `README.md` → Documentação do projeto.  

---

## ⚙️ Metodologia  

1. **Pré-processamento**
   - Conversão de datas em features de calendário (ano, mês, semana ISO, dia da semana, fim de mês, trimestre, semana do mês).  
   - Criação de variáveis de **lags** (1 a 4) e **médias móveis** (3 e 8 semanas).  
   - Feature de **semanas desde a última venda** (recência).  
   - Enriquecimento com atributos estáveis de produtos e PDVs (`categoria`, `subcategoria`, `premise`, `zipcode`).  

2. **Modelagem**
   - Algoritmo principal: **LightGBM Regressor**, com tunagem de hiperparâmetros.  
   - Validação temporal: treino (semanas 1–44 de 2022) e validação (semanas 45–52 de 2022).  
   - Blend simples entre modelo e *naive baseline* (lag-1).  

3. **Métricas de Avaliação**
   - **WMAPE** (Weighted Mean Absolute Percentage Error) → métrica oficial do desafio.  
   - Outras métricas monitoradas: **MAE**, **RMSE**, **sMAPE**.  

---

## 📊 Resultados Obtidos  

### Validação interna (2022/sem. 45–52)  
- **MAE** : 2.5090  
- **RMSE** : 10.7950  
- **WMAPE** : 0.4979  
- **sMAPE** : 0.4764  

### Evolução das Submissões  
1. **Submissão 1** → Versão inicial (baseline com lags) → `WMAPE ≈ 1.0`  
2. **Submissão 2** → Inclusão de features adicionais e LightGBM simples → `WMAPE ≈ 0.90`  
3. **Submissão 3** → Modelo tunado com regularização e early stopping → `WMAPE ≈ 0.90` (posição ~75 no ranking até o momento).  

---

## 📂 Submissões  

Arquivos enviados:  
- `submissao_final_ajustada.parquet` (1ª submissão)  
- `submissao2.csv` e `submissao2.parquet` (2ª submissão)  
- `submissao3.csv` e `submissao3.parquet` (3ª submissão, modelo tunado)  

Formato exigido pelo desafio:  

```csv
semana ; pdv ; produto ; quantidade
1 ; 1023 ; 123 ; 120
2 ; 1045 ; 234 ; 85

📈 Visualizações

Foram gerados gráficos auxiliares para análise exploratória (EDA):

Séries semanais de vendas agregadas.

Distribuição por categorias de produtos.

Comparação entre previsões do modelo e baseline (lag-1).

🚀 Execução

Clone este repositório:

git clone https://github.com/andreiaspi/bigdataforecast-2025.git
cd bigdataforecast-2025


Instale as dependências principais:

pip install pandas numpy lightgbm scikit-learn pyarrow


Execute o notebook principal no Google Colab:

/notebooks/forecast_lgbm.ipynb

O output final será salvo em:

/submissoes/submissao3.csv
/submissoes/submissao3.parquet

👤 Equipe

Participação individual: Andreia Spinella

Projeto desenvolvido no Google Colab, com Python:

pandas, numpy, lightgbm, scikit-learn

📝 Observações

O modelo atual ainda pode ser melhorado com feature engineering adicional (promoções, sazonalidade especial, eventos).

Foram priorizadas soluções rápidas e escaláveis devido ao limite de tempo da competição.

As submissões foram limitadas ao máximo de 1.500.000 linhas exigido pelo desafio.
