# SageMaker Model Monitor — Agendando monitoramento com Monitoring Schedule

Na última seção, vimos que podemos criar uma **baseline** usando o **SageMaker Model Monitor**.  
Agora, queremos ver como **programar o monitoramento** para que ele analise continuamente os dados enviados ao endpoint de inferência e verifique se alguma **restrição foi violada**.

A ideia aqui é monitorar **qualidade e consistência dos dados de entrada**, e não apenas olhar o resultado final da previsão.

---

## Objetivo desta etapa

Nesta etapa, vamos:

- criar um **agendamento de monitoramento** (monitoring schedule)
- reutilizar o **monitor** e a **baseline** criados anteriormente
- enviar dados de inferência com uma inconsistência proposital
- verificar se o monitor detecta a violação
- analisar o relatório gerado
- parar/deletar o agendamento para evitar custos

---

## 01 - Criando o agendamento de monitoramento

Vamos criar o **monitoring schedule** utilizando o monitor que criamos anteriormente.

> Lembre-se de sempre debugar o código e consultar a documentação para entender cada linha de configuração.

Nesta etapa, basicamente:

- criamos o **schedule**
- passamos os arquivos de **estatísticas** e **restrições** da baseline
- informamos o **endpoint** criado anteriormente
- definimos a frequência de execução (ex.: a cada 1 hora)

!

---

## 02 - Gerando inferência com dado inconsistente (teste do monitor)

Agora vamos invocar nosso modelo com uma amostra propositalmente inconsistente.

Neste exemplo, enviaremos dados **sem a coluna `education`**.

Ou seja:

- a inferência ainda pode gerar resultado
- porém os dados de entrada não estão no mesmo padrão do treinamento

Esse é exatamente o tipo de problema que queremos que o monitor detecte.

Mesmo com previsão sendo retornada, a **qualidade dos dados pode estar ruim**, e queremos ser avisados quando isso acontecer.

!

---

## 03 - Acompanhando as execuções do schedule

Nas células seguintes, podemos verificar informações como:

- status do agendamento
- quantidade de execuções realizadas
- última execução do schedule

Como o agendamento foi configurado para rodar **a cada 1 hora**, é necessário aguardar esse intervalo para a primeira execução.

No seu caso, você comentou que já existem duas execuções.

Também é possível acompanhar essas execuções na aba:

- **SageMaker -> Processing Jobs**

!

---

## 04 - Verificando os resultados e relatório de violação

Podemos observar os resultados no bucket S3 e também fazer o download dos arquivos gerados.

!

!

Ao abrir o relatório de violação, veremos a inconsistência detectada:

- no dataset atual chegaram **8 colunas**
- mas nas restrições da baseline eram esperadas **9 colunas**

Exemplo de mensagem:

```text
current dataset: 8, Number of columns in baseline constraints: 9
