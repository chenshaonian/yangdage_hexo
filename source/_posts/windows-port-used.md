---
title: Windows下端口占用程序查询
---

### 1.打开命令行工具
输入以下命令：

```bash
netstat -aon|findstr 4000
```

可以看到类似以下的输出，最后一列为占用端口的程序的进程id

```bash
TCP    0.0.0.0:8082           0.0.0.0:0              LISTENING       2480
TCP    [::]:8082              [::]:0                 LISTENING       2480
```

### 2.根据进程ID查询程序信息
输入以下命令：

```bash
tasklist|findstr 2480
```

控制台会输出该进程的程序信息

```bash
java.exe                      2480 Console                    3    253,168 K
```