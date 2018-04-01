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

## 1. Action
show操作
```py
df.head(5) #类似RDD，返回一个list，里面多个Row
df.take(5) #类似RDD，效果同head
df.collect() #多个Row组成的list
df.limit(5) #取前5行，不同的是，是transformation

df.show() #返回20条数据
df.show(30) #返回30条数据

df.first() #返回一个Row


df.describe() #返回统计值，是一个action，但返回的是DataFrame
df.describe('col1','col2') #返回指定字段的统计值


```

## 查询

```py
df.where('col1=1 and col2="abc"') #是transformation，返回RDD
df.filter('col1=1 and col2="abc"') #与where效果完全相同
df.filter(df.col1>5)

df.select('col1','col2') # 取指定列,返回RDD
df.selectExpr('col1 as id','col2*2 as col2_value')
```
## drop
```py
df.drop('col1','col2')
```

## order
```py
df.orderBy(['col1','col2'],ascending=[0,1])
```
## 列名
```py
df.columns #返回一个list，内容是列名
df.dtypes
```

## groupby
```py
df.groupby('col1') # 返回一个GroupedData对象，可以对这个对象进行很多操作

#例如：
df.groupby('col1').max('col2','col3')
# min,sum,mean,count

# agg
df.groupby('col1').agg({'col2':'mean'})
```
2018年2月22日，pyspark 2.3 上线，对udf和agg，apply等有巨大的改进  

一下是相关网站：  
http://spark.apache.org/docs/2.1.1/api/python/index.html  
https://databricks.com/blog/2017/10/30/introducing-vectorized-udfs-for-pyspark.html  
http://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.GroupedData    
https://blog.csdn.net/dabokele/article/details/52802150  
https://blog.csdn.net/sparkexpert/article/details/51042970
## 去重
```py
df.distinct() #返回一个去重的DataFrame
df.dropDuplicates()
df.dropDuplicates(['col1','col2'])


df.dropna(how='any', thresh=None, subset=None)

```

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



## 待解决的问题
1. df.select()，df.filter() 可以筛选行，那么能不能传入udf，实现高级筛选
2. agg,apply 在2.3版本中的使用


## 参考文献
https://blog.csdn.net/wy250229163/article/details/52354278
