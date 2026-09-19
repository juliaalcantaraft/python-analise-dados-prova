# Guia 06: Datas, séries por dia e filtros

## Converter e extrair partes da data

```python
df["data_publicacao"] = pd.to_datetime(df["data_publicacao"])
df["dia_publicacao"] = df["data_publicacao"].dt.date       # só a data
df["hora_pub"] = df["data_publicacao"].dt.hour             # hora (0 a 23)
df["dia_sem"] = df["data_publicacao"].dt.dayofweek         # segunda = 0 ... domingo = 6
df["mes"] = df["data_publicacao"].dt.month
```

`.dt` dá acesso às funções de data de uma coluna.

## Tabela por dia, em ordem cronológica

```python
tabela_diaria = (
    df.groupby("dia_publicacao")
    .agg(
        publicacoes=("id_publicacao", "count"),
        taxa_engajamento_media=("taxa_engajamento_pct", "mean"),
        alcance_total=("alcance", "sum"),
    )
    .sort_index()          # ordena pelo dia
    .round(2)
)
print(tabela_diaria)
```

## Filtros (recortes)

```python
# reels publicados a partir das 18h
recorte = df[(df["formato"] == "reel") & (df["hora"] >= 18)]
print("Linhas no recorte:", len(recorte))
```

- "a partir de 18h" inclui as 18h: `>= 18`. "Depois das 18h" seria `> 18`.
- Confira antes como o valor está escrito: `df["formato"].value_counts()`.

## Os N maiores

```python
top5 = (
    recorte
    .sort_values("taxa_engajamento_pct", ascending=False)
    .head(5)[["id_publicacao", "dia_publicacao", "hora", "tema", "taxa_engajamento_pct"]]
)
print(top5.to_string(index=False))
```

Os colchetes duplos no final escolhem só as colunas pedidas. Se o recorte tiver menos de 5 linhas, aparecem menos.

## Como descrever a variação diária

- Descreva com verbos neutros: "oscilou entre X% (dia) e Y% (dia)", "caiu de A para B".
- **Evite** "tendência", "melhorou ao longo da campanha", "vai continuar". São poucas semanas de dados sintéticos.
- Diga que cada dia tem poucas publicações (2 ou 3), então a média diária muda muito por causa de um post.
- Cite lacunas de datas, se houver.

## Por que um recorte (top 5) não prova causalidade

1. **Seleção pelo resultado:** os cinco foram escolhidos por terem a maior taxa.
2. **Sem comparação:** não se olhou outros horários e formatos.
3. **Fatores misturados:** tema, dia, seguidores e legenda também podem explicar.
4. **Poucos casos** e dados sintéticos.
