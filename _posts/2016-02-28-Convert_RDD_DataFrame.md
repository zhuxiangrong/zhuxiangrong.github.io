---
layout: post
tags: spark scala
date: 2016-02-28 15:13
title: Spark中将RDD转化为DataFrame的方法
published: true
---

把RDD转化为DataFrame的两种方法:
####方法1:通过case class 或者 Tuple
scala中的Tuple绝对是个奇葩的存在，用惯了python的tuple，会感觉很别扭。Scala中的Tuple其实是case class类型的，理解了这个，也就理解了Tuple的不太“寻常”的用法。言归正传，把一个RDD变成一个DataFrame，其中一个比较简单的方法就是把RDD变成RDD[Tuple]。

```
val myArray = Array((1,1,1),(2,2,2),(3,3,3))
val myRDD = sc.parallelize(myArray)
val myDF = myRDD.toDF("a","b","c")
```

另外一个方法是定义case class:

```
case class mycc (a:Int,b:Int,c:Int)
val myArray = Array(Array(1,1,1),Array(2,2,2),Array(3,3,3))
val myRDD = sc.parallelize(myArray)
val myRDD2 = myRDD.map(row => mycc(row(0),row(1),row(2)))
val mydf = myRDD2.toDF()
```

####方法2：通过SQLContext CreateDataFrame
用case class一个麻烦的地方在于得去定义个case class，如果列比较多的时候还是麻烦的。而且Tuple和case class的一个共同的局限性在于它们能容纳的列最多22个。当列比较多时，只能用CreateDataFrame方法，这个方法比较麻烦是需要自定义DataFrame的schema，但是一个优势是可以运行时动态创建这个shcema.

```
import org.apache.spark.SparkContext
import org.apache.spark.sql.types.{StructField, StructType}
import org.apache.spark.sql.{SQLContext, Row, DataFrame}
import org.apache.spark.sql.types._
import org.apache.spark.sql.functions._

val myCols = Array("a","b","c") //列名字
val sqlCont = new SQLContext(sc)
val myArray = Array(Array(1,1,1),Array(2,2,2),Array(3,3,3))
val myRDD = sc.parallelize(myArray)
val myRDD2 = myRDD.map(row => Row(row(0),row(1),row(2))) //生成RDD[Row]
val schema = StructType(for(colName <- myCols) yield StructField(colName,IntegerType,true))
val myDF = sqlCont.createDataFrame(resultData,schema)
```

