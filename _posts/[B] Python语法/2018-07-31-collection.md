---
layout: post
title: 【Python】collection
categories:
tags: Python语法
keywords:
description:
order: 1011
---

## Counter
```py
import collections
counter=collections.Counter('aaabbc')
# 返回 Counter({'a': 3, 'b': 3})
counter.keys(),counter.values()
counter.most_common(2) # 返回频率最高的n个，例如 [('a', 3), ('b', 2)]

# 其它生成 Counter 对象的方法
collections.Counter({'a':2,'b':1}) # 返回 Counter({'a': 2, 'b': 1})
```

## defaultdict
在 dict 的基础上，复写了一个方法
```py
import collections
d=collections.defaultdict(int)
# 与 dict 的区别是，dict 中没有的key 需要重新赋值。这里不需要，而是用 int 赋值过了
for i in range(5):
    d[i]+=i

d=collections.defaultdict(list)
for i in range(5):
    d[i].append(i)
```

## namedtuple
```

```
