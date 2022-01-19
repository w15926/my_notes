# 配置文件

```shell
vi
open ~/.gitconfig

# proxy = http://127.0.0.1:4780

[core]
	excludesfile = /Users/lalala/.gitignore_global
[difftool "sourcetree"]
	cmd = opendiff \"$LOCAL\" \"$REMOTE\"
	path = 
[mergetool "sourcetree"]
	cmd = /Applications/Sourcetree.app/Contents/Resources/opendiff-w.sh \"$LOCAL\" \"$REMOTE\" -ancestor \"$BASE\" -merge \"$MERGED\"
	trustExitCode = true
[user]
	name = w15926
	email = 
[pull]
	rebase = false
[filter "lfs"]
	required = true
	clean = git-lfs clean -- %f
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
[http]
	sslBackend = openssl
	proxy = 127.0.0.1:4780
[remote "origin"]
	proxy = 
[commit]
	template = /Users/lalala/.stCommitMsg
```



# 代理

## 设置代理

> 梯子设置智能代理的时候，git可以不用配置代理。

```shell
git config --global http.proxy https://127.0.0.1:4780
git config --global https.proxy https://127.0.0.1:4780
```



## 取消代理

> ip地址和端口改成你自己的，并且是在开代理的情况下提交。如果不开代理提交出错的话，要么就开启代理，要么就取消设置代理：

```shell
git config --global --unset http.proxy
git config --global --unset https.proxy
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

