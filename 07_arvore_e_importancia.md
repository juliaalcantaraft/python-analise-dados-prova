# Guia 07: Alvo, treino/teste e árvore (importância das características)

## Passo 1: criar o alvo (0 ou 1) pelo percentil

```python
p75 = df["taxa_engajamento_pct"].quantile(0.75)     # valor que separa os 25% melhores
df["alvo"] = (df["taxa_engajamento_pct"] >= p75).astype(int)   # >= (Q6) ou > (Q8): siga o enunciado
print(df["alvo"].value_counts())                     # com 120 linhas: 90 de 0 e 30 de 1
```

- "**Acima** do percentil 75" → `>`.
- "No percentil 75 **ou acima**" ou apenas "pelo percentil 75" → `>=`.
- `.astype(int)` transforma verdadeiro/falso em 1/0.

## Passo 2: escolher as características (X) e o alvo (y)

Só variáveis **conhecidas antes** da publicação:

```python
colunas_antes = ["tema", "formato", "seguidores_autor", "videos_autor", "duracao_segundos",
                 "tamanho_legenda", "n_emojis", "n_hashtags", "hora", "dia_semana"]
X = pd.get_dummies(df[colunas_antes], columns=["tema", "formato"], dtype=int)
y = df["alvo"]
```

`get_dummies` troca cada categoria por colunas 0/1 (`tema_saude`, `formato_reel`...). Modelos só entendem números.

**Nunca inclua:** `id_publicacao`, `alcance`, curtidas, comentários, compartilhamentos, salvamentos, `taxa_engajamento_pct`, `taxa_utilidade_pct`. É vazamento de dados: informação posterior à publicação.

## Passo 3: treino e teste

```python
X_treino, X_teste, y_treino, y_teste = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)
```

- `test_size=0.25`: 25% para teste (75% treino).
- `random_state=42`: o sorteio fica repetível.
- `stratify=y`: mantém a mesma proporção de 0 e 1 nos dois conjuntos ("preservando a proporção do alvo").
- Em **regressão** (alvo numérico), não use `stratify`.

## Passo 4: ajustar a árvore

```python
arvore = DecisionTreeClassifier(max_depth=4, random_state=42, class_weight="balanced")
arvore.fit(X_treino, y_treino)
```

- `max_depth=4`: no máximo 4 níveis de perguntas.
- `class_weight="balanced"`: dá mais peso à classe rara.
- `.fit(...)` é o treinamento.

## Passo 5: importâncias

```python
importancias = (
    pd.Series(arvore.feature_importances_, index=X.columns)
    .sort_values(ascending=False)
    .head(5)
    .round(3)
)
print(importancias.to_frame("importancia"))
```

Gráfico horizontal:
```python
fig, ax = plt.subplots(figsize=(8, 5))
barras = ax.barh(importancias.index, importancias.values)
ax.invert_yaxis()
ax.set_title("Quais características a árvore mais usou?")
ax.set_xlabel("Importância na árvore")
ax.set_ylabel("Característica")
ax.bar_label(barras, fmt="%.3f")
plt.tight_layout()
```

## Como interpretar

- As importâncias somam 1. Um valor maior significa que a árvore **usou mais** essa variável para dividir os dados.
- Categorias aparecem separadas (`formato_reel`, `tema_saude`), porque viraram colunas 0/1.
- **Não é causalidade.** Mostra associação nesta amostra. A árvore não testa o que aconteceria se você mudasse a variável.
- Variáveis correlacionadas dividem ou disputam a importância.
- Variáveis numéricas com muitos valores diferentes têm mais pontos de corte possíveis e podem aparecer bem colocadas por acaso.
- Com poucos registros, o ranking é instável.

## O que poderia mudar o ranking

Incluir publicações de outra campanha ou período; mudar o percentil de corte; adicionar ou remover uma variável correlacionada; mudar o sorteio de treino e teste (`random_state`); mudar a profundidade da árvore.
