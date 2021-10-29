# How to set up new Unix environments for Github.com

Just a note to myself on how to set up Github access on a random unix box. This is
sort of [documented](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent),
but I'm trying to distill it.

You can no longer use passwords to remote git repos. So, on the Unix box, first do:
```
$ ssh-keygen -t ed25519 -C "github@saccade.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/cabox/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/cabox/.ssh/id_ed25519.
Your public key has been saved in /home/cabox/.ssh/id_ed25519.pub.
The key fingerprint is:
SHA256:p4hBFj8TcsDsAdtDxhJ3wJlOVhsCUdElQVwm+EdG8lg github@saccade.com
The key's randomart image is:
+--[ED25519 256]--+
|  =@%%%=E        |
|  .*%Bo%o        |
|  .*=o*o.        |
|   oo..o.        |
|    .  .S .      |
|     o . o       |
|    . . .        |
|                 |
|                 |
+----[SHA256]-----+
```
Now you need to run the agent. This needs to be part of your `.bash_login` or whatever.
```
$ eval "$(ssh-agent -s)"
Agent pid 8928
```

Take the key you made above, and "add" it to the agent.
```
$ ssh-add ~/.ssh/id_ed25519
Enter passphrase for /home/cabox/.ssh/id_ed25519: 
Identity added: /home/cabox/.ssh/id_ed25519 (github@saccade.com)
```

Finally, do `cat ~/.ssh/id_ed25519.pub` and copy the output.
Go to https://github.com/settings/keys then click "New SSH key" and paste in the contents.

And remember, you need to do `eval "$(ssh-agent -s)"` for every session you run git in in the future.

Once you jump through these hoops, you should be able to `git push/pull` to a cloned repo on Github.
