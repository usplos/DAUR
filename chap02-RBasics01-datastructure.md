## R 数据结构

R 的基础数据结构包括向量（Vector）、矩阵（Matrix）、数组（Array）、数据框（DataFrame）和列表（List），根据数据元素是否异质（是否可以包含不同类型的数据）及其存储方式，可对之做下表格分类。

|        | 向量 | 矩阵 | 数组 | 数据框 | 列表|
|:------:|:---:|:----:|:----:|:-----:|:---:|
| 数据类型 | 同质 | 同质 | 同质 | 异质 | 异质|
| 存储方式 | 一维 | 二维 | 多维 | 二维 | 一维 |

下面进行具体介绍。

### 向量 Vector

向量是 R 中最简单的数据结构。用来存储数值型（numeric）、字符型（character）或逻辑型（logical）数据，且一个向量只能存储一种类型的数据。向量使用`c()`函数进行创建，其中`c`表示 concatenate（联结、串联），或者 combine。R 中不存在“标量”（scalar），单个数字或字符串等所谓的标量其实是只含一个元素的向量。

```{r}
vector_num <- c(1, 2, 3)                   # 创建数值型向量 vector_num
vector_char <- c("one", "two", "three")    # 创建字符型向量 vector_char
vector_log <- c(T, TRUE, F, FALSE)         # 创建逻辑型向量 vector_log
```
其中：

- `T`与`F`是逻辑型数据`TRUE`与`FALSE`的缩写。多数情况下，`T`与`TRUE`、`F`与`FALSE`分别等价，故定义变量名时尽量不要使用这两个大写字母。从规范性出发，尽量不要用缩写表示逻辑型数据。

- `c(1, 2, 3)`与`c(1:3)`等价。还可使用类似`vector_num <- 1:5`的懒人命令，但更推荐规范的创建方式。

使用`vector_name[]`的形式可调用向量中的元素（element），`[]`中填入表示元素位置的整数。

```{r}
vector_char <- c("one", "two", "three")
vector_char[2]
vector_char[2:3]
vector_char[c(1, 3)]
```
以上三种方式分别调用`vector_char`向量的第2个元素、第2到第3个元素、第1和第3个元素。

在 R 中，向量的长度（length）是指其所包含的元素个数，这可用函数`length()`判定。

```{r}
length(vector_char)
```

可使用`is.numeric()`、`is.character()`、`is.logical()`命令来判断向量是否为指定类型。

```{r}
is.numeric(vector_num)
is.character(vector_chr)
is.logical(vector_log)
is.numeric(vector_chr)
```

要注意的是，所谓的数值型向量其实包含两种类型数据：数型数据（integer）与双精度型（double）。前者只存储整数而不包含小数，后者可存储`2e-308`至`2e+308`之间的实数，其中`E`表示科学计数法，`2E-308`表示 $2 \times -10^308$，`2E+308`表示 $2 \times 10^308$。在 R 中，用`typeof()`函数可判定对象的类型，而`mode()`函数可判定对象的模式。两者之间存在微妙的区别，试观察如下结果。

```{r}
x <- c(1:5)
typeof(x)
mode(x)
y <- x / 5
typeof(y)
mode(y)
```

向量是 R 数据结构的基础。

### 矩阵 Matrix

矩阵是仅包含同质数据的二维数据结构，可以简单理解为许多同类型、等长度向量的组合。矩阵使用`matrix()`函数进行创建。
```r
> matrix_01 <- matrix(1:6, nrow = 2, ncol = 3)
# 创建一个2行3列的矩阵，默认按列填充
> matrix_01
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

> matrix_02 <- matrix(1:6, nrow = 2, ncol = 3, byrow = TRUE)
# 创建按行填充的矩阵
> matrix_02
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6

> cells <- c(1:6)
> rnames <- c("R1", "R2", "R3")
> cnames <- c("C1", "C2")
> matrix_03 <- matrix(cells, nrow = 3, ncol = 2, byrow = TRUE, dimnames = list(rnames, cnames))
# 创建按行填充、3行2列、行名为R1-3、列名为C1-2的矩阵
> matrix_03   
   C1 C2
R1  1  2
R2  3  4
R3  5  6

> vector_mat <- c(1, 3, 5, 7, 9, 11)
> dim(vector_mat) <- c(3, 2)
# 通过对向量添加 dim() 属性进行创建。
> vector_ma
     [,1] [,2]
[1,]    1    7
[2,]    3    9
[3,]    5   11
```

矩阵内元素的访问方式与向量类似。
```r
> matrix_04 <- matrix(1:8, nrow = 4)
> matrix_04
     [,1] [,2]
[1,]    1    5
[2,]    2    6
[3,]    3    7
[4,]    4    8
> matrix_04[2,]
[1] 2 6      # 访问第2行的元素
> matrix_04[,1]
[1] 1 2 3 4  # 访问第1列的元素
> matrix_04[4,2]
[1] 8        # 访问第4行第2个的元素
> matrix_04[c(1, 3),2]
[1] 5 7      # 访问第1和第3行的第2个元素
```

### 数组 Array

数组是矩阵的拓展，同样只能存储同质数据，但可有2个以上维度。数组使用`array()`函数创建。
```r
# 创建3x2x2、各维度各有命名的数组，并填充数字1到12
> dim1 <- c("a1","a2","a3")
> dim2 <- c("b1","b2")
> dim3 <- c("c1","c2")
> array_01 <- array(1:12, c(3, 2, 2), dimnames = list(dim1, dim2, dim3))
> array_01
, , c1
   b1 b2
a1  1  4
a2  2  5
a3  3  6

, , c2
   b1 b2
a1  7 10
a2  8 11
a3  9 12
```

```r
# 通过对向量添加 dim() 属性来创建
> vector_arr <- c(1, 3, 5, 7, 9, 11, 13, 15)
> dim(vector_arr) <- c(2, 2, 2)
> vector_arr
, , 1
     [,1] [,2]
[1,]    1    5
[2,]    3    7

, , 
     [,1] [,2]
[1,]    9   13
[2,]   11   15
```

数组中元素的访问方式跟矩阵相同。

### 数据框 DataFrame

数据框是 R 里应用最多的数据结构，能存储不同类型的数据，可视为许多等长度向量的组合，亦可视为解除了存储数据类型限制的矩阵。数据框使用`data.frame()`函数创建。

```r
# 创建一个学生信息数据框
> studentId <- c(1:4)
> stu_names <- c("a001", "a002", "a003", "a007")
> is_boy <- c(T, TRUE, F, FALSE)
> studentdata <- data.frame(studentId, stu_names, is_boy)
> studentdata
  studentId stu_names is_boy
1         1      a001   TRUE
2         2      a002   TRUE
3         3      a003  FALSE
4         4      a007  FALSE
```
当两个数据框行数相同时，可使用`cbind()`进行合并；当两个数据框列数相同且列名一一对应时，可使用`rbind()`进行合并。
```r
> cbind(studentdata, data.frame(age = c(18, 18, 17, 16)))
  studentId stu_names is_boy age
1         1      a001   TRUE  18
2         2      a002   TRUE  18
3         3      a003  FALSE  17
4         4      a007  FALSE  16
```
```r
> rbind(studentdata, data.frame(studentId = 5, stu_names = "a008", is_boy = T))
  studentId stu_names is_boy
1         1      a001   TRUE
2         2      a002   TRUE
3         3      a003  FALSE
4         4      a007  FALSE
5         5      a008   TRUE
```
上面两个例子是临时创建了两个未命名的数据框，将其合并到了已有数据框`studentdata`中。在数据框内，列表示变量，行表示观测，刚刚的合并分别是增添了一个变量和一组观测。

使用`dataframe_name[col_number]`的形式来读取数据框的元素。
```r
> studentdata[2:3]
# 读取第2到3列
  stu_names is_boy
1      a001   TRUE
2      a002   TRUE
3      a003  FALSE
4      a007  FALSE
> studentdata$studentId
# 利用 $ 符号直接选定指定列
[1] 1 2 3 4
```
此外使用绑定函数`attach()`和解绑函数`detach()`可以让命令变得简单。绑定后可省略数据框名进行框内数据访问。
```r
> attach(studentdata)
# 绑定函数 studentdata
> studentId
[1] 1 2 3 4
> detach(studentdata)
# 解除函数 studentdata 的绑定
> studentID
Error: object 'studentID' not found
```

### 列表 List

列表是 R 中最复杂的数据类型，可包含任何类型的数据，包括向量、矩阵、数组、数据框和其他列表。列表使用`list()`函数创建，并可以为列表内项目命名。
```r
> a <- c(1:3)
> b <- matrix(1:6, c(2, 3))
> c <- array(1:12, c(2, 2, 3))
> d <- studentdata
# 分别创建向量、矩阵、数组和数据框
> list_01 <- list(name_01 = a, name_02 = b, name_03 = c, name_04 = d)
# 创建包含向量、矩阵、数组和数据框的列表，并将列表内数据分别命名为 name_01-04
> list_01
$name_01
[1] 1 2 3

$name_02
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

$name_03
, , 1
     [,1] [,2]
[1,]    1    3
[2,]    2    4

, , 2
     [,1] [,2]
[1,]    5    7
[2,]    6    8

, , 3
     [,1] [,2]
[1,]    9   11
[2,]   10   12

$name_04
  studentId stu_names is_boy
1         1      a001   TRUE
2         2      a002   TRUE
3         3      a003  FALSE
4         4      a007  FALSE
```
列表使用`[[]]`（双重方括号）或`$`符号进行数据的读取。
```r
> list_01[[2]]
# 读取列表 list_01 的第2个元素
     [,1] [,2] [,3]
[1,]    1    3    5
[2,]    2    4    6

> list_01[[2]][2]
# 读取列表 list_01 第2个元素的第2项
[1] 2

> list_01[[4]]
# 读取列表 list_01 第4个元素
studentId stu_names is_boy
1         1      a001   TRUE
2         2      a002   TRUE
3         3      a003  FALSE
4         4      a007  FALSE

> list_01[[4]][2]
# 读取列表 list_01 第4个元素的第2列
  stu_names
1      a001
2      a002
3      a003
4      a007

> list_01$name_01
# 读取列表 list_01 内名为 name_01 的元素
[1] 1 2 3
```

### 因子 Factor

因子是建立在整型(integer)向量基础上、只能包含预先定义数值的一种向量，具有 class()^[表示该向量是不同于其他向量的因子]、levels()^[显示因子所有可能的取值] 两种属性。
```r
> factor_01 <- factor(c("boy", "girl" ,"girl", "boy"))
# 创建因子 factor_01，并设置初始值为("boy", "girl" ,"girl", "boy")，内含四个元素
> factor_01
[1] boy  girl girl boy 
Levels: boy girl
```
R 会将因子 factor_01 存储为向量(1, 2, 2, 1)的形式，其中 boy = 1, girl = 2，按字母顺序进行赋值，levels 亦按字母顺序排列。
```r
> class(factor_01)
[1] "factor"
# 读取向量 factor_01 的类型
> levels(factor_01)
[1] "boy"  "girl"
# 读取因子 factor_01 的 levels

> factor_01[3] <- "dog"
Warning message:
In `[<-.factor`(`*tmp*`, 3, value = "dog") :
  invalid factor level, NA generated
# 将因子 factor_01 的第三个元素赋值为 "dog" 失败
> factor_01[3] <- "boy"
> factor_01
[1] boy  girl boy  boy 
Levels: boy girl
# 将因子 factor_01 的第三个元素赋值为 "boy" 成功
```
当因子内数据具有顺序时，使用`ordered = TRUE`指令进行说明。
```r
> factor_02 <- c("huge", "big", "small")
> factor_02 <- factor(factor_02, ordered = T)
> factor_02
[1] huge  big   small
Levels: big < huge < small
```
可以看到`levels`中表示大小的三个单词有了顺序，但此顺序与我们常识不符，需要对 levels 进行指定。
```r
> factor_02 <- factor(factor_02, ordered = T, levels = c("small", "big", "huge"))
> factor_02
[1] huge  big   small
Levels: small < big < huge
```

### tibble

tibble 继承了 data.frame，是用来替换 data.frame 类型的扩展的数据框，语法跟 data.frame 相同，但应用更加方便，可存储任意类型数据。tibble 可使用`tibble()`函数创建。
```r
> library(tibble)  # 加载 tibble 包
> tbl_01 <- tibble(x = 1:5, y = letters[1:5])
                   # 创建 tibble 类型的数据框
> tbl_01
# A tibble: 5 x 2
      x     y
  <int> <chr>
1     1     a
2     2     b
3     3     c
4     4     d
5     5     e
```
可以看到 tibble 类型的数据框列名下标示出了此列数据类型的缩写^[tibble 定义了7种数据类型的缩写：int(整型)、dbl(双精度)、chr(字符型)、dttm(日期 + 时间)、lgl(逻辑型)、fctr(因子)、date(日期)]。

```r
# 查看 tbl_01 的属性
> attributes(tbl_01)
$names      # 列名
[1] "x" "y"

$class      # 类型
[1] "tbl_df"     "tbl"        "data.frame"

$row.names  # 行名
[1] 1 2 3 4 5
```
由于 tibble 是一个新的数据类型，应当注意 tibble 与其他原有类型之间的转换，转型函数：`as.tibble()`。此外 tibble 类型与传统数据框还有一些区别，若感兴趣可自行搜索了解。

### R中的子集选取运算符(Subsetting operators in R)

R中的子集选区运算符主要包括`[ ]`，`[[ ]]`，`$`三种。其中`[ ]` 和 `[[ ]]` 相似，但是前者返回的是列表，后者返回的是列表中的内容。`$` 运算符与选定的字符串一起就成为 `[[ ]]` 的缩写。

#### `[ ]` 和 `[[ ]]`

如果只想获取对象内的内容而不想让内容再以相同的对象类型输出时，应该使用 `[[ ]]`，这是因为当时用 `[ ]` 时，总是与原来相同的数据类型输出。要获取内容，就需要使用 `[[ ]]`.

有这样一句话可以很好地帮助我们理解两者之间的区别。

"如果把列表 `x` 看成装载对象（货物）的火车，那 `x[[5]]` 就是5号车厢中的对象（货物），而 `x[4:6]` 就是火车的第4-6号车厢"
--@RLangTip

`[[ ]]`必须和一个正整数或者字符串一起使用才能达到返回内容的目的。比如下面这两种都是返回`mtcars`的第一列的内容。

```{r}
mtcars[[1]]
mtcars[["mpg"]]
```

如果我们用`[ ]`的方式提取第一列，可以看到提取的结构仍然保持原来的数据结构，仍为一个数据框。
```{r}
mtcars[1]
```

我们可以进一步考察他们产生的类型，会发现，用`[ ]`选取的方式选出的仍为`list`，用`[[ ]]`的方式选取，变成了`double`类型的变量了，也就是成了一个原子向量了。

```{r}
typeof(mtcars[1])
typeof(mtcars[[1]])
```

#### 简化与保留

在子集选取时，一定要注意最后产生的是什么类型的数据，是否仍保持着原来的数据结构，因为在像'read.csv'等函数中，输入的数据类型有一定的限制，输入矩阵或者数据框类型会出现错误，而对于另一些函数，输入类型不能是原子向量而是矩阵或者数据框，因此我们要学会如何保留和简化数据。
在对矩阵和数据框进行子集选取的时候，通常会省略 `drop = FALSE`, 导致很多错误，我们可以看一下这个条件设置不同时数据输出的类型。

```{r}
typeof(mtcars[, 1, drop = FALSE])
typeof(mtcars[, 1, drop = TRUE])
typeof(mtcars[, 1, drop = TRUE]) == typeof(mtcars[[1]])
```

当加上`drop = FALSE` 的声明后，`typeof(mtcars[, 1, drop = TRUE])` 的输出和`typeof(mtcars[[1]])` ，就一样了，仅保留数据的内容。

不同类型的数据的简化和保留是不同的，大致如下：

向量
简化：`a[[1]]`
保留：`a[1]`

列表
简化：`a[[1]]`
保留：`a[1]`

因子
简化：`a[1:3, drop = T]`
保留：`a[1:3]`

数组
简化：`a[1, ]` 或 `a[,1]`
保留：`a[1, ,drop = F]` 或 `a[, 1, drop = T]`

数据框
简化：`a[1, ]` 或 `a[[1]]`
保留：`a[, 1, drop = F]` 或 `a[1]`

不同数据类型的保留的结果会输出相同类型的变量，但是简化的结果不同。

向量：如果给元素赋予了名字，那么简化时名字将会被去掉。

```{r}
student = c(a = "xiaoming", b = "xiaohong")
student[[1]]
```

列表：返回的是列表中的对象，而不是一个列表。

```{r}
student = list(class1 = c("xiaoming", "xiaohong"), class2 = c("daming", "dahua"))
student[[1]]
```

因子：扔掉所有不同的水平。

```{r}
grade = factor(c("excellent", "poor", "bad", "excellent") ,c("excellent", "good", "not bad", "bad", "poor"))
grade[1, drop = T]
```

矩阵或者数组：抛弃所有长度为1的维度。

```{r}
normmatrix = matrix(round(rnorm(16,10,3)), nrow = 4)
normmatrix[2, , drop = F]
normmatrix[2, , drop = T]
```

数据框：如果仅输出一列，返回的是向量而不是数据框

```{r}
mtcars[["mpg"]]
```

#### `$` 运算符

`$` 是一个简写运算符，这里 `mtcars$mpg` 等价于 `mtcars[["mpg", exact = FALSE]]`。通常用来访问数据框中的变量。

使用 `$` 的最常见的错误是，知道数据框某一列的名字，但是这个名字储存在一个变量中，如果 `$` 后面不使用列的名字而是使用变量的名字就会出错。

```{r}
x = "mpg"
mtcars$x
```

`$` 和 `[[ ]]` 之间的不同是，前者是部分匹配。

```{r}
class1 = data.frame(stunumber = c(1:3), stuname = c("xiaoming", "xiaohong", "xiaohua"), stringsAsFactors = F)
class1[["stuname"]]
class1$stuna
```

如果想要避免这类情况发生，可以将全局变量 `warnPartialMatchDollar` 设置为`TRUE`，但是可能影响其他代码的执行，特别是加载了一些包之后。

总的来说，`[ ]` 选取后的数据类型与原数据类型相同，`[[ ]]` 选取后的数据类型会发生变化，它输出的是列表的内容，`$` 是多用于选取和引用数据框中数据的部分匹配的运算符。根据具体的情况（是编程还是分析数据，以及有些函数在输入类型上的限制）选取适合的引用形式，会帮助我们更高效地达到目的。
