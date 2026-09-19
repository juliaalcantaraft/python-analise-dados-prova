# Guia 02: Inspeção da base

Objetivo: **ver** a tabela e o que tem de errado nela **antes** de usar. Nesta etapa você não corrige nada.

## Receita completa

```python
df = pd.read_csv("dados/publicacoes_brutas.csv")

print(df.head(5))                  # cinco primeiras linhas
linhas, colunas = df.shape         # (linhas, colunas)
print(f"A base tem {linhas} linhas e {colunas} colunas.")

ausentes = df.isna().sum()         # vazios por coluna
ausentes = ausentes[ausentes > 0]  # só colunas com pelo menos 1 ausência
print(ausentes)
```

Não use `display(...)`. Ele não existe no site da prova (use `print`).

## Outros comandos de inspeção

| Quero ver | Comando |
|---|---|
| nomes das colunas | `df.columns.tolist()` |
| tipo de cada coluna | `df.dtypes` ou `df.info()` |
| mínimo, máximo, média das colunas numéricas | `df.describe()` |
| quantas vezes cada valor aparece | `df["tema"].value_counts()` |
| valores diferentes de uma coluna | `df["tema"].unique()` |
| quantas linhas duplicadas | `df.duplicated().sum()` |
| quantos ids repetidos | `df["id_publicacao"].duplicated().sum()` |
| as linhas duplicadas | `df[df["id_publicacao"].duplicated(keep=False)]` |
| todas as datas | `df["data_publicacao"].tolist()` |
| linhas com algum vazio | `df[df.isna().any(axis=1)]` |

## Como ler

- **Valor ausente** é uma célula vazia, mostrada como `NaN`. Uma coluna com ausência ganha `.0` (por exemplo `929.0`), porque o pandas guarda o vazio como número decimal.
- **`isna()` só conta células vazias de verdade.** Um `"-"`, `"n/d"` ou `"sem dado"` é texto e **não** é contado. Se `dtypes` mostrar `object` em uma coluna que deveria ser número, há texto misturado.
- **Falha de padronização** (data em outro formato, tema com maiúscula ou espaço) **não** aparece em `isna()`. Você a encontra com `value_counts()` e olhando as datas.
- `head()` **não revela duplicatas**. Use `duplicated()`.

## O que costuma ser cobrado no texto

Uma limitação que impede tratar os resultados como retrato de todas as redes, públicos ou bairros: dados sintéticos, uma só campanha, um período curto e poucos registros. Modelo pronto no guia 10.
