na ultima seção vimos que podemos criar uma baseline usando o sagemaker model monitor e agora queremos ver como podemos prgramar nosso model monitor de modo que ele esteja apenas pegando os dados que foram enviados ao nosso endpont para inferencia e analisando se alguma restrição foi violada


01 - vamos criar nosso agendamento de monitoramento ultilizando o monitor que criamos anteriormente

lembre-se de sempre debuggar o código e ler documentações para começarmos a entender cada linha de configuração, mas aqui basicamente criamos o schedule e passamos nossas statisticas e restições do nosso baseline e especificamos, é claro, o endpoint crado anteriormente.

00


02 - aqui invocaremos nosso modelo no qual geraremos aprenas algum tipo de dados de amostra, não passando a coluna "education", ou seja, essa amostra não faz parte da nossa inferência, e é exatamente o que queremos observar pelo monitor, pois mesmo sem essa coluna, os resultados podem ser gerados, porem talves não seja uma qualidade de dados tão boa , portanto não perceberiamos isso pelos resultados, e queremos que sejamos abvisados caso algum dado falte.

01

nas celulas seguintes podemos ver o status do nosso agendamento, a quantidade que foi rodada e o ultimo agendamento executado. lembrando que o nosso agendamente foi programado para a cada 1 hora, então é necessário esperar 1 hora para que a primeira execução seja realizada, no meu caso, já tenho duas execuções

você pode acompanhar as execuções na aba "processing jobs" no painel do SageMaker

02

03 - podemos observar os resultados no nosso buckt, mas tambem podemos baixalos 

04
03

e veremos nosso relátorio de violação, no qual temos no dataset atual 8 colunas, porem no nosso arquivo de violção esperamos 9

"current dataset: 8, Number of columns in baseline constraints: 9"

05

03 - e por fim, parea evitar custos, paramos e deletaremos nosso monitor schedule

06