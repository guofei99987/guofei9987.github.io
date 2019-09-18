---
layout: post
title: NLP中主要的feature提取方法
categories:
tags: 2_4_NLP
keywords:
description:
order: 301
---


## 1. 文本数据的初步清洗
### step1 读入数据
```py
import pandas as pd
df=pd.read_csv('http://www.guofei.site/datasets_for_ml/SMSSpamCollection/SMSSpamCollection.csv',sep='\t',header=None,names=['label','sentences'])
Y=(df.label=='spam')*1
X=df.sentences.values
```
### step2 初步清洗
清洗目标：
1. 统一变成小写
2. 去除标点、数字等
3. 去除单个字母的单词
4. （如有）去除stopwords中的词语


```py
import re
regex=re.compile('[a-zA-Z]{2,}') # 2次以上字母组合，去除标点符号和数字
X=[regex.findall(sentence.lower()) for sentence in X]
X=[' '.join(words) for words in X] # 重新整合成句子
# X=[' '.join([word for word in words if word not in stops]) for words in X] # 如果有stopswords的话


# train_test_split
from sklearn.model_selection import train_test_split
X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.2)
```


给一个中文的例子：
1. 用jieba拆词
2. 去除数字
3. 去除停词
4. 用空格拼回

```py
import jieba
import re
X = [jieba.lcut(sentence, cut_all=False) for sentence in X]  # 拆词
regex = re.compile(u'[\u4e00-\u9fa5]')
X = [[word for word in sentence if regex.match(word) is not None] for sentence in X]  # jieba 拆开的词，一串数字也作为一个词返回，这里过滤一下
# X=[[word for word in sentence if word not in stops] for sentence in X] # 停词
X = [' '.join(sentence) for sentence in X]
```


## 2. VocabularyProcessor
给每个词对应一个数字标号（dictionary），然后把整个句子按照 dictionary 的一一对应关系，转换成数字列表。  
*tensorflow.contrib.learn.preprocessing.VocabularyProcessor 可以实现这个功能，但是这个函数将被移除。*  
*网上的实现也不够 Pythonic*  
**所以自己写了个类** ， 轻量、好用  


```py
import collections


class VocabularyProcessor:
    '''
    author: guofei
    site: www.guofei.site
    输入：一个列表，列表中的元素是句子，句子中的单词用空格分开。这里不提供数据清洗功能，所以应当先进行数据清洗，然后使用这个类
    输出：一个列表的列表，长度是词语个数，元素是单词的序号，例如 [[1, 2], [1, 2, 0, -1, -1, 3], [-1, 0, -1, -1], [2, 1, 0, -1, 3]]
    ```
    corpus = ['this is the first document',
              'this is the second second document',
              'and the third one',
              'is this the first document']
    wp = VocabularyProcessor(max_vocabulary_size=5)
    wp.fit(corpus)
    X = wp.transform(corpus)
    ```
    - 剔除低频词，并且把所有的低频词标记为 'RARE'，其序号固定为 0
    - max_vocabulary_size=10000 最多保留多少单词
    '''
    def __init__(self, max_vocabulary_size=10000):
        self.max_vocabulary_size = max_vocabulary_size
        self.word_count = None
        self.dictionary = None
        self.reverse_dictionary = None

    def fit(self, sentences):
        words = [word for sentence in sentences for word in sentence.split(' ')]
        word_count_ = collections.Counter(words)  # 词频
        word_count = word_count_.most_common(self.max_vocabulary_size - 1)  # 最多词频

        words_most = [word for word, num in word_count]  # 频率最高的词
        len_words_most=len(words_most)
        dictionary = dict(zip(words_most, range(1,len_words_most)))
        reverse_dictionary = dict(zip(range(1,len_words_most), words_most))

        # 添加 'RARE' ,也就是低频词语
        rare_count = sum([word_count_[word] for word in word_count_ if word not in words_most])
        word_count = [('RARE', rare_count)] + word_count
        dictionary['RARE'] = 0
        reverse_dictionary[0] = 'RARE'

        self.word_count = word_count
        self.dictionary = dictionary
        self.reverse_dictionary = reverse_dictionary

    def transform(self, sentences):
        return [[self.dictionary.get(word, 0) for word in sentence.split(' ')] for sentence in sentences]

    def fit_transform(self, sentences):
        self.fit(sentences)
        return self.transform(sentences)
```


使用方法
```py
corpus = ['this is the first document',
          'this is the second second document',
          'and the third one',
          'is this the first document']
wp = VocabularyProcessor(max_vocabulary_size=5)
wp.fit(corpus)
X = wp.transform(corpus)
print(X)
```
>[[1, 2, 0, -1, 3], [1, 2, 0, -1, -1, 3], [-1, 0, -1, -1], [2, 1, 0, -1, 3]]

fit之后，还可以查询字典的其它信息
```py
wp.word_count, wp.dictionary, wp.reverse_dictionary
```


## 3. 词袋（Bag of Words）

### 词袋介绍
结果的长度等于字典的长度，结果的元素值是在这句话中单词出现的次数。  

例如，我们的词袋为
```py
{'tensorflow': 4, 'makes': 3, 'machine': 2, 'learning': 1, 'easy': 0}
```
词袋的计算结果为
```py
sentence1 = 'TensorFlow does make machine learning easy!'
sentence1_list = [0,1,1,1,1,1]

sentence2 = 'Machine learning is easy'
sentence2 = [1, 1, 1, 0, 0]
```

### 代码实现
```py
from sklearn.feature_extraction import text

count_vectorizer = text.CountVectorizer()
# ngram_range
# max_df : float in range [0.0, 1.0] or int, default=1.0
# min_df : float in range [0.0, 1.0] or int, default=1
# max_features : int or None, default=None
# vocabulary : Mapping or iterable, optional


# 源数据
X = ['This is the first document.',
     'This is the second second document.',
     'And the third one.',
     'Is this the first document?']

count_vectorizer.fit(X)
X_transform = count_vectorizer.transform(X)
# 1. 不在vocabulary中的词，在transform阶段被忽略
# 2. 返回的 numpy 的稀疏矩阵，用 X.toarray() 转换成普通矩阵

X_transform.toarray()  # 是一个shape=(num_sentence,num_words) 的array

# count_vectorizer.fit_transform(corpus) # 合并写法

count_vectorizer.vocabulary_  # vocabulary向量（dict格式）
# {u'and': 0, u'document': 1, u'first': 2, u'is': 3, u'one': 4, u'second': 5, u'the': 6, u'third': 7, u'this': 8}
```
其它自定义配置
```py
ngram_range=(1, 2) # 分词时，除了自身外，还可以保留前后单词，例如，这个案例可以以这个为词典：
# ['bi', 'grams', 'are', 'cool', 'bi grams', 'grams are', 'are cool']
```


## TF-IDF
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
