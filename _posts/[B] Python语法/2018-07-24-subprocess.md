---
layout: post
title: 【Python】安全工具
categories:
tags: Python语法
keywords:
description:
order: 1011
---

`os.system` 在使用上有一定的局限性，因此，用 `subprocess` 来代替

## 1. subprocess.call
```
subprocess.call(args, *, stdin=None, stdout=None, stderr=None, shell=False)
```
1.1 参数args描述了子进程中需要执行的命令；

1.2 父进程会等待子进程的结束，并获得call函数的返回值

```py
import subprocess
cmd = ['ls', '-l']
ret = subprocess.call(cmd)
cmd = ['exit 1']
ret = subprocess.call(cmd, shell=True)
```

1.3 如果子进程不需要进行交互,就可以使用该函数来创建
## 2. subprocess.check_call
```py
subprocess.check_call(args, *, stdin=None, stdout=None, stderr=None, shell=False)
```
2.1 check_call()与call()唯一的区别在于返回值。如果args执行之后的返回值为0，那么check_all返回0；如果返回值不为0，那么将raise出来一个CalledProcessError
```py
cmd1 = ['exit 1'] # 将会报错
cmd2 = ['ls', '-l'] # 不会报错，因为返回的是0
ret = subprocess.check_call(cmd2, shell=True)
```

## 3. subprocess.check_output
```py
subprocess.check_output(args, *, stdin=None, stderr=None, shell=False, universal_newlines=False)
```
3.1 子进程执行args中的命令，并将其输出形成字符串返回  

3.2 如果返回值非零，那么将raise一个CalledProcessError。这一对象实例中有returncode属性以及output属性（args命令的output）
```py
cmd1 = ['exit 1'] # 将会报错
cmd2 = ['ls', '-l'] # 不会报错，因为返回的是0
ret = subprocess.check_output(cmd2, shell=True) 
```
## 4. subprocess.Popen

```py
class subprocess.Popen(args, bufsize=0, executable=None, stdin=None, stdout=None, stderr=None, preexec_fn=None, close_fds=False, shell=False, cwd=None, env=None, universal_newlines=False, startupinfo=None, creationflags=0)
```
4.2 Popen对象创建后，主程序不会自动等待子进程完成。我们必须调用对象的wait()方法，父进程才会等待 (也就是阻塞block)

4.3 Popen中封装的其他函数：

4.3.1 Popen.poll()：检查子进程的状态，查看子进程是否结束

4.3.2 Popen.wait()：等待子进程的结束

4.3.3 Popen.communicate(input=None)：与子进程进行交互。向stdin发送数据，或从stdout和stderr中读取数据。可选参数input指定发送到子进程的参数。Communicate()返回一个元组：(stdoutdata, stderrdata)。注意：如果希望通过进程的stdin向其发送数据，在创建Popen对象的时候，参数stdin必须被设置为PIPE。同样，如果希望从stdout和stderr获取数据，必须将stdout和stderr设置为PIPE。

4.3.4 Popen.send_signal(signal)：向子进程发送信号

4.3.5 Popen.terminate()：停止(stop)子进程。在windows平台下，该方法将调用Windows API TerminateProcess（）来结束子进程

4.3.6 Popen.kill()：杀死子进程

4.3.7 Popen.stdin：如果在创建Popen对象是，参数stdin被设置为PIPE，Popen.stdin将返回一个文件对象用于策子进程发送指令；否则返回None

4.3.8 Popen.stdout：如果在创建Popen对象是，参数stdout被设置为PIPE，Popen.stdout将返回一个文件对象用于策子进程发送指令；否则返回None

4.3.9 Popen.stderr：如果在创建Popen对象是，参数stdout被设置为PIPE，Popen.stdout将返回一个文件对象用于策子进程发送指令；否则返回None

4.3.10 Popen.pid：获取子进程的进程ID

4.3.11 Popen.returncode：获取进程的返回值。如果进程还没有结束，返回None
## 参考文献
https://blog.csdn.net/yuchen1986/article/details/22059873
