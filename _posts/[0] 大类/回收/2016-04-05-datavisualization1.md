---
layout: post
title: 【回收】【可视化方法】
categories: 回收
tags:
keywords: model evaluation
description:

---



### boxplot

**回收原因：plt.boxplot的x轴只能是数字，用sns.boxplot代替**  
```py
import pandas as pd
import scipy.stats as stats
df = pd.DataFrame({'a': stats.uniform(0, 1).rvs(size=20), 'b': stats.norm(1, 1).rvs(size=20)})

import matplotlib.pyplot as plt
fig, ax = plt.subplots()
ax.boxplot(df.values)
plt.show()
```

<img src='http://www.guofei.site/public/postimg/boxplot.png'>

|参数|意义|
|--|--|
|notch=True|形状变成扁的|
|flierprops={'markerfacecolor':'g', 'marker':'D'}|离群点的样式|
|showfliers=False|是否显示离群点|
|vert=False|变成横的box图|

### sns.boxplot
```py
import seaborn as sns
sns.set(style="ticks")

# Load the example tips dataset
tips = sns.load_dataset("tips")

# Draw a nested boxplot to show bills by day and sex
sns.boxplot(x="day", y="total_bill", hue="sex", data=tips, palette="PRGn")
sns.despine(offset=10, trim=True)
plot.show()
```





<img src='http://www.guofei.site/public/postimg/boxplot.png'>


### 小提琴图

*基于seaborn[^violinplot]*  
<a href='http://seaborn.pydata.org/generated/seaborn.violinplot.html' target="violinplot">官方网站看这里</a>  



```py
import pandas as pd
import scipy.stats as stats
df = pd.DataFrame(
    {'a': stats.uniform(0, 1).rvs(size=20), 'b': stats.norm(1, 1).rvs(size=20), 'c': stats.norm(1, 1).rvs(size=20)})


import seaborn as sns
import matplotlib.pyplot as plt
fig, ax = plt.subplots(figsize=(11, 6))
sns.violinplot(data=df, palette="Set3", bw=.2, cut=3, linewidth=1)
plt.show()
```

<img src='http://www.guofei.site/public/postimg/violinplot.png'>

#### 小提琴图2

数据准备：  
```py
import pandas as pd
import scipy.stats as stats
df=pd.DataFrame({'a':stats.uniform(0,1).rvs(size=200),'b':stats.norm(1,1).rvs(size=200),'c':stats.randint(low=1,high=4).rvs(size=200),'col_bool':stats.uniform(0,1).rvs(size=200)>0.5})
df
```

<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>a</th>
      <th>b</th>
      <th>c</th>
      <th>col_bool</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.283142</td>
      <td>1.046024</td>
      <td>3</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.421666</td>
      <td>0.413187</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.448498</td>
      <td>0.841689</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.306374</td>
      <td>1.811465</td>
      <td>2</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.606838</td>
      <td>2.348296</td>
      <td>2</td>
      <td>True</td>
    </tr>
  </tbody>
</table>

第一种画法：
```py
import seaborn as sns
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(11, 6))
sns.violinplot(data=df, x='c', y='a', hue="col_bool", palette="Set3", bw=0.2, cut=2, linewidth=1)
plt.show()

```

<img src='http://www.guofei.site/public/postimg/violinplot1.png'>  


第二种画法：  
```py
import seaborn as sns
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(11, 6))
sns.violinplot(data=df, x='c', y='a', hue="col_bool", palette="Set3", bw=0.2, cut=2, linewidth=1, split=True)
plt.show()
```

<img src='http://www.guofei.site/public/postimg/violinplot2.png'>  
