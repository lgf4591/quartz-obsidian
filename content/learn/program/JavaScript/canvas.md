---
aliases: [Canvas ]
author: lgf
created: 45_2022-11-12 19:15:01
modified: 45_2022-11-12 19:54:53
tags: [learn/program/javascript]
title: Canvas 
---
# Canvas
## 基础知识

Canvas 是用使用 JS 画布的思想来绘制图形，下面通过一些示例掌握 Canvas 的使用

> 向军大叔每晚八点在 [抖音 (opens new window)](https://live.douyin.com/houdunren) 和 [bilibli (opens new window)](https://space.bilibili.com/282190994) 直播

![xj-small](https://doc.houdunren.com/assets/img/xj.161cc3f2.jpg)

### 项目模板

以下示例因为使用到了 Typescript，所以我们使用 vite 创建 typescript 项目，并选择使用 `vanilla` 模板来开发

项目安装执行结果

目录结构

## 矩形绘制

下面来学习使用 strokeRect 方法绘制边框矩形

### 实心矩形

使用 fillRect 方法可以绘制实心矩形，下面是 fillRect 方法的参数说明

| 参数   | 说明                 |
| ------ | -------------------- |
| x      | 矩形左上角的 x 坐标  |
| y      | 矩形左上角的 y 坐标  |
| width  | 矩形的宽度，以像素计 |
| height | 矩形的高度，以像素计 |

下面使用纯色填充画布

![image-20210816175329529](https://doc.houdunren.com/assets/img/image-20210816175329529.b7fa8cf9.png)

### 空心矩形

使用 strokeRect 方法可以绘制空心矩形，下面是 strokeRect 方法的参数说明

| 参数   | 说明                 |
| ------ | -------------------- |
| _x_    | 矩形左上角的 x 坐标  |
| _y_    | 矩形左上角的 y 坐标  |
| width  | 矩形的宽度，以像素计 |
| height | 矩形的高度，以像素计 |

下面绘制实线边框的示例代码

![image-20210817033803269](https://doc.houdunren.com/assets/img/image-20210817033803269.7240fc50.png)

## 圆形绘制

使用 canvas 可以绘制圆形

### Arc

下面是绘制圆方法 arc 的参数说明

| 参数               | 说明                                                                |
| ------------------ | ------------------------------------------------------------------- |
| x                  | 圆的中心的 x 坐标。                                                 |
| _y_                | 圆的中心的 y 坐标。                                                 |
| _r_                | 圆的半径。                                                          |
| _sAngle_           | 起始角，以弧度计。（弧的圆形的三点钟位置是 0 度）。                 |
| _eAngle_           | 结束角，以弧度计。                                                  |
| _counterclockwise_ | 可选。规定应该逆时针还是顺时针绘图。False = 顺时针，true = 逆时针。 |

### 绘制空心圆

![image-20210820165631351](https://doc.houdunren.com/assets/img/image-20210820165631351.ccc17509.png)

### 绘制实心圆

下面来掌握使用 canvas 绘制填充圆，绘制圆使用 arc 函数，具体参数说明参考上例。

![image-20210820165947519](https://doc.houdunren.com/assets/img/image-20210820165947519.0b10586f.png)

## 节点绘制

我们可以通过以下方法定义不同节点、线条样式来绘制图形

-   beginPath() 重置绘制路径
-   lineTo() 开始绘制线条
-   moveTo() 把路径移动到画布中的指定点，但不会创建线条(lineTo 方法会绘制线条)
-   closePath() 闭合线条绘制，即当前点连接到线条开始绘制点
-   lineWidth 线条宽度
-   strokeStyle 线条的样式，可以是颜色 、渐变
-   stroke() 根据上面方法定义的节点绘制出线条

### 绘制多边形

下面是根据节点来绘制三角形图形

![image-20210817033440109](https://doc.houdunren.com/assets/img/image-20210817033440109.6fa7dba4.png)

## 线性渐变

使用 canvas 的 createLinearGradient() 方法可以创建线性的渐变对象，用于实现线性渐变效果。

### createLinearGradient

下面是 createLinearGradient 线性渐变的参数

| 参数 | 描述                |
| ---- | ------------------- |
| x0   | 渐变开始点的 x 坐标 |
| y0   | 渐变开始点的 y 坐标 |
| x1   | 渐变结束点的 x 坐标 |
| y1   | 渐变结束点的 y 坐标 |

### 渐变边框

下面是绘制激变的边框的效果

![image-20210816175218929](https://doc.houdunren.com/assets/img/image-20210816175218929.b1826e65.png)

### 渐变填充

渐变也可以用于填充

![image-20210816185552244](https://doc.houdunren.com/assets/img/image-20210816185552244.e6eac755.png)

## 清空区域

下面是将红色画布上清除一块区域，清除后的内容是透明的。

![image-20210819015538839](https://doc.houdunren.com/assets/img/image-20210819015538839.d17ab022.png)

## 填充文字

下面掌握使用 canvas 的 fillText 方法绘制填充文字

### fillText

下面是 fillText 方法的参数

| 参数       | 描述                                      |
| ---------- | ----------------------------------------- |
| _text_     | 规定在画布上输出的文本。                  |
| _x_        | 开始绘制文本的 x 坐标位置（相对于画布）。 |
| _y_        | 开始绘制文本的 y 坐标位置（相对于画布）。 |
| _maxWidth_ | 可选。允许的最大文本宽度，以像素计。      |

### textBaseline

textBaseline 用于定义文字基线

| 参数        | 说明                             |
| ----------- | -------------------------------- |
| alphabetic  | 默认。文本基线是普通的字母基线。 |
| top         | 文本基线是 em 方框的顶端。。     |
| hanging     | 文本基线是悬挂基线。             |
| middle      | 文本基线是 em 方框的正中。       |
| ideographic | 文本基线是表意基线。             |
| bottom      | 文本基线是 em 方框的底端。       |

### textAlign

textAlign 用于文本的对齐方式的属性

| 参数   | 说明                                                                  |
| ------ | --------------------------------------------------------------------- |
| left   | 文本左对齐                                                            |
| right  | 文本右对齐                                                            |
| center | 文本居中对齐                                                          |
| start  | 文本对齐界线开始的地方 （左对齐指本地从左向右，右对齐指本地从右向左） |
| end    | 文本对齐界线结束的地方 （左对齐指本地从左向右，右对齐指本地从右向左） |

### 示例代码

![image-20210821030506745](https://doc.houdunren.com/assets/img/image-20210821030506745.c6ddab7d.png)

### 激变文字

![image-20210816185904257](https://doc.houdunren.com/assets/img/image-20210816185904257.6c21c2cf.png)

## 图片填充

下面掌握将图片填充到画布

### 参数说明

| 参数      | 描述                               |
| --------- | ---------------------------------- |
| image     | 规定要使用的图片、画布或视频元素。 |
| repeat    | 默认。该模式在水平和垂直方向重复。 |
| repeat-x  | 该模式只在水平方向重复。           |
| repeat-y  | 该模式只在垂直方向重复。           |
| no-repeat | 该模式只显示一次（不重复）。       |

### 示例代码

![image-20210816213728363](https://doc.houdunren.com/assets/img/image-20210816213728363.3ef9d578.png)

## 图片缩放

下面将图片直接绘制到画布上。

![image-20210816214705537](https://doc.houdunren.com/assets/img/image-20210816214705537.9a641256.png)

## 绘制像素

下面是绘制像素点的示例

![image-20210816215724423](https://doc.houdunren.com/assets/img/image-20210816215724423.3fc16b8f.png)

### 绘制不规则

![image-20220123141057183](https://doc.houdunren.com/assets/img/image-20220123141057183.cae0153f.png)

## 黑板实例

下面我们为开发个小黑板功能，可以在上面写字并可以生成截图。

![image-20210817031900983](https://doc.houdunren.com/assets/img/image-20210817031900983.42a6208f.png)

以下是使用 typescript 编写，如果你没有 ts 环境，请删除代码中的类型声明。
