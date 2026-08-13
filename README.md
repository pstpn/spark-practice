# Spark practice

Five labs on Spark and the table formats around it, from a single value computed on a
cluster to a partitioned table loaded snapshot by snapshot.

| Lab | Topic |
|-----|-------|
| [lab_01](lab_01) | A Spark cluster in Docker, the same job in cluster and in client mode, Fibonacci by a UDF and by a window with `lag` |
| [lab_02](lab_02) | Online Retail at the RDD level, `map` and `reduceByKey` for top products, orders and spend per customer |
| [lab_03](lab_03) | Word count over a corpus, cleaned by a regex UDF and split with `explode` |
| [lab_04](lab_04) | NBA players, salaries and season statistics joined into cost per efficiency, written as parquet partitioned by year and ranked in a window |
| [lab_05](lab_05) | Apache Iceberg, a table partitioned by season and loaded year by year as snapshots |

Datasets are not kept in the repository, each lab reads its own `data` directory.

## Running

```bash
cd lab_02 && make run
```

The cluster of lab 1 comes up separately.

```bash
cd lab_01 && make up-cluster && make run-cluster
```
