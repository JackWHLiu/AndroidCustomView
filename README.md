# Android绘图6要素






​		所有要参与Android自定义控件开发的开发者，绘图6要素都是他入门的必经之路。这6要素是指Canvas（画板）、Paint（画笔）、Bitmap（位图）、Drawable（可绘制的）、Rect（矩形）和Path（路径）。为什么说这些是最基本的呢？

![img](https://img-blog.csdnimg.cn/20200729102322356.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)肯定是有依据的。系统源代码注释说是4个基本组件，我觉得值得我们注意的基础绘图类起码有6个，下面我分别来详细讲解下这些类。



**Canvas篇**

​		我们从Canvas讲起，Canvas翻译成”画板、画布“，绘制自定义控件，和我们画画类似。巧妇难为无米之炊，没有纸你没法画画吧！无论你是A4纸还是宣纸，首先，你都得有这个画画的前提条件。我们有了Canvas，就可以开始绘制我们的界面了。

```java
//画颜色
void drawColor(int color);
```

给画板设置背景颜色。

```java
//画一个点
void drawPoint(float x, float y, Paint paint);
```

通过点的坐标P(x,y)绘制一个点，x为点的横坐标，y为点的纵坐标。

```java
//画很多个点
void drawPoints(float[] pts, Paint paint);
```

pts这个参数是所有点的坐标，长度一定要是2的倍数，且数组每2个值表示一个点。例如：你传入4个浮点型数据，x0,y0,x1,y1。x0,y0会绘制成一个点，x1,y1会绘制成一个点。

```java
//画一条线
void drawLine(float startX, float startY, float stopX, float stopY, Paint paint);
```

我们在数学中学过，两点确定一条直线，所以我们需要起始点（x0,y0）和结束点(x1,y1)两个点。startX和startY就是我们的x0和y0，而stopX和stopY就是我们的x1和y1。

```java
//画很多条线
void drawLines(float[] pts, Paint paint);
```

pts这个参数是所有点的坐标，长度一定要是4的倍数，且数组每4个值表示一条线。例如：你传入8个浮点型数据，x0,y0,x1,y1,x2,y2,x3,y3。x0,y0,x1,y1会绘制成一条直线，x2,y2,x3,y3会绘制成一条直线。

```java
//画矩形
void drawRect(float left, float top, float right, float bottom, Paint paint);
```

left、top、right和bottom分别为矩形最左、最上、最右和最下边的位置，即左边垂直线段的x坐标、上边水平线段的y坐标、右边垂直线段的x坐标、下边水平线段的y坐标。

```java
//画圆角矩形
void drawRoundRect(float left, float top, float right, float bottom, float rx, float ry, Paint paint);
```

left、top、right和bottom分别为矩形最左、最上、最右和最下边的位置，即左边垂直线段的x坐标、上边水平线段的y坐标、右边垂直线段的x坐标、下边水平线段的y坐标。rx为生成圆角的椭圆的x轴半径，ry为生成圆角的椭圆的y轴半径。

```java
//画圆形
void drawCircle(float cx, float cy, float radius, Paint paint);
```

画一个圆需要知道圆的圆心（Oa）和半径（Ra）。cx表示圆心的x坐标，cy表示圆心的y坐标，radius表示半径的长度。

```java
//画椭圆
void drawOval(float left, float top, float right, float bottom, Paint paint);
```

画椭圆，你可以想象成从矩形中内切出一个椭圆。left、top、right和bottom分别为矩形最左、最上、最右和最下边的位置，即左边垂直线段的x坐标、上边水平线段的y坐标、右边垂直线段的x坐标、下边水平线段的y坐标。

```java
//画扇形
void drawArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle, boolean useCenter, Paint paint);
```

画扇形，你可以想象成画矩形内切椭圆的一部分。left、top、right和bottom分别为矩形最左、最上、最右和最下边的位置，即左边垂直线段的x坐标、上边水平线段的y坐标、右边垂直线段的x坐标、下边水平线段的y坐标。startAngle表示开始的角度，0度是沿着直角坐标系中x轴的正方向，即从圆心向正右方向画一条线，这里的圆心等于矩形的中心。sweepAngle表示将这条线顺时针旋转的角度，旋转后的线与原始线的夹角。useCenter为true的话，则前面所说，旋转前后的两条线，所扫过圆弧的两端点，分别与圆心相连，所组成的扇形。如果useCenter为false，则直接连接圆弧的两端点，组成的小扇形。

```java
//画位图
void drawBitmap(Bitmap bitmap, float left, float top, Paint paint);
```

left和top分别为起始点的x和y坐标。

```java
//画位图
void drawBitmap(Bitmap bitmap, Rect src, Rect dst, Paint paint);
```

如果src为null，则绘制整张位图。如果src不为null，截取bitmap的一部分进行绘制，截取坐标为src指定的，dst为绘制在画板的位置。

```java
//画文字
void drawText(String text, float x, float y, Paint paint);
```

text为要绘制的文字的内容，x和y为要绘制的文字的横坐标和纵坐标。绘制文字要将位置绘制准确，需要使用到文字的基线baseline。

​		画布还可以切割，用户只能看到切割后的内容。

```java
//通过矩形切割画板
canvas.clipRect();
//通过路径切割画板
canvas.clipPath();
```

Region.Op参数有以下几个枚举可用。

```java
Region.Op.DIFFERENCE	//第一次不同于第二次的部分显示出来
Region.Op.REPLACE	//显示第二次的
Region.Op.REVERSE_DIFFERENCE	//第二次不同于第一次的部分显示出来
Region.Op.INTERSECT	//交集显示
Region.Op.UNION	//并集显示
Region.Op.XOR	//补集显示，就是全集的减去交集部分
```



**Paint篇**

​		光有画布，没有画笔，也是什么也画不了的，所以接下来，我们要买支好画笔。

```java
Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);	//抗锯齿，更平滑的画笔，应该是支铅笔
```

如果你不设置ANTI_ALIAS_FLAG，也是可以的。

```java
Paint paint = new Paint();	//默认的画笔，性能好，但精确度不高，可能是支蜡笔吧
```

Paint的基本实用方法和技巧

图形绘制相关

- 重置

  ```java
  paint.reset();
  ```

  

- 设置画笔颜色

  ```java
  paint.setColor(Color.RED);
  ```

  

- 设置透明度

  ```java
  paint.setAlpha(255);
  ```

  

- 设置画笔的样式

  ```java
  paint.setStyle(Paint.Style.FILL);//仅填充内容
  paint.setStyle(Paint.Style.FILL_AND_STROKE);//填充内容并描边
  paint.setStyle(Paint.Style.STROKE);//仅描边
  ```

  

- 设置画笔的宽度

  ```java
  paint.setStrokeWidth(50);	//单位，px
  ```

  

- 设置线帽，即画笔底部的形状

  ```java
  paint.setStrokeCap(Paint.Cap.BUTT);//没有
  paint.setStrokeCap(Paint.Cap.ROUND);//圆形
  paint.setStrokeCap(Paint.Cap.SQUARE);//方形
  ```

  

- 设置线条相交的地方怎么补齐

  ```java
  paint.setStrokeJoin(Paint.Join.MITER);//锐角
  paint.setStrokeJoin(Paint.Join.ROUND);//圆弧
  paint.setStrokeJoin(Paint.Join.BEVEL);//直线
  ```

  

- 设置抗锯齿，会损失一定的性能

  ```java
  paint.setAntiAlias(true);
  ```

  

文字绘制相关

- 获取字符行间距

  ```java
  paint.getFontSpacing();
  ```

  

- 获取字符之间的间距

  ```java
  paint.getLetterSpacing();
  ```

  

- 设置字符之间的间距

  ```java
  paint.setLetterSpacing(letterSpacing);
  ```

  

- 设置文本删除线

  ```java
  paint.setStrikeThruText(true);
  ```

  

- 设置文本下划线

  ```java
  paint.setUnderlineText(true);
  ```

  

- 设置文本大小

  ```java
  paint.setTextSize(textSize); //单位，px
  ```

  

- 获取文本大小

  ```java
  paint.getTextSize();
  ```

  

- 设置字体类型

  ```java
  paint.setTypeface(Typeface.BOLD); //设置字体类型
  ```

  Typeface.BOLD 加粗

  Typeface.ITALIC 倾斜

  Typeface.create(familyName, style);//加载自定义字体

- 设置文字倾斜

  ```java
  paint.setTextSkewX(-0.25f);	//默认0，官方推荐的-0.25f是斜体
  ```

  

- 设置文本的对齐方式

  ```java
  paint.setTextAlign(Align.LEFT);
  paint.setTextAlign(Align.CENTER);
  paint.setTextAlign(Align.RIGHT);
  ```

  

- 计算指定长度的字符串

  ```java
  int breakText(String text, boolean measureForwards, float maxWidth, float[] measuredWidth);
  ```

  示例代码：

  ```java
  paint.setTextSize(50);
  float[] measuredWidth = new float[1];
  int breakText = paint.breakText(str, true, 200, measuredWidth);
  Log.i("Jack", "breakText="+breakText+", str.length()="+str.length()+", measredWidth:"+measuredWidth[0]);
  ```

- 计算字符串所占的宽高

  ```java
  void getTextBounds(String text, int start, int end, Rect bounds);
  ```

  text为要计算宽高的字符串，start表示从第几个位置开始截取，end表示截取到第几个位置之前。bounds你要事先创建好一个空的Rect，此方法一调用，Rect就有值了，然后可以通过rect.width()和rect.height()获取宽高。

- 仅计算字符串的宽度

  ```java
  float measureText(String text);
  ```

- 精确计算字符串中单个文字的宽度

  ```java
  int getTextWidths(String text, int start, int end, float[] widths);
  ```

  获取字符串中每个字符的宽度，并把结果填入参数 widths。事先创建好widths数组，此方法一调用，widths就会被赋值。

- 文字的基线

  ```java
  FontMetrics fontMetrics = paint.getFontMetrics();
  ```

  公式1：指定左上角的顶点坐标，绘制文本
  $$
  基线Y = 顶点Y - fontMetrics.top;
  $$

  ```java
  float baselineY = topY - fontMetrics.top;
  ```

  公式2：指定中间位置，绘制文本
  $$
  基线Y = 中间Y + (fontMetrics.bottom-fontMetrics.top)/2-fontMetrics.bottom
  $$

  ```java
  float baselineY = centerY + (fontMetrics.bottom-fontMetrics.top)/2-fontMetrics.bottom
  ```

  通过了公式计算后，使用这个baselineY的值传入参数y。



**Bitmap篇**

​		Bitmap是内存中的图像，简单说，就是保存了一个个图像颜色信息的矩阵。让我们看看使用Bitmap都要注意些什么吧！

- BitmapConfig

|           | total bytes | alpha channel |
| --------- | ----------- | ------------- |
| ALPHA_8   | 1 byte      | 8bit          |
| RGB_565   | 2 bytes     | /             |
| ARGB_8888 | 4 bytes     | 8bit          |

- 创建Bitmap

  ```java
  //创建一个指定宽高的空的位图
  Bitmap createBitmap(int width, int height, Config config);
  ```

  这就是最简单的创建位图的方式。默认像素点的色值全部为0，即黑色。

- 保存到文件

  ```java
  boolean compress(CompressFormat format, int quality, OutputStream stream)
  ```

  这个方法需要在子线程调用。CompressFormat有3个枚举值，CompressFormat.JPEG、CompressFormat.PNG和CompressFormat.WEBP。quality的取值范围是0~100，其中100的质量最高。

- 获取像素点的色值

  ```java
  int getPixel(int x, int y);
  ```

  

- 修改像素点的色值

  ```java
  void setPixel(int x, int y, int color);
  ```

  

- 获取位图的宽度

  ```java
  int getWidth();
  ```

  

- 获取位图的高度

  ```java
  int getHeight();
  ```

  

- 使用JNI处理像素点的色值

  ```java
  Bitmap createBitmap(int[] colors, int offset, int stride, int width, int height, Config config);
  ```

  这种方式用于通过JNI处理像素后，回传色值数组创建Bitmap。因为磁盘IO是非常耗时的，假设一个图像有1000*1000=100万像素，那么你通过Java的for循环调用setPixel()方法，就要调用100万次输入输出流，在性能上和C/C++相比有9倍左右的差距。因为我们使用native代码是把所有像素点一次性处理好，然后一起打包返回给Java层的，效率自然而然就更高了。colors为所有像素点的色值。offset表示从第几个开始拿，索引是从0开始的。stride为步长，即一行应该显示多少个像素点，它是小于等于width的，否则会报错。比如绘制灰色浮雕效果时，是把每一个像素的R、G和B，用下一个像素点的R、G、B减上一个，然后再加上127，让R、G、B都往中间色值靠拢。至于为什么要加127，学过计算机图形学或计算机图像处理学的应该知道中性灰的概念。而如第二行第一个像素点和第一行最后一个像素点的RGB差异不能作为浮雕效果轮廓，所以要去掉边缘像素，这个stride就是用于舍弃边缘像素的。
