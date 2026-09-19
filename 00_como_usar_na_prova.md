# Como usar este material na prova

## O método de 6 passos

1. **Leia o enunciado inteiro** e sublinhe os verbos: carregue, mostre, remova, padronize, converta, crie, calcule, ordene, faça o gráfico, compare, explique.
2. **Numere os pedidos.** Cada verbo vira um bloco de código (ou uma frase de texto).
3. **Ache a receita** na tabela abaixo e abra o guia indicado.
4. **Copie e troque** só o que a tabela "O que trocar" mostrar (nomes de coluna, caminho do arquivo, números).
5. **Rode em partes.** Depois de cada bloco, olhe a saída: o número de linhas está certo? Apareceram as colunas esperadas? Erro? Veja o arquivo de erros comuns.
6. **Escreva o texto** da Markdown usando um modelo do guia 10, com os números da sua saída.

## Da frase do enunciado ao código

| Se o enunciado diz... | Use | Guia |
|---|---|---|
| carregue o arquivo, mostre as primeiras linhas | `pd.read_csv`, `head` | 02 |
| quantidade de linhas e colunas | `df.shape` | 02 |
| valores ausentes por coluna | `isna().sum()` | 02 |
| remover duplicidade | `drop_duplicates(subset=...)` | 03 |
| padronizar texto / nomes escritos de formas diferentes | `str.strip().str.lower()` | 03 |
| converter data / tipos adequados | `pd.to_datetime`, `pd.to_numeric` | 03 |
| tratar ausências de forma justificada | `dropna(subset=...)` ou `fillna` | 03 |
| criar coluna com fórmula | `df["nova"] = ...` | 03 |
| tabela por tema / formato / dia | `groupby(...).agg(...)` | 04 |
| mediana, média, soma, contagem | `median`, `mean`, `sum`, `count` | 04 |
| ordenar da maior para a menor | `sort_values(..., ascending=False)` | 04 |
| gráfico de barras / linhas / dispersão | `plt.subplots`, `ax.bar`, `ax.plot`, `ax.scatter` | 05 |
| título, eixos, fonte no gráfico | `set_title`, `set_xlabel`, `set_ylabel`, `fig.text` | 05 |
| por dia / criar coluna só com a data | `dt.date` | 06 |
| recorte / apenas X a partir de Y | filtro com `&` | 06 |
| cinco maiores | `sort_values(...).head(5)` | 06 |
| percentil 75 / criar alvo 0/1 | `quantile(0.75)` | 07 |
| transformar categorias em números | `pd.get_dummies` | 07 |
| treino e teste, preservando proporção | `train_test_split(..., stratify=y)` | 07 |
| árvore, importância das características | `DecisionTreeClassifier`, `feature_importances_` | 07 |
| estimar um valor numérico, MAE, R² | `LinearRegression`, `DecisionTreeRegressor` | 08 |
| classificar, precisão, recall, F1 | `precision_score`, `recall_score`, `f1_score` | 09 |
| matriz de confusão | `confusion_matrix` | 09 |
| corte / limiar 0,50 e 0,30 | `predict_proba` | 09 |
| explique, recomende, cite limitação | modelos de texto | 10 |

## O que trocar ao adaptar um código

| O que aparece no código | Troque por |
|---|---|
| `"dados/publicacoes_analise.csv"` | o caminho do arquivo da prova |
| `"tema"`, `"formato"`, `"alcance"`... | os nomes de coluna da prova (copie do dicionário, respeitando maiúsculas e acentos) |
| `df_...` (nome da variável) | qualquer nome, mas use o mesmo em todo o bloco |
| `quantile(0.75)` | o percentil pedido (0.90 para percentil 90, por exemplo) |
| `test_size=0.25` | a proporção de teste pedida (25% de teste = 0.25) |
| `random_state=42` | o valor pedido |
| `max_depth=4` | a profundidade pedida |
| `[0.50, 0.30]` | os cortes pedidos |
| `.head(5)` | o número pedido |
| `>= 18` e `"reel"` | o filtro pedido |
| `median` / `mean` | a estatística pedida |
| título e rótulos do gráfico | os do enunciado |

## Checklist final antes de entregar

- [ ] Cada pedido do enunciado tem uma linha ou um bloco de código correspondente.
- [ ] Rodei a célula do início ao fim sem erro.
- [ ] Olhei a saída e ela faz sentido (contagens, ordem, valores).
- [ ] Gráfico: título, eixo X, eixo Y e fonte (se pedida).
- [ ] Texto: dentro do limite de frases e sem afirmar **causalidade**.
- [ ] Em previsão: usei só variáveis conhecidas **antes** (sem vazamento de dados).
- [ ] Reli o enunciado uma última vez procurando algo que eu tenha pulado.
