# 环境变量

```shell
cd ~
vim ~/.bash_profile # 可选，vim进入编辑模式，open打开文件直接编辑
open ~/.bash_profile
source ~/.bash_profile # 应用修改后的配置
```

```shell
# 类似情况
# 在任意目录中打开根目录中的文件
open ~/.bash_profile
# 打开当前目录的文件
open .bash_profile

open .zshrc
```



**zsh**

```shell
 open ~/.zshrc
```



# 端口

- 查看当前端口进程

```shell
sudo lsof -i :80
sudo lsof -i :8080
sudo lsof -i :8081
sudo lsof -i :8082
sudo lsof -i :8083
```

- 关闭当前端口的所有进程

```shell
lsof -P | grep ':8080' | awk '{print $2}' | xargs kill -9
```

