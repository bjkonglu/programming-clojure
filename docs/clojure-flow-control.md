# clojure-flow-control

## 分支结构与if
> Clojure的if会对其第一个参数进行求值。倘若结果逻辑为真，就返回对第二参数求值的结果。

1. usage: (*if* expre yes-result no-result)
2. example: (if (< 2 3) "2<3" "2>3")
3. 特殊用法：Clojure的if,其每个分支只能有一个形式。如果想在某个分支中做几件事情，采用*do*.
    - *do*: *do*可以接受任意数量的形式，对这些形式逐个求值，并返回最后一个形式的求值结果。
    - example: (if (< number 10) "yes" (do (println number " number is so big") "no"))
## 循环与loop/recur
> 在Clojure中，loop就是流程控制的瑞士军刀

1. usage: (loop [bindings *] exprs*)
2. 说明：loop与let的工作方式很像，首先建立绑定bindings，然后对exprs求值。特殊点是，loop设置了一个循环点(recursion point)，随后这个循环点将成为特殊形式*recur*返回目标。
3. *recur*-usage: (recur exprs*)
4. recur说明：早先有loop建立的绑定，**会被recur重新绑定为新的值，并且控制程序返回到loop的顶端**。
5. 常见的循环形式：
    - 待更新...

## for
> Clojure的for用于序列解析，而不是循环。

## 元数据
> 定义：在Clojure中，元数据是与一个对象逻辑上的值产生正交的那些数据。

|元数据键|用途|
|:-------|:---|
|:ns       |命名空间               |
|:name     |本地名称               |
|:file     |源文件名               |
|:line     |源代码行号             |
|:arglists |被*doc*用到的参数信息  |
|:tag      |预期的参数或返回值类型 |
|:doc      |被*doc*用到的文档      |
|:macro(宏)|true表示这是一个宏     |


