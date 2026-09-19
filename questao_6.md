# Questão 6: Variáveis mais usadas pela árvore

## Enunciado

A equipe quer entender quais características a árvore usou mais para separar peças que mereceram divulgação adicional. Use somente `dados/publicacoes_analise.csv` e refaça esta questão de modo independente.

1. Crie `mereceu_divulgacao_adicional` pelo percentil 75 de `taxa_engajamento_pct`.
2. Use apenas as mesmas características disponíveis antes da publicação listadas na Questão 8 e transforme as categorias em números quando necessário.
3. Reserve 75% para treino e 25% para teste, usando `random_state=42` e preservando a proporção do alvo. Ajuste uma árvore de classificação com profundidade máxima 4, `random_state=42` e pesos balanceados entre as classes.
4. Produza uma tabela ordenada e um gráfico de barras horizontal com as cinco características de maior importância para a árvore. O gráfico deve ter título e eixos nomeados.

Na resposta final, explique por que uma importância alta não prova causalidade e descreva uma mudança na base que poderia alterar esse ranking.

## Código

```python
# Questão 6
df_arvore = pd.read_csv("dados/publicacoes_analise.csv")

# 1) Alvo: 1 se a taxa de engajamento está no percentil 75 ou acima
p75 = df_arvore["taxa_engajamento_pct"].quantile(0.75)
df_arvore["mereceu_divulgacao_adicional"] = (df_arvore["taxa_engajamento_pct"] >= p75).astype(int)
print("Percentil 75:", round(p75, 2))
print(df_arvore["mereceu_divulgacao_adicional"].value_counts())

# 2) Características conhecidas antes da publicação; categorias viram colunas 0/1
colunas_antes = [
    "tema", "formato", "seguidores_autor", "videos_autor", "duracao_segundos",
    "tamanho_legenda", "n_emojis", "n_hashtags", "hora", "dia_semana",
]
X = pd.get_dummies(df_arvore[colunas_antes], columns=["tema", "formato"], dtype=int)
y = df_arvore["mereceu_divulgacao_adicional"]

# 3) Treino (75%) e teste (25%), preservando a proporção do alvo
X_treino, X_teste, y_treino, y_teste = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)

arvore = DecisionTreeClassifier(max_depth=4, random_state=42, class_weight="balanced")
arvore.fit(X_treino, y_treino)

# 4) As cinco características de maior importância
importancias = (
    pd.Series(arvore.feature_importances_, index=X.columns)
    .sort_values(ascending=False)
    .head(5)
    .round(3)
)
print(importancias.to_frame("importancia"))

# Gráfico de barras horizontal
fig, ax = plt.subplots(figsize=(8, 5))
barras = ax.barh(importancias.index, importancias.values)
ax.invert_yaxis()   # a mais importante fica no topo
ax.set_title("Quais características a árvore mais usou para separar publicações de maior engajamento?")
ax.set_xlabel("Importância na árvore")
ax.set_ylabel("Característica")
ax.bar_label(barras, fmt="%.3f")
plt.tight_layout()
```

## Saída de referência

- Percentil 75: 4,89. Alvo: 90 com valor 0 e 30 com valor 1.

| Característica | Importância |
|---|---|
| formato_reel | 0,282 |
| n_hashtags | 0,176 |
| dia_semana | 0,142 |
| tamanho_legenda | 0,119 |
| tema_cultura | 0,102 |

## Resposta (Markdown)

**Resposta da Questão 6**

Uma importância alta mostra apenas que a árvore usou bastante aquela característica para separar, nesta amostra, as publicações do quarto melhor engajamento das demais, como no caso de formato_reel (0,282). Isso é uma associação estatística e não prova causalidade: a árvore não testa o que aconteceria se o formato fosse mudado, e características correlacionadas entre si dividem ou disputam a importância. Com apenas 120 publicações e 30 no grupo de destaque, a árvore também pode escolher uma variável por acaso. Uma mudança na base que poderia alterar o ranking seria incluir publicações de outra campanha ou período, ou trocar o percentil de corte: com mais casos ou outro perfil de autores, características como n_hashtags, dia_semana ou tamanho_legenda poderiam ganhar ou perder posição para formato_reel.

## Nota

Nesta questão usei `>=` (percentil 75 "ou acima"); na Questão 8 o enunciado diz "acima" e usei `>`. Com 120 linhas o resultado foi o mesmo (30 com valor 1).

## Guias relacionados

[07 Árvore e importância](../guias/07_arvore_e_importancia.md)
