
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
