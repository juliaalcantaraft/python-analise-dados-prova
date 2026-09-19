# Questão 4: Acompanhamento diário

## Enunciado

A equipe de comunicação precisa acompanhar o desempenho das publicações ao longo dos dias da campanha. Use somente `dados/publicacoes_analise.csv`, em uma nova variável.

1. Converta `data_publicacao` para data/hora e crie `dia_publicacao`, contendo somente a data.
2. Crie uma tabela por `dia_publicacao` com a quantidade de publicações, a média de `taxa_engajamento_pct` e o alcance total. Ordene-a cronologicamente.
3. Faça um gráfico de linhas da taxa média de engajamento por dia. Inclua título, eixos nomeados e a fonte "Fonte: dados sintéticos do Festival ViraBairro (2026)" no próprio gráfico.
4. Crie um recorte apenas de `reel` publicados a partir das 18h. Mostre as cinco publicações desse recorte com maior `taxa_engajamento_pct`, incluindo `id_publicacao`, `dia_publicacao`, `hora`, `tema` e `taxa_engajamento_pct`.

Na resposta final, descreva a variação diária sem afirmar tendência de longo prazo e explique por que o recorte de reels noturnos não prova que horário ou formato causam engajamento.

## Código

```python
# Questão 4
df_diario = pd.read_csv("dados/publicacoes_analise.csv")

# 1) Converter a data e criar o dia (somente a data)
df_diario["data_publicacao"] = pd.to_datetime(df_diario["data_publicacao"])
df_diario["dia_publicacao"] = df_diario["data_publicacao"].dt.date

# 2) Tabela diária, em ordem cronológica
tabela_diaria = (
    df_diario.groupby("dia_publicacao")
    .agg(
        publicacoes=("id_publicacao", "count"),
        taxa_engajamento_media=("taxa_engajamento_pct", "mean"),
        alcance_total=("alcance", "sum"),
    )
    .sort_index()
    .round(2)
)
print(tabela_diaria)

# 3) Gráfico de linhas: taxa média de engajamento por dia
fig, ax = plt.subplots(figsize=(10, 5))
ax.plot(tabela_diaria.index, tabela_diaria["taxa_engajamento_media"], marker="o")
ax.set_title("Como variou a taxa média de engajamento por dia de publicação?")
ax.set_xlabel("Dia de publicação")
ax.set_ylabel("Taxa média de engajamento (%)")
plt.xticks(rotation=45)
fig.text(0.01, 0.01, "Fonte: dados sintéticos do Festival ViraBairro (2026)", fontsize=8, ha="left")
plt.tight_layout(rect=[0, 0.04, 1, 1])

# 4) Recorte: reels publicados a partir das 18h, cinco maiores taxas
reels_noturnos = df_diario[(df_diario["formato"] == "reel") & (df_diario["hora"] >= 18)]

top5_reels = (
    reels_noturnos
    .sort_values("taxa_engajamento_pct", ascending=False)
    .head(5)[["id_publicacao", "dia_publicacao", "hora", "tema", "taxa_engajamento_pct"]]
)
print(top5_reels.to_string(index=False))
```

## Saída de referência

- 56 dias na tabela (01/08 a 28/08 e 01/09 a 28/09); não há publicações de 29 a 31/08.
- 2 ou 3 publicações por dia.
- Menor taxa média diária: 3,07% (24/08). Maior: 5,84% (23/09).
- `alcance_total` sobe ao longo de cada ciclo, mas a taxa de engajamento não acompanha.
- 21 reels a partir das 18h.

| id_publicacao | dia_publicacao | hora | tema | taxa_engajamento_pct |
|---|---|---|---|---|
| OCB-003 | 2026-08-03 | 22 | cultura | 6,82 |
| OCB-107 | 2026-09-23 | 18 | mobilidade | 6,80 |
| OCB-087 | 2026-09-03 | 22 | cultura | 6,15 |
| OCB-097 | 2026-09-13 | 20 | mobilidade | 6,08 |
| OCB-027 | 2026-08-27 | 20 | mobilidade | 5,64 |

## Resposta (Markdown)

**Variação diária.** A taxa média de engajamento oscilou entre 3,07% (24/08) e 5,84% (23/09), com dias mais altos e mais baixos se alternando ao longo de todo o período, sem uma subida ou queda contínua. Cada dia tem apenas 2 ou 3 publicações, então a média diária muda bastante por causa de um único post, e a base cobre poucas semanas de dados sintéticos, o que não permite falar em tendência de longo prazo. Também não há publicações entre 29 e 31/08, e o gráfico liga 28/08 direto a 01/09.

**Reels noturnos.** O recorte mostra os cinco reels publicados a partir das 18h com maior taxa (entre 5,64% e 6,82%), mas isso não prova que horário ou formato causam engajamento. Eles foram escolhidos justamente por terem os melhores resultados, e não há comparação com outros horários e formatos. Além disso, esses reels diferem em tema, dia e perfil do autor, fatores que também podem explicar a taxa. São poucos casos de dados sintéticos, então o que se observa é uma associação, e não uma relação de causa e efeito.

## Guias relacionados

[06 Datas, séries e filtros](../guias/06_datas_series_e_filtros.md), [05 Gráficos](../guias/05_graficos.md)
