# Spark practice

Five labs on Apache Spark and the table formats around it, moving from a single value
computed on a cluster to a partitioned table loaded snapshot by snapshot. Each lab is a
self-contained PySpark program with a Makefile, and the data each one reads sits in its
`data` directory.

## lab_01 — a cluster, two ways to use it

The starting point is a standing Spark cluster, a master and a worker brought up with
Docker Compose, and the same task run against it two ways. In cluster mode the driver
connects to `spark://` from the host; in client mode the job is handed to the master with
`spark-submit` from inside the container. The task itself computes the nth Fibonacci
number, first with a plain Python UDF and then, in `main_lag.py`, without one — the sequence
is built inside Spark with a window and `lag`, so the recurrence runs in the engine rather
than in Python.

## lab_02 — RDD map and reduce

The Online Retail dataset, read from csv with an explicit schema, worked entirely at the RDD
level. `map` and `reduceByKey` give the five best-selling products, the number of orders and
the total spent per customer, and the average invoice per customer, each written back out as
csv. This is the map-reduce core of Spark before any of the DataFrame convenience sits on
top of it.

## lab_03 — word count over a corpus

A text corpus split on blank lines, cleaned by a UDF that strips punctuation and short words
with a regular expression, then split and `explode`d into one row per word. A `reduceByKey`
over the result returns the ten most frequent words. The cleaning is the point: what counts
as a word is a decision made in the UDF, not a given.

## lab_04 — joins, windows and a partitioned write

Three NBA datasets, the players, their salaries from 1985 to 2018, and per-season
statistics. A per-season efficiency metric is computed, the three sources are joined on
player and year, and a cost-per-efficiency figure is derived from salary and that metric.
The result is written as parquet partitioned by year, and read back through a window that
ranks players within each season to surface the five best value signings of every year.

## lab_05 — Apache Iceberg

The same kind of player-salary data, this time loaded into an Iceberg table rather than a
pile of parquet files. A catalog is created over SQLite, a table is defined partitioned by
season, and the years are appended one at a time, each new snapshot carrying the previous
year forward and merging that season's updates by register value. The table is then queried
by partition for a single season and for the ten highest-paid players in it, which is where
Iceberg earns its place — the partitioning and the snapshot history are the table's own, not
something the reader has to reconstruct.
