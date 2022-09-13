---
title: "Apache Spark Architecture"
date: 2022-09-11
menu:
  sidebar:
    name: "Apache Spark Architecture"
    identifier: apache-spark-architecture
    parent: apache-spark
    weight: 10
draft: true
---

- Unified Computing engine and set of libraries for parallel data processing on compute cluster.
- Supports multiple programming languages, Python, Scala, Java and R.
- **Unified**
  - Supports wide range of data analytics tasks on same compute engine
  - Provides consistent and composable API.
  - Makes it easy and efficient to develop applications
  - Enables high performance by optimizing across different libraries and functions.
- **Compute Engine**
  - Supports wide range of storage systems
  - Limits scope to compute engine and not the end storage
  - Data already resides in many systems and moving data to another system is an expensive operation. Spark supports loading data from multiple storage systems and perform computation.
- **Libraries**: Includes libraries for:
  - SQL and structured data (SparkSQL)
  - Machine learning (Mllib)
  - Streaming (Spark streaming and structured streaming)
  - Graph analytics (GraphX)

## Spark Architecture

---

- Single node does not have enough resources to process high volume of data.
- To process BigData you need a cluster of nodes that pool all the available resources.
- To perform computation on cluster of nodes, we require two components:
  - Framework to manage tasks across cluster, this is done by Apache Spark.
  - Framework to manage the nodes in the cluster also called a **Cluster Manager**. Apache spark supports following cluster manager:
    - Spark Standalone cluster
    - YARN
    - Kubernete
    - Mesos
- To perform compuation on cluster, we have to develop and submit a Spark application (using `spark-submit`)

#### Spark Application

- Application is a user program developed using Spark APIs.
- It consists of a [Driver Process](#spark-driver-process) and [executors](#spark-executors).
- Let's look at a simple Spark application, that finds Top 5 customers with highest order amount.

```python
import sys

from pyspark.sql import SparkSession
import pyspark.sql.functions as f

if len(sys.argv) != 2:
  print('Usage: sum_multiple.py <end_number>')
  sys.exit(1)

retail_file = sys.argv[1]

spark = SparkSession \
        .builder \
        .appName("Sum Multiples of 3, 5 or 7") \
        .getOrCreate()

retail_df = spark.read.format("csv") \
            .option("header", "true") \
            .option("inferSchema", "true") \
            .load(retail_file)

top_orders_df = retail_df \
                .withColumn("OrderAmount", f.expr("Quantityt * UnitPrice")) \
                .select("CustomerID", "OrderAmount") \
                .where("CustomerID is not null") \
                .groupBy("CustomerID") \
                .agg(f.sum("OrderAmount").alias("TotalOrderAmount")) \
                .orderBy("TotalOrderAmount", ascending = False) \
                .limit(5)

print(top_orders_df.collect())
```

**Submit Spark Application**

Submit spark application using below command:

```bash
spark-submit top_customers.py /databricks-datasets/online_retail/data-001/data.csv
```

Following happens when we submit application using spark-submit:

- requests cluster manager to allocate resources for the **[Spark Driver process](#spark-driver-process)**
- Cluster manager acknowledges the request and places driver process on a node in the cluster
- the local process exits and application starts running on the cluster

###### Spark Driver Process

Driver process runs the user program and performs following functions:

- Maintains the state of spark application
- Responds to user program or input
- Analyzes, Distributes and schedules tasks on the executors

Driver process initializes SparkSession which communicates with cluster manager to launch executor processes. Cluster manager launches executor processes and sends the information back to Spark Session.

###### Spark Executors

Spark Executors perform following functions:

- Run tasks assigned by the Driver process.
- Return the status to Driver process.

https://medium.com/datalex/sparks-logical-and-physical-plans-when-why-how-and-beyond-8cd1947b605a
