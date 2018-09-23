# Add SSH Key

1. Open CMD, and entering ssh-keygen -t rsa -C "youremail@example.com"
2. Just Enter.
3. You can find the SSH Key pair in /c/username/.ssh directory(note: *id_rsa* is private key and *id_rsa.pub* is public key.)
4. Log in github, go to "Account Settings", and you can find "SSH Keys"page. Then click "New SSH Key", enter something for title. Pasting *id_rsa* in the `Key`textfield.
5. That's all.