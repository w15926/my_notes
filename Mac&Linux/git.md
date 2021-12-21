# 配置文件

```shell
vi
open ~/.gitconfig

# proxy = http://127.0.0.1:4780
```



# github

## …or create a new repository on the command line

或者在命令行上创建一个新的存储库

```shell
echo "# uniapp_study" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/w15926/uniapp_study.git
git push -u origin main
```



## …or push an existing repository from the command line

或者从命令行推送现有存储库

```shell
git remote add origin https://github.com/w15926/uniapp_study.git
git branch -M main
git push -u origin main
```



# gitee

## 创建 git 仓库:

```shell
mkdir aa
cd aa
git init
touch README.md
git add README.md
git commit -m "first commit"
git remote add origin https://gitee.com/w15926/aa.git
git push -u origin master
```



## 已有仓库?

```shell
cd existing_git_repo
git remote add origin https://gitee.com/w15926/aa.git
git push -u origin master
```

