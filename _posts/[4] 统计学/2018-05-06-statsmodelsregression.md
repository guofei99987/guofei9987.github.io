---
layout: post
title: 【statsmodels】回归专题（进阶）(补全中).
categories:
tags: 4统计学
keywords:
description:
order: 409
---


## 几种回归模型概述
介绍一下几种与回归statistical model[^statsmodels]有关的模型  
$Y=X\beta +u$,where$u\sim N(0,\Sigma)$  
* OLS : ordinary least squares for i.i.d. errors :$\Sigma=\textbf{I}$
* WLS : weighted least squares for heteroskedastic errors :$\text{diag}(\Sigma)$
* GLS : generalized least squares for arbitrary covariance :$\Sigma$
* GLSAR : feasible generalized least squares with autocorrelated AR(p) errors
  :$\Sigma=\Sigma\left(\rho\right)$





## OLS
setp1. 载入包和数据
```py
%matplotlib inline

import matplotlib.pyplot as plt
import os
import numpy as np
import pandas as pd
import statsmodels.api as sm
# from statsmodels.formula.api import ols
import statsmodels.formula.api as smf
df=pd.read_csv('http://archive.ics.uci.edu/ml/machine-learning-databases/auto-mpg/auto-mpg.data',sep='\s+',na_values='?',
               header=None,names=['mpg','cylinders','displacement','horsepower','weight','acceleration','model_year','origin','car_name'])
df=df.dropna(how='any')
df.describe()
```
step2. 构建模型
```py
lm_s = smf.ols(formula='mpg ~ displacement', data=df).fit()
```
step3. 模型结果
```py
lm_s.predict(df) # 返回预测值y，是一个array
lm_s.resid # 返回残差，是一个pd.Series
lm_s.params # 返回参数估计值，是pd.Series类型

lm_s.summary()
lm_s.rsquared #R^2
lm_s.rsquared_adj

lm_s.aic # 越低越好
lm_s.bic # 越低越好
```
step4. 残差分析
```py
sm.stats.linear_rainbow(lm_s)
sm.stats.durbin_watson(lm_s.resid) #dw检验
```
官网上还有多种检验方法
http://www.statsmodels.org/stable/stats.html#residual-diagnostics-and-specification-tests  

5 除此之外还有一些不常用的属性，例如自由度、似然值等等。为了节省本文篇幅，就不摘抄了，看[官网](http://www.statsmodels.org/stable/regression.html#attributes)  



## 对象一览（未整理）
### Model Classes
```
OLS(endog[, exog, missing, hasconst])	A simple ordinary least squares model.
GLS(endog, exog[, sigma, missing, hasconst])	Generalized least squares model with a general covariance structure.
WLS(endog, exog[, weights, missing, hasconst])	A regression model with diagonal but non-identity covariance structure.
GLSAR(endog[, exog, rho, missing])	A regression model with an AR(p) covariance structure.
yule_walker(X[, order, method, df, inv, demean])	Estimate AR(p) parameters from a sequence X using Yule-Walker equation.
QuantReg(endog, exog, **kwargs)	Quantile Regression
RecursiveLS(endog, exog, **kwargs)	Recursive least squares
```
### Results Classes

```

RegressionResults(model, params[, ...])	This class summarizes the fit of a linear regression model.
OLSResults(model, params[, ...])	Results class for for an OLS model.
QuantRegResults(model, params[, ...])	Results instance for the QuantReg model
RecursiveLSResults(model, params, filter_results)	Class to hold results from fitting a recursive least squares model.
```

## 其它重要模型（未整理）


### Generalized Linear Models  
smf.glm
smf.GLM


### Generalized Estimating Equations
smf.gee
smf.GEE


### Robust Linear Models
smf.rlm



### Linear Mixed Effects Models
smf.mixedlm  


#### 理论  
对第i个group：  
$Y=X\beta+Z\gamma+\varepsilon$  
其中，  
$n_i$是样本数，$k_{fe}$指的是 fixed effect 对应的变量数，$k_{re}$指的是 random effect 对应的变量数  
$Y_{n_i\times 1}$  
$X_{n_i\times k_{fe}},\beta_{k_{fe}\times 1}$ fixed effect对应的样本和参数  
$Z_{n_i\times k_{re}},\gamma_{k_{re}\times1}$ random effect 对应的样本和参数  
$\gamma$的期望为0，协方差矩阵为$\Psi$，each group gets its own independent realization of gamma  
$\varepsilon$均值为0，方差为$\sigma^2 I$,并且group内和group间都独立。  


#### 性质1  
$X,Y,Z$都是观察得到的样本  
$\beta,\Psi,\sigma^2$ 是需要估计出的量(使用ML或REML估计算法)  
#### 性质2
$E[Y\mid X,Z]=X\beta$  




有两种特殊的Linear Mixed Effects Model
- random intercepts models
- random slopes models


官网案例  


step1：加载包和数据  
```py
import numpy as np
import statsmodels.api as sm
import statsmodels.formula.api as smf
data = sm.datasets.get_rdataset('dietox', 'geepack').data
```

step2：  
a random intercept for each group
```py
md = smf.mixedlm("Weight ~ Time", data, groups=data["Pig"])
mdf = md.fit()
print(mdf.summary())
```


step3：  
a random intercept+a random slope
```py
md = smf.mixedlm("Weight ~ Time", data, groups=data["Pig"], re_formula="~Time")
mdf = md.fit()
print(mdf.summary())
```


step4:  
```py
md = smf.mixedlm("Weight ~ Time", data, groups=data["Pig"],re_formula="~Time")
free = sm.regression.mixed_linear_model.MixedLMParams.from_components(np.ones(2),np.eye(2))
mdf = md.fit(free=free)
print(mdf.summary())
```

### Regression with Discrete Dependent Variable
sm.Logit

### ANOVA
```
moore_lm = ols('conformity ~ C(fcategory, Sum)*C(partner_status, Sum)',data=data).fit()
table = sm.stats.anova_lm(moore_lm, typ=2) # Type 2 ANOVA DataFrame
```
### Time Series analysis tsa
### Time Series Analysis by State Space Methods statespace
### Methods for Survival and Duration Analysis
### Statistics stats

这里面很多假设检验模型
### Nonparametric Methods nonparametric
### Generalized Method of Moments gmm
### Contingency tables
### Multiple Imputation with Chained Equations
### Multivariate Statistics multivariate
### Empirical Likelihood emplike
### Other Models miscmodels
### Distributions
### Graphics

有几个画图功能还不错

### Input-Output iolib
### Tools
### The Datasets Package
### Sandbox





## 举例

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


![regression_plot](https://github.com/guofei9987/StatisticsBlog/blob/master/%E9%99%84%E4%BB%B6/regression_plot.png?raw=true)










## 参考资料
[^lihang]: [李航：《统计学习方法》](https://www.weibo.com/u/2060750830?refer_flag=1005055013_)  
[^wangxiaochuan]: [王小川授课内容](https://weibo.com/hgsz2003)  
[^EM]: 我的另一篇博客[EM算法理论篇](http://www.guofei.site/2017/11/09/em.html)  
[^AppliedRegression]: 《应用回归分析》，人民大学出版社  
[^statsmodels]: [statsmodels官网statistical model](http://www.statsmodels.org/stable/regression.html#technical-documentation)
