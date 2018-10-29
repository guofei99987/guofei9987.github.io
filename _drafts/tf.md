
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
