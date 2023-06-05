### Git 基础

#### 获取 Git 仓库

使用现有项目或目录导入到Git

```bash
$ git init
$ git add *
$ git commit -m 'initial'
```
从服务器克隆Git仓库

```bash
$ git clone [url] [dirname]
```

#### git status

```bash
$ git status
$ git add [filename]
$ git add [dirname]

暂存区状态概览
$ git status -s
$ git status --short
```

#### git diff

````bash
查看修改之后**还没有暂存**起来的变化
$ git diff [filename]

查看已暂存的将要添加到下次提交里的内容
$ git diff --cached [filename]
$ git diff --staged [filename]
````

#### git difftool [vimdiff]
#### git difftool --tool -help

#### git commit
#### git commit -m 'commit description'
#### git commit -a -m 'commit description'

#### git rm

```bash
$ git rm [filename]

如果删除之前修改过并且**已经放到暂存区域**的话
$ git rm -f [filename]

让文件保留在磁盘，但是并不想让 Git 继续跟踪
$ git rm --cached [filename]
```

#### git mv [filename] [new filename]

.gitignore 文件 [@See Github](https://github.com/github/gitignore)

1. 所有空行或者以 ＃ 开头的行都会被 Git 忽略。
2. 可以使用标准的 glob 模式匹配。
3. 匹配模式可以以（/）开头防止递归。
4. 匹配模式可以以（/）结尾指定目录。
5. 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反。

```bash
# no .a files
*.a

# but do track lib.a, even though you're ignoring .a files above
!lib.a

# only ignore the TODO file in the current directory, not subdir/TODO
/TODO

# ignore all files in the build/ directory
build/

# ignore doc/notes.txt, but not doc/server/arch.txt
doc/*.txt

# ignore all .pdf files in the doc/ directory
doc/**/*.pdf
```

#### git log

```bash
$ git log

每次提交的内容差异
$ git log -p -1

每次提交的简略的统计信息
$ git log --stat - 1

格式化显示日志
$ git log -1 --oneline
$ git log -1 --pretty=oneline
$ git log -1 --pretty=short
$ git log -1 --pretty=full
$ git log -1 --pretty=fuller
$ git log -1 --pretty=format:"%h - %an, %ar : %s"
$ git log -10 --date=format:%c --pretty=format:"%h - %an, %ar %cd %s"
$ git log -10 --date=format:%c --pretty=format:"%h - %an, %ar %cd %s" --graph
$ git log -1 --graph --decorate --shortstat --raw --pretty=medium --dirstat

查询条件限制
$ git log --since=2.weeks
$ git log --after=2.weeks
$ git log --before=2.weeks
$ git log --until=2.weeks
$ git log --author=[user]
$ git log --grep=[commit keyword]
$ git log --all-match=[user || commit keyword]
```

#### git 撤销操作

```bash
提交完了发现有漏掉的文件没有提交或者提交信息写错了
$ git commit --amend
```
```bash
取消暂存的文件
$ git reset HEAD [filename]

危险操作 可能导致工作目录中所有当前进度丢失
$ git reset --hard [filename]

不加选项地调用 git reset 并不危险 — 它只会修改暂存区域
```
```bash
撤销对文件的修改
$ git checkout -- [filename]
```

#### 远程仓库使用

查看远程仓库
```bash
$ git remove
$ git remove -v
$ git remote show origin
```

添加远程仓库
```bash
$ git remote add <shortname> <url>
$ git remote add pb https://github.com/paulboone/ticgit
```

从远程仓库中抓取与拉取
```bash
$ git fetch [remote-name]
$ git fetch pb
```

推送到远程仓库
```bash
$ git push [remote-name]
$ git push origin master
```

远程仓库的移除与重命名
```bash
$ git remote rename pb paul
$ git remote rm paul
```

### git 打标签

列出标签
```bash
$ git tag
$ git tag -l 'v1.8.5'
```

附注标签
```bash
$ git tag -a v1.4 -m 'version 1.4'
$ git tag
$ git show v1.4
```

轻量标签
```bash
$ git tag v1.4
$ git tag
$ git show v1.4
```

后期打标签
```bash
$ git tag v1.4 [version]
```

共享标签
```bash
$ git push origin [tag]
$ git push origin --tags
```

检出标签
```bash
$ git checkout -b [branch] [tag]
```

#### Git 别名
```bash
$ git config --global alias.unstage 'reset HEAD --'

$ git unstage [filename]
$ git reset HEAD -- [filename]
```