[TOC]

# 对象类型

## Vector

都是相同类型的才能在一个向量里

不同类型都会变成字符串

类型转换类似`vec <- as.numeric(vec)`

下标从**1**开始

### 生成
```R
# c()函数将多个元素组合成一个向量
a <- c(1, 3, 5, 1, 3)

# :运算符：使用冒号运算符可以生成一个起始值到结束值的序列向量 闭区间
a <- 1:10

# seq()函数可以生成一个等差数列的向量。可以指定起始值、结束值和步长
a <- seq(1, 10, 2)

# rep()函数可以生成一个重复的向量。可以指定重复的元素和重复的次数
a <- rep(1, 10) # 1 重复 10次
```
还有一些特殊函数

### 删除

R中的向量无法直接在原始向量上增删，一般都是创建新的向量然后赋值

```R
# 使用索引删除元素
vector <- c(1, 2, 3, 4, 5)
vector <- vector[-3]  # 删除索引为3的元素
# 输出结果: vector = 1, 2, 4, 5

# 使用逻辑条件删除元素 方括号里面是向量名
vector <- c(1, 2, 3, 4, 5)
vector <- vector[vector != 3]  # 删除值为3的元素
# 输出结果: vector = 1, 2, 4, 5

# 使用函数 subset() 删除元素
vector <- c(1, 2, 3, 4, 5)
vector <- subset(vector, vector != 3)  # 删除值为3的元素
# 输出结果: vector = 1, 2, 4, 5
```

### 插入
```R
# 使用 append() 函数插入元素
vector <- c(1, 2, 3, 5)
vector <- append(vector, 4)  # 在末尾插入元素4
# 输出结果: vector = 1, 2, 3, 5, 4

# 使用 append() 函数插入元素
vector <- c(1, 2, 3, 5)
vector <- append(vector, 4, 2)  # 在index为2后面插入元素4
# 输出结果: vector = 1, 2, 4, 3, 5

# 使用函数 insert() 插入元素
library(insert)
vector <- c(1, 2, 4, 5)
vector <- insert(vector, 3, 3)  # 在索引为3的位置插入元素3
# 输出结果: vector = 1, 2, 3, 4, 5
```

### 修改
```R
# 利用下标赋值
vector[2] <- 23

# 修改多个为同样的值
vector[c(1, 3)] <- 23

# 利用c()函数拼接
a <- c(b, c)
```

### 查找
注意which返回的是索引下标
```R
# 示例数据
vector <- c(2, 4, 6, 8, 10)

# 查找大于5的元素的索引
indexes <- which(vector > 5)
# 输出结果: indexes = 3, 4, 5

# 查找偶数元素的索引
indexes <- which(vector %% 2 == 0)
# 输出结果: indexes = 1, 2, 3, 4, 5

# 查找等于10的元素的索引
indexes <- which(vector == 10)
# 输出结果: indexes = 5
```
若要返回对应值，直接将which的结果放入中括号
```R
# 示例数据
vector <- c(2, 4, 6, 8, 10)

# 查找向量中大于等于7的元素索引
indices <- which(vector >= 7)
# 输出结果: indices = 4 5

# 获取满足条件的元素
matching_values <- vector[indices]
# 输出结果: matching_values = 8 10
```

### 对向量的运算
```R
# 示例数据
vector <- c(2, 4, 6, 8, 10)

# 最大值
val <- max(vector)
# 输出结果: val = 10

# 最小值
val <- min(vector)
# 输出结果: val = 2

# 长度
val <- length(vector)
# 输出结果: val = 5

# 求和
val <- sum(vector)
# 输出结果: val = 30

# 每个元素都增加
vector + 1

# 每个元素都平方
vector^2

# 每个元素都开方
sqrt(vector)
```
## Array
### 生成
```R
# array(vector, dim=维度数组)
arr <- array(1:6, dim=(2, 3))
```
### 维度
```R
# 修改维度从(2, 3)变成(3, 2)
arr <- c(3, 2)

# 获得数组维度
dim(arr)
```
### 获得元素
```R
my_array <- array(data = 1:30, dim = c(5, 3, 2))

# 获取第一个维度为1的子数组
sub_array <- my_array[1, , ]

# 获取第二个维度为1的子数组
sub_array <- my_array[,1 , ]

# 获取第二个维度范围为2-4的切片
slice <- my_array[, 2:4, ]

# 取做不元素作为子集，这与x本身不同
x[]

# 下标越界不报错，自动补充NA
x <- c(1,4,6.25)
x[5]
## [1] NA
x
## [1] 1.00 4.00 6.25
x[5] <- 9
x
## [1] 1.00 4.00 6.25   NA 9.00
```
## Matrix
An array with 2 dimensions
### 生成
```R
my_matrix <- matrix(data = 1:9, nrow = 3, ncol = 3)

matrix(1:8, nrow = 2)

matrix(1:8, ncol = 2)
```
### 获取元素
```R
# 获取第一行和第三列的元素
element <- my_matrix[1, 3]

# 获取第二行和第三行的子矩阵
sub_matrix <- my_matrix[2:3, ]

# 获取第二列和第四列的切片
slice <- my_matrix[, c(2, 4)]

# 创建一个矩阵，其中包含原矩阵中第1行和第3行，以及第2列和第4列的元素
slice <- my_matrix[c(1, 3), c(2, 4)]
```
### 修改维度
```R
new_dim <- c(2, 4)
dim(my_matrix) <- new_dim
```

### byrow填充方向
```R
# 创建一个包含数字 1 到 9 的向量
my_vector <- 1:9

# 使用 byrow = TRUE 创建一个 3x3 的矩阵
my_matrix_byrow <- matrix(data = my_vector, nrow = 3, ncol = 3, byrow = TRUE)

# 使用 byrow = FALSE 创建一个 3x3 的矩阵
my_matrix_bycol <- matrix(data = my_vector, nrow = 3, ncol = 3, byrow = FALSE)

# 输出两个矩阵
my_matrix_byrow
my_matrix_bycol
```
```t
# my_matrix_byrow
     [,1] [,2] [,3]
[1,]    1    2    3
[2,]    4    5    6
[3,]    7    8    9

# my_matrix_bycol
     [,1] [,2] [,3]
[1,]    1    4    7
[2,]    2    5    8
[3,]    3    6    9
```
### 矩阵运算
#### 矩阵拼接
```R
# 创建两个矩阵
mat1 <- matrix(1:6, ncol = 2)
mat2 <- matrix(7:12, ncol = 2)

# 使用 rbind() 函数进行行合并
combined_mat <- rbind(mat1, mat2)

# 查看合并后的矩阵
combined_mat

     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
[4,]    7   10
[5,]    8   11
[6,]    9   12

# 创建两个矩阵
mat1 <- matrix(1:3, ncol = 1)
mat2 <- matrix(4:6, ncol = 1)

# 使用 cbind() 函数进行列合并
combined_mat <- cbind(mat1, mat2)

# 查看合并后的矩阵
combined_mat

     [,1] [,2]
[1,]    1    4
[2,]    2    5
[3,]    3    6
```
#### 矩阵转置
```R
# 创建一个矩阵
mat <- matrix(1:6, nrow = 2)

# 对矩阵进行转置
transposed_mat <- t(mat)
```
#### 将矩阵转换为向量
```R
# 创建一个矩阵
mat <- matrix(1:6, nrow = 2)

# 将矩阵转换为向量
vector <- as.vector(mat)
```
#### 返回矩阵的维度
```R
# 创建一个矩阵
mat <- matrix(1:6, nrow = 2)

# 使用 dim() 函数返回矩阵的维度
dim(mat)

# 使用 nrow() 和 ncol() 函数返回矩阵的行数和列数
nrow(mat)
ncol(mat)
```
#### 对行或列求和
```R
# 创建一个矩阵
mat <- matrix(1:6, nrow = 2)

# 对矩阵各列求和
column_sums <- colSums(mat)

row_sums <- rowSums(mat)
```
#### 对行或列求均值
```R
# 创建一个矩阵
mat <- matrix(1:6, nrow = 2)

# 对矩阵各列求均值
column_means <- colMeans(mat)

# 对矩阵各行求均值
row_means <- rowMeans(mat)
```
#### 计算行列式
```R
# 创建一个矩阵
mat <- matrix(c(1, 2, 3, 4), nrow = 2)

# 计算矩阵的行列式
determinant <- det(mat)
```
#### 矩阵相乘
```R
M
#      [,1] [,2]
# [1,]    1    3
# [2,]    2    4
N
#      [,1] [,2]
# [1,]    5    7
# [2,]    6    8
K<-M%*%N			#两矩阵相乘
K
#      [,1] [,2]
# [1,]   23   31
# [2,]   34   46
```
## Data frames
类似于二维表格形式的数据，包含多个变量列和观察值行，可以包含不同的数据类型
列的长度可以不同，每一列可以包含不同数量的元素
可以通过列名对数据进行检索
### 生成
```R
# 创建一个DataFrame
df <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  age = c(25, 30, 35),
  city = c("New York", "London", "Paris")
)
```
其中`data.frame`有一个参数`stringAsFactor`，用于控制将字符变量作为因子或作为字符向量进行处理
在R中，默认情况下，当创建DataFrame时，字符串变量会被自动转换为因子类型。因子是一种特殊的数据类型，用于表示分类变量或离散变量的水平（取值）
然而，有时候我们可能更希望将字符串变量保留为字符向量而不是因子，这时可以使用`stringAsFactor`参数来控制该行为
```R
# 创建一个包含字符串变量的DataFrame
df <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  city = c("New York", "London", "Paris"),
  stringsAsFactors = TRUE  # 默认值
)

# 查看DataFrame的结构
str(df)

# 输出：
# 'data.frame': 3 obs. of  2 variables:
#  $ name: Factor w/ 3 levels "Alice","Bob","Charlie": 1 2 3
#  $ city: Factor w/ 3 levels "London","New York",..: 2 1 3

# 将字符串变量保留为字符向量
df <- data.frame(
  name = c("Alice", "Bob", "Charlie"),
  city = c("New York", "London", "Paris"),
  stringsAsFactors = FALSE
)

# 查看DataFrame的结构
str(df)

# 输出：
# 'data.frame': 3 obs. of  2 variables:
#  $ name: chr  "Alice" "Bob" "Charlie"
#  $ city: chr  "New York" "London" "Paris"
```
### 查看
```R
# 使用head()或print()函数可以查看DataFrame的前几行，默认是前六行
head(df)

print(df)

# 可以使用$符号或者[[]]操作符来访问DataFrame的列

# 访问name列
df$name

# 或者使用[[]]
df[["name"]]

# 注意一下两者不同的输出形式
df["age"]
#   age
# 1  25
# 2  30
# 3  35
df[["age"]]
# [1] 25 30 35

# 获取第一列
df[,1]

# 获取第一行
df[1,]

# 获取第一行第一列的元素
df[1,1]
```
### 过滤
```R
# 过滤出年龄大于等于30的行
df_filtered <- df[df$age >= 30, ]

# 使用subset()函数同样效果
subset(df, age >= 30)
```
### 修改
```R
# 添加一个新的列
df$gender <- c("Female", "Male", "Male")
```
### 获取df的相关属性
1. colnames(df)：

- 该函数用于获取数据框（df）的列名（column names）。
- 返回一个字符向量，包含数据框的所有列名。

2. rownames(df)：

- 该函数用于获取数据框（df）的行名（row names）。
- 返回一个字符向量，包含数据框的所有行名。

3. nrow(df)：

- 该函数用于获取数据框（df）的行数（number of rows）。
- 返回一个整数值，表示数据框的行数。

4. ncol(df)：

- 该函数用于获取数据框（df）的列数（number of columns）。
- 返回一个整数值，表示数据框的列数。
### 合并函数merge()
基本语法`merge(x, y, by = NULL, by.x = NULL, by.y = NULL)`
参数说明:
- `x` 和 `y`：要合并的两个数据框（或表）。
- `by`：指定用于合并的列名（键）。默认情况下，它会自动识别两个数据框中名称相同的列作为键。
- `by.x` 和 `by.y`：如果两个数据框的键列列名不同，可以使用这两个参数分别指定 x 和 y 数据框中的键列名。
- 除了上述基本参数外，`merge()` 函数还提供了其他一些可选的参数，例如 `all`、`all.x` 和 `all.y`，用于控制合并时的行为。
```R
df1 <- data.frame(id = 1:3, name = c("John", "Mary", "Emma"))
df2 <- data.frame(id = 2:4, city = c("NYC", "LA", "SFO"))

# 根据id将两个数据框合并
merge(df1, df2, by="id")
#   id name city
# 1  2 Mary  NYC
# 2  3 Emma   LA
```
当两个要合并的数据框中的键列在列名上存在差异时，可以使用 `by.x` 参数来指定 `x` 数据框中的键列名。下面是一个使用示例：

假设我们有两个数据框，一个包含员工信息，另一个包含员工的薪资信息。它们的键列名分别为 `employee_id` 和 `emp_id`。我们想要根据这两个键列将两个数据框进行合并。
```R
# 创建示例数据框
employee_data <- data.frame(employee_id = c(1, 2, 3),name = c("John", "Mary", "Emma"))

salary_data <- data.frame(emp_id = c(2, 3, 4),salary = c(5000, 6000, 7000))

# 使用 merge 函数合并数据框，通过 by.x 和 by.y 指定键列名
merged_data <- merge(employee_data, salary_data, by.x = "employee_id", by.y = "emp_id")

# 输出合并后的结果
print(merged_data)
#   employee_id  name salary
# 1           2  Mary   5000
# 2           3  Emma   6000
```
`all.x` 是一个用于 `merge` 函数的逻辑参数，用于指定是否保留所有在 `x` 数据框中出现的观测，无论它们在 `y` 数据框中是否有对应的匹配项。当设置为 `TRUE` 时，即保留 `x` 中的所有观测；当设置为 `FALSE`（默认值）时，只保留 `x` 和 `y` 中的交集
假设有两个数据框，一个包含员工信息，另一个包含员工的薪资信息。我们想要根据员工 ID 将这两个数据框进行合并，同时保留没有薪资信息的员工。
```R
# 创建示例数据框
employee_data <- data.frame(employee_id = c(1, 2, 3),name = c("John", "Mary", "Emma"))
salary_data <- data.frame(employee_id = c(2, 3),salary = c(5000, 6000))

# 使用 merge 函数合并数据框，设置 all.x = TRUE
merged_data <- merge(employee_data, salary_data, by = "employee_id", all.x = TRUE)

# 输出合并后的结果
#   employee_id name salary
# 1           1 John     NA
# 2           2 Mary   5000
# 3           3 Emma   6000
```
### 聚合操作
在 R 中用于根据指定的因子变量对数据进行聚合操作。它可以根据一个或多个变量对数据进行分组，并对每个组进行计算或汇总
基本语法`aggregate(formula, data, FUN, ...)`
参数:
- `formula`制定了聚合操作的公式，通常包括一个或多个变量作为聚合的依据
  - 例如`value ~ group`
  - 左侧表示要聚合的变量
  - 而右侧表示用于分组的变量
- `data`数据集or数据框
- `FUN`应用于每个分组的函数

基本使用
```R
# 创建示例数据框
df <- data.frame(group = c("A", "A", "B", "B", "B"),
                 value = c(10, 15, 20, 25, 30))

# 对数据框进行分组并计算每个组的平均值
result <- aggregate(value ~ group, data = df, FUN = mean)

# 输出结果
#   group value
# 1     A 12.5
# 2     B 25.0
```

聚合输出多个
```R
df <- data.frame(group = c("A", "A", "B", "B", "B"), value = c(10, 15, 20, 25, 30))

# 计算每个组的总和和均值
result <- aggregate(value ~ group, data = df, FUN = function(x) c(sum = sum(x), mean = mean(x)))

# 输出结果
#   group value.sum value.mean
# 1     A        25       12.5
# 2     B        75       25.0
```

根据多个因子变量聚合
```R
df <- data.frame(group1 = c("A", "A", "B", "B", "B"), group2 = c("X", "Y", "X", "Y", "Z"), value = c(10, 15, 20, 25, 30))

# 根据group1和group2进行聚合，计算每个组的总和
result <- aggregate(value ~ group1 + group2, data = df, FUN = sum)

# 输出结果
#   group1 group2 value
# 1      A      X    10
# 2      B      X    20
# 3      A      Y    15
# 4      B      Y    25
# 5      B      Z    30
```

自定义根据什么聚合
```R
df <- data.frame(value = 1:10)

# 自定义函数来判断奇偶性
check_even_odd <- function(x) {
  if (x %% 2 == 0) {
    return("Even")
  } else {
    return("Odd")
  }
}

# 根据奇偶性进行聚合，并计算每个组的平均值
result <- aggregate(value ~ check_even_odd(value), data = df, FUN = mean)

# 输出结果
#   check_even_odd(value) value
# 1                   Even     6
# 2                    Odd     5
```
## List
可以存储不同类型的对象

### 生成
```R
my_list <- list(1, "apple", c(1, 2, 3))# 包含不同类型对象的列表

# 输出
# [[1]]
# [1] 1

# [[2]]
# [1] "apple"

# [[3]]
# [1] 1 2 3
```
### 获得元素
用`[[]]`或者命名了才能用`$`
```R
fruits <- list(apple = 5, banana = 7, orange = 3)
print(fruits$apple)  # 输出：5
print(fruits$banana)  # 输出：7
```
因为list里什么都能装，当然可以用递归的方式
`my_list[[1]][[2]][["colName"]]`
### 命名
除了初始化的时候命名
还可以使用`names()`函数
```R
fruits <- list("apple", "banana", "orange")
names(fruits) <- c("fruit1", "fruit2", "fruit3")
print(fruits$fruit1)  # 输出："apple"
print(fruits$fruit2)  # 输出："banana"
```
### 遍历list
```R
for (item in my_list) {
     print(item)
}
# [1] 1
# [1] "apple"
# [1] 1 2 3
```
### 修改
```R
append(my_list, "aa")
[[1]]
[1] 1

[[2]]
[1] "apple"

[[3]]
[1] 1 2 3

[[4]]
[1] "aa"
```
## Time Series
安装时间顺序排列的一些列观测数据点
基本语法`ts(data, start, end, frequency)`
参数含义
- `data`：时间序列的观测数据，可以是向量、矩阵或数据框。
- `start`：时间序列的起始时间点，可以是一个数值或表示时间的字符向量。
- `end`：时间序列的结束时间点，同样可以是一个数值或表示时间的字符向量。
- `frequency`：时间序列的频率，表示每单位时间所包含的数据点数目。一年中的月数、一年中的天数、一天中的小时数等。默认值为1。
  - 年度数据：frequency = 1
  - 季度数据：frequency = 4
  - 月度数据：frequency = 12
  - 周数据：frequency = 52（通常一个年份有52周，有时会根据具体情况而定）
  - 日数据：frequency = 365（通常假设一年有365天）
  - 小时数据：frequency = 24（通常假设一天有24小时）