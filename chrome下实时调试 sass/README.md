## chrome下实时调试 sass

### 背景

sass 3.3 以上增加了使用sourcemaps进行实时开发调试sass源码的功能。

sourcemaps 为sass源代码和编译后css代码构建了一个映射关系，在chrome中查看元素样式可以直接定位到相应的sass代码行。

### 环境

* chrome 28+
* sass 3.3 Alpha 以上
* ruby

### 效果

![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/edit-sass.gif)

### 调试步骤

1.  确认sass版本至少为sass 3.3

    ```
        $ sass -v    //查看sass版本
        $ gem install sass --pre   //安装pre-release版
        或者
        $ gem install sass --version=3.3.0.alpha.184  //安装
    ```

2.  开启chrome Devtools

    打开 chrome://flags，找到 devtools 并启动

    ![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/devtools.jpg)

3.  使用 sass 监控输出样式文件

    浏览器已经可以定位到scss源代码了，接下来需要做的是在浏览器和本地源代码建立map关系，使得在浏览器中修改scss代码的时候可以在浏览器中实时查看效果。

    打开控制台，进入项目文件夹。输入命令：

    ```
        $ sass --watch -scss --sourcemap test.scss:test.css
    ```

    当scss文件发生改变，css文件会发生相应的改变，浏览器监控到css改变自动reload。

4.  设置 chrome source maps

    进入开发者工具，在 general tab 中 勾选 enable css source maps 和auto-reloadg enerated css，当css发生变化时页面会自动reload

    ![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/source-maps.JPG)

    此时打开引用到生成的css文件的页面，查看元素的样式，会发现文件路径已为scss源文件。

    ![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/elements.JPG)

    点击进去可查看scss源代码，如下图。

    ![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/hover.JPG)

5.  在workspace 中导入本地代码

    进入开发者工具，在workspace tab中导入本地代码

    ![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/addfolder.JPG)

    回到source，会看到代码已经导入。 找到scss文件，右键点击 Map to network resource

    ![image](https://github.com/yukiyuki1900/workspace/blob/master/chrome%E4%B8%8B%E5%AE%9E%E6%97%B6%E8%B0%83%E8%AF%95%20sass/maptonet.jpg)

    此时浏览器会有个弹窗，点击确定。

    source里编辑scss，然后ctr+s 保存，会发现左边浏览器样式已经发送改变啦~~

    是不是很好玩 O(∩_∩)O~~