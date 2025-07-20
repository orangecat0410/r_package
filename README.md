# mycolor
An R package that can extract colors from images

该R脚本提供从图片中提取代表色的功能，支持单张图片或整个文件夹批量处理，并提供颜色后处理功能。

功能概览

1. 核心提取函数

• K-means聚类法 (dominant_colors 和 dominant_dir_colors)：

  • 通过K-means聚类提取主要颜色

  • 参数：k（颜色数量），nowhite（是否过滤白色）

• 频率阈值法 (get_colors 和 get_dir_colors)：

  • 基于像素颜色频率提取代表色

  • 参数：threshold（颜色最小占比），nowhite（是否过滤白色）

2. 颜色后处理函数

• mycolor2：

  • 对颜色向量进行排序和采样

  • 支持三种排序方式：色阶谱(level)、明暗度(depth)、原始顺序(raw)

  • 支持三种采样方式：随机(radom)、等距(Equidistant)、邻近采样(neighbor)

安装方法

install.packages(c("png", "ggplot2", "scales"))


使用示例

1. 单张图片处理

# K-means法提取
colors_kmeans <- dominant_colors("图片路径.png", k=8, nowhite=TRUE)
scales::show_col(colors_kmeans)

# 频率阈值法提取
colors_freq <- get_colors("图片路径.png", threshold=0.02, nowhite=TRUE)


2. 文件夹批量处理

# 处理整个文件夹
folder_results <- get_dir_colors("图片文件夹路径", threshold=0.01)

# 合并所有颜色
all_colors <- unlist(folder_results)


3. 颜色后处理

# 随机抽取40个颜色（按色阶排序）
color_subset <- mycolor2(
  yourcolor = all_colors,
  NUM = 40,
  source = "level",
  selection = "radom",
  seed = 123
)


4. 高级工作流

# 1. 提取文件夹所有颜色
results <- get_dir_colors("pic", threshold=0.01)

# 2. 合并颜色并排序
combined <- unlist(results)
sorted_colors <- mycolor2(combined, NUM=100, source="level", selection="Equidistant")

# 3. 可视化结果
scales::show_col(sorted_colors)


参数详解

提取函数参数

参数 类型 说明

k 整数 提取的颜色数量 (默认5)

threshold 数值 颜色最小占比阈值 (默认0.01)

nowhite 逻辑值 是否过滤#FFFFFF类白色 (默认TRUE)
后处理函数参数
参数 说明

NUM 输出颜色数量 (必须<500)

source 排序方式：level(色阶谱)/depth(明暗度)/raw(原始顺序)

selection 采样方式：radom(随机)/Equidistant(等距)/neighbor(邻近连续采样)

seed 随机种子 (NA表示不固定)

start_pre 邻近采样的起始位置比例 (0-1之间)

注意事项

1. 当前仅支持PNG格式图片
2. 白色过滤基于正则表达式 "^.(F).(F).(F)"
3. 使用mycolor2时建议先合并多个图片的颜色向量
4. 可视化依赖scales::show_col()函数

示例效果

 (注：此处添加示例图片链接)

将此内容保存为README.md放在GitHub仓库根目录即可。文档包含：
• 功能摘要

• 安装说明

• 参数详解表

• 完整使用示例

• 注意事项

建议补充实际运行截图作为示例效果图，让用户更直观了解输出结果。
