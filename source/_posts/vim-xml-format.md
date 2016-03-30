---
title: Vim下XML文件格式化
categories: 学习
---

### 1.打开需要格式化的文件

使用vim命令打开需要格式化的xml文件，html也属于xml的一种，因此也可使用该方法格式化html文件。
现有一个如下的未格式化的文件：
![origin image](https://raw.githubusercontent.com/yangdage/images/master/20160330/20160328154539.png)

### 2.保存临时修改
如果我们打开后有对文件进行编辑操作，需要保存所做的修改

```bash
:w
```
在命令模式下输入上面的命令，保存改动，不退出文件

### 3.启用自动缩进选项

```bash
:filetype indent on
```

### 4.设置文件类型

```bash
:set filetype=xml
```

### 5.全局格式化

回到文件头：

``` bash
gg
```
使用缩进（格式化）
```bash
=
```

格式化到文件尾:

```bash
G
```

### 6.格式化结果
![origin image](https://raw.githubusercontent.com/yangdage/images/master/20160330/20160330115035.png)