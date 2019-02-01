---
layout: post
title: 【pandas】去重、填充、排序、变换
categories:
tags: 1B_Pandas
keywords:
description:
order: 103
---


## 去重

```python
data.drop_duplicates(inplace=True)
data.drop_duplicates(subset='column1') # 找第一列重复者
data.duplicated(keep='last') # 'first','last',返回布尔类型
```

```py
data.duplicated(keep='last')  # 返回Series，bool类型，存放是否是重复行/列
# keep='first','last'
```

## 删除空数据

```py
dropna(how='any') # how='all'
```

## 删除整行&整列

```python
data.drop('animal2', axis='columns', inplace=True)
```

参数可以是list，以删除多行/多列  


## 替换数据
```python
data.replace([4,5],np.nan,inplace=True)
data.replace({4:np.nan,5:999},inplace=True)
```

## 填充空数据

可以向上填充/向下填充
```python
a=data.fillna(method='bfill',inplace=True)
# method :bfill,ffill,
```

也可以用值填充
```python
a=data.fillna(data.mean(),inplace=True)
```

值填充时，可以每列不一样
```py
df.fillna({'a':999,'b':888,'c':777,'d':666})
```

线性插值填充

```py
df1.interpolate()
```

线性插值填充：把index作为间隔

```py
df1.interpolate(method='index')
```

## 计算新列

```python
import pandas as pd
import numpy as np
df=pd.DataFrame(np.arange(16).reshape(-1,4),index=list('abcd'),columns=list('wxyz'))
df.loc[:,'ww']=df.loc[:,'w']*2+df.loc[:,'x']
```



## 改变数据类型

```python
df.loc[:,['ww']].astype('float') # int
```



## 排序
### sort
- sort_values按值排序
- sort_index按index排序

```python
df.sort_values(by=['w','z'],ascending=[False,True],inplace=True)
df.sort_index(ascending=True,inplace=True)
```

### sorted
sorted(iterable,key,reverse)可以对任何iterable的对象进行排序

```
a=['111','26','76','3']
sorted(a,key=int)
sorted(a,key=lambda x:int(x[0]))

b=[{'a':4},{'b':2},{'c':3}]
sorted(b,key=lambda x:x[1])
```

### rank

返回排序的序号
```
import pandas as pd
import numpy as np
df=pd.DataFrame(np.random.rand(16).reshape(-1,4),columns=list('wxyz'))
df.loc[:,'w']=[0,1,1,2]
df
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>w</th>
      <th>x</th>
      <th>y</th>
      <th>z</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0.098404</td>
      <td>0.099138</td>
      <td>0.381158</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>0.776177</td>
      <td>0.478243</td>
      <td>0.523116</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>0.397995</td>
      <td>0.040227</td>
      <td>0.362902</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2</td>
      <td>0.997362</td>
      <td>0.072824</td>
      <td>0.709957</td>
    </tr>
  </tbody>
</table>


```py
df.rank(ascending=True,method='average')
```
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>w</th>
      <th>x</th>
      <th>y</th>
      <th>z</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>3.0</td>
      <td>2.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2.5</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>3.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.5</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4.0</td>
      <td>4.0</td>
      <td>2.0</td>
      <td>4.0</td>
    </tr>
  </tbody>
</table>

method说明：
- 'average'(default): 相等分组中，按平均值
- 'min': 取最小排名
- 'max': 取最大排名
- 'first': 按照原始数据中出现的顺序分配排名


## 变换
### transfrom：按列变换
```py
df = pd.DataFrame([['about', 'a|b|cc'], ['cool', 'x|yy|z']], columns=['col1', 'col2'], index=['a', 'b'])

def func(x):
    return x.upper()

df.transform({'col1': func})

# 1. transform返回一个DataFrame
# 2. func每次接收一个单元格。如果不能运行，常识接收整列作为 <Series>
# 3_1. 如果 func 返回一个 Series，那么把这个Series展开为多列
# 3_2. 如果 func 返回其它对象（数字、list等），那么把这个对象塞到新 DataFrame 的单元格内
```
例子：
```py
df = pd.DataFrame([['a|b|cc'], ['x|yy|z']], columns=['col1'])

# 用字符串方法做一切操作
df.transform({'col1': lambda x: x.upper()})
df.transform({'col1': lambda x: len(x)})
df.transform({'col1': lambda x: 'a' in x})

df.transform({'col2': lambda x: x.split('|')})
df.transform({'col2': lambda x: ','.join(x.split('|'))})

# 可以直接加减乘
df.col1 + df.col2 * 2
```

## agg



### apply：更灵活的变换

```python
data.apply(func1,axis=0) # 返回Series
# axis=0，输入一列
# axis=1，输入一行

def func(series):


# 1. apply 返回 Series
# 2. func 的输入是 原 DataFrame 的一行/一列，格式为 <Series>
# 3. 如果return数字、列表等其它对象，那么把其它对象作为每一个元素塞到一个单元格内
    # return内容就是data.apply这个series中的元素
    # 如果return一个 <Series> ,那么把这个 Series 作为新列

```

## 其它

### 用map修改index&columns
- rename可以完全替代这个
- 参数不能是dict


index有map()方法，但没有apply方法，案例：  
```python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.arange(16).reshape(4, -1), index=list('abcd'), columns=list('gfjk'))
df.index = df.index.map(str.upper)
```


## 非groupby的agg
### agg
```py
import pandas as pd
import numpy as np
from scipy import stats
rv=stats.uniform()
df=pd.DataFrame(rv.rvs(size=(100,5)),columns=list('abcde'))

def func(data):
    return data.max()

df.agg([np.mean,func]) # 此例返回2行，一行mean，一行func
# func接受每一列作为Series，返回一个数字

df.agg({'a':func}) # 对指定列做func，与上一条有个区别，func可以返回多组数字，这种情况下，func返回的多组数字是这条语句返回的多行
```

### transform
```py
import pandas as pd
import numpy as np
from scipy import stats
rv=stats.uniform()
df=pd.DataFrame(rv.rvs(size=(100,5)),columns=list('abcde'))

def func(data):
    return data+1

df.transform({'a':func,'b':func}) # func需要接受df每一列作为Series，返回同样大小的Series
```
