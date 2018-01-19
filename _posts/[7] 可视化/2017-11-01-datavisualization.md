---
layout: post
title: 【Python】可视化方法汇总
categories:
tags: 7可视化
keywords:
description:
---

## 单变量

### 分布图

sns.distplot

[看这里](http://www.guofei.site/2017/10/01/seabron.html#title1)  

<img src='http://www.guofei.site/public/postimg2/seaborn1_1.png'>

### box图
sns.boxplot  
<a href='http://seaborn.pydata.org/examples/grouped_boxplot.html' target="title7">官方示例</a>  


```py
import seaborn as sns
df = sns.load_dataset("tips")

sns.boxplot(data=df, x="day", y="total_bill", hue="sex", palette="PRGn")
```
>palette='Set1','Set2','Set3','PRGn'...  


<img src='http://www.guofei.site/public/postimg/boxplot.png'>  


### 小提琴图

*基于seaborn[^violinplot]*  
<a href='http://seaborn.pydata.org/generated/seaborn.violinplot.html' target="violinplot">官方网站看这里</a>  



#### 小提琴图1
每一列数据作为一个小提琴
```py
import seaborn as sns
df = sns.load_dataset("tips")
sns.violinplot(data=df, palette="Set3", bw=.2, cut=3, linewidth=1)
```

<img src='http://www.guofei.site/public/postimg/violinplot.png'>


#### 小提琴图2
定义x列和y列，定义分类hue（可选）
```py
import seaborn as sns
df = sns.load_dataset("tips")
sns.violinplot(data=df, x="day", y="total_bill", hue="sex", bw=.2, cut=3, linewidth=1, palette="PRGn")
```
<img src='http://www.guofei.site/public/postimg/violinplot1.png'>  

#### 小提琴图3
```py
import seaborn as sns
df = sns.load_dataset("tips")

sns.violinplot(data=df, x="day", y="total_bill", hue="sex",split=True, bw=.2, cut=3, linewidth=1, palette="PRGn")
```


<img src='http://www.guofei.site/public/postimg/violinplot2.png'>  



### qq图

用来看看是否服从特定分布[^qqplot]

*(所用库：statsmodels)*

```py
from scipy.stats import t
data = t(df=5).rvs(size=1000)

import statsmodels.api as sm
from matplotlib import pyplot as plt
fig = sm.qqplot(data=data, dist=t, distargs=(3,), fit=True, line='45')
plt.show()
```

<img src='http://www.guofei.site/public/postimg/datavisualization1.png'>


## 多变量

散点图

### jointplot
数据准备
```py
import pandas as pd
import scipy.stats as stats

df=pd.DataFrame({'a':stats.norm(loc=0,scale=1).rvs(size=200),'b':stats.uniform(loc=3,scale=4).rvs(size=200)})
```

画图有2种方法[^jointplot]

```py
import pandas as pd
import scipy.stats as stats
df = pd.DataFrame({'a': stats.norm(loc=0, scale=1).rvs(size=200), 'b': stats.uniform(loc=3, scale=4).rvs(size=200)})

import matplotlib.pyplot as plt
import seaborn as sns
sns.jointplot(x='a', y='b', data=df, kind="kde", size=5, space=0)# 方法1
# sns.jointplot(x=df.a, y=df.b, kind="kde", size=5, space=0)# 方法2
plt.show()
```

kind='kde':  
<img src='http://www.guofei.site/public/postimg/jointplot_kde.png'>  


kind='hex'  
<img src='http://www.guofei.site/public/postimg/jointplot_hex.png'>  


kind='scatter'  
<img src='http://www.guofei.site/public/postimg/jointplot_scatter.png'>  


kind='reg'  
<img src='http://www.guofei.site/public/postimg/jointplot_reg.png'>  

### PairGrid

[去seaborn官网查看](http://seaborn.pydata.org/tutorial/axis_grids.html#plotting-pairwise-relationships-in-a-dataset)  


<img src='http://seaborn.pydata.org/_images/axis_grids_50_0.png'>

### clustermap
[去seaborn官网查看](http://seaborn.pydata.org/examples/structured_heatmap.html)



```py
import pandas as pd
import scipy.stats as stats
df = pd.DataFrame(stats.uniform(loc=0, scale=2).rvs(size=1000).reshape(-1, 5))


import matplotlib.pyplot as plt
import seaborn as sns
sns.clustermap(df)
plt.show()
```
<img src='http://www.guofei.site/public/postimg/clustermap1.png'>


对应的参数：  
```py
col_cluster=False
row_cluster=False
```

还有第二种图：
```py
sns.clustermap(df.corr())
plt.show()
```
<img src='http://www.guofei.site/public/postimg/clustermap2.png'>   


## 参考文献
[^violinplot]:  http://seaborn.pydata.org/generated/seaborn.violinplot.html  

[^qqplot]:  http://www.statsmodels.org/stable/generated/statsmodels.graphics.gofplots.qqplot.html
[^jointplot]: http://seaborn.pydata.org/generated/seaborn.jointplot.html
