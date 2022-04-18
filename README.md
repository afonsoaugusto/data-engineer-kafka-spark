# Curso Data Engineering com Kafka e Spark

## Kafka

### dia 1

- visualização -> <https://softwaremill.com/kafka-visualisation/>

- sneak peak
- cap (distribuido) vs acid (relacional)
- treinamentos gratuitos <https://www.confluent.io/blog/confluent-developer-launches-free-apache-kafka-courses-and-tutorials-online/>

#### arquiteturas de dados

- lambda
  - batch   -> passado e presente
  - stream  
    -> janela de tempo
    -> structure stream (pesquisar palestra)
        -> <https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html>
- kappa
  - enterprise data hub (origem -> kafka -> consumers)
  - tierização do dado
    - hot storage
    - cold storage
    -> exemplo o pulsar
    -> kafka com sync (s3)
- mesh

#### Divisão do cluster de kafka de tipos

- configuração acks
- configuração de callback

#### links

- <https://www.confluent.io/blog/how-kafka-is-used-by-netflix/>
- <https://netflixtechblog.com/tagged/kafka>

#### TMM vs Message Broker

- Exactly once semantics

- Queue -> Efemero
- Topico -> Efemero ou persistente

- RabbitMQ
  - message broker
  -> streams no rabbitMQ
    -> processamento de dados no rabbitMQ

- Apache Pulsar
  -> plataforma de streaming

- Apache Kafka
  -> plataforma de streaming
  -> APIs

#### Data Ingestion

#### Data Processing

- Ferramentas gerenciadas não garantem que o dado não seja duplicado no consumo

#### Kafka Fundamentals

- Java, Scala
- centralização de dados

#### Kafka log structure

- zero copy operations
- escrita sequencial linear

- <https://cwiki.apache.org/confluence/display/KAFKA/KIP-500%3A+Replace+ZooKeeper+with+a+Self-Managed+Metadata+Quorum>

#### Arquitetura do kafka

- Pull Architecture

### dia 2 - ingestion

#### Kafka trabalha melhor com Avro

-> Ele performa melhor com dados Avro.
-> Confluent schema registry

#### Producer

-> librdKafka -> <https://github.com/edenhill/librdkafka>


#### Message Durability

- Fire and Forget -> lança e solta
- Synchronous Send -> espera o retorno do kafka
- Asynchronous Send -> Fica perguntando se tem resposta? (callback)

#### Acknowledgements = [Acks]

- Acks[0] = não espera a resposta do kafka (fire and forget)
- Acks[1] = padrão do kafka, pode ter data loss
- Acks[ALL] = espera a replicação acontecer

#### Idempotent Producers & EOS

- At [Most] Once
- At [Least] Once
- [Exactly] Once Semantics -> enable.idempotent
  - <https://www.confluent.io/blog/exactly-once-semantics-are-possible-heres-how-apache-kafka-does-it/>

#### Para não ter duplicado e ordenado

- max.in.flight.requests.per.connection=1
- enable.idempotence=true
- acks=ALL
- retries > 10000

#### batching & Compression

- batch.size
- linger.ms

#### Sticky Partitioner

#### links

- <https://www.confluent.io/blog/exactly-once-semantics-are-possible-heres-how-apache-kafka-does-it/>
- <https://towardsdatascience.com/10-configs-to-make-your-kafka-producer-more-resilient-ec6903c63e3f#:~:text=An%20ack%20is%20an%20acknowledgment,before%20considering%20a%20successful%20commit.>
- <https://github.com/owsplumbers/trn-spec-kafka>

###  dia 3 - processing

#### Faust

- <https://faust.readthedocs.io/en/latest/>

#### Evento

- Ordenado
- Imutável
- Replayable

#### Stream processing

- Time
  - Event Time
  - Log Append Time
  - Processing Time

- State
  (preferencia para estado em memoria na aplicação - rocksdb), consulta externa pode ser prejudicial
  - Local or Internal State
  - External State

- Stream Table Duality
  - Tables
  - Streams

#### Flink

- Garante Exactly-Once semantics <https://flink.apache.org/features/2018/03/01/end-to-end-exactly-once-apache-flink.html>

- Boundend -> janela de tempo determinado - inicio e fim definidos
- Unbounded -> janela de tempo indefinido - do inicio e até o fim

#### Kafka Streams

- <https://kafka.apache.org/31/documentation/streams/>
- Base para o KsqlDB -> KsqlDB é um wrapper para o Kafka Streams
- Biblioteca de processamento

- KTable
- KStream
  - Stream (changelog) -> como se fosse um topico com log compation

#### KsqlDB

- <https://docs.ksqldb.io/en/latest/>

#### Spark

- Engine de computação em memoria

- RDD, Spark Streaming -> Deprecated, api de baixo nivel

- Spark SQL, DataSets &  DataFrames -> alto nivel 

- spark.read -> sempre earliest

- leitura
  - earliest -> do primeiro até o fim
  - latest -> do ultimo lido até o fim (se é a primeira vez que roda, ele vai começar do inicio)

#### faust

- Exactle once semantics

#### links, pesquisa:

- Database em memoria para armazenar estado -> <http://rocksdb.org/ >
- <https://sortbenchmark.org/>


### dia 4 - sinks serving

#### Consumer group

- todo consumidor pertence a um consumer group

#### 3 casos que rabalancer acontece

O rebalancer para o consumer group para reajustar

- adicionar um novo consumidor no consumer group
- o hartbeat do consumer group falhou (desligou) ele remove o consumidor
- modificação no topico = novas partições - para todos os consumers groups

#### commits e offsets

- <https://docs.confluent.io/platform/current/clients/consumer.html#offset-management>

- o padrão do kafka te permite ter data loss ou dados duplicados.

#### kafka connect

- Movimento de dados, In e Out

#### morden data wherehouse

- mpp -> massive parallel processing

#### Real Time Ingest

- EOS
- Kafka as consumer

####

#### links

- biblioteca python marshmallow para abstração de schema
  - <https://marshmallow.readthedocs.io/en/stable/>  - marshmallow: simplified object serialization¶
- <https://engineering.linkedin.com/blog/2021/text-analytics-on-linkedin-talent-insights-using-apache-pinot>

### dia 5 - best-pratices

#### Estratégias de desenvolvimento

- Message Ordering in Apache Kafka

Producer Configs
- Exactly-Once Enabled
- max.inflight.connectors <=5
- acks=all
- partition=1

#### Transactions - multi-operation-commits

- Como rollback acontece?
- <https://stackoverflow.com/questions/56156749/how-does-kafka-know-whether-to-roll-forward-or-roll-back-a-transaction>
- <https://docs.google.com/document/d/11Jqy_GjUGtdXJK94XGsEIK7CP1SnQGdp2eF0wSw9ra8/edit#bookmark=kix.uu5bwrue4nmm>

```
eu perdi aqui
como faz o rollback? ele insere outro evento apagando? ou ele remove o evento?

19:37
Respondido privadamente
nesse caso ele nao inseri a operação e atomica ou ela acontece como um todo ou simples nao acontece
```

#### Stream Table Duality

- kafka topic log compaction
- <https://towardsdatascience.com/log-compacted-topics-in-apache-kafka-b1aa1e4665a7>

#### Windowing

- Session
- Hopping
- Tumbling

- <https://kafka.apache.org/20/documentation/streams/developer-guide/dsl-api.html#windowing>

#### Log

- INFO é importante

#### Storage Resizing

- Adicionar mais discos - JBOD

#### Partition Reassign

- Reorganização das partições com a adição de mais discos.
- operação offline

#### Cruise Control - administração do cluster

- <https://github.com/linkedin/cruise-control>
- <https://engineering.linkedin.com/blog/2019/02/introducing-kafka-cruise-control-frontend>

#### kafka mirrormaker

- conecta dois clusters
- <https://docs.confluent.io/4.0.0/multi-dc/mirrormaker.html>

#### https://lenses.io/

- Mapa da topologia

#### Tipos de clusters

- Otimizado para Throughput
- Otimizado para Latency
- Otimizado para Durability
- Otimizado para Availability

#### Security

- communication - SSL
- Authentication - SCRAM
- Authorization - ACL

####

####

####

####

#### Links

- Cursos do Stéphane Maarek kafka
- https://mateus-oliveira.medium.com/kafka-no-k8s-strimzi-zero-to-hero-round-1-a48e16f887ba
- <https://www.confluent.io/blog/transactions-apache-kafka/>
- <https://developer.confluent.io/learn/kafka-transactions-and-guarantees/>
- <https://strimzi.io/documentation/>

## Spark

### day-1

Spark = engine de computação distribuida em memoria

#### Bibliotecas depreciadas:
- RDD
- Spark Streaming

#### Melhor metodo para trazer dados para o spark

- Data Lake
- Kafka

#### Spark trabalha com Partições

- As partições por padrão são configuradas em 128 MB
- Cada partição tem uma thread.

#### whole stage code generation

- <https://databricks-prod-cloudfront.cloud.databricks.com/public/4027ec902e239c93eaaa8714f173bcfc/6122906529858466/293651311471490/5382278320999420/latest.html>
- <https://databricks.com/session_na20/understanding-and-improving-code-generation>

#### Pandas

- pandas não escala em maquinas, ele paralelisa em threads (single node)

#### PySpark da 3.2 e depois

- Escrever pandas dentro do spark sem fricção.

#### Links

- <https://comparecloud.in/>

### day-2 - batch etl

#### Melhores praticas Data Lake

- Raw Ingestion (S3, pode ser tbm o kafka) -> Landing Zone (dado cru sem tratamento, json, xml)
- Transformation -> Processing Zone -> (parquet, avro, iceberg, delta, hudi)
- Curated Zone

#### Data Lakehouse

- Hudi, Iceberg e Delta

#### Delta Lake

- <https://databricks.com/wp-content/uploads/2020/12/cidr_lakehouse.pdf>
- <https://databricks.com/wp-content/uploads/2021/10/The-Data-Lakehouse.pdf>

#### Delta Sharing

- <https://databricks.com/blog/2021/05/26/introducing-delta-sharing-an-open-protocol-for-secure-data-sharing.html>

#### Databricks

- Criar um cluster

- Opensource do Databricks -> Apache Zeppelin <https://zeppelin.apache.org/>

- Protoloco mais rapido para acessar o data lake -> da microsoft abfss (azure data lake gen2)

#### Links

### day-3 Near Real Time

#### Event Stream

- Eventos são ordenados
- São imutaveis
- são reprocessaveis

#### Real Time Stream Processing

- Open Source OSS
  - Kafka
  - Spark
  - Apex
  - Flink
  - Storm
  - Beam
- Azure
  - HDInsight
  - Synapse Analitycs
  - Stream Analytics
  - Azure Functions
- GCP
- AWS

#### Message Delivery Guarantees [Idempotent Produces & EOS]

- At [Most] Once -> Pode ter perda
- At [Least] Once -> Pode ter perda e duplicacao
- Exactly [Once] -> Sem perda ou duplicacao + ordenacao do envio

#### Kafka Producer [Message Durrability]

Message Methods=comportamento do produtor em relação a resposta do broker
- Fire-and-Forget
- Syncrhonous Send
- Asyncrhonous Send

Acks=comportamento do broker em relação ao recebimento da mensagem
- No-ack [0]
- Acks [1]
- Acks [ALL]

#### The log Structure

- Headers

- Habilitar o SSL vc perde o zero copy
  - <https://cloudnweb.dev/2019/05/heres-what-makes-apache-kafka-so-fast-kafka-series-part-3/>

#### Modelos de stream

- Naive approach -> sempre tem que listar os arquivos

#### Links

- Tipos de erros em que o producer não faz retry
- simulado Stephane Maarek confluent kafka - <https://www.udemy.com/course/confluent-certified-developer-for-apache-kafka/>
- <https://www.oak-tree.tech/blog/kafka-admin-remove-messages>


### day-4 lakehouse

#### ETL vs ELT

- ETL = Extract Transform Load
- ELT = Extract Load Transform

#### MDW - Morden Data Warehouse

- MPP = Massive Parallel Processing

#### Data Lakehouse

- Remove a cópia da Gold para o DW

#### Data Virtualization

- Subir o dado para a memoria e processar
- Trino <https://trino.io/>
- <https://www.starburst.io/wp-content/uploads/2021/04/Trino-Oreilly-Guide.pdf>

#### Links

- Bill Inmon - pai do DW - https://ia800202.us.archive.org/9/items/2005BuildingTheDataWarehouse4thEditionWilliamH.Inmon/2005%20-%20Building%20The%20Data%20Warehouse%20%284th%20Edition%29%20%28William%20H.%20Inmon%29.pdf
- https://cloud.google.com/blog/products/data-analytics/introducing-bigquery-omni
- https://cloud.google.com/bigquery-omni/docs/azure/create-connection


### day-5 best-pratices

#### The spark lifecycle

- A bronze pode ter caracteristica de stage
  - Ela poderia ter um ttl de 7 dias por exemplo

- A silver é um fim

#### Spark plan execution

- Task: single thread

- Job -> Stage -> Task

#### Exchange

- Acontece quando:
  - Join
  - Repartition
  - Coalesce
  - Sort

#### Adaptive Query Execution

#### Links