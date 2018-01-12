## github add ssh
  
* __配置 user name 和 user email__

  $ git config --global user.name "example"
  $ git config --global user.email "example@qq.com"

* __生成ssh key__

  $ ssh-keygen -t rsa -C "example@qq.com"
  第一个提示：ssh文件的保存路径， 直接回车采用默认即可
  第二个提示：输入密码
  第三个提示：重复输入密码
  最后得到了两个文件：id_rsa和id_rsa.pub
  
* __add ssh key to github__

  登录github，在settings -> SSH Keys中添加ssh密钥
  添加时，将本地文件id_rsa.pub打开，将内容粘贴到网站输框中提交即可。
  
  
## git的基本操作
    
    克隆：$ git clone <url>
  
    更新：$ git pull

    加入暂存区：$ git add <file>
  
    加入暂存区(所有文件)：$ git add .   (提示：提前添加.gitignore文件，过滤不用提交的内容）
  
    查看状态：$ git status
  
    提交：$ git commit -m "第一次提交"
  
    推送至远程仓储：$ git push -u origin master
    
    强制推送至远程仓储：$ git push -f origin master

## 版本回退

    查看日志：$ git log

    回到指定版本：$ git reset --hard <commit_id>

    回到上个版本：$ git reset --hard HEAD^

    查看命令历史：$ git reflog

## 撤销修改

    场景1：撤销工作区的修改 $ git checkout -- <file>

    场景2：撤销已添加到暂存区的修改 $ git reset Head <file>

    场景3：已提交到版本库，使用上面的版本回退功能

## 删除文件

    本地删除：$ rm <file>

    从版本库删除：$ git rm <file>

## 分支管理

    查看分支：$ git branch
    
    查看远程分支：$ git branch -a

    创建分支：$ git branch <name>

    切换分支：$ checkout <name>

    创建+切换分支：$ checkout -b <name>

    合并某分支到当前分支(快速合并,日志无合并记录)：$ git merge <name>

    合并某分支到当前分支(普能合并,会创建一个新的commit,日志有合并记录)：$ git merge --no-ff -m "测试普通合并" <name>

    删除分支：$ git branch -d <name>

    强行删除未合并过的分支：$ git brach -D <name>

    查看分支合并图：$ git log --graph

## 主干bug修改(临时紧急修改)
  
    1、暂存当前分支的工作现场(假定当前分支为dev,未被git管理的文件不会被保存)：$ git stash

    2、切换至主干：$ checkout master

    3、创建并切换至临时分支：$ git branch -b bug001

    4、修复并提交bug

      4.1、$ git add <bugfile>

      4.2、$ git commit -m '修复bug001'

      4.3、$ git checkout master

      4.4、$ git merge -no-ff -m '合并修复bug001' bug001

      4.5、$ git branch -d bug001

    5、切回工作分支：$ git chckout dev

    6、查看暂存的快照(工作现场)：$ git stash list

    7、恢复快照

      7.1、恢复快照：$ git stash apply

      7.2、删除快照：$ git stash drop

      * 捷命令合并+删除快照：$ git stash pop

## 远程仓库
  
    查看远程库信息：$ git remote  （详细信息：$ git remote -v)

    一般推送远程仓库版本

      * master分支是主分支，因此要时刻与远程同步；

      * dev分支是开发分支，团队成员共用分支，需要与远程同步；

      * bug分支只用于在本地修复bug，不用推到远程；

      * feature分支是否推到远程，取决于合作开发还是个人开发;

## 标签管理

    1、在当前分支上打标签（默认打在最近的commit)：$ git tag <tagname>

    2、给历史commit打标签

      2.1、查看commit历史：$ git log --pretty=online --abbrev-commit

      2.2、通过commit_id打标签：$ git tag <tagname> <commit_id>

      * 打标签 + commit_id + 说明：$ git tag -a <tagname> -m "version 0.1 released" <commit_id>

    3、查看标签

      * 查看标签：$ git tag

      * 查看指定标签及说明: $ git show <tagname>

    4、推送本地标签：$ git push origin <tagname>

    5、推送所有未推送过的本地标签：$ git push origin --tags

    6、删除本地标签：$ git tag -d <tagname>

    7、删除远程标签：$ git push origin :refs/tags/<tagname>


## 其它配置

    1、git显示颜色：$ git config --global color.ui true

    2、忽略特殊文件：添加.gitignore文件

    3、配置别名

      * 配置别名：$ git config --global alias.<alias> <command>

      * 删除别名：

      * 示例配置st表示status：$ git config --global alias.st status, 使用时输入: $ git st;

      * 示例配置命令git reset HEAD file(可以把暂存区的修改撤销掉)为unstage：$ git config --global alias.unstage 'reset HEAD' 使用方法：$ git unstage;

      * 丧心病狂的配置(听说这命令很好用)：git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

    4、删除配置： 当前用户的Git配置文件放在用户主目录下的一个隐藏文件.gitconfig中，找到alias删除对应内容
    
    5、git config --global core.autocrlf true  签出代码的时候自动转换换行符为windows的格式，避免出现文件内容没变化但是文件被显示有修改的情况

## 搭建Git服务器

  * 待完善

## 解决冲突

    1、保用本地修改或手动修改合并后的内容
      git ptash
      git pull
      git stash pop
      然后可以使用git diff -w +文件名 来确认代码自动合并的情况
    
    2、用代码库中的文件完全覆盖本地工作版本
      git reset --hard
      git pull
      
 ## 删除远程分支已删除的本地分支(update by 2017-02-27)
   
    1、git remote show origin
    
    2、git remote prune origin
    
    3、状态为stale的本地分支将被删除
      
## 查看历史修改

    1. git log filename   可以看到fileName相关的commit记录
    
    2. git log -p filename  可以显示每次提交的diff
    
    3. git show c5e69804bbd9725b5dece57f8cbece4a96b9f80b filename 只看某次提交中的某个文件变化，可以直接加上fileName
    
