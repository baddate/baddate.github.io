@def title = "Git config username and email"
@def published = "10 July 2019"
@def tags = ["Tips", "Git"]


## User name

1. Open Terminal.

2. Set a Git username:
`git config --global user.name "yourname"`

3. Confirm that you have set the Git username correctly:

```bash
$ git config --global user.name
> yourname
```

## User email

1. Open Terminal.

2. Set a Git useremail:
`git config --global user.email "email@addr.com"`

3. Confirm that you have set the Git user email correctly:

```bash
$ git config --global user.email
> email@addr.com
```

## Add ssh key
### generate ssh key
1. Open git bash and run `ssh-keygen -t rsa -C "youremail"`
2. Open github settings and add `id_rsa.pub` file to ssh key
