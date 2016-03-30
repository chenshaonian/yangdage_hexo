---
title: Sublime下的Package Control
categories: 学习
---

有时候我们安装的Sublime默认情况下没有安装Package Control(暂且叫它插件管理器)，没有安装它的情况下就不能很方便的安装官方的插件。

打开sublime，查看是否存在以下目录

![package control](https://raw.githubusercontent.com/yangdage/images/master/20160330/20160330174915.png)

如果没有，我们需要安装Package Control

通过快捷键 **ctrl+`** 或者 **View > Show Console** 菜单打开控制台，复制粘贴以下脚本，确保中间无换行。

Sublime3:

```bash
import  urllib.request,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();urllib.request.install_opener(urllib.request.build_opener(urllib.request.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib.request.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read())
```

Sublime2:

```bash
import  urllib2,os;pf='Package Control.sublime-package';ipp=sublime.installed_packages_path();os.makedirs(ipp)ifnotos.path.exists(ipp)elseNone;urllib2.install_opener(urllib2.build_opener(urllib2.ProxyHandler()));open(os.path.join(ipp,pf),'wb').write(urllib2.urlopen('http://sublime.wbond.net/'+pf.replace(' ','%20')).read());print('Please restart Sublime Text to finish installation')
```

重启sublime，通过快捷键**ctrl+shift+p**打开命令面板，输入package，查看Package Control是否安装正常

![package control](https://raw.githubusercontent.com/yangdage/images/master/20160330/20160330175617.png)

出现如上命令选项证明安装已正常，可以愉快的安装乱七八糟的插件了。