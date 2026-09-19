# Guia 03: Limpeza de dados

Sempre parta do arquivo bruto em uma **nova variável**. Depois de cada limpeza, **guarde o resultado** (`df = df...`).

## 1. Remover duplicidade

```python
df["id_publicacao"] = df["id_publicacao"].str.strip()      # tira espaços do id
df = df.drop_duplicates(subset="id_publicacao")            # mantém a 1ª de cada id
```

`subset="id_publicacao"` compara só pelo id. Sem o `subset`, o pandas compara a linha inteira.

## 2. Padronizar texto (tema, formato...)

```python
df["tema"] = df["tema"].str.strip().str.lower()   # sem espaços nas pontas e minúsculo
```

Antes, veja as grafias com `df["tema"].value_counts()`.

**Acentos** (por exemplo `Saúde` e `saude`): duas opções.

```python
# opção A: trocar valores específicos
df["tema"] = df["tema"].replace({"saúde": "saude"})

# opção B: tirar acentos de tudo
df["tema"] = (df["tema"].str.normalize("NFKD")
              .str.encode("ascii", errors="ignore").str.decode("utf-8"))
```

Depois de padronizar, rode `value_counts()` de novo: só devem sobrar os valores certos.

## 3. Converter datas

Quando há **mais de um formato** de data, tente cada formato e junte:

```python
d1 = pd.to_datetime(df["data_publicacao"], format="%Y-%m-%d %H:%M", errors="coerce")
d2 = pd.to_datetime(df["data_publicacao"], format="%Y/%m/%d %H:%M", errors="coerce")
d3 = pd.to_datetime(df["data_publicacao"], format="%d/%m/%Y %H:%M", errors="coerce")
df["data_publicacao"] = d1.fillna(d2).fillna(d3)
print("Datas não convertidas:", df["data_publicacao"].isna().sum())   # deve dar 0
```

Códigos de formato: `%Y` ano com 4 dígitos, `%m` mês, `%d` dia, `%H` hora, `%M` minuto.
`errors="coerce"` deixa vazio (NaN) o que não converte, em vez de dar erro.
Para um formato só: `pd.to_datetime(df["coluna"])`.

**Dia/mês ou mês/dia?** Olhe a sequência das outras datas. Se `05/08/2026` está entre `04/08` e `06/08`, é 5 de agosto (dia primeiro).

## 4. Converter números

```python
colunas_num = ["alcance", "curtidas", "comentarios"]
for col in colunas_num:
    df[col] = pd.to_numeric(df[col], errors="coerce")   # texto inválido vira NaN
```

## 5. Ausências e valores inválidos

Decida e **justifique**. Regra do enunciado: "sem criar informação que não esteja nos arquivos".

| Opção | Código | Quando usar | Justificativa |
|---|---|---|---|
| Descartar linhas sem coluna essencial | `df = df.dropna(subset=["a", "b"])` | A coluna entra na fórmula ou no cálculo | Não dá para calcular; preencher inventaria dado |
| Manter a linha | (nada) | A coluna vazia não é usada na análise | Descartar perderia informação sem motivo |
| Preencher com a mediana | `df["x"] = df["x"].fillna(df["x"].median())` | Só se o enunciado aceitar preenchimento | Cria informação: justifique e use com cuidado |

Descarte apenas as linhas **com vazio nas colunas que a fórmula usa**. Uma ausência em coluna que não entra na conta não obriga a descartar.

**Valores inválidos** (por exemplo, alcance zero ou negativo, quando o alcance é denominador): filtre com `df = df[df["alcance"] > 0]` e cite no texto.

## 6. Criar coluna com fórmula

```python
df["taxa_utilidade_pct"] = (
    (df["compartilhamentos"] + df["salvamentos"]) / df["alcance"] * 100
)
```

Use parênteses na soma **antes** de dividir.

## 7. Conferências depois da limpeza

```python
print(df.shape)                      # quantas linhas restaram
print(df["tema"].value_counts())     # temas padronizados
print(df.isna().sum())               # ausências restantes
```
