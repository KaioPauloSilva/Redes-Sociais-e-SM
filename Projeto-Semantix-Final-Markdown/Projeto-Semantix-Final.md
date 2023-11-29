# Redes Sociais e Saúde Mental

## Entendimento do Projeto

Este é um projeto proposto pela Semantix, no curso de Cientista de Dados da instituição EBAC.

O objetivo deste projeto encontrar uma problemática da vida real que possa ser solucionada através de análise de dados e machine learning. A ideia do projeto é explanar e justificar a relevância do uso de dados para encontrar a solução.

Este projeto é baseado em uma base de dados do Kaggle. No entanto, é importante lembrar que os dados coletados podem ser enviesados e podem não representar a verdade absoluta. É possível que os dados sejam coletados de uma amostra específica ou que a coleta de dados tenha sido feita de maneira tendenciosa. Portanto, é importante ter cuidado ao interpretar os resultados e considerar as limitações dos dados. Além disso, é importante realizar mais pesquisas para confirmar os resultados obtidos

## Entendimento dos Dados

   A base de dados é composta por 7 variáveis e 12 questões baseadas em escala Likert de 1 a 5, que dão entendimento de frequencia ou intensidade em varios aspectos da Saúde Mental. Um score baixo de 1 geralmente indica baixa frequência ou intensidade, e um escore alto de 5 geralmente indica alta frequência ou intensidade.

----------

**Acordo**                             

Concordo totalmente - 5           

Concordo - 4

Não concordo nem discordo - 3

Discordo - 2

Discordo totalmente - 1

-----------------

**Frequência**

Muito frequentemente - 5

Frequentemente - 4

Ocasionalmente -3

Raramente -2

Nunca - 1

### Dicionário de dados

#### Variáveis

| Variável                | Descrição                                           | Tipo         |
| ----------------------- |:---------------------------------------------------:| ------------:|
| idade                   |  Idades dos participantes da pesquisa               | int          |
| genero                  |  Masculino / Feminino / Outros                      | object       |
| estado_civil            |  Em relancionamento / Solteiro / Casado / Divorciado| object       |
| ocupacao                |  Estudante / Assalariado  / Aposentado              | object       |
| usa_rede_social         |  1 = Usa rede social / 0 = Não usa Rede Social      | bool         |
| redes_sociais_usadas    |  Quais redes sociais o participante usa             | object       |
| tempo_medio_gasto       |  Tempo medio gasto em rede sociais                  | object       |
| TDAH                    |  Pontuação escala Likert                            | int          |
| Ansiedade               |  Portuação escala Likert                            | int          |
| Autoestima              |  Pontuação escala Likert                            | int          |
| Depressao               |  Pontuação escala Likert                            | int          |

#### Perguntas de escala

 - 1. Com que frequência você se pega usando as redes sociais sem um propósito específico?  [gasta_tempo_nas_redes_sociais_sem_motivo].
 
<d> 
     
 - 2. Com que frequência você se distrai com as redes sociais quando está ocupado fazendo algo? [distraido_pelas_redes_sociais].

<d> 
    
 - 3. Você se sente inquieto se não usa as redes sociais por um tempo? [inquieto_sem_uso_redes_sociais].

<d>    
     
 - 4. Em uma escala de 1 a 5, o quão facilmente você se distrai? [escala_distraido].

<d>    
     
 - 5. Em uma escala de 1 a 5, o quanto você se preocupa? [escala_preocupacoes].
 
<d>     
     
 - 6. Você acha difícil se concentrar nas coisas? [escala_concentracao].
 
<d>     
    
 - 7. Em uma escala de 1 a 5, com que frequência você se compara a outras pessoas bem-sucedidas por meio das redes sociais [comparacao_pessoas_sucedidas].
 
<d>
 
 - 8. Seguindo a pergunta anterior, como você se sente em relação a essas comparações, em geral? [sentimentos_sobre_comparacao].
 
<d> 
    
 - 9. Com que frequência você procura validação nas funcionalidades das redes sociais? [busca_por_validacao].
    
<d>
    
 - 10. Com que frequência você se sente deprimido ou para baixo? [sentimento_pra_baixo].
    
<d>
    
 - 11. Em uma escala de 1 a 5, com que frequência seu interesse em atividades diárias flutua? [escala_interesse_atividades_diarias].
    
<d>
    
 - 12. Em uma escala de 1 a 5, com que frequência você enfrenta problemas relacionados ao sono? [escala_problema_sono]


### Carregando Pacotes


```python
import pandas as pd
import numpy as np

import matplotlib.pyplot as plt
import seaborn as sns
import plotly.express as px

from pycaret.clustering import *
from pycaret.clustering import ClusteringExperiment
```

### Carregando Dados
 - Nota-se que os dados estão em inglês.


```python
data_en = pd.read_csv('smmh.csv')
```


```python
data_en
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Timestamp</th>
      <th>1. What is your age?</th>
      <th>2. Gender</th>
      <th>3. Relationship Status</th>
      <th>4. Occupation Status</th>
      <th>5. What type of organizations are you affiliated with?</th>
      <th>6. Do you use social media?</th>
      <th>7. What social media platforms do you commonly use?</th>
      <th>8. What is the average time you spend on social media every day?</th>
      <th>9. How often do you find yourself using Social media without a specific purpose?</th>
      <th>...</th>
      <th>11. Do you feel restless if you haven't used Social media in a while?</th>
      <th>12. On a scale of 1 to 5, how easily distracted are you?</th>
      <th>13. On a scale of 1 to 5, how much are you bothered by worries?</th>
      <th>14. Do you find it difficult to concentrate on things?</th>
      <th>15. On a scale of 1-5, how often do you compare yourself to other successful people through the use of social media?</th>
      <th>16. Following the previous question, how do you feel about these comparisons, generally speaking?</th>
      <th>17. How often do you look to seek validation from features of social media?</th>
      <th>18. How often do you feel depressed or down?</th>
      <th>19. On a scale of 1 to 5, how frequently does your interest in daily activities fluctuate?</th>
      <th>20. On a scale of 1 to 5, how often do you face issues regarding sleep?</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4/18/2022 19:18:47</td>
      <td>21.0</td>
      <td>Male</td>
      <td>In a relationship</td>
      <td>University Student</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>5</td>
      <td>...</td>
      <td>2</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4/18/2022 19:19:28</td>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4/18/2022 19:25:59</td>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, Instagram, YouTube, Pinterest</td>
      <td>Between 3 and 4 hours</td>
      <td>3</td>
      <td>...</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4/18/2022 19:29:43</td>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, Instagram</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>...</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4/18/2022 19:33:31</td>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>5/21/2022 23:38:28</td>
      <td>24.0</td>
      <td>Male</td>
      <td>Single</td>
      <td>Salaried Worker</td>
      <td>University, Private</td>
      <td>Yes</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>...</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>477</th>
      <td>5/22/2022 0:01:05</td>
      <td>26.0</td>
      <td>Female</td>
      <td>Married</td>
      <td>Salaried Worker</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, YouTube</td>
      <td>Between 1 and 2 hours</td>
      <td>2</td>
      <td>...</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>478</th>
      <td>5/22/2022 10:29:21</td>
      <td>29.0</td>
      <td>Female</td>
      <td>Married</td>
      <td>Salaried Worker</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>...</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>479</th>
      <td>7/14/2022 19:33:47</td>
      <td>21.0</td>
      <td>Male</td>
      <td>Single</td>
      <td>University Student</td>
      <td>University</td>
      <td>Yes</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>2</td>
      <td>...</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>480</th>
      <td>11/12/2022 13:16:50</td>
      <td>53.0</td>
      <td>Male</td>
      <td>Married</td>
      <td>Salaried Worker</td>
      <td>Private</td>
      <td>Yes</td>
      <td>Facebook, YouTube</td>
      <td>Less than an Hour</td>
      <td>2</td>
      <td>...</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>481 rows × 21 columns</p>
</div>




```python
data_en.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 481 entries, 0 to 480
    Data columns (total 21 columns):
     #   Column                                                                                                                Non-Null Count  Dtype  
    ---  ------                                                                                                                --------------  -----  
     0   Timestamp                                                                                                             481 non-null    object 
     1   1. What is your age?                                                                                                  481 non-null    float64
     2   2. Gender                                                                                                             481 non-null    object 
     3   3. Relationship Status                                                                                                481 non-null    object 
     4   4. Occupation Status                                                                                                  481 non-null    object 
     5   5. What type of organizations are you affiliated with?                                                                451 non-null    object 
     6   6. Do you use social media?                                                                                           481 non-null    object 
     7   7. What social media platforms do you commonly use?                                                                   481 non-null    object 
     8   8. What is the average time you spend on social media every day?                                                      481 non-null    object 
     9   9. How often do you find yourself using Social media without a specific purpose?                                      481 non-null    int64  
     10  10. How often do you get distracted by Social media when you are busy doing something?                                481 non-null    int64  
     11  11. Do you feel restless if you haven't used Social media in a while?                                                 481 non-null    int64  
     12  12. On a scale of 1 to 5, how easily distracted are you?                                                              481 non-null    int64  
     13  13. On a scale of 1 to 5, how much are you bothered by worries?                                                       481 non-null    int64  
     14  14. Do you find it difficult to concentrate on things?                                                                481 non-null    int64  
     15  15. On a scale of 1-5, how often do you compare yourself to other successful people through the use of social media?  481 non-null    int64  
     16  16. Following the previous question, how do you feel about these comparisons, generally speaking?                     481 non-null    int64  
     17  17. How often do you look to seek validation from features of social media?                                           481 non-null    int64  
     18  18. How often do you feel depressed or down?                                                                          481 non-null    int64  
     19  19. On a scale of 1 to 5, how frequently does your interest in daily activities fluctuate?                            481 non-null    int64  
     20  20. On a scale of 1 to 5, how often do you face issues regarding sleep?                                               481 non-null    int64  
    dtypes: float64(1), int64(12), object(8)
    memory usage: 79.0+ KB
    

### Tratamento dos Dados

 -  Nesta etapa vai ser feito o tratamento dos dados.


```python
data = data_en.rename(columns={'1. What is your age?' : 'idade',
                  '2. Gender' : 'genero',
                  '3. Relationship Status' : 'estado_civil',
                  '4. Occupation Status' : 'ocupacao',
                  '5. What type of organizations are you affiliated with?' : 'organizacoes_afiliadas',
                  '6. Do you use social media?' : 'usa_rede_social',
                  '7. What social media platforms do you commonly use?' : 'redes_sociais_usadas',
                  '8. What is the average time you spend on social media every day?' : 'tempo_medio_gasto',
                  '9. How often do you find yourself using Social media without a specific purpose?' : 'gasta_tempo_nas_redes_sociais_sem_motivo',
                  '10. How often do you get distracted by Social media when you are busy doing something?' : 'distraido_pelas_redes_sociais',
                  "11. Do you feel restless if you haven't used Social media in a while?" : 'inquieto_sem_uso_redes_sociais',
                  '12. On a scale of 1 to 5, how easily distracted are you?' : 'escala_distraido',
                  '13. On a scale of 1 to 5, how much are you bothered by worries?' : 'escala_preocupacoes',
                  '14. Do you find it difficult to concentrate on things?' : 'escala_concentracao',
                  '15. On a scale of 1-5, how often do you compare yourself to other successful people through the use of social media?' : 'comparacao_pessoas_sucedidas',
                  '16. Following the previous question, how do you feel about these comparisons, generally speaking?' : 'sentimentos_sobre_comparacao',
                  '17. How often do you look to seek validation from features of social media?' : 'busca_por_validacao',
                  '18. How often do you feel depressed or down?' : 'sentimento_pra_baixo',
                  '19. On a scale of 1 to 5, how frequently does your interest in daily activities fluctuate?' : 'escala_interesse_atividades_diarias',
                  '20. On a scale of 1 to 5, how often do you face issues regarding sleep?' : 'escala_problema_sono'})
```

 - Foi feito a tradução das perguntas de escala e a modificação do nome das variaveis


```python
data.columns
```




    Index(['Timestamp', 'idade', 'genero', 'estado_civil', 'ocupacao',
           'organizacoes_afiliadas', 'usa_rede_social', 'redes_sociais_usadas',
           'tempo_medio_gasto', 'gasta_tempo_nas_redes_sociais_sem_motivo',
           'distraido_pelas_redes_sociais', 'inquieto_sem_uso_redes_sociais',
           'escala_distraido', 'escala_preocupacoes', 'escala_concentracao',
           'comparacao_pessoas_sucedidas', 'sentimentos_sobre_comparacao',
           'busca_por_validacao', 'sentimento_pra_baixo',
           'escala_interesse_atividades_diarias', 'escala_problema_sono'],
          dtype='object')



 - As Colunas Timestamp e organizações_afiliadas vão ser excluidas, pois não tem relevancia na analise.


```python
data.drop(columns=['Timestamp', 'organizacoes_afiliadas'], inplace=True)
```


```python
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>usa_rede_social</th>
      <th>redes_sociais_usadas</th>
      <th>tempo_medio_gasto</th>
      <th>gasta_tempo_nas_redes_sociais_sem_motivo</th>
      <th>distraido_pelas_redes_sociais</th>
      <th>inquieto_sem_uso_redes_sociais</th>
      <th>escala_distraido</th>
      <th>escala_preocupacoes</th>
      <th>escala_concentracao</th>
      <th>comparacao_pessoas_sucedidas</th>
      <th>sentimentos_sobre_comparacao</th>
      <th>busca_por_validacao</th>
      <th>sentimento_pra_baixo</th>
      <th>escala_interesse_atividades_diarias</th>
      <th>escala_problema_sono</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21.0</td>
      <td>Male</td>
      <td>In a relationship</td>
      <td>University Student</td>
      <td>Yes</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>2</td>
      <td>5</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>Yes</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>Yes</td>
      <td>Facebook, Instagram, YouTube, Pinterest</td>
      <td>Between 3 and 4 hours</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>Yes</td>
      <td>Facebook, Instagram</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>3</td>
      <td>5</td>
      <td>3</td>
      <td>5</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21.0</td>
      <td>Female</td>
      <td>Single</td>
      <td>University Student</td>
      <td>Yes</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>5</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>5</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24.0</td>
      <td>Male</td>
      <td>Single</td>
      <td>Salaried Worker</td>
      <td>Yes</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26.0</td>
      <td>Female</td>
      <td>Married</td>
      <td>Salaried Worker</td>
      <td>Yes</td>
      <td>Facebook, YouTube</td>
      <td>Between 1 and 2 hours</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29.0</td>
      <td>Female</td>
      <td>Married</td>
      <td>Salaried Worker</td>
      <td>Yes</td>
      <td>Facebook, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21.0</td>
      <td>Male</td>
      <td>Single</td>
      <td>University Student</td>
      <td>Yes</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53.0</td>
      <td>Male</td>
      <td>Married</td>
      <td>Salaried Worker</td>
      <td>Yes</td>
      <td>Facebook, YouTube</td>
      <td>Less than an Hour</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
<p>481 rows × 19 columns</p>
</div>



#### Variavel Idade



```python
data['idade'].unique()
```




    array([21. , 22. , 20. , 24. , 23. , 25. , 28. , 34. , 26. , 35. , 18. ,
           19. , 56. , 65. , 17. , 40. , 33. , 55. , 27. , 14. , 38. , 32. ,
           30. , 16. , 48. , 47. , 50. , 49. , 51. , 46. , 36. , 37. , 45. ,
           42. , 31. , 69. , 91. , 15. , 29. , 43. , 26.7, 52. , 44. , 60. ,
           13. , 53. ])



 - Nota-se a variável idade esta em float, vai ser alterada para int


```python
data['idade'] = data['idade'].astype('int')
```


```python
data['idade'].unique()
```




    array([21, 22, 20, 24, 23, 25, 28, 34, 26, 35, 18, 19, 56, 65, 17, 40, 33,
           55, 27, 14, 38, 32, 30, 16, 48, 47, 50, 49, 51, 46, 36, 37, 45, 42,
           31, 69, 91, 15, 29, 43, 52, 44, 60, 13, 53])



#### Variável Genero


```python
data['genero'].unique()
```




    array(['Male', 'Female', 'Nonbinary ', 'Non-binary', 'NB', 'unsure ',
           'Trans', 'Non binary ', 'There are others???'], dtype=object)




```python
data.drop(data.loc[data['genero'] =='There are others???'].index, inplace=True)
```


```python
data.replace('Male','Masculino', inplace=True)
data.replace('Female','Feminino', inplace=True)
# Transformandoos os generos diversificados em apenas uma categoria
data.replace('Non-binary','Outros', inplace=True)
data.replace('Nonbinary ','Outros', inplace=True)
data.replace('NB','Outros', inplace=True)
data.replace('unsure ','Outros', inplace=True)
data.replace('Non binary ','Outros', inplace=True)
data.replace('Trans','Outros', inplace=True)
```


```python
data['genero'].unique()
```




    array(['Masculino', 'Feminino', 'Outros'], dtype=object)



#### Variável estado-civil


```python
data['estado_civil'].unique()
```




    array(['In a relationship', 'Single', 'Married', 'Divorced'], dtype=object)




```python
data.replace('In a relationship','Em Relacionamento', inplace=True)
data.replace('Single','Solteiro', inplace=True)
data.replace('Married','Casado', inplace=True)
data.replace('Divorced','Divorciado', inplace=True)
```


```python
data['estado_civil'].unique()
```




    array(['Em Relacionamento', 'Solteiro', 'Casado', 'Divorciado'],
          dtype=object)



#### Variável ocupacao


```python
data['ocupacao'].unique()
```




    array(['University Student', 'School Student', 'Salaried Worker',
           'Retired'], dtype=object)




```python
data.replace('University Student','Estudante', inplace=True)
data.replace('School Student','Estudante', inplace=True)
data.replace('Salaried Worker','Assalariado', inplace=True)
data.replace('Retired','Aposentado', inplace=True)
```


```python
data['ocupacao'].unique()
```




    array(['Estudante', 'Assalariado', 'Aposentado'], dtype=object)



#### Variável usa_rede_social 


```python
data['usa_rede_social'].replace('Yes',1, inplace=True)
data['usa_rede_social'].replace('No',0, inplace=True)
```


```python
data['usa_rede_social'].unique()
```




    array([1, 0], dtype=int64)




```python
data['usa_rede_social'] = data['usa_rede_social'].astype('bool')
```


```python
data['usa_rede_social'].unique()
```




    array([ True, False])




```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    Int64Index: 480 entries, 0 to 480
    Data columns (total 19 columns):
     #   Column                                    Non-Null Count  Dtype 
    ---  ------                                    --------------  ----- 
     0   idade                                     480 non-null    int32 
     1   genero                                    480 non-null    object
     2   estado_civil                              480 non-null    object
     3   ocupacao                                  480 non-null    object
     4   usa_rede_social                           480 non-null    bool  
     5   redes_sociais_usadas                      480 non-null    object
     6   tempo_medio_gasto                         480 non-null    object
     7   gasta_tempo_nas_redes_sociais_sem_motivo  480 non-null    int64 
     8   distraido_pelas_redes_sociais             480 non-null    int64 
     9   inquieto_sem_uso_redes_sociais            480 non-null    int64 
     10  escala_distraido                          480 non-null    int64 
     11  escala_preocupacoes                       480 non-null    int64 
     12  escala_concentracao                       480 non-null    int64 
     13  comparacao_pessoas_sucedidas              480 non-null    int64 
     14  sentimentos_sobre_comparacao              480 non-null    int64 
     15  busca_por_validacao                       480 non-null    int64 
     16  sentimento_pra_baixo                      480 non-null    int64 
     17  escala_interesse_atividades_diarias       480 non-null    int64 
     18  escala_problema_sono                      480 non-null    int64 
    dtypes: bool(1), int32(1), int64(12), object(5)
    memory usage: 69.8+ KB
    

### Análise de Univariada

### *idade*


```python
fig = px.box(data, y='idade')
fig.show()
```


        <script type="text/javascript">
        window.PlotlyConfig = {MathJaxConfig: 'local'};
        if (window.MathJax && window.MathJax.Hub && window.MathJax.Hub.Config) {window.MathJax.Hub.Config({SVG: {font: "STIX-Web"}});}
        if (typeof require !== 'undefined') {
        require.undef("plotly");
        define('plotly', function(require, exports, module) {
            /**
* plotly.js v2.27.0
* Copyright 2012-2023, Plotly, Inc.
* All rights reserved.
* Licensed under the MIT license
*/
/*! For license information please see plotly.min.js.LICENSE.txt */
        });
        require(['plotly'], function(Plotly) {
            window._Plotly = Plotly;
        });
        }
        </script>




<div>                            <div id="60b00c32-bc69-4991-bb32-60cf063eea10" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("60b00c32-bc69-4991-bb32-60cf063eea10")) {                    Plotly.newPlot(                        "60b00c32-bc69-4991-bb32-60cf063eea10",                        [{"alignmentgroup":"True","hovertemplate":"idade=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa"},"name":"","notched":false,"offsetgroup":"","orientation":"v","showlegend":false,"x0":" ","xaxis":"x","y":[21,21,21,21,21,22,21,21,21,20,24,21,22,21,22,23,21,25,28,34,22,23,26,23,23,35,20,35,22,21,22,35,24,18,19,21,21,23,22,22,21,21,56,19,24,22,65,17,40,33,22,24,26,55,26,27,22,19,22,20,25,25,18,25,19,24,21,18,14,25,33,23,14,38,23,26,24,21,18,28,22,21,21,21,17,24,22,32,22,20,22,28,22,17,30,22,14,20,22,16,22,22,22,22,21,23,23,48,21,47,48,26,47,47,27,34,20,22,23,20,48,20,20,22,22,20,24,23,20,16,50,47,24,38,50,49,47,48,51,50,47,27,40,46,21,47,20,47,48,49,21,32,47,50,48,21,47,36,37,19,30,48,48,48,47,47,48,21,48,21,20,20,20,21,20,47,35,23,21,21,37,48,47,25,22,45,47,47,24,21,22,19,21,22,20,42,35,38,17,32,21,22,27,21,27,20,27,24,26,20,31,21,34,22,20,23,48,37,22,50,21,23,21,22,22,22,19,23,46,21,24,20,25,21,23,22,36,22,69,25,22,23,23,19,21,27,25,20,34,23,34,26,25,22,20,23,91,22,23,21,36,21,22,22,22,20,26,21,20,21,19,20,22,23,21,21,22,26,22,17,23,21,22,20,24,22,24,22,26,24,22,25,21,20,22,25,17,22,26,22,23,25,21,30,23,22,21,20,18,20,15,22,17,21,22,23,24,21,22,22,21,21,18,19,22,21,21,22,21,23,19,26,27,25,20,20,22,18,22,20,24,21,29,21,19,21,43,22,24,18,19,37,24,29,19,35,22,23,22,25,19,29,21,21,22,23,22,20,27,24,21,23,21,23,24,21,30,23,22,29,20,26,24,26,22,23,19,21,20,22,22,23,22,20,27,22,21,21,24,19,52,26,44,44,19,35,21,20,24,21,24,21,17,26,18,18,18,23,20,23,25,26,20,19,17,24,29,30,21,23,21,20,21,22,22,22,27,21,23,22,21,23,21,20,23,22,20,21,25,21,28,22,60,49,34,56,51,23,21,50,22,28,13,14,13,23,18,18,15,20,20,35,26,30,32,24,26,29,21,53],"y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"idade"}},"legend":{"tracegroupgap":0},"margin":{"t":60},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('60b00c32-bc69-4991-bb32-60cf063eea10');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.histogram(data, x='idade', nbins=150)
fig.show()
```


<div>                            <div id="d4e18e04-d861-4427-ba8e-62fce3357eef" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("d4e18e04-d861-4427-ba8e-62fce3357eef")) {                    Plotly.newPlot(                        "d4e18e04-d861-4427-ba8e-62fce3357eef",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"idade=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"","nbinsx":150,"offsetgroup":"","orientation":"v","showlegend":false,"x":[21,21,21,21,21,22,21,21,21,20,24,21,22,21,22,23,21,25,28,34,22,23,26,23,23,35,20,35,22,21,22,35,24,18,19,21,21,23,22,22,21,21,56,19,24,22,65,17,40,33,22,24,26,55,26,27,22,19,22,20,25,25,18,25,19,24,21,18,14,25,33,23,14,38,23,26,24,21,18,28,22,21,21,21,17,24,22,32,22,20,22,28,22,17,30,22,14,20,22,16,22,22,22,22,21,23,23,48,21,47,48,26,47,47,27,34,20,22,23,20,48,20,20,22,22,20,24,23,20,16,50,47,24,38,50,49,47,48,51,50,47,27,40,46,21,47,20,47,48,49,21,32,47,50,48,21,47,36,37,19,30,48,48,48,47,47,48,21,48,21,20,20,20,21,20,47,35,23,21,21,37,48,47,25,22,45,47,47,24,21,22,19,21,22,20,42,35,38,17,32,21,22,27,21,27,20,27,24,26,20,31,21,34,22,20,23,48,37,22,50,21,23,21,22,22,22,19,23,46,21,24,20,25,21,23,22,36,22,69,25,22,23,23,19,21,27,25,20,34,23,34,26,25,22,20,23,91,22,23,21,36,21,22,22,22,20,26,21,20,21,19,20,22,23,21,21,22,26,22,17,23,21,22,20,24,22,24,22,26,24,22,25,21,20,22,25,17,22,26,22,23,25,21,30,23,22,21,20,18,20,15,22,17,21,22,23,24,21,22,22,21,21,18,19,22,21,21,22,21,23,19,26,27,25,20,20,22,18,22,20,24,21,29,21,19,21,43,22,24,18,19,37,24,29,19,35,22,23,22,25,19,29,21,21,22,23,22,20,27,24,21,23,21,23,24,21,30,23,22,29,20,26,24,26,22,23,19,21,20,22,22,23,22,20,27,22,21,21,24,19,52,26,44,44,19,35,21,20,24,21,24,21,17,26,18,18,18,23,20,23,25,26,20,19,17,24,29,30,21,23,21,20,21,22,22,22,27,21,23,22,21,23,21,20,23,22,20,21,25,21,28,22,60,49,34,56,51,23,21,50,22,28,13,14,13,23,18,18,15,20,20,35,26,30,32,24,26,29,21,53],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"idade"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('d4e18e04-d861-4427-ba8e-62fce3357eef');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


 -  Nota-se que a maioria dos particpantes são adultos de 20 a 30 anos.

### *genero*


```python
fig = px.histogram(data, x='genero', color='genero')
fig.show()
```


<div>                            <div id="39b9b907-bece-409f-9db4-6a902d0c5bf9" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("39b9b907-bece-409f-9db4-6a902d0c5bf9")) {                    Plotly.newPlot(                        "39b9b907-bece-409f-9db4-6a902d0c5bf9",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"genero=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Masculino","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"Masculino","offsetgroup":"Masculino","orientation":"v","showlegend":true,"x":["Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino","Masculino"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"genero=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Feminino","marker":{"color":"#EF553B","pattern":{"shape":""}},"name":"Feminino","offsetgroup":"Feminino","orientation":"v","showlegend":true,"x":["Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino","Feminino"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"genero=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Outros","marker":{"color":"#00cc96","pattern":{"shape":""}},"name":"Outros","offsetgroup":"Outros","orientation":"v","showlegend":true,"x":["Outros","Outros","Outros","Outros","Outros","Outros"],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"genero"},"categoryorder":"array","categoryarray":["Masculino","Feminino","Outros"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"title":{"text":"genero"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('39b9b907-bece-409f-9db4-6a902d0c5bf9');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### *estado_civil*


```python
fig = px.histogram(data, x='estado_civil', color='estado_civil')
fig.show()
```


<div>                            <div id="2ee8381d-093e-4209-8106-305c0f761439" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("2ee8381d-093e-4209-8106-305c0f761439")) {                    Plotly.newPlot(                        "2ee8381d-093e-4209-8106-305c0f761439",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"estado_civil=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Em Relacionamento","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"Em Relacionamento","offsetgroup":"Em Relacionamento","orientation":"v","showlegend":true,"x":["Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento","Em Relacionamento"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"estado_civil=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Solteiro","marker":{"color":"#EF553B","pattern":{"shape":""}},"name":"Solteiro","offsetgroup":"Solteiro","orientation":"v","showlegend":true,"x":["Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro","Solteiro"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"estado_civil=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Casado","marker":{"color":"#00cc96","pattern":{"shape":""}},"name":"Casado","offsetgroup":"Casado","orientation":"v","showlegend":true,"x":["Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado","Casado"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"estado_civil=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Divorciado","marker":{"color":"#ab63fa","pattern":{"shape":""}},"name":"Divorciado","offsetgroup":"Divorciado","orientation":"v","showlegend":true,"x":["Divorciado","Divorciado","Divorciado","Divorciado","Divorciado","Divorciado","Divorciado"],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"estado_civil"},"categoryorder":"array","categoryarray":["Em Relacionamento","Solteiro","Casado","Divorciado"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"title":{"text":"estado_civil"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('2ee8381d-093e-4209-8106-305c0f761439');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


### *ocupacao*


```python
fig = px.histogram(data, x='ocupacao', color='ocupacao')
fig.show()
```


<div>                            <div id="9614db9b-a507-4155-896b-411bac7c9d2b" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("9614db9b-a507-4155-896b-411bac7c9d2b")) {                    Plotly.newPlot(                        "9614db9b-a507-4155-896b-411bac7c9d2b",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"ocupacao=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Estudante","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"Estudante","offsetgroup":"Estudante","orientation":"v","showlegend":true,"x":["Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante","Estudante"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"ocupacao=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Assalariado","marker":{"color":"#EF553B","pattern":{"shape":""}},"name":"Assalariado","offsetgroup":"Assalariado","orientation":"v","showlegend":true,"x":["Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado","Assalariado"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"ocupacao=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Aposentado","marker":{"color":"#00cc96","pattern":{"shape":""}},"name":"Aposentado","offsetgroup":"Aposentado","orientation":"v","showlegend":true,"x":["Aposentado","Aposentado","Aposentado","Aposentado","Aposentado","Aposentado","Aposentado","Aposentado"],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"ocupacao"},"categoryorder":"array","categoryarray":["Estudante","Assalariado","Aposentado"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"title":{"text":"ocupacao"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('9614db9b-a507-4155-896b-411bac7c9d2b');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


 - A maioria dos participantes desta pesquisa são estudantes.
 - Os aposentados são a minoria.

### *usa_rede_social*


```python
fig = px.histogram(data, x='usa_rede_social', color='usa_rede_social')
fig.show()
```


<div>                            <div id="b18369e4-097f-4d8e-8a03-5fc5e963afa6" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("b18369e4-097f-4d8e-8a03-5fc5e963afa6")) {                    Plotly.newPlot(                        "b18369e4-097f-4d8e-8a03-5fc5e963afa6",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"usa_rede_social=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"True","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"True","offsetgroup":"True","orientation":"v","showlegend":true,"x":[true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true,true],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"usa_rede_social=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"False","marker":{"color":"#EF553B","pattern":{"shape":""}},"name":"False","offsetgroup":"False","orientation":"v","showlegend":true,"x":[false,false,false],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"usa_rede_social"},"categoryorder":"array","categoryarray":[true,false]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"title":{"text":"usa_rede_social"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('b18369e4-097f-4d8e-8a03-5fc5e963afa6');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


 - Apenas 3 pessoas não utilizam rede sociais

### *tempo_medio_gasto*


```python
fig = px.histogram(data, x='tempo_medio_gasto', color='tempo_medio_gasto')
fig.show()
```


<div>                            <div id="e5a3fe48-1433-4dd7-b268-158b982634cd" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("e5a3fe48-1433-4dd7-b268-158b982634cd")) {                    Plotly.newPlot(                        "e5a3fe48-1433-4dd7-b268-158b982634cd",                        [{"alignmentgroup":"True","bingroup":"x","hovertemplate":"tempo_medio_gasto=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Between 2 and 3 hours","marker":{"color":"#636efa","pattern":{"shape":""}},"name":"Between 2 and 3 hours","offsetgroup":"Between 2 and 3 hours","orientation":"v","showlegend":true,"x":["Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours","Between 2 and 3 hours"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"tempo_medio_gasto=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"More than 5 hours","marker":{"color":"#EF553B","pattern":{"shape":""}},"name":"More than 5 hours","offsetgroup":"More than 5 hours","orientation":"v","showlegend":true,"x":["More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours","More than 5 hours"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"tempo_medio_gasto=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Between 3 and 4 hours","marker":{"color":"#00cc96","pattern":{"shape":""}},"name":"Between 3 and 4 hours","offsetgroup":"Between 3 and 4 hours","orientation":"v","showlegend":true,"x":["Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours","Between 3 and 4 hours"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"tempo_medio_gasto=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Less than an Hour","marker":{"color":"#ab63fa","pattern":{"shape":""}},"name":"Less than an Hour","offsetgroup":"Less than an Hour","orientation":"v","showlegend":true,"x":["Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour","Less than an Hour"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"tempo_medio_gasto=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Between 1 and 2 hours","marker":{"color":"#FFA15A","pattern":{"shape":""}},"name":"Between 1 and 2 hours","offsetgroup":"Between 1 and 2 hours","orientation":"v","showlegend":true,"x":["Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours","Between 1 and 2 hours"],"xaxis":"x","yaxis":"y","type":"histogram"},{"alignmentgroup":"True","bingroup":"x","hovertemplate":"tempo_medio_gasto=%{x}\u003cbr\u003ecount=%{y}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Between 4 and 5 hours","marker":{"color":"#19d3f3","pattern":{"shape":""}},"name":"Between 4 and 5 hours","offsetgroup":"Between 4 and 5 hours","orientation":"v","showlegend":true,"x":["Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours","Between 4 and 5 hours"],"xaxis":"x","yaxis":"y","type":"histogram"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"tempo_medio_gasto"},"categoryorder":"array","categoryarray":["Between 2 and 3 hours","More than 5 hours","Between 3 and 4 hours","Less than an Hour","Between 1 and 2 hours","Between 4 and 5 hours"]},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"count"}},"legend":{"title":{"text":"tempo_medio_gasto"},"tracegroupgap":0},"margin":{"t":60},"barmode":"relative"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('e5a3fe48-1433-4dd7-b268-158b982634cd');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


## Soma da escala Likert para diferentes aspectos de saúde-mental

As perguntas medem 4 aspectos do bem-estar mental:

- TDAH - TRANSTORNO DO DÉFICIT DE ATENÇÃO COM HIPERATIVIDADE

- Ansiedade

- Autoestima

- Depressão


Novas 4 colunas vão ser adicionadas ao DataFrame com a somatoria da escala para perguntas que definem cada aspecto de bem-estar mental.


```python
data.columns
```




    Index(['idade', 'genero', 'estado_civil', 'ocupacao', 'usa_rede_social',
           'redes_sociais_usadas', 'tempo_medio_gasto',
           'gasta_tempo_nas_redes_sociais_sem_motivo',
           'distraido_pelas_redes_sociais', 'inquieto_sem_uso_redes_sociais',
           'escala_distraido', 'escala_preocupacoes', 'escala_concentracao',
           'comparacao_pessoas_sucedidas', 'sentimentos_sobre_comparacao',
           'busca_por_validacao', 'sentimento_pra_baixo',
           'escala_interesse_atividades_diarias', 'escala_problema_sono'],
          dtype='object')




```python
TDAH = ['gasta_tempo_nas_redes_sociais_sem_motivo', 'distraido_pelas_redes_sociais', 'escala_distraido', 'escala_concentracao']
data['TDAH Score'] = data[TDAH].sum(axis=1)

Ansiedade = ['inquieto_sem_uso_redes_sociais', 'escala_preocupacoes']
data['Ansiedade Score'] = data[Ansiedade].sum(axis=1)

Autoestima = ['comparacao_pessoas_sucedidas', 'sentimentos_sobre_comparacao','busca_por_validacao']
data['Autoestima Score'] = data[Autoestima].sum(axis=1)

Depressao = ['escala_problema_sono', 'escala_interesse_atividades_diarias','sentimento_pra_baixo']
data['Depressao Score'] = data[Depressao].sum(axis=1)

Total = ['TDAH Score', 'Ansiedade Score','Autoestima Score','Depressao Score']
data['Total Score'] = data[Total].sum(axis=1)
```


```python
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>usa_rede_social</th>
      <th>redes_sociais_usadas</th>
      <th>tempo_medio_gasto</th>
      <th>gasta_tempo_nas_redes_sociais_sem_motivo</th>
      <th>distraido_pelas_redes_sociais</th>
      <th>inquieto_sem_uso_redes_sociais</th>
      <th>...</th>
      <th>sentimentos_sobre_comparacao</th>
      <th>busca_por_validacao</th>
      <th>sentimento_pra_baixo</th>
      <th>escala_interesse_atividades_diarias</th>
      <th>escala_problema_sono</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>Total Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>Masculino</td>
      <td>Em Relacionamento</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>5</td>
      <td>4</td>
      <td>5</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube, Pinterest</td>
      <td>Between 3 and 4 hours</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>3</td>
      <td>1</td>
      <td>4</td>
      <td>2</td>
      <td>5</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>35</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>1</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>35</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>5</td>
      <td>4</td>
      <td>...</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>44</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>Masculino</td>
      <td>Solteiro</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>42</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>Feminino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Between 1 and 2 hours</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>2</td>
      <td>4</td>
      <td>4</td>
      <td>4</td>
      <td>1</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>35</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>Feminino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>...</td>
      <td>3</td>
      <td>4</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>34</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>Masculino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>3</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>4</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>37</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>Masculino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Less than an Hour</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>3</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>26</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 24 columns</p>
</div>



### Categorizando os aspectos mentais de acordo com a somatoria de cada Score


```python
data['TDAH_cut'] = pd.qcut(data['TDAH Score'], 4)

data['Ansiedade_cut'] = pd.qcut(data['Ansiedade Score'], 3)

data['Autoestima_cut'] = pd.qcut(data['Autoestima Score'], 3)

data['Depressão_cut'] = pd.qcut(data['Depressao Score'], 3)

data['Total_cut'] = pd.qcut(data['Total Score'], 3)

data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>usa_rede_social</th>
      <th>redes_sociais_usadas</th>
      <th>tempo_medio_gasto</th>
      <th>gasta_tempo_nas_redes_sociais_sem_motivo</th>
      <th>distraido_pelas_redes_sociais</th>
      <th>inquieto_sem_uso_redes_sociais</th>
      <th>...</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>Total Score</th>
      <th>TDAH_cut</th>
      <th>Ansiedade_cut</th>
      <th>Autoestima_cut</th>
      <th>Depressão_cut</th>
      <th>Total_cut</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>Masculino</td>
      <td>Em Relacionamento</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
      <td>(16.0, 20.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube, Pinterest</td>
      <td>Between 3 and 4 hours</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>35</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>35</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>5</td>
      <td>4</td>
      <td>...</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>44</td>
      <td>(16.0, 20.0]</td>
      <td>(7.0, 10.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>Masculino</td>
      <td>Solteiro</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>...</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>42</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>Feminino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Between 1 and 2 hours</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>35</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>Feminino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>...</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>34</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(2.999, 8.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>Masculino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>37</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>Masculino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Less than an Hour</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>26</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(2.999, 8.0]</td>
      <td>(11.999, 33.667]</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 29 columns</p>
</div>




```python
data['TDAH_cut'].unique()
```




    [(16.0, 20.0], (14.0, 16.0], (3.999, 11.0], (11.0, 14.0]]
    Categories (4, interval[float64, right]): [(3.999, 11.0] < (11.0, 14.0] < (14.0, 16.0] < (16.0, 20.0]]




```python
data['Ansiedade_cut'].unique()
```




    [(1.999, 5.0], (5.0, 7.0], (7.0, 10.0]]
    Categories (3, interval[float64, right]): [(1.999, 5.0] < (5.0, 7.0] < (7.0, 10.0]]




```python
data['Autoestima_cut'].unique()
```




    [(2.999, 7.0], (7.0, 9.0], (9.0, 15.0]]
    Categories (3, interval[float64, right]): [(2.999, 7.0] < (7.0, 9.0] < (9.0, 15.0]]




```python
data['Depressão_cut'].unique()
```




    [(11.0, 15.0], (8.0, 11.0], (2.999, 8.0]]
    Categories (3, interval[float64, right]): [(2.999, 8.0] < (8.0, 11.0] < (11.0, 15.0]]




```python
data['Total_cut'].unique()
```




    [(42.0, 60.0], (33.667, 42.0], (11.999, 33.667]]
    Categories (3, interval[float64, right]): [(11.999, 33.667] < (33.667, 42.0] < (42.0, 60.0]]



## Análise das variáveis com as perguntas

### *idade*


```python
#Pessoas com TDAH por idade
fig = px.box(data, x='idade', color='TDAH_cut')
fig.show()
```


<div>                            <div id="c258d59e-72b7-4fb5-a3e6-ed0938255824" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("c258d59e-72b7-4fb5-a3e6-ed0938255824")) {                    Plotly.newPlot(                        "c258d59e-72b7-4fb5-a3e6-ed0938255824",                        [{"alignmentgroup":"True","hovertemplate":"TDAH_cut=(16.0, 20.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(16.0, 20.0]","marker":{"color":"#636efa"},"name":"(16.0, 20.0]","notched":false,"offsetgroup":"(16.0, 20.0]","orientation":"h","showlegend":true,"x":[21,21,24,21,22,25,21,19,22,22,24,26,22,21,23,22,21,21,21,32,20,28,22,14,22,16,20,22,23,20,20,22,27,48,20,20,20,23,21,22,21,20,21,27,24,21,23,21,23,22,69,23,27,26,25,23,20,20,21,21,22,26,22,21,21,30,22,21,17,21,22,22,22,21,22,23,18,24,21,22,24,18,24,22,22,23,22,20,21,21,30,29,26,23,20,23,22,22,21,24,26,35,21,20,21,21,18,20,20,29,21,20,21,22,21,28,32],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"TDAH_cut=(14.0, 16.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(14.0, 16.0]","marker":{"color":"#EF553B"},"name":"(14.0, 16.0]","notched":false,"offsetgroup":"(14.0, 16.0]","orientation":"h","showlegend":true,"x":[21,28,23,20,22,22,21,22,27,25,18,19,23,24,21,18,28,17,22,22,17,30,22,22,26,34,22,20,49,50,20,47,19,47,20,25,24,21,27,27,34,37,19,22,19,23,91,22,22,21,22,23,24,22,20,26,22,23,21,21,25,21,19,25,21,20,22,19,21,22,22,19,17,26,18,23,23,24,23,21,21,22,20,28,13,20,26,24],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"TDAH_cut=(3.999, 11.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(3.999, 11.0]","marker":{"color":"#00cc96"},"name":"(3.999, 11.0]","notched":false,"offsetgroup":"(3.999, 11.0]","orientation":"h","showlegend":true,"x":[21,21,21,20,21,34,22,23,23,21,56,65,33,55,26,22,19,20,25,25,24,18,33,38,26,20,21,23,23,48,21,48,20,24,23,20,16,50,47,38,50,47,48,47,40,46,48,49,21,32,47,50,48,47,36,37,30,48,48,47,48,21,48,35,21,48,47,45,22,42,38,17,32,22,20,26,20,21,22,23,48,22,46,24,20,21,36,23,20,34,20,22,21,36,22,21,22,26,24,25,22,25,17,22,18,20,22,24,19,19,43,37,19,35,23,21,22,24,23,24,21,27,21,52,44,44,24,18,25,17,30,22,22,22,23,23,60,34,56,23,50,13,18,18,15,20,35,26,21,53],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"TDAH_cut=(11.0, 14.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(11.0, 14.0]","marker":{"color":"#ab63fa"},"name":"(11.0, 14.0]","notched":false,"offsetgroup":"(11.0, 14.0]","orientation":"h","showlegend":true,"x":[21,22,21,22,21,23,26,23,35,35,35,24,18,21,21,19,24,17,40,22,14,25,14,24,22,22,22,22,47,48,47,47,27,24,51,21,47,21,21,21,47,37,47,47,19,21,22,35,31,20,22,50,23,21,22,22,25,25,22,21,25,34,22,23,21,20,26,19,23,22,17,21,20,24,22,22,23,25,23,20,15,21,21,18,21,26,27,20,20,22,22,20,29,21,19,29,19,29,27,23,23,22,26,24,20,19,24,26,19,27,21,23,21,20,25,21,22,49,51,21,22,14,23,30,29],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"idade"}},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"legend":{"title":{"text":"TDAH_cut"},"tracegroupgap":0},"margin":{"t":60},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('c258d59e-72b7-4fb5-a3e6-ed0938255824');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
#Pessoas com ansiedade
fig = px.box(data, x='idade', color='Ansiedade_cut')
fig.show()
```


<div>                            <div id="a922e11d-d72f-458d-8a69-89d762b3944f" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("a922e11d-d72f-458d-8a69-89d762b3944f")) {                    Plotly.newPlot(                        "a922e11d-d72f-458d-8a69-89d762b3944f",                        [{"alignmentgroup":"True","hovertemplate":"Ansiedade_cut=(1.999, 5.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(1.999, 5.0]","marker":{"color":"#636efa"},"name":"(1.999, 5.0]","notched":false,"offsetgroup":"(1.999, 5.0]","orientation":"h","showlegend":true,"x":[21,21,20,24,21,21,22,23,22,21,21,56,24,65,40,33,55,26,22,20,25,19,33,38,26,22,32,22,17,22,14,20,16,23,48,21,47,48,20,22,20,16,50,47,38,50,48,51,47,40,46,47,48,49,21,47,50,48,21,47,36,37,30,48,47,48,21,48,21,35,48,47,47,47,22,21,42,35,17,32,26,21,34,22,23,48,37,23,21,46,24,20,25,21,36,22,23,21,25,20,34,34,25,22,22,23,36,22,22,26,21,19,22,24,24,22,25,22,22,21,23,20,24,18,19,21,19,26,27,22,43,19,37,19,23,29,21,22,23,22,24,22,20,27,24,44,44,21,26,18,20,19,30,21,23,25,21,22,60,34,56,51,50,28,13,14,13,18,15,35,21,53],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Ansiedade_cut=(5.0, 7.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(5.0, 7.0]","marker":{"color":"#EF553B"},"name":"(5.0, 7.0]","notched":false,"offsetgroup":"(5.0, 7.0]","orientation":"h","showlegend":true,"x":[21,21,21,22,21,21,22,21,23,25,28,34,26,23,35,20,35,24,18,21,17,22,19,22,25,25,18,24,18,14,25,14,23,24,21,21,24,22,30,22,22,22,21,23,47,47,27,34,20,20,20,24,24,49,47,27,21,20,32,48,20,20,47,23,45,21,19,38,21,22,27,20,24,20,31,20,22,50,22,22,22,23,21,23,25,23,19,27,20,21,21,20,21,22,23,21,22,17,21,24,22,26,21,25,26,25,30,18,20,15,22,17,21,23,21,21,22,23,20,18,22,20,29,21,22,24,29,35,25,19,21,23,22,27,24,21,21,23,24,21,23,29,20,26,26,23,19,21,22,21,21,19,52,26,19,24,23,25,23,21,21,22,22,22,27,23,22,21,21,20,23,28,49,23,21,22,23,18,20,20,26,24,26,29],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Ansiedade_cut=(7.0, 10.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(7.0, 10.0]","marker":{"color":"#00cc96"},"name":"(7.0, 10.0]","notched":false,"offsetgroup":"(7.0, 10.0]","orientation":"h","showlegend":true,"x":[21,22,23,23,35,22,21,22,19,21,22,19,22,24,26,27,21,23,21,18,28,22,21,17,20,28,22,22,22,48,26,20,22,23,22,23,50,47,19,48,47,20,21,20,21,21,37,25,22,24,22,20,21,27,27,21,19,22,22,69,23,26,23,91,22,20,20,21,22,26,23,20,22,20,17,22,23,22,21,22,22,22,21,21,21,22,25,20,24,21,21,19,24,18,22,22,20,30,20,22,23,22,22,35,20,24,21,21,17,18,18,23,26,20,17,24,29,21,20,22,20,21,30,32],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"idade"}},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"legend":{"title":{"text":"Ansiedade_cut"},"tracegroupgap":0},"margin":{"t":60},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('a922e11d-d72f-458d-8a69-89d762b3944f');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
#Pessoas com autoestima baixa
fig = px.box(data, x='idade', color='Autoestima_cut')
fig.show()
```


<div>                            <div id="b143762c-8a67-4aa8-ae89-86e9bd2d30b5" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("b143762c-8a67-4aa8-ae89-86e9bd2d30b5")) {                    Plotly.newPlot(                        "b143762c-8a67-4aa8-ae89-86e9bd2d30b5",                        [{"alignmentgroup":"True","hovertemplate":"Autoestima_cut=(2.999, 7.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(2.999, 7.0]","marker":{"color":"#636efa"},"name":"(2.999, 7.0]","notched":false,"offsetgroup":"(2.999, 7.0]","orientation":"h","showlegend":true,"x":[21,21,21,21,21,20,21,22,21,23,21,25,28,22,35,19,21,23,22,21,56,24,17,40,33,22,24,55,26,22,20,25,18,25,19,24,18,25,33,38,26,22,21,24,22,32,22,28,22,17,22,14,20,22,22,23,48,21,47,20,48,20,20,22,20,20,16,38,50,47,48,47,32,50,48,47,36,37,48,48,48,21,20,47,23,21,45,47,22,21,42,35,17,32,27,27,27,26,21,22,23,23,23,46,21,23,36,22,21,20,34,22,20,22,36,26,21,20,21,19,23,22,20,22,24,24,25,26,25,18,20,15,22,17,21,24,18,22,21,23,27,25,20,22,22,24,29,19,21,43,22,24,18,19,37,29,19,22,22,29,21,22,23,21,21,22,20,23,20,19,52,44,44,35,20,21,24,17,18,18,23,20,22,22,21,23,25,21,28,22,56,21,50,22,28,13,14,13,23,18,20,35],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Autoestima_cut=(7.0, 9.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(7.0, 9.0]","marker":{"color":"#EF553B"},"name":"(7.0, 9.0]","notched":false,"offsetgroup":"(7.0, 9.0]","orientation":"h","showlegend":true,"x":[21,21,24,34,23,23,23,20,22,24,18,22,21,19,22,19,14,23,24,21,18,28,21,17,22,30,16,22,47,47,34,20,24,50,47,24,49,51,47,40,46,47,48,49,21,47,21,30,48,47,47,20,20,35,21,37,48,47,22,47,24,22,20,21,22,20,21,22,20,23,19,27,25,26,25,23,21,21,22,22,21,22,24,26,22,25,21,22,17,23,21,30,23,22,21,20,22,21,19,22,19,26,20,20,21,24,35,23,25,22,24,23,26,22,21,22,22,27,22,21,26,24,21,26,23,26,19,24,30,21,23,21,20,23,21,60,49,34,23,18,15,20,30,21,53],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Autoestima_cut=(9.0, 15.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(9.0, 15.0]","marker":{"color":"#00cc96"},"name":"(9.0, 15.0]","notched":false,"offsetgroup":"(9.0, 15.0]","orientation":"h","showlegend":true,"x":[22,21,22,26,35,35,21,22,21,65,26,27,22,25,21,23,14,21,20,22,22,21,23,48,26,27,22,23,22,23,50,27,21,20,19,48,21,21,20,25,21,19,38,21,24,20,31,34,20,48,37,22,50,21,22,22,19,21,24,25,22,69,25,22,23,34,23,91,23,22,22,20,20,21,22,26,17,23,21,22,20,22,22,23,22,22,21,21,21,21,18,21,19,21,23,20,27,21,24,30,23,29,20,26,24,23,19,22,21,24,19,21,18,25,20,17,29,21,20,22,27,23,22,21,21,22,20,51,26,32,24,26,29],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"idade"}},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"legend":{"title":{"text":"Autoestima_cut"},"tracegroupgap":0},"margin":{"t":60},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('b143762c-8a67-4aa8-ae89-86e9bd2d30b5');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
#Pessoas com depressão
fig = px.box(data, x='idade', color='Depressão_cut')
fig.show()
```


<div>                            <div id="0269d927-ae2f-4cc7-80eb-a73ad03dc343" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("0269d927-ae2f-4cc7-80eb-a73ad03dc343")) {                    Plotly.newPlot(                        "0269d927-ae2f-4cc7-80eb-a73ad03dc343",                        [{"alignmentgroup":"True","hovertemplate":"Depressão_cut=(11.0, 15.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(11.0, 15.0]","marker":{"color":"#636efa"},"name":"(11.0, 15.0]","notched":false,"offsetgroup":"(11.0, 15.0]","orientation":"h","showlegend":true,"x":[21,21,21,24,22,26,23,35,22,19,23,26,19,25,18,14,23,14,23,18,28,22,21,20,30,22,22,22,23,23,20,20,20,24,20,49,27,21,21,19,48,20,20,23,21,37,21,21,22,20,27,24,34,22,21,25,22,69,25,19,23,26,23,91,21,20,20,21,22,26,17,23,20,24,22,21,20,22,25,30,21,15,22,21,22,21,22,22,21,21,22,23,18,29,19,18,19,24,25,21,23,22,20,27,21,24,30,29,26,20,23,22,21,24,26,19,35,21,17,18,18,26,20,21,23,21,20,21,22,22,20,21,28,20,32,21],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Depressão_cut=(8.0, 11.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(8.0, 11.0]","marker":{"color":"#EF553B"},"name":"(8.0, 11.0]","notched":false,"offsetgroup":"(8.0, 11.0]","orientation":"h","showlegend":true,"x":[21,21,21,22,21,21,21,22,21,23,23,20,22,21,35,22,19,24,17,22,24,27,22,22,20,25,25,19,24,21,18,25,26,24,21,21,21,22,22,28,17,22,14,20,22,23,21,47,26,47,27,34,20,22,20,23,51,50,47,20,48,47,37,30,48,47,20,20,48,25,22,24,22,27,21,20,20,50,21,21,22,22,22,19,23,23,22,22,23,23,27,25,23,22,21,22,23,21,22,21,22,24,22,25,22,23,21,22,18,23,21,18,19,22,21,21,25,20,20,20,24,21,21,21,37,29,35,22,23,21,21,23,20,24,26,23,19,21,22,22,22,21,19,52,21,20,24,24,26,23,23,17,24,29,22,22,21,23,22,21,20,23,25,28,22,22,13,20,35,26,24,26],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Depressão_cut=(2.999, 8.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(2.999, 8.0]","marker":{"color":"#00cc96"},"name":"(2.999, 8.0]","notched":false,"offsetgroup":"(2.999, 8.0]","orientation":"h","showlegend":true,"x":[20,21,25,28,34,22,23,35,24,18,21,21,22,21,21,56,22,65,40,33,55,26,33,38,17,24,32,22,22,16,22,21,48,48,47,48,22,22,16,50,47,24,38,50,47,48,47,40,46,47,48,49,21,32,47,50,36,48,47,48,21,48,21,21,47,35,21,47,45,47,47,22,19,42,35,38,17,32,21,20,27,26,31,21,22,23,48,37,23,46,24,20,21,36,21,25,20,34,34,22,20,22,21,36,22,22,20,26,21,19,22,24,26,22,25,17,26,23,20,20,17,24,19,26,27,22,22,43,22,24,19,23,22,19,29,21,22,24,23,22,22,20,27,44,44,21,18,20,25,19,30,27,21,23,21,60,49,34,56,51,23,21,50,14,13,23,18,18,15,30,29,53],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"idade"}},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"legend":{"title":{"text":"Depressão_cut"},"tracegroupgap":0},"margin":{"t":60},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('0269d927-ae2f-4cc7-80eb-a73ad03dc343');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
fig = px.box(data, x='idade', color='Total_cut')
fig.show()
```


<div>                            <div id="31f48d79-30f5-402a-992e-6b886d59e464" class="plotly-graph-div" style="height:525px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("31f48d79-30f5-402a-992e-6b886d59e464")) {                    Plotly.newPlot(                        "31f48d79-30f5-402a-992e-6b886d59e464",                        [{"alignmentgroup":"True","hovertemplate":"Total_cut=(42.0, 60.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(42.0, 60.0]","marker":{"color":"#636efa"},"name":"(42.0, 60.0]","notched":false,"offsetgroup":"(42.0, 60.0]","orientation":"h","showlegend":true,"x":[21,21,21,21,24,22,26,23,35,21,22,19,22,24,26,27,22,21,23,23,18,28,22,21,21,20,28,30,22,22,26,34,20,22,23,20,20,49,50,27,21,20,19,48,20,20,20,21,37,25,22,24,21,22,20,21,27,24,34,22,21,19,21,22,22,69,25,23,19,27,23,26,23,91,21,20,20,21,21,22,26,17,23,21,20,24,22,21,20,22,21,30,22,21,23,21,22,22,21,21,21,22,23,18,21,19,18,24,22,25,21,23,22,20,27,21,30,23,29,20,26,23,19,20,23,22,22,21,24,26,19,35,21,20,21,17,18,23,26,20,29,21,23,21,20,21,23,21,22,20,21,26,32],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Total_cut=(33.667, 42.0]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(33.667, 42.0]","marker":{"color":"#EF553B"},"name":"(33.667, 42.0]","notched":false,"offsetgroup":"(33.667, 42.0]","orientation":"h","showlegend":true,"x":[21,21,22,21,22,21,23,25,28,23,35,20,22,24,18,21,21,22,21,19,22,17,22,19,25,25,18,19,14,25,14,24,21,21,17,24,22,22,17,22,14,16,22,22,22,21,23,23,47,48,47,47,27,22,22,20,24,23,24,51,47,47,21,48,47,20,21,23,19,21,21,22,27,27,20,31,20,37,50,21,22,22,22,23,25,23,22,23,25,23,21,22,22,20,21,22,23,22,22,22,26,22,23,25,15,22,17,21,22,21,18,22,26,25,20,20,20,24,29,21,21,22,24,19,29,35,22,19,21,23,24,21,24,26,22,21,22,22,19,24,21,24,26,18,23,20,17,24,22,27,22,21,20,25,28,49,51,22,28,13,20,20,30,24,26,29,21],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"},{"alignmentgroup":"True","hovertemplate":"Total_cut=(11.999, 33.667]\u003cbr\u003eidade=%{x}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"(11.999, 33.667]","marker":{"color":"#00cc96"},"name":"(11.999, 33.667]","notched":false,"offsetgroup":"(11.999, 33.667]","orientation":"h","showlegend":true,"x":[21,21,20,21,34,22,23,35,23,21,56,24,65,40,33,55,26,22,20,25,24,18,33,38,26,22,32,22,20,48,21,48,20,20,16,50,47,38,50,47,48,47,40,46,48,49,21,32,47,50,48,47,36,37,30,48,47,48,21,48,21,47,35,21,48,47,45,47,47,22,42,35,38,17,32,20,26,21,22,23,48,23,46,24,20,21,36,21,25,20,34,34,22,20,22,36,22,26,21,19,22,24,26,24,25,22,25,17,23,20,18,20,24,19,21,19,27,22,22,43,37,19,23,29,21,22,24,23,22,20,27,21,52,44,44,18,25,19,30,22,22,21,23,23,21,22,60,34,56,23,21,50,13,14,23,18,18,15,35,53],"x0":" ","xaxis":"x","y0":" ","yaxis":"y","type":"box"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"idade"}},"yaxis":{"anchor":"x","domain":[0.0,1.0]},"legend":{"title":{"text":"Total_cut"},"tracegroupgap":0},"margin":{"t":60},"boxmode":"group"},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('31f48d79-30f5-402a-992e-6b886d59e464');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>


#### **Insights**

- Acredito que a análise por idade não seja adequada pois a população em sua maioria e de adultos com media de 20 anos, mas nota-se que entre as pessoas com a idade entre 20 anos e 30 anos tem maior problemas com saúde mental.

### *genero*


```python
sns.histplot(data, x='genero', hue='TDAH_cut',multiple="dodge",palette="muted", shrink=.8)
```




    <Axes: xlabel='genero', ylabel='Count'>




    
![png](output_84_1.png)
    



```python
sns.histplot(data, x='genero', hue='Ansiedade_cut',multiple="dodge",palette="muted", shrink=.8)
```




    <Axes: xlabel='genero', ylabel='Count'>




    
![png](output_85_1.png)
    



```python
sns.histplot(data, x='genero', hue='Autoestima_cut',multiple="dodge",palette="muted", shrink=.8)
```




    <Axes: xlabel='genero', ylabel='Count'>




    
![png](output_86_1.png)
    



```python
sns.histplot(data, x='genero', hue='Depressão_cut',multiple="dodge",palette="muted", shrink=.8)
```




    <Axes: xlabel='genero', ylabel='Count'>




    
![png](output_87_1.png)
    



```python
sns.histplot(data, x='genero', hue='Total_cut',multiple="dodge",palette="muted", shrink=.8)
```




    <Axes: xlabel='genero', ylabel='Count'>




    
![png](output_88_1.png)
    


#### **Insights**

- Entre o genero Masculino e Feminino o genero que possui mais problemas com saúde mental é o sexo feminino, mas na base de dados possuimos mais pessoas do genero feminino, pode ser que esta análise enviesada.
- Não é possivel analisar a categoria Outros pois é uma parte muito pequena da população.

### *tempo_medio_gasto*


```python
plt.Figure()

ax = sns.histplot(data, x='tempo_medio_gasto', hue='TDAH_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Between 2 and 3 hours'),
     Text(1, 0, 'More than 5 hours'),
     Text(2, 0, 'Between 3 and 4 hours'),
     Text(3, 0, 'Less than an Hour'),
     Text(4, 0, 'Between 1 and 2 hours'),
     Text(5, 0, 'Between 4 and 5 hours')]




    
![png](output_91_1.png)
    



```python
plt.Figure()

ax = sns.histplot(data, x='tempo_medio_gasto', hue='Ansiedade_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Between 2 and 3 hours'),
     Text(1, 0, 'More than 5 hours'),
     Text(2, 0, 'Between 3 and 4 hours'),
     Text(3, 0, 'Less than an Hour'),
     Text(4, 0, 'Between 1 and 2 hours'),
     Text(5, 0, 'Between 4 and 5 hours')]




    
![png](output_92_1.png)
    



```python
plt.Figure()

ax = sns.histplot(data, x='tempo_medio_gasto', hue='Autoestima_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Between 2 and 3 hours'),
     Text(1, 0, 'More than 5 hours'),
     Text(2, 0, 'Between 3 and 4 hours'),
     Text(3, 0, 'Less than an Hour'),
     Text(4, 0, 'Between 1 and 2 hours'),
     Text(5, 0, 'Between 4 and 5 hours')]




    
![png](output_93_1.png)
    



```python
plt.Figure()

ax = sns.histplot(data, x='tempo_medio_gasto', hue='Depressão_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Between 2 and 3 hours'),
     Text(1, 0, 'More than 5 hours'),
     Text(2, 0, 'Between 3 and 4 hours'),
     Text(3, 0, 'Less than an Hour'),
     Text(4, 0, 'Between 1 and 2 hours'),
     Text(5, 0, 'Between 4 and 5 hours')]




    
![png](output_94_1.png)
    


#### **Insights**

- Nota-se que pessoas que passam mais de 5 horas, e pessosas que passam entre 4 a 5 horas possuem mais chance de ter problemas com saúde mental.

## Preparação dos Dados


```python
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>usa_rede_social</th>
      <th>redes_sociais_usadas</th>
      <th>tempo_medio_gasto</th>
      <th>gasta_tempo_nas_redes_sociais_sem_motivo</th>
      <th>distraido_pelas_redes_sociais</th>
      <th>inquieto_sem_uso_redes_sociais</th>
      <th>...</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>Total Score</th>
      <th>TDAH_cut</th>
      <th>Ansiedade_cut</th>
      <th>Autoestima_cut</th>
      <th>Depressão_cut</th>
      <th>Total_cut</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>Masculino</td>
      <td>Em Relacionamento</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
      <td>(16.0, 20.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube, Pinterest</td>
      <td>Between 3 and 4 hours</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>35</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram</td>
      <td>More than 5 hours</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>35</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>Feminino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>5</td>
      <td>4</td>
      <td>...</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>44</td>
      <td>(16.0, 20.0]</td>
      <td>(7.0, 10.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>Masculino</td>
      <td>Solteiro</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>...</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>42</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>Feminino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Between 1 and 2 hours</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>35</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>Feminino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Between 2 and 3 hours</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>...</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>34</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(2.999, 8.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>Masculino</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>Between 2 and 3 hours</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>37</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>Masculino</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>Less than an Hour</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>26</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(2.999, 8.0]</td>
      <td>(11.999, 33.667]</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 29 columns</p>
</div>




```python
data.loc[data['tempo_medio_gasto'] == 'Less than an Hour', 'tempo_medio_gasto'] = 0
data.loc[data['tempo_medio_gasto'] == 'Between 1 and 2 hours', 'tempo_medio_gasto'] = 1
data.loc[data['tempo_medio_gasto'] == 'Between 2 and 3 hours', 'tempo_medio_gasto'] = 2
data.loc[data['tempo_medio_gasto'] == 'Between 3 and 4 hours', 'tempo_medio_gasto'] = 3
data.loc[data['tempo_medio_gasto'] == 'Between 4 and 5 hours', 'tempo_medio_gasto'] = 4
data.loc[data['tempo_medio_gasto'] == 'More than 5 hours', 'tempo_medio_gasto'] = 5
data['tempo_medio_gasto'] = data['tempo_medio_gasto'].astype('int64')


data.loc[data['genero'] == 'Masculino', 'genero'] = 0
data.loc[data['genero'] == 'Feminino', 'genero'] = 1
data.loc[data['genero'] == 'Outros', 'genero'] = 2
data['genero'] = data['genero'].astype('int64')
```


```python
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>usa_rede_social</th>
      <th>redes_sociais_usadas</th>
      <th>tempo_medio_gasto</th>
      <th>gasta_tempo_nas_redes_sociais_sem_motivo</th>
      <th>distraido_pelas_redes_sociais</th>
      <th>inquieto_sem_uso_redes_sociais</th>
      <th>...</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>Total Score</th>
      <th>TDAH_cut</th>
      <th>Ansiedade_cut</th>
      <th>Autoestima_cut</th>
      <th>Depressão_cut</th>
      <th>Total_cut</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>0</td>
      <td>Em Relacionamento</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>2</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
      <td>(16.0, 20.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>5</td>
      <td>4</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>43</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube, Pinterest</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>35</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram</td>
      <td>5</td>
      <td>4</td>
      <td>2</td>
      <td>1</td>
      <td>...</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>35</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>2</td>
      <td>3</td>
      <td>5</td>
      <td>4</td>
      <td>...</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>44</td>
      <td>(16.0, 20.0]</td>
      <td>(7.0, 10.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(42.0, 60.0]</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>0</td>
      <td>Solteiro</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, Instagram, YouTube</td>
      <td>2</td>
      <td>3</td>
      <td>4</td>
      <td>3</td>
      <td>...</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>42</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>1</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>2</td>
      <td>...</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>35</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>1</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>4</td>
      <td>...</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>34</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(2.999, 8.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>0</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>True</td>
      <td>Facebook, Twitter, Instagram, YouTube, Discord...</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>2</td>
      <td>...</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>37</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(11.0, 15.0]</td>
      <td>(33.667, 42.0]</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>0</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>True</td>
      <td>Facebook, YouTube</td>
      <td>0</td>
      <td>2</td>
      <td>3</td>
      <td>1</td>
      <td>...</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>26</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(2.999, 8.0]</td>
      <td>(11.999, 33.667]</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 29 columns</p>
</div>




```python
data.columns
```




    Index(['idade', 'genero', 'estado_civil', 'ocupacao', 'usa_rede_social',
           'redes_sociais_usadas', 'tempo_medio_gasto',
           'gasta_tempo_nas_redes_sociais_sem_motivo',
           'distraido_pelas_redes_sociais', 'inquieto_sem_uso_redes_sociais',
           'escala_distraido', 'escala_preocupacoes', 'escala_concentracao',
           'comparacao_pessoas_sucedidas', 'sentimentos_sobre_comparacao',
           'busca_por_validacao', 'sentimento_pra_baixo',
           'escala_interesse_atividades_diarias', 'escala_problema_sono',
           'TDAH Score', 'Ansiedade Score', 'Autoestima Score', 'Depressao Score',
           'Total Score', 'TDAH_cut', 'Ansiedade_cut', 'Autoestima_cut',
           'Depressão_cut', 'Total_cut'],
          dtype='object')




```python
data.drop(columns=['usa_rede_social','redes_sociais_usadas','gasta_tempo_nas_redes_sociais_sem_motivo',
       'distraido_pelas_redes_sociais', 'inquieto_sem_uso_redes_sociais',
       'escala_distraido', 'escala_preocupacoes', 'escala_concentracao',
       'comparacao_pessoas_sucedidas', 'sentimentos_sobre_comparacao',
       'busca_por_validacao', 'sentimento_pra_baixo',
       'escala_interesse_atividades_diarias', 'escala_problema_sono','Total Score', 'TDAH_cut', 'Ansiedade_cut', 'Autoestima_cut',
       'Depressão_cut', 'Total_cut'], inplace=True)
```


```python
pd.get_dummies(data)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>tempo_medio_gasto</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>estado_civil_Casado</th>
      <th>estado_civil_Divorciado</th>
      <th>estado_civil_Em Relacionamento</th>
      <th>estado_civil_Solteiro</th>
      <th>ocupacao_Aposentado</th>
      <th>ocupacao_Assalariado</th>
      <th>ocupacao_Estudante</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>0</td>
      <td>2</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>1</td>
      <td>5</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>1</td>
      <td>3</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>1</td>
      <td>5</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>1</td>
      <td>2</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>0</td>
      <td>2</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>1</td>
      <td>1</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>1</td>
      <td>2</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>0</td>
      <td>2</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>0</td>
      <td>0</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 14 columns</p>
</div>



## Modelagem


```python
s = setup(data, session_id = 123)
```


<style type="text/css">
#T_1738a_row5_col1 {
  background-color: lightgreen;
}
</style>
<table id="T_1738a">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_1738a_level0_col0" class="col_heading level0 col0" >Description</th>
      <th id="T_1738a_level0_col1" class="col_heading level0 col1" >Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_1738a_level0_row0" class="row_heading level0 row0" >0</th>
      <td id="T_1738a_row0_col0" class="data row0 col0" >Session id</td>
      <td id="T_1738a_row0_col1" class="data row0 col1" >123</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row1" class="row_heading level0 row1" >1</th>
      <td id="T_1738a_row1_col0" class="data row1 col0" >Original data shape</td>
      <td id="T_1738a_row1_col1" class="data row1 col1" >(480, 9)</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row2" class="row_heading level0 row2" >2</th>
      <td id="T_1738a_row2_col0" class="data row2 col0" >Transformed data shape</td>
      <td id="T_1738a_row2_col1" class="data row2 col1" >(480, 14)</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row3" class="row_heading level0 row3" >3</th>
      <td id="T_1738a_row3_col0" class="data row3 col0" >Numeric features</td>
      <td id="T_1738a_row3_col1" class="data row3 col1" >7</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row4" class="row_heading level0 row4" >4</th>
      <td id="T_1738a_row4_col0" class="data row4 col0" >Categorical features</td>
      <td id="T_1738a_row4_col1" class="data row4 col1" >2</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row5" class="row_heading level0 row5" >5</th>
      <td id="T_1738a_row5_col0" class="data row5 col0" >Preprocess</td>
      <td id="T_1738a_row5_col1" class="data row5 col1" >True</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row6" class="row_heading level0 row6" >6</th>
      <td id="T_1738a_row6_col0" class="data row6 col0" >Imputation type</td>
      <td id="T_1738a_row6_col1" class="data row6 col1" >simple</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row7" class="row_heading level0 row7" >7</th>
      <td id="T_1738a_row7_col0" class="data row7 col0" >Numeric imputation</td>
      <td id="T_1738a_row7_col1" class="data row7 col1" >mean</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row8" class="row_heading level0 row8" >8</th>
      <td id="T_1738a_row8_col0" class="data row8 col0" >Categorical imputation</td>
      <td id="T_1738a_row8_col1" class="data row8 col1" >mode</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row9" class="row_heading level0 row9" >9</th>
      <td id="T_1738a_row9_col0" class="data row9 col0" >Maximum one-hot encoding</td>
      <td id="T_1738a_row9_col1" class="data row9 col1" >-1</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row10" class="row_heading level0 row10" >10</th>
      <td id="T_1738a_row10_col0" class="data row10 col0" >Encoding method</td>
      <td id="T_1738a_row10_col1" class="data row10 col1" >None</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row11" class="row_heading level0 row11" >11</th>
      <td id="T_1738a_row11_col0" class="data row11 col0" >CPU Jobs</td>
      <td id="T_1738a_row11_col1" class="data row11 col1" >-1</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row12" class="row_heading level0 row12" >12</th>
      <td id="T_1738a_row12_col0" class="data row12 col0" >Use GPU</td>
      <td id="T_1738a_row12_col1" class="data row12 col1" >False</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row13" class="row_heading level0 row13" >13</th>
      <td id="T_1738a_row13_col0" class="data row13 col0" >Log Experiment</td>
      <td id="T_1738a_row13_col1" class="data row13 col1" >False</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row14" class="row_heading level0 row14" >14</th>
      <td id="T_1738a_row14_col0" class="data row14 col0" >Experiment Name</td>
      <td id="T_1738a_row14_col1" class="data row14 col1" >cluster-default-name</td>
    </tr>
    <tr>
      <th id="T_1738a_level0_row15" class="row_heading level0 row15" >15</th>
      <td id="T_1738a_row15_col0" class="data row15 col0" >USI</td>
      <td id="T_1738a_row15_col1" class="data row15 col1" >b631</td>
    </tr>
  </tbody>
</table>




```python
models()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Name</th>
      <th>Reference</th>
    </tr>
    <tr>
      <th>ID</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>kmeans</th>
      <td>K-Means Clustering</td>
      <td>sklearn.cluster._kmeans.KMeans</td>
    </tr>
    <tr>
      <th>ap</th>
      <td>Affinity Propagation</td>
      <td>sklearn.cluster._affinity_propagation.Affinity...</td>
    </tr>
    <tr>
      <th>meanshift</th>
      <td>Mean Shift Clustering</td>
      <td>sklearn.cluster._mean_shift.MeanShift</td>
    </tr>
    <tr>
      <th>sc</th>
      <td>Spectral Clustering</td>
      <td>sklearn.cluster._spectral.SpectralClustering</td>
    </tr>
    <tr>
      <th>hclust</th>
      <td>Agglomerative Clustering</td>
      <td>sklearn.cluster._agglomerative.AgglomerativeCl...</td>
    </tr>
    <tr>
      <th>dbscan</th>
      <td>Density-Based Spatial Clustering</td>
      <td>sklearn.cluster._dbscan.DBSCAN</td>
    </tr>
    <tr>
      <th>optics</th>
      <td>OPTICS Clustering</td>
      <td>sklearn.cluster._optics.OPTICS</td>
    </tr>
    <tr>
      <th>birch</th>
      <td>Birch Clustering</td>
      <td>sklearn.cluster._birch.Birch</td>
    </tr>
  </tbody>
</table>
</div>




```python
exp = ClusteringExperiment()
exp.setup(data, session_id = 123)
kmeans = create_model('kmeans')
```


<style type="text/css">
#T_b731e_row5_col1 {
  background-color: lightgreen;
}
</style>
<table id="T_b731e">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_b731e_level0_col0" class="col_heading level0 col0" >Description</th>
      <th id="T_b731e_level0_col1" class="col_heading level0 col1" >Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_b731e_level0_row0" class="row_heading level0 row0" >0</th>
      <td id="T_b731e_row0_col0" class="data row0 col0" >Session id</td>
      <td id="T_b731e_row0_col1" class="data row0 col1" >123</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row1" class="row_heading level0 row1" >1</th>
      <td id="T_b731e_row1_col0" class="data row1 col0" >Original data shape</td>
      <td id="T_b731e_row1_col1" class="data row1 col1" >(480, 9)</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row2" class="row_heading level0 row2" >2</th>
      <td id="T_b731e_row2_col0" class="data row2 col0" >Transformed data shape</td>
      <td id="T_b731e_row2_col1" class="data row2 col1" >(480, 14)</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row3" class="row_heading level0 row3" >3</th>
      <td id="T_b731e_row3_col0" class="data row3 col0" >Numeric features</td>
      <td id="T_b731e_row3_col1" class="data row3 col1" >7</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row4" class="row_heading level0 row4" >4</th>
      <td id="T_b731e_row4_col0" class="data row4 col0" >Categorical features</td>
      <td id="T_b731e_row4_col1" class="data row4 col1" >2</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row5" class="row_heading level0 row5" >5</th>
      <td id="T_b731e_row5_col0" class="data row5 col0" >Preprocess</td>
      <td id="T_b731e_row5_col1" class="data row5 col1" >True</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row6" class="row_heading level0 row6" >6</th>
      <td id="T_b731e_row6_col0" class="data row6 col0" >Imputation type</td>
      <td id="T_b731e_row6_col1" class="data row6 col1" >simple</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row7" class="row_heading level0 row7" >7</th>
      <td id="T_b731e_row7_col0" class="data row7 col0" >Numeric imputation</td>
      <td id="T_b731e_row7_col1" class="data row7 col1" >mean</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row8" class="row_heading level0 row8" >8</th>
      <td id="T_b731e_row8_col0" class="data row8 col0" >Categorical imputation</td>
      <td id="T_b731e_row8_col1" class="data row8 col1" >mode</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row9" class="row_heading level0 row9" >9</th>
      <td id="T_b731e_row9_col0" class="data row9 col0" >Maximum one-hot encoding</td>
      <td id="T_b731e_row9_col1" class="data row9 col1" >-1</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row10" class="row_heading level0 row10" >10</th>
      <td id="T_b731e_row10_col0" class="data row10 col0" >Encoding method</td>
      <td id="T_b731e_row10_col1" class="data row10 col1" >None</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row11" class="row_heading level0 row11" >11</th>
      <td id="T_b731e_row11_col0" class="data row11 col0" >CPU Jobs</td>
      <td id="T_b731e_row11_col1" class="data row11 col1" >-1</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row12" class="row_heading level0 row12" >12</th>
      <td id="T_b731e_row12_col0" class="data row12 col0" >Use GPU</td>
      <td id="T_b731e_row12_col1" class="data row12 col1" >False</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row13" class="row_heading level0 row13" >13</th>
      <td id="T_b731e_row13_col0" class="data row13 col0" >Log Experiment</td>
      <td id="T_b731e_row13_col1" class="data row13 col1" >False</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row14" class="row_heading level0 row14" >14</th>
      <td id="T_b731e_row14_col0" class="data row14 col0" >Experiment Name</td>
      <td id="T_b731e_row14_col1" class="data row14 col1" >cluster-default-name</td>
    </tr>
    <tr>
      <th id="T_b731e_level0_row15" class="row_heading level0 row15" >15</th>
      <td id="T_b731e_row15_col0" class="data row15 col0" >USI</td>
      <td id="T_b731e_row15_col1" class="data row15 col1" >f75c</td>
    </tr>
  </tbody>
</table>








<style type="text/css">
</style>
<table id="T_45c6a">
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_45c6a_level0_col0" class="col_heading level0 col0" >Silhouette</th>
      <th id="T_45c6a_level0_col1" class="col_heading level0 col1" >Calinski-Harabasz</th>
      <th id="T_45c6a_level0_col2" class="col_heading level0 col2" >Davies-Bouldin</th>
      <th id="T_45c6a_level0_col3" class="col_heading level0 col3" >Homogeneity</th>
      <th id="T_45c6a_level0_col4" class="col_heading level0 col4" >Rand Index</th>
      <th id="T_45c6a_level0_col5" class="col_heading level0 col5" >Completeness</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_45c6a_level0_row0" class="row_heading level0 row0" >0</th>
      <td id="T_45c6a_row0_col0" class="data row0 col0" >0.2842</td>
      <td id="T_45c6a_row0_col1" class="data row0 col1" >432.5744</td>
      <td id="T_45c6a_row0_col2" class="data row0 col2" >1.1296</td>
      <td id="T_45c6a_row0_col3" class="data row0 col3" >0</td>
      <td id="T_45c6a_row0_col4" class="data row0 col4" >0</td>
      <td id="T_45c6a_row0_col5" class="data row0 col5" >0</td>
    </tr>
  </tbody>
</table>








```python
kmeans_cluster = assign_model(kmeans)
kmeans_cluster
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>tempo_medio_gasto</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>Cluster</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>0</td>
      <td>Em Relacionamento</td>
      <td>Estudante</td>
      <td>2</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>Cluster 0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>5</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>Cluster 0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>3</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>Cluster 3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>5</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>Cluster 3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>2</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>Cluster 0</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>0</td>
      <td>Solteiro</td>
      <td>Assalariado</td>
      <td>2</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>Cluster 0</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>1</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>1</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>Cluster 3</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>1</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>2</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>Cluster 2</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>0</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>2</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>Cluster 3</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>0</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>0</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>Cluster 1</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 10 columns</p>
</div>




```python
plot_model(kmeans, plot = 'cluster')
```






<div>                            <div id="98811256-deee-4ba6-8ee4-b0919cfb69e8" class="plotly-graph-div" style="height:600px; width:100%;"></div>            <script type="text/javascript">                require(["plotly"], function(Plotly) {                    window.PLOTLYENV=window.PLOTLYENV || {};                                    if (document.getElementById("98811256-deee-4ba6-8ee4-b0919cfb69e8")) {                    Plotly.newPlot(                        "98811256-deee-4ba6-8ee4-b0919cfb69e8",                        [{"customdata":[[21.0],[21.0],[22.0],[22.0],[24.0],[20.0],[21.0],[23.0],[17.0],[26.0],[22.0],[21.0],[21.0],[22.0],[20.0],[20.0],[20.0],[21.0],[22.0],[25.0],[21.0],[22.0],[21.0],[22.0],[22.0],[21.0],[23.0],[22.0],[21.0],[17.0],[22.0],[21.0],[22.0],[30.0],[21.0],[23.0],[22.0],[22.0],[21.0],[27.0],[21.0],[27.0],[20.0],[22.0],[21.0],[24.0],[22.0],[25.0],[21.0],[23.0],[20.0],[20.0],[20.0],[20.0],[27.0],[24.0],[22.0],[21.0],[23.0],[25.0],[26.0],[23.0],[27.0],[19.0],[23.0],[21.0],[22.0],[22.0],[23.0],[25.0],[21.0],[23.0],[19.0],[22.0],[22.0],[19.0],[22.0],[25.0],[26.0],[25.0],[20.0],[23.0],[18.0],[18.0],[26.0],[17.0],[21.0],[24.0],[24.0],[21.0],[35.0],[44.0],[52.0],[17.0],[21.0],[24.0],[21.0],[32.0],[30.0],[35.0],[20.0],[22.0],[21.0],[20.0],[22.0],[23.0],[23.0],[23.0],[21.0],[20.0],[21.0],[23.0],[30.0],[22.0],[27.0],[23.0],[23.0],[21.0],[25.0],[22.0],[22.0],[24.0],[18.0],[24.0],[22.0],[21.0],[19.0],[29.0],[21.0],[24.0],[18.0],[22.0],[20.0],[null],[24.0],[22.0],[22.0],[20.0],[21.0],[19.0],[23.0],[22.0],[23.0],[26.0],[20.0],[29.0],[22.0],[30.0],[21.0],[23.0],[23.0],[24.0],[21.0],[25.0],[18.0],[21.0],[21.0],[21.0],[22.0],[28.0],[18.0],[17.0],[21.0],[23.0],[23.0],[21.0],[20.0],[25.0],[22.0],[24.0],[27.0],[20.0],[28.0],[22.0],[20.0],[20.0],[23.0],[22.0],[20.0],[22.0],[27.0],[22.0],[22.0],[16.0],[22.0],[22.0],[17.0],[26.0],[26.0],[22.0],[24.0],[21.0],[21.0],[22.0],[21.0],[24.0],[21.0],[22.0],[25.0],[27.0],[23.0],[20.0],[22.0],[21.0],[22.0],[26.0],[19.0],[20.0],[21.0],[22.0],[22.0],[22.0],[21.0],[19.0],[22.0]],"hovertemplate":"Cluster=Cluster 0\u003cbr\u003ePCA1=%{x}\u003cbr\u003ePCA2=%{y}\u003cbr\u003eFeature=%{customdata[0]}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Cluster 0","marker":{"color":"#636efa","opacity":0.5,"symbol":"circle"},"mode":"markers","name":"Cluster 0","orientation":"v","showlegend":true,"x":[-5.978369300325241,-6.239391374089608,-5.246992118823475,-4.874256356844176,-2.9273824832275483,-6.8334043067714365,-5.366771976774837,-3.9882038386941177,-9.19584357197311,-2.1663902275405262,-6.029463800547811,-6.448171248488486,-5.77121826869303,-4.5841872657266505,-7.913981672259323,-7.322404026946391,-7.241747379319378,-5.285972187640155,-4.402516319248747,-1.5214792029818633,-5.811220261925465,-4.94967665223487,-6.31253663127593,-6.248604007623393,-6.110493391766317,-6.551758909285699,-3.741757031955597,-4.384072247296929,-5.438607985983885,-9.451109096205455,-4.824391768607211,-6.121855984387387,-4.878360138291051,2.8787215315687265,-5.91603488495984,-3.2788737510437005,-4.348729851860327,-4.624843578952291,-5.500661789523985,-0.9765115542714253,-6.220166486791275,0.39412186237110214,-7.245893247085495,-4.697924961456291,-6.322260209452882,-2.8933247712293824,-5.147169322169711,-1.6905538024583857,-6.169642931585301,-3.8660674251057703,-7.148190379613405,-7.210524095837671,-7.0816542818307315,-6.896013878426852,0.9111781975261858,-3.6418328882059403,-4.341651552056741,-6.217452164464064,-4.329616283328196,-2.005527943390508,-1.6900242318991414,-4.176709276529122,0.011243605738018539,-7.831475250562077,-3.7438128262523174,-6.596099420636191,-4.245043281251386,-4.784495770948542,-3.9213228964699636,-1.2736519708726568,-6.187462671322104,-3.8722909339109397,-7.883221893382153,-4.272080055171852,-5.000510758335635,-8.212204959110935,-5.406791362700542,-1.829233065117657,-7.257745513616456,-0.7829540335250138,-3.9100757990217714,-6.166720147388033,-3.397550957253336,-8.628255830445292,-9.285154903891822,-0.6837856050980958,-10.029604973689638,-6.371573141771062,-5.520516420589535,-7.133594209619596,-5.86193710210023,-8.03014984402018,-1.1521129001782693,-2.3870681922577695,-3.3169334743920125,1.589210130563234,-4.091475537044332,-2.4293126384765134,4.133069475915007,-0.6198260847483397,-6.695257531733166,0.9837616268021038,1.533037289601208,-6.115046427190117,-7.1561818603339375,-5.450988909388803,-5.978086222650912,-4.473761165737687,-3.4848595139943086,-6.194897684786173,-7.770259130363529,-6.042551093078217,-6.337442250993357,-6.617109918554931,-5.157276085096113,-5.410905718261272,-4.227170439038401,-5.652644032450909,-1.6898389374810392,-4.853799860332874,-4.895709127030502,-3.001324013338654,-9.065909731254667,-2.6111429189315882,-4.465151849843023,-5.663045456006861,-8.147436833243624,2.202245518676175,-5.617386498021806,-2.8942955079868753,-9.531820790176114,-5.182228924654222,-8.168560075089331,0.5136169305883995,-6.348210007799277,-4.801780596552324,-4.710113477618275,-4.488262896050136,-7.684431804751174,-5.320989217259791,-7.555925528462044,-3.839990407745292,-4.110288930390864,-4.120653580360243,-0.705525116071168,-6.661821602743786,0.8931805573314429,-3.2063823647664553,2.432685567742286,-2.4309325767325083,-5.701916158612094,-0.7791799053955264,-5.319163141689694,-1.7328922018128579,-8.966717037843502,-6.490053115177256,-6.332330977559968,-5.732806605518157,-5.235738892413854,0.9728801605734277,-8.957583740615554,-9.227914818350655,-5.51788942511885,-4.099458002759517,-5.075673441259997,-6.270383196656431,-6.811065755677023,-1.667946461150545,-4.697400866822368,-2.462706981353725,0.014591976253181177,-7.526788933959228,0.6320491173556445,-4.946724290947218,-7.19233605948518,-7.140992705968628,-4.966331412057555,-5.08694624117551,-7.271099656393903,-4.4924432766499445,0.7621922714646616,-4.810863503993217,-4.4689990682293095,-10.16681681286414,-5.3559506785542155,-4.478353365927177,-9.460859789669014,-0.8470509652704756,-1.5917147162479575,-4.22424983596791,-3.190632861901222,-5.946307171053895,-5.662804951197392,-4.097217966750067,-5.282482382055497,-3.3359741206161417,-5.403467470326529,-6.2412091011751745,-1.3422341850054371,-0.3626912795310686,-4.121263150407042,-6.401634500146031,-4.435433239518529,-5.963925388945291,-4.747244307458153,-0.6437745679710748,-8.569402433241486,-6.790900931650526,-5.36963342130515,-4.722590076364248,-4.18595588780317,-4.281581649015482,-5.637240006577113,-7.415488706725263,-4.449692983035883],"xaxis":"x","y":[3.174224579298262,4.215580458304574,4.68498618066305,2.6890745052098053,3.22525781654298,1.7901070632767293,1.2963966737804995,3.9688111244099584,-0.008839837176853165,9.636959591439554,9.272713929128852,5.681065657005676,2.7287740674302827,1.7110894906220464,7.403613988299778,4.0686424902254625,5.473163160978106,0.05120700997992206,1.060981708495349,0.9771917596274895,2.781098144655031,2.0315724210919983,5.191677499923704,10.05095490273149,8.524850810002016,5.541276135129657,2.7711874122255384,0.19770718537585374,0.6298091025010796,0.05572119365375806,1.7099101100224703,3.9665975597952743,3.4000768176034026,5.097058016547192,3.3489685745840694,0.6193218517326915,0.42652562648677544,1.9166801732538445,1.207534738063649,8.005924135617407,5.052985995175929,1.437954772171332,4.301161086951493,1.8412900199862714,5.797117027860809,3.1782322790048516,3.8898104736097565,3.0038961520335303,3.5398082555592656,2.672998298901621,4.4557656494265645,4.589134969763916,3.857330127129151,2.352504159730627,-0.12927613326042395,6.896662884926027,1.076834704840278,4.312566260481801,4.88911352340643,3.7736836266884546,7.28600937894249,4.2622303937910075,4.278897183513119,2.258841560069065,3.3497374436440577,6.9733078948593485,0.0738683033386087,3.1943043786017142,2.7475063375657203,1.2507359045650102,4.231817385198681,2.906949924298352,3.16366940853903,0.33764343318732387,3.1729064581746362,4.434869126878307,5.602935312670385,2.3898447323292715,4.654573172070725,3.25253426653861,2.969213136383668,-0.3442532546656742,0.586308584207371,0.7505556351985686,4.72246985050236,2.1960939099431096,2.290896329191582,5.209055510788294,1.1712200183242412,3.3051596018073526,2.8372775104799337,3.664037391553492,4.157227547060604,1.2389800362488574,5.064409328282622,6.571405247818021,3.571039881416097,1.7440673634995896,9.182415013504636,3.0966999405737416,1.1239090801913731,4.357894447172117,0.8125813998830483,3.999313561324493,3.970007426864014,6.02470622772919,3.2608200962280876,1.5187074705983061,1.6937269815786082,4.069501017367934,7.573411322674713,3.8080360876661157,5.04160595867174,6.234993080164983,3.9730857815532947,5.671586087310171,4.746906879247831,1.8819489098073425,2.844407662653572,2.4380733512538204,3.043089859359589,3.604602743203795,2.420899563820039,1.5143048587434007,0.8682280215855557,1.6665468029018589,3.1167237566216133,2.6247366130957746,2.14942197994313,2.7136348932022054,5.979358911545255,4.698589038243452,9.63205859039003,2.1889686233039334,5.9526250838094334,6.466884218320299,1.747964575984636,0.9181639223332787,6.0950200284433995,0.36515751569377713,1.4991135673525469,3.0482917783823553,4.279046944991858,-0.3520992776650426,2.2013723129936857,2.9019812688739575,9.29830304296588,1.1169044327197266,7.757101844368192,0.9561123012072736,1.7730467633566696,2.929454190753707,0.11326114800702808,2.882300629828448,1.876870229487495,5.944180562409324,4.468883939434535,1.8293713479463396,4.502960299731855,4.031663025889524,2.644624618084669,-0.10469171383890384,1.1353290265735259,3.9986983596769123,9.873384051824921,5.430226001836986,1.6134424560381158,1.4391358174328661,2.764315259918341,1.444065214290513,3.928480946654491,6.2179465451351295,5.599379790834226,2.5384503761660624,3.621215027520967,3.6788976431122546,8.751251350628996,3.944727944756215,4.256144810599646,1.1346843039496124,0.6645145062762899,3.161483354828209,0.3801097531015034,-0.8914163553866166,5.3782816130377205,0.06675029524338888,0.05206083547816509,3.8476911458815772,8.172519041955184,0.5777179941266228,4.135272217210266,2.392120444122578,2.24797037875151,-0.03706703484545976,0.7192046733635973,5.433195941671608,0.4712359634879247,10.042225215563532,0.5849139790559108,6.268423566915578,4.039180336823288,0.44269149087743925,0.9588251399208353,3.795372854394526,2.607962671477718,2.3945760562497838,5.459906715519163,2.3454525788200207,-0.2800183783739173,2.2726420545839496,-0.5569430712986952,-0.1542001428063175,1.540176875065666,0.04217374777247699,0.9167192267785874],"yaxis":"y","type":"scatter","textposition":"top center"},{"customdata":[[44.0],[34.0],[56.0],[21.0],[48.0],[91.0],[55.0],[69.0],[65.0],[60.0],[48.0],[47.0],[22.0],[26.0],[47.0],[48.0],[43.0],[56.0],[19.0],[50.0],[47.0],[48.0],[21.0],[42.0],[48.0],[48.0],[46.0],[48.0],[47.0],[50.0],[51.0],[47.0],[48.0],[47.0],[48.0],[47.0],[47.0],[47.0],[48.0],[50.0],[49.0],[47.0],[49.0],[48.0],[48.0],[47.0],[45.0],[48.0],[47.0],[46.0],[47.0],[47.0],[47.0],[47.0],[50.0],[50.0]],"hovertemplate":"Cluster=Cluster 1\u003cbr\u003ePCA1=%{x}\u003cbr\u003ePCA2=%{y}\u003cbr\u003eFeature=%{customdata[0]}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Cluster 1","marker":{"color":"#EF553B","opacity":0.5,"symbol":"circle"},"mode":"markers","name":"Cluster 1","orientation":"v","showlegend":true,"x":[20.279050907538593,31.746712974169096,24.88837241278074,26.00976958677715,23.136259244335598,62.043263646510106,29.971200350980823,39.69326203559227,38.844469967080215,22.86154837175632,21.40081717443421,20.602309428792626,35.37796918254434,20.130882230581424,20.853863886767762,23.74703466166025,19.044565259170675,31.356381560790208,26.20893282754989,24.974138399040296,20.8576622798815,22.031890283286355,27.719320319646027,17.610329541368582,23.113631486605414,19.962403753211802,21.330369387837013,21.70016964026675,21.927829627002975,23.42292884455559,24.503848958093943,20.18863391797951,23.906143590724795,21.81147616082401,22.911456301479806,20.741308813702535,21.417123924825237,20.586579679168022,23.36014024710211,25.220953492174942,21.443603030000414,21.365491364672902,23.017253437838313,22.126014546551186,23.159486400120972,21.491658439307844,19.365985067328115,21.999515692415603,21.596818605924845,21.475875945140896,21.881007550693052,21.12375051515375,20.79014487122748,21.07533494669886,22.69445236262112,24.739963487485635],"xaxis":"x","y":[-9.568831704313236,-6.367422541218763,3.231309318772719,-8.207675141352551,-3.2359472219117356,19.449429198357034,-2.210036040309907,19.699544207995256,4.345775344472265,3.2577603085071747,5.130234962288471,3.381398560080531,-3.4842548593674314,-8.858013042309205,2.422977839828453,-6.342613069227395,-7.842338847974733,-4.243578242817883,1.2006773809940456,-2.8663745878480085,2.612308786306509,1.6274701715112374,-0.6836190795346988,-6.7100076349916735,-2.742145451853782,10.74234565816111,-4.108935974695526,2.8631226386861637,-2.1469934519543346,5.774999223529101,4.837259200081631,5.820157101744243,-5.69578719945787,-1.444968332411704,-2.9130245233344416,3.314333905849139,-0.6088139044062882,3.5042533485058995,-4.5961267770443195,-3.857793410250264,8.83095174834305,-0.3493593571782339,1.5509370553911543,1.3594760518872162,-4.224986896666198,-0.12866256719965205,-0.15806556695261312,1.9507444836478152,-1.0112889460523784,-5.694707184831391,-2.3552056590633668,1.531783272595057,2.103795385047462,1.3331544003529372,8.38878466039272,-1.5910941199599595],"yaxis":"y","type":"scatter","textposition":"top center"},{"customdata":[[36.0],[31.0],[38.0],[34.0],[28.0],[29.0],[30.0],[34.0],[34.0],[35.0],[32.0],[40.0],[20.0],[32.0],[19.0],[35.0],[26.0],[35.0],[35.0],[40.0],[26.0],[33.0],[34.0],[33.0],[29.0],[36.0],[49.0],[38.0],[37.0],[30.0],[37.0],[35.0],[37.0],[34.0],[38.0],[32.0],[36.0],[37.0],[35.0],[29.0]],"hovertemplate":"Cluster=Cluster 2\u003cbr\u003ePCA1=%{x}\u003cbr\u003ePCA2=%{y}\u003cbr\u003eFeature=%{customdata[0]}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Cluster 2","marker":{"color":"#00cc96","opacity":0.5,"symbol":"circle"},"mode":"markers","name":"Cluster 2","orientation":"v","showlegend":true,"x":[11.080378006766216,5.071340928561528,12.96997063479608,8.697188246652491,1.8349410085446503,4.479083980537549,3.1367735321287906,8.807454308017785,6.708315398207988,10.88257157385647,7.689818213422401,14.54893658425862,9.470553368614784,6.212324531457465,6.853026027797665,8.722836551949325,3.873090667094336,9.139729054459886,8.080761584945922,14.274337584969594,3.514299454807042,8.288429549925063,6.992808722467987,8.034105229548219,3.033042976149844,10.7534545854376,10.111283697081365,13.45246122943091,11.576116757501355,4.880108530872821,10.870453780530399,9.243316715257096,10.104293492127644,10.12617600969035,12.47401656956869,6.314732035439292,11.816124526398312,11.678396424850042,9.388656516964339,3.2937343535497003],"xaxis":"x","y":[-4.547743210346099,0.5126604941580464,-3.7874196978898813,-2.3125106776383992,0.2999848703123241,-2.1539668299308454,3.7404749642967583,-2.464062337402803,6.716340692031871,-7.743252498125062,-7.705994104496862,-1.5907905467619974,-2.011980936689044,-0.3013802083720628,10.013591206839434,2.5608835666885907,0.5945930282924948,-0.3359583944452956,5.520625440575613,-0.028380728553515217,-1.806200039728612,-6.240334370210602,4.902360359983823,-5.239870470822236,-0.3939594062659434,-3.5398449207854292,-8.885736685056076,-5.879874690759484,-2.324324819783277,-3.9713538528251777,2.3065256295965,-0.19616887256927162,5.222171465450115,-9.760651258553128,-1.1403185032042151,-1.58862033707105,-7.432962272717181,-2.9530679947190026,-1.2584881229466607,-1.3965507490903948],"yaxis":"y","type":"scatter","textposition":"top center"},{"customdata":[[23.0],[24.0],[24.0],[21.0],[18.0],[21.0],[21.0],[21.0],[26.0],[22.0],[20.0],[24.0],[21.0],[23.0],[17.0],[20.0],[19.0],[21.0],[21.0],[21.0],[21.0],[25.0],[28.0],[51.0],[23.0],[50.0],[28.0],[13.0],[14.0],[13.0],[23.0],[18.0],[18.0],[15.0],[21.0],[21.0],[24.0],[20.0],[20.0],[21.0],[21.0],[21.0],[18.0],[21.0],[23.0],[23.0],[23.0],[22.0],[20.0],[22.0],[21.0],[21.0],[23.0],[21.0],[21.0],[22.0],[22.0],[22.0],[27.0],[22.0],[22.0],[21.0],[24.0],[24.0],[21.0],[23.0],[21.0],[23.0],[23.0],[22.0],[21.0],[22.0],[22.0],[22.0],[24.0],[24.0],[23.0],[24.0],[22.0],[25.0],[20.0],[14.0],[22.0],[25.0],[17.0],[20.0],[26.0],[26.0],[23.0],[19.0],[21.0],[29.0],[21.0],[23.0],[23.0],[20.0],[21.0],[20.0],[25.0],[20.0],[24.0],[20.0],[22.0],[20.0],[16.0],[22.0],[23.0],[21.0],[22.0],[22.0],[20.0],[26.0],[22.0],[21.0],[22.0],[19.0],[22.0],[23.0],[17.0],[21.0],[25.0],[14.0],[18.0],[24.0],[19.0],[19.0],[19.0],[25.0],[14.0],[23.0],[19.0],[21.0],[22.0],[20.0],[19.0],[22.0],[27.0],[21.0],[21.0],[26.0],[25.0],[20.0],[22.0],[22.0],[21.0],[20.0],[18.0],[20.0],[15.0],[22.0],[22.0],[24.0],[20.0],[26.0],[20.0],[21.0],[18.0],[19.0],[22.0],[21.0],[21.0],[26.0],[19.0],[26.0],[27.0],[20.0],[20.0],[22.0],[24.0]],"hovertemplate":"Cluster=Cluster 3\u003cbr\u003ePCA1=%{x}\u003cbr\u003ePCA2=%{y}\u003cbr\u003eFeature=%{customdata[0]}\u003cextra\u003e\u003c\u002fextra\u003e","legendgroup":"Cluster 3","marker":{"color":"#ab63fa","opacity":0.5,"symbol":"circle"},"mode":"markers","name":"Cluster 3","orientation":"v","showlegend":true,"x":[-3.4658423891107257,-1.726366490597424,-1.7303063663877636,-3.9529993372606977,-7.748142974804412,-4.898397461853984,-4.924892941374001,-2.8634458003324608,-1.7879063997459341,-5.69899160770695,2.347479083202714,-6.944544942054618,-4.082825317926814,-2.3926591617635666,-8.780938917796044,-2.1046173126314063,-8.665518810299695,-2.2565949085825268,-4.735621731088949,-4.869286211884645,-1.0206153220789986,-4.232333044194846,-3.5642666316272944,-2.4782088636953534,-4.7092442820713325,-3.989982624943036,-12.37065579620253,-11.168292561887482,-12.949386722960867,-2.7959113030056564,-6.711758509028984,-7.539663532062671,-9.274323732959491,-5.889139239513231,-4.883549903236649,-4.812904096869053,0.5989624277779094,-3.2501665034277036,-2.0788804232561082,-6.0803467725226055,-2.0455504523789907,-3.6218644722110085,-6.527933655314986,-3.9598218861872083,-2.6306325820175025,-0.13674734533040958,-2.3820400413747143,-3.1662905093479687,-6.377383488875151,-2.8806051605951266,-4.911858751319777,-4.107695902514832,-2.92542526905943,-4.877625002895685,-3.63691584572259,-3.8306140703783513,-3.8444817496135464,1.222169550512432,-4.983583623564251,-3.9017965241705945,-4.665053848207903,-3.701794819786975,-4.841122794312781,-1.7287132607809748,-2.506730816579088,-3.1166425633726167,-4.338323711848074,-2.7801551555036705,-3.0741814763943287,-4.066338744865367,-4.590227661240496,-4.104664205235497,-1.9089437819731048,-3.9229083977500947,-1.5498416084173683,-1.5821376894305905,-1.689786989536378,-0.8631894594197661,-3.7767560931870587,-0.47574984712906837,-5.057282113536362,-12.168031155276623,-2.423884980260642,0.2696697850350988,-8.060078297089252,-6.015585759480127,0.010098867677050974,0.4655916713554726,-2.347752779786637,-6.584204222992355,-3.9436910283858557,-4.814133240232994,-2.8346097066531213,-3.2710816842533883,-2.509126710610275,-5.490283887308579,-4.649849520986579,-4.138626744890086,-0.6817873000507291,-5.04726303540033,-1.3134914776635567,-5.489043160989432,-3.7281494310357655,-5.244205816124264,-8.910821590839152,-2.4452324269515544,-3.0684736286886274,-4.481852968775079,-2.90023580304601,-4.246569266371653,-5.739175190631837,0.20038020015952354,-3.633081293427663,-4.882995901829215,-3.789874020196997,-6.760726502024305,-1.2985095459775289,-2.6227810273180405,-6.899252798385468,-5.136661694175568,-1.1985326294348198,-11.895890475011447,-7.339235374968827,-1.445108183278088,-7.230420945565346,-6.989283084002179,-5.998737714688651,-0.4589663878087464,-12.232826396430543,-2.1257450724309006,-6.77084448997207,-3.7227803835458846,-2.3841404068819507,-4.385742865741941,-6.812395221357826,-3.257804421764209,-1.6236761936635054,-5.260322382012378,-1.7073160589198582,1.555254334275454,-1.076906212899825,-5.72066456555315,-3.584704350002994,-3.0388470715084153,-3.2384356141909563,-5.560932159525207,-7.338907185368985,-5.061522778917682,-11.311636994965513,-3.6024656448777574,-3.835548151038071,-1.6361376195534632,-5.482337160579698,2.0874484435249925,-4.9861538155642,-5.054602373523339,-8.237139871196497,-6.027917170823844,-3.726649518154998,-4.991665254924181,-4.586270431467362,1.4596332200389448,-6.07419400489866,0.1400141545030906,1.425297914152466,-5.823764778069795,-6.268607757713486,-4.199031241948233,-0.4037023099864138],"xaxis":"x","y":[-4.096881565164868,-2.5545556826064626,-1.7566117251404887,-6.133669204360467,-2.3459529810091886,-1.5403117612791146,-0.8001683204808775,-11.576514004933987,-1.1585601333922515,-3.085002176409946,-5.9139998907113,-2.1826711639002916,-5.127804106533188,-4.318682005985625,-2.8701309557493557,-0.7583557960031313,-2.5500803237525647,-0.4646883632817144,-3.497767888606263,-2.852279792255752,-1.3072606650593166,-5.467119929710134,-3.9369608825320284,-3.3695097724928753,-2.9036779735171825,-1.6597729992029846,-5.763047229388113,-6.020957109801139,-2.619684947111903,-2.6222375377869143,-7.918435729995714,-4.030234615318642,-9.690935412445613,-1.8834388334930068,-2.205525650752603,-2.491865208667511,-2.4995478902729986,-14.483711326959067,-5.145845484355425,-1.03857497590223,-5.529300412408014,-7.43784804407018,-8.94354504690293,-5.7283313747756965,-2.2987607735672224,-4.0277235914923075,-4.26987181957741,-5.310728194724555,-3.9350243553126485,-6.436598467093189,-0.7005381845667812,-4.718835297922643,-1.1854941064320683,-2.0492946363529403,-4.009739118088172,-1.2733100644469502,-2.81386004007814,-1.0295675733660772,-2.0485477473547937,-1.8440855565038312,-2.278828035635157,-7.408886938242553,-2.6811322998521296,-2.3137901522575666,-2.787847484858824,-0.8175292162754404,-4.743724698987591,-2.3620851840023116,-0.7183571306271265,-0.6293252178628234,-2.7816879751664088,-0.7353170011039001,-11.85163801866141,-1.4240794122554612,-3.3721727505634727,-2.4674303871445193,-7.8008856958510595,-6.636323498988602,-2.058311388777593,-3.3777773081529032,-6.758559052120458,-1.7824595533615673,-7.843643182264089,-6.325897655452088,-6.235169613187343,-1.3100994932180996,-0.9596834422643234,-2.9181830100681116,-4.6265212430409015,-4.278553124456965,-7.098824002158585,-2.0693326317587246,-12.151340944170952,0.051814380252621924,-2.9712835852010833,-3.9988850883853586,-2.7228642756046377,-9.514610761180785,-2.686500131224917,-5.813453916612112,-3.8865572308460723,-4.209335767374926,-2.380368175992814,-4.548454346812524,-6.8643052093337795,-9.336808841702453,-0.7965912401338201,-3.456523659570761,-5.645619425984744,-0.08740390949789272,-2.216449250239876,-1.913462325137939,-2.9605020619659985,-1.2162096821022446,-1.8184211358852185,-1.8530463122435485,-14.073293167260841,-2.8567517441348595,-11.526640289751121,-0.37761548598948524,-0.2581212524309933,-2.0528875607198653,-4.811872827255658,-3.6392471172982392,-0.9082588732280386,-1.9291552441678825,-6.572780048963336,-3.5130178564519055,-1.0734140692175667,-4.859229632544615,-2.305704602727699,-7.823229241720493,-8.981735395170748,-9.063194585302169,-2.4534978746596137,-5.38482837229236,-2.8041988055608327,-0.695241122730777,-7.366979098037443,-7.8872714174035385,0.2746346831516118,-2.111139321201437,-2.955905185449449,-6.388138779596884,-10.201368889355832,-3.04667141655189,-5.258829694857894,-5.855559829754361,-0.8328822362138525,-2.6035372553244613,-2.14836985670877,-2.43244778317924,-3.3034261817992374,-10.86712301496271,-5.905037399062953,-1.1055506378140618,-1.0571433684874738,-5.3980697859064115,-2.4790381488421476,-0.8400514125469147,-3.447431013953836,-7.865275227933436,-5.818773736711332,-0.958118951613504,-2.414437448664835,-2.230197326585043,-0.8525921853169515,-0.8118808773261635,-9.041198816966363],"yaxis":"y","type":"scatter","textposition":"top center"}],                        {"template":{"data":{"histogram2dcontour":[{"type":"histogram2dcontour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"choropleth":[{"type":"choropleth","colorbar":{"outlinewidth":0,"ticks":""}}],"histogram2d":[{"type":"histogram2d","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmap":[{"type":"heatmap","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"heatmapgl":[{"type":"heatmapgl","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"contourcarpet":[{"type":"contourcarpet","colorbar":{"outlinewidth":0,"ticks":""}}],"contour":[{"type":"contour","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"surface":[{"type":"surface","colorbar":{"outlinewidth":0,"ticks":""},"colorscale":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]]}],"mesh3d":[{"type":"mesh3d","colorbar":{"outlinewidth":0,"ticks":""}}],"scatter":[{"fillpattern":{"fillmode":"overlay","size":10,"solidity":0.2},"type":"scatter"}],"parcoords":[{"type":"parcoords","line":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolargl":[{"type":"scatterpolargl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"bar":[{"error_x":{"color":"#2a3f5f"},"error_y":{"color":"#2a3f5f"},"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"bar"}],"scattergeo":[{"type":"scattergeo","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterpolar":[{"type":"scatterpolar","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"histogram":[{"marker":{"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"histogram"}],"scattergl":[{"type":"scattergl","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatter3d":[{"type":"scatter3d","line":{"colorbar":{"outlinewidth":0,"ticks":""}},"marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattermapbox":[{"type":"scattermapbox","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scatterternary":[{"type":"scatterternary","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"scattercarpet":[{"type":"scattercarpet","marker":{"colorbar":{"outlinewidth":0,"ticks":""}}}],"carpet":[{"aaxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"baxis":{"endlinecolor":"#2a3f5f","gridcolor":"white","linecolor":"white","minorgridcolor":"white","startlinecolor":"#2a3f5f"},"type":"carpet"}],"table":[{"cells":{"fill":{"color":"#EBF0F8"},"line":{"color":"white"}},"header":{"fill":{"color":"#C8D4E3"},"line":{"color":"white"}},"type":"table"}],"barpolar":[{"marker":{"line":{"color":"#E5ECF6","width":0.5},"pattern":{"fillmode":"overlay","size":10,"solidity":0.2}},"type":"barpolar"}],"pie":[{"automargin":true,"type":"pie"}]},"layout":{"autotypenumbers":"strict","colorway":["#636efa","#EF553B","#00cc96","#ab63fa","#FFA15A","#19d3f3","#FF6692","#B6E880","#FF97FF","#FECB52"],"font":{"color":"#2a3f5f"},"hovermode":"closest","hoverlabel":{"align":"left"},"paper_bgcolor":"white","plot_bgcolor":"#E5ECF6","polar":{"bgcolor":"#E5ECF6","angularaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"radialaxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"ternary":{"bgcolor":"#E5ECF6","aaxis":{"gridcolor":"white","linecolor":"white","ticks":""},"baxis":{"gridcolor":"white","linecolor":"white","ticks":""},"caxis":{"gridcolor":"white","linecolor":"white","ticks":""}},"coloraxis":{"colorbar":{"outlinewidth":0,"ticks":""}},"colorscale":{"sequential":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"sequentialminus":[[0.0,"#0d0887"],[0.1111111111111111,"#46039f"],[0.2222222222222222,"#7201a8"],[0.3333333333333333,"#9c179e"],[0.4444444444444444,"#bd3786"],[0.5555555555555556,"#d8576b"],[0.6666666666666666,"#ed7953"],[0.7777777777777778,"#fb9f3a"],[0.8888888888888888,"#fdca26"],[1.0,"#f0f921"]],"diverging":[[0,"#8e0152"],[0.1,"#c51b7d"],[0.2,"#de77ae"],[0.3,"#f1b6da"],[0.4,"#fde0ef"],[0.5,"#f7f7f7"],[0.6,"#e6f5d0"],[0.7,"#b8e186"],[0.8,"#7fbc41"],[0.9,"#4d9221"],[1,"#276419"]]},"xaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"yaxis":{"gridcolor":"white","linecolor":"white","ticks":"","title":{"standoff":15},"zerolinecolor":"white","automargin":true,"zerolinewidth":2},"scene":{"xaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"yaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2},"zaxis":{"backgroundcolor":"#E5ECF6","gridcolor":"white","linecolor":"white","showbackground":true,"ticks":"","zerolinecolor":"white","gridwidth":2}},"shapedefaults":{"line":{"color":"#2a3f5f"}},"annotationdefaults":{"arrowcolor":"#2a3f5f","arrowhead":0,"arrowwidth":1},"geo":{"bgcolor":"white","landcolor":"#E5ECF6","subunitcolor":"white","showland":true,"showlakes":true,"lakecolor":"white"},"title":{"x":0.05},"mapbox":{"style":"light"}}},"xaxis":{"anchor":"y","domain":[0.0,1.0],"title":{"text":"PCA1"}},"yaxis":{"anchor":"x","domain":[0.0,1.0],"title":{"text":"PCA2"}},"legend":{"title":{"text":"Cluster"},"tracegroupgap":0},"margin":{"t":60},"plot_bgcolor":"rgb(240,240,240)","title":{"text":"2D Cluster PCA Plot"},"height":600},                        {"responsive": true}                    ).then(function(){

var gd = document.getElementById('98811256-deee-4ba6-8ee4-b0919cfb69e8');
var x = new MutationObserver(function (mutations, observer) {{
        var display = window.getComputedStyle(gd).display;
        if (!display || display === 'none') {{
            console.log([gd, 'removed!']);
            Plotly.purge(gd);
            observer.disconnect();
        }}
}});

// Listen for the removal of the full notebook cells
var notebookContainer = gd.closest('#notebook-container');
if (notebookContainer) {{
    x.observe(notebookContainer, {childList: true});
}}

// Listen for the clearing of the current output cell
var outputEl = gd.closest('.output');
if (outputEl) {{
    x.observe(outputEl, {childList: true});
}}

                        })                };                });            </script>        </div>



```python
plot_model(kmeans, plot = 'elbow')
```






    
![png](output_109_1.png)
    



```python
kmeans_cluster['TDAH_cut'] = pd.qcut(kmeans_cluster['TDAH Score'], 4)

kmeans_cluster['Ansiedade_cut'] = pd.qcut(kmeans_cluster['Ansiedade Score'], 3)

kmeans_cluster['Autoestima_cut'] = pd.qcut(kmeans_cluster['Autoestima Score'], 3)

kmeans_cluster['Depressão_cut'] = pd.qcut(kmeans_cluster['Depressao Score'], 3)

kmeans_cluster
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>estado_civil</th>
      <th>ocupacao</th>
      <th>tempo_medio_gasto</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
      <th>Cluster</th>
      <th>TDAH_cut</th>
      <th>Ansiedade_cut</th>
      <th>Autoestima_cut</th>
      <th>Depressão_cut</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>21</td>
      <td>0</td>
      <td>Em Relacionamento</td>
      <td>Estudante</td>
      <td>2</td>
      <td>18</td>
      <td>4</td>
      <td>7</td>
      <td>14</td>
      <td>Cluster 0</td>
      <td>(16.0, 20.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>5</td>
      <td>15</td>
      <td>7</td>
      <td>7</td>
      <td>14</td>
      <td>Cluster 0</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(11.0, 15.0]</td>
    </tr>
    <tr>
      <th>2</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>3</td>
      <td>11</td>
      <td>6</td>
      <td>7</td>
      <td>11</td>
      <td>Cluster 3</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(2.999, 7.0]</td>
      <td>(8.0, 11.0]</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>5</td>
      <td>12</td>
      <td>6</td>
      <td>8</td>
      <td>9</td>
      <td>Cluster 3</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
    </tr>
    <tr>
      <th>4</th>
      <td>21</td>
      <td>1</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>2</td>
      <td>17</td>
      <td>9</td>
      <td>9</td>
      <td>9</td>
      <td>Cluster 0</td>
      <td>(16.0, 20.0]</td>
      <td>(7.0, 10.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(8.0, 11.0]</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>24</td>
      <td>0</td>
      <td>Solteiro</td>
      <td>Assalariado</td>
      <td>2</td>
      <td>15</td>
      <td>6</td>
      <td>10</td>
      <td>11</td>
      <td>Cluster 0</td>
      <td>(14.0, 16.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
    </tr>
    <tr>
      <th>477</th>
      <td>26</td>
      <td>1</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>1</td>
      <td>10</td>
      <td>6</td>
      <td>10</td>
      <td>9</td>
      <td>Cluster 3</td>
      <td>(3.999, 11.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(8.0, 11.0]</td>
    </tr>
    <tr>
      <th>478</th>
      <td>29</td>
      <td>1</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>2</td>
      <td>12</td>
      <td>6</td>
      <td>10</td>
      <td>6</td>
      <td>Cluster 2</td>
      <td>(11.0, 14.0]</td>
      <td>(5.0, 7.0]</td>
      <td>(9.0, 15.0]</td>
      <td>(2.999, 8.0]</td>
    </tr>
    <tr>
      <th>479</th>
      <td>21</td>
      <td>0</td>
      <td>Solteiro</td>
      <td>Estudante</td>
      <td>2</td>
      <td>10</td>
      <td>5</td>
      <td>9</td>
      <td>13</td>
      <td>Cluster 3</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(11.0, 15.0]</td>
    </tr>
    <tr>
      <th>480</th>
      <td>53</td>
      <td>0</td>
      <td>Casado</td>
      <td>Assalariado</td>
      <td>0</td>
      <td>9</td>
      <td>2</td>
      <td>8</td>
      <td>7</td>
      <td>Cluster 1</td>
      <td>(3.999, 11.0]</td>
      <td>(1.999, 5.0]</td>
      <td>(7.0, 9.0]</td>
      <td>(2.999, 8.0]</td>
    </tr>
  </tbody>
</table>
<p>480 rows × 14 columns</p>
</div>




```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='TDAH_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_111_1.png)
    



```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='Ansiedade_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_112_1.png)
    



```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='Autoestima_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_113_1.png)
    



```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='Depressão_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_114_1.png)
    


## Insights

A partir da modelagem realizada, foi possível observar que os indivíduos pertencentes ao **Cluster 0** apresentam maior propensão a problemas de saúde mental, enquanto os demais grupos não apresentam médias altas na escala Likert.


```python
kmeans_cluster['idade_cut'] = pd.qcut(kmeans_cluster['idade'], 6)
```


```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='idade_cut',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_117_1.png)
    



```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='genero',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_118_1.png)
    



```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='estado_civil',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_119_1.png)
    



```python
plt.Figure()

ax = sns.histplot(kmeans_cluster, x='Cluster', hue='tempo_medio_gasto',multiple="dodge",palette="muted", shrink=.8)
ax.tick_params(axis='x', labelrotation=45)
ax.set_xticklabels(ax.get_xticklabels(), rotation=45, ha='right')
```




    [Text(0, 0, 'Cluster 0'),
     Text(1, 0, 'Cluster 3'),
     Text(2, 0, 'Cluster 2'),
     Text(3, 0, 'Cluster 1')]




    
![png](output_120_1.png)
    



```python
kmeans_cluster.corr()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>idade</th>
      <th>genero</th>
      <th>tempo_medio_gasto</th>
      <th>TDAH Score</th>
      <th>Ansiedade Score</th>
      <th>Autoestima Score</th>
      <th>Depressao Score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>idade</th>
      <td>1.000000</td>
      <td>-0.134974</td>
      <td>-0.361333</td>
      <td>-0.301063</td>
      <td>-0.253629</td>
      <td>-0.062461</td>
      <td>-0.304066</td>
    </tr>
    <tr>
      <th>genero</th>
      <td>-0.134974</td>
      <td>1.000000</td>
      <td>0.215704</td>
      <td>0.102384</td>
      <td>0.150707</td>
      <td>0.062021</td>
      <td>0.102340</td>
    </tr>
    <tr>
      <th>tempo_medio_gasto</th>
      <td>-0.361333</td>
      <td>0.215704</td>
      <td>1.000000</td>
      <td>0.453670</td>
      <td>0.443020</td>
      <td>0.184825</td>
      <td>0.346333</td>
    </tr>
    <tr>
      <th>TDAH Score</th>
      <td>-0.301063</td>
      <td>0.102384</td>
      <td>0.453670</td>
      <td>1.000000</td>
      <td>0.676207</td>
      <td>0.358809</td>
      <td>0.621464</td>
    </tr>
    <tr>
      <th>Ansiedade Score</th>
      <td>-0.253629</td>
      <td>0.150707</td>
      <td>0.443020</td>
      <td>0.676207</td>
      <td>1.000000</td>
      <td>0.422322</td>
      <td>0.580797</td>
    </tr>
    <tr>
      <th>Autoestima Score</th>
      <td>-0.062461</td>
      <td>0.062021</td>
      <td>0.184825</td>
      <td>0.358809</td>
      <td>0.422322</td>
      <td>1.000000</td>
      <td>0.402243</td>
    </tr>
    <tr>
      <th>Depressao Score</th>
      <td>-0.304066</td>
      <td>0.102340</td>
      <td>0.346333</td>
      <td>0.621464</td>
      <td>0.580797</td>
      <td>0.402243</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
plt.figure(figsize=(10, 8))
sns.heatmap(kmeans_cluster.corr().round(decimals=2), annot=True)
plt.show()
```


    
![png](output_122_0.png)
    


## Insights
A partir da análise realizada, é possível inferir que pessoas entre 20 a 22 anos que possuem um tempo médio gasto alto podem ter maior propensão a problemas de saúde mental. No entanto, é importante lembrar que essa é uma inferência baseada em uma análise de dados e não deve ser considerada uma verdade absoluta. É necessário realizar mais pesquisas para confirmar essa hipótese .

# Possiveis Soluções

Aqui estão algumas possíveis soluções que pessoas podem utilizar para ajudar a reduzir o tempo gasto em redes sociais:

**Defina limites de tempo**: Defina um limite diário para o tempo gasto em redes sociais e tente cumpri-lo. Você pode usar aplicativos de gerenciamento de tempo para ajudá-lo a monitorar o tempo gasto em redes sociais e definir lembretes para limitar o uso.

**Desative as notificações**: As notificações podem ser uma distração constante e interromper sua produtividade. Desative as notificações de redes sociais para evitar distrações desnecessárias.

**Encontre outras atividades**: Encontre outras atividades que você goste e que possam substituir o tempo gasto em redes sociais. Isso pode incluir hobbies, exercícios físicos, leitura ou passar tempo com amigos e familiares.

**Conecte-se com a natureza**: Passar tempo ao ar livre e em contato com a natureza pode ajudar a reduzir o estresse e a ansiedade. Considere fazer caminhadas, andar de bicicleta ou simplesmente passar algum tempo em um parque local.

**Procure ajuda profissional**: Se você está lutando para reduzir o tempo gasto em redes sociais e isso está afetando sua saúde mental, considere procurar ajuda profissional. Um terapeuta ou psicólogo pode ajudá-lo a desenvolver estratégias para lidar com o uso excessivo de redes sociais e melhorar sua saúde mental.

Aqui estão algumas possíveis soluções que o **Estado** pode utilizar para ajudar a reduzir o tempo gasto em redes sociais de sua população:

**Campanhas de conscientização**: O estado pode lançar campanhas de conscientização para educar as pessoas sobre os efeitos negativos do uso excessivo de redes sociais na saúde mental e no bem-estar.

**Regulamentação**: O estado pode regulamentar o uso de redes sociais, limitando o tempo que as pessoas podem passar em plataformas de redes sociais ou restringindo o acesso a determinados recursos.

**Incentivos**: O estado pode oferecer incentivos para as pessoas que reduzem o tempo gasto em redes sociais, como descontos em serviços públicos ou programas de bem-estar.

**Pesquisa**: O estado pode financiar pesquisas para entender melhor os efeitos do uso excessivo de redes sociais na saúde mental e no bem-estar e desenvolver estratégias eficazes para lidar com o problema.

**Parcerias**: O estado pode trabalhar em parceria com empresas de tecnologia e organizações sem fins lucrativos para desenvolver soluções inovadoras para reduzir o tempo gasto em redes sociais.


```python

```