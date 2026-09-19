# Preparação do simulado e resumo do dicionário

## Contexto

O Observatório Conexão Bairro (OCB) é uma organização **fictícia** que apoia iniciativas culturais locais. Coordena a comunicação do **Festival ViraBairro**, com atividades gratuitas de cultura, mobilidade, saúde e trabalho. Nas oito semanas anteriores ao evento a equipe publicou convites, informações de serviço, histórias de participantes e guias práticos. Cada linha das bases é **uma publicação**. Os dados são sintéticos e não permitem concluir causalidade nem generalizar para outras contas.

## Arquivos

- `dados/publicacoes_brutas.csv`: base recebida, com falhas intencionais (padronização, duplicidade, tipos, ausências).
- `dados/publicacoes_analise.csv`: histórico já tratado, usado nas questões 3 a 8.
- `dados/dicionario-festival-virabairro.md`: dicionário.

## Célula de preparação (executar uma vez)

```python
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.ensemble import ExtraTreesClassifier, RandomForestClassifier
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.metrics import confusion_matrix, f1_score, mean_absolute_error, precision_score, r2_score, recall_score
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier, DecisionTreeRegressor

pd.options.display.max_columns = 100
```

## Campos do dicionário

| Campo | Descrição | Disponível antes de publicar? |
|---|---|---|
| id_publicacao | Identificador | Não é característica (só identificador) |
| data_publicacao | Data e hora | Sim |
| tema | Categoria | Sim |
| formato | Categoria da peça (carrossel, imagem, reel) | Sim |
| alcance | Contas alcançadas | **Não** |
| curtidas, comentarios, compartilhamentos, salvamentos | Reações recebidas | **Não** |
| seguidores_autor | Seguidores antes da publicação | Sim |
| videos_autor | Vídeos publicados antes | Sim |
| duracao_segundos | Duração do vídeo (0 se não for audiovisual) | Sim |
| tamanho_legenda | Caracteres da legenda | Sim |
| n_emojis | Emojis na legenda | Sim |
| n_hashtags | Hashtags na legenda | Sim |
| hora | Hora (0 a 23) | Sim |
| dia_semana | 0 = segunda ... 6 = domingo | Sim |
| taxa_engajamento_pct | (curtidas + comentários + compartilhamentos + salvamentos) ÷ alcance × 100 | **Não** |

Nas questões 6, 7 e 8, não use identificador, alcance, reações ou taxas como características: seria **vazamento de dados**.

## Fórmula da taxa de utilidade

`taxa_utilidade_pct = (compartilhamentos + salvamentos) ÷ alcance × 100`

(Na cópia do enunciado a fórmula veio em branco; esta foi a versão confirmada durante o estudo. Reconfira no site.)
