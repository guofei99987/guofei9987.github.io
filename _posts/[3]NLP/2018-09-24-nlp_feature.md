---
layout: post
title: 【NLP2】主流feature生成方法
categories:
tags: 3_NLP
keywords:
description:
order: 301
---
## 词袋
```py
from sklearn.feature_extraction import text
count_vectorizer=text.CountVectorizer()
# ngram_range
# max_df : float in range [0.0, 1.0] or int, default=1.0
# min_df : float in range [0.0, 1.0] or int, default=1
# max_features : int or None, default=None
# vocabulary : Mapping or iterable, optional


# 源数据
corpus = ['This is the first document.',
          'This is the second second document.',
          'And the third one.',
          'Is this the first document?']


count_vectorizer.fit(corpus)
# count_vectorizer.fit_transform(corpus)
X=count_vectorizer.transform(corpus)
# corpus中没有出现的词，在transform阶段被忽略
X.toarray() # 是一个shape=(num_sentence,num_words) 的array

count_vectorizer.vocabulary_ # vocabulary向量（dict格式）


# 下面这个函数用来分词
analyze = bigram_vectorizer.build_analyzer()
analyze('Bi-grams are cool!')
```
其它自定义配置
```py
ngram_range=(1, 2) # 分词时，除了自身外，还可以保留前后单词，例如，这个案例可以以这个为词典：
# ['bi', 'grams', 'are', 'cool', 'bi grams', 'grams are', 'are cool']
```

## 介绍
TF-IDF(Text Frequency-Inverse Document Frequency)   
有时候我们想要调整某些特定单词的权重，提高有用单词的权重，降低常用或无意义单词的权重。  


$f_{ij}=$frequency of term (feature) i in doc j  
$TF_{ij}=\dfrac{f_{ij}}{\sum_k f_{kj}}$  


$n_i=$number of docs that mention term i  
$N=$total number of docs  
$IDF_{i}=\log\dfrac{N}{n_i}$  


$TF-IDF_{ij}=TF_{ij}\times IDF_i$  


```py
from sklearn.feature_extraction import text
tf_idf_transformer=text.TfidfTransformer()

counts = [[3, 0, 1],
           [2, 0, 0],
           [3, 0, 0],
           [4, 0, 0],
           [3, 2, 0],
           [3, 0, 2]]
tf_idf_transformer.fit(counts)

tf_idf_transformer.transform(counts).toarray()
```
由于tf-idf经常用于文本特征，因此有另一个类称为TfidfVectorizer，将 CountVectorizer 和 TfidfTransformer 的所有选项合并在一个模型中：
```py
TfidfVectorizer # =CountVectorizer+TfidfTransformer
```


## 参考资料
http://scikit-learn.org/stable/modules/feature_extraction.html  
https://blog.csdn.net/pipisorry/article/details/41957763  
Nick McClure:《TensorFlow机器学习实战指南》 机械工业出版社  
lan Goodfellow:《深度学习》 人民邮电出版社  
王琛等：《深度学习原理与TensorFlow实战》 电子工业出版社  
李嘉璇：《TensorFlow技术解析与实战》 人民邮电出版社  
黄文坚：《TensorFlow实战》 电子工业出版社  
郑泽宇等：《TensorFlow实战Google深度学习框架》 电子工业出版社
