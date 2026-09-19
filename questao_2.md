# Questão 2: Tratamento dos registros

## Enunciado

A base recebida tem nomes de temas escritos de formas diferentes, datas em formatos distintos, um registro duplicado e alguns valores ausentes. Prepare os dados sem criar informação que não esteja nos arquivos.

Parta novamente de `dados/publicacoes_brutas.csv`, em uma nova variável. Faça o tratamento necessário para:

1. remover duplicidade por `id_publicacao`;
2. padronizar os valores de `tema` para uma forma consistente;
3. converter `data_publicacao` e as colunas numéricas usadas no cálculo para tipos adequados;
4. tratar ausências ou valores inválidos de maneira justificada; e
5. criar `taxa_utilidade_pct`, definida por: **(compartilhamentos + salvamentos) ÷ alcance × 100** *(fórmula confirmada; na cópia original veio em branco)*.

Depois, produza uma tabela por tema com o número de publicações e a mediana de `taxa_utilidade_pct`, ordenada da maior para a menor mediana. Não precisa salvar arquivo.

Na Markdown, registre as decisões de limpeza em até quatro frases. Se descartar, preencher ou converter algo, diga por quê.

## Diagnóstico que orientou a limpeza

| Achado | Onde | Ação |
|---|---|---|
| Temas com grafias diferentes | `Cultura`, `mobilidade ` (espaço no fim), `Saúde` | strip, lower e trocar `saúde` por `saude` |
| Três formatos de data | `2026-08-01 12:00`, `05/08/2026 18:00`, `2026/08/04 09:00` | três `to_datetime` com `fillna` |
| 1 id duplicado | OCB-018 (linhas idênticas) | `drop_duplicates(subset="id_publicacao")` |
| 3 ausências em linhas diferentes | OCB-021 sem `alcance`; OCB-025 sem `comentarios`; OCB-029 sem `salvamentos` | descartar só quem não tem coluna da fórmula |
| Tipos numéricos | já corretos (`float64`, `int64`) | `to_numeric` como precaução |

## Código

```python
# Questão 2
# Recarregue dados/publicacoes_brutas.csv em uma nova variável.
# Faça a limpeza solicitada e calcule taxa_utilidade_pct.
# Produza a tabela-resumo por tema, com quantidade de publicações e mediana da taxa.

df_tratado = pd.read_csv("dados/publicacoes_brutas.csv")

# 1) Remover duplicidade por id_publicacao
df_tratado["id_publicacao"] = df_tratado["id_publicacao"].str.strip()
df_tratado = df_tratado.drop_duplicates(subset="id_publicacao")

# 2) Padronizar tema: sem espaços nas pontas, minúsculo e sem o acento de "saúde"
df_tratado["tema"] = df_tratado["tema"].str.strip().str.lower().replace({"saúde": "saude"})

# 3) Converter tipos: datas (três formatos) e colunas numéricas
d1 = pd.to_datetime(df_tratado["data_publicacao"], format="%Y-%m-%d %H:%M", errors="coerce")
d2 = pd.to_datetime(df_tratado["data_publicacao"], format="%Y/%m/%d %H:%M", errors="coerce")
d3 = pd.to_datetime(df_tratado["data_publicacao"], format="%d/%m/%Y %H:%M", errors="coerce")
df_tratado["data_publicacao"] = d1.fillna(d2).fillna(d3)

colunas_num = ["alcance", "curtidas", "comentarios", "compartilhamentos", "salvamentos"]
for col in colunas_num:
    df_tratado[col] = pd.to_numeric(df_tratado[col], errors="coerce")

# 4) Ausências: descartar só as linhas sem alguma coluna usada na fórmula
colunas_formula = ["compartilhamentos", "salvamentos", "alcance"]
df_tratado = df_tratado.dropna(subset=colunas_formula)

# 5) Taxa de utilidade
df_tratado["taxa_utilidade_pct"] = (
    (df_tratado["compartilhamentos"] + df_tratado["salvamentos"])
    / df_tratado["alcance"] * 100
)

# Tabela por tema: número de publicações e mediana, da maior para a menor
tabela_tema = (
    df_tratado.groupby("tema")["taxa_utilidade_pct"]
    .agg(publicacoes="count", mediana="median")
    .sort_values("mediana", ascending=False)
    .round(2)
)
print(tabela_tema)
```

## Saída de referência

Após o diagnóstico: 35 linhas depois de remover a duplicata; temas: saude 12, trabalho 8, mobilidade 8, cultura 7; 0 datas não convertidas.

**Não conferido durante o estudo:** a tabela final. O esperado é **33 publicações** (35 menos OCB-021 e OCB-029): saude 11, mobilidade 8, trabalho 7, cultura 7. Confira a soma.

## Explicação

- O `dropna(subset=...)` remove só as linhas com vazio nas colunas da fórmula. OCB-025 fica, porque falta `comentarios`, que não entra na conta.
- Não se preenche com média ou zero, porque isso cria informação que não está nos arquivos.

## Resposta (Markdown)

**Decisões de limpeza da Questão 2**

Removi a duplicata OCB-018 pelo `id_publicacao`, porque as duas linhas eram idênticas e nenhuma informação se perde. Padronizei os temas para minúsculas, sem espaços nas pontas e sem acento, unindo "Cultura", "mobilidade " e "Saúde" aos seus grupos. Converti `data_publicacao` para data/hora, lendo os três formatos presentes, e mantive as colunas numéricas como número. Descartei OCB-021 (sem alcance) e OCB-029 (sem salvamentos), porque a taxa de utilidade não pode ser calculada sem essas colunas e preencher com média ou zero criaria informação que não está nos arquivos; mantive OCB-025, que só não tem comentários, coluna fora da fórmula.

## Guias relacionados

[03 Limpeza](../guias/03_limpeza_de_dados.md), [04 Métricas por grupo](../guias/04_metricas_por_grupo.md)
