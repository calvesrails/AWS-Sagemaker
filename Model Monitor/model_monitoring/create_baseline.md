# Monitoramento de modelo em endpoint com SageMaker Model Monitor (Baseline)

Nesta seção, queremos dar uma olhada prática na **configuração de um monitor em um endpoint já existente**.

A ideia é monitorar um modelo em produção para garantir que o desempenho e os dados de entrada estejam de acordo com o esperado.

---

## Por que monitorar um modelo em produção?

Depois que um modelo é implantado, o trabalho não termina. Em produção, os dados que chegam para inferência podem mudar com o tempo.

Essas mudanças podem causar problemas como:

- degradação da qualidade das previsões
- entrada de dados com formato diferente do treino
- valores fora do intervalo esperado
- colunas faltando ou colunas extras
- tipos de dados diferentes do esperado

Por isso, o monitoramento é importante para detectar desvios e problemas antes que impactem o sistema de forma silenciosa.

---

## O que é o SageMaker Model Monitor?

O **SageMaker Model Monitor** é um recurso do SageMaker que ajuda a monitorar endpoints em produção.

Ele permite:

- capturar dados de inferência (inputs/outputs)
- comparar dados atuais com uma referência (baseline)
- detectar violações de restrições
- gerar relatórios de monitoramento
- executar análises de forma contínua ou por agendamento (cronograma)

Em resumo, ele ajuda a verificar se os dados que chegam ao endpoint continuam parecidos com os dados usados no treinamento.

---

## O que é o baseline neste contexto?

Neste fluxo, começaremos com um modelo já implantado em um endpoint e criaremos um monitor para ele.

Para o monitor funcionar corretamente, precisamos criar um **baseline** usando os dados de treinamento.

Esse baseline gera, basicamente, **dois arquivos JSON**:

1. **Estatísticas (statistics)**  
   Resume características dos dados de treino (ex.: distribuições, contagens, tipos, intervalos, etc.).

2. **Restrições (constraints)**  
   Define regras esperadas com base nos dados de treino, que serão usadas para verificar se os dados de inferência estão chegando corretamente.

### Como isso ajuda na prática?

As restrições ajudam a detectar problemas nos dados de inferência, por exemplo:

- número de colunas diferente do treinamento
- tipo de dado diferente do esperado
- valores fora do intervalo observado no treino
- problemas de integridade/consistência

Essas verificações podem ser executadas:

- manualmente
- continuamente
- com **monitoring schedules** (agendamento)

Ao final, podemos analisar o relatório gerado.

---

## 01 - Preparando os dados no notebook

Vamos começar preparando os dados utilizando o notebook:

- `/utils/sagemaker_model_monitoring.ipynb`

Nesse exemplo, teremos **oito features** diferentes. Isso será útil mais adiante para observar possíveis diferenças na quantidade de features que chegam na inferência.

Podemos rodar a primeira célula com nosso bucket já configurado.

![](/Model%20Monitor/images/baseline/monitor-00.png)

![](/Model%20Monitor/images/baseline/monitor-01.png)

---

## 02 - Criando o job de treino (para gerar o artifact do modelo)

Também podemos criar nosso job de treinamento para depois fazer o deploy usando o **artifact** gerado.

![](/Model%20Monitor/images/baseline/monitor-02.png)

![](/Model%20Monitor/images/baseline/monitor-03.png)

---

## 03 - Deploy em endpoint real-time com captura de dados

Agora vamos fazer o deploy em um **real-time endpoint** utilizando o artifact gerado no treinamento.

![](/Model%20Monitor/images/baseline/monitor-04.png)

![](/Model%20Monitor/images/baseline/monitor-05.png)

![](/Model%20Monitor/images/baseline/monitor-06.png)

### Data Capture (captura de dados)

Na variável `data_capture_config`, estamos adicionando a configuração de **captura de dados** do endpoint.

Isso é importante porque o endpoint precisa capturar os dados de inferência para que o **Model Monitor** possa usá-los na análise depois.

Em seguida, fazemos o deploy.

![](/Model%20Monitor/images/baseline/monitor-07.png)

Também podemos rodar a célula seguinte para verificar se a inferência está funcionando corretamente.

![](/Model%20Monitor/images/baseline/monitor-08.png)

---

## 04 - Configurando o baseline com os dados de treino

Agora que o endpoint está funcionando, podemos configurar o **baseline** usando os dados de treinamento.

Nesta etapa, criamos alguns paths e informamos as localizações no bucket S3 onde os arquivos serão armazenados.

### O que o baseline faz aqui?

Nesse contexto, o baseline serve como uma **referência dos dados de treino**.  
Ele é usado para gerar:

- estatísticas dos dados
- restrições esperadas

Depois, essas informações serão comparadas com os dados que chegam na inferência para identificar possíveis desvios.

Em seguida, criamos o objeto monitor.

![](/Model%20Monitor/images/baseline/monitor-09.png)

![](/Model%20Monitor/images/baseline/monitor-10.png)

---

## 05 - Gerando e explorando estatísticas e restrições

Com a etapa anterior concluída, podemos rodar a próxima célula para gerar e explorar as **estatísticas** e **restrições** do baseline.

Esses resultados serão enviados como arquivos JSON para o bucket S3.

![](/Model%20Monitor/images/baseline/monitor-11.png)

![](/Model%20Monitor/images/baseline/monitor-12.png)

Também podemos visualizar algumas restrições geradas (como validações de integridade) executando as células seguintes.

![](/Model%20Monitor/images/baseline/monitor-13.png)

---



Deixaremos a parte de **Analyze data with Monitoring Schedules** para a próxima seção.
