# Guia 01: Python básico para esta prova

Pense no Python como uma planilha em que você dá ordens escritas.

## Bibliotecas (a célula de preparação)

```python
import pandas as pd                # trabalha com tabelas (DataFrames)
import matplotlib.pyplot as plt    # faz gráficos
from sklearn.model_selection import train_test_split   # separa treino e teste
```

`import pandas as pd` significa "traga a ferramenta pandas e deixe-me chamá-la de `pd`". Essa célula roda **uma vez** no início.

## Variável

Uma caixinha com nome. O `=` **guarda** algo nela.

```python
df = pd.read_csv("dados/arquivo.csv")   # guarda a tabela na variável df
```

`df` vem de *DataFrame* (tabela). O nome é escolha sua.

## Comentário e print

```python
# tudo depois do # é ignorado pelo computador
print(df.shape)     # mostra o resultado na tela
```

Dentro de uma célula com várias linhas, só o que estiver dentro de `print(...)` aparece.

## Escolher coluna e linhas

| Quero | Código |
|---|---|
| uma coluna | `df["tema"]` |
| várias colunas | `df[["id_publicacao", "tema"]]` (colchetes duplos) |
| linhas que cumprem uma condição | `df[df["hora"] >= 18]` |
| duas condições (e) | `df[(df["formato"] == "reel") & (df["hora"] >= 18)]` |
| duas condições (ou) | `df[(df["tema"] == "saude") \| (df["tema"] == "cultura")]` |
| valor em uma lista | `df[df["tema"].isin(["saude", "cultura"])]` |

Regras: cada condição entre **parênteses**; `&` significa "e"; `|` significa "ou"; `==` compara (um `=` só guarda valor).

## Lista e dicionário

```python
colunas = ["tema", "formato", "hora"]         # lista: itens entre [ ]
modelos = {"A": LinearRegression(), "B": DecisionTreeRegressor()}   # dicionário: nome -> valor
```

## Repetir com for

```python
for col in ["alcance", "curtidas"]:
    df[col] = pd.to_numeric(df[col], errors="coerce")   # repete para cada coluna da lista
```

A linha dentro do `for` começa com **4 espaços**.

## Texto com valores dentro (f-string)

```python
print(f"A base tem {df.shape[0]} linhas")   # o que está entre { } vira valor
```

## Ponto e encadeamento

`df["tema"].str.strip().str.lower()` lê-se da esquerda para a direita: pega a coluna, tira espaços, deixa minúsculo.
