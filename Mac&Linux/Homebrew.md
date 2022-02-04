# Homebrew常见报错

- brew安装失败常见报错

```shell
Error: Another active Homebrew update process is already in progress.
Please wait for it to finish or terminate it to continue.
```

出现这样的bug原因是brew正在安装软件而被ctrl+z强制撤销造成的。

解决方法

```shell
rm -rf /usr/local/var/homebrew/locks
```



- brew安装时候出现curl: (22) The requested URL returned error: 404

更新brew就好了

```shell
brew update
```

