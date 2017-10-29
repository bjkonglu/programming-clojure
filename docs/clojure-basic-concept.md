# Basic concept

## Forms
>下面展示Clojure主要的数据类型，在Clojure中被叫做*形式*。

|form|example|describe|
|:----|:-------|:----|
|Boolean  |true, false               |true为真，false为假；除了false,在进行布尔求值的上下文中，nil也为假；除了false和nil，其他任何东西在布尔求值的上下文中为真|
|Character|\a                        |--|
|Keyword  |:tag, :doc                |关键字是一种相当常见的键类型|
|List     |(1,2,3), (println, "foo") |--|
|Map      |{:name "Bill", :age 42}   |映射表是一种由键值对组成的容器，使用一对花括号作为其字面表示法|
|Nil      |nil                       |--|
|Number   |1, 4.2                    |--|
|Set      |#{:snap :crackle :pop}    |--|
|String   |"hello world"             |Clojure字符串就是|
|Symbol   |user/foo, java.lang.String|符号可以用来命名各式各样的东西：函数，操作符，Java类，命令空间和Java包，数据结构和引用类型|
|Vector   |[1 2 3]                   |--|

### 特殊用法
1. 谓词：谓词是一种返回true和false的函数,例如true?,false?,nil?等 -> 布尔值与nil
2. 记录：如果有好几个映射表都拥有相同的键，那么可以用*defrecord*创建一种*记录类型*，对一事实加以描述。usage: (*defrecord* *name* [arguments])

## 读取器宏
>定义：读取器负责读入Clojure形式，并将其*文本*转换为*Clojure数据结构*。所谓*读取器宏*，是指由*宏字符*前缀触发的特殊读取器行为。

|读取器宏|示例|描述|
|:-------------|:---|:---|
|注释          |；单行注释                     |--|
|匿名函数      |#(.toUpperCase %)              |采用隐式参数名称，更简洁的匿名函数语法，其中参数被命名为%1、%2，依次类推。对于第一个参数可以使用%表示|
|正则表达式    |#"\w+"                         |--|
|引述          |'form => (quote form)          |引号(')可以阻止求值|
|解引用(deref) |@form => (deref form)          |--|
|元数据        |^metadata form                 |--|
|变量引述      |#'x => (var x)                 |--|

## 函数
>定义：所谓*函数调用*，在Clojure中只不过是列表的*起始元素*可以被解析成函数。

### USAGE
1. (defn name doc-string? attr-map? [params*] body)
2. (defn name doc-string? attr-map? ([params*] body)+)
3. (defn name doc-string? ([params*] body)+ atrr-map?)
    - [attr-map](clojure-flow-control.md)：表示关联到函数对象上的元数据。
    - 2中表示函数可以接受多组参数列表和函数主题。
    - 参数列表中包含一个*&*号，可以创建一个具有可变元素的函数。Clojure会把所有剩余的参数放进一个序列中，并绑定到&号后面的那个名称上。

### 匿名函数
>除了能用defn来创建具名函数以外，还可以使用*fn*创建*匿名函数*

usage: (*fn* [params*] body)
1. 隐式参数名称：采用隐式参数名称，更简洁的匿名函数语法，其中参数被命名为%1、%2，依次类推。对于第一个参数可以使用%表示
    - usage: #(body)

## 变量、绑定和命名空间
> 当定义一个对象时，这个对象会被存储在一个Clojure变量(var)中。

1. *let*的作用就是建立一组词法绑定
    - usage: (let [bindings*] exprs*)
    - 说明: 这里的bindings会在随后的exprs中生效，此外，exprs中最后一个表达式的值，就会成为let的返回值。
2. *解构*:在任意一个需要绑定名称的位置，你都可以在*绑定式*中嵌入一个向量或者映射表，借此深入容器内部。
    - examples: (defn greet-author [{fname :first-name}] (println "Hello, " fname)), (let [[x y] [1 2 3]] [x y])
    - 在*解构*表达式内部，*:as*字句允许我们绑定真个封闭结构
3. 命名空间：
    - *in-ns*用来切换命名空间，必要时Clojure会创建一个新的，usage: (in-ns name)
    - *import*把一个或者多个类名从Java包映射到当前的命名空间中(注意：import仅用于Java类)，usage: (import '(package Class+))
4. **惯例**: 在一个Clojure源文件的顶部，我们会使用*ns*宏来*import* Java类和*require*命名空间。
    - usage: (ns name& reference) 其中reference部分则可以包含:import、:require和:use
    - example: (ns example.exploring (:require [clojure.string :as str]) (:import (java.io.File)))
## 调用Java
> Clojure提供了简单、直接的语法用来调用Java代码，包含：*创建对象*、*调用方法*、访问*静态方法*和*字段*。

1. *创建对象*：
    - usage: (new classname)
2. *调用方法*：
    - usage: (. class-or-instance member-symbol & args) | (. class-or-instance (member-symbol & args))
3. *导入Java类*：
    - usage: (import [& import-lists]) 
    - 说明：import-list => (package-symbol & class-name-symbols)
    - example: (import '(java.util Random) '(java.text MessageFormat))

