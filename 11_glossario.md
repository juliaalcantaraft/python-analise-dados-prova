# Guia 11: Glossário em linguagem simples

| Termo | O que é |
|---|---|
| **DataFrame (`df`)** | A tabela no pandas |
| **Coluna / linha** | Uma variável / uma observação (aqui, uma publicação) |
| **CSV** | Arquivo de tabela em texto, com valores separados por vírgula (ou ponto e vírgula) |
| **Valor ausente (NaN)** | Célula vazia |
| **Duplicidade** | A mesma observação registrada mais de uma vez |
| **Padronização** | Fazer valores equivalentes terem a mesma escrita (`Saúde`, `saude `, `SAUDE` → `saude`) |
| **Tipo de dado** | Número, texto ou data; a conversão certa permite calcular |
| **Mediana** | Valor do meio: metade dos valores fica acima e metade abaixo |
| **Média** | Soma dividida pela quantidade; puxada por extremos |
| **Percentil 75** | Valor abaixo do qual estão 75% dos dados; acima dele ficam os 25% maiores |
| **Agrupar (`groupby`)** | Separar as linhas por categoria e calcular algo em cada grupo |
| **Característica (feature, X)** | Informação usada para prever |
| **Alvo (target, y)** | O que se quer prever |
| **Vazamento de dados** | Usar como característica algo que só se sabe depois; deixa o resultado bonito e falso |
| **Treino e teste** | Parte dos dados para o modelo aprender e parte para avaliá-lo em dados que ele não viu |
| **`random_state`** | Número que deixa o sorteio de treino/teste repetível |
| **`stratify`** | Mantém a proporção do alvo nos conjuntos de treino e teste |
| **Variável dummy** | Coluna 0/1 que representa uma categoria (`formato_reel`) |
| **Regressão** | Prever um número |
| **Classificação** | Prever uma categoria |
| **Clusterização** | Agrupar registros parecidos, sem alvo conhecido |
| **Árvore de decisão** | Modelo que faz perguntas em sequência para separar os casos |
| **Profundidade máxima** | Número máximo de níveis de perguntas da árvore |
| **Importância (da característica)** | Quanto a árvore usou aquela variável para dividir os dados; soma 1 |
| **MAE** | Erro absoluto médio: quanto a previsão erra, em média, na unidade do alvo |
| **R²** | Parte da variação do alvo que o modelo explica |
| **Precisão** | Das indicadas pelo modelo, quantas eram corretas |
| **Recall** | Das que deveriam ser indicadas, quantas o modelo achou |
| **F1** | Equilíbrio entre precisão e recall |
| **Matriz de confusão** | Tabela de acertos e erros por classe (VN, FP, FN, VP) |
| **Falso positivo** | O modelo indica, mas não era |
| **Falso negativo** | O modelo não indica, mas era |
| **Corte (limiar)** | Probabilidade mínima para indicar uma peça |
| **Causalidade** | Uma coisa provocar a outra; os dados deste tipo de simulado mostram só **associação** |
| **Dados sintéticos** | Fictícios, criados para treino; não representam pessoas ou organizações reais |
