@def title = "How to config Git proxy"
@def published = "10 July 2019"
@def tags = ["Tips", "Git", "Proxy"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

### set a socks5 proxy

```bash
git config --global http.proxy socks5://127.0.0.1:1080
git config --global https.proxy socks5://127.0.0.1:1080
```

### set a http proxy

```bash
git config --global http.proxy http://127.0.0.1:1080
git config --global https.proxy http://127.0.0.1:1080
```

### unset proxy

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```