## npm版本升级

最近做项目发现我的mac本地的npm版本是2.4，现在npm4都出来了，所以想升级下npm。

随意搜了下npm升级的方法，马上看到一篇文章说执行

```
	$npm install -g npm
```

即可升级到最新版本， so easy。

结果这个命令执行完后，再查看npm版本，发现

![image]()

坑爹。

google一下，发现遇到同样问题的大有人在，原因是执行npm更新命令的时候得在``~/``目录下，不能在其他随意的目录下。

[http://stackoverflow.com/questions/35018661/bash-usr-local-bin-npm-no-such-file-or-directory](http://stackoverflow.com/questions/35018661/bash-usr-local-bin-npm-no-such-file-or-directory)

[https://github.com/npm/npm/issues/4099](https://github.com/npm/npm/issues/4099)

而此时，如果使用``brew install node``重装node，也不会重装npm，只好手动安装npm了。


### 方法一：
在``~/``目录下，执行

```
	curl http://npmjs.org/install.sh | sh
```

这个命令我在本地执行一直报错，无解。

### 方法二：
因为方法一的命令就是将npmjs的脚步down到本地再执行sh，所以我就在这拆成了两个步骤。先将``install.sh`` down下来（看本目录，亲测可用），然后再执行

```
	sh install.sh
```

如果遇到权限问题，则加个sudo

```
	sudo sh install.sh
```

再执行``npm -v``

![image]()

好耶