# Guia 04: Métricas por grupo

`groupby` separa as linhas em grupos e calcula algo em cada um (como uma tabela dinâmica).

## Um grupo (por tema)

```python
tabela = (
    df.groupby("tema")["taxa_utilidade_pct"]
    .agg(publicacoes="count", mediana="median")
    .sort_values("mediana", ascending=False)
    .round(2)
)
print(tabela)
```

## Só uma estatística

```python
df.groupby("tema")["taxa_utilidade_pct"].median().sort_values(ascending=False)
```

## Várias métricas de colunas diferentes

```python
tabela = (
    df.groupby("dia_publicacao")
    .agg(
        publicacoes=("id_publicacao", "count"),
        taxa_media=("taxa_engajamento_pct", "mean"),
        alcance_total=("alcance", "sum"),
    )
)
```

O padrão é `nome_novo=("coluna", "operação")`.

## Duas variáveis (combinação tema × formato)

```python
tabela = (
    df.groupby(["tema", "formato"])
    .agg(publicacoes=("id_publicacao", "count"),
         mediana=("taxa_engajamento_pct", "median"))
    .sort_values("mediana", ascending=False)
    .round(2)
    .reset_index()          # devolve tema e formato para colunas normais
)
```

Uma **lista** de colunas em `groupby` cria um grupo para cada combinação.

## Operações mais usadas

| Operação | Nome | Observação |
|---|---|---|
| contar linhas | `"count"` | conta valores não vazios |
| média | `"mean"` | sensível a valores extremos |
| mediana | `"median"` | valor do meio; resiste a extremos |
| soma | `"sum"` | totais |
| mínimo / máximo | `"min"` / `"max"` | |
| percentil | `df["col"].quantile(0.75)` | 0.75 = percentil 75 |

## Ordenar

| Quero | Código |
|---|---|
| da maior para a menor | `.sort_values("coluna", ascending=False)` |
| da menor para a maior | `.sort_values("coluna")` |
| por ordem do índice (por exemplo, datas) | `.sort_index()` |
| só os N primeiros | `.head(N)` |

## Mediana ou média?

- **Mediana:** a publicação típica; metade dos valores fica acima e metade abaixo. Uma publicação isolada que viralizou pouco a afeta.
- **Média:** soma dividida pela quantidade; é puxada por extremos.

## Cuidados de leitura

- Grupos com **poucas publicações** geram medianas instáveis. Sempre mostre o `n` (contagem).
- Se duas medianas são quase iguais (por exemplo, 1,15 e 1,13), diga que é praticamente um empate.
- Uma média de médias diárias dá o mesmo peso a dias com 2 e com 3 publicações.
