# Iniciando os estudos de SageMaker Experiments com MLflow

Nesta seção, daremos início ao estudo de **SageMaker Experiments** utilizando o **MLflow**.

O objetivo é entender como registrar, organizar e acompanhar experimentos de Machine Learning, incluindo métricas, parâmetros, artefatos e execuções (runs).

---

## O que é SageMaker Experiments?

O **SageMaker Experiments** é um recurso do Amazon SageMaker usado para **organizar e rastrear experimentos de Machine Learning**.

Com ele, podemos acompanhar informações como:

- execuções de treinamento (runs)
- hiperparâmetros utilizados
- métricas de avaliação
- artefatos gerados
- versões e comparações entre testes

Isso facilita a análise de resultados e a reprodutibilidade dos experimentos.

---

## O que é MLflow?

O **MLflow** é uma plataforma open source para gerenciamento do ciclo de vida de Machine Learning, muito usada para:

- **tracking de experimentos**
- registro de parâmetros e métricas
- armazenamento de artefatos
- comparação entre execuções
- organização de modelos

No contexto deste estudo, usaremos o MLflow integrado ao SageMaker para registrar e visualizar nossos experimentos.

---

## O que é o MLflow Tracking Server?

O **MLflow Tracking Server** é o componente responsável por **receber e armazenar os registros dos experimentos**.

Ele centraliza informações como:

- parâmetros (ex.: hiperparâmetros)
- métricas (ex.: loss, accuracy, RMSE)
- artefatos (modelos, gráficos, arquivos)
- metadados das execuções

Em outras palavras, ele funciona como o “servidor de rastreamento” onde os detalhes dos experimentos ficam registrados para consulta e comparação.

---

## Configurando o Tracking Server (MLflow)

Primeiro, iremos configurar nosso **Tracking Server** para registrar os detalhes dos nossos experiments.

---

## 01 - Criar o app de MLflow no SageMaker Studio

No **SageMaker Studio**, clicamos no botão **"Create MLflow app"**.

Em seguida, preenchemos as informações principais, como:

- nome do app
- IAM Role
- local de armazenamento dos artefatos (Artifacts)

> Observação: você pode criar um bucket S3 específico para armazenar os artefatos do MLflow.

![Criação do app MLflow no SageMaker Studio](/Experiments%20MLFlow/images/server/server-00.png)

---

## 02 - Aguardar a criação e abrir o MLflow

Após iniciar a criação do app MLflow, o processo pode levar alguns minutos.

Quando estiver pronto, clicamos em **"Open MLflow"** e seremos redirecionados para a interface do MLflow.

![MLflow app criado no SageMaker Studio](/Experiments%20MLFlow/images/server/server-01.png)
![Tela inicial do MLflow](/Experiments%20MLFlow/images/server/server-02.png)

---

## Conclusão desta etapa

Com o **MLflow Tracking Server** configurado e acessível no SageMaker Studio, estamos prontos para começar a registrar e acompanhar nossos experimentos de Machine Learning.
