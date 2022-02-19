## Add a remote repo
`git remote add origin`


## Multiple github accounts with SSH
### How to?
1. Generate ssh key pairs for accounts and add them to GitHub accounts.

2. Edit/Create ssh config file (~/.ssh/config):

```python
# Default github account: oanhnn
Host github.com
   HostName github.com
   IdentityFile ~/.ssh/oanhnn_private_key
   IdentitiesOnly yes
   
# Other github account: superman
Host github-superman
   HostName github.com
   IdentityFile ~/.ssh/superman_private_key
   IdentitiesOnly yes
```

3. Add ssh private keys to your agent:

``` shell
$ ssh-add ~/.ssh/oanhnn_private_key
$ ssh-add ~/.ssh/superman_private_key
```

4. Test your connection

```shell
$ ssh -T git@github.com
$ ssh -T git@github-superman
```


With each command, you may see this kind of warning, type `yes`:
```
The authenticity of host 'github.com (192.30.252.1)' can't be established.
RSA key fingerprint is xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:
Are you sure you want to continue connecting (yes/no)?
```

If everything is OK, you will see these messages:
```
Hi oanhnn! You've successfully authenticated, but GitHub does not provide shell access.

Hi superman! You've successfully authenticated, but GitHub does not provide shell access.
```

5. Now all are set, just clone your repositories
``` shell
$ git clone git@github-superman:org2/project2.git /path/to/project2
$ cd /path/to/project2
$ git config user.email "superman@org2.com"
$ git config user.name  "Super Man"
```

Done!

**If you are creating a new repository on local:**

-   Initialize Git in the project folder:  
    `git init`.
-   Create the new repository in the GitHub account and then add it as the Git remote to the local repository:  
    `git remote add origin git@github-superman:username/repo_name.git`
-   Push the initial commit to the GitHub repository:  
    `git add .`  
    `git commit -m "Initial commit"`  
    `git push -u origin master`