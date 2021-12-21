# 启动服务

电脑重启后启动（仅适用当前电脑）

``` shell
sudo  /usr/local/mongod/bin/mongod -f /usr/local/mongod/mongod.cnf
```

退出服务

``` shell
use admin
db.shutdownServer()
```



# MongoDB术语/概念

非关系行数据库

| SQL术语/概念 | MongoDB术语/概念 | 解释/说明                            |
| :----------- | :--------------- | :----------------------------------- |
| database     | database         | 数据库                               |
| table        | collection       | 数据库表/集合                        |
| row          | document         | 数据记录行/文档                      |
| column       | field            | 数据字段/域                          |
| index        | index            | 索引                                 |
| table joins  |                  | 表连接，MongoDB不支持                |
| primary key  | primary key      | 主键，MongoDB自动将_id字段设置为主键 |



# 常用命令

### help查看命令提示
- help
- db.help()
- db.test.hrlp()
- db.test.find().help()
### 创建/切换数据库
- use music
### 查询数据库
- show dbs
### 查看当前使用的数据库
- db/db.getName()
### 显示当前DB状态
- db/stats()
### 查看当前DB版本
- db.version()
### 查看当前DB的链接机器地址
- db.getMongo()
### 删除数据库
- db.dropDatabase()