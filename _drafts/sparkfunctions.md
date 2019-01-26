
## 可以被代替的
F.split 正则表达式分割


abs
acos
asin
atan
atan2
avg
cos
cosh
exp
expml:$e^{x-1}$
factorial:阶乘，sql也有这个命令
hypot(col1, col2) $\sqrt{(a^2 + b^2)}$
log(col1,col2) # 对数，col1和col2都可以换成数字
log10(col)
log2(col)
log1p(col) # $\log(x+1)$
power(col1,col2) # 指数，col1和col2都可以换成数字

max
mean
min


count
countDistinct



base64
basestring
bin

bround 小数形式的四舍五入
ceil 向上取整
floor 向下取整


cbrt 三次方根

F.coalesce
取第一个不为空的元素（sql也有这个命令）

F.corr 皮尔逊相关系数
kurtosis(col)  峰度


approx_count_distinct
## 字符串
initcap(col) 每个单词的首字母大写（一个元素的字符串中可以有多个单词）（sql也有这个命令）
lower(col) 转为小写


instr(str, substr) str中，substr第一次出现的位置，可以用sql

locate(substr, str, pos=1) # str中，pos后，substr第一次出现的位置号，位置号从1开始，如果没找到就返回0

lpad(col, len, pad) # 用pad把col填充到len长度
ltrim(col) # 把左边的空格删掉



## 窗口函数
lag(col, count=1, default=None) # 窗口函数，返回往前数第count行的内容（ sql也有同样的命令）（ 机器上好像报错）
lead # 窗口函数，返回往后数第count行的内容（ sql也有同样的命令）（ 机器上好像报错）
first
last

## 时间

add_month : 增加一个月，mysql可以用 `select DATE_ADD('2018-02-05',INTERVAL 1 month) as dt` 但spark中不行
```
df = spark.createDataFrame([('2015-04-08',)], ['dt'])
df.select(add_months(df.dt, 1).alias('next_month')).collect()
```


current_date() 当前日期
current_timestamp() 当前时间

date_format(date, format) 把时间转化成指定格式

date_sub(start, days)
date_trunc(format, timestamp) 把时间四舍五入到某一个精度
datediff(end, start)

dayofmonth(col)
dayofweek(col)
dayofyear(col)

F.year() 从文本时间中取年
hour() 取小时
minute()
month()




last_day(date) # 所在月的最后一天

months_between(date1, date2, roundOff=True)
- 月份差
- date1大于date2，返回正值
- 默认返回整数，但roundOff=False 时返回8位小数


next_day(date, dayOfWeek) # 返回下一个“周n”，dayOfWeek=“Mon”, “Tue”, “Wed”, “Thu”, “Fri”, “Sat”, “Sun”




## array相关
F.array  
把两列粘合成一个list，粘合后，该列的元素类似 array<double>  格式

F.array_contains(col, value)  
其中col 是array 格式
- 如果array为null，返回null
- 如果array包含value，返回value
- 如果array不包含value，返回false


F.array_distinct(col)
col是array
功能：移除array中的重复

F.array_except(col1, col2)
col1和col2都是array
功能：生成在col1中，但不在col2中的元素组成的array

F.array_intersect(col1, col2)
功能：差集（剔除重复）

array_union(col1, col2)
交集（剔除重复）

arrays_overlap(a1, a2)
- 如果有共同的元素，返回 true
- 如果没有共同元素，如果任一有空元素，返回 null
- 否则返回false


F.array_join(col, delimiter, null_replacement=None)  
将array拼接成一个字符串，其中分割符为 delimiter 。如果指定了null_replacement，用null_replacement 替换空值，否则视空值不存在

F.concat
也是拼接（可用于 string，binary，array）如果array有一个为空，返回空。这个函数与sql的concat不太一样

F.concat_ws

F.array_max(col)
array中的最大值

F.array_min(col)
array中的最小值

F.array_position(col, value)
返回 array 从的第一个value 的序号，如果没有则返回null，序号是从1开始的

F.array_remove(col, element)
从array中删除所有的element

array_repeat(col, count)
array中的元素重复3次，生成一个新array

array_sort(col)
返回升序array，null放到最后

arrays_zip
http://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.functions.arrays_zip


covar_pop(col1, col2)
两个array的相关系数

covar_samp

F.greatest('col1','col2',...)
多列中最大的那个，sql也有这个命令

F.least('col1','col2',...) 最小的那个

## 待查询
pyspark.sql.functions.explode_outer(col)  
http://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.functions.explode_outer














## aggfunction
F.collect_list
collect_set




F.first
agg funciton: 返回一个group的第一条记录
F.last 返回最后一条记录

## 参考文献
http://spark.apache.org/docs/latest/api/python/pyspark.sql.html#module-pyspark.sql.functions
