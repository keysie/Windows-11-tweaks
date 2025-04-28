# Make ```git``` work with the native Windows ssh client

1. Make sure the native ssh-client actually works first (see other file in this repo)
2. Remove any old git installations
3. Install git using ```winget install --id Git.Git -e --source winget```
4. Edit your global git configration using ```git config --global --edit```
5. Tell git which ssh-binary to use by adding/modifying the ```[core]``` block like this:
   ```
   ...
   [core]
      sshcommand = "C:/Windows/System32/OpenSSH/ssh.exe"
   ...
   ```
6. Also tell git not to use the wrong kind of ssh in case you get errors like ```error fatal: ssh variant 'simple' does not support setting port``` (don't ask me why this is necessary):
   ```
   ...
   [ssh]
      variant = ssh
   ...
   ```
