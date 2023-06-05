### Git 内部原理

#### 底层命令和高层命令

#### Git 对象

#### Git 引用

```bash
$ git update-ref refs/heads/master 1a410efbd13591db07496601ebc7a059dd55cfe9

$ git update-ref refs/heads/test cac0ca

HEAD 引用 .git/refs/heads
$ git symbolic-ref HEAD
$ git symbolic-ref HEAD refs/heads/test

标签引用 .git/refs/tags
	轻量标签的全部内容——一个固定的引用
$ git update-ref refs/tags/v1.0 [version]
$ git update-ref refs/tags/v1.0 cac0cab538b970a37ea1e769cbbde608743bc96d

	创建一个附注标签来验证这个过程（-a 选项指定了要创建的是一个附注标签）
$ git tag -a v1.1 1a410efbd13591db07496601ebc7a059dd55cfe9 -m 'test tag'

远程引用 .git/refs/remotes
```

#### 包文件

```bash
$ git gc
```

#### 引用规格

引用规格的格式由一个可选的 + 号和紧随其后的 <src>:<dst> 组成，其中 <src> 是一个模式（pattern），代表远程版本库中的引用；<dst> 是那些远程引用在本地所对应的位置。 + 号告诉 Git 即使在不能快进的情况下也要（强制）更新引用。

```bash
$ git remote add origin https://github.com/schacon/simplegit-progit

.git/config

[remote "origin"]
	url = https://github.com/schacon/simplegit-progit
	fetch = +refs/heads/*:refs/remotes/origin/*

$ git log origin/master
$ git log remotes/origin/master
$ git log refs/remotes/origin/master

引用规格推送
$ git push origin master:refs/heads/qa/master

[remote "origin"]
	url = https://github.com/schacon/simplegit-progit
	fetch = +refs/heads/*:refs/remotes/origin/*
	push = refs/heads/master:refs/heads/qa/master

删除引用
$ git push origin :topic
```

#### 传输协议

哑协议

智能协议

#### 维护与数据恢复

维护

```bash
$ git gc --auto
.git/packed-refs
```

数据恢复

```bash
$ git log --pretty=oneline
$ git reset --hard 1a410efbd13591db07496601ebc7a059dd55cfe9
$ git log --pretty=oneline
$ git reflog -10
$ git log -g
$ git branch recover-branch [version]
$ git log --pretty=oneline recover-branch

没有日志了
$ git fsck --full

dangling commit
```

移除对象

```bash
$ git count-objects -v
$ git verify-pack -v .git/objects/pack/pack-29…69.idx | sort -k 3 -n | tail -3
$ git rev-list --objects --all | grep 82c99a3
$ git log --oneline --branches -- git.tgz
$ git filter-branch --index-filter 'git rm --ignore-unmatch --cached git.tgz' -- 7b30847^..
$ rm -Rf .git/refs/original
$ rm -Rf .git/logs/
$ git gc

完全地移除那个对象
$ git prune --expire now
$ git count-objects -v
```

#### 环境变量