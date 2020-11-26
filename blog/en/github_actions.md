@def title = "Make a TODO robot with Github Actions"
@def published = "03 January 2020"
@def tags = ["Tips", "Git","Github", "Github Actions"]


## What's Github Actions
[Github Actions](https://github.com/features/actions) is a __CI__(continuous integration) and __CD__(continuous deployment) service that help you automate your software development workflows in the same place you store code and collaborate on pull requests and issues. It was launched in October 2018 and was officially available to all users in November 2019. With Github Actions you can write individual tasks, called _actions_, and combine them to create a custom workflow. *Workflows* are custom automated processes that you can set up in your repository to build, test, package, release, or deploy any code project on GitHub.

You can create workflows using actions defined in your repository, open source actions in a public repository on GitHub, or a published Docker container image. But _workflows in forked repositories don't run by default._ If you don't want to create your actions by yourself, you can discover actions in the [GitHub Marketplace](https://github.com/marketplace?type=actions), also you can share your actions with the Marketplace.

Ok, now let's go.
## Getting started
We can get this *Send Mail* Actions from [here](https://github.com/marketplace/actions/send-email) or [source](https://github.com/dawidd6/action-send-mail). It help us send email to our inbox.

## Configuring Github Actions
We should create a directory named `.github/workflows` to store our workflow files at the root of our repository. In `.github/workflows`, add a `.yml` or `.yaml` file for our workflow. For example, `.github/workflows/auto.yml`.

### First we set a trigger
```yaml
name: 'GitHub Actions Email Bot'

on:
    schedule:
      - cron: '0 22 * * *'
```
In the above code, `name` is actions description, `on` is the trigger condition. We use the POSIX cron syntax to schedule the workflow. It is triggered at 6:00am(UTC+8) everyday.
### Next writing a TODO list and converting it from markdown to html
```yaml
runs-on: ubuntu-latest
    steps:
          - name: 'Checkout'
            uses: actions/checkout@v2.0.0
          - name: 'Get pandoc'
            run:  sudo apt-get install pandoc
          - name: 'Convert My TODO List'
            run:  pandoc ./TODO.md > todo.html
```

### Last configuring the send email actions
```yaml
    - name: 'Send email'
            uses: dawidd6/action-send-mail@v1.3.0
            with:
              server_address: smtp.gmail.com
              server_port: 465
              username: ${{ secrets.MAIL_USERNAME }}
              password: ${{ secrets.MAIL_PASSWORD }}
              subject: My TODO List
              body: file://todo.html
              to: test@example.com
              from: TODO List Notification
              content_type: text/html
```
For security, we should set our email `username` and `password` in the `settings/secrets` menu of the project. I set my email username like `MAIL_USERNAME`, password like `MAIL_PASSWORD`.

Now we can receive a TODO list email every morning.

----------------

This is my full `todo.yaml`:
```yaml
name: 'GitHub Actions Email Bot'

on:
    schedule:
      - cron: '0 22 * * *'

jobs:
       Todo_bot:
        runs-on: ubuntu-latest
        steps:
          - name: 'Checkout'
            uses: actions/checkout@v2.0.0
          - name: 'Get pandoc'
            run:  sudo apt-get install pandoc
          - name: 'Get My TODO List'
            run:  pandoc ./TODO.md > result.html
          - name: 'Send email'
            uses: dawidd6/action-send-mail@v1.3.0
            with:
              server_address: smtp.gmail.com
              server_port: 465
              username: ${{ secrets.MAIL_USERNAME }}
              password: ${{ secrets.MAIL_PASSWORD }}
              subject: TODO List
              body: file://result.html
              to: test@example.com
              from: TODO List Notification
              content_type: text/html
```

