https://blog.csdn.net/create115721/article/details/79243641

## 快捷键
Shift + M 合并cell.

## 显示
```py
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```
每一行都会显示，而不是最后一行才显示，例如:
```py
1
2
3
```
会把3个都显示出来
## 清除
清除单元格的输出
```py
from IPython.display import clear_output
clear_output()
```
## magic
加一个百分号可以运行cmd
```
%ls
%run test.ipynb
```

```py
# 计时
%%time
pass
```

## 运行平台命令
```
!ls
```






























ending
