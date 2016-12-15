## Git Guide

### Create a new repository

    git init
    
    
### Checkout a repository
    
    git clone username@host:/path/to/repository


### Push changes

    git add . // 添加到暂存区
    git commit -m "message" // 提交到仓库区
    git remote add origin <server>  // 将你的仓库连接到远程服务器
    git push origin master // 提交到远端仓库
    
### Branch

    git branch -a // 列出所有本地分支和远程分支
    git branch [branch-name] // 新建一个分支，但依然停留在当前分支
    git checkout -b [branch] // 新建一个分支，并切换到该分支
    git branch -d [branch-name] // 删除分支
    git branch -m [name] [newname] // 重命名分支

### Tag

    git tag -d <tagname> // 删除本地tag
    git push origin --delete tag <tagname> // 删除远程tag
    git push --tags // 推送tag

### Rolling back the local repository

	git reset --hard commit-id //回滚到commit-id，讲commit-id之后提交的commit都去除
	git reset --hard HEAD~3 //将最近3次的提交回滚

### Rolling back the remote repository

先将本地分支退回到某个commit，删除远程分支，再重新push本地分支

	git checkout the_branch
	git branch backup //备份分支
	git reset --hard commit_id //回滚到the_commit_id
	git push origin :the_branch //删除远程 the_branch
	git push origin the_branch //用回滚后的本地分支重新建立远程分支
	git push origin :the_branch_backup //成功后删除这个备份分支

## Git Commit Guidelines

```
<type>(<scope>): <subject>
```
### Scope

- main  
- login
- dashboard
- ...

### Type

- feat: A new feature
- fix: A bug fix
- docs: Documentation only changes
- style: Changes that do not affect the meaning of the code (white-space, formatting, missing semi-colons, etc)
- refactor: A code change that neither fixes a bug nor adds a feature
- test: Adding missing tests
- remove: Removing files

example:
```
docs(main): update README.md
```

