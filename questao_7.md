# Questão 7: Estimativa de engajamento

## Enunciado

A equipe de comunicação precisa estimar, antes da publicação, a `taxa_engajamento_pct` esperada de uma nova peça. Use somente `dados/publicacoes_analise.csv`.

1. No campo de resposta, indique o tipo de aprendizado adequado: regressão, classificação ou clusterização: e explique por que a variável-alvo exige essa escolha.
2. Use como características somente `tema`, `formato`, `seguidores_autor`, `videos_autor`, `tamanho_legenda`, `n_emojis`, `n_hashtags`, `hora`, `dia_semana` e `duracao_segundos`. Transforme categorias em números quando necessário. Não use identificador, alcance, interações nem variáveis calculadas a partir da taxa: elas não estão disponíveis antes da publicação.
3. Reserve 75% dos registros para treino e 25% para teste, com `random_state=42`.
4. Ajuste uma regressão linear e uma árvore de regressão com profundidade máxima 4. Compare os dois modelos com uma tabela de MAE e R².
5. Para o modelo com menor MAE, faça um gráfico de dispersão entre valores reais e previstos. Inclua uma linha de referência em que previsão e valor real seriam iguais, além de título e eixos nomeados.

Na resposta final, explique o que o MAE mede, indique o modelo escolhido e registre uma limitação que impeça interpretar a previsão como relação causal.

## Código

```python
# Questão 7
df_est = pd.read_csv("dados/publicacoes_analise.csv")

# Características conhecidas antes da publicação; categorias viram colunas 0/1
colunas_antes = [
    "tema", "formato", "seguidores_autor", "videos_autor", "tamanho_legenda",
    "n_emojis", "n_hashtags", "hora", "dia_semana", "duracao_segundos",
]
X = pd.get_dummies(df_est[colunas_antes], columns=["tema", "formato"], dtype=int)
y = df_est["taxa_engajamento_pct"]

# Treino (75%) e teste (25%)
X_treino, X_teste, y_treino, y_teste = train_test_split(
    X, y, test_size=0.25, random_state=42
)

# Dois modelos
modelos = {
    "Regressão linear": LinearRegression(),
    "Árvore de regressão": DecisionTreeRegressor(max_depth=4, random_state=42),
}

resultados = []
previsoes = {}
for nome, modelo in modelos.items():
    modelo.fit(X_treino, y_treino)
    pred = modelo.predict(X_teste)
    previsoes[nome] = pred
    resultados.append({
        "modelo": nome,
        "MAE": mean_absolute_error(y_teste, pred),
        "R2": r2_score(y_teste, pred),
    })

tabela_modelos = pd.DataFrame(resultados)
print(tabela_modelos.round(3).to_string(index=False))

# Modelo com menor MAE
melhor = tabela_modelos.sort_values("MAE").iloc[0]["modelo"]
print("Modelo com menor MAE:", melhor)

# Gráfico: valores reais x previstos
fig, ax = plt.subplots(figsize=(7, 6))
ax.scatter(y_teste, previsoes[melhor])
minimo = min(y_teste.min(), previsoes[melhor].min())
maximo = max(y_teste.max(), previsoes[melhor].max())
ax.plot([minimo, maximo], [minimo, maximo], linestyle="--", color="gray", label="Previsão = valor real")
ax.set_title(f"Valores reais e previstos da taxa de engajamento ({melhor})")
ax.set_xlabel("Taxa de engajamento real (%)")
ax.set_ylabel("Taxa de engajamento prevista (%)")
ax.legend()
plt.tight_layout()
```

## Saída de referência

| Modelo | MAE | R² |
|---|---|---|
| Regressão linear | 0,349 | 0,644 |
| Árvore de regressão | 0,516 | 0,129 |

Modelo com menor MAE: **regressão linear**.

## Resposta (Markdown)

**Resposta da Questão 7**

**Tipo de aprendizado:** regressão, porque a variável-alvo, taxa_engajamento_pct, é uma quantidade numérica contínua e o objetivo é estimar seu valor; classificação preveria categorias e clusterização não tem um alvo a prever.

**MAE e modelo escolhido:** o MAE é a média dos erros absolutos, isto é, quantos pontos percentuais a previsão erra, em média, em relação ao valor real. O modelo com menor MAE foi a regressão linear, com MAE de 0,349 contra 0,516 da árvore de regressão, e por isso o escolhi; o R² de 0,644 indica que ele explica parte considerável da variação da taxa no teste, enquanto a árvore (R² de 0,129) explica pouco.

**Limitação:** as características são usadas apenas para prever, e a previsão mostra uma associação nesta amostra de dados sintéticos, não indicando que mudar o formato ou o horário cause mais engajamento. Além disso, o conjunto de teste tem só 30 publicações, então as métricas podem mudar bastante com outra separação entre treino e teste.

## Guias relacionados

[08 Regressão](../guias/08_regressao.md), [05 Gráficos](../guias/05_graficos.md)
