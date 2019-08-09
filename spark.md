# Apache Spark

Apache Spark is a distributed streaming service.


## Vocabulary and concepts

Mapper: map a row into another by transforming it. It is not very costly
Shuffle: Match multiple rows (reduce, join...). It is a very costly operation.


Flume Java: run the same code on my local machine for debug than the distributed code on the cluster. It is also possible to run on my local machine from pulling data on the distant cluster

DAG: Direct Acyclic Graph

- Transform:
- Shuffle:
- Action:
- <Spark 2.0: RDD: Resilient Distributed Dataset,
- >=Spark 2.0: Dataset: One important aspect of a dataset or a RDD is that they are imutable. This helps with caching and simplify error catching during the process.

RDD:
- Logical model across distributed Storage
- Resilient and immutable
- Compile-time type safe
- structured or unstructured
- Lazy (not materialized before doing an action on them)

RDD are a low level API and does not helps spark optimize.

Dataframe and Dataset are a higher level API


Spark handles dynamic allocation if the app needs more power.

Skew: happens if one job happens on one core and the job is not well spitted. An example using join, if many rows have a key to `null` then all that rows will be processed by the same thread and will become the bottleneck. There is always to fix skew.
Skew can be fixed by using a salt on the key and then when spiting the job on a mod-hash of the key.

Nested Structures: used to make joins less costly.

To use the MapReduce paradigm in Spark can be used using the `flatMap()` function.

Sources:

* https://databricks.com/blog/2016/01/25/deep-learning-with-apache-spark-and-tensorflow.html
* https://www.youtube.com/watch?v=x8xXXqvhZq8
* https://www.youtube.com/watch?v=Ofk7G3GD9jk
* https://spark.apache.org/docs/latest/running-on-kubernetes.html
