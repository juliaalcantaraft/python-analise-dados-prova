# Questão 3: Escolha de tema para divulgação

## Enunciado

A equipe de comunicação trabalha com quatro temas e só consegue dar destaque principal a um deles na divulgação do festival. A escolha deve priorizar conteúdo que as pessoas tendem a salvar ou compartilhar.

Use somente `dados/publicacoes_analise.csv`. Crie `taxa_utilidade_pct` com a mesma fórmula da questão anterior. Em seguida:

1. monte uma tabela com a mediana da taxa por tema; e
2. faça um gráfico de barras que permita comparar os quatro temas.

O gráfico deve ter título que comunique a pergunta, eixos nomeados e a indicação "Fonte: dados sintéticos do Festival ViraBairro (2026)" no próprio gráfico.

Na Markdown, escreva uma recomendação de 3 a 5 frases para a equipe: indique um tema para receber o destaque principal, explique o que a mediana representa e apresente uma limitação. A escolha é de comunicação; não afirme causalidade.

## Código

```python
# Questão 3
# Carregue dados/publicacoes_analise.csv e crie taxa_utilidade_pct.
# Calcule a mediana por tema e faça o gráfico de barras solicitado.
# Inclua título, rótulos dos eixos e a fonte no próprio gráfico.

df_analise = pd.read_csv("dados/publicacoes_analise.csv")

# Fórmula (mesma da Questão 2)
df_analise["taxa_utilidade_pct"] = (
    (df_analise["salvamentos"] + df_analise["compartilhamentos"])
    / df_analise["alcance"] * 100
)

# 1) Tabela: mediana da taxa por tema, da maior para a menor
tabela_mediana = (
    df_analise.groupby("tema")["taxa_utilidade_pct"]
    .median()
    .sort_values(ascending=False)
    .round(2)
    .to_frame("mediana_taxa_utilidade_pct")
)
print(tabela_mediana)

# 2) Gráfico de barras
fig, ax = plt.subplots(figsize=(8, 5))
barras = ax.bar(tabela_mediana.index, tabela_mediana["mediana_taxa_utilidade_pct"])
ax.set_title("Qual tema tem maior taxa de utilidade (salvamentos + compartilhamentos por alcance)?")
ax.set_xlabel("Tema")
ax.set_ylabel("Mediana da taxa de utilidade (%)")
ax.bar_label(barras, fmt="%.2f")
fig.text(0.01, 0.01, "Fonte: dados sintéticos do Festival ViraBairro (2026)", fontsize=8, ha="left")
plt.tight_layout(rect=[0, 0.04, 1, 1])
```

Sem `plt.show()`: no site do professor ele gerou um aviso e não exibiu o gráfico.

## Saída de referência

Contagens na base: saude 35, mobilidade 33, trabalho 26, cultura 26.

| Tema | Mediana da taxa de utilidade |
|---|---|
| mobilidade | 1,15 |
| cultura | 1,13 |
| saude | 1,10 |
| trabalho | 0,96 |

Mobilidade lidera por apenas 0,02 ponto sobre cultura (praticamente empate).

## Resposta (Markdown)

**Recomendação para a equipe**

Recomendo dar o destaque principal ao tema **mobilidade**, que teve a maior mediana de taxa de utilidade (1,15%), seguido de perto por cultura (1,13%) e saúde (1,10%). A mediana representa a publicação típica de cada tema: metade das publicações ficou acima desse valor e metade abaixo, o que reduz o peso de casos isolados. A taxa mede quantas pessoas salvaram ou compartilharam em relação ao alcance, e por isso serve para priorizar conteúdo que as pessoas tendem a guardar ou repassar. Como limitação, a diferença entre mobilidade e cultura é de apenas 0,02 ponto percentual, quase um empate, e os dados são sintéticos, de uma única campanha e com poucas publicações por tema. Por isso a escolha é uma decisão de comunicação apoiada numa associação observada, e não prova que o tema cause mais salvamentos e compartilhamentos.

## Guias relacionados

[04 Métricas por grupo](../guias/04_metricas_por_grupo.md), [05 Gráficos](../guias/05_graficos.md), [10 Textos](../guias/10_como_escrever_as_respostas.md)
