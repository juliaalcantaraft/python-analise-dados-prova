# Python para análise de dados: material de estudo e consulta para a prova

Material de Júlia Ferraz, construído a partir do simulado **Festival ViraBairro** (8 questões).
Tudo aqui usa dados **sintéticos** (fictícios), criados só para treino.

## Por onde começar

1. Leia **[00_como_usar_na_prova.md](00_como_usar_na_prova.md)**. Ele explica o método de 6 passos e tem a tabela que liga cada frase do enunciado ao código certo.
2. Abra **[01_ambiente_e_erros_comuns.md](01_ambiente_e_erros_comuns.md)** antes da prova. Ele lista os erros que já apareceram no site do professor (por exemplo, `display` não existe).
3. Na prova, procure a **receita** no guia do tema, copie, e troque só o que a tabela "O que trocar" indicar.

## Guias por tema (receitas curtas, com explicação)

| Guia | Quando usar |
|---|---|
| [01_python_basico.md](guias/01_python_basico.md) | Entender variável, biblioteca, filtro, lista, dicionário |
| [02_inspecao_da_base.md](guias/02_inspecao_da_base.md) | "Carregue, mostre as primeiras linhas, dimensões, ausentes" |
| [03_limpeza_de_dados.md](guias/03_limpeza_de_dados.md) | Duplicatas, padronizar texto, converter datas e números, ausências, criar coluna |
| [04_metricas_por_grupo.md](guias/04_metricas_por_grupo.md) | Tabela por tema, por formato, por dia; média, mediana, soma, contagem |
| [05_graficos.md](guias/05_graficos.md) | Barras, barras horizontais, linhas, dispersão; título, eixos e fonte |
| [06_datas_series_e_filtros.md](guias/06_datas_series_e_filtros.md) | Análise por dia, filtros com `&`, top 5 |
| [07_arvore_e_importancia.md](guias/07_arvore_e_importancia.md) | Percentil, alvo 0/1, árvore, importância das variáveis |
| [08_regressao.md](guias/08_regressao.md) | Prever um número; MAE e R² |
| [09_classificacao_e_cortes.md](guias/09_classificacao_e_cortes.md) | Vários classificadores, precisão, recall, F1, matriz de confusão, cortes |
| [10_como_escrever_as_respostas.md](guias/10_como_escrever_as_respostas.md) | Modelos de texto para a Markdown |
| [11_glossario.md](guias/11_glossario.md) | Termos e métricas em linguagem simples |

## Simulado resolvido (enunciado, código, saída de referência e resposta)

Antes das questões, veja **[00_preparacao_e_dicionario.md](simulado/00_preparacao_e_dicionario.md)** (célula de imports, dicionário e fórmula).

| Questão | Tema | Arquivo |
|---|---|---|
| 1 | Diagnóstico inicial da base | [questao_1.md](simulado/questao_1.md) |
| 2 | Tratamento dos registros | [questao_2.md](simulado/questao_2.md) |
| 3 | Escolha de tema para divulgação | [questao_3.md](simulado/questao_3.md) |
| 4 | Acompanhamento diário | [questao_4.md](simulado/questao_4.md) |
| 5 | Tema e formato | [questao_5.md](simulado/questao_5.md) |
| 6 | Variáveis mais usadas pela árvore | [questao_6.md](simulado/questao_6.md) |
| 7 | Estimativa de engajamento | [questao_7.md](simulado/questao_7.md) |
| 8 | Priorização de divulgação | [questao_8.md](simulado/questao_8.md) |

## Pontos que ainda precisam de conferência (não verificados durante o estudo)

- **Questão 2:** a tabela final por tema não foi conferida. O esperado é 33 publicações no total (saude 11, mobilidade 8, trabalho 7, cultura 7).
- **Gráficos:** o código não usa `plt.show()`. Não foi confirmado se o site do professor exibe o gráfico assim. Veja o arquivo de ambiente.
- **Fórmula da taxa de utilidade:** `(compartilhamentos + salvamentos) ÷ alcance × 100`. Foi confirmada por você durante o estudo, mas na cópia do enunciado a fórmula veio em branco. Reconfira no site.

## Uso responsável

Este material é para estudo e consulta, e o professor autorizou consultar o repositório na prova. Confirme com ele o que exatamente pode ser aberto e copiado. Os textos das respostas são **modelos**: na prova, escreva com as suas palavras e com os números da sua saída.

## Como subir este material no seu GitHub

1. Descompacte o arquivo `.zip` no computador.
2. Entre em github.com com a sua conta e clique em **New repository** (botão verde ou o `+` no canto superior).
3. Nome sugerido: `python-analise-dados-prova` (sem acentos nem espaços). Marque **Public** (assim você acessa na prova sem complicação de login) e clique em **Create repository**.
4. Na página do repositório novo, clique em **uploading an existing file**.
5. Arraste para a página **o conteúdo da pasta** (o `README.md` e as pastas `guias` e `simulado`). Se o navegador não aceitar pastas, envie os arquivos de cada pasta separadamente. O GitHub aceita arrastar pastas na maioria dos navegadores atuais.
6. Escreva uma mensagem curta (por exemplo, "Material de estudo") e clique em **Commit changes**.
7. Copie o link do repositório e **teste abrindo em outra aba**. Salve o link em um lugar fácil (favoritos e bloco de notas).
8. Nos blocos de código, o GitHub mostra um botão de **copiar** no canto superior direito.
