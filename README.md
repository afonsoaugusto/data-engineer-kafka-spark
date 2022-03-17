# Curso Data Engineering com Kafka e Spark

## Kafka

### dia 1

- visualização -> <https://softwaremill.com/kafka-visualisation/>

- sneak peak
- cap (distribuido) vs acid (relacional)

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

####

####

####

####

####

####

#### links, pesquisa:

- Database em memoria para armazenar estado -> <http://rocksdb.org/ >
- <https://sortbenchmark.org/>
