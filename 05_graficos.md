# Guia 05: Gráficos

Todo gráfico do simulado pediu: **título**, **eixos nomeados** e, em vários, a **fonte** dentro do gráfico. Confira sempre essa lista.

Não use `plt.show()` no site da prova (ver arquivo de ambiente). Se o gráfico não aparecer, teste sem essa linha.

## Modelo base

```python
fig, ax = plt.subplots(figsize=(8, 5))    # cria a "folha" (fig) e o gráfico (ax)
# ... aqui vai o desenho (bar, barh, plot, scatter) ...
ax.set_title("Título que comunica a pergunta?")
ax.set_xlabel("Nome do eixo X")
ax.set_ylabel("Nome do eixo Y")
fig.text(0.01, 0.01, "Fonte: dados sintéticos do Festival ViraBairro (2026)", fontsize=8, ha="left")
plt.tight_layout(rect=[0, 0.04, 1, 1])    # deixa espaço embaixo para a fonte
```

`fig.text` escreve a fonte **dentro da figura**, como o enunciado exige ("no próprio gráfico").

## Barras verticais (comparar categorias)

```python
barras = ax.bar(tabela.index, tabela["mediana"])
ax.bar_label(barras, fmt="%.2f")           # escreve o valor sobre cada barra
```

## Barras horizontais (muitas categorias ou nomes longos)

```python
barras = ax.barh(rotulos, valores)
ax.invert_yaxis()                          # a maior fica no topo
ax.bar_label(barras, fmt="%.2f")
```

Rótulo combinado de duas colunas:
```python
rotulos = tabela["tema"] + " | " + tabela["formato"]
# se incluir número, converta: tabela["publicacoes"].astype(str)
```

## Linhas (evolução por dia)

```python
ax.plot(tabela.index, tabela["taxa_media"], marker="o")
plt.xticks(rotation=45)                    # inclina as datas do eixo X
```

## Dispersão com linha de referência (real × previsto)

```python
ax.scatter(y_teste, previsao)
minimo = min(y_teste.min(), previsao.min())
maximo = max(y_teste.max(), previsao.max())
ax.plot([minimo, maximo], [minimo, maximo], linestyle="--", color="gray", label="Previsão = valor real")
ax.legend()
```

Quanto mais perto da linha tracejada, melhor a previsão.

## Boas práticas

- **Título informativo:** diga a pergunta ("Qual tema tem a maior taxa de utilidade?") ou o achado.
- **Eixos nomeados com unidade:** "Taxa de engajamento (%)".
- **Não corte o eixo Y** para exagerar diferenças pequenas (barras partem de zero).
- Barras **ordenadas** tornam a comparação mais fácil.
- Lacunas nas datas (dias sem dados) fazem a linha ligar dois dias distantes com uma reta. Isso não é "zero", é ausência de dados.
