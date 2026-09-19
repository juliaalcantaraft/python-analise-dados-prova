# Questão 8: Priorização de divulgação

## Enunciado

Nos dias finais da campanha, a equipe só consegue dar divulgação adicional a poucas publicações. Antes de uma nova publicação ir ao ar, a pergunta é: quais peças têm maior chance de receber a classificação "merece divulgação adicional"?

Use somente `dados/publicacoes_analise.csv`. Crie `mereceu_divulgacao_adicional`: valor 1 quando `taxa_engajamento_pct` estiver acima do percentil 75 da própria base, e 0 nos demais.

1. Na Markdown, diga qual tipo de aprendizado de máquina é adequado: regressão, classificação ou clusterização: e explique brevemente por que os outros dois não respondem diretamente a essa decisão.
2. Use apenas características disponíveis antes da publicação: `tema`, `formato`, `seguidores_autor`, `videos_autor`, `tamanho_legenda`, `n_emojis`, `n_hashtags`, `hora`, `dia_semana` e `duracao_segundos`. Transforme as categorias em colunas numéricas quando necessário. Não use identificador, alcance, interações ou `taxa_engajamento_pct`: isso seria vazamento de dados.
3. Reserve 75% das publicações para treino e 25% para teste, usando `random_state=42` e preservando a proporção de publicações que mereceram divulgação adicional nos dois conjuntos.
4. Escolha três classificadores desta lista e compare-os: regressão logística, árvore de classificação, Random Forest, Extra Trees, AdaBoost e Gaussian Naive Bayes.
5. No conjunto de teste, apresente precisão, recall e F1 dos três modelos em uma tabela e exiba a matriz de confusão do modelo com maior F1.
6. Na regressão logística já ajustada e no mesmo conjunto de teste, compare os cortes 0,50 e 0,30 para decidir quando uma publicação recebe divulgação adicional. Apresente precisão, recall e F1 dos dois cortes em uma tabela.

Na resposta final, informe o modelo e o corte escolhidos. Explique, em até seis frases, o que falso positivo e falso negativo significam para a equipe de comunicação e justifique sua decisão com base no trade-off observado entre precisão e recall.

## Código

```python
# Questão 8
df_prior = pd.read_csv("dados/publicacoes_analise.csv")

# Alvo: 1 se a taxa está acima do percentil 75 da própria base
p75 = df_prior["taxa_engajamento_pct"].quantile(0.75)
df_prior["mereceu_divulgacao_adicional"] = (df_prior["taxa_engajamento_pct"] > p75).astype(int)
print(df_prior["mereceu_divulgacao_adicional"].value_counts())

# Características conhecidas antes da publicação; categorias viram colunas 0/1
colunas_antes = [
    "tema", "formato", "seguidores_autor", "videos_autor", "tamanho_legenda",
    "n_emojis", "n_hashtags", "hora", "dia_semana", "duracao_segundos",
]
X = pd.get_dummies(df_prior[colunas_antes], columns=["tema", "formato"], dtype=int)
y = df_prior["mereceu_divulgacao_adicional"]

# Treino (75%) e teste (25%), preservando a proporção do alvo
X_treino, X_teste, y_treino, y_teste = train_test_split(
    X, y, test_size=0.25, random_state=42, stratify=y
)
print("Proporção de 1 no treino:", round(y_treino.mean(), 3), "| no teste:", round(y_teste.mean(), 3))

# Três classificadores
modelos = {
    "Regressão logística": LogisticRegression(max_iter=1000, random_state=42),
    "Árvore de classificação": DecisionTreeClassifier(max_depth=4, random_state=42),
    "Random Forest": RandomForestClassifier(random_state=42),
}

# Métricas no conjunto de teste
resultados = []
previsoes = {}
for nome, modelo in modelos.items():
    modelo.fit(X_treino, y_treino)
    pred = modelo.predict(X_teste)
    previsoes[nome] = pred
    resultados.append({
        "modelo": nome,
        "precisao": precision_score(y_teste, pred, zero_division=0),
        "recall": recall_score(y_teste, pred),
        "f1": f1_score(y_teste, pred),
    })

tabela_modelos = pd.DataFrame(resultados)
print(tabela_modelos.round(3).to_string(index=False))

# Matriz de confusão do modelo com maior F1
melhor = tabela_modelos.sort_values("f1", ascending=False).iloc[0]["modelo"]
print("\nModelo com maior F1:", melhor)
matriz = confusion_matrix(y_teste, previsoes[melhor])
print(pd.DataFrame(matriz, index=["real 0", "real 1"], columns=["previsto 0", "previsto 1"]))

# Cortes 0,50 e 0,30 na regressão logística já ajustada
regressao = modelos["Regressão logística"]
prob = regressao.predict_proba(X_teste)[:, 1]

linhas = []
for corte in [0.50, 0.30]:
    pred_corte = (prob >= corte).astype(int)
    linhas.append({
        "corte": corte,
        "publicacoes_indicadas": int(pred_corte.sum()),
        "precisao": precision_score(y_teste, pred_corte, zero_division=0),
        "recall": recall_score(y_teste, pred_corte),
        "f1": f1_score(y_teste, pred_corte),
    })
print()
print(pd.DataFrame(linhas).round(3).to_string(index=False))
```

## Saída de referência

- Alvo: 90 com valor 0 e 30 com valor 1. Proporção de 1: 0,256 no treino e 0,233 no teste (o teste tem 30 publicações, 7 com valor 1).

| Modelo | Precisão | Recall | F1 |
|---|---|---|---|
| Regressão logística | 1,000 | 0,857 | 0,923 |
| Árvore de classificação | 0,444 | 0,571 | 0,500 |
| Random Forest | 1,000 | 0,571 | 0,727 |

Matriz de confusão (regressão logística, maior F1):

| | previsto 0 | previsto 1 |
|---|---|---|
| real 0 | 23 | 0 |
| real 1 | 1 | 6 |

| Corte | Peças indicadas | Precisão | Recall | F1 |
|---|---|---|---|---|
| 0,50 | 6 | 1,000 | 0,857 | 0,923 |
| 0,30 | 9 | 0,778 | 1,000 | 0,875 |

O `ConvergenceWarning` da regressão logística é só um aviso de escala e pode ser ignorado.

## Resposta (Markdown)

**Resposta da Questão 8.1: tipo de modelo**

O tipo adequado é a **classificação**, porque a decisão da equipe é binária (a peça merece ou não divulgação adicional) e o alvo mereceu_divulgacao_adicional é uma categoria com dois valores, 0 e 1. A **regressão** estimaria um valor numérico de engajamento, e não responde diretamente à pergunta de quem deve receber o destaque, pois ainda faltaria definir um limite para transformar o número em decisão. A **clusterização** agruparia publicações parecidas sem usar um rótulo conhecido, então não usa a informação que define o que merece divulgação e não diz quais peças têm maior chance de merecer.

**Resposta da Questão 8: decisão e risco de erro**

Escolhi a regressão logística, que teve o maior F1 (0,923) entre os três modelos, com o corte de 0,50. Um falso positivo é uma peça indicada para divulgação adicional que não mereceria, e faz a equipe gastar um dos poucos espaços à toa; um falso negativo é uma peça que mereceria, mas fica de fora, e a equipe perde a chance de impulsioná-la. Ao baixar o corte de 0,50 para 0,30, o recall subiu de 0,857 para 1,000, mas a precisão caiu de 1,000 para 0,778, porque as peças indicadas passaram de 6 para 9 e duas eram falsos positivos. Como a equipe só consegue divulgar poucas publicações, o desperdício de vagas pesa mais, e por isso prefiro o corte de 0,50, que não teve falso positivo e teve o maior F1 (0,923 contra 0,875). Se houvesse mais vagas, o corte de 0,30 seria defensável para não perder nenhuma peça boa. Como ressalva, o teste tem só 30 publicações e 7 do grupo de interesse, então a diferença entre os cortes é de uma única publicação e as métricas são instáveis.

## Sobre a decisão

O corte 0,30 também seria aceitável, se justificado (por exemplo, se perder uma peça boa custasse mais do que gastar uma vaga). O que se avalia é o raciocínio com base no trade-off observado.

## Guias relacionados

[09 Classificação e cortes](../guias/09_classificacao_e_cortes.md), [07 Árvore e alvo](../guias/07_arvore_e_importancia.md)
