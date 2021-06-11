# EEL891 - 2020.02 - Relatório do Trabalho 1
Classificação: sistema de apoio à decisão p/ aprovação de crédito

Aluno: Pedro Boechat



## Organização do Repositório

Nesse repositório podem ser encontrados dois Jupyter Notebooks:

- [`preprocessing.ipynb`](https://github.com/pedroboechat/EEL891_Trabalho1/blob/main/preprocessing.ipynb):

  Contém o código utilizado para realizar o pré-processamento dos conjuntos de treinamento e teste para utilização no modelo classificador.

- [`model.ipynb`](https://github.com/pedroboechat/EEL891_Trabalho1/blob/main/model.ipynb):

  Contém o código utilizado para implementar o modelo classificador e exportar os dados de predição.

e uma pasta [`data`](https://github.com/pedroboechat/EEL891_Trabalho1/tree/main/data) que contém os dados utilizados nos notebooks e os dados submetidos no [Kaggle](https://www.kaggle.com/c/eel891-202002-trabalho-1/).



## Pré-processamento

Por meio da análise dos dados usando a biblioteca `Pandas` e das anotações do arquivo [`dicionario_de_dados.xlsx`](https://github.com/pedroboechat/EEL891_Trabalho1/blob/main/data/dicionario_de_dados.xlsx), foram identificadas, primeiramente, colunas irrelevantes para o modelo ou com dados inconsistentes, que serão removidas. Em seguida, foram identificadas colunas com valores binários que necessitavam de conversão de (Y/N) para (0/1) e colunas com muitos valores não numéricos distintos que necessitavam codificação. Finalmente, foram identificadas colunas com valores numéricos que necessitavam aplicação de padronização. Colunas que não se encaixavam em nenhum dos critérios anteriores foram mantidas sem modificação. Assim, o resumo do pré-processamento dos dados é:

**1. Colunas removidas:**

```
'id_solicitante',
'grau_instrucao',
'estado_onde_nasceu',
'codigo_area_telefone_residencial',
'possui_telefone_celular',
'qtde_contas_bancarias_especiais',
'valor_patrimonio_pessoal',
'estado_onde_trabalha',
'possui_telefone_trabalho',
'codigo_area_telefone_trabalho',
'local_onde_trabalha'
```

**2. Colunas binarizadas:**

```
'possui_telefone_residencial',
'vinculo_formal_com_empresa'
```

**3. Colunas codificadas (*One-hot Encoding*):**

```
'sexo',
'forma_envio_solicitacao',
'estado_onde_reside'
```

**4. Colunas padronizadas:**

```
'produto_solicitado',
'dia_vencimento',
'tipo_endereco',
'idade',
'estado_civil',
'qtde_dependentes',
'nacionalidade',
'qtde_contas_bancarias',
'renda_mensal_regular',
'renda_extra',
'meses_no_trabalho',
'local_onde_reside',
'tipo_residencia',
'meses_na_residencia',
'profissao',
'ocupacao',
'profissao_companheiro',
'grau_instrucao_companheiro'
```

**5. Colunas sem modificação:**

```
'possui_email',
'possui_cartao_visa',
'possui_cartao_mastercard',
'possui_cartao_diners',
'possui_cartao_amex',
'possui_outros_cartoes',
'possui_carro',
'inadimplente' (y)
```

Além disso, foi identificada a falta de dados em algumas colunas, necessitando a utilização de um *imputer* para não prejudicar o modelo. São essas:

```
'tipo_residencia',
'meses_na_residencia',
'profissao',
'ocupacao',
'profissao_companheiro',
'grau_instrucao_companheiro'
```



## Implementação do modelo classificador

Com a biblioteca `Pandas`, os dados dos conjuntos de treinamento e teste são importados no formato de DataFrame e depois valores alocados em arrays. No caso do conjunto de treinamento os valores  são separados em dois arrays (um para as variáveis independentes e outro para a variável dependente).

Com a biblioteca `SciKit-Learn` vão ser realizadas as ações necessárias em certas colunas, identificadas anteriormente no pré-processamento dos dados. Utilizando um *imputer*, se substituem os valores faltantes das colunas, identificados por `np.nan`, pela média ao longo do eixo. Com o transformador de coluna e o one-hot encoder, realiza-se a codificação das colunas. Utiliza-se um *scaler* para padronizar os valores das colunas.

Terminado todo o pré-processamento dos dados, importamos da biblioteca `CatBoost` a classe que implementa o classificador. O `CatBoost` utiliza a técnica de *gradient boosting* para encontrar e treinar o melhor modelo de classificação para os dados que temos. Alimentamos o *fit* do classificador com os dados do conjunto de treinamento.

Com o *fit* do classificador concluído, utilizamos o *cross validation score* do `sklearn` para realizar uma análise preliminar do modelo treinado. Em seguida utilizamos o classificador para predizer os valores do conjunto de teste e exportamos esses para um arquivo CSV, que será enviado no Kaggle.



## Resultados

O modelo encontrou uma acurácia de 0.6006 e um desvio padrão de 0.0089 com o *cross validation score* e uma acurácia de 0.58880 no Kaggle.

