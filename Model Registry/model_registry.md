# SageMaker Model Registry — Registro e versionamento de modelos

Nesta seção, veremos o **SageMaker Model Registry**, onde podemos registrar nossos modelos, catalogar, controlar versões e compartilhar com membros da equipe.

Esse recurso ajuda a organizar o ciclo de vida dos modelos e a centralizar o gerenciamento das versões treinadas.

---

## O que é o SageMaker Model Registry?

O **SageMaker Model Registry** é um repositório central para **registrar e gerenciar modelos de Machine Learning**.

Com ele, podemos:

- registrar versões de modelos
- organizar modelos por grupos
- controlar status de aprovação
- acompanhar métricas e metadados
- rastrear linhagem (lineage) do modelo
- integrar com pipelines para registro automático

---

## Conceitos importantes do Model Registry

### 1. Model Package
Um **Model Package** representa uma versão registrada de um modelo.

Ele pode incluir informações como:

- localização do modelo (ex.: artifact no S3)
- imagem/container de inferência
- métricas do modelo
- metadados
- status de aprovação

Em outras palavras, cada versão registrada no Registry é tratada como um **pacote de modelo**.

---

### 2. Model Group (Model Package Group)
Um **Model Group** (ou **Model Package Group**) é um agrupamento de versões do mesmo modelo.

Exemplo:
- versão 1
- versão 2
- versão 3

Isso permite controlar a evolução do modelo ao longo do tempo, mantendo histórico e organização.

---

### 3. Approval Status (Model Approval Status)
O **Model Approval Status** é o status de aprovação de uma versão do modelo.

Estados comuns incluem:

- `PendingManualApproval`
- `Approved`
- `Rejected`

Esse status ajuda no fluxo de governança, por exemplo:

- somente modelos aprovados podem ir para produção
- modelos pendentes aguardam validação
- modelos rejeitados ficam registrados, mas não são promovidos

---

### 4. Model Lineage (Linhagem do modelo)
A **Model Lineage** mostra o histórico e os relacionamentos do modelo, como:

- dataset usado
- job de treinamento
- artefatos gerados
- versões registradas
- etapas anteriores do pipeline

Isso é importante para rastreabilidade e auditoria.

---

### 5. Model Metrics (Métricas do modelo)
Podemos associar métricas ao modelo registrado, por exemplo:

- accuracy
- RMSE
- AUC
- precision / recall

Essas métricas ajudam a comparar versões e decidir qual modelo deve ser aprovado/promovido.

---

### 6. Integração com pipelines (registro automático)
O Model Registry pode ser integrado com **pipelines de ML** (ex.: SageMaker Pipelines), permitindo:

- registrar modelos automaticamente após treino/avaliação
- aplicar regras de aprovação
- automatizar promoção entre ambientes

Isso melhora o fluxo de MLOps e reduz trabalho manual.

---

## 01 - Criando um grupo de modelos no SageMaker Studio

Podemos ver a listagem e criar nossos grupos de modelos em:

- **Models -> aba My models** no SageMaker Studio

Vamos criar um grupo clicando no botão:

- **"Register model group"**

![](/Model%20Registry/images/registry-00.png)

---

## Tela de criação do grupo

Na página de criação do grupo, podemos informar o **nome do grupo** e selecionar modelos para controlar as versões atuais e futuras.

Podemos registrar modelos a partir de diferentes origens, como:

- **S3** (artifact/modelo salvo)
- **Jobs** (modelos já treinados)
- **JumpStart Catalog** (modelos disponíveis no catálogo)

![](/Model%20Registry/images/registry-01.png)

![](/Model%20Registry/images/registry-02.png)

Também podemos associar, de forma opcional:

- **Model Approval Status**
- **metadados customizados**

Depois disso, clicamos em **"Register"** para seguir.

---

## 02 - Visualizando a versão registrada (Version 1)

Após o registro, podemos ter uma visão geral da **Version 1** do modelo, incluindo:

- visão geral (overview)
- atividade
- linhagem (lineage)
- detalhes da versão

![](/Model%20Registry/images/registry-03.png)

---

## Alterando o status de aprovação

Também podemos mudar o **status de aprovação** da versão do modelo.

Isso é útil para governança e promoção de modelos no fluxo de MLOps.

![](/Model%20Registry/images/registry-04.png)

![](/Model%20Registry/images/registry-05.png)

---

## Conclusão desta etapa

Com o **SageMaker Model Registry**, temos um repositório centralizado para armazenar e gerenciar nossos modelos existentes, com versionamento, status de aprovação, métricas e rastreabilidade.
