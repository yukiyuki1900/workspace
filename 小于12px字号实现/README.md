## 小于12px字号实现

我们都知道在浏览器里展现的字体最小字号是12px，即便在css里设置字号小于12px，展示的时候也会是12px。

那如果UE设计的时候就是需要展示小于12px的文字呢？毕竟不是所有情况下都能用图片解决的。

这个时候呢可以考虑下使用**transform**，通过缩放的方式实现小于12px的字号


```
    <div class="fontSize9">font-size: 9px</div>

    .fontSize9 {
        font-size: 12px;
        transform: scale(0.75);
        -webkit-transform: scale(0.75);
        transform-origin: 50%;
        -webkit-transform-origin: 50%;
    }
```

这样就实现了小于12px的文字。具体可看[test.html](https://github.com/yukiyuki1900/workspace/blob/master/%E5%B0%8F%E4%BA%8E12px%E5%AD%97%E5%8F%B7%E5%AE%9E%E7%8E%B0/test.html)

效果图

![image](https://github.com/yukiyuki1900/workspace/blob/master/%E5%B0%8F%E4%BA%8E12px%E5%AD%97%E5%8F%B7%E5%AE%9E%E7%8E%B0/fontSize1.png)

但是细心的观众朋友会发现，文字怎么缩进去啦，虽然文字变小了但是样式歪了呀。

那是因为缩小的是整个div元素，如果给元素加个外边框就可以看到:

![image](https://github.com/yukiyuki1900/workspace/blob/master/%E5%B0%8F%E4%BA%8E12px%E5%AD%97%E5%8F%B7%E5%AE%9E%E7%8E%B0/fontSize2.png)

解决方法也很简单，不要缩小整个div嘛，给文字包一层span标签，缩小这个标签就好了

```
    <div>
        <span class="fontSize9">font-size: 9px</span>
    </div>

```

效果如下：

![image](https://github.com/yukiyuki1900/workspace/blob/master/%E5%B0%8F%E4%BA%8E12px%E5%AD%97%E5%8F%B7%E5%AE%9E%E7%8E%B0/fontSize3.png)

看起来还是会缩进一些呢，这个时候再对页面样式做一些微调就好了。比如缩放75%，那就是左右两边分别会留出宽度为12.5%的空白，那么设置下margin-left: -12.5%就好了。注意这个12.5%是以父节点为基准的，所以还得在外面再包一层宽度和文字宽度一样的标签

```
    <div>
        <div class="inline">
            <span class="fontSize9 marginLeft3">font-size: 9px</span>
        </div>
    </div>
    
    .inline {
        display: inline-block;
    }
    
    .marginLeft3 {
        margin-left: -12.5%;
    }

```

![image](https://github.com/yukiyuki1900/workspace/blob/master/%E5%B0%8F%E4%BA%8E12px%E5%AD%97%E5%8F%B7%E5%AE%9E%E7%8E%B0/fontSize4.png)

完美！

具体可看效果[test.html](https://github.com/yukiyuki1900/workspace/blob/master/%E5%B0%8F%E4%BA%8E12px%E5%AD%97%E5%8F%B7%E5%AE%9E%E7%8E%B0/test.html)
