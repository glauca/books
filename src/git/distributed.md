### 分布式 Git

#### 工作流

1. 集中式工作流
2. 集成管理者工作流
3. 司令官与副官工作流 [dictator lieutenant]

#### 向一个项目贡献

将你的工作最少拆分为每个问题一个提交 并且为每一个提交附带一个有用的信息 使同事开发者必须审查你的改动时尽量让事情容易些

```bash
$ git push -u origin featureA:featureB

$ git checkout -b featureC origin/master


$ git request-pull origin/master myfork


$ git checkout featureA
$ git rebase origin/master
$ git push -f myfork featureA


$ git checkout -b featureBv2 origin/master
$ git merge --no-commit --squash featureB
# (change implementation)
$ git commit
$ git push myfork featureBv2
```

```bash
$ git format-patch -M origin/master
```

```bash
分支附带命名空间
$ git branch sc/ruby_client master
or
$ git checkout -b sc/ruby_client master
```

```bash
$ git cherry-pick e43a6fd3e94888d76779ad79fb568ed180e5fcdf

$ git config --global rerere.enabled true
```

```bash
$ git push --tags
```

```bash
生成一个构建号
$ git describe master
```

```bash
准备一次发布
$ git archive master --prefix='project/' | gzip > `git describe master`.tar.gz
$ git archive master --prefix='project/' --format=zip > `git describe master`.zip
```

```bash
制作提交简报
$ git shortlog --no-merges master
$ git shortlog --no-merges master --not v1.0.1
```