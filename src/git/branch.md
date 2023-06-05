### 分支

Git 的 master 分支并不是一个特殊分支 它就跟其它分支完全没有区别 之所以几乎每一个仓库都有 master 分支 是因为 git init 命令默认创建它 并且大多数人都懒得去改动它

#### 创建分支
```bash
$ git branch [new branch]
```

```bash
$ git log --oneline --decorate
```

分支切换
```bash
$ git checkout [branch name]
```

```bash
$ git log --oneline --decorate --graph --all
```

#### 分支的新建与合并

```bash
$ git checkout -b [new branch]
```

```bash
$ git merge [branch name]
```

```bash
$ git branch -d [branch name]
$ git branch -D [branch name]
```

```bash
$ git branch --merged/--no-merged
```

#### 远程分支

```bash
$ git clone -o [remote name] [url] [dirname]
```

#### 同步远程分支
```bash
$ git fetch [remote name]
```

#### 推送
```bash
$ git push [remote name] [local branch]
$ git push [remote name] [local branch]:[remote branch]
```

```bash
将远程分支合并到当前所在的分支
$ git merge origin/serverfix

如果想要在自己的 serverfix 分支上工作 可以将其建立在远程跟踪分支之上
$ git checkout -b serverfix origin/serverfix
```

#### 跟踪分支
```bash
$ git checkout -b [branch] [remotename]/[branch

跟踪远程仓库
$ git checkout --track origin/serverfix

将本地分支与远程分支设置为不同名字
$ git checkout -b sf origin/serverfix

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支
$ git branch -u origin/serverfix
```

```bash
$ git branch -vv
```

#### 删除远程分支
```bash
$ git push origin --delete [branch name]
```

#### 重命名远程分支
```bash
$ git push --delete origin [branch]
$ git branch -m [branch] [new branch]
$ git push origin [new branch]
```

#### 变基
```bash
$ git checkout develop
$ git rebase master
$ git checkout master
$ git merge develop
```

![从一个特性分支里再分出一个特性分支的提交历史](https://git-scm.com/book/en/v2/book/03-git-branching/images/interesting-rebase-1.png)

假设你希望将 client 中的修改合并到主分支并发布 但暂时并不想合并 server 中的修改 因为它们还需要经过更全面的测试 这时 你就可以使用 git rebase 命令的 --onto 选项 选中在 client 分支里但不在 server 分支里的修改（即 C8 和 C9） 将它们在 master 分支上重演

```bash
$ git checkout client
$ git rebase --onto master server client
$ git checkout master
$ git merge client

$ git rebase master server
$ git checkout master
$ git merge server

$ git branch -d client
$ git branch -d server
```

以上命令的意思是 "取出 client 分支，找出处于 client 分支和 server 分支的共同祖先之后的修改，然后把它们在 master 分支上重演一遍"" 这理解起来有一点复杂 不过效果非常酷

```bash
git pull --rebase
```