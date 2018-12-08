---
layout: post
title: 【Python】可视化方法汇总
categories:
tags: 7可视化
keywords:
description:
order: 752
---

## 单变量

### 分布图 sns.distplot()

```py
from scipy import stats
import matplotlib.pyplot as plt  # 导入
import seaborn as sns

fig,ax=plt.subplots(2,1)
x = stats.norm.rvs(loc=0, scale=1, size=100)
sns.distplot(x, bins=20, kde=True, hist=True, rug=True, fit=stats.gamma,ax=ax[0]);
# bins直方图多少个矩阵条
# hist=True显示直方图
# kde=True 显示核密度分布图
# fit=stats.gamma 拟合
# rug=True 在x轴上显示每个观测上生成的小细条（边际毛毯）

sns.distplot(x, hist=False, color="g", kde_kws={"shade": True}, ax=ax[1])
# kde图上阴影
plt.show()
```

|displot的输入参数|解释|
|--|--|
|bins|直方图多少个矩阵条|
|hist|是否显示直方图|
|kde|是否显示核密度估计图|
|fit|是否显示拟合图|
|rug|是否显示边际毛毯|


<img src='http://www.guofei.site/public/postimg2/seaborn1_1.png'>



### box图
sns.boxplot  
<a href='http://seaborn.pydata.org/examples/grouped_boxplot.html' target="title7">官方示例</a>  
[matplotlib版本示例](http://www.guofei.site/2016/04/05/datavisualization1.html#title0)  

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
#split=True
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




## 常用自定义画图

一种回归并画图的例子

```py
# 导入包与数据
import pandas as pd
from statsmodels.sandbox.regression.predstd import wls_prediction_std
import matplotlib.pyplot as plt
import numpy as np
df=pd.DataFrame(np.random.rand(200).reshape(-1,2),columns=['x','resid'])
df.loc[:,'y']=2*df.loc[:,'x']+0.5*df.loc[:,'resid']+1

# 建模
import statsmodels.formula.api as smf
lm_s = smf.ols(formula='y ~ x', data=df).fit()

# 画图
prstd, iv_l, iv_u = wls_prediction_std(lm_s)
fig, ax = plt.subplots()
ax.plot(df.x, df.y, 'o', label="data")
ax.plot(df.x, lm_s.fittedvalues, 'r-', label="OLS")
ax.plot(df.x, iv_u, 'b--')
ax.plot(df.x, iv_l, 'b--')
ax.legend(loc='best')
plt.title('R2={a:.2f},y={b:.3f}x+{c:.3f}'.format(a=lm_s.rsquared,b=lm_s.params[0],c=lm_s.params[1]))
plt.show()
```

![regression_plot](https://www.guofei.site/pictures_for_blog/regression_plot.png)


## 参考文献
[^violinplot]:  http://seaborn.pydata.org/generated/seaborn.violinplot.html  

[^qqplot]:  http://www.statsmodels.org/stable/generated/statsmodels.graphics.gofplots.qqplot.html
[^jointplot]: http://seaborn.pydata.org/generated/seaborn.jointplot.html
