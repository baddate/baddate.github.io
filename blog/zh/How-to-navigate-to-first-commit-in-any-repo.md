@def title = "如何快速查看github代码库中某次commit的记录"
@def published = "3 December 2020"
@def tags = ["Tips", "Github"]

如果你想要学习一个开源库，最好的方法就是从头开始看源码，所以你可能想要从第一次commit开始看。有的人可能觉得很简单啊，进入commit页面直接翻到最后一页就好了啊。的确，如果是你看的是一个小的项目，提交次数不会太多，你可能翻几页就到最后了，可是，如果是像Linux这种项目，将近100k次提交，手动翻页翻到明年了。。。

![linux commit](https://cdn.jsdelivr.net/gh/baddate/imagedb/linux-commits-count.png)



![](https://cdn.jsdelivr.net/gh/baddate/imagedb/linux-commit.png)

这时候就需要一个简单的方法能够定位到第一次commit。

所以第一个想法就是在URL上操作，仔细观察对比之后可以发现，GitHub的翻页是根据commit的SHA值来定位的，

第二页的url是

[https://github.com/torvalds/linux/commits/master?after=127c501a03d5db8b833e953728d3bcf53c8832a9+34&branch=master](https://github.com/torvalds/linux/commits/master?after=127c501a03d5db8b833e953728d3bcf53c8832a9+34&branch=master)

仔细观察其中的`127c501a03d5db8b833e953728d3bcf53c8832a9`是最新一次commit的SHA值，后面还有一个`+34`就是定位到第36次(因为是after1+34，所以要是36)的commit。所以要定位到Linux的第一次commit就可以把+34改为+967826(967828-2)

![](https://cdn.jsdelivr.net/gh/baddate/imagedb/linux-commit-first.png)

到这里就成功定位到第一次commit了，当然，以此类推，你可以定位到任何一个commit。