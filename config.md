<!--
Add here global page variables to use throughout your
website.
The website_* must be defined for the RSS to work
-->
@def website_title = "Alex's Blog"
@def website_descr = "Personal blog site by alex"
@def website_url   = "https://blog.johan.zone"

@def author = "Alex"

@def mintoclevel = 2

<!-- Stuff related to the site styling -->
@def div_content = "container"
@def dir = "/blog/zh"
@def dir_name = "ZH-Blog"
@def dir2 = "/blog/en"
@def dir2_name = "EN-Blog"

<!--
you can use this to add another navi item.
@def dir3 = ""
@def dir3_name = ""
-->

<!--
Add here files or directories that should be ignored by Franklin, otherwise
these files might be copied and, if markdown, processed by Franklin which
you might not want. Indicate directories by ending the name with a `/`.
-->
@def ignore = ["node_modules/", "franklin", "franklin.pub"]

<!--
Add here global latex commands to use throughout your
pages. It can be math commands but does not need to be.
For instance:
* \newcommand{\phrase}{This is a long phrase to copy.}
-->
\newcommand{\R}{\mathbb R}
\newcommand{\scal}[1]{\langle #1 \rangle}
