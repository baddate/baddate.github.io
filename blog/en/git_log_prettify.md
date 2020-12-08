@def title = "How to prettify git log output"
@def published = "07 August 2019"
@def tags = ["Tips", "Git"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

### 4 formats
```bash
git log --color --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit

git log --color --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

git log --color --stat --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Cblue %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit

git log --graph --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit
```
### Config with alias
__For example:__

_use 4th format._

```bash
git config --global alias.logs "log --graph --date=format:'%Y-%m-%d %H:%M:%S' --pretty=format:'%C(cyan)%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cd) %C(bold blue)<%an>%Creset' --abbrev-commit"
```