# Guia 09: Classificação, métricas e cortes

## Por que classificação e não regressão ou clusterização

- **Classificação:** a decisão é binária (merece ou não) e o alvo é uma categoria.
- **Regressão:** estimaria um número e ainda faltaria definir um limite para virar decisão.
- **Clusterização:** agrupa parecidos **sem** usar o rótulo conhecido, então não responde "quais têm maior chance de merecer".

## Receita: comparar três classificadores

```python
# preparo do alvo, X, y e treino/teste: ver guia 07 (com stratify=y)

modelos = {
    "Regressão logística": LogisticRegression(max_iter=1000, random_state=42),
    "Árvore de classificação": DecisionTreeClassifier(max_depth=4, random_state=42),
    "Random Forest": RandomForestClassifier(random_state=42),
}

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
```

Outros modelos da lista (precisam de import, além dos da célula de preparação):
```python
from sklearn.ensemble import AdaBoostClassifier
from sklearn.naive_bayes import GaussianNB
```
`ExtraTreesClassifier` e `RandomForestClassifier` já vêm importados na célula de preparação.

## Matriz de confusão do modelo com maior F1

```python
melhor = tabela_modelos.sort_values("f1", ascending=False).iloc[0]["modelo"]
matriz = confusion_matrix(y_teste, previsoes[melhor])
print(pd.DataFrame(matriz, index=["real 0", "real 1"], columns=["previsto 0", "previsto 1"]))
```

Como ler:

| | previsto 0 | previsto 1 |
|---|---|---|
| **real 0** | verdadeiro negativo | **falso positivo** |
| **real 1** | **falso negativo** | verdadeiro positivo |

## Métricas

| Métrica | Pergunta que responde | Fórmula |
|---|---|---|
| **Precisão** | Das peças que o modelo indicou, quantas de fato mereciam? | VP ÷ (VP + FP) |
| **Recall** | Das peças que mereciam, quantas o modelo achou? | VP ÷ (VP + FN) |
| **F1** | Equilíbrio entre precisão e recall | média harmônica dos dois |

VP = verdadeiro positivo; FP = falso positivo; FN = falso negativo.

## Cortes (limiares) na regressão logística

O modelo dá uma **probabilidade**; o corte decide a partir de qual valor a peça é indicada.

```python
regressao = modelos["Regressão logística"]
prob = regressao.predict_proba(X_teste)[:, 1]     # probabilidade de ser da classe 1

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
print(pd.DataFrame(linhas).round(3).to_string(index=False))
```

**Trade-off:** baixar o corte faz o modelo indicar **mais** peças, então o recall sobe (acha mais peças boas) e a precisão tende a cair (mais indicações erradas). Subir o corte faz o contrário.

## Como decidir o corte

- **Poucas vagas para divulgar** → um falso positivo desperdiça uma vaga rara → favorece **precisão** → corte maior (0,50).
- **Custo alto de perder uma peça boa** ou muitas vagas → favorece **recall** → corte menor (0,30).
- Qualquer escolha é aceitável se **justificada** com os números da tabela e com o contexto do enunciado.

## Falso positivo e falso negativo para a equipe (modelo)

- **Falso positivo:** peça indicada para divulgação adicional que não mereceria; gasta uma das poucas vagas à toa.
- **Falso negativo:** peça que mereceria, mas ficou de fora; perde-se a chance de impulsioná-la.

## Cuidados

- O conjunto de teste é pequeno (30 publicações, cerca de 7 do grupo de interesse): uma peça muda muito as métricas.
- `ConvergenceWarning` na regressão logística é só um aviso de escala.
- Vazamento de dados: nada de alcance, interações, taxas ou identificador em `X`.
