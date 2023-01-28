# 触控板三指及以上手势失灵

```shell
killall Dock
```



# 休眠的三种模式

> 默认为3，0为普通的老睡眠。
>
> >  0：系统不会将内存备份到永久存储。系统必须从内存（RAM）中唤醒。断电时系统将失去上下文。
>
> >  3：系统将存储一个内存副本到永久存储器（磁盘），并在休眠期间**为内存供电**。系统将从内存中唤醒，除非断电迫使它从磁盘映像恢复。
>
> >  25：系统将内存的副本存储到永久存储器（磁盘），并将**切断内存的电源**。系统将从磁盘映像恢复。

查看

```shell
pmset -g
```

设置

```shell
sudo pmset -a hibernatemode 25
```



# 非AppleStore的App安装问题

> 开启任何来源

```shell
sudo spctl  --master-disable
```



> 若还没没用则终端输入`sudo xattr -r -d com.apple.quarantine /Applications/WebStrom.app`后面加安装的App路径（可拖动图标）

```shell
sudo xattr -r -d com.apple.quarantine /Applications/xxx.app
```



# 安装软件时提示“文件已损坏，您应该将它移到废纸篓” 

> 软件名存在空格的话要改app名字去掉空格

```shell
sudo xattr -r -d com.apple.quarantine /Applications/IntelliJ IDEA.app
```

