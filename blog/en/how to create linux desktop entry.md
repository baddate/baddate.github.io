@def title = "How to Create Linux Desktop Entry"
@def published = "13 May 2020"
@def tags = ["Tips", "Linux"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

1. create a file named `yourappname.desktop` in `/usr/share/applications/` directory or `~/.local/share/applications/`.
2. input the following lines:
```bash
[Desktop Entry]
Type=Application
Terminal=false
Exec=/path/to/app
Name=app
Comment=app comment
Icon=/path/to/app/icon
```