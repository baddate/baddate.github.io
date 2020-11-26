@def title = "Add SSH Key"
@def published = "23 September 2019"
@def tags = ["Tips", "Git"]


>OS: archlinux	
>terminal: alacritty

1. Open terminal, and entering ssh-keygen 
2. You can find the SSH Key pair in `~/.ssh` directory(note: *id_rsa* is **private key** and *id_rsa.pub* is public key.)
3. Log in github, go to "Account Settings", and you can find "SSH Keys"page. Then click "New SSH Key", enter something for title and pasting contents of *id_rsa.pub* in the `Key`textfield.
