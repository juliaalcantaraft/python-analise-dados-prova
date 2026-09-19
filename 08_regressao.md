# Guia 08: Regressão (prever um número)

## Qual tipo de aprendizado?

| Alvo | Tipo |
|---|---|
| **número contínuo** (taxa, preço, tempo) | **regressão** |
| **categoria** (sim/não, tipo A/B/C) | **classificação** |
| **sem alvo**; quer agrupar parecidos | **clusterização** |

Justificativa-modelo: "Regressão, porque a variável-alvo é uma quantidade numérica contínua e o objetivo é estimar seu valor."

## Receita (dois modelos, tabela de MAE e R²)

```python
colunas_antes = ["tema", "formato", "seguidores_autor", "videos_autor", "tamanho_legenda",
                 "n_emojis", "n_hashtags", "hora", "dia_semana", "duracao_segundos"]
X = pd.get_dummies(df[colunas_antes], columns=["tema", "formato"], dtype=int)
y = df["taxa_engajamento_pct"]

X_treino, X_teste, y_treino, y_teste = train_test_split(X, y, test_size=0.25, random_state=42)

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
    resultados.append({"modelo": nome,
                       "MAE": mean_absolute_error(y_teste, pred),
                       "R2": r2_score(y_teste, pred)})

tabela_modelos = pd.DataFrame(resultados)
print(tabela_modelos.round(3).to_string(index=False))

melhor = tabela_modelos.sort_values("MAE").iloc[0]["modelo"]   # modelo com menor MAE
print("Modelo com menor MAE:", melhor)
```

O gráfico real × previsto está no guia 05.

## O que as métricas medem

- **MAE (erro absoluto médio):** média das diferenças, sem sinal, entre real e previsto. Fica na **mesma unidade** do alvo. MAE 0,35 significa que a previsão erra em média 0,35 ponto percentual. **Menor é melhor.**
- **R²:** quanto da variação do alvo o modelo explica. Perto de 1 é bom; perto de 0 é parecido com prever sempre a média; pode ser **negativo** (pior que a média).

## Limitações para citar

- A previsão mostra **associação**, não causa e efeito: não prova que mudar o formato ou o horário aumente o engajamento.
- O conjunto de teste é pequeno (25% de 120 = 30 publicações), então as métricas mudam bastante com outra separação.
- Dados sintéticos, uma só campanha.
- Uma árvore ter ido pior que a regressão linear **nesta base** não significa que árvores sejam piores em geral.
