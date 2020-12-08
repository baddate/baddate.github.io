@def title = "nodejs10.x on CentOS7.6"
@def published = "23 September 2019"
@def tags = ["Tips", "Nodejs", "Linux", "NPM"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

## install nodejs10.x

```bash
curl -sL https://rpm.nodesource.com/setup_10.x | bash -
sudo yum install nodejs
```

## install yarn

```bash
curl -sL https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
yum install yarn
```

## npm change src mirror

### 更换为淘宝源

```bash
# temp use
npm --registry https://registry.npm.taobao.org install express
# change permanently
npm config set registry https://registry.npm.taobao.org
# check
npm config get registry
```
