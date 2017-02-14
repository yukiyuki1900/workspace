## 移动端1px分割线完美实现

页面上如果要实现1px分割线的话，传统的写法是：

```
    .border {
        border: 1px solid;
    }
```

可是在移动端开发的时代，这样写在手机上展示实际上是2px，这是因为在移动端的页面用了viewpoint的meta标签，所有定义的px变成了设备像素而不是网页像素。所有一般如果UE在页面验收比较严格，非要实现1px的像素分割线的时候，在项目中我用过两种方法，都非常的简单：

### 方法一：border-image
这是我最先最先使用的方法

```
    .cc li{
        -webkit-border-image: url("./img/cc.png") 2 2 stretch;
        border-image: url("./img/cc.png") 2 2 stretch;
        border: 1px solid;
    }
```

```cc.png```为一张6*6像素的图片。

#### 实现原理

以下是6*6像素的图片，青色的线将其分割成9个部分。

![image](https://github.com/yukiyuki1900/workspace/blob/master/%E7%A7%BB%E5%8A%A8%E7%AB%AF1px%E5%88%86%E5%89%B2%E7%BA%BF%E5%AE%8C%E7%BE%8E%E5%AE%9E%E7%8E%B0/border-img.JPG)

8个部分分别对应着以下8个位置：

![image](https://github.com/yukiyuki1900/workspace/blob/master/%E7%A7%BB%E5%8A%A8%E7%AB%AF1px%E5%88%86%E5%89%B2%E7%BA%BF%E5%AE%8C%E7%BE%8E%E5%AE%9E%E7%8E%B0/border-img2.png)

如第一张图，上边为1px像素的图片+1px像素的透明，2px的图片会被压缩到1px的边框容器内，相当于使用了background-size：50% 50%的效果。 这样做的好处在于可以利用正常设置边框来完成实际显示1px的边框。
其中，图片填充边框有几种状态 图像边框是否应平铺(repeated)、铺满(rounded)或拉伸(stretched)。默认为stretched填充，正是因为用了这种方式，才会让2px的图片(1px线+1px透明)压缩显示到1px的边框中。所以相当于使用background-size：50% 50%的效果。由于图标边框的四个角(就是上图中的左上角，右上角)所以这种实现方式不能够实现圆角。
具体可见详细讲解文档 [css3 border-image 详解](http://www.zhangxinxu.com/wordpress/2010/01/css3-border-image/)

这个方法实现需要依赖图片，不够灵活


### 方法二：使用::after或::before元素实现border
这是我现在比较喜欢使用的也是至今比较流行的方法。对比起使用border-image的方法，使用::after和::before实现border比较灵活，不用依赖图片，可以直接在css定义border的颜色

通过::before和::after设置分割线，然后通过transform将线收缩为1px


具体可看demo[border.html](https://github.com/yukiyuki1900/workspace/blob/master/%E7%A7%BB%E5%8A%A8%E7%AB%AF1px%E5%88%86%E5%89%B2%E7%BA%BF%E5%AE%8C%E7%BE%8E%E5%AE%9E%E7%8E%B0/border/border.html)

