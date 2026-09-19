# Questão 1: Diagnóstico inicial da base

## Enunciado

A equipe recebeu uma base sem documentação prévia. Antes de usá-la nas decisões de comunicação, precisa fazer um diagnóstico inicial dos registros.

Em uma célula de código, carregue `dados/publicacoes_brutas.csv` e mostre:

- as cinco primeiras linhas;
- a quantidade de linhas e colunas; e
- a quantidade de valores ausentes por coluna, mostrando somente as colunas que têm ao menos uma ausência.

Na Markdown, explique em até duas frases uma limitação que impeça tratar esses resultados como retrato de todas as redes, públicos ou bairros.

## Código

```python
# Carregue dados/publicacoes_brutas.csv e faça a inspeção solicitada.
# Mostre as cinco primeiras linhas, as dimensões e somente as colunas com valores ausentes.

df_brutas = pd.read_csv("dados/publicacoes_brutas.csv")

# 1) Cinco primeiras linhas
print("Cinco primeiras linhas:")
print(df_brutas.head(5))

# 2) Quantidade de linhas e colunas
linhas, colunas = df_brutas.shape
print(f"\nA base tem {linhas} linhas e {colunas} colunas.")

# 3) Valores ausentes por coluna (somente colunas com pelo menos 1 ausência)
ausentes = df_brutas.isna().sum()
ausentes = ausentes[ausentes > 0]
print("\nValores ausentes por coluna (somente colunas com ausência):")
print(ausentes)
```

## Saída de referência

- 36 linhas e 12 colunas.
- Ausências: `alcance` 1, `comentarios` 1, `salvamentos` 1.
- Na 5ª linha (índice 4) a data está como `05/08/2026 18:00` (dia primeiro), diferente das demais (`2026-08-01 12:00`): falha de padronização que `isna()` não detecta.

## Explicação

- `read_csv` lê o arquivo; `head(5)` mostra as 5 primeiras linhas; `shape` devolve (linhas, colunas).
- `isna().sum()` conta vazios por coluna; `[ausentes > 0]` mantém só as colunas com ao menos uma ausência.
- Nesta questão só se **diagnostica**; a limpeza é a Questão 2.

## Resposta (Markdown)

**Resposta da Questão 1: limite de uso**

Os dados são sintéticos e cobrem apenas as publicações desta campanha, feitas nas oito semanas anteriores ao festival, e a base tem só 36 registros, então ela não representa outras redes, públicos ou bairros. Por isso, os resultados descrevem só esse histórico e não podem ser generalizados nem interpretados como causa e efeito.

## Guias relacionados

[02 Inspeção](../guias/02_inspecao_da_base.md), [10 Textos](../guias/10_como_escrever_as_respostas.md)
