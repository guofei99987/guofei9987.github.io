---
layout: post
title: 【pandas】读入与读出
categories:
tags: 1_2_Pandas
keywords:
description:
order: 101
---

## 创建
### 1. 按列创建
- 方式1
```python
import pandas as pd
df = pd.DataFrame({'col1': [1] * 9, 'col2': ['one', 'tow', 'three'] * 3}, index=range(9))
```
- 方式2
```python
import pandas as pd
d = {'col1' : pd.Series([1., 2., 3.], index=['idx1', 'idx2', 'idx3']),'col2' : pd.Series([1., 2., 3., 4.], index=['idx1', 'idx2', 'idx3', 'idx4'])}
df = pd.DataFrame(d)
```


### 2. 按行创建
- 方式1
```py
df = pd.DataFrame([[1, 2], [3, 4], [5, 6]], index=range(3), columns=['col1', 'col2'])
```
- 方式2
```python
d = [{'col1' : 1,'col2':1},{'col1' : 2,'col2' : 2},{'col1' : 3,'col2' : 3},{'col2' : 4}]
df = pd.DataFrame(d,index=['idx1', 'idx2', 'idx3', 'idx4'],columns=['col1','col2'])
df.index.name='index'
```

## 各种to
- to_dict
```python
df.to_dict(orient='Series')
df.to_dict(orient='records')
# 两个都是返回一个Series组成的dict
```
- to_excel&read_excel
```python
# 从EXCEL读入DataFrame：
bonus = pd.read_excel('bonus_schedule.xls')
# 将DataFrame写入EXCEL：
bonus.to_excel('foo1.xlsx', sheet_name='sheet1')
```

### to_csv&read_csv
```python
macrodata = pd.read_csv('macrodata.csv')
macrodata.to_csv('d:/foo.csv')
# header： 选择哪一行作为columns name，读入的数据从header的下一行开始
#     - int：这一行作为columns name
#     - list of ints：几行合起来作为columns name
#     - None：不用数据作为columns name，而是用自然数
# index_col：  选择那一列作为index name
#     - int：选择第几列作为index name
#     - list：选择多列作为多层index name(**非常强大！**)
#     - None：不用数据做index name，而是用自然数
# names： 自定义columns name
# sep：`'\t', '\s+'` 等
```

### to_json

生成数据
```Python
import pandas as pd
a=pd.DataFrame({"col1":['str1','str2','str3'],"col2":[1,2,3]},index=["idx1","idx2","idx3"])
```

out:

<table>
<tr><th></th><th>col1</th><th>col2</th></tr>
<tr><td>idx1</td><td>str1</td><td>1</td></tr>
<tr><td>idx2</td><td>str2</td><td>2</td></tr>
<tr><td>idx3</td><td>str3</td><td>3</td></tr>
</table>



- orient='index'
```Python
a.to_json('a.json',orient='index')
```
    - out:
```Json
{"idx1":{"col1":"str1","col2":1},"idx2":{"col1":"str2","col2":2},"idx3":{"col1":"str3","col2":3}}
```

- orient='columns'
```Python
a.to_json('a.json',orient='columns')
```
    - out:  
```Json
{"col1":{"idx1":"str1","idx2":"str2","idx3":"str3"},"col2":{"idx1":1,"idx2":2,"idx3":3}}
```
- orient='records'
```Python
a.to_json('a.json',orient='records')
```
    - out:  
```Json
[{"col1":"str1","col2":1},{"col1":"str2","col2":2},{"col1":"str3","col2":3}]
```
- orient='split'
```Python
a.to_json('a.json',orient='split')
```
    - out:
```Json
{"columns":["col1","col2"],"index":["idx1","idx2","idx3"],"data":[["str1",1],["str2",2],["str3",3]]}
```

- orient='values'
```Python
a.to_json('ax1.json', orient='values')
```
    - out:
```Json
[["str1",1],["str2",2],["str3",3]]
```

## 其它to
```py
df.to_clipboard # 到剪切板上
to_panel
to_period # 把时间序列数据，变成频率数据
to_latex
to_html
to_string
to_pickle # 存到内存中
to_sql # 也挺有用，在另一篇博客里详解
```

## 循环
- 每次读一行
```py
df=pd.DataFrame(np.random.rand(5,2),columns=list('ab'))
for index,row in df.iterrows():
    print(index,row['a'],row['b']) # row是一个 <Series>
```
- 每次读一列
```py
df=pd.DataFrame(np.random.rand(5,2),columns=list('ab'))
for col_name,col in df.iteritems():
    print(col_name,col) # col是一个 <Series>
```


## pd.set_option

```py
pd.set_option('display.max_columns',5000)
pd.set_option('display.max_columns', None) # 显示所有的列
pd.set_option('display.width',100000)
pd.set_option('display.max_rows', None) # 显示所有行
pd.set_option('display.max_colwidth',100) # 有时候一个单元格里面的内容太长，超过上限会不显示并加上省略号
```
