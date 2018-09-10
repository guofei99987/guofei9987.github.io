---
layout: post
title: 【spark】DataFrame.
categories:
tags: 1A_数据平台
keywords:
description:
order: 154
---
DataFrame的生成见于另一篇文章  
DataFrame转RDD后也有一系列的使用技巧，见于另一篇文章  

这里介绍DataFrame的操作
## 基本操作
### 导入所需包
```py
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("appName").enableHiveSupport().getOrCreate()
```

### show
```py
# 注释部分用的少
# df.head(5) #类似RDD，返回一个list，里面多个Row
# df.take(5) #类似RDD，效果同head
# df.collect() #多个Row组成的list
# df.first() #返回一个Row
df.limit(5) #取前5行，不同的是，是transformation
df.show() #返回20条数据
df.show(30) #返回30条数据


df.describe() #返回统计值，是一个action，但返回的是DataFrame
df.describe('col1','col2') #返回指定字段的统计值

df.columns #返回一个list，内容是列名
df.dtypes


df.drop('col1','col2') # 删除某些列
df.withColumnRenamed('col1','col1_new') # 给指定列改名
```


## 查询

```py
# df.where('col1=1 and col2="abc"') #是transformation，返回RDD
df.filter('col1=1 and col2="abc"') #与where效果完全相同
df.filter(df.col1>5)

df.select('col1','col2') # 取指定列,返回RDD
df.selectExpr('col1 as id','col2*2 as col2_value')
```

## 数据清洗类操作
```py
# 去重
df.distinct() #返回一个去重的DataFrame
df.dropDuplicates()
df.dropDuplicates(subset=['col1','col2'])


df.dropna(how='any', thresh=None, subset=None)
# thresh 是一个阈值，例如，thresh=3，代表一行的缺失值达到3个以上时，移除这一行

df.fillna(0)
```

例子:一个数据表，可能重复的数据'id'这个字段也不一样，那么要去重就只能在除id字段以外的所有字段中去重，这么写：
```py
df.dropDuplicates(subset=[i for i in df.columns if i != 'id'])

```


## 统计分析类操作
### order
```py
df.orderBy(['col1','col2'],ascending=[0,1])
```
### 分位数
```py
df.approxQuantile('col1',[0.25,0.75],0.05) # 返回一个list，大小与第二个参数相同，表示分位数。
# 第一个参数是列名，第二个参数是分位数，第三个参数是准确度，设定为0时代价巨大

df.corr('pv','uv') # 相关系数，目前只支持两个字段，只支持Person相关系数
```

## groupby
```py
df.groupby('col1') # 返回一个GroupedData对象，可以对这个对象进行很多操作

#例1
df.groupby('col1').max('col2','col3')
# min,sum,mean,count
df.groupBy('col1').max() # 不加参数就是对所有列做同样的操作


# agg1:默认函数
df.groupby('col1').agg({'col2':'mean','col3':'sum'}) # 似乎不能与F混用

# agg2：F中的函数
from pyspark.sql import functions as F
df.groupBy('col1').agg(F.countDistinct('col2'))

# agg3：自定义函数
## agg3_1：udf左右于被 groupBy 的列，一一映射就有意义
spark.udf.register('udf_func1',lambda x:x+1)
df.groupBy('a').agg({'a':'udf_func1','b':'std'})
## agg3_2：udf作用于普通列：
# http://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.GroupedData.apply
# 2.3版本才能用，还没去实验

# UEF用于非groupby的场景
def func(x):
    return x + 1
spark.udf.register('func',func)
df.selectExpr('func(a)')


# 这个在pyspark 2.3才能用，还没试过：
from pyspark.sql.functions import pandas_udf, PandasUDFType

df = spark.createDataFrame(
    [(1, 1.0), (1, 2.0), (2, 3.0), (2, 5.0), (2, 10.0)],
    ("id", "v"))

@pandas_udf("id long, v double", PandasUDFType.GROUPED_MAP)
def substract_mean(pdf):
    # pdf is a pandas.DataFrame
    v = pdf.v
    return pdf.assign(v=v - v.mean())

df.groupby("id").apply(substract_mean).show()
# 参考： http://spark.apache.org/docs/latest/sql-programming-guide.html
```



### 非groupby下的agg

```py
df1.agg({'col1':'max','col2':'min'}) # 返回1行2列的 DataFrame

```


一下是相关网站：  
http://spark.apache.org/docs/2.1.1/api/python/index.html  
https://databricks.com/blog/2017/10/30/introducing-vectorized-udfs-for-pyspark.html  
http://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.GroupedData    
https://blog.csdn.net/dabokele/article/details/52802150  
https://blog.csdn.net/sparkexpert/article/details/51042970



## 合并操作

### 1. 纵向
```py
df1.union(df2) # 并集：纵向合并，不会删除重复
df1.intersect(df2) # 交集
df1.subtract(df2) # 差集
```
注意，这里的交集和差集是按照整个列

### 2. 横向
```py
df1.join(df2) #笛卡尔积，慎用！
df1.join(df2,on='id')
a.join(b,on=['id','dt'],how='inner') #innner, left, right


#进阶用法
a.join(b,on=a.id==b.id,how='right').show()
a.join(b,on=[a.id==b.id,a.col1>b.col2+1],how='right').show()
```


## funs
增添一列递增、唯一（但不连续）的数字
```py
import pyspark.sql.functions as fn
df_abnormal_id=df1.select(fn.monotonically_increasing_id().alias('id'),'*')
```


## 参考文献
https://blog.csdn.net/wy250229163/article/details/52354278
