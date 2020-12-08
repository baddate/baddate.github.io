@def title = "How to syn git repo between locally and remotely."
@def published = "22 June 2019"
@def tags = ["Tips", "Git"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

### The remote repository has changed and the local repository has not changed

>1. git remote -v
>2. git fetch origin master
>3. git log master.. origin/master
>4. git merge origin/master

### The remote repository has not changed and the local repository has changed

>1. git add -A or git add . or git add --all
>2. git commit -m "the content of your modify"
>3. git push -u origin master
