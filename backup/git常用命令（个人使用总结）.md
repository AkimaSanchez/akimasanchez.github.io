### git是什么？
&emsp;&emsp;Git是目前最流行的版本控制系统之一，被广泛应用于各种软件开发项目中，包括开源项目和商业项目。不仅能提高开发效率，还帮助开发者更好地管理代码和协作开发。
#### 什么是版本控制系统？
&emsp;&emsp;版本控制系统是一种用于管理文件、代码或其他文档在开发过程中不同版本变化的系统。
&emsp;&emsp;依作者本人的理解就是有多个人在黑板上写东西，从原始的“一”开始，不断推进，写成“不”字，一个人用“不”字写了个“甭”字，另一个人写了个“否”字，最终决定用“否”字，但是，后面大家都觉得“否”字不行，然后我们可以回退到“不”字再开始写。或者说，读者可以看到自己的手机设置中的“关于手机”，在那我们可以看到系统版本，系统的开发过程可以看成一条线，线上的节点会产生不同版本的系统，不知各位有没有刷过机，刷机圈有一句“佳话”：小米的开发版比稳定版稳定（^v^），那时候的小米系统还不是很好用，不堪新系统bug的机主就会刷回他们喜欢的旧版本，也有些会更新到新版本，二者都一定程度上控制了版本。
&emsp;&emsp;接下来介绍几个比较常用git命令，如果不想用git命令，作者建议使用Visual Studio Code，其中的源代码管理功能挺成熟的，简化了版本控制的操作，使用者无需编写git命令也可实现版本控制，可以去B站搜索相关视频进行了解。
1. 添加用户与邮箱
```git
git config --global user.name <your name>
git config --global user.email <your email>
```
2. git配置
```git
git config -e <--global> #编辑git配置文件
git config --list #查看配置
```
3. 创建版本库
```git
git clone <url> #下载一个项目及整个代码历史，加上--depth xx可指定历史数
git submodule update --init --recursive #对于一些很久的分支，需要用该命令更新一下子模块
git status #查看状态，确定有无未跟踪的子模块
```
4. 动他人代码时
```git
git checkout -b <name/date> #为当前分支创建新分支
git add <file name> #添加到暂存区
git commit -m "commit message" #提交并添加信息，这里建议每修改一次提交一次，便于查看，commit message可以替换为如“文件名，修改了什么“，这样方便他人和自己知道代码修改了什么
git push --set-upstream origin <your branch name> #第一次提交时使用，推送本地分支到远程仓库，并设置上游分支
git push -f <remote> <branch> #强制提交
```
5. 撤回提交，同时会保留更改内容，更改内容仍然在工作区中
```git
git reset --soft HEAD^
```
6. 远程操作
```git
git remote -v #查看所有远程仓库
git remote add <shortname> <url> #增加一个新的远程仓库
git fetch <remote> #下载远程仓库所有变动
```
附上网上找的git速查命令，感谢riku大佬：
![git速查命令](https://i.postimg.cc/t40nRfpt/22636a8352a76b978394710aeee45de.jpg)