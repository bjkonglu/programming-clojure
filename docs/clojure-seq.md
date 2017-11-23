# clojure-seq
> 在Clojure中，*字符串*、*列表*、*向量*、*映射表*、*集合*和*树*这些数据结构都可以通过同一个抽象概念来访问：*序列*

|序号|可序化的容器|
|:---|:-----------|
|1   |所有的Clojure容器    |
|2   |所有的Java容器       |
|3   |Java数组和字符串     |
|4   |正则表达式的匹配结果 |
|5   |目录结构             |
|6   |输入/输出流          |
|7   |xml树                |

## 常见序列操作
|序号|usage|描述|
|:---|:----|:---|
|1   |(first aseq)                  |获取队列的第一个元素                         	 |
|2   |(reset aseq)                  |获取第一个元素后的一切                  		 |
|3   |(cons elem aseq)              |向队列的前端添加元素，来创建一个新的序列		 |
|4   |(seq coll)                    |返回一个序列，该序列源自任务一个可序列化的其他容器  |
|5   |(next aseq)                   |返回一个序列，该序列由除第一个元素的其他元素组成    |
|6   |(sorted-set& elements)        |创建一个有序（自然顺序）的的集合                    |
|7   |(sorted-map& elements)        |创建一个有序的映射表                                |
|8   |conj coll element & elements  |向容器添加一个或者多个元素                          |
|9   |into to-coll from-coll        |将容器中的元素添加到另一个容器                      |

```
注意：rest与next区别：(rest ())->返回()；(next ())->nil
```
## 序列库
> 序列库的函数主要分为四类：1.创建序列的函数；2.过滤序列的函数；3.序列谓词；4.序列转换函数

### 创建序列
|序号|usage|描述|
|:---|:----|:---|
|1   |(range start? end step?)  |范围包含start，不包含end，默认start为0，step为1                                      |
|2   |(repeat n x)              |重复n次元素x,但只有一个参数时，repeat会返回一个惰性的无限序列                        |
|3   |(iterate f x)             |iterate起始于值x,并持续地对每个值应用函数*f*，以便计算下一个值                       |
|4   |(cycle coll)              |函数接受一个容器，并无限的对其进行循环                                               |
|5   |(interleave& colls)       |从每个容器中交错提取元素，直到其中一个容器被耗尽                                     |
|6   |(interpose separator coll)|把输入序列的每个元素用分割符隔开，并作为新的序列返回                                 |
|7   |(join separator sequence) |将输入序列的每个元素用分隔符隔开，并作为字符串打印。相当于interpose和(apply str ...) |
|8   |(list& elements)          |创建列表                                                                             |
|9   |(vector& elements)        |创建向量                                                                             |
|10  |(hash-set& elements)      |创建集合                                                                             |
|11  |(hash-map key-1 val-1 ...)|创建映射表                                                                           |

```
注意：(set coll)：set是接受容器；(vec coll)：vec也是接受容器
```

### 过滤序列
|序号|usage|描述|
|:---|:----|:---|
|1   |(filter pred coll)    |filter接受一个谓词和一个容器作为参数，并返回一个序列。这个序列的所有元素都经过谓词判断为真|
|2   |(take-while pred coll)|从序列中截取开头的一段，其中每个元素都被谓词判定为真，直到谓词判断为假                    |
|3   |(drop-while pred coll)|从序列起始位置开始，逐个丢弃元素，直至谓词判断为假                                        |
|4   |(split-at index coll) |在指定位置将容器一分为二                                                                  |
|5   |(split-with pred coll)|通过谓词，将容器一分为二                                                                  |

### 序列谓词
> 序列谓词会要求其他谓词应如何判定序列中的每一个元素

|序号|usage|描述|
|:---|:----|:---|
|1   |(every? pred coll)     |要求其他谓词对序列中的每个元素都判断为真                                                    |
|2   |(some pred coll)       |只要有一个元素被谓词判定为非假，some就会返回这个值，如果没有任何元素符合，则some返回nil     |
|3   |(not-every? pred coll) |--                                                                                          |
|4   |(not-any? pred coll)   |--                                                                                          |

### 序列转换
>转换函数用于对序列中的值进行转换

|序号|usage|描述|
|:---|:----|:---|
|1   |(map f coll)              |--|
|2   |(reduce f coll)           |--|
|3   |(sort comp? coll)         |--|
|4   |(sort-by a-fn comp? coll) |--|

```
所有的过滤和转换的祖先都是列表解析，Clojure中，使用for宏来进行解析。
Usage: (for [binding-form coll-exps filter-exps? ...] exps)
使用过滤条件的时候,使用子句(:when/:while)
(for [n [1 2 3 4 5] :when (even? n)] n)
```

## Java序列化
> 在Java世界中，包含了以下内容：容器API，正则表达式，文件系统的遍历，XML处理以及关系型数据库结果集

### 序列化Java容器


### 序列化正则表达式
usage: (re-seq regexp string)

### 序列化文件系统
```
Clojure通过file-seq提供了一个深度优化的遍历方式。
```

### 序列化流
```
我们可以使用line-seq将Java的Reader以行的方式进行序列化。clojure.java.io库提供了一个reader函数，能够从流、文件、URL或URI返回一个Reader.
Reader需要被明确关闭，所以创建Reader时，需要包含在一个with-open中。
```

### 序列化XML
```
clojure.xml/parse函数能解析XML文件、流或者URL，并以Clojure映射表的形式，返回数据的树形结构，嵌入在其内部的那些向量代表了子节点。
```

## 调用特定于结构的函数

### 列表函数

