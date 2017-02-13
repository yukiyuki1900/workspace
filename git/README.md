## git相关

> 百度最近在力推git，连svn模块的申请都停止了，只能申请git的模块。最近负责组内git的推广，看了蒋鑫的《git权威指南》和codecademy git，这里先梳理一遍吧

> git和svn底层最本质区别就是，一个按照文件存储，一个是快照（各种patch）存储的。
> git和svn不一样，svn所有的数据库都是在远程服务器上的，每个文件下都有.svn文件，git不是，git本地就是一个数据仓库，你可以离线使用，当你git commit的时候实际上提交到是本地的仓库，只有git push才会到远程服务器。本地只有根目录下有一个.git文件夹,这个文件夹中保存着所有的分支信息，文件索引和快照

如果在github上新建一个repository，创建完之后github上会有提示：
```
	echo "# git-something" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin https://github.com/yukiyuki1900/git-something.git
	git push -u origin master
```

我们来看下每个步骤分别都代表了啥

* 第一步：

	```
		$echo "# git-something" >> README.md
	```
	创建一个README.md文件。

* 第二步：

	```
		$git init
	```
	初始化一个空的git代码仓库。这时候在根目录会生成一个.git文件。
	这时如果执行
	```
		$ls -aF
		./ ../ .git/
	```
	这个.git就是Git的版本仓库哟~

	现在我们已经有了一个git的项目，一个git项目分为3个部分：**working copy（工作区）**、**staging area（暂存区）**、**datastore（版本库）**
	借用codecademy git的一张图方便理解：
	
	![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-parts.png)

	* 工作区为本地的开发环境，可以对文件进行增删查改
	* 暂存区管理工作区修改的文件，通过git add将工作区修改的数据添加到暂存区
	* 版本库负责保存本次文件的修改，通过git commit可将修改的数据添加到版本库

	如果此时执行```git status```命令，可看到git提示“可将修改添加到暂存区”

	![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-status.png)

	红色字为本次的修改的文件（新增的README.md文件）

* 第三步：
	
	```
		git add README.md
	```
	将README.md添加到暂存区中

	此时如果执行```git status```命令，可看到文件已添加到暂存区，并提示可以```git rm --cached```来删掉暂存区中已提交的文件

	![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-status2.png)

	绿色字为已提交到暂存区的文件
	
	如果此时修改了一下README.md的文件，执行```git diff```命令：

	![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-diff.png)

	可查看到工作区与暂存区数据的diff
	再次执行```git add```命令将此时的修改提交到暂存区中

* 第四步：

	```
		git commit -m "first commit"
	```
	将暂存区的修改提交到版本库中。
	执行```git log```可查看提交日志。

	在操作中，可使用git diff查看修改内容
	```
		git diff 不加参数，比较的是工作区和暂存区的数据。
		git diff –-cached 比较的是暂存区和版本库中的数据
		git diff HEAD/master 比较的是版本库和工作区的数据
	```

###分支操作
熟悉svn操作的同学都对trunk，branch很熟悉，在git里是没有trunk和branch区别的，git里可以拥有多个代码仓库，地址为 ```orign/*```。

创建新分支：
```
	git branch 新分支名
	比如：

	git branch lalal
```

查看当前所在分支：
```
	git branch
	或者
	git branch -a
```
![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-branch.png)

本地有两个仓库：lalal 和 master，但远端只有orign/master一个仓库（因为lalal并没有提交到远端）。目前本地是在master分支下。

checkout lalal分支

```
	git checkout lalal
	git branch -a
```
![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-branch2.png)

可以看到本地以及切换到lalal分支下了

提交分支到远端：
```
	git push origin 分支名

	比如：
	git push origin lalal
```

合并分支代码（当前所在分支合入另一个分支代码）：
```
	git merge lalal
```
![image](https://github.com/yukiyuki1900/workspace/blob/master/git/git-branch3.png)

因为lalal分支没有提交，所以合入没有什么输出。


或者我们可以约定俗成，将master分支作为trunk使用，最后上线都会将其他分支merge到master里，保持master分支最新，以后拉分支由orign/master为蓝本拉取最新代码
orign/master  -> trunk
orign/*       -> branck

这样git就能实现多分支并行开发（svn得co出多少个workspace呢？）

### 与svn操作对比

* 基本操作5条命令

<table>
	<tr>
		<td><strong>场景</strong></td>
		<td><strong>svn</strong></td>
		<td><strong>git</strong></td>
	</tr>
	<tr>
		<td>下载代码</td>
		<td>svn checkout</td>
		<td>git clone</td>
	</tr>
	<tr>
		<td>加入文件</td>
		<td>svn add</td>
		<td>git add</td>
	</tr>
	<tr>
		<td>本地提交</td>
		<td>--</td>
		<td>git commit</td>
	</tr>
	<tr>
		<td>提交到服务器</td>
		<td>svn submit</td>
		<td>git push</td>
	</tr>
	<tr>
		<td>获取其他人的更新</td>
		<td>svn update</td>
		<td>git fetch/pull</td>
	</tr>

</table>

* 本地提交、暂存、撤销修改

<table>
	<tr>
		<td><strong>场景</strong></td>
		<td><strong>svn</strong></td>
		<td><strong>git</strong></td>
	</tr>
	<tr>
		<td>开发有了些小成果，赶紧提交一下</td>
		<td>svn木有办法，只能提交到线上服务器</td>
		<td>git commit</td>
	</tr>
	<tr>
		<td>急事插入，保存工作现场一会再恢复</td>
		<td>svn得重新checkout一个workspace</td>
		<td>git stash</td>
	</tr>
	<tr>
		<td>跑了个编译，产生很多临时文件，污染开发目录</td>
		<td>svn status一个个删除</td>
		<td>git clean -fd</td>
	</tr>
	<tr>
		<td>改坏了代码，从保存的版本恢复</td>
		<td>svn revert（得联网从服务器拉取代码，需要等待）</td>
		<td>git checkout filename（文件快照就保存在本地，秒开）</td>
	</tr>
	<tr>
		<td>误提交的修复（包括commit log和message）</td>
		<td>svn木有办法撒</td>
		<td>git reset恢复到某个版本或者用git commit --amend来替换掉上次的提交</td>
	</tr>
</table>

### 基本git命令

<table>
	<tr>
		<td><strong>任务</strong></td>
		<td><strong>说明</strong></td>
		<td><strong>git命令</strong></td>
	</tr>
	<tr>
		<td>告诉Git你是谁</td>
		<td>设置账号和邮箱，作为每次commit的提交人</td>
		<td>
			git config --global user.name “” <br/>
			git config --global user.email “”
		</td>
	</tr>
	<tr>
		<td>获得一个代码库</td>
		<td>新建</td>
		<td>git init</td>
	</tr>
	<tr>
		<td>获得一个代码库</td>
		<td>从远端服务器下载</td>
		<td>git clone git@host/path/to/repo</td>
	</tr>
	<tr>
		<td>加入文件</td>
		<td>加入到暂存区</td>
		<td>
			git add <strong>filename</strong>/<strong>foldername</strong> <br/>
			git add *
		</td>
	</tr>
	<tr>
		<td>提交</td>
		<td>提交到本地代码库的当前分支的head（还没有到远端服务器）</td>
		<td>git commit -m”Commit message”</td>
	</tr>
	<tr>
		<td>查看更改状态</td>
		<td>查看修改的和待提交的文件</td>
		<td>git status</td>
	</tr>
	<tr>
		<td>分支管理</td>
		<td>新建并切换到某个分支</td>
		<td>git checkout -b <strong>branchname</strong></td>
	</tr>
	<tr>
		<td>分支管理</td>
		<td>切换到已有分支</td>
		<td>git checkout <strong>branchname</strong></td>
	</tr>
	<tr>
		<td>分支管理</td>
		<td>列出所有分支</td>
		<td>git branch</td>
	</tr>
	<tr>
		<td>分支管理</td>
		<td>删除分支</td>
		<td>git checkout -d <strong>branchname</strong></td>
	</tr>
	<tr>
		<td>分支管理</td>
		<td>将分支推送到远端</td>
		<td>git push origin <strong>branchname</strong></td>
	</tr>
	<tr>
		<td>分支管理</td>
		<td>删除远端代码库的分支</td>
		<td>git push origin :<strong>branchname</strong></td>
	</tr>
	<tr>
		<td>从远端获取更新</td>
		<td>获取更新并merge到当前分支</td>
		<td>git pull origin <strong>branchname</strong></td>
	</tr>
	<tr>
		<td>从远端获取更新</td>
		<td>把其他分支merge到当前分支</td>
		<td>git merge origin <strong>branchname</strong></td>
	</tr>
	<tr>
		<td>从远端获取更新</td>
		<td>仅获取更新，不更新代码（仅更新origin/*分支）</td>
		<td>git fetch</td>
	</tr>
	<tr>
		<td>撤销修改</td>
		<td>如果改坏代码，从head版本恢复（覆盖当前文件）</td>
		<td>git checkout -- <strong>filename</strong></td>
	</tr>
	<tr>
		<td>撤销修改</td>
		<td>如果改坏的代码已经提交到本地代码库，从远端的head恢复（本地提交被丢弃）</td>
		<td>git fetch && git reset --hard origin/master</td>
	</tr>
</table>

### 一些方便的配置

为了精简一些git的操作，可以做些配置：
```
	$git config --global color.ui true   #git命令输出开启颜色
	$git remote -v   #查看分支地址
	$git diff      #查看修改内容，同svn diff
	$git status    #查看修改文件，同svn st

	如果想要输出更精简些，可以增加参数-s
	$git status -s   #查看修改文件，同svn st

	要是想和svn一样有简洁的操作命令，也可以设置别名
	$git config  --global alias.st status
	$git config  --global alias.ci commit
	$git config  --global alias.co checkout 
```

