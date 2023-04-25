## 安装配置

- 安装

  - Git Bash：Linux命令行
  - Git CMD：Windows风格命令行
  - Git GUI：图形化界面

- 查看配置

  ```shell
  git config -l
  git config --system --list # 系统配置
  git config --global --list # 全局配置，用户名和邮箱必须要配置
  
  ```

  





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





## 工作区 暂存区 版本库 远程库

- Workspace    |git add files|    Stage(Index)

  ```shell
  git add . # 添加所有文件到暂存区
  ```

- Stage(Index)    |git commit|    Repository

  ```shell
  git commit -m "xxx" # 提交暂存区内容到本地仓库
  ```

- Repository    |git push|    Remote Directory





- Remote Directory    |git fetch/clone|    Repository

  ```shell
  git clone remoteurl.git # 克隆到本地路径
  ```

- Repository    |git reset|    Stage(Index)

- Stage(Index)    |git checkout|    Workspace

- Remote Directory    |git pull|    Workspace



被Git管理的文件有四种状态：未跟踪(Untracked)，未修改(Unmodified)，已修改(modified)，已暂存(staged)

```shell
git status # 查看文件状态
```

























