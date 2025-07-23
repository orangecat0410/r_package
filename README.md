# mycolor
An R package that can extract colors from images

该R脚本提供从图片中提取代表色的功能，支持单张图片或整个文件夹批量处理，并提供颜色后处理功能。

# 安装方法
install.packages("https://github.com/orangecat0410/r_package/raw/main/mycolor_0.0.0.9000.tar.gz",
                 repos = NULL,
                 type = "source")

# 功能概述
这个R包提供了一系列从图像中提取主要（代表）颜色的功能。它支持单张PNG图像或整个文件夹中的PNG图像。提取颜色的方法有两种：一种是基于K-means聚类的代表色提取，另一种是基于颜色频率的阈值提取。此外，还提供了颜色后处理功能，如去除白色、颜色排序和选择等。

# 函数详细说明及示例
# 1. dominant_colors
​​功能​​：使用K-means聚类从单张PNG图像中提取代表色。

​​参数​​：

image_path: PNG图片的路径。
k: 生成的颜色数量（聚类中心数）。
nowhite: 是否去除纯白色（接近#FFFFFF的颜色）。
​​返回​​：一个包含代表色的十六进制颜色代码向量。

​​示例​​：

#提取5种代表色，并去除白色
colors <- dominant_colors("path/to/image.png", k=5, nowhite=TRUE)
print(colors)


# 2. get_colors
​​功能​​：基于颜色频率和阈值从单张PNG图像中提取颜色。

​​参数​​：

file_path: PNG图片的路径。
threshold: 颜色比例阈值（0到1之间的浮点数），只有比例超过此阈值的颜色才会被保留。
nowhite: 是否去除纯白色。
​​返回​​：一个包含符合条件的十六进制颜色代码向量（按频率降序排列）。

​​示例​​：

#提取频率超过1%的颜色，并去除白色
colors <- get_colors("path/to/image.png", threshold=0.01, nowhite=TRUE)
print(colors)


# 3. dominant_dir_colors
​​功能​​：从指定文件夹中的所有PNG图像中提取代表色（使用K-means方法）。

​​参数​​：

folder_path: 包含PNG图片的文件夹路径。
k: 每张图片生成的颜色数量。
nowhite: 是否去除纯白色。
toc: 是否将结果合并为一个向量（TRUE）还是保留为每个图片一个向量的列表（FALSE）。
​​返回​​：如果toc=TRUE，则返回合并后的颜色向量；否则，返回一个列表，每个元素对应一张图片的颜色向量。

​​示例​​：

#从文件夹中每张图片提取5种代表色，去除白色，结果合并为一个向量
all_colors <- dominant_dir_colors("path/to/folder", k=5, nowhite=TRUE, toc=TRUE)
print(all_colors)

#保留为列表
color_list <- dominant_dir_colors("path/to/folder", k=5, nowhite=TRUE, toc=FALSE)
print(color_list)

# 4. get_dir_colors
​​功能​​：从指定文件夹中的所有PNG图像中基于频率阈值提取颜色。

​​参数​​：

folder_path: 包含PNG图片的文件夹路径。
threshold: 颜色比例阈值（0到1之间的浮点数）。
nowhite: 是否去除纯白色。
toc: 是否将结果合并为一个向量（TRUE）还是保留为每个图片一个向量的列表（FALSE）。
​​返回​​：如果toc=TRUE，则返回合并后的颜色向量；否则，返回一个列表，每个元素对应一张图片的颜色向量。

​​示例​​：

#从文件夹中每张图片提取频率超过1%的颜色，去除白色，结果合并为一个向量
all_colors <- get_dir_colors("path/to/folder", threshold=0.01, nowhite=TRUE, toc=TRUE)
print(all_colors)

#保留为列表
color_list <- get_dir_colors("path/to/folder", threshold=0.01, nowhite=TRUE, toc=FALSE)
print(color_list)


# 5. mycolor2
​​功能​​：从一个颜色列表（向量）中选择指定数量的颜色，并支持不同的排序和选择方法。

​​参数​​：

yourcolor: 包含十六进制颜色代码的向量（必须是以#开头的6位或8位代码）。
NUM: 需要选择的颜色数量（必须小于输入向量的颜色总数）。
source: 原始颜色的排序方式，可选：
"raw": 原始顺序（默认）。
"level": 按色相（H）排序，然后按饱和度（S）和明度（V）降序。
"depth": 按颜色深度（明度）排序（由浅到深）。
selection: 选择方式，可选：
"Equidistant": 等间距选择（在排序后的颜色向量上等距取点）。
"radom": 随机选择。
"neighbor": 选择相邻的一段颜色（需要指定起始位置或随机起始）。
seed: 随机种子（用于随机选择或等间距的随机起始），默认为NA（不使用）。
start_pre: 当selection="neighbor"时，指定起始位置的比例（0到1之间），默认为NA（使用随机起始位置）。
​​返回​​：一个包含选定颜色的向量。

​​示例​​：

#假设有一个颜色向量color_vector
#如果第一个变量为向量外的其他值，则自动使用函数内置的色表
#按色相排序后，随机等间距选择40个颜色
selected_colors <- mycolor2(color_vector, NUM=40, source="level", selection="Equidistant", seed=123, start_pre=NA)

#按深度排序，随机选择40个颜色
selected_colors <- mycolor2(color_vector, NUM=40, source="depth", selection="radom", seed=123)

#按原始顺序，选择相邻的40个颜色，从中间位置开始
selected_colors <- mycolor2(color_vector, NUM=40, source="raw", selection="neighbor", start_pre=0.5)
# 注意事项
函数中使用了png包读取图像，scales包用于显示颜色。请确保已安装这些包。
在mycolor2函数中，如果选择等间距或随机选择，设置随机种子（seed）可以保证结果可重现。
去除白色（nowhite=TRUE）是通过匹配十六进制代码接近"#FFFFFF"的颜色实现的。注意：该方法可能无法去除所有白色背景（例如，接近白色但不是#FFFFFF的颜色），但可以去除纯白。
# 作者
Full sugar white bread rolls
