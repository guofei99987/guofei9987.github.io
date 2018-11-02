
```
with tf.Graph().as_default():
    pass
```

## slim
很有用的东西，可以不用设置中间变量，给函数赋值
```
slim=tf.contrib.slim
with slim.arg_scope([slim.conv2d,slim.fully_connected],weights_regularizer=slim.l2_regularizer(weights_decay)):
    pass
```

### 队列和同步运算
Enqueue，Deq
ueue，MutexAcquire，MuterRelease
### 控制流
Merge，Switch，Enter，Leave，NextIteration

```py
x=tf.placeholder(shape=(None,6),dtype=tf.float32)
w1=tf.Variable(tf.random_normal(shape=(6,2)))
a=tf.matmul(x,w1) #矩阵乘法

sess.run(tf.global_variables_initializer())
sess.run(a,feed_dict={x:rv.rvs(size=(3,6))})
```


有很多已经训练好的模型，可以直接下载下来作为预训练模型  
https://github.com/tensorflow/models


keras

## 人脸检测
人脸检测：检测并定位图片中的人脸  
人脸关键点检测：返回五官和轮廓关键点的位置  
人脸属性检测：年龄、种族、性别、情绪


## 移动端
两种方法加快运行速度
### 1. 量化
quantitative  
例如，float32变成int8  
训练时不能量化，因为牵涉到梯度。  
部署时可以量化，因为神经网络对噪声的鲁棒性很强  

## 性能指标
### 1. 人脸识别
Top k 错误率  
Top k 召回率  
识别速度  
注册速度（注册一个人的时间）  
### 2. 聊天机器人
1. 回答与问应当语义一致、语法正确、逻辑正确
2. 有趣、多样，少一些安全回答（好啊，是啊之类的）
3. 个性表达一致


## 其它
```
tf.train.batch
tf.train.shuffle_batch
```

```
lstm:dropout
```
