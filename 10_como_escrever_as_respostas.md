# Guia 10: Como escrever as respostas na Markdown

Estes são **modelos**. Na prova, troque os colchetes pelos números da sua saída e reescreva com as suas palavras. Respeite o **limite de frases** do enunciado (conte).

## Regras de ouro

1. **Nunca afirme causalidade.** Use "está associado a", "aparece junto com", "nesta amostra".
2. **Cite números** da sua saída (mediana, MAE, contagens).
3. **Registre uma limitação** sempre que pedida: dados sintéticos, uma campanha, poucos registros, teste pequeno.
4. **Explique o que a métrica mede** quando o enunciado pedir (mediana, MAE, precisão, recall).
5. **Justifique** decisões de limpeza (descartei/preenchi/converti *porque*...).

## Modelo: limite de uso (diagnóstico da base)

> Os dados são sintéticos e cobrem apenas as publicações desta campanha, em um período curto e com poucos registros, então não representam outras redes, públicos ou bairros. Por isso, os resultados descrevem só esse histórico e não podem ser generalizados nem interpretados como causa e efeito.

## Modelo: decisões de limpeza (até 4 frases)

> Removi a duplicata [id] pelo id_publicacao, porque as linhas eram idênticas e nenhuma informação se perde. Padronizei os temas para [minúsculas, sem espaços e sem acento]. Converti data_publicacao para data/hora [lendo os N formatos presentes] e as colunas numéricas para número. Descartei [ids] por falta de [colunas da fórmula], porque preencher com média ou zero criaria informação que não está nos arquivos; mantive [id], cuja ausência está em coluna fora da fórmula.

## Modelo: recomendação de tema (3 a 5 frases)

> Recomendo dar o destaque principal ao tema [X], que teve a maior mediana da [métrica] ([valor]%), à frente de [Y] ([valor]%). A mediana representa a publicação típica do tema: metade das publicações ficou acima desse valor e metade abaixo, o que reduz o peso de casos isolados. [Explique o que a métrica mede.] Como limitação, a diferença para [Y] é de apenas [N] ponto(s) percentual(is) e os dados são sintéticos, de uma única campanha e com poucas publicações por tema. A escolha é uma decisão de comunicação apoiada em uma associação observada, e não prova que o tema cause mais [resultado].

## Modelo: variação diária e recorte (sem causalidade)

> A taxa média de engajamento oscilou entre [mín]% ([dia]) e [máx]% ([dia]), com dias altos e baixos se alternando, sem subida ou queda contínua. Cada dia tem só [2 ou 3] publicações, então a média diária é sensível a um único post, e a base cobre poucas semanas de dados sintéticos, o que não permite falar em tendência de longo prazo.
>
> O recorte mostra os cinco [reels noturnos] com maior taxa, mas isso não prova que horário ou formato causam engajamento: eles foram escolhidos justamente por terem os melhores resultados, não há comparação com outros horários e formatos, e tema, dia e perfil do autor também podem explicar a taxa. São poucos casos de dados sintéticos, então o que se observa é uma associação.

## Modelo: por que olhar duas variáveis juntas

> Analisar tema e formato juntos é diferente de analisar cada um separadamente porque o efeito de um pode depender do outro: em [tema A], a mediana vai de [X]% em [formato 1] a [Y]% em [formato 2], enquanto em [tema B] ela varia pouco, e a mediana do tema sozinho esconde essa diferença.

## Modelo: limitação de base pequena por combinações

> Ao dividir a base em [N] combinações, cada uma fica com poucas publicações ([mín] a [máx]), então as medianas são instáveis e a diferença entre as primeiras colocadas pode ser casual.

## Modelo: importância não prova causalidade

> Uma importância alta mostra apenas que a árvore usou bastante aquela característica para separar as publicações nesta amostra, ou seja, uma associação estatística, e não que ela cause o resultado; a árvore não testa o que aconteceria se a característica fosse alterada, e características correlacionadas dividem a importância entre si. Uma mudança na base que poderia alterar o ranking seria incluir publicações de outra campanha ou período, ou mudar o percentil de corte, o que poderia fazer características como [variáveis] ganharem ou perderem posição.

## Modelo: tipo de aprendizado

> **[Regressão / Classificação]**, porque a variável-alvo [é uma quantidade numérica contínua e o objetivo é estimar seu valor / é uma categoria e a decisão é binária]. A [regressão / classificação] não responde diretamente à decisão porque [...]. A clusterização agrupa registros parecidos sem uma resposta conhecida a prever, então não usa o rótulo que define o alvo.

## Modelo: MAE e escolha do modelo

> O MAE é a média dos erros absolutos, isto é, quantos [pontos percentuais] a previsão erra, em média, em relação ao valor real. O modelo com menor MAE foi [modelo] ([valor] contra [valor]), e por isso o escolhi; o R² de [valor] indica que ele explica [parte considerável / pouco] da variação no teste. Como limitação, as características só servem para prever e mostram associação, não causa; e o teste tem só [N] publicações, então as métricas podem mudar com outra separação.

## Modelo: decisão de modelo e corte (até 6 frases)

1. Modelo e corte escolhidos, com a métrica que justifica (maior F1).
2. Falso positivo no contexto da equipe.
3. Falso negativo no contexto da equipe.
4. O que mudou ao trocar o corte (recall e precisão, com números).
5. Qual erro pesa mais neste caso e por quê (poucas vagas → precisão).
6. Ressalva: teste pequeno, poucos casos do grupo de interesse.

## Expressões úteis

| Em vez de... | Escreva... |
|---|---|
| "causa", "faz aumentar", "provoca" | "está associado a", "aparece junto com" |
| "a tendência é de alta" | "oscilou", "variou entre" |
| "o modelo prova" | "o modelo indica", "sugere" |
| "sempre", "nunca" | "nesta amostra", "neste período" |
| "o melhor tema" | "o tema com maior mediana" |
