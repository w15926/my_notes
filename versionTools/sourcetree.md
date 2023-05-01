# 常见问题

- remote: Support for password authentication was removed on August 13, 2021. 

工具/选项/身份验证/添加/凭据——身份验证必须使用 OAuth，不要选择 Basic。



# 变基 rebase

- 合并另一个分支本身及之前的所有提交记录

方式一：合并 -> 用变基代替合并

方式二：选中其它分支的某条记录 -> 右键 -> 变基

- 合并几次提及为一次

例：

选中第三条记录 -> 右键 -> 交互式变基 -> 勾选 -> 用以前的提及来square -> 确认



# 遴选 cherry-pick

- 合并指定一个分支

选中其它分支的某条记录 -> 右键 -> 遴选

> 跨越多个节点合并容易产生冲突