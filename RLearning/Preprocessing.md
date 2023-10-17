# Preprocessing
## Data filtering
### which
使用`which`来筛选满足特定条件的数据索引：

```R
# 创建一个向量
data <- c(5, 10, 15, 20, 25, 30)

# 使用which筛选出大于15的元素索引
filtered_index <- which(data > 15)

# 打印输出筛选后的索引
print(filtered_index)
```

输出结果：
```
[1] 4 5 6
```

### subset

使用`subset`根据某个条件过滤数据框：

```R
# 创建一个包含姓名和年龄的数据框
data <- data.frame(
  Name = c("Alice", "Bob", "Charlie", "Dave"),
  Age = c(25, 30, 35, 40)
)

# 使用subset筛选出年龄大于30的行
filtered_data <- subset(data, Age > 30)

# 打印输出筛选后的数据框
print(filtered_data)
```

输出结果：
```
     Name Age
3 Charlie  35
4    Dave  40
```

## Handling missing values
一些特殊的数值
- FALSE：以0计算
- NA(缺失值)：参与计算
- NULL：不参与计算
- NaN：无意义的数，比如`sqrt(-5)`

### 缺失值检测
1. 判断x是否缺失值的函数是`is.na(x)`，是返回`TRUE`,否则返回`FALSE`。
```R
# 创建一个包含缺失值的向量
x <- c(10, NA, 20, NA, 30)

# 使用is.na判断变量是否为缺失值
is_missing <- is.na(x)

# 打印输出判断结果，boolean值向量
print(is_missing)
```
输出结果：
```
[1] FALSE  TRUE FALSE  TRUE FALSE
```
2. 判断x是否完整的函数是**`complete.cases(x)`。对行判断，以行为单位**
```R
# 创建一个包含缺失值的向量
x <- c(10, 20, NA, 30, NA)

# 使用complete.cases判断变量是否完整
is_complete <- complete.cases(x)

# 打印输出判断结果
print(is_complete)
```

输出结果：
```
[1]  TRUE  TRUE FALSE  TRUE FALSE
```
3. `summary`可以显示每个变量的缺失值数量。
```R
# 创建一个包含缺失值的数据框
data <- data.frame(
  Var1 = c(10, 20, NA, 30, NA),
  Var2 = c(NA, 40, 50, NA, 60)
)

# 使用summary显示每个变量的缺失值数量
summary(data)
```

输出结果：
```
      Var1            Var2      
 Min.   :10.0   Mode :logical  
 1st Qu.:17.5   FALSE:2        
 Median :25.0   TRUE :3        
 Mean   :22.5   NA's :0        
 3rd Qu.:30.0                  
 Max.   :30.0                  
```
4. 返回数据缺失模式使用`mice`包中的`md.pattern(x)`函数。

```R
# 安装和加载mice包
install.packages("mice")
library(mice)

# 创建一个包含缺失值的数据框
data <- data.frame(
  Var1 = c(10, 20, NA, 30, NA),
  Var2 = c(NA, 40, 50, NA, 60),
  Var3 = c(NA, 70, NA, NA, 80)
)

# 使用md.pattern返回数据的缺失模式
missing_pattern <- md.pattern(data)

# 打印输出缺失模式
print(missing_pattern)
```

输出结果：
```
      Var1 Var2 Var3    
[1,]     1    1    1 0
[2,]     0    1    1 1
[3,]     0    0    1 2
```

### 缺失值处理
1. 当缺失数据较少时直接删除相应样本
```R
# 创建包含缺失数据的数据框
data <- data.frame(
  Var1 = c(10, NA, 20, NA, 30),
  Var2 = c(40, 50, NA, 60, NA)
)

# 删除包含缺失数据的样本
data_clean <- na.omit(data)

# 打印输出删除缺失数据后的数据框
print(data_clean)
```

输出结果：
```
  Var1 Var2
1   10   40
```
2. 对缺失数据进行插补，众数、均值...
a. 使用众数插补缺失数据：

```R
# 创建包含缺失数据的向量
x <- c(10, NA, 20, NA, 30)

# 计算众数
mode <- as.numeric(names(table(x))[which.max(table(x))])

# 使用众数插补缺失数据
x_imputed <- ifelse(is.na(x), mode, x)

# 打印输出插补后的向量
print(x_imputed)
```

输出结果：
```
[1] 10 20 20 20 30
```

b. 使用平均数插补缺失数据：

```R
# 创建包含缺失数据的向量
x <- c(10, NA, 20, NA, 30)

# 计算平均数
mean_val <- mean(x, na.rm = TRUE)

# 使用平均数插补缺失数据
x_imputed <- ifelse(is.na(x), mean_val, x)

# 打印输出插补后的向量
print(x_imputed)
```

输出结果：
```
[1] 10 20 20 20 30
```
3. 对缺失数据使用不敏感的分析方法，如决策树
```R
# 创建包含缺失数据的数据框
data <- data.frame(
  Var1 = c(10, NA, 20, NA, 30),
  Var2 = c(40, 50, NA, 60, NA),
  Target = c("A", "B", "B", "A", "A")
)

# 使用决策树进行分类分析
library(rpart)

# 创建决策树模型
tree_model <- rpart(Target ~ Var1 + Var2, data = data, method = "class")

# 打印决策树模型的预测结果
print(predict(tree_model, data, type = "class"))
```

输出结果：
```
 1  2  3  4  5 
A  B  B  A  A 
Levels: A B
```
## Dealing with outliers
异常值（离群点）是指测量数据中的随机错误或偏差，包括错误值或偏离均值的孤立点值。在数据处理中，异常值会极大的影响回归或分类的效果
为了避免异常值造成的损失，需要在数据预处理阶段进行异常值检测。另外，某些情况下，异常值检测也可能是研究的目的，如数据造假的发现、电脑入侵检测等。

1. 箱线图：
- `coef=1.5`：这是一个可选参数，用于定义异常值的阈值。在计算异常值时，将使用箱线图中的 IQR（四分位数间距）乘以该系数，默认值为 1.5。

- `do.conf=TRUE`：这是一个可选参数，用于指示是否计算箱线图的置信区间。设置为 `TRUE` 表示要计算置信区间。

- `do.out=TRUE`：这是一个可选参数，用于指示是否将异常值包含在返回结果中。设置为 `TRUE` 表示要获取异常值。

最后，通过 `y$stats` 和 `y$out` 可以访问箱线图统计信息的结果。`y$stats` 是一个包含五个值的向量，分别表示箱线图的下限（minimum）、下四分位数（lower hinge）、中位数（median）、上四分位数（upper hinge）、上限（maximum）；`y$out` 则是一个向量，包含异常值的数值。
```R
# 创建示例数据
data <- c(10, 15, 20, 25, 30, 35, 40, 300)

# 绘制箱线图
boxplot(data)

# 根据箱线图识别离群点
outliers <- boxplot(data)$out

# 处理离群点（例如：将离群点替换为中位数）
data_clean <- ifelse(data %in% outliers, median(data), data)
```
还可以这样
```R
# 创建示例数据
data <- c(10, 15, 20, 25, 30, 35, 40, 300)

# 根据箱线图识别离群点
boxplot_result <- boxplot.stats(data, coef = 1.5, do.conf = TRUE, do.out = TRUE)

# 绘制箱线图
boxplot(data)

# 输出箱线图统计信息
stats <- boxplot_result$stats
cat("离群点 (outliers): ", boxplot_result$out, "\n")
```

2. 散点图：
```R
# 生成示例数据（假设x为含有两列的数据框）
x <- data.frame(col1 = c(1, 2, 3, 4, 5, 6, 7, 8, 9, 100), col2 = c(10, 20, 30, 40, 50, 60, 70, 80, 90, 100))

# 计算离群点的索引
a <- which(x[, 2] %in% boxplot.stats(x[, 2])$out)
b <- which(x[, 1] %in% boxplot.stats(x[, 1], coef = 1.0)$out)
p2 <- union(a, b)

# 绘制散点图
plot(x, pch = 16, col = "blue", main = "Scatter Plot with Outliers")
points(x[p2, ], pch = "x", col = "red", cex = 2)
```

在这个示例中，首先定义了一个示例数据框x，包含两列。然后，使用`which()`函数结合`boxplot.stats()`来计算每列中的离群点的索引，将结果存储在变量a和b中。接下来，使用`union()`函数将a和b中的索引合并到变量p2中。

最后，使用`plot()`函数绘制散点图，并将散点标记为蓝色（默认颜色）。然后，使用`points()`函数将离群点（在数据框x中对应索引的行）以红色叉形标记出来。


3. 聚类方法：

```R
# 使用k-means聚类算法来检测离群点
k <- kmeans(iris[, c(1, 2)], centers = 3)
cluster <- k$cluster

# 确定聚类中心和样本与中心的距离
centers <- k$centers[cluster, ]
distances <- sqrt(rowSums((iris[, 1:2] - centers) ^ 2))

# 找出距离最大的前6个样本，将其视为离群点
out <- order(distances, decreasing = TRUE)[1:6]

# 绘制散点图
plot(iris[, c(1, 2)], main = "Scatter Plot with Outliers")
points(iris[out, c(1, 2)], col = "red", pch = "x", cex = 2)
```

在这个示例中，我们使用k-means聚类算法将iris数据集的前两列（Sepal Length和Sepal Width）进行聚类，将数据分成3个簇。然后，我们根据每个样本与其所属簇的聚类中心的距离，确定离群点的索引，并将其存储在变量out中。

最后，使用`plot()`函数绘制原始数据的散点图，并使用`points()`函数将离群点（在数据集iris中对应索引的行）以红色叉形标记出来。

## Data deduplication
当处理数据时，你可能需要了解数据中唯一值和重复值的情况。在R中，你可以使用`unique()`和`duplicated()`函数来完成这些任务。

1. `unique()`: 

```
unique(x)
```

`unique()`函数用于返回向量或数据框中的唯一值。它接受一个参数 `x`，表示待处理的向量或数据框。函数将返回一个包含 `x` 中唯一值的新向量或数据框。**直接去重**

示例用法：
```R
x <- c(1, 2, 3, 2, 4, 1, 3)
unique_values <- unique(x)
print(unique_values)
```
输出：
```
[1] 1 2 3 4
```

在上面的示例中，我们有一个包含重复值的向量 `x`。使用`unique()`函数，我们得到了去重后的唯一值向量 `unique_values`。

2. `duplicated()`:

```
duplicated(x)
```

`duplicated()`函数用于判断向量或数据框中的每个元素是否为重复值。它接受一个参数 `x`，表示待处理的向量或数据框。函数返回一个逻辑向量，其长度与 `x` 相同，指示每个元素是否为重复值。

示例用法：
```R
x <- c(1, 2, 3, 2, 4, 1, 3)
is_duplicated <- duplicated(x)
print(is_duplicated)
```
输出：
```
[1] FALSE FALSE FALSE  TRUE FALSE  TRUE  TRUE
```

注意这里第一个2是FALSE，第二个2才是TRUE

在上面的示例中，我们有一个包含重复值的向量 `x`。使用`duplicated()`函数，我们得到了一个逻辑向量 `is_duplicated`，其中 `TRUE` 表示对应位置的元素为重复值，`FALSE` 表示对应位置的元素为唯一值。

## Data normalization
[见RMD](../PPT/数据预处理.Rmd)
## sampling
当你需要从一个数据集中随机抽取样本时，可以使用R中的`sample()`函数。`sample()`函数可以从给定的向量中随机选择指定数量的元素。

`sample()`函数的基本语法如下：
```R
sample(x, size, replace = FALSE, prob = NULL)
```

其中，
- `x`: 表示待抽样的向量。
- `size`: 表示要抽取的样本的大小。
- `replace`: 一个逻辑值，表示是否进行有放回的抽样。如果为`TRUE`，则抽样时允许重复选择同一个元素；如果为`FALSE`，则抽样时不允许重复选择同一个元素。
- `prob`: 一个可选的表示每个元素被选择的概率的向量。默认为`NULL`，表示所有元素的选择概率相等。

下面是几个`sample()`函数的示例用法：

1. 从向量中随机选择一个元素：
```R
x <- c("A", "B", "C", "D", "E")
random_element <- sample(x, size = 1)
print(random_element)
```
输出：
```
[1] "C"
```

2. 从向量中随机选择多个元素：
```R
x <- c("A", "B", "C", "D", "E")
random_elements <- sample(x, size = 3)
print(random_elements)
```
输出：
```
[1] "E" "B" "A"
```

3. 从向量中进行有放回的随机抽样（允许重复选择同一个元素）：
```R
x <- c("A", "B", "C", "D", "E")
random_elements <- sample(x, size = 5, replace = TRUE)
print(random_elements)
```
输出：
```
[1] "B" "A" "D" "E" "E"
```

4. 根据给定的概率进行随机选择：
```R
x <- c("A", "B", "C", "D", "E")
probabilities <- c(0.1, 0.2, 0.3, 0.2, 0.2)
random_elements <- sample(x, size = 3, prob = probabilities)
print(random_elements)
```
输出：
```
[1] "E" "D" "A"
```

## sorting
[见RMD](../PPT/数据预处理.Rmd)
## vectorization
当你想要将一个对象转换为向量时，可以使用R中的`as.vector()`函数。`as.vector()`函数可以将各种类型的对象（如列表、因子、数组等）转换为一维的向量。

`as.vector()`函数的基本语法如下：
```R
as.vector(x, mode = "any")
```

其中，
- `x`：表示要转换的对象。
- `mode`：表示要转换为的向量的类型。默认值为"any"，表示根据对象的属性和类型的不同，可能返回不同的向量类型。可以使用诸如"logical"、"numeric"、"character"等参数来指定返回的向量类型。

下面是几个`as.vector()`函数的示例用法：

1. 将列表转换为向量：
```R
list_obj <- list(1, 2, 3)
vector_obj <- as.vector(list_obj)
print(vector_obj)
```
输出：
```
[1] 1 2 3
```

2. 将因子对象转换为向量：
```R
factor_obj <- factor(c("A", "B", "C"))
vector_obj <- as.vector(factor_obj)
print(vector_obj)
```
输出：
```
[1] 1 2 3
Levels: A B C
```
注意，因子被转换为了整数向量，并在输出中显示了原始因子的级别。

3. 将矩阵或数组对象转换为向量：
```R
matrix_obj <- matrix(1:9, nrow = 3)
vector_obj <- as.vector(matrix_obj)
print(vector_obj)
```
输出：
```
[1] 1 2 3 4 5 6 7 8 9
```
矩阵或数组对象按照列的顺序转换为了一维向量。

-------------------------------------------------------------------------

另外，还有一个用于将列表及其嵌套的子列表（如果存在）转换为简单向量的函数是`unlist()`。`unlist()`的作用是将嵌套结构展开，并返回一个扁平化的向量。

`unlist()`函数的基本语法如下：
```R
unlist(x, recursive = TRUE, use.names = TRUE)
```
其中，
- `x`：表示要展开的对象，通常为列表或嵌套列表。
- `recursive`：一个逻辑值，表示是否递归展开嵌套结构。默认为`TRUE`，表示递归展开所有子列表；如果设置为`FALSE`，则只展开最外层的列表。
- `use.names`：一个逻辑值，表示是否保留原始列表中的元素名称作为输出向量的元素名称。默认为`TRUE`，表示保留元素名称；如果设置为`FALSE`，则输出向量的元素名称为自动生成的数字。

下面是一个`unlist()`函数的示例用法：

```R
nested_list <- list(a = 1, b = list(2, 3), c = list(list(4, 5), 6))
flattened_vector <- unlist(nested_list)
print(flattened_vector)
```
输出：
```R
 a   b1   b2 c1 c2   c3 
 1    2    3   4   5   6 
```
展开的向量中的元素名称保留了原始列表中的层次结构。

`unlist()`函数特别适用于处理具有嵌套结构的数据，例如嵌套的列表或数据框。

## Cross-tabulation
列联表`table()`用于统计和分析两个或多个变量之间关系的二维表格
### 基本使用
```R
# 创建一个包含两个变量的数据框
data <- data.frame(Gender = c("Male", "Female", "Male", "Male", "Female"),
                   AgeGroup = c("18-24", "25-34", "35-44", "25-34", "18-24"))

# 创建列联表
table_result <- table(data$Gender, data$AgeGroup)
print(table_result)
```
输出
```
        18-24 25-34 35-44
  Female     1     0     1
  Male       1     2     0
```
说明男的对应在18-24的有1个，25-34的有2个等等

### 也可以对一个变量分析

```R
  (mytable=with(Arthritis,table(Improved)))
  prop.table(mytable)
```
这段代码利用了R中的`with()`函数和`table()`函数来创建和分析一个叫做`Arthritis`的数据集中的变量`Improved`的列联表。

首先，在**`with()`函数中指定了数据集**`Arthritis`。这意味着在接下来的代码中，我们可以直接引用`Arthritis`数据集中的变量，而**不需要每次都使用`Arthritis$`的前缀**。

然后，我们使用`table()`函数创建了一个名为`mytable`的列联表。`Improved`是`Arthritis`数据集中的一个变量，它记录了患者在接受治疗后的改善情况。`table(Improved)`将统计不同改善情况的频数，并创建一个包含这些频数的列联表。

接着，我们使用`prop.table()`函数对列联表进行了处理。`prop.table(mytable)`会计算出**每个单元格的相对比例**。这样做可以将频数转换为相对频率，以便更好地理解不同改善情况在整体中的比例。

请注意，`prop.table()`函数在不指定额外参数的情况下，**默认计算相对频率，即将频数除以总和**。如果你想计算其他类型的相对比例，可以通过`margin`参数指定边际和。

### 指定边际和
```
(mytable=xtabs(~Treatment+Improved,data=Arthritis))
margin.table(mytable,1)
margin.table(mytable,2)
```
这段代码使用了R中的`xtabs()`函数来创建一个交叉表（透视表），并使用`margin.table()`函数计算行和列的边际和。

首先，`xtabs()`函数的语法是`xtabs(formula, data)`，其中`formula`是一个公式，用于指定要交叉的变量，`data`是指定的数据集。在这里，公式`~Treatment+Improved`表示我们想要以`Treatment`和`Improved`两个变量为基础创建交叉表。`Arthritis`则是指定的数据集。

然后，我们使用`margin.table()`函数计算了交叉表的行边际和和列边际和。`margin.table()`函数的语法是`margin.table(x, margin)`，其中`x`是交叉表对象，`margin`是指定计算边际和的维度。在这里，`margin.table(mytable, 1)`表示计算行边际和，而`margin.table(mytable, 2)`表示计算列边际和。

输出
```
         Improved
Treatment None Some Marked
  Placebo   29    7      7
  Treated   13    7     21
Treatment
Placebo Treated 
     43      41 
Improved
  None   Some Marked 
    42     14     28 
```

### 边际和的占比
```R
  (mytable=xtabs(~Treatment+Improved,data=Arthritis))
  prop.table(mytable,1)
  prop.table(mytable,2)
  prop.table(mytable)
```
这段代码使用了R中的`xtabs()`函数来创建一个交叉表（透视表），并使用`prop.table()`函数计算了交叉表的比例。

首先，代码`mytable=xtabs(~Treatment+Improved,data=Arthritis)`使用`xtabs()`函数创建了一个交叉表，交叉表的行对应于`Treatment`变量的不同取值，列对应于`Improved`变量的不同取值。该交叉表存储在变量`mytable`中，以便后续使用。

接下来，我们使用`prop.table()`函数计算了交叉表的比例。`prop.table()`函数的语法是`prop.table(x, margin)`，其中`x`是交叉表对象，`margin`是指定计算比例的维度。

- `prop.table(mytable, 1)`表示计算行比例，即每个单元元素相对于**所在行总和的比例**。
- `prop.table(mytable, 2)`表示计算列比例，即每个单元元素相对于**所在列总和的比例**。
- `prop.table(mytable)`表示计算整个交叉表的比例，即每个单元元素相对于整个表的总和的比例。

输出
```
         Improved
Treatment None Some Marked
  Placebo   29    7      7
  Treated   13    7     21
         Improved
Treatment      None      Some    Marked
  Placebo 0.6744186 0.1627907 0.1627907
  Treated 0.3170732 0.1707317 0.5121951
         Improved
Treatment      None      Some    Marked
  Placebo 0.6904762 0.5000000 0.2500000
  Treated 0.3095238 0.5000000 0.7500000
         Improved
Treatment       None       Some     Marked
  Placebo 0.34523810 0.08333333 0.08333333
  Treated 0.15476190 0.08333333 0.25000000
```

### 增加总和的行/列

`addmargins()`
[见RMD](../PPT/数据预处理.Rmd)

## Grouping and summarizing
`aggregate(x, by, FUN, ...)`

### 根据多个条件聚合
```R
aggregate(state.x77,
        list(Region = state.region,
             Cold = state.x77[,"Frost"] > 130),
        mean)
```
根据state.region和是否Cold
state.region有Northeast、South、North Central和West
Cold有T和F
因此一共有8行，这里去掉了表头
```
Northeast	FALSE	8802.8000	4780.400	
South	FALSE	4208.1250	4011.938	
North Central	FALSE	7233.8333	4633.333	
West	FALSE	4582.5714	4550.143	
Northeast	TRUE	1360.5000	4307.500	
North Central	TRUE	2372.1667	4588.833	
West	TRUE	970.1667	4880.500	
```
**利用`aggregate(formula, data, FUN, ...)`**也可以
```R
# 创建一个数据集
data <- data.frame(
  name = c("Alice", "Bob", "Charlie", "David", "Emma"),
  age = c(20, 22, 21, 21, 20),
  gender = c("F", "M", "M", "F", "F"),
  score = c(85, 90, 95, 80, 75)
)

# 对年龄和性别进行分类，并计算平均分数
result <- aggregate(score ~ age + gender, data, mean)

# 输出结果
print(result)
```