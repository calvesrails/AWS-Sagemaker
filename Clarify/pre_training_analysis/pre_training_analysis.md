# Iniciando os estudos de SageMaker Clarify (análise de viés antes do treino)

Nesta seção, começaremos a estudar recursos do **Amazon SageMaker Clarify** para **detectar possíveis vieses nos dados antes do treinamento do modelo**.

Ao final, iremos gerar um **relatório (incluindo PDF)** com os resultados dessa análise.

---

## O que é análise de viés no conjunto de dados antes do treino?

A **análise de viés pré-treinamento (pre-training bias analysis)** é uma etapa usada para avaliar se o conjunto de dados possui padrões que podem gerar resultados injustos ou desbalanceados no modelo.

Essa análise é importante porque:

- identifica possíveis desigualdades entre grupos (ex.: gênero, faixa etária, etc.)
- ajuda a avaliar riscos antes de treinar o modelo
- melhora a governança e a transparência do processo de ML
- permite corrigir problemas de dados antes do treinamento

Em outras palavras, o objetivo é analisar o dataset **antes** do modelo aprender com ele.

---

## Script utilizado

Utilizaremos o script/notebook localizado em:

- `Clarify/scripts/SageMaker_Clarify_Demo.ipynb`

> Importante: mantenha a prática de **debugar e entender o código** antes de executar as células.

---

## 01 - Preparando o ambiente no SageMaker Studio

No **SageMaker Studio**, podemos:

- criar um novo **JupyterLab**
- ou utilizar um JupyterLab já criado

Depois disso, podemos exportar/abrir nosso script no ambiente e também utilizar o dataset que já conhecemos:

- `Employee.csv`

![Preparando ambiente no SageMaker Studio](/Clarify/images/pre_training/pre-00.png)

---

## 02 - Inicializando a sessão do SageMaker e carregando o dataset

Vamos iniciar nossa sessão do SageMaker e carregar o dataset.

> Lembre-se de substituir, na célula, o nome do bucket pelo nome do **seu bucket S3**.

![Sessão SageMaker e carregamento do dataset](/Clarify/images/pre_training/pre-01.png)

---

## 03 - Preparando os dados (compatível com XGBoost)

Como novamente estamos utilizando o **XGBoost**, vamos preparar os dados de forma semelhante às etapas de treinamento anteriores.

Isso inclui, por exemplo:

- ajustes de formato
- tratamento necessário para o algoritmo
- organização do dataset para treino/teste

![Preparação dos dados para XGBoost](/Clarify/images/pre_training/pre-03.png)

---

## 04 - Upload dos dados (treino/teste) no S3 e recuperação local

Nesta etapa, faremos o upload dos dados separados entre **treino** e **teste** para o S3.

Também iremos recuperá-los para utilização local no laboratório/notebook.

![Upload e recuperação dos dados](/Clarify/images/pre_training/04.png)

---

## 05 - Configurando a análise de viés (Bias Analysis)

Agora vamos configurar a nossa análise de viés propriamente dita.

Nesta etapa, definimos:

- local de saída da análise
- configurações da análise
- configurações dos dados
- e, principalmente, o **atributo sensível (facet)** que queremos analisar

No nosso caso, o atributo sensível será:

- `gender`

> Importante: sempre debugue e entenda o que o código faz, e não apenas execute a célula.

![Configuração da análise de viés](/Clarify/images/pre_training/pre-05.png)

---

## 06 - Executando a análise com o ClarifyProcessor

Agora vamos rodar a análise de fato, inicializando o **ClarifyProcessor** e passando as configurações criadas no passo anterior.

![Execução da análise com ClarifyProcessor](/Clarify/images/pre_training/pre-06.png)

A análise pode demorar um pouco.  
Após a execução, serão gerados arquivos na pasta:

- `pre-training-analysis/`

Incluindo os relatórios da análise (como PDF).

![Arquivos gerados da análise](/Clarify/images/pre_training/pre-7.png)

---

## 07 - Revisando os resultados da análise de viés

Depois que a análise for concluída, podemos revisar os resultados da execução (run) e da análise gerada.

Na saída da execução, podemos visualizar algumas métricas e informações relevantes.

![Saída da execução com métricas](/Clarify/images/pre_training/pre-08.png)

O relatório é gerado em múltiplos formatos.  
Vamos observar a versão em **PDF**:

![Relatório em PDF gerado](/Clarify/images/pre_training/pre-09.png)

---

## Analisando o relatório PDF gerado

Ao fazer o download do relatório, podemos ver que a análise foi gerada de forma completa, incluindo várias seções importantes.

### Analysis Configuration

Mostra a configuração utilizada na análise, como:

- dataset analisado
- atributo sensível (facet)
- parâmetros/configurações da execução

![Seção Analysis Configuration](/Clarify/images/pre_training/pre-10.png)

### Pre-training Bias Metrics

Apresenta as métricas de viés calculadas antes do treinamento, ajudando a avaliar possíveis desequilíbrios nos dados.

![Seção Pre-training Bias Metrics](/Clarify/images/pre_training/pre-11.png)

---

## Conclusão desta etapa

Com o **SageMaker Clarify**, conseguimos analisar o dataset antes do treinamento e gerar relatórios com métricas de viés, o que ajuda a tornar o processo de Machine Learning mais confiável, transparente e responsável.
