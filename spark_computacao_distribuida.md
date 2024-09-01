## O que é o Apache Spark?

O Apache Spark é um mecanismo de análise unificado e extremamente rápido para processamento de big data e aprendizado de máquina, além de processamento de dados em larga escala em vários petabytes de dados em clusters de milhares de nós.

O Spark é uma plataforma de processamento de dados distribuídos. Isso significa que ele divide grandes conjuntos de dados em partes menores e as processa em paralelo, o que torna o processamento muito mais rápido.

O Spark foi desenvolvido desde o início para solucionar as deficiências do Hadoop. O Hadoop era lento e ineficiente para trabalhos de computação interativos e iterativos, além de ser muito complexo para aprender e desenvolver. 
Por outro lado, o Spark oferece APIs muito mais simples, rápidas e fáceis de desenvolver.

O Spark pode ser 100 vezes mais rápido que o Hadoop para processamento de dados em grande escala, explorando a computação na memória e outras otimizações.

Semelhante à maioria dos outros mecanismos de Big Data, o Spark é executado em uma plataforma de computação distribuída.

O Spark tem um mecanismo unificado para suportar cargas de trabalho variadas. Por exemplo, ele usa um único mecanismo para cargas de trabalho de streaming e em lote. Ele vem com bibliotecas de alto nível, incluindo
suporte para consultas SQL, dados de streaming, aprendizado de máquina e processamento de gráficos. Essas bibliotecas padrão aumentam a produtividade do desenvolvedor e podem ser perfeitamente combinadas para criar fluxos de trabalho complexos.

## Arquitetura do Spark

![Arquitetura Spark](images/arquitetura_spark.png 'Arquitetura Spark')

No centro da arquitetura do Spark está o Spark Core. Ele contém a funcionalidade básica do Spark. O Spark Core cuida do agendamento de tarefas, do gerenciamento de memória, da recuperação de falhas, da
comunicação com os sistemas de armazenamento, etc. Ele também abriga a principal API de abstração de programação do Spark, chamada RDD ou Resilient Distributed Datasets.

Os RDDs são uma coleção de itens distribuídos em vários nós de computação, no cluster, que podem ser processados em paralelo. O Spark Core fornece as APIs para criar e manipular essas coleções RDD.

O desenvolvimento inicial do Spark foi feito usando essas APIs, mas isso tinha suas desvantagens. Era difícil usá-lo para operações complexas e era difícil otimizá-lo para o Spark, cabendo principalmente
ao desenvolvedor escrever o código otimizado.

Para otimizar a carga de trabalho, o Spark introduziu o mecanismo SQL. Ele inclui o Catalyst Optimizer, que se encarrega de converter uma consulta computacional em um plano de execução altamente
eficiente, e o Tungsten Project, que é responsável pelo gerenciamento da memória e pela eficiência da CPU.

A abstração de nível mais alto, como o Spark SQL e as APIs Dataset e DataFrame, facilita o desenvolvimento de aplicativos e também se beneficia das otimizações do mecanismo SQL. Portanto, a abordagem 
recomendada para desenvolver aplicativos no Spark é usar essas APIs de nível superior em vez da API RDD.

As APIs Dataset e DataFrame podem ser invocadas a partir de qualquer uma das linguagens específicas do domínio, como Scala, Python, Java ou R. Além disso, temos o conjunto de bibliotecas, como
Spark Structured Streaming para streaming, ML Live para aprendizado de máquina e Graphics para processamento de gráficos.

Além disso, o Spark vem com seu gerenciador de recursos autônomo, mas você pode escolher outros gerenciadores de recursos, como YARN, Apache Mesos e Kubernetes.

Combinando tudo isso, o Spark fornece a plataforma unificada para realizar cargas de trabalho de processamento de gráficos, aprendizado de máquina, lotes e
streaming usando um único mecanismo de execução e um conjunto padrão de APIs.

## Detalhando passo-a-passo a arquitetura do Spark

![Arquitetura Spark](images/arquitetura_spark2.png 'Arquitetura Spark')

### Componentes Principais

**Driver Program / Spark Driver**
- **O que é:** O driver é o componente principal que coordena a aplicação Spark. Ele é responsável por criar o Spark Context (ou SparkSession), gerenciar a execução do job e interagir com o cluster manager.
- **Função:** Recebe o código da aplicação, cria o plano de execução e distribui as tarefas para os executors.

**Spark Context / SparkSession**
- **O que é:** O Spark Context é uma instância do driver que se conecta ao cluster e gerencia as operações Spark.
- **Função:** É a interface principal para o usuário interagir com o Spark. Ele cria RDDs, DataFrames, e define transformações e ações.

**Cluster Manager**
- **O que é:** O cluster manager é responsável por alocar recursos no cluster.
- **Função:** Pode ser o Spark Standalone, YARN (do Hadoop) ou Mesos. Ele distribui os recursos (como memória e CPUs) entre o driver e os executors.

**Worker Nodes**
- **O que é:** São os nós no cluster que executam as tarefas.
- **Função:** Cada worker node pode ter um ou mais executors, que são responsáveis por realizar o trabalho real.

**Executors**
- **O que é:** São processos que executam tarefas em paralelo em cada worker node.
- **Função:** Eles realizam o processamento dos dados e armazenam os resultados temporariamente na memória ou em disco.

**Tasks**
- **O que é:** São as unidades de trabalho que os executors realizam.
- **Função:** Cada tarefa é uma parte do job que precisa ser processada. As tarefas são distribuídas pelos executors pelo driver.

### Processo passo-a-passo

1. **Preparação do job:** Você escreve um job em Spark, por exemplo, um script que lê um arquivo, faz algumas transformações e escreve os resultados.

2. **Envio do Job:** Você envia o job para o Spark. O driver programa inicia o Spark Context e se conecta ao cluster manager.

3. **Alocação de Recursos:** O cluster manager aloca recursos para o job. Ele decide quantos worker nodes e executors serão usados para o processamento.

4. **Criação de Tarefas:** O driver divide o trabalho em tarefas pequenas. Por exemplo, se o job é para processar um grande arquivo, ele divide o arquivo em partes menores.

5. **Distribuição das Tarefas:** O driver envia essas tarefas para os executors nos worker nodes. Cada executor recebe uma parte das tarefas para processar.

6. **Execução das Tarefas:** Os executors processam as tarefas em paralelo. Eles leem os dados, aplicam as transformações (como filtros, agregações) e armazenam os resultados temporariamente na memória ou no disco.

7. **Coleta de Resultados:** Após a execução, os resultados parciais são enviados de volta ao driver. O driver pode combinar esses resultados e apresentar a saída final.

8. **Finalização:** O driver encerra o job e libera os recursos alocados pelo cluster manager.

### Resumo do fluxo de dados

**Job → Driver → Cluster Manager → Executors nos Worker Nodes**

**Driver → Cria Tarefas → Distribui para Executors**

**Executors → Executam Tarefas → Enviam Resultados ao Driver**

## E o Cluster?

Um cluster é um grupo de computadores (ou nós) interconectados que trabalham juntos para realizar tarefas como se fossem uma única unidade. Em um cluster, os computadores
colaboram para processar dados e executar aplicações, oferecendo maior capacidade de processamento e armazenamento do que um único computador poderia oferecer.

Podemos detalhar um cluster da seguinte forma:

1. **Nós do Cluster:**
   - **Computadores Individuais:** Um cluster é composto por múltiplos computadores, chamados de nós ou máquinas.

   - **Nó Master e Nó Worker:** Em muitos clusters, um nó é designado como o nó master (ou controlador), e os outros são nós workers (ou de trabalho). O nó master coordena o trabalho, enquanto os nós workers executam as tarefas reais.

2. **Interconexão:**
   - **Rede Interna:** Os nós do cluster estão conectados por uma rede interna rápida. Essa rede permite a comunicação eficiente entre os nós para coordenar tarefas e compartilhar dados.

   - **Comunicação:** No cluster, os nós trocam informações para garantir que as tarefas sejam realizadas corretamente e os dados sejam processados de forma eficiente.

3. **Recursos Compartilhados:**
   - **Processamento:** O cluster compartilha a carga de trabalho entre os nós, distribuindo as tarefas de processamento. Isso melhora a performance e permite que o cluster processe grandes volumes de dados rapidamente.

   - **Armazenamento:** Os dados podem ser distribuídos e armazenados em vários nós. Isso proporciona alta disponibilidade e recuperação de dados em caso de falhas.

4. **Gerenciamento e Coordenação:**
   - **Nó Master:** O nó master gerencia a distribuição de tarefas e o planejamento. Ele decide qual nó worker vai executar quais tarefas e garante que todos os nós estejam trabalhando juntos de maneira eficiente.

   - **Cluster Manager:** Um Cluster Manager pode ser usado para gerenciar recursos e a alocação de tarefas. Exemplos de cluster managers são o YARN (do Hadoop), Mesos e o Spark Standalone Cluster Manager.

5. **Execução de Tarefas:**
   - **Distribuição de Tarefas:** As tarefas são divididas em partes menores e distribuídas entre os nós workers. Cada nó worker executa sua parte da tarefa.

   - **Execução Paralela:** Os nós workers executam as tarefas em paralelo, o que acelera o processamento geral.

### Características de um Cluster

1. **Escalabilidade:**
   - **Horizontais e Verticais:** Você pode adicionar mais nós (escala horizontal) ou atualizar os recursos de cada nó (escala vertical) para aumentar a capacidade do cluster.
  
2. **Resiliência e Redundância:**
   - **Tolerância a Falhas:** Se um nó falhar, outros nós podem continuar o trabalho. O cluster pode redistribuir tarefas para manter a operação contínua.
   - **Backup e Recuperação:** Os dados podem ser replicados entre nós para garantir que não sejam perdidos em caso de falha.
  
3. **Alto Desempenho:**
   - **Processamento Paralelo:** A execução paralela de tarefas em múltiplos nós melhora o desempenho e a eficiência do processamento de dados.
  
4. **Gerenciamento Centralizado:**
   - **Controle e Monitoramento:** O cluster manager fornece uma visão centralizada do estado do cluster, permitindo o controle e o monitoramento dos recursos e das tarefas.
  
5. **Flexibilidade:**
   - **Diversidade de Aplicações:** O cluster pode ser configurado para diferentes tipos de aplicações, como processamento de dados, análise, e execução de aplicações de aprendizado de máquina.
