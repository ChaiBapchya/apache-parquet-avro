# apache-parquet-avro
Experiment with Apache Parquet and Apache Avro

## Using Spark2.1.0 binary prebuilt with Hadoop 2.7

## Learnt how to use 3rd party / external SPARK packages like spark-csv, spark-avro (parquet inbuilt)

# Steps 
1. Call pyspark with Avro package  

`[root@chai chai]# pyspark --packages com.databricks:spark-avro_2.11:3.2.0`

Without Avro package it throws error -
```
pyspark.sql.utils.AnalysisException: u'Failed to find data source: com.databricks.spark.avro. Please find an Avro package at http://spark.apache.org/third-party-projects.html;'
```

Download package. Unzip it. Put it in the directory and rename folder spark-avro_2.11:3.2.0

2. Reading Avro file  

`>>> df1 = sqlContext.read.load("/usr/local/spark/examples/src/main/resources/users.avro","com.databricks.spark.avro")`


3. Reading Parquet file

`>>> df = sqlContext.read.load("/usr/local/spark/examples/src/main/resources/users.parquet")`


4. Storing as Parquet file
```
>>> df.select("name", "favorite_color").write.save("namesAndFavColors.parquet")
```


5. Storing as Avro file
```
>>> df.select("name", "favorite_color").write.save("namesAndFavColors.avro","com.databricks.spark.avro")
```


# Resolved Error

```
 java.sql.SQLException: Unable to open a test connection to the given database. 
 JDBC url = jdbc:derby:;databaseName=metastore_db;create=true, username = APP. 
 Terminating connection pool (set lazyInit to true if you expect to start your database after your app). 

 Original Exception: 
 ------
java.sql.SQLException: 
Failed to start database 'metastore_db' with class loader org.apache.spark.sql.hive.client.IsolatedClientLoader$$anon$1@223bc09d, 
see the next exception for details. ```
```

Same exception consisted of following exception as -

`Caused by: ERROR XSDB6: Another instance of Derby may have already booted the database /usr/local/spark/metastore_db.`

All these 3 exceptions went thanks to 

http://stackoverflow.com/questions/34465516/caused-by-error-xsdb6-another-instance-of-derby-may-have-already-booted-the-da

**_Solution -_**
```
$ ps -ef | grep spark-shell

$ kill -9 Spark-Shell-processID
```

## Doubt -
```
[root@chai chai]# ps -ef | grep spark-shell
root     18382 18362  1 20:10 pts/1    00:00:48 /usr/lib/jvm/jdk1.8.0_121/bin/java -cp /usr/local/spark/conf/:/usr/local/spark/jars/* -Xmx1g org.apache.spark.deploy.SparkSubmit --name PySparkShell --packages com.databricks:spark-avro_2.11:3.2.0 pyspark-shell
root     20312 16067  0 21:08 pts/1    00:00:00 grep --color=auto spark-shell
```

Process 1 killed

`[root@chai chai]#kill -9 18382`

Process 20312 unable to be killed stating not a process why?

```
[root@chai chai]# kill -9 20312
bash: kill: (20312) - No such process
```


# Unresolved 

## 1. Warning -

`WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable`

**_Possible Solution - _**
+ http://stackoverflow.com/questions/23572724/why-does-bin-spark-shell-give-warn-nativecodeloader-unable-to-load-native-had
+ https://discuss.pivotal.io/hc/en-us/articles/219403388-How-to-eliminate-error-message-WARN-util-NativeCodeLoader-Unable-to-load-native-hadoop-library-for-your-platform-with-gphdfs 

## 2. Warning -

`WARN ObjectStore: Failed to get database global_temp, returning NoSuchObjectException`


## 3. Error
```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

**_Possible Solutions - _**
1. https://www.slf4j.org/codes.html#StaticLoggerBinder 
2. https://teamtreehouse.com/community/i-got-an-error-slf4j-failed-to-load-class-orgslf4jimplstaticloggerbinder 
3. http://sparkjava.com/documentation.html#examples




# Sources - 

1. To read write Parquet files - http://www.kdnuggets.com/2015/10/ibm-seti-spark-sql-parquets.html

2. Spark Packages - https://spark-packages.org/

3. PySpark tutorial - https://github.com/mahmoudparsian/pyspark-tutorial/tree/master/howto 

4. Apache Parquet Documentation - https://parquet.apache.org/documentation/latest/

5. SQL Programming - https://spark.apache.org/docs/1.6.1/sql-programming-guide.html

6. Parquet + S3 - https://www.appsflyer.com/blog/the-bleeding-edge-spark-parquet-and-s3/ 

7. https://www.svds.com/how-to-choose-a-data-format/ 

8. https://groups.google.com/forum/#!topic/parquet-dev/lfWanFOc040

9. http://apache-spark-user-list.1001560.n3.nabble.com/Write-to-Parquet-File-in-Python-td22186.html 

10. Parquet for Spark SQL - https://developer.ibm.com/hadoop/2015/12/03/parquet-for-spark-sql/ 

11. Parquet - http://www.kdnuggets.com/?s=parquet

12. Spark SQL Parquet - http://www.kdnuggets.com/2015/10/ibm-seti-spark-sql-parquets.html

13. SparkSQL Real-time analytics - http://www.kdnuggets.com/2015/09/spark-sql-real-time-analytics.html

14. http://spark.apache.org/docs/2.0.0/api/python/pyspark.sql.html

15. Spark Dataframe Operations - https://www.analyticsvidhya.com/blog/2016/10/spark-dataframe-and-operations/ 

16. Intro to RDDs - https://www.analyticsvidhya.com/blog/2016/09/comprehensive-introduction-to-apache-spark-rdds-dataframes-using-pyspark/ 

17. PySpark TUtorial - https://www.dezyre.com/apache-spark-tutorial/pyspark-tutorial 
