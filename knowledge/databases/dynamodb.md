# DynamoDB

## SQL vs NoSQL
Moore's Law no longer an enabler for RDBMS:
1. CPU performance is flattening as there is a physical limitation,
1. storage costs are dropping - the storage scaling is infinite.

["A Relational Model of Data for Large Shared Data Banks" - Edgar Frank "Ted" Codd](https://www.seas.upenn.edu/~zives/03f/cis550/codd.pdf)

SQL (optimized for storage):
1. normalized/relational,
1. ad-hoc queries,
1. scale vertically,
1. good for OLAP.

SQL - access pattern agnostic. Great choice when people want to ask questions about the data.

NoSQL (Optimized for compute):
1. denormalized/hierarchical,
1. instantiated views,
1. scale horizontally,
1. build for OLTP at scale.

NoSQL - you need to understand access patterns. It's a great choice when there is a limited number of supported queries (access patterns).

There is no big difference between NoSQL solutions.

## DynamoDB
DynamoDb offers elastic scaling (auto-scaling). On-demand capacity mode - no capacity planning, provisioning, or reservations - just API calls.
Global distributed active-active NoSQL technology. Global Tables with multi-Region replication - 5x9s.

## NoSQL Design Discovery

A NoSQL DB is a collection of objects (documents/items). They have attributes. Not all the items have the same attributes.
The table is the first index.

Objects are decorated with attributes. Those attributes are going to assign indexes to those attributes. The objects will be regrouped into new partitions on the indexes, rejoining them, into groups to support other access patterns.

**NoSQL doesn't need joins. It leverages indexes. NoSQL data modeling is about discovering access patterns and designing indexes.**

DynamoDB supports two types of **Primary Keys**:
1. **Simple Primary Key** (Partition key) - a key that uniquely defines an item.
2. **Composite Primary Key** (Partition key, Sort key) - a composite of two attributes.
    * **Partition key** - identifies a set of objects. Defines a grouping of objects within a table.
    * **Sort key** - defines unique objects within a partition.

With the composite key you can access an item only when providig both partition and sort keys. Moreover, to retrieve only a subset of items by particular partition key, you can provide a partition key value and a range of values for the sort key.

For querying data only by sort key, use the [Secondary Indexes](#secondary-indexes).

## Secondary Indexes
When decorating items with additional attributes you define **secondary indexes**. Essentially you're telling DynamoDB to replicate these things. When replicating the whole item you're doubling your storage. If a data access pattern doesn't need all attributes then don't replicate them.

A partition/shared key is used for building an unordered hash index. A table can then be partitioned for scale.

**Index overloading** - use generic keys once more to use indexes for multiple access patterns.

SQL - data normalization
NoSQL - no data normalization -> data duplication

SQL Joins are modeled with keys and attributes. The time complexity is constant - `O(log(N))`.

Many to Many relationships - flip PK and SK keys. Unidirectional relationship.

## Syncing Denormalized Data and Complex Queries

DynamoDB exposes streams to allow users to handle denormalized data synchronization. Updates need to be propagated.

Computing aggregation is maintained in real time. DynamoDB streams ordered events so that they can be passed to a lambda that computes the aggregation and writes the data back to the table. It's an eventual consistency aggregation framework.

By using streams and lambda, complex indexing can be then outsourced to services like OpenSearch.

When attaching lambdas to streams you should be aware that lambdas might slow down the streams.
**DynamoDB Streams + AWS Lambda = reliable "at least once" delivery.**

Streams scales with the table. Processing scales with the lambda processing capacity. Don't do heavy processing on the lambda stream. Use downstream lambdas or SQS instances.

DynamoDB is a complete data processing engine with guaranteed execution on the change stream.

## Access Patterns
User stories define access patterns. In NoSQL, you start with access patterns. In SQL otherwise.
GSI - Global Secondary Indexes 


## Materials
1. [AWS re:Invent 2021 - DynamoDB deep dive: Advanced design patterns](https://www.youtube.com/watch?v=xfxBhvGpoa0)
1. [AWS DynamoDB Index Overloading](https://jdwalker.github.io/aws/2020/02/29/dynamodboverloading)
1. [Core components of Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.CoreComponents.html)
1. [Using Global Secondary Indexes in DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GSI.html)