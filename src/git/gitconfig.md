### .gitconfig 配置文件

```bash
[filter "lfs"]
	clean = git-lfs clean %f
	smudge = git-lfs smudge %f
	required = true
[user]
	name = bob
[user]
	email = kavin@126.com
[core]
    autocrlf = false
    safecrlf = true
```