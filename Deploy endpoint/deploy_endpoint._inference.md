# Deploy de endpoint no SageMaker (Real-Time) — Resumo

Nesta etapa, fazemos o **deploy do modelo treinado** para gerar previsões com **inferência em tempo real (real-time inference)**.

## Conceitos principais

- **Inferência**: uso de um modelo já treinado para gerar previsões com novos dados.
- **Inferência em real-time**: o modelo fica disponível em um endpoint ativo, respondendo previsões rapidamente após uma requisição.

---

## Fluxo da atividade

### 1. Preparação e treino do modelo
No JupyterLab, abrimos o notebook:

- `/script/sagemaker_model_deployment_examples.ipynb`

E utilizamos o dataset:

- `employee.csv`

Rodamos as células iniciais para preparar os dados e treinar o modelo com XGBoost (como já visto nas seções anteriores).

> Lembre-se de ajustar o nome do bucket no script.

![](/Deploy%20endpoint/images/real_time/real-00.png)

---

### 2. Deploy como endpoint real-time
Após o treino, realizamos o deploy do modelo como endpoint ativo para inferência em tempo real.

- `initial_instance_count`: quantidade inicial de instâncias
- `instance_type`: tipo de instância (impacta custo e desempenho)
- `endpoint_name`: nome do endpoint no SageMaker

![](/Deploy%20endpoint/images/real_time/real-01.png)
![](/Deploy%20endpoint/images/real_time/real-02.png)

---

### 3. Teste de previsões (invocação do endpoint)
Depois do deploy, testamos o endpoint realizando previsões com dados de teste (ex.: 20 registros).

O resultado é retornado rapidamente, demonstrando a inferência em tempo real.

![](/Deploy%20endpoint/images/real_time/real-03.png)

---

### 4. Exclusão do endpoint
Após o teste, podemos excluir o endpoint para evitar custo desnecessário.

![](/Deploy%20endpoint/images/real_time/real-04.png)

---

### 5. Deploy usando model artifact
Também é possível criar o endpoint usando o **model artifact** salvo no S3 (ex.: `model.tar.gz` gerado no treinamento).

#### O que é o model artifact?
O **model artifact** é o arquivo gerado ao final do treinamento que contém o modelo treinado e os arquivos necessários para que ele possa ser reutilizado em deploys e inferência.

Em geral, ele permite:

- reaproveitar o modelo sem precisar treinar novamente
- realizar novos deploys
- versionar modelos treinados

Para isso, adicionamos a **URL (S3 URI)** do artifact no script e repetimos o fluxo de deploy.

![](/Deploy%20endpoint/images/real_time/real-06.png)
![](/Deploy%20endpoint/images/real_time/real-05.png)

---

### 6. Nova invocação do endpoint
Após o deploy via artifact, realizamos novamente previsões e observamos que a resposta continua rápida (real-time inference).

![](/Deploy%20endpoint/images/real_time/real-07.png)

---


## Outras formas de inferência no SageMaker

Nesta parte vimos como:

- fazer o **deploy** do modelo
- verificar a criação do **endpoint**
- realizar **inferência**
- deletar o endpoint ao final

No mesmo script, também são apresentadas **outras formas de inferência**, cada uma com seu caso de uso específico.

Como parte do estudo, fica o convite para você **executar e debugar as outras células**, analisando o que muda no fluxo, no tempo de resposta, no custo e na forma de uso.

---

## Outras formas de inferência (visão geral)

### 1. Batch Inference (Batch Transform)

A **inferência em batch** é usada quando queremos processar **grandes volumes de dados de uma vez**, sem necessidade de resposta imediata.

### Quando usar?
- previsões em lote (ex.: milhares/milhões de registros)
- processamento agendado (diário, semanal, mensal)
- cenários em que baixa latência **não é necessária**

### Características
- não exige endpoint sempre ativo
- geralmente lê dados do S3 e grava resultados no S3
- costuma ser uma boa opção para reduzir custo em comparação com endpoint contínuo

### Exemplo de uso
- prever churn de toda a base de clientes uma vez por semana
- score de risco em massa
- classificação de grandes arquivos de dados

---

### 2. Serverless Inference

A **inferência serverless** permite fazer deploy sem precisar gerenciar instâncias fixas de endpoint.

O SageMaker provisiona recursos de forma automática conforme as requisições chegam.

### Quando usar?
- tráfego variável ou imprevisível
- aplicações com baixo/médio volume de chamadas
- cenários em que você quer simplificar operação e evitar endpoint sempre ligado

### Características
- não precisa manter instância dedicada o tempo todo
- escalonamento automático
- pode ser mais econômico para cargas esporádicas
- pode ter maior latência inicial em alguns cenários (ex.: primeira chamada após inatividade)

### Exemplo de uso
- APIs internas com uso ocasional
- protótipos e ambientes de teste
- aplicações com picos de acesso

---

### 3. Asynchronous Inference (Inferência Assíncrona)

A **inferência assíncrona** é usada quando a previsão pode demorar mais e não precisa ser retornada imediatamente na mesma conexão da requisição.

Em vez de esperar a resposta na hora, você envia a requisição e o resultado é processado em segundo plano, sendo disponibilizado depois (normalmente em S3).

### Quando usar?
- payloads grandes
- inferências que demoram mais tempo
- cenários em que o cliente não precisa de resposta imediata

### Características
- ideal para processamento mais pesado
- evita timeout em chamadas síncronas
- integração comum com S3 para entrada/saída
- permite desacoplar envio da requisição e leitura do resultado

### Exemplo de uso
- processamento de documentos grandes
- inferência em imagens/vídeos maiores
- tarefas de ML com tempo de processamento mais alto

---

## Resumo rápido: qual usar?

- **Real-time**: resposta imediata (baixa latência)
- **Batch**: grande volume em lote (sem urgência)
- **Serverless**: tráfego variável, sem gerenciar instância fixa
- **Assíncrona**: processamento demorado/payload grande, resposta posterior

---

## Sugestão de estudo prático

Ao executar as outras células do script, tente observar:

- qual recurso é criado no SageMaker
- se há endpoint ou não
- onde entra o dado (local/S3)
- onde sai o resultado (resposta imediata/S3)
- tempo de resposta
- custo e cenário ideal de uso

Isso ajuda muito a consolidar o entendimento do pipeline e a escolher o tipo de inferência correto para cada caso.
