
## 下面这个不推荐了
原因：SQLcontext不支持udf

### Hive to Spark
```py
from pyspark.sql import SparkSession
spark = SparkSession.builder.appName("test").enableHiveSupport().getOrCreate()
df_data = spark.sql('select ...') #返回spark下的DataFrame类型

# 另一种做法（更推荐这种）
from pyspark.sql import HiveContext
hiveCtx=HiveContext(sc)
df_data=hiveCtx.sql('select ...')
```
### Spark to Hive
```py
df_data.toDF('col_name1','col_name2').registerTempTable("table3")
spark.sql("create table tmp.tmp_test_pyspark2hive as select * from table3")
# table3可以作为一个表用了，因此还有众多可以写sql的用法
```

## Spark与pandas的交互
### spark to pandas

```py
df_data.toPandas() #返回DataFrame类型
```
### pandas to spark
```py
spark.createDataFrame(<pd.DataFrame>)
```
