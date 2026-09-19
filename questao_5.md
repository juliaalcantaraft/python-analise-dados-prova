# Questão 5: Tema e formato das publicações

## Enunciado

A equipe de comunicação quer entender como tema e formato aparecem juntos nas publicações. Analisar cada variável separadamente pode ocultar diferenças: um tema pode ter resultados distintos em vídeo, carrossel ou imagem.

Use somente `dados/publicacoes_analise.csv`. Crie uma tabela com uma linha para cada combinação de `tema` e `formato`, contendo:

1. número de publicações; e
2. mediana de `taxa_engajamento_pct`.

Ordene a tabela da maior para a menor mediana. Depois, faça um gráfico de barras que compare as combinações de tema e formato. O gráfico deve ter título informativo, eixos nomeados e a fonte "Fonte: dados sintéticos do Festival ViraBairro (2026)" no próprio gráfico.

Na Markdown, escreva de 4 a 6 frases: destaque uma combinação que mereça ser testada pela equipe, explique por que comparar duas variáveis é diferente de analisar apenas uma e registre uma limitação da base.

## Código

```python
# Questão 5
df_comb = pd.read_csv("dados/publicacoes_analise.csv")

# Tabela por combinação de tema e formato, da maior para a menor mediana
tabela_comb = (
    df_comb.groupby(["tema", "formato"])
    .agg(
        publicacoes=("id_publicacao", "count"),
        mediana_engajamento=("taxa_engajamento_pct", "median"),
    )
    .sort_values("mediana_engajamento", ascending=False)
    .round(2)
    .reset_index()
)
print(tabela_comb.to_string(index=False))

# Gráfico de barras horizontais: uma barra por combinação
rotulos = (
    tabela_comb["tema"] + " | " + tabela_comb["formato"]
    + " (n=" + tabela_comb["publicacoes"].astype(str) + ")"
)

fig, ax = plt.subplots(figsize=(10, 7))
barras = ax.barh(rotulos, tabela_comb["mediana_engajamento"])
ax.invert_yaxis()   # a maior mediana fica no topo
ax.set_title("Quais combinações de tema e formato têm a maior mediana de engajamento?")
ax.set_xlabel("Mediana da taxa de engajamento (%)")
ax.set_ylabel("Combinação de tema e formato")
ax.bar_label(barras, fmt="%.2f")
fig.text(0.01, 0.01, "Fonte: dados sintéticos do Festival ViraBairro (2026)", fontsize=8, ha="left")
plt.tight_layout(rect=[0, 0.04, 1, 1])
```

## Saída de referência

| tema | formato | publicacoes | mediana_engajamento |
|---|---|---|---|
| mobilidade | reel | 11 | 5,10 |
| cultura | reel | 8 | 4,82 |
| saude | reel | 12 | 4,67 |
| cultura | carrossel | 10 | 4,54 |
| mobilidade | carrossel | 10 | 4,53 |
| cultura | imagem | 8 | 4,39 |
| saude | carrossel | 11 | 4,29 |
| trabalho | reel | 8 | 4,27 |
| trabalho | carrossel | 10 | 3,73 |
| mobilidade | imagem | 12 | 3,60 |
| saude | imagem | 12 | 3,38 |
| trabalho | imagem | 8 | 3,04 |

Os `n` somam 120. Reel ocupa as três primeiras posições; três das quatro combinações com imagem ficam nas três últimas.

## Resposta (Markdown)

**Resposta da Questão 5**

A combinação **mobilidade + reel** teve a maior mediana de engajamento (5,10%) e merece ser testada pela equipe em novas publicações, com acompanhamento do resultado. Analisar tema e formato juntos é diferente de analisar cada um separadamente porque o efeito de um pode depender do outro: em mobilidade, a mediana vai de 5,10% no reel a 3,60% na imagem, enquanto em cultura ela varia pouco entre os formatos (de 4,82% a 4,39%), e a média do tema sozinho esconde essa diferença. Como limitação, ao dividir a base em 12 combinações, cada uma fica com apenas 8 a 12 publicações, então as medianas são instáveis e a diferença entre as primeiras colocadas (5,10% contra 4,82%) pode ser casual. Além disso, os dados são sintéticos e de uma única campanha, então o resultado mostra uma associação observada e não prova que o formato ou o tema causem mais engajamento.

Atenção: a Questão 3 usa a taxa de **utilidade**; esta usa a taxa de **engajamento**. São métricas diferentes.

## Guias relacionados

[04 Métricas por grupo](../guias/04_metricas_por_grupo.md), [05 Gráficos](../guias/05_graficos.md)
