## CSS3 box-sizing属性

说到CSS3的box-sizing属性，必须得先解释下盒子模型：

CSS盒子模型规定，每个元素包含内容区域(content)、内边距(padding)、边框(border)、外边框(margin)4个部分。

![image]()

对于标准的w3c盒子模型来说，元素的宽高指的是content部分的宽高。

而因为历史的原因，在一些旧版的ie对盒子模型里元素的宽高包含了content、padding和border。

而通过定义box-sizing的属性，则可以指定元素的宽高是否包含padding和border区域。

### 属性

<table>
    <tr>
        <th>属性</th>
        <th>值</th>
        <th>描述</th>
    </tr>
    <tr>
        <td>box-sizing</td>
        <td>content-box</td>
        <td>元素的宽高只包含content</td>
    </tr>
    <tr>
        <td></td>
        <td>border-box</td>
        <td>元素的宽高包含content、padding、border</td>
    </tr>
    <tr>
        <td></td>
        <td>inherit</td>
        <td>继承父元素的属性值</td>
    </tr>
</table>


### 参考材料

* [CSS3 box-sizing 属性](http://www.w3school.com.cn/cssref/pr_box-sizing.asp)