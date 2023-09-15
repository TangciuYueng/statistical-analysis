[TOC]

# 数据类型和操作
## 数据类型
### Logical
长度不等，是倍数就把短的从头重复利用
```R
c(1, 3, 5) > 2
## [1] FALSE  TRUE  TRUE
(1:4) >= (4:1)
## [1] FALSE FALSE  TRUE  TRUE
```

与NA比较产生NA
```R
c(1, NA, 3) > 2
## [1] FALSE    NA  TRUE
NA == NA
## [1] NA
```

检查是否属于利用`%in%`
```R
c(1,3) %in% c(2,3,4)
## [1] FALSE  TRUE
c(NA,3) %in% c(2,3,4)
## [1] FALSE  TRUE
c(1,3) %in% c(NA, 3, 4)
## [1] FALSE  TRUE
c(NA,3) %in% c(NA, 3, 4)
## [1] TRUE TRUE
```

`match(x, y)`也可以检查是否属于，但返回的是对于`x`的每个元素，找到在`y`中**首次**出现的下标
```R
match(c(1, 3), c(2,3,4,3))
## [1] NA  2
```

#### 运算符
- `&&`和`||`是短逻辑,只取操作对象的第一个值运算,具备短路规则,只返回**一个逻辑值或NA**（新版本中好像就不支持对向量使用了
- `&`和`|`是长逻辑,循环遍历的取操作的对象的每一对值运算,直到每一个元素都参与了运算,其返回值长度等于操作**对象长度的最大值**

#### 逻辑运算函数
若`cond`是逻辑向量，用a`ll(cond)`测试`cond`的所有元素为真；用a`ny(cond)`测试`cond`至少一个元素为真。`cond`中允许有缺失值，结果可能为缺失值
```R
c(1, NA, 3) > 2

## 有NA但是如果有其他仍可以返回F/T
## [1] FALSE    NA  TRUE
all(c(1, NA, 3) > 2)
## [1] FALSE
any(c(1, NA, 3) > 2)

## [1] TRUE
all(NA)
## [1] NA
any(NA)
## [1] NA
```

函数`which()`返回**真值对应的所有下标**
```R
which(c(FALSE, TRUE, TRUE, FALSE, NA))
## [1] 2 3
which((11:15) > 12)
## [1] 3 4 5
```

函数`identical(x,y)`比较两个R对象的**内容**是否完全相同，结果只会取标量TRUE与FALSE两种
```R
identical(c(1,2,3), c(1,2,NA))
## [1] FALSE
identical(c(1L,2L,3L), c(1,2,3))
## [1] FALSE
```

如果不区分数据类型，可以使用`all.equal()`
```R
all.equal(c(1,2,3), c(1,2,NA))
## [1] "'is.NA' value mismatch: 1 in current 0 in target"
all.equal(c(1L,2L,3L), c(1,2,3))
## [1] TRUE
```

函数`duplicated()`返回每个元素是**否为重复值**的结果
```R
duplicated(c(1,2,1,3,NA,4,NA))
## [1] FALSE FALSE  TRUE FALSE FALSE FALSE  TRUE
```

函数`unique()`可以返回去掉重复值的结果
```R
a<-c(1,3,2,1,4,2,1,3,1,23,45,612)
unique(a) 
## [1]   1   3   2   4  23  45 612
```

### Integer
### Numeric
### Character
转义字符`"\(\"\n`
如果懒得写，可以的情况下使用原始字符串raw string：`r"(...)"`，其中`...`是实际内容
```R
r"(adfa())"
## [1] "adfa()"
r"--(-)_(adfa())--"
#* [1] "-)_(adfa()"
```
#### nchar()
统计字符串长度，返回一个整数向量
```R
# 单个字符串的字符数
text <- "Hello, World!"
charCount <- nchar(text)
print(charCount) 
# 输出: 13

# 字符向量的字符数
texts <- c("Hello", "World", "R programming")
charCounts <- nchar(texts)
print(charCounts)
# 输出: 5 5 14
```
#### substring()
提取子字符串，基本语法`substring(x, first, last)`
`first` 和 `last` 都是数字，则表示提取字符串中的一个连续子字符串
```R
string <- "Hello, world!"
substring(string, 1, 5)  # 提取第1到第5个字符，结果是 "Hello"
substring(string, 8, 13) # 提取第8到第13个字符，结果是 "world!"
```
`first` 和 `last` 都是向量，则表示提取多个非连续的子字符串，起始位置和结束位置由对应的向量元素确定，因此输入向量长度应该相等
```R
substring(string, c(1, 8), c(5, 13))  # 提取第1到第5个字符和第8到第13个字符，结果是 "Hello" 和 "world!"
```
#### paste()
用于将多个对象或文本连接成一个字符串
基本语法`paste(..., sep = " ", collapse = NULL)`
参数说明: 
- `...`: 表示要连接的对象或文本。可以是字符向量、数字、逻辑值或其他可转换为字符的对象。
- `sep`: 可选参数，用于指定连接每个对象时要使用的分隔符。默认值为一个空格。
- `collapse`: 可选参数，用于指定在连接多个对象时是否要应用额外的折叠操作。如果设置为 `NULL`（默认值），则不进行折叠操作。
```R
# 例1: 简单连接
paste("Hello", "World")  # 返回 "Hello World"

# 例2: 指定分隔符
paste("a", "b", "c", sep = "-")  # 返回 "a-b-c"

# 例3: 连接向量
vec <- c("a", "b", "c")
paste(vec, collapse = "-")  # 返回 "a-b-c"

# 例4: 连接数字和字符
num <- 123
paste("Number:", num)  # 返回 "Number: 123"

# 例5: 连接逻辑值
logic <- TRUE
paste("Value is", logic)  # 返回 "Value is TRUE"
```
#### 模式匹配
##### regexpr()
在字符串中搜索模式
基本语法`regexpr(pattern, text, ignore.case = FALSE, perl = FALSE, fixed = FALSE)`
参数说明: 
- `pattern`: 要搜索的模式（正则表达式）。
- `text`: 要在其中进行搜索的字符串。
- `ignore.case`: 是否忽略大小写，默认为`FALSE`，表示区分大小写。
- `perl`: 是否使用Perl兼容的正则表达式语法，默认为`FALSE`。
- `fixed`: 是否将pattern作为固定字符串进行匹配，默认为FALSE

注意只返回第一个匹配的位置，如果要搜所所有的位置可以使用`gregexpr`函数
```R
text <- "The cat is cute."
pattern <- "c."

result <- regexpr(pattern, text)

# 输出结果
# [1] 5
# attr(,"match.start")
# [1] 5
# attr(,"match.length")
# [1] 2
# attr(,"useBytes")
# [1] TRUE

# 可以用这样的形式查看
attr(res, "match.length")

# 利用正负判断有没有匹配 没有匹配就是-1
res >= TRUE
```
返回值怎么用还是看`rematches`函数
##### regmatches()
用于提取正则表达式匹配结果的函数。它可以根据正则表达式的匹配位置，从字符向量或字符串中提取出匹配的子串。

基本语法`regmatches(x, m, invert = FALSE)`
参数说明: 
- `x`: 一个字符向量或字符串，表示要从中提取匹配结果的源数据。
- `m`: 一个匹配位置的整数向量，表示匹配结果的位置。可以通过`regexpr`或`gregexpr`函数获得。
- `invert`: 逻辑值，表示是否提取与给定匹配位置不相交的子串。默认为`FALSE`，表示提起与匹配位置相交的子串。

返回值: 
- 一个字符串或字符向量

```R
text <- "Hello, my name is Alice."
pattern <- "\\b[a-zA-Z]+\\b"

result <- regexpr(pattern, text)
matched_substring <- regmatches(text, result)

# 输出结果
# [1] "Hello" "my"    "name"  "is"
```

注意，`\b`是一个**单词边界**，表示一个位置，即在该位置之前或之后，存在一个单词字符和一个非单词字符（或字符串的开头/结尾）。
##### grep()
在字符**向量**中搜索
基本功能`grep(pattern, x, ignore.case = FALSE, perl = FALSE, value = FALSE, fixed = FALSE, ...)`

参数说明:
- `pattern`: 指定要匹配的模式，可以是一个正则表达式或普通的字符向量。
- `x`: 要搜索的字符向量。
- `ignore.case`: 可选参数，表示是否在匹配模式时忽略大小写。默认值为 `FALSE`。
- `perl`: 可选参数，表示是否使用Perl风格的正则表达式。默认值为 `FALSE`。
- `fixed`: 可选参数，表示 `pattern` 是否是一个固定的字符串，而不是正则表达式。默认值为 `FALSE`。当 `pattern` 为 `TRUE` 时候，`.`/`*`/`+`等字符视为字面字符，而不会被解释为正则表达式的特殊符号。
- `value`是否返回匹配的值而不是位置，默认为`FLASE`。如果为`TRUE`，则返回匹配的值。

```R
# 创建一个字符向量
x <- c("apple", "banana", "orange", "grape", "mango")

# 在字符向量中搜索匹配的模式，返回匹配的位置
grep("an", x)
# 输出: 2 4

# 在字符向量中搜索匹配的模式，返回匹配的值
grep("an", x, value = TRUE)
# 输出: [1] "banana" "mango"

# 忽略大小写，返回匹配的值
grep("o", x, ignore.case = TRUE, value = TRUE)
# 输出: [1] "orange" "mango"

# 将模式视为字面字符串，返回匹配的位置
grep(".", x, fixed = TRUE)
# 输出: 1 2 3 4 5
```
##### glob2rx()
用于将`glob`模式转换为正则表达式
基本语法`glob2rx(glob_pattern)`

```R
# 转换glob模式为正则表达式
glob_pattern <- "*.txt"
regex_pattern <- glob2rx(glob_pattern)
print(regex_pattern)
# 输出: "^.*\\.txt$"

# 在字符向量中查找匹配的正则表达式
x <- c("file1.txt", "file2.jpg", "file3.txt", "file4.csv")
matches <- grep(regex_pattern, x, value = TRUE)
print(matches)
# 输出: [1] "file1.txt" "file3.txt"

# 另一个示例: 转换glob模式为正则表达式
glob_pattern2 <- "file[0-9]?\\.csv"
regex_pattern2 <- glob2rx(glob_pattern2)
print(regex_pattern2)
# 输出: "^file[0-9]\\..csv$"
```
Glob模式是一种用于匹配文件路径或文件名的简单模式匹配语法。它通常在命令行、脚本和编程语言中使用，用于筛选和匹配符合模式的文件或路径。

Glob模式使用通配符字符和特殊字符来表示文件名或路径的模式。以下是Glob模式中最常用的通配符字符: 

- `*`: 匹配任意数量的字符（包括零个字符）。
- `?`: 匹配一个字符。
- `[...]`: 匹配指定范围内的字符。例如，`[0-9]`匹配任意一个数字字符。
- `[^...]`: 匹配不在指定范围内的字符。例如，`[^0-9]`匹配任意一个非数字字符。

除了通配符字符外，Glob模式还可以包含普通字符和特殊字符（如转义字符）来进行更精确的匹配。

以下是一些示例来说明Glob模式的使用: 

- `*.txt`: 匹配所有以`.txt`结尾的文件。
- `file?.csv`: 匹配文件名为`fileX.csv`形式的文件，其中`X`是任意一个字符。
- `[aeiou]*.jpg`: 匹配所有以元音字母开头，并且以`.jpg`结尾的文件。
- `file[0-9][0-9]?.txt`: 匹配文件名为`fileX.txt`或`fileXX.txt`形式的文件，其中`X`是一个数字。
##### gsub()
用于替换字符串中的模式的函数
基本语法`gsub(pattern, replacement, x, ignore.case = FALSE, perl = FALSE, fixed = FALSE, useBytes = FALSE)`
参数说明: 
- `pattern`: 指定要匹配的模式，可以是一个正则表达式或普通的字符向量。
- `replacement`: 指定要替换模式匹配的部分的内容。
- `x`: 要在其中进行替换的字符向量或字符串.
- `ignore.case`: 可选参数，表示是否在匹配模式时忽略大小写。默认值为 `FALSE`。
- `perl`: 可选参数，表示是否使用Perl风格的正则表达式。默认值为 `FALSE`。
- `fixed`: 可选参数，表示 `pattern` 是否是一个固定的字符串，而不是正则表达式。默认值为 `FALSE`。当 `pattern` 为 `TRUE` 时候，`.`/`*`/`+`等字符视为字面字符，而不会被解释为正则表达式的特殊符号。
- `useBytes`: 可选参数，表示是否按字节处理字符串，而不是字符。默认值为 `FALSE`。

```R
# 例1: 替换字符串中的模式
text <- "Hello, Hello, Hello"
result <- gsub("Hello", "Hi", text)
result  # 返回 "Hi, Hi, Hi"

# 例2: 忽略大小写匹配
text <- "Hello, hello, HELLO"
result <- gsub("hello", "Hi", text, ignore.case = TRUE)
result  # 返回 "Hi, Hi, Hi"

# 例3: 使用正则表达式进行替换
text <- "abc123xyz456"
result <- gsub("[0-9]", "*", text)
result  # 返回 "abc***xyz***"

# 例4: 替换固定字符串
text <- "The cat is cute."
pattern <- "c."
result <- gsub(pattern, "XX", text, fixed = TRUE)
result  # 返回 "The XXt is cute."
result <- gsub(pattern, "XX", text, fixed = FALSE)
result  # 返回 "The XX XX XXte."

# 例5: 按字节处理字符串
text <- "你好，世界"
result <- gsub("世界", "World", text, useBytes = TRUE)
result  # 返回 "你好，World"
```
#### strsplit()
将字符串或字符向量按照指定的分隔符进行拆分
基本语法`strsplit(x, split, fixed = FALSE, perl = FALSE, useBytes = FALSE)`
参数说明: 
- `x`: 要拆分的源字符串。
- `split`: 指定的分隔符，可以是一个字符向量（如单个字符或多个字符的组合）。
- `fixed`: 一个逻辑值，用于指示是否使用固定的字符串匹配。如果为TRUE，则将分隔符视为固定字符串；如果为FALSE（默认值），则将分隔符视为正则表达式模式。
- `perl`: 一个逻辑值，用于指示是否启用perl兼容的正则表达式。如果为TRUE，则启用perl模式匹配；如果为`FALSE`（默认值），则使用R的基本正则表达式。此参数仅在`fixed = FALSE`时有效。
- `useBytes`: 一个逻辑值，用于指示是否按字节拆分字符串。默认为`FALSE`，表示按字符拆分。

```R
strsplit(c("aa-aa", "bb-bb", "dae--f"), split="-")

# 输出结果
# [[1]]
# [1] "aa" "aa"

# [[2]]
# [1] "bb" "bb"

# [[3]]
# [1] "dae" "" "f"  
```
#### chartr()
用于进行字符串替换。它用指定为参数的新字符替换字符串中现有字符的所有匹配项
基本语法`chartr(old, new, x)`
参数说明: 
- `old`: 要替换的旧字符串。
- `new`: 新字符串。
- `x`: 目标字符串。

```R
sentence <- "Hello, World!"
new_sentence <- chartr("o", "0", sentence)
print(new_sentence)
```

Output:
```
Hell0, W0rld!
```
### Factor
在R语言中，`factor`是一种用于表示分类变量的数据类型。它在数据分析和统计建模中非常有用，用于表示具有**有限数量的离散取值**的变量。

`factor`对象由两个主要部分组成: 级别（levels）和值（values）。级别是变量可能的取值，而值是实际观察到的变量的取值。

基本语法`factor(x, levels, labels, ordered, ...)`

参数说明: 
- ordered: 设置是否对因子水平排序，默认 FALSE 为无序因子, TRUE 为有序因子；用ordered函数令他具有顺序，如果使用了这个函数，那么因子当中首先出现的level将小于后出现的。
- exclude：排除的字符。从x中剔除的水平值，默认为NA值。
- nmax：水平的上限数量。

**注意不能将factor直接当作字符型操作**

以下是创建和使用`factor`对象的示例: 

```R
color_factor <- factor(c("red", "blue", "white", "red"))

# 查看factor对象
color_factor
# 查看factor对象的级别
levels(color_factor)
```

输出结果: 
```
> 
[1] red   blue  white red  
Levels: blue red white

[1] "blue"  "green" "red"  
```

此外，`factor`对象还具有属性，比如`labels`和`levels`。`labels`属性是级别的可读标签，而`levels`属性是级别的**内部表示**。您可以使用`levels()`和`labels()`函数来访问和修改这些属性。

例如: 
```R
# 创建一个factor对象并自定义级别标签
gender <- factor(c("male", "female", "male", "male"), levels = c("male", "female"), labels = c("M", "F"))

# 查看factor
gender
# 查看factor对象的级别和标签
levels(gender)
labels(gender)
```

输出结果: 
```
[1] M F M M
Levels: M F

[1] "male"   "female"
[1] "M" "F" 
```
#### cut()
`cut()` 是 R 语言中用于创建离散化变量的函数。它允许将连续的数值变量划分为多个离散的区间，并将每个观测值映射到对应的区间。

`cut()` 的语法如下: 

```R
cut(x, breaks, labels = NULL, include.lowest = FALSE, right = TRUE, dig.lab=3, ordered_result=FALSE,...)
```

其中，参数的含义如下: 

- `x`: 要划分的数值向量。
- `breaks`: 指定用于划分的区间。它可以是一个标量，表示将 x 划分为的等距区间的数量；也可以是一个包含分割点的向量，表示明确的区间边界。
- `labels`: 可选参数，用于指定划分后的区间的标签。如果不提供标签，则默认使用每个区间的边界作为标签。
- `include.lowest`: 逻辑值，指示是否将最小值包含在最低区间内。
- `right`: 逻辑值，指示是否将观测值划分为右闭合的区间（默认）或左闭合的区间。
- `dig.lab`：一个整数，用于指定标签的小数位数。
- `ordered_result`：一个逻辑值，表示输出的分组是否被视为有序。

以下是一个使用 `cut()` 的示例: 

```R
# 创建一个数值向量
grades <- c(85, 72, 93, 67, 78, 89, 80, 65)

# 将数值向量划分为三个等距区间
cut_grades <- cut(grades, breaks = 3)
cut2 <- cut(grades, breaks=3, labels=c("low", "mid", "high"))

# 输出划分后的区间
cut_grades
cut2
```

输出: 

```
[1] (83.7,93]   (65,74.3]   (83.7,93]   (65,74.3]   (74.3,83.7]
[6] (83.7,93]   (74.3,83.7] (65,74.3]  
Levels: (65,74.3] (74.3,83.7] (83.7,93]

[1] high low  high low  mid  high mid  low 
Levels: low mid high
```

返回值是按照原来数组顺序，分成每个元素对应哪个区间

### Date & Times

## 特殊constants
1. `NA`：表示缺失值（missing value）。用于表示数据中的缺失或未知值。

2. `NaN`：表示非数值（Not a Number）。用于表示一个数值无法被定义或计算的情况，比如0/0的结果。

3. `Inf`：表示正无穷大（positive infinity）。用于表示一个数大到无法用浮点数表示的情况，例如1/0的结果。

4. `-Inf`：表示负无穷大（negative infinity）。用于表示一个数小到无法用浮点数表示的情况，例如-1/0的结果。

5. `NULL`: 表示不存在的数字，注意与`NA`区分，后者是存在单位知。

## 操作
不支持自动类型转换
### Math operation
- `+`addition
- `-`subtraction
- `*`multiplication
- `/`division
- `^` or `**`exponentiation
- x`%%`y modules(x mod y)
- x`%/%`y integer division
### Relational operation
- `==` Equals
- `!=` Does not equal
- `>` Greater than
- `>=`
- `<`
- `<=`
- `%in%` Included in
- `is.na()` Is a missing value 检测是否是`NA`
### Regular operation
1. `length(object)`: 显示对象中元素/成分的数量。

示例：
```R
vector <- c(1, 2, 3, 4, 5)
length(vector)  # 返回 5
```

2. `dim(object)`: 显示对象的维度，如矩阵或数组的维数。

示例：
```R
matrix <- matrix(1:4, nrow = 2, ncol = 2)
dim(matrix)  # 返回 2 2
```

3. `str(object)`: 显示对象的结构，提供有关类型、长度和内容的信息。

示例：
```R
vector <- c("apple", "banana", "orange")
str(vector)  # 显示 'chr [1:3] "apple" "banana" "orange"'
```

4. `class(object)`: 显示对象的类别。

示例：
```R
number <- 123
class(number)  # 返回 'numeric'
```

5. `mode(object)`: 显示对象的模式/存储方式。

示例：
```R
vector <- c(1, 2, 3, 4, 5)
mode(vector)  # 返回 'numeric'
```

6. `names(object)`: 显示对象各成分的名称。

示例：
```R
vector <- c(a = 1, b = 2, c = 3)
names(vector)  # 返回 'a', 'b', 'c'
```

7. `c(object, object, ...)`: 将多个对象合并成一个向量。

示例：
```R
vector1 <- c(1, 2, 3)
vector2 <- c(4, 5, 6)
combined_vector <- c(vector1, vector2)  # 返回 1, 2, 3, 4, 5, 6
```

8. `cbind(object, object, ...)`: 按列合并对象，创建一个矩阵或数据框。

示例：
```R
vector1 <- c(1, 2, 3)
vector2 <- c(4, 5, 6)
combined_matrix <- cbind(vector1, vector2)  # 返回一个 3x2 的矩阵
```

9. `rbind(object, object, ...)`: 按行合并对象，创建一个矩阵或数据框。

示例：
```R
vector1 <- c(1, 2, 3)
vector2 <- c(4, 5, 6)
combined_matrix <- rbind(vector1, vector2)  # 返回一个 2x3 的矩阵
```
### Convert operation

| 函数 | from vector | from matrix | from data frame |
|------|-------------|-------------|----------------|
| to one long vector | `c(x, y)` | `as.vector(mymatrix)` | - |
| To matrix | `cbind(x,y) rbind(x,y)` | - | `as.matrix(mydataframe)` |
| To data frame | `data.frame(x,y)` | `as.data.frame(mymatrix)` | - |

# Control flow
1. 条件语句（Conditional Statements）：
   - `if-else`语句：根据给定条件的真假执行不同的代码块**特别注意else必须跟在第一个大括号后面，否则报错**。
   - `switch`语句：根据给定表达式匹配不同的案例执行相应的代码块。

2. 循环语句（Loop Statements）：
   - `for`循环：用于重复执行一段代码，所需迭代次数已知。
   - `while`循环：在给定条件为真时重复执行一段代码，所需迭代次数未知。
   - `repeat`循环：创建一个无限循环，需要在循环体中使用`break`语句终止循环。
   - `break`语句：跳出当前循环，继续执行循环之后的代码。
   - `next`语句：跳过当前迭代，继续下一次迭代。
   - `return`语句：在函数中使用，结束函数的执行，并返回一个值。

3. 控制流程语句（Control Flow Statements）：
   - `ifelse`函数：根据给定条件的真假返回不同的值，`ifelse(con, statement1, statement2)`。
   - `stop`函数：停止程序的执行，并返回一个错误消息。
   - `warning`函数：生成一个警告消息，但不会停止程序的执行。

# Function
## 示例
```R
> f<-function(a,b)   #写一个求和函数,function()是固定格式
+ {
+   k<-a+b
+   return(k)      #返回结果为k，如果没有设置返回值，则函数会返回最后一行执行的结果
+ }
> f(3,4)			#使用函数名加一个括号调用函数
[1] 7
```
有局部变量和全局变量之分，在函数中的局部变量在函数结束后被删除

## 管道
`|>`将每次调用函数看成对自变量的加工， 加工完以后通过管道传送给下一道工序加工
```R
exp(1) |> log()
```

# I/O
## scan()
`scan(file = "", what = double(), nmax = -1, n = -1, sep = "")`
`what`接收读入的**数据类型**，若文件有多种类型，也可以不写，`sep`为数据分隔符

## readline()
从键盘读入一行数据返回，字符串
类似Python的`input`，也可以有提示语
```R
res <- readline()
```
## readlines()
可以一次性读取文件多行，可以设置行数
`readlines(filePath, n=?)`

## sprintf()
功能与C类似
```R
sprintf('file%03d.txt', c(1, 99, 100))
## [1] "file001.txt" "file099.txt" "file100.txt"
```

## file.show()
## read.table()
## read.csv()
## write.table()
## write.csv()