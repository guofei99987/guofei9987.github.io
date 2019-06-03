---
layout: post
title: 【spark】工程上的一些实践技巧
categories:
tags: 1_1_数据平台
keywords:
description:
order: 159
---


记录我在工程中的一些实用方法（都是个人偏好）  

1. **submit时传入参数** 用[sys.argv](http://www.guofei.site/2018/06/05/sysos.html#title1)来读取submit时附加的参数  
2. **脚本中调用别的包**

## 1. 被提交脚本，传入参数的设计
app_1_2_1.py（被提交的spark脚本）:
```py
from pyspark.sql import SparkSession
import os
import sys

spark = SparkSession.builder.appName("AppName").enableHiveSupport().getOrCreate()

arrs = sys.argv
cal_dt_str = eval(arrs[1])['cal_dt_str']
```

## 2. 提交脚本，批量提交
这里实现了以下功能：
1. 给被提交脚本传入数字、字符串等变量
2. 批量提交spark脚本
3. 提交的spark脚本全部运行完毕后，打印每个脚本的返回码、运行时间、运行开始时间、运行结束时间
4. 如果有脚本运行失败，退出时返回错误码


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
      {pyfile} "{arrs}"
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


import os
import datetime

today = datetime.datetime.now()
oneday = datetime.timedelta(days=1)
cal_dt_str = (today - oneday).strftime('%Y-%m-%d')
# cal_dt_str='2018-07-19'
print('开始计算' + cal_dt_str + '的数据')

input_file = [
    {'deploymode': 'client', 'pyfile': 'app_1_2_1.py', 'arrs': {'cal_dt_str': cal_dt_str}},
    {'deploymode': 'client', 'pyfile': 'app_1_2_2.py', 'arrs': {'cal_dt_str': cal_dt_str}},
]


def func_run(deploymode, pyfile, arrs):
    start_time = datetime.datetime.now()
    code = os.system(command1.format(deploymode=deploymode, pyfile=pyfile, arrs=arrs))
    end_time = datetime.datetime.now()
    return pyfile, code, (end_time - start_time).seconds, \
           start_time.strftime('%Y-%m-%d %H:%M:%S'), end_time.strftime('%Y-%m-%d %H:%M:%S')


result_code_list = []

for i in input_file:
    result_code = func_run(deploymode=i['deploymode'], pyfile=i['pyfile'], arrs=i['arrs'])
    result_code_list.append(result_code)

for i in result_code_list: print(i)
for i in result_code_list:
    if i[1] != 0:
        exit(i[1])

```

## 3. 并行提交代码
实现功能：
1. 给被提交脚本传入数字、字符串等变量
2. 同时提交多组脚本。例如按天运行的脚本，如果想重跑一年的数据，如果改代码会浪费人的时间和精力（虽然会节省机器的时间，但人的时间比机器的时间之前）。
3.

para_run.py  
```py
def paral_submit3(left_dt, right_dt, pyfile, step=-1, job_num=10):
    if left_dt > right_dt:
        print('左值不能大于右值')
        return False
    cal_dt = left_dt if step > 0 else right_dt
    oneday = datetime.timedelta(days=1)
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
          {pyfile} "{arrs}"
    '''
    log, subjob = [], dict()
    while True:
        if len(subjob) < job_num and left_dt <= cal_dt <= right_dt:
            cal_dt_str = cal_dt.strftime('%Y-%m-%d')
            subjob[cal_dt_str] = subprocess.Popen(command1.format(deploymode='cluster', pyfile=pyfile,
                                                                  arrs={'cal_dt_str': cal_dt_str}), shell=True)
            cal_dt += step * oneday
        for dt, task in subjob.items():
            tmp_task_poll = task.poll()
            if tmp_task_poll is not None:
                log.append([dt, tmp_task_poll])  # 用来记录每个脚本的运行状态
                subjob.pop(dt)  # 运行完毕的，从task中删掉
        if cal_dt > right_dt or cal_dt < left_dt:
            if len(subjob) == 0:
                # 所有日期全部跑完，且池子中的数据也已经清空
                break
        print('finished:', log)
        print('running:', subjob.keys())
        time.sleep(1)  # 每秒检测一次
    print('并行模块执行完毕，返回码如下:', log)
    return log, [i for i in log if i[1] != 0]


def paral_drop(left_dt, right_dt, table_name):
    cal_dt = right_dt
    oneday = datetime.timedelta(days=1)
    for i in range(2000):
        if cal_dt < left_dt: break
        print(cal_dt)
        os.system('''
            hive -e 'alter table {table_name} drop if exists partition (dt="{cal_dt_str}")'
            '''.format(cal_dt_str=cal_dt.strftime('%Y-%m-%d'), table_name=table_name))
        cal_dt -= oneday
```
使用：
```py
import sys

sys.path.append('test') # 上面的 para_run.py 放入的目录
from guofei8_tools import paral_run
import datetime


right_dt = datetime.datetime(year=2019, month=1, day=21)
left_dt = datetime.datetime(year=2017, month=1, day=1)

pyfile = 'spark_scripy.py' # 待提交的 spark 脚本
step = -1  # 运行间隔
job_num = 40 # 并行提交的个数
paral_run.paral_submit3(left_dt=left_dt, right_dt=right_dt, pyfile=pyfile, step=step, job_num=job_num)

# 批量删除分区
# paral_run.paral_drop(left_dt, right_dt, table_name)
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
