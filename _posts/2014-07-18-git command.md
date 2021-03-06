---
layout: post
title: git command
category: Git
tags: [git]
---

## 参考文档
[http://www.imooc.com/article/1111](http://www.imooc.com/article/1111)

[https://git-scm.com/book/zh/v2](https://git-scm.com/book/zh/v2)

[http://blog.csdn.net/sunboy_2050/article/details/7529022](http://blog.csdn.net/sunboy_2050/article/details/7529022)



## 操作

### Command

1. 创建一个工作区域
	* clone      Clone a repository into a new directory
	* init       Create an empty Git repository or reinitialize an existing one

2. 修改
   * add        Add file contents to the index
   * mv         **Move or rename** a file, a directory, or a symlink
   * reset      Reset current HEAD to the specified state
   * rm         Remove files from the working tree and from the index

3. 查看记录和状态
   * bisect     Use binary search to find the commit that introduced a bug
   * grep       Print lines matching a pattern
   * log        Show commit logs
   * show       Show various types of objects
   * status     Show the working tree status
   * reflog

4. 对历史记录进行调整
   * branch     List, create, or delete branches
   * checkout   Switch branches or restore working tree files
   * commit     Record changes to the repository
   * diff       Show changes between commits, commit and working tree, etc
   * merge      Join two or more development histories together
   * rebase     Forward-port local commits to the updated upstream head
   * tag        Create, list, delete or verify a tag object signed with GPG

5. 工作区域合作
   * fetch      Download objects and refs from another repository
   * pull       Fetch from and integrate with another repository or a local branch
   * push       Update remote refs along with associated objects
   
   
### 1 基础命令
* git init		
* git clone git://github.com/project.git --从服务器clone代码
* git status 查看状态
* git config
* git clean -f 	--把临时文件清除 		   

### 2 修改 work on the current change
1. add
	* git add	file	--添加file文件到本地库
	* git add .		--添加当前目录下的所有文件到本地库

2. reset
	* git reset commitId		--hard HEAD 回滚到add之前的状态
	* git reset --hard
	* git reset --soft 
	* git reset origin/master
	* git reset gerrit/master

3. rm
	* `git rm file1，git commit`  	--删除文件（从暂存区和工作区删除）
	* `git rm --cached file, git commit`	--删除文件（只从暂存区）
	* git rm -f file					--强行删除文件（从暂存区和工作区删除）
	* git rm -r gittest //删除目次

* git remote add origin git＠github.com:ellocc/gittest.git //这个相当于指定本地库与github上的哪个项目相连


* git blame wear
* git revert
* git stash			--保存和恢复进度
* git stash pop	


### 3 查看 examine the history and state
* git status
* git log
* git log --oneline --显示所有提交
* git log --author  --显示某个用户的所有提交
* git log -p			--显示某个文件的所有修改
* 
* git reflog 
* git grep "var"
* 


### 4 调整 grow, mark and tweak your common history
1. branch
	* git branch 			--查看本地分支
	* git branch -a		--查看所有分支
	* git branch -r 		--查看远程所有分支
	* git branch -b dev	--新建本地分支dev
	* git branch -D master dev 			--删除本地分支dev
	* git checkout dev	--切换本地分支dev
	* git checkout --track origin/dev 	--切换到远程分支
	* git merge origin/dev					--将分支dev合并到当前分支
	* git fetch origin dev 					--将远程分支拉取下来
	* git branch -r -d branch_remote_name--删除远程分支
	* git branch --track					--基于远程分支创建新的可追溯的分支

2. checkout
	* git checkout dev 	--切换分支
	* git checkout file --放弃修改

3. commit 
	* git commit 
	* git commit --amend
	* git commit -a
	* git commit -m
	* git commit -am ""	--提交并添加注释
	* git commit -v 		--可以看commit的差异
	* git commit --allow-empty
	
4. diff
	* git diff						  --查看当前修改并未add的差异
	* git diff --cached / --staged  --查看已经add但未提交的差异
	* git diff -files/-raw			  --比较暂存区和工作区
	* git diff -index / --cached -raw	--比较暂存区和版本库
	* git diff -tree					  --比较两个树对象
	* git difftool --helper			  --默认使用差异比较工具,怎么关闭
	* 点击q 关闭比对窗口后，鼠标点下刚才执行git difftool终端窗口，按ctrl+c就中断了
	* git diff commitId1 commitId2  --比较两个版本号的差异
	* git diff HEAD

5. merge
	* git merge
	* git mergetool
	* git merge gerrit/master --ff	--将远程master分支合并到本地当前分支

6. rebase
	* git rebase
	* git rebase origin/master
	* git rebase -i (interactive)
	* git rebase --abort 
	* git rebase --continue
	* 

	
7. tag  给当前版本打标签
	* git tag  --查看版本
	* git tag [name] --创建版本
	* git tag -d [name]  --删除版本
	* git tag -r  --查看远程版本
	* git push origin [name] --创建远程版本
	* git push origin :refs/tafs/[name]  --删除远程版本
	* git pull origin --tags  --合并远程tag到本地
	* git push origin --tags  --上传本地tag到远程
	* git tag -a [name] -m "your Message"  -- 创建带注释的tag

```
usage: git tag [-a | -s | -u <key-id>] [-f] [-m <msg> | -F <file>] <tagname> [<head>]
   or: git tag -d <tagname>...
   or: git tag -l [-n[<num>]] [--contains <commit>] [--points-at <object>]
		[--format=<format>] [--[no-]merged [<commit>]] [<pattern>...]
   or: git tag -v <tagname>...

    -l, --list            list tag names
    -n[<n>]               print <n> lines of each tag message
    -d, --delete          delete tags
    -v, --verify          verify tags

Tag creation options
    -a, --annotate        annotated tag, needs a message
    -m, --message <message>
                          tag message
    -F, --file <file>     read message from file
    -s, --sign            annotated and GPG-signed tag
    --cleanup <mode>      how to strip spaces and #comments from message
    -u, --local-user <key-id>
                          use another key to sign the tag
    -f, --force           replace the tag if exists
    --create-reflog       create a reflog

Tag listing options
    --column[=<style>]    show tag list in columns
    --contains <commit>   print only tags that contain the commit
    --merged <commit>     print only tags that are merged
    --no-merged <commit>  print only tags that are not merged
    --sort <key>          field name to sort on
    --points-at <object>  print only tags of the object
    --format <format>     format to use for the output
```

### 5 协作 collaborate
1. fetch
	* git fetch origin
	* git fetch ssh://xxxxx

2. pull
	* git pull ssh://xxxxx
	* 

3. push
	* git push
	* git push gerrit HAED:/refs/master
	* git push -u origin master 	--将本地库提交到github上。

4. cherry-pick
	* git cherry-pick 				--提交拣选
	* git cherry-pick --continue
	* git cherry-pick eecef8a
	* git cherry-pick FETCH_HEAD	

### 6 .gitconfig
1. 配置diff，然后使用git merge

```
git config --global diff.tool diffmerge
git config --global difftool.diffmerge.cmd 'diffmerge "$LOCAL" "$REMOTE"'
git config --global merge.tool diffmerge
git config --global mergetool.diffmerge.cmd 'diffmerge --merge --result="$MERGED" "$LOCAL" "$(if test -f "$BASE"; then echo "$BASE"; else echo "$LOCAL"; fi)" "$REMOTE"'
git config --global mergetool.diffmerge.trustExitCode true
```
2. 配置用户信息

```
git config --global user.name "yourName"
git config --global user.email "yourEmail@xx.com"
```

### 7 .gitignore

1. /开头表示目录
2. * 通配多个字符
3. ？通配单个子符
4. [] 包含单个字符的匹配列表
5. ！ 不忽略匹配到的文件和目录
6. gitignore从上到下进行规制匹配，如果前面的规则匹配范围更大，后面的规则将不会生效

[例子](http://blog.csdn.net/benkaoya/article/details/7932370#comments)(借用别人)

```
# 忽略*.o和*.a文件
 *.[oa]
# 忽略*.b和*.B文件，my.b除外
*.[bB]
!my.b
# 忽略dbg文件和dbg目录
dbg
# 只忽略dbg目录，不忽略dbg文件
dbg/
# 只忽略dbg文件，不忽略dbg目录
dbg
!dbg/
# 只忽略当前目录下的dbg文件和目录，子目录的dbg不在忽略范围内
/dbg
```

### 8 remote
1. git clone git://github.com/xxx/project.git  --检出仓库
2. git remote -v  --查看远程仓库
3. git remote add [name] [url]  --添加远程仓库
4. git remote rm [name]  --删除远程仓库
5. git remote set-url --push [name] [newUrl]  --修改远程仓库
6. git pull [remoteName] [localBranchName]  --拉取远程仓库
7. git push [remoteName] [localBranchName]  --推送到远程仓库
8. git push origin test:master  --提交本地test分作为远程master的分支
9. git push origin test:test --提交本地test分支作为远程test分支



