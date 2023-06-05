### [Pro Git](https://git-scm.com/book/)

#### [.gitconfig](./gitconfig.md) 配置
#### auto_update_git_repository.sh

```bash
#!/bin/bash

cd /www/

git reset --hard
git fetch -p
git clean -fd
git checkout master
git pull --rebase origin master
```

1. [起步](./start.md)
2. [Git 基础](./basic.md)
3. [Git 分支](./branch.md)
4. [Git 服务器](./server.md)
5. [Git 分布式](./distributed.md)
6. [Github](./github.md)
7. [Git 工具](./tools.md)
8. [自定义 Git](./custom.md)
9. [Git 与其他系统](./peer.md)
9. [Git 内部原理](./fundamental.md)
