ElasticMQ
=========

ElasticMQ is a simple message queue system, written entirely in [Scala](http://scala-lang.org).

Currently messages are persisted in a database (by default an in-memory H2 instance)
using [Squeryl](http://squeryl.org/).

ElasticMQ implements a subset of the [SQS](http://aws.amazon.com/sqs/) REST interface,
providing an SQS alternative e.g. for testing purposes.

The REST server is implemented using [Netty](http://www.jboss.org/netty), a high-performance,
asynchronous, event-driven server Java framework.

The SQS interface has been tested using the [Amazon Java SDK](http://aws.amazon.com/sdkforjava/) library;
see the `rest-sqs-testing-amazon-java-sdk` module for the testsuite.

In the future... ElasticMQ may provide many more exciting features :).

Starting an ElasticMQ server
----------------------------

    // First we need to create a Node
    val node = NodeBuilder.withInMemoryStorage().build()
    // Then we can expose the native client using the SQS REST interface
    val server = SQSRestServerFactory.start(node.nativeClient, 8888, "http://localhost:8888")
    // ... use ...
    // Finally we need to stop the server and the node
    server.stop()
    node.shutdown()

Alternatively, you can use MySQL to store the datea:

    val node = NodeBuilder.withMySQLStorage("elasticmq", "root", "").build()

ElasticMQ dependencies in SBT
-----------------------------

    val elasticmqCore = "org.elasticmq" %% "core" % "0.1"
    val elasticmqSqs  = "org.elasticmq" %% "rest-sqs" % "0.1"

    val smlResolver = "SotwareMill Public Releases" at "http://tools.softwaremill.pl/nexus/content/repositories/snapshots/"

ElasticMQ dependencies in Maven
-------------------------------

Dependencies:

    <dependency>
        <groupId>org.elasticmq</groupId>
        <artifactId>core_2.9.1</artifactId>
        <version>0.1</version>
    </dependency>
    <dependency>
        <groupId>org.elasticmq</groupId>
        <artifactId>rest-sqs_2.9.1</artifactId>
        <version>0.1</version>
    </dependency>

And our repository:

    <repository>
        <id>SotwareMillPublicReleases</id>
        <name>SotwareMill Public Releases</name>
        <url>http://tools.softwaremill.pl/nexus/content/repositories/snapshots/</url>
    </repository>

Current versions
--------

*Stable*: 0.1

*Development*: 0.2-SNAPSHOT

DB Schema
---------

The MySQL Schema can be found in `core/src/main/resource/schema-mysql.sql` file. It should be easily adaptable to
other databases.