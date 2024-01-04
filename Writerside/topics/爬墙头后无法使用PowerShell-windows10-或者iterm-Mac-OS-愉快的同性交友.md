# 爬墙头后无法使用PowerShell(windows10)或者iterm(Mac OS)愉快的同性交友

最近遇到了个问题，就是明明翻过了墙头，无论用`PowerShell`还是`iterm`，通过命令行来愉快的同性交友。就是各种`443`或者`Timed out`。仔细的翻了下，找到了解决办法，分享给大家。



## Mac



非`root`用户下

```shell
vim ~/.bash_profile
```



在文件里面添加以下

```shell
 export https_proxy=http://127.0.0.1:7890
 export http_proxy=http://127.0.0.1:7890
 export all_proxy=socks5://127.0.0.1:7890
```



退出编辑后，`source`一下即可。



`root`用户下

```shell
vim ~/.zshrc
```



在文件里面添加以下

```shell
 export https_proxy=http://127.0.0.1:7890
 export http_proxy=http://127.0.0.1:7890
 export all_proxy=socks5://127.0.0.1:7890
```



退出编辑后，`source`一下即可。



PS:我的`Mac`在`root`下是没有`.zshrc`文件的，我是自己手动在`~/`目录下创建的。如果没有的老铁们可以参考一下我的做法。还有就是上面的`7890`端口，这个端口是我的梯子用的端口，并不是都是`7890`端口的。这一点尤为注意。





## Windows PowerShell



在`windows`下，场景又变的不一样了。

`windows`自带的终端我是没有使用的。

我是在`Microsoft Store`里面安装的`Power Shell`，这个怎么设置代理呢。

需要在`我的电脑`的`文档`里面找到一个文件夹，这个文件夹可能叫`PowerShell`也可能叫`WindowsPowerShell`。

在文件夹里面创建一个文件`Microsoft.PowerShell_profile.ps1`



在文件里面添加如下

```shell
$env:HTTPS_PROXY="http://127.0.0.1:52091"

$env:HTTP_PROXY="http://127.0.0.1:52091"

$env:all_proxy="socks5://127.0.0.1:52092"
```





不需要像`Mac`一样`source`，直接保存退出，重启`Power Shell`即可。



嗯，享受伟大的同性交友吧~~~