---
layout: post
title: 【string】字符串，编码，正则表达式
categories:
tags: Python语法
keywords:
description:
order: 1203
---




## 转义字符

```python
print("\"大家好\"")
#\\表示一个斜杠\
#\n表示换行
```

引号可以嵌套
```python
c2 = 'It is a "dog"!'
c2= "It's a dog!"
```

三引号：
```python
c1="""he
she
my
you are
hello"""
```

字符串前面加r，使转移符\ 失效
```python
r"hello boy\nhello boy"
```

## 索引
```
x="hello"
x[-1]
x[::-2]
```
## +与*
- 加法操作连接字符串
- 乘法操作重复连接字符串


## 字符串方法

```python
len('abc')

'aaabc'.count('aa')
'aaabc'.endswith('bc'),'aaabc'.startswith('bc')  #bool类型,是否以指定字符串开头/结尾
'aaabc'.index('bc')  #找到则返回序号，找不到则引发ValueError
'aaabc'.find('bc')  #找到则返回第一个的序号，找不到则返回-1，'aaabc'.find('a',1)数字表示从第几个开始找
'aaabc'.replace('bc','x')  #替换指定字符

'aaabc'.upper(),'aaabC'.lower()  #转大写/转小写
a.swapcase() # 返回一个字符串，大写转为小写，同时小写转为大写
'aaabc'.capitalize()  # 返回首字母大写的字符串

','.join(list('abc'))  #前面的字符也可以为空，这时相当于对字符串做加号
'a,b,c'.split(',')    #按','分割，并返回<list>。如果后面的参数为空，按照空格分割

'  \n a,b, c '.strip()   #去两边的空格与换行,strip,lstrip,rstrip
'abc! '.strip('a') #去掉指定字符

'abc'.ljust(5) , 'abc'.rjust(5) ,'abc'.center(10)  # 填充空格使其达到指定长度
'123'.isdecimal(); '123'.isnumeric();'123'.isdigit()
'ab'.islower();'ab'.isupper()

for <var> in <string>

len()  返回字符串的长度
str()  返回数字对应的字符串

```

## 格式化方法
### format()方法

```python
x="{1}{2}:计算机的CPU占用了{0}%。"
print(x.format(10,"2016-12-31","python"))
"{date} {name}:计算机的CPU占用了{}%。".format(10,date="2016-12-31",name="python")
```
注1：{}中的数字代表序号，序号可以省略  
注2：用两个大括号来print大括号  
注3：槽的高级使用方式： <序号>：<填充><对齐><宽度>，<精度><类型>
- <填充>:用于填充的字符
- <对齐>：<左对齐,>右对齐, ^居中对齐
- <宽度>：数字，实际值宽度不够则填充，实际值宽度太宽则用实际的
- ，：千分位的逗号
- <精度>：浮点小数部分的精度，字符串最大输出长度
- <类型>:整数类型b,c,d,o,x,X 浮点类型e,E,f,%
1. b:二进制，c:整数对应的unicode字符，d:整数十进制，o八进制，x小写十六进制，X大写十六进制
2. e:小写的科学计数法，E:大写科学计数法，f浮点形式，%浮点数的百分形式

### %
```py
"%(date)s %(name)s:计算机的CPU占用了%(data)s 。"%{'data':'10','date':"2016-12-31",'name':"python"}
```


几个案例：
```py
string.upper().find("HI")
string.split()#按空格分割,返回<list>
string.split("a")#按a分割,返回<list>
string.replace("o","a")#把string中的o替换成a
```

### join
```py
','.join(list('abc'))
```





## 编码问题
```py
str(1)#数字转字符
int('1')#字符转数字
int('51',base=14)#指定进制字符转十进制
ord("A")#字符转ascii码
chr(97)#ascii码转字符
```

### encode & decode
```py
s="中文字符串" # Python3 默认的字符串是 utf-8 格式
bs=s.encode("utf-8") # utf-8 转为 byte 格式
bs.decode("utf-8") # byte 格式 转为 utf-8

# 除此之外，还有 "utf-8", "utf-16", "ascii", "ISO-8859-1" 等格式
# byte 格式，print出来类似 b'\xe4\xbd\xa0\xe5\xa5\xbd\xe5\x90\x97\xef\xbc\x8c123 hello' 这样，这是utf-8的样子
# 假如是这样的 '%u8a84%u12bc' 这是 unicode 编码，每段4位16进制数对应ascii码，例如 chr(int('12cd', base=16))
```
- ascii码 7个二进制位
- Unicode 每个字符2个字节（4位16进制）
- UTF-8：可变长度的unicode，英文对应单字节，中文对应3字节

在计算机内存中，统一使用Unicode编码，当需要保存到硬盘或者需要传输的时候，就转换为UTF-8编码。


## enumerate迭代器
```python
String1 ='hello world'
for index,letter in enumerate(String1):
    print(index,letter)
for i in enumerate(String1)
    print(i) #i是tuple类型
```
此外，enumerate也可以用于list

## string模块
```python
import string  #string模块，处理字符比较方便
string.punctuation
string.digits
string.whitespace
```

返回的是：
```
'!"#$%&\'()*+,-./:;<=>?@[\]^_`{|}~'
'0123456789'
' \t\n\r\x0b\x0c'
```

## regex正则表达式
step1：编译一个可重用的regex对象
```py
import re
regex=re.compile('\s+') # 返回一个regex对象
```
step2：使用regex对象

```py
text='a\n b \t  c'
regex.split(text) # 返回list

regex.findall(text) # 返回list
regex.finditer(text) # 迭代器,存放的是<SRE_Match object>
m = regex.match(text) # 是否精确匹配，返回<SRE_Match object>

regex.sub(sentence, replace_char, 2) # 把 sentence 前2个符合的语句，替换成replace_char
```

step3：使用'SRE_Match'对象


### 正则表达式的写法
```py
m.start() # 返回起始位置
m.end() # 返回结束位置(不包含该位置的字符).

m.span() # 返回一个tuple表示(m.start(), m.end())

```


1.  句点符号：通配符
```
'.' # 如果句号没在[]中出现，表示匹配任意一个（只有一个）字符（包括空格，但除去换行符）。
```
2. 方括号符号
```  
 '[oum]' ——找到方括号中的任意一个即是匹配  
^表示取反，例如[^abc]表示除了a,b,c之外的所有字符  
+ 表示多次重复c[aeiou]+t  
```
3.  范围表达式
```
 '[c1-c2]' ——匹配从字符c1开始到字符c2结束的字母序列（按字母表中的顺序）中的任意一个。
例如，[a-zA-Z]表示英文字母，[0-9]表示任意数字  
\w 相当于[a-zA-Z0-9_]
\W 相当于[^a-zA-Z0-9_]
\s 相当于[\t\f\n\r]
\S 相当于[^\t\f\n\r]
\d 相当于[0-9]
\D 相当于[^0-9]
```
4. 转义符  
```
\n ——特殊字符，为了防止混淆
\.
\^
\xN或\x{N} 匹配八进制数值为N的字符
\oN或\o{N} 匹配十六进制数值为N的字符
\a Alarm(beep)
\b Backspace
\t 水平Tab
\n New line
\v 垂直Tab
\f 换页符
\r 回车符
\e Escape
\c 某些在正则表达式中有语法功能或特殊意义的字符c，要用\c来匹配，例如句号
```
5. 范围表达式  
```
\oN or \o{N}\  ASCII码表
\xN or \x{N}
'[\u4e00-\u9fa5]' # 全部中文
'[。；，：“”（）、？《》]' # 中文标点
```
6. 量词  
```
(expr)* #匹配任意多个(或0个)字符
(expr)? #0或1次
(expr)+ #1次以上
(expr){m,n} #m次以上，n次以下。例如ab{1,3}c,可以匹配abc,abbc,abbbc
(expr){m,} #m次以上
(expr){n} #n次
```


## 参考文献
https://docs.python.org/3/
