## git学习笔记

git checkout 切换分支，对文件操作是放弃对文件的修改

#### git reset 
- --soft 回退commit，不会回退已经add的信息。可以用来撤销commit但是未push的操作
- --mixed(默认参数)，回退commit和index索引信息，只保留源码。可以用来撤销add的文件
- --hard,彻底回退到某个版本，什么都不保留

#### git stash
用于缓存未提交的代码
- pop 应用最近一次缓存的stash,可以指定具体的那次的缓存 例：stash@{1}
- list 缓存stash的列表
- drop 删除
- clear 清空stash列表

#### 提交分支到远程
git push origin branch-name

#### 切换远程分支到本地
git checkout -b branch-local origin/branch-remote