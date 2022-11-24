[toc]

# 配置Git

## Install or use system defaults

```shell
brew install git # Or go to the website to download
```



## Generate Key

1. Basic information

```shell
git config --global user.name "your_name"  
git config --global user.email "your_email"
```

2. Get Key

```shell
ssh-keygen -t rsa -C "your_email"
```

> 密钥生成成功后，一般会在本地的/Users/用户/.ssh目录下会生成id_rsa（私钥）、id_rsa.pub（公钥）两个文件。



## Add key(contents in id_rsa.pub) to Github or other

GitHub：settings > SSH and GPG keys > SSH Keys



# 常用命令

### …or create a new repository on the command line

```shell
echo "# WebGIS-loopFrame" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/w15926/WebGIS-loopFrame.git
git push -u origin main
```

### …or push an existing repository from the command line

```shell
git remote add origin https://github.com/w15926/WebGIS-loopFrame.git
git branch -M main
git push -u origin main
```



# 常见报错

fatal: The current branch V2.2 has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin V2.2



## remote: Support for password authentication was removed on August 13, 2021. 

1. 生成个人访问令牌

Github：settings > Developer settings > Personal access tokens > Tokens(classic) > Personal access tokens (classic)

> 生成的令牌要复制保存起来，刷新一下就没了。



2. 修改现有项目的url

```shell
git remote set-url origin  https://<your_token>@github.com/<your_user_name>/<your_project>.git
```

> 替换<your_token>、<your_user_name>、<your_project>即可



克隆的时候

```shell
git clone https://<your_token>@github.com/<your_user_name>/<your_project>.git
```



## Git克隆时报错：abort: repository xxx not found（访问不了Github）

1. 修改host文件，添加 Github 相关域名的绑定。

```shell
open /etc/hosts  # or sudo vi /etc/hosts
```



2. 添加到host文件里（推荐每年更新一次）

> 当前使用的 Github 相关域名比较合适的 IP 值

```shell
# update: 20220222
# Github Hosts
# domain: github.com
140.82.113.4 github.com
140.82.114.9 nodeload.github.com
140.82.112.5 api.github.com
140.82.112.10 codeload.github.com
185.199.108.133 raw.github.com
185.199.108.153 training.github.com
185.199.108.153 assets-cdn.github.com
185.199.108.153 documentcloud.github.com
140.82.114.17 help.github.com

# domain: githubstatus.com
185.199.108.153 githubstatus.com

# domain: fastly.net
199.232.69.194 github.global.ssl.fastly.net

# domain: githubusercontent.com
185.199.108.133 raw.githubusercontent.com
185.199.108.154 pkg-containers.githubusercontent.com
185.199.108.133 cloud.githubusercontent.com
185.199.108.133 gist.githubusercontent.com
185.199.108.133 marketplace-screenshots.githubusercontent.com
185.199.108.133 repository-images.githubusercontent.com
185.199.108.133 user-images.githubusercontent.com
185.199.108.133 desktop.githubusercontent.com
185.199.108.133 avatars.githubusercontent.com
185.199.108.133 avatars0.githubusercontent.com
185.199.108.133 avatars1.githubusercontent.com
185.199.108.133 avatars2.githubusercontent.com
185.199.108.133 avatars3.githubusercontent.com
185.199.108.133 avatars4.githubusercontent.com
185.199.108.133 avatars5.githubusercontent.com
185.199.108.133 avatars6.githubusercontent.com
185.199.108.133 avatars7.githubusercontent.com
185.199.108.133 avatars8.githubusercontent.com
# End of the section
```

