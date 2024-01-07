[教程地址](https://zhuanlan.zhihu.com/p/474801204)

## 一、开启任何来源
> 因为macOS 启用了新的安全机制，所以在网上下载的应用安装好之后不能打开首先查看<font color="#ff0000">安全性设置</font>的<font color="#ff0000">任何来源</font>是否打开

![[Mac安全性打开任何来源位置.png|750]]

如果没有这个选项，我们打开终端，输入以下命令: `sudo spctl --master-disable`
<br/>

## 绕过公证
> 如果第一步执行完成之后还是无法打开软件，可能是苹果进一步收缩了对未签名应用的权限，这时候就需要过终端执行命令行代码来绕过应用签名认证

此时可以使用以下命令来绕过公证：`sudo xattr -rd com.apple.quarantine /Applications/xxxxxx.app`
- 其中<mark class="hltr-red">/Applications/xxxxxx.app</mark>是打不开的软件的路径，可以使用从访达中拖拽APP图标到终端的方式输入
<br/>

## 应用签名
> 如果第二步操作之后还是无法打开应用，可以尝试对应用签名

### 安装Command Line Tools 工具

打开终端输入以下命令：`xcode-select --install`, 在弹出窗口后选择继续安装

### 对应用签名
打开终端工具输入并执行如下命令对应用签名：`sudo codesign --force --deep --sign - (应用路径)`
- 如果遇到以下错误
	- `/文件位置 : replacing existing signature`
	- `/文件位置 : resource fork,Finder information,or similar detritus not allowed`

现在终端执行以下命令： `xattr -cr /文件位置`，在重新执行签名命令
<br/>

## SIP系统完整性保护

> 如果以上方法尝试后还打不开应用只能关闭苹果的SIP系统完整性保护了

<mark class="hltr-red">注意：关闭SIP完整性保护后会产生其他副作用</mark>