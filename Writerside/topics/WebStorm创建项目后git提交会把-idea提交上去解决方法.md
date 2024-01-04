# WebStorm创建项目后git提交会把.idea提交上去解决方法

最近开始整点儿前端的活。

于是，用了`WebStorm`写了个小demo开始整，代码会怼到`GitHub`上去。
但是，发现了个问题，新建项目后，把他分享到`GitHub`上面去，一切都操作完后，他没有其他`IDE`的那种`init commit`，会弹框，让你选择`commit`,而是自顾自的`commit`，并且`commit`的文案，还不用你去想，也不是什么默认的`init commit`。完全自行发挥。

于是，对于我这个强迫症患者来说，就想搞一把他，给他上一课。琢磨了一下，发现这个应该是属于`WebStorm`的`bug`，那我只能从`git`上下手了。

`git`有`工作区`、`暂存区`、`版本库`的概念，就目前这情况来说，我本地仓库是有提交记录，我需要把我的提交记录`push`给远程仓库。但是，他已经`add`和`commit`了。

那么，问题来了，`add`和`commit`操作后，文件去了哪里了呢？

是的，文件去往了`暂存区`，我们只需要把`暂存区`的缓存删除掉，而不是删除`工作区`的文件，是不是就可以了呢。

遇事不决就去`--help`

`git`有个`rm`命令，他的`rm`命令后面可以跟参数，找了`help`的`help`后，发现有指令可以用。

```shell
git rm -r --cached .idea
```

这是一个完整的命令，我们来分析一波。

首先，`rm`命令，是删除指令，他可以从`工作区`与`暂存区`删除文件，他的删除虽然有用，但是，会从`工作区`删除，肯定不是我们想要的。

继续往下，你会发现他还有一些其他指令

# -r （只显示一个-就可以）
`-r`，这个`-r`在`git`里面是这么描述的
```shell
-r
Allow recursive removal when a leading directory name is given.
当给出了主目录名时，允许递归删除。
```
昂，加这个参数的目的是为了防止`.idea`或者指令后面其他的目录有多层嵌套。


# -cached （只让显示一个-，不是我少-）
`--cached`，这个命令是让对应文件夹或者文件，从`git`的跟踪清单里面删除。他在`git`里面是这么描述的。

```shell
--cached
Use this option to unstage and remove paths only from the index. Working tree files, whether modified or not, will be left alone.
使用此选项可取消分级并仅从索引中删除路径。工作树文件，无论是否被修改，都将被单独保留。
```

我这翻译都是机翻啊，见谅。这个大致意思就是从索引中删除，那么索引是什么呢？答案是`暂存区`。之所以叫`暂存区`实际上是为了更加贴合中文的语境。

那么，这就明白了，从`暂存区`删除后，我们可以通过`.gitignore`过滤掉`.idea`文件夹，让他不要被提交就可以了。