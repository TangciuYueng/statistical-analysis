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
2. 判断x是否完整的函数是`complete.cases(x)`。
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
## Data normalization
## sampling
## sorting
## vectorization
## Cross-tabulation
## Grouping and summarizing