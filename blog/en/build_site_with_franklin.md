@def title = "Build site with Franklin.jl"
@def published = "30 November 2020"
@def tags = ["Courses", "Julia", "Franklin.jl", "Github"]

## Forward

>[Franklin.jl](https://franklinjl.org/) is a simple, customisable static site generator oriented towards technical blogging and light, fast-loading pages.

At first, I want to learning Julia with a small open source project, smaller and better. Then I search on github with topic`#julia`, and I find this tool. As I learned about it on its site, I think I can use this for my massively personal site(maybe it is beautiful, but too complex), and learning Julia when rebuilding my site.

Therefore, I write this tutorial as my beginning.

Last but not least, Franklin.jl has many features:

- Augmented markdown allowing definition of LaTeX-like commands,

- Easy inclusion of user-defined div-blocks,

- Maths rendered via KaTeX, code via highlight.js both can be pre-rendered,

- Can live-evaluate Julia code blocks,

- Live preview of modifications,

- Simple optimisation step to compress and pre-render the website,

- Simple publication step to deploy the website,

- Straightforward integration with Literate.jl.

## Setup Environment

1. Download Julia from [official site](https://julialang.org/downloads/) for your OS.
2. Install it.
3. Open julia REPL.
4. Then excute `using Pkg; Pkg.add(Franklin)`

## Start building
1. New a site with the following scripts:
```julia
julia> using Franklin
julia> newsite("mySite", template="pure-sm")
✓ Website folder generated at "mySite" (now the current directory).
→ Use serve() from Franklin to see the website in your browser.
```
Once you excuted the command above, this Julia REPL workspace will be in your site folder.

2. Start local server for debug:

```julia
julia> serve()
→ Initial full pass...
→ Starting the server...
✓ LiveServer listening on http://localhost:8000/ ...
  (use CTRL+C to shut down)
```
you can get this page in your browser:
![Franklin Example](/_img/franklin_example_page.png)
*note: this `mySite` folder will be created in your current directory. You can check your current directory with`pwd()`.*

## Site Structure
### Page structure
your `mySite` folder structure like this:

```
├─.github
│  └─workflows
├─_assets
│  └─scripts
│      └─output
├─_css
├─_layout
├─_libs
│  ├─highlight
│  ├─katex
│  │  └─fonts
│  └─pure
└─__site
    ├─assets
    │  ├─menu1
    │  │  ├─code
    │  │  │  └─output
    │  │  └─output
    │  └─scripts
    │      └─output
    ├─css
    ├─layout
    ├─libs
    │  ├─highlight
    │  ├─katex
    │  │  └─fonts
    │  └─pure
    ├─menu1
    ├─menu2
    ├─menu3
    └─tag
        ├─code
        ├─image
        └─syntax
```

The `.github` folder is generated for Github Actions, you can config some fields for your customization.Fox example, you can change trigger branch from `dev` to your __workspace branch__ like `master`.
```yaml
name: Build and Deploy
on:
  push:
    branches:
-      - dev
+      - master
```

Then you should change another __BRANCH__(I named it as site branch) field here:
```yaml
    - name: Build and Deploy
      uses: JamesIves/github-pages-deploy-action@releases/v3
      with:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
-       BRANCH: master
+       BRANCH: dev
        FOLDER: __site
```

*NOTE: you must keep the site branch __different from__ the workspace branch(in the case it's `master`), because you site files will be generated on `dev` branch*

If you use config above, your `master` branch is your workspace branch and your `dev` branch is your Github Pages site branch.

## Deploying on GitHub
### local config
1. Create a personal repo on github, named `username.github.io`, or you can name it whatever you want to if you have a domain.

2. Open local folder you created with Franklin before with bash or any other shell.

3. Follow these steps below to config your repo: \
    i)`git remote add origin URL_TO_YOUR_REPO` \
    ii)`git add . && git commit -m "initial files"` \
    iii)`git push -u origin master`

### github config

1. Open `Settings` on your repo page, and scrolling down until you find `GitHub Pages` option.

2. Config it as below:
![Github Pages on Settings](/_img/githubpage.png)

    note: you must make the branch __same__ as the **site branch** in the `deploy.yml`

__DONE!__

You can find some examples of websites using Franklin.jl from [Franklin.jl](https://github.com/tlienart/Franklin.jl).

By the way, I following this [blog site](https://abhishalya.github.io/) as templates and customizing it. It is a very simple and wonderful site.

Please email me if there are any errors.

## REFERENCE

[https://franklinjl.org/](https://franklinjl.org/)

[https://github.com/abhishalya/abhishalya.github.io](https://github.com/abhishalya/abhishalya.github.io)

[https://docs.github.com/](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages)
