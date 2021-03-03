@def title = "Git中的空树"
@def published = "8 December 2020"
@def tags = ["Tips", "Git"]

~~~
<ul>
{{for tag in tags}}
  &#x1F516;<a href="/tag/{{fill tag}}" style="text-transform: uppercase;color:#696969">{{fill tag}}</a>
{{end}}
</ul>
~~~

```julia:./tableinput/gen
testcsv = "152,some string, 1.5f0
0,another string,2.87"
write("assets/pages/tableinput/testcsv2.csv", testcsv)
```
\tableinput{custom h1,custom h2,custom h3}{./tableinput/testcsv2.csv}