# 数据可视化
## plot
`plot` 函数是R语言中用于创建图形的基本绘图函数之一，它具有许多参数，允许您自定义图形的外观和内容。以下是 `plot` 函数的一些主要参数及其含义：

1. `x`：x轴上的数据。这是一个必需的参数，用于指定图形中的横坐标数据。

2. `y`：y轴上的数据。这也是一个必需的参数，用于指定图形中的纵坐标数据。

3. `type`：指定绘制的图形类型。常见的取值包括：
   - `"p"`：绘制散点图（点图）。
   - `"l"`：绘制折线图。
   - `"b"`：同时绘制点和线。
   - `"o"`：绘制线，但是在点之间不连接线段。
   - `"h"`：绘制高密度图。
   - `"s"`：绘制阶梯图。
   - `"n"`：不绘制图形，只创建一个空白的图形框架。

4. `main`：图形的标题（主标题）。

5. `xlab`：x轴的标签。

6. `ylab`：y轴的标签。

7. `xlim`：x轴的范围，以一个包含两个值的向量表示，如 `c(min, max)`。

8. `ylim`：y轴的范围，以一个包含两个值的向量表示，如 `c(min, max)`。

9. `col`：绘制的图形的颜色，可以是字符向量或颜色名称。

10. `pch`：绘制的点的形状（仅当 `type = "p"` 时有效），通常是一个整数或字符。

11. `lty`：绘制的线的类型（仅当 `type = "l"` 或 `type = "b"` 时有效），通常是一个整数。

12. `lwd`：线的宽度（仅当 `type = "l"` 或 `type = "b"` 时有效），通常是一个正数。

13. `cex`：点的字符大小（仅当 `type = "p"` 时有效），通常是一个正数。

14. `...`：其他参数，用于传递给具体图形类型的绘图函数。


```R
# 创建示例数据
x <- c(1, 2, 3, 4, 5)
y <- c(3, 6, 8, 10, 13)

# 创建散点图并自定义参数
plot(x, y, type = "p", main = "Scatter Plot", xlab = "X-axis", ylab = "Y-axis", col = "blue", pch = 16, cex = 1.5)
```

### 散点图
```R
# 想设置相同的随机数
set.seed(1234)
# 一到一百随机取八十个
x <- sample(1:100, 80, replace=FALSE)
# 正态分布噪声
y <- 2 * x + 20 + rnorm(80, 0, 10)
plot(x, y)
```

1. `set.seed(1234)`: 这行代码设置了随机数种子，以确保每次运行代码时都生成**相同的随机数**。助于结果的可重复性。

2. `x <- sample(1:100, 80, replace = FALSE)`: 这行代码创建了一个名为 `x` 的向量，其中包含了从1到100的整数，共选取了80个样本（`sample` 函数的第二个参数），并且不允许重复抽样（`replace = FALSE`）。


### 折线图
希望按照x的顺序从小到大的折线，而不是杂乱无章
配合`order`函数使用

线条类型
- `type="l"`告诉`plot`是折线
- `type="b"`告诉`plot`是点划线
- `tly=NUMBER`更多线条类型
- `lwd=NUMBER`线条宽度
```R
plot(x[order(x)], y[order(x)], type="l")

plot(x[order(x)], y[order(x)], type="l", lty=2)
```
#### 线性回归模型
```R
# 返回k、b
model <- lm(y~x)
# 根据k、b画线
abline(model)
```
1. `model <- lm(y~x)`: 这行代码使用 `lm` 函数拟合了一个线性回归模型，其中 `y` 是因变量，`x` 是自变量。该模型尝试找到一条最佳拟合直线，以描述 `x` 和 `y` 之间的关系，并将结果存储在 `model` 中。

2. `abline(model, col = "blue")`: 这行代码使用 `abline` 函数在散点图上绘制了线性回归模型的拟合直线，该直线由前面拟合的 `model` 模型定义。线的颜色被设置为蓝色（`col = "blue"`）。

##### lm
假设有一个数据框 `df` 包含以下数据：

```R
df <- data.frame(
  x = c(1, 2, 3, 4, 5),
  y = c(3, 6, 8, 10, 13)
)
```

这里的 `x` 是自变量，`y` 是因变量。现在，我们想建立一个线性回归模型，来拟合 `x` 和 `y` 之间的关系。

```R
# 使用 lm 函数建立线性回归模型
model <- lm(y ~ x, data = df)

# 打印模型摘要
summary(model)
```

上述代码中，我们首先使用 `lm` 函数创建了一个线性回归模型，通过指定公式 `y ~ x` 来表示因变量 `y` 与自变量 `x` 之间的线性关系。我们还指定了数据框 `df` 作为数据源，以确保R可以找到相应的变量。


还有其他一些辅助函数`legend`、`axis`、`mtext`、`grid` 和 `par` 的作用以及示例：

1. `legend` 函数：

   - **作用**：`legend` 函数用于在绘图中添加图例，以解释图形中的不同元素或数据系列。
   
   - **参数**：
     - `x`：图例的横坐标位置。
     - `y`：图例的纵坐标位置。
     - `legend`：一个字符向量，包含图例中的标签。
     - `col`：标签的颜色。
     - `pch`：标签的点形状。
     - `lty`：标签的线型。
     - `lwd`：标签的线宽。
     - `title`：图例的标题。
     - 其他参数用于控制图例的外观。

   - **示例**：
     ```R
     x <- 1:5
     y <- x^2
     plot(x, y, type = "l", col = "blue", lwd = 2, ylim = c(0, 30), xlab = "X", ylab = "Y")
     legend("topright", legend = "y = x^2", col = "blue", lwd = 2)
     ```
     当然`legend`和`col`也可以接受向量

2. `axis` 函数：

   - **作用**：`axis` 函数用于自定义坐标轴，包括标签、刻度和样式。
   
   - **参数**：
     - `side`：指定要绘制的坐标轴的位置（1：底部，2：左侧，3：顶部，4：右侧）。
     - `at`：指定刻度的位置。
     - `labels`：刻度标签。
     - `tick`：是否绘制刻度线。
     - 其他参数用于控制坐标轴的外观。

   - **示例**：
     ```R
     x <- 1:5
     y <- x^2
     plot(x, y, type = "l", col = "blue", lwd = 2, ylim = c(0, 30), xlab = "X", ylab = "Y")
     axis(side = 1, at = 1:5, labels = c("A", "B", "C", "D", "E"))
     ```

3. `mtext` 函数：

   - **作用**：`mtext` 函数用于在绘图中添加自定义文本，例如标题或注释。
   
   - **参数**：
     - `text`：要添加的文本内容。
     - `side`：指定文本的位置（1：底部，2：左侧，3：顶部，4：右侧）。
     - `line`：指定文本的行号。
     - `at`：指定文本的横坐标位置。
     - `col`：文本的颜色。
     - 其他参数用于控制文本的外观。

   - **示例**：
     ```R
     x <- 1:5
     y <- x^2
     plot(x, y, type = "l", col = "blue", lwd = 2, ylim = c(0, 30), xlab = "X", ylab = "Y")
     mtext("Custom Text", side = 3, line = 1, col = "red")
     ```

4. `grid` 函数：

   - **作用**：`grid` 函数用于在绘图中添加网格线。
   
   - **参数**：通常不需要参数，只需在绘图前运行 `grid()` 即可添加网格线。

   - **示例**：
     ```R
     x <- 1:5
     y <- x^2
     plot(x, y, type = "l", col = "blue", lwd = 2, ylim = c(0, 30), xlab = "X", ylab = "Y")
     grid()
     ```

5. `par` 函数：

   - **作用**：`par` 函数用于设置全局绘图参数，控制图形的多个方面，如边距、颜色、字体等。
   
   - **参数**：参数众多，用于设置不同的绘图参数。

   - **示例**：
     ```R
     x <- 1:5
     y <- x^2
     par(mar = c(5, 5, 4, 2))  # 设置边距
     plot(x, y, type = "l", col = "blue", lwd = 2, ylim = c(0, 30), xlab = "X", ylab = "Y")
     ```


##### abline
`abline` 是R语言中用于在图形上绘制直线的函数。它通常用于添加一条直线到散点图、箱线图等图形中，以进行可视化分析。下面是 `abline` 函数的主要参数以及一个示例：

**参数：**

- `a`：直线的截距（y轴上的值）。
- `b`：直线的斜率（直线的倾斜程度）。
- `h`：如果指定，表示在横坐标上的位置。
- `v`：如果指定，表示在纵坐标上的位置。
- `coef`：用于指定直线的截距和斜率的向量，通常以 `c(a, b)` 的形式提供。
- `col`：用于指定直线的颜色。
- `lty`：用于指定直线的线型（例如，实线、虚线等）。
- `lwd`：用于指定直线的宽度。

**示例：**

假设我们有一组散点数据，并且我们想在散点图上添加一条直线，以表示某种趋势或拟合线性回归模型的结果。

首先，创建一个简单的散点图：

```R
# 创建示例数据
x <- c(1, 2, 3, 4, 5)
y <- c(3, 6, 8, 10, 13)

# 绘制散点图
plot(x, y, xlim = c(0, 6), ylim = c(0, 15), main = "Scatter Plot")
```

然后，使用 `abline` 函数添加一条直线到散点图上，例如，添加一条通过 `(1, 3)` 和 `(5, 13)` 两点的直线：

```R
# 添加直线
abline(a = 0, b = 2, col = "blue", lty = 2)
```

### 文本
`text` 函数是R语言中用于在绘图中添加文本标签的函数。它允许您在图形上的**特定位置**添加文字注释，用于标识数据点、提供图形的说明、添加标题等等。下面是 `text` 函数的主要参数以及一个示例：

**参数：**

- `x`：文本标签的x坐标位置。
- `y`：文本标签的y坐标位置。
- `labels`：要显示的文本标签的内容，可以是一个字符向量，每个元素对应一个标签。
- `pos`：文本标签的位置，指定为一个整数或字符向量，**表示文本相对于(x, y)坐标的位置**，例如，"topleft"、"bottomright"、1、2、3、4 等。注意这里的(x, y)是我们设置的要标记的点的位置
- `col`：文本标签的颜色。
- `cex`：文本标签的字符大小缩放因子。
- `font`：文本标签的字体样式，可以是整数或字符向量。
- `adj`：文本标签的对齐方式，指定文本在(x, y)坐标周围的对齐方式。
- `...`：其他可选参数，用于进一步自定义文本标签的外观。

**示例：**

```R
# 创建示例数据
x <- c(1, 2, 3, 4, 5)
y <- c(3, 6, 8, 10, 13)

# 绘制散点图
plot(x, y, xlim = c(0, 6), ylim = c(0, 15), main = "Scatter Plot")

# 添加文本标签
labels <- c("Point 1", "Point 2", "Point 3", "Point 4", "Point 5")
text(x, y, labels = labels, pos = 3, col = "blue", cex = 1.2, font = 2)
```

在这个示例中，我们首先绘制了一个散点图，然后使用 `text` 函数在每个数据点的位置上添加了文本标签。`x` 和 `y` 参数指定了文本标签的位置，`labels` 参数包含了要显示的文本内容，`pos` 参数指定了文本标签的位置为 "pos = 3"，这表示文本标签在(x, y)坐标的左下方，`col` 参数设置文本标签的颜色为蓝色，`cex` 参数控制文本标签的字符大小，`font` 参数指定了字体样式。

### 饼图
`pie` 函数是R语言中用于创建饼图（Pie Chart）的函数。饼图是一种常用于表示数据的图形，特别适用于显示各个部分相对于整体的占比或分布情况。以下是 `pie` 函数的主要特点和用法：

- **特点**：
  - 饼图由一个圆形区域表示，整个圆形代表总体或总和。
  - 数据被分成不同的扇形区域，每个扇形区域的大小表示相应数据部分的比例。
  - 饼图常用于可视化各类别或部分在整体中的百分比分布。

- **用法**：
  使用 `pie` 函数创建饼图时，通常需要指定一个数值向量来表示各个部分的大小（比例），并可以自定义其他参数以调整图形的外观。

  下面是 `pie` 函数的一些常见参数：

  - `x`：一个数值向量，表示各个扇形区域的大小（比例）。
  - `labels`：一个字符向量，表示各个扇形区域的标签。
  - `main`：饼图的标题。
  - `col`：各个扇形区域的颜色。
  - `border`：各个扇形区域的边界颜色。
  - `radius`：饼图的半径。
  - `init.angle`：起始角度，可以用于旋转饼图的起始位置。
  - `clockwise`：一个逻辑值，表示饼图的绘制方向是否顺时针（TRUE）或逆时针（FALSE）。

下面是一个示例，演示如何使用 `pie` 函数创建一个简单的饼图：

```R
# 创建示例数据
categories <- c("Category A", "Category B", "Category C")
values <- c(30, 50, 20)

# 创建饼图
pie(values, labels = categories, main = "Pie Chart Example", col = rainbow(length(values)))
```

在这个示例中，我们提供了各个部分的大小（`values`）和标签（`categories`），然后使用 `pie` 函数创建了一个饼图。`col` 参数指定了各个扇形区域的颜色，`main` 参数设置了图形的标题。

### 条形图
`barplot` 函数是R语言中用于创建条形图（Bar Chart）的函数。条形图是一种常用于可视化分类数据的图表，它用垂直或水平的条形表示不同类别的数据，并且条形的高度（或宽度）通常表示数据的数量或频率。以下是 `barplot` 函数的主要特点和用法：

- **特点**：
  - 条形图通常用于显示离散的数据，其中每个条形代表一个不同的类别或组。
  - 可以创建垂直条形图（默认）或水平条形图，具体取决于参数的设置。
  - 条形的高度（或宽度）通常表示某种度量，如频率、计数或百分比。

- **用法**：
  使用 `barplot` 函数创建条形图时，通常需要指定一个数值向量来表示各个类别的数据，并可以自定义其他参数以调整图形的外观。

  下面是 `barplot` 函数的一些常见参数：

  - `height`：一个数值向量，表示条形的高度。它是 `barplot` 函数的必需参数。
  - `names.arg`：一个字符向量，表示每个条形的标签。
  - `beside`：一个逻辑值，用于控制是否绘制多组条形并排显示。
  - `horiz`：一个逻辑值，用于指定是否创建水平条形图。
  - `col`：条形的颜色。
  - `border`：条形的边界颜色。
  - `main`：条形图的标题。

下面是一个示例，演示如何使用 `barplot` 函数创建一个简单的垂直条形图：

```R
# 创建示例数据
categories <- c("Category A", "Category B", "Category C")
values <- c(30, 50, 20)

# 创建垂直条形图
barplot(values, names.arg = categories, main = "Vertical Bar Chart Example", col = "blue")
```

在这个示例中，我们提供了各个类别的数据（`values`）和标签（`categories`），然后使用 `barplot` 函数创建了一个垂直条形图。`names.arg` 参数用于设置每个条形的标签，`col` 参数设置了条形的颜色，`main` 参数设置了图形的标题。

### 直方图
用的最多，最佳能看出随机变量的分布
分组要合理，不然有的组就没有了
`hist` 函数是R语言中用于创建直方图（Histogram）的函数。直方图是一种用于可视化数据分布的图形，它将数据分成一系列称为“组”或“箱子”的区间，并显示每个区间中数据点的频率或计数。以下是 `hist` 函数的主要用法和参数：

**语法：**

```R
hist(x, breaks = "Sturges", freq = NULL, probability = FALSE, 
     include.lowest = TRUE, right = TRUE, labels = FALSE, 
     main = NULL, xlab = NULL, ylab = "Frequency",
     xlim = NULL, ylim = NULL, axes = TRUE)
```
**参数说明：**

- `x`：要创建直方图的数值向量。
- `breaks`：**定义组（箱子）的方法。可以是一个整数，表示组的数量，也可以是一个向量，指定每个组的分界点**。
- `freq`：一个逻辑值，表示是否显示频数（计数）。如果为 TRUE，则纵轴表示频数，如果为 FALSE（默认），则表示频率。
- `probability`：一个逻辑值，表示是否绘制概率密度直方图。如果为 TRUE，则直方图的面积将等于1。
- `include.lowest`：一个逻辑值，表示是否将最低值包含在第一个组中。
- `right`：一个逻辑值，表示是否将组的右边界作为闭区间。如果为 TRUE（默认），则组是右闭的。
- `labels`：一个逻辑值，表示是否添加组标签。
- `main`：直方图的主标题。
- `xlab`：X轴标签。
- `ylab`：Y轴标签。
- `xlim`：X轴的范围。
- `ylim`：Y轴的范围。
- `axes`：一个逻辑值，表示是否绘制坐标轴。

**示例：**

下面是一个简单的示例，演示如何使用 `hist` 函数创建一个直方图：

```R
# 创建示例数据
data <- c(22, 34, 45, 56, 66, 77, 89, 92, 102, 115, 126, 138, 145)

# 创建直方图
hist(data, breaks = c(0, 50, 100, 150), main = "Histogram Example", xlab = "Value", ylab = "Frequency", freq = F)
```

在这个示例中，我们提供了一个数据向量 `data`，然后使用 `hist` 函数创建了一个包含5个组的直方图。可以通过修改参数来自定义直方图的外观和标签。

#### 核密度函数
记得要`freq = F`

核密度估计（Kernel Density Estimation，KDE）是一种用于估计连续随机变量的概率密度函数的非参数方法。在R语言中，你可以使用 `density` 函数来执行核密度估计。以下是 `density` 函数的使用方法和一些相关的参数：

**语法：**

```R
density(x, bw = "nrd0", kernel = "gaussian", n = 512, from = min(x), to = max(x))
```

**参数说明：**

- `x`：要估计密度的数值向量。
- `bw`：带宽（bandwidth）参数，用于控制核密度估计的平滑程度。可以是以下几个选项之一：
  - "nrd0"：使用默认带宽。
  - 数值：手动指定带宽值。
- `kernel`：核函数的类型，用于控制估计过程中的平滑性。常见的选项包括：
  - "gaussian"（默认）：高斯核函数。
  - "rectangular"：矩形核函数。
  - "triangular"：三角核函数。
  - "epanechnikov"：Epanechnikov核函数。
  - 等等。
- `n`：用于估计密度的点的数量。
- `from` 和 `to`：用于指定估计密度的范围。

**示例：**

下面是一个示例，演示如何使用 `density` 函数执行核密度估计并可视化结果：

```R
# 创建示例数据
data <- c(22, 34, 45, 56, 66, 77, 89, 92, 102, 115, 126, 138, 145)

# 执行核密度估计
density_estimation <- density(data)

# 可视化核密度估计结果
plot(density_estimation, main = "Kernel Density Estimation", xlab = "Value", ylab = "Density")
```

在这个示例中，我们首先创建了一个示例数据向量 `data`，然后使用 `density` 函数对数据进行核密度估计。最后，我们使用 `plot` 函数可视化了核密度估计的结果。

核密度估计是一种有用的工具，用于理解和可视化连续随机变量的概率分布。通过调整带宽参数和核函数类型，您可以对估计的平滑度进行微调，以便更好地适应数据的特性。这可以帮助您在数据分析中发现隐藏的分布模式和特征。

### 箱线图
`boxplot`
- 上下是最大/最小值
- 中位数
- 箱子的上下是25%和75%

**boxplot** 函数是R语言中用于创建箱线图（Box Plot）的函数。箱线图是一种用于可视化数据分布和离群值的图表。箱线图显示了数据的中位数、四分位数（上下四分位数），以及可能存在的离群值。以下是 `boxplot` 函数的主要用法和参数：

**语法**：

```R
boxplot(x, data, notch = FALSE, varwidth = FALSE, names = NULL, 
        outline = TRUE, horizontal = FALSE, add = FALSE, at = NULL, 
        ...)
```

**参数说明**：

- `x`：要创建箱线图的数值向量或数据框（DataFrame）。
- `data`：可选参数，指定包含数据的数据框，如果 `x` 是变量名称而不是向量，则需要提供 `data` 参数。
- `notch`：一个逻辑值，表示是否在箱体的中间添加缺口以表示**置信区间**。默认为 FALSE。
- `varwidth`：一个逻辑值，表示是否使用箱子的宽度表示数据集大小的差异。默认为 FALSE。
- `names`：用于指定每个箱线图的标签的字符向量。
- `outline`：一个逻辑值，表示是否显示离群值。如果为 TRUE（默认），则离群值将以点的形式显示。
- `horizontal`：一个逻辑值，表示是否创建水平箱线图。如果为 TRUE，则箱线图将水平显示。
- `add`：一个逻辑值，表示是否将箱线图添加到现有图形中。如果为 TRUE，则将箱线图添加到当前图形。
- `at`：可选参数，指定箱线图的位置。如果提供了 `at` 参数，则可以在同一图中绘制多个箱线图。

**示例**：

以下是一个示例，演示如何使用 `boxplot` 函数创建一个垂直箱线图：

```R
# 创建示例数据
data <- list( group1 = c(22, 34, 45, 56, 66),
              group2 = c(77, 89, 92, 102, 115, 126),
              group3 = c(138, 145, 160, 175))

# 创建垂直箱线图
boxplot(data, names = c("Group 1", "Group 2", "Group 3"), 
        main = "Box Plot Example", xlab = "Groups", ylab = "Values")
```

在这个示例中，我们使用 `boxplot` 函数创建了一个垂直箱线图，显示了三个不同组的数据分布。通过修改参数，您可以自定义箱线图的外观和标签，以满足特定的可视化需求。

### 其他函数
- **fivenum**：`fivenum` 是R语言中的一个函数，用于计算一个数值向量的五数概括（Five-Number Summary）。这五个数字包括最小值、**第一四分位数（Q1）、中位数（Q2）、第三四分位数（Q3）和最大值**。五数概括可以帮助我们了解数据的中心趋势、离散程度和分布形状。
  - **fivenum***的示例：

    ```R
    # 创建示例数据
    data <- c(10, 15, 20, 25, 30, 35, 40)

    # 计算五数概括
    fivenum(data)
    ```

    结果将显示：

    ```
    [1] 10 20 25 32.5 40
    ```

    这表示示例数据的最小值为10，第一四分位数为20，中位数为25，第三四分位数为32.5，最大值为40。

- **summary**：`summary` 是R语言中的一个函数，用于生成有关数据集的摘要统计信息。`summary` 函数可以应用于各种不同类型的数据，包括数值型和因子型数据。对于数值型数据，`summary` 函数通常提供了**平均值、中位数、最小值、最大值以及第一四分位数和第三四分位数**等统计量。对于因子型数据，它提供了每个因子水平的计数。
  - **summary** 的示例：

    ```R
    # 创建示例数据
    data <- c(12, 18, 24, 30, 36)

    # 生成摘要统计信息
    summary(data)
    ```

    结果将显示：

    ```
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    12.00   18.00   24.00   24.00   30.00   36.00 
    ```

    这表示示例数据的最小值为12，第一四分位数为18，中位数为24，平均值为24，第三四分位数为30，最大值为36。

- **quantile**：`quantile` 是R语言中的一个函数，用于计算数值向量的分位数。分位数是将数据分成若干等分的值，通常用于了解数据的分布情况。`quantile` 函数可以计算指定分位数的值，例如，**第一四分位数（25th分位数）、中位数（50th分位数）和第三四分位数（75th分位数）**。这些分位数帮助我们理解数据的集中度和分散度。

  - **quantile** 的示例：

    ```R
    # 创建示例数据
    data <- c(15, 20, 25, 30, 35, 40, 45)

    # 计算第一四分位数和第三四分位数
    quantile(data, c(0.25, 0.75))
    ```

    结果将显示：

    ```
    25% 75% 
    22.5  37.5 
    ```

    这表示示例数据的第一四分位数（25th分位数）为22.5，第三四分位数（75th分位数）为37.5。

### 函数图
`curve(func)`
想要多个函数画在同一张图中，后面的`curve`调用利用`add=TRUE`属性
`curve` 函数是 R 语言中用于绘制曲线图的函数之一。它的主要作用是根据给定的数学函数或表达式，生成并绘制相应的曲线。

以下是 `curve` 函数的基本用法以及常用参数的说明：

```R
curve(expr, from = NULL, to = NULL, n = 101, add = FALSE, type = "l", xname = "x", yname = NULL, ...)
```

参数说明：

- `expr`: 要绘制的函数或表达式，通常是关于 `x` 的表达式。
- `from`: x 轴的起始值。
- `to`: x 轴的结束值。
- `n`: 生成曲线上的点的数量，用于控制曲线的光滑程度。
- `add`: 一个逻辑值，如果为 TRUE，则将曲线添加到现有的图形中，否则会创建一个新的图形。
- `type`: 绘图的类型，通常为 "l"（线条）或 "p"（点）。
- `xname`: x 轴的标签。
- `yname`: y 轴的标签。
- `...`: 其他传递给绘图函数的参数。

示例：

以下是一个示例，演示如何使用 `curve` 函数绘制函数 `y = x^2` 的曲线：

```R
# 绘制 y = x^2 的曲线，x 从 -2 到 2
curve(x^2, from = -2, to = 2, n = 100, type = "l", xlab = "x", ylab = "y", main = "y = x^2 Curve")

# 第二个示例
myfun=function(xvar) {
    1/(1+exp(-xvar+10))
}
curve(myfun(x),from=0,to=20)
curve(1-myfun(x),add=T,col="red")
```

### 布局layout
`par(mfrow=VECTOR)`定义多少行多少列
`par` 函数是 R 语言中用于设置图形参数的函数。它允许您控制图形的各种属性，如图形大小、颜色、字体、坐标轴、标签等。通过设置这些参数，您可以自定义绘图的外观和布局。

以下是 `par` 函数的基本用法以及一些常用参数的说明：

```R
par(...)
```

`...` 表示可以接受多个参数，每个参数都是以 `name=value` 的形式指定，用于设置不同的图形参数。以下是一些常用的图形参数和它们的说明：

- `mfrow`: 一个长度为2的向量，用于指定图形布局的行数和列数。例如，`mfrow=c(2, 2)` 将图形分为2行2列，可以同时绘制4个图。
- `mfcol`: 与 `mfrow` 类似，但是按照列优先的方式布局图形。
- `mar`: 一个长度为4的数值向量，指定图形的边距，包括下、左、上、右边距。
- `oma`: 一个长度为4的数值向量，指定外部边距，包括下、左、上、右边距。
- `par(mfrow=c(2, 2))` ：设置图形布局为2行2列。
- `par(col="blue")`：设置图形线条的颜色为蓝色。
- `par(cex=1.5)`：设置文本和点的缩放因子为1.5，使它们变大。
- `par(pch=19)`：设置点的形状为实心圆。
- `par(lwd=2)`：设置线条的宽度为2。
- `par(xpd=TRUE)`：允许绘制超出图形区域的内容。

要重置图形参数为默认值，可以使用 `par(reset=TRUE)`。

示例：

以下示例演示如何使用 `par` 函数设置图形参数，包括布局、颜色和文本大小：

```R
# 设置图形布局为2行2列
par(mfrow=c(2, 2))

# 设置线条颜色为红色
par(col="red")

# 设置文本大小为1.2倍
par(cex=1.2)

# 绘制四个简单的示例图形
plot(1:10, main="Plot 1")
plot(1:10, col="blue", main="Plot 2")
plot(1:10, pch=19, main="Plot 3")
plot(1:10, col="green", pch=3, main="Plot 4")
```

这个示例将图形分为2行2列，并分别设置了线条颜色和文本大小，然后绘制了四个简单的图形。
## ggplot
- 核心理念是将绘图和数据分离
- 按图层作图
- 按图层构建图形
- 绘图命令不能独立使用，必须与画布配合

## 绘图命令
在 ggplot2 中，`+` 是用来将不同的图层（layers）组合在一起的操作符。这使得您可以在同一个图形中叠加多个几何对象或修改图形的不同方面，从而创建复杂的可视化图表。

例如，您可以先创建一个基本的 ggplot 对象，然后使用 `+` 添加几何对象、调整主题、修改标签等，以逐步完善图形。

示例：

```R
library(ggplot2)

# 创建一个基本的散点图
p <- ggplot(data = mpg, aes(x = displ, y = hwy)) + geom_point()

# 添加线性回归线
p <- p + geom_smooth(method = "lm", se = FALSE)

# 修改坐标轴标签
p <- p + labs(x = "Displacement (L)", y = "Highway MPG")

# 修改主题
p <- p + theme_minimal()

# 显示图形
print(p)
```

在这个示例中，首先创建了一个散点图 `p`，然后使用 `+` 逐步添加了线性回归线、修改了坐标轴标签和主题。

另外，`aes`（aesthetic）函数用于指定**图形的美学映射**，也就是将数据变量映射到图形的可视化属性上，比如位置、颜色、大小等。在 ggplot2 中，几乎所有的图层都会使用 `aes` 来映射数据到图形的属性。

示例：

```R
library(ggplot2)

# 创建一个散点图，并映射汽车排量到 x 轴，公路里程到 y 轴
ggplot(data = mpg, aes(x = displ, y = hwy)) + geom_point()
```

在这个示例中，`aes(x = displ, y = hwy)` 指定了 x 轴用 `displ` 数据，y 轴用 `hwy` 数据。

### geom_XXX(aes, alpha=, position)
几何绘图

以下是一些常用的 ggplot2 函数和它们的功能以及示例：

1. **geom_area（面积图）**：用于创建面积图，通常用来表示累积数据趋势。

   ```R
   library(ggplot2)
   
   data <- data.frame(x = 1:5, y = c(2, 4, 1, 6, 3))
   
   ggplot(data, aes(x = x, y = y)) +
     geom_area()
   ```

2. **geom_bar（条形图）**：用于创建条形图，用来表示类别数据的分布。

   ```R
   library(ggplot2)
   
   data <- data.frame(category = c("A", "B", "C", "D"), count = c(15, 8, 12, 20))
   
   ggplot(data, aes(x = category, y = count)) +
     geom_bar(stat = "identity")
   ```

3. **geom_boxplot（箱线图）**：用于创建箱线图，展示数据的分布和离散度。

   ```R
   library(ggplot2)
   
   data <- data.frame(group = rep(1:3, each = 50), value = rnorm(150))
   
   ggplot(data, aes(x = factor(group), y = value)) +
     geom_boxplot()
   ```

4. **geom_contour（等高线图）**：用于创建等高线图，常用于二维密度估计。

   ```R
   library(ggplot2)
   
   x <- seq(-2, 2, length = 100)
   y <- seq(-2, 2, length = 100)
   z <- outer(x, y, function(x, y) x^2 + y^2)
   
   data <- expand.grid(x = x, y = y)
   data$z <- as.vector(z)
   
   ggplot(data, aes(x = x, y = y, z = z)) +
     geom_contour()
   ```

5. **geom_density（密度图）**：用于创建核密度估计图，展示连续变量的分布。

   ```R
   library(ggplot2)
   
   data <- data.frame(value = rnorm(100))
   
   ggplot(data, aes(x = value)) +
     geom_density()
   ```

6. **geom_errorbar（误差线）**：通常添加到其他图形上，用来表示数据的不确定性。

   ```R
   library(ggplot2)
   
   data <- data.frame(group = c("A", "B", "C"), mean = c(5, 8, 6), sd = c(1, 2, 1))
   
   ggplot(data, aes(x = group, y = mean)) +
     geom_bar(stat = "identity") +
     geom_errorbar(aes(ymin = mean - sd, ymax = mean + sd), width = 0.2)
   ```

7. **geom_histogram（直方图）**：用于创建直方图，展示数据的分布。

   ```R
   library(ggplot2)
   
   data <- data.frame(value = rnorm(100))
   
   ggplot(data, aes(x = value)) +
     geom_histogram(binwidth = 0.5, fill = "blue", color = "black")
   ```

8. **geom_jitter（点图）**：用于创建点图，自动添加了扰动，用于展示数据的分布。

   ```R
   library(ggplot2)
   
   data <- data.frame(category = rep(c("A", "B"), each = 50), value = rnorm(100))
   
   ggplot(data, aes(x = category, y = value)) +
     geom_jitter(width = 0.2, height = 0, alpha = 0.7)
   ```

9. **geom_line（线）**：用于创建线图，用来表示数据的趋势。

   ```R
   library(ggplot2)
   
   data <- data.frame(x = 1:5, y = c(2, 4, 1, 6, 3))
   
   ggplot(data, aes(x = x, y = y)) +
     geom_line()
   ```

10. **geom_point（散点图）**：用于创建散点图，用来表示两个变量之间的关系。

    ```R
    library(ggplot2)
   
    data <- data.frame(x = 1:20, y = rnorm(20))
   
    ggplot(data, aes(x = x, y = y)) +
      geom_point()
    ```

11. **geom_text（文本）**：用于在图上添加文本标签。

    ```R
    library(ggplot2)
   
    data <- data.frame(x = 1:5, y = c(2, 4, 1, 6, 3), label = c("A", "B", "C", "D", "E"))
   
    ggplot(data, aes(x = x, y = y, label = label)) +
      geom_point() +
      geom_text()
    ```


### stat_XXX()
统计绘图

以下是一些 ggplot2 中常用的统计图形函数 (`stat_`) 及其功能的介绍和示例：

1. **stat_abline**：用来添加一条直线，通常通过斜率和截距来表示。

   示例：
   ```R
   library(ggplot2)
   
   data <- data.frame(x = 1:10, y = 2 * (1:10) + 1)
   
   ggplot(data, aes(x = x, y = y)) +
     geom_point() +
     stat_abline(intercept = 1, slope = 2, color = "red")
   ```

2. **stat_boxplot**：用于创建带有触须的箱线图，展示数据的分布和离散度。

   示例：
   ```R
   library(ggplot2)
   
   data <- data.frame(group = rep(1:3, each = 50), value = rnorm(150))
   
   ggplot(data, aes(x = factor(group), y = value)) +
     geom_boxplot() +
     stat_boxplot(geom = "errorbar", width = 0.5, color = "blue")
   ```

3. **stat_contour**：用于创建三维数据的等高线图。

   示例：
   ```R
   library(ggplot2)
   
   x <- seq(-2, 2, length = 100)
   y <- seq(-2, 2, length = 100)
   z <- outer(x, y, function(x, y) x^2 + y^2)
   
   data <- expand.grid(x = x, y = y)
   data$z <- as.vector(z)
   
   ggplot(data, aes(x = x, y = y, z = z)) +
     stat_contour()
   ```

4. **stat_density**：用于创建核密度估计图，展示连续变量的分布。

   示例：
   ```R
   library(ggplot2)
   
   data <- data.frame(value = rnorm(100))
   
   ggplot(data, aes(x = value)) +
     geom_density() +
     stat_density(geom = "line", color = "blue")
   ```

5. **stat_density2d**：用于创建二维密度图，展示二维数据的分布。

   示例：
   ```R
   library(ggplot2)
   
   data <- data.frame(x = rnorm(100), y = rnorm(100))
   
   ggplot(data, aes(x = x, y = y)) +
     geom_point() +
     stat_density2d(aes(fill = ..level..), geom = "polygon") +
     scale_fill_gradient(low = "white", high = "blue")
   ```

6. **stat_function**：用于添加函数曲线，可以通过指定函数来生成曲线。

   示例：
   ```R
   library(ggplot2)
   
   ggplot() +
     stat_function(fun = function(x) sin(x), xlim = c(0, 2 * pi), color = "red") +
     stat_function(fun = function(x) cos(x), xlim = c(0, 2 * pi), color = "blue")
   ```

7. **stat_hline**：用于添加水平线。

   示例：
   ```R
   library(ggplot2)
   
   ggplot() +
     stat_hline(yintercept = 0, color = "green", linetype = "dashed") +
     geom_point(data = data.frame(x = 1:5, y = c(2, 4, 1, 6, 3)), aes(x = x, y = y))
   ```

8. **stat_smooth**：用于添加平滑曲线，通常用于拟合趋势线。

   示例：
   ```R
   library(ggplot2)
   
   data <- data.frame(x = 1:10, y = c(2, 4, 1, 6, 3, 5, 8, 7, 9, 10))
   
   ggplot(data, aes(x = x, y = y)) +
     geom_point() +
     stat_smooth(method = "lm", se = FALSE, color = "red")
   ```

### scale_XXX()
标度绘图