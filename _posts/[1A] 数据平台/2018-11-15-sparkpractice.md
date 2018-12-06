---
layout: post
title: 【spark】工程上的一些实践技巧
categories:
tags: 1A_数据平台
keywords:
description:
order: 154
---


记录我在工程中的一些实用方法（都是个人偏好）  

1. **submit时传入参数** 用[sys.argv](http://www.guofei.site/2018/06/05/sysos.html#title1)来读取submit时附加的参数  
2. **脚本中调用别的包**

## 1. submit时传参数
func2.py（被提交的spark脚本）:
```py
from pyspark.sql import SparkSession
import os
import sys

spark = SparkSession.builder.appName("AppName").enableHiveSupport().getOrCreate()

def parseArgs(arrs=sys.argv):
    """"
    parse k，v
    """
    dict_arrs = {}
    for i in arrs:
        if i.find('=') != -1:
            k, v = i.split('=')
            dict_arrs[k] = v
    return dict_arrs

dict_arrs = parseArgs()
cal_dt_str = dict_arrs['cal_dt_str']
print(cal_dt_str)
```

```py
command1 = '''
spark-submit   --master yarn \
      --deploy-mode {deploymode} \
      --driver-memory 6g \
      --executor-memory 10g \
      --num-executors 100 \
      --executor-cores 6 \
      --conf spark.yarn.appMasterEnv.yarn.nodemanager.container-executor.class=DockerLinuxContainer \
      --conf spark.executorEnv.yarn.nodemanager.container-executor.class=DockerLinuxContainer \
      --conf spark.yarn.appMasterEnv.yarn.nodemanager.docker-container-executor.image-name=my-docker.guofei.me:5000/wise_algorithm:latest \
      --conf spark.executorEnv.yarn.nodemanager.docker-container-executor.image-name=my-docker.guofei.me:5000/wise_algorithm:latest \
      --py-files jieba.zip \
      {pyfile} cal_dt_str={cal_dt_str}
'''
# --master:
#      spark://host:port  独立集群作为集群url
#      yarn     yarn作为集群
#      local    本地模式
#      local[N] 本地模式，n个核心
#      local[*] 本地模式，最大核心

# --deploy-mode:
#     client本地，cluster集群

# --executor-memory 执行器的内存
# --driver-memory 驱动器的内存


import subprocess

cal_dt = datetime.datetime(year=2018, month=1, day=13)
end_dt= datetime.datetime(year=2017, month=12, day=1)
# cal_dt+=28*oneday

iter_num = (cal_dt-end_dt)/oneday
job_num = 5
for i in range(iter_num // job_num):
    subjob = list()
    for i in range(job_num):
        print('#' * 1000)
        print(cal_dt)
        print('#' * 1000)
        a = subprocess.Popen(command1.format(deploymode='cluster', pyfile='file_name.py',cal_dt_str=cal_dt.strftime('%Y-%m-%d')), shell=True)
        subjob.append(a)
        cal_dt -= 1 * oneday
    for i in subjob:
        i.wait()
```



## 1. submit时传参数


guofei_tools.py:
```py
# coding=utf-8
import sys


def parseArgs_0(arrs=sys.argv):
    return arrs


def parseArgs(arrs=sys.argv):
    """"
    parse k，v
    """
    dict_arrs = {}
    for i in arrs:
        if i.find('=') != -1:
            k, v = i.split('=')
            dict_arrs[k] = v
    return dict_arrs
```

func2.py（被提交的spark脚本）:
```py
from pyspark.sql import SparkSession
import os
import sys

spark = SparkSession.builder.appName("AppName").enableHiveSupport().getOrCreate()

# 下面两行用于 CLI和client 两种模式，可以直接调用py文件中的函数
# client 和 cluster 模式下，
sys.path.append('../../tools')
os.remove('guofei_tools.pyc')
import guofei_tools

dict_arrs = guofei_tools.parseArgs()
cal_dt_str = dict_arrs['cal_dt_str']
print(cal_dt_str)

b = guofei_tools.parseArgs_0()
print(b)
```

main.py（程序入口）：
```py
#!/usr/bin/python
# coding=utf-8
import os
import datetime

command1 = '''
spark-submit   --master yarn \
      --deploy-mode {deploymode} \
      --driver-memory 6g \
      --executor-memory 10g \
      --num-executors 100 \
      --executor-cores 6 \
      --conf spark.yarn.appMasterEnv.yarn.nodemanager.container-executor.class=DockerLinuxContainer \
      --conf spark.executorEnv.yarn.nodemanager.container-executor.class=DockerLinuxContainer \
      --conf spark.yarn.appMasterEnv.yarn.nodemanager.docker-container-executor.image-name=me.guofei.com:4000/spark_docker:latest \
      --conf spark.executorEnv.yarn.nodemanager.docker-container-executor.image-name=me.guofei.com:4000/spark_docker:latest \
      --py-files ../../tools/tools.zip \
      {pyfile} cal_dt_str={cal_dt_str}
'''

today = datetime.datetime.now()
oneday = datetime.timedelta(days=1)
cal_dt_str = (today - oneday).strftime('%Y-%m-%d')
print('开始计算' + cal_dt_str + '的数据')

input_file = [
    {'deploymode': 'cluster', 'pyfile': 'fun2.py', 'cal_dt_str': cal_dt_str}
]


def func_run(deploymode, pyfile, cal_dt_str):
    start_time = datetime.datetime.now()
    code = os.system(command1.format(deploymode=deploymode, pyfile=pyfile, cal_dt_str=cal_dt_str))
    end_time = datetime.datetime.now()
    return pyfile, code, (end_time - start_time).seconds, \
           start_time.strftime('%Y-%m-%d %H:%M:%S'), end_time.strftime('%Y-%m-%d %H:%M:%S')


result_code_list = []

for i in input_file:
    result_code = func_run(deploymode=i['deploymode'], pyfile=i['pyfile'], cal_dt_str=i['cal_dt_str'])
    result_code_list.append(result_code)

for i in result_code_list: print(i)
```

## 其它实用技巧
```py
spark.sparkContext.uiWebUrl
# Spark Web UI
```
