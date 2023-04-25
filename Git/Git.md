## 基础指令

Git版本

```shell
git --version
```



信息配置：配置用户名和邮箱，在版本库提交时用到

```shell
git config --global user.name "xxx"
git config --global user.email "xxx@xx.com"
```



指令别名：更为简洁地使用指令

```shell
git config --global alias.st status
git config --global alias.ci commit
git config --global alias.ch checkout
git config --global alias.br branch
```



开启颜色显示

```shell
git config --global color.ui true
```



初始化Git

```shell
git init
git init url # 自动完成目录创建
```



添加文件到提交任务中

```shell
git add xxx.txt
```



提交到版本库并说明

```shell
git commit -m "msg"
-a:所有变更的文件包括修改和删除，进行提交
```



工作区文件搜索

```shell
git grep "txt"
```



查看提交日志

```shell
git log --pretty=oneline
```



查看差异输出

```shell
git diff
HEAD:版本库与当前工作分支对比
--cached:版本库与暂存区文件差异
--staged:版本库与提交任务文件差异
```



查看文件状态

```shell
git status [-s:精简输出]
```





## 工作区 暂存区 版本库

工作区 add 到暂存区

暂存区(index) commit 到版本库(master/HEAD)

reset HEAD 用工作区目录树重写暂存区目录树

rm --cached <file> 删除暂存区文件

checkout . / -- <file> 工作区文件被暂存区文件覆盖

checkout HEAD . / <file> 版本库master中的文件覆盖暂存区和工作区文件





















