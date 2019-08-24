---
title: Add SSH Key
date: 2018-09-23 22:20:20
lastmod: 2019-7-10
tags: 
    - Tips
    - Git
categories: 
    - 2018-09
url: 
    /tips/git_addSSHKey

---

>OS: archlinux	
>terminal: alacritty

1. Open terminal, and entering ssh-keygen 
2. You can find the SSH Key pair in `~/.ssh` directory(note: *id_rsa* is **private key** and *id_rsa.pub* is public key.)
3. Log in github, go to "Account Settings", and you can find "SSH Keys"page. Then click "New SSH Key", enter something for title and pasting contents of *id_rsa.pub* in the `Key`textfield.
