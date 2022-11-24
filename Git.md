## Merging branches containing submodules

### Branch A steps

1. Make sure pull all the latest changes from the submodule level
1. Commit and push submodules hash (new commits) - root

### Branch B steps

1. Checkout root and **all submodules** to branch B
1. Make sure pull all the latest changes from the submodule level
1. Merge branch A into branch B on submudule level
1. Push from submodule
1. Commit submodules hash (new commits) - root **Don't push**
1. Merge branch A into branch B
1. Might get conflict style.css -> just recomplied
1. Commit and push


## Gitconfig
```
open "$HOME/.gitconfig"

#write down

[alias]
    s = status
    f = fetch
    a = add
    aa = add --all
    c = commit -m
    ca = commit -am
    co = checkout
    co-all = !sh -c 'git co $1 && git sm co $1' -
    m = merge --no-edit
    l = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
    b = branch
    rh = reset HEAD --hard
    ip = add -ip
    sm = submodule foreach git
    puh = !git push origin "$(git rev-parse --abbrev-ref HEAD)" 
    puh-all = !git push origin "$(git rev-parse --abbrev-ref HEAD)" && git sm puh
    plh = !git pull origin "$(git rev-parse --abbrev-ref HEAD)" --no-rebase
    plh-all = !git pull origin "$(git rev-parse --abbrev-ref HEAD)" --no-rebase && git sm plh
```

### Example
```
git plh-all
git f -a
git sm co $branchname
git sm f -a
```


## See the all changes from specific commit 
```
git diff 54b79f2c0e6a04d8bd83d96e2465f853470028db...HEAD

```

## Use Patch
```
// create patch
git diff patch > $name.patch

open the patch, you can edit that file.

// apply patch
git apply path
``` 

# Error
## When your submodule lost connection
fatal: 'sites/all/react/.git' not recognized as a git repository


```
//try run git innit inside of submodule directory
git init


still not working? (if it seems like lost remote)
//check git config in submodule directory
cat .git/config 

```

1. solution 1:: create .git/config 
```
//if it doesn't have remote url in there, edit the config file
vim .git/config 

// arrow down, and press 'o', then paste -> esc, :wq

```

2. solution 2:: remove submodule file clone again
```
//from the root
rm -rf $submodule-path


// get info from the .gitsubmodules file
git clone $submodule-url $submodule-path
```

# Remove a folder from git tracking 

// git stop tracking directory

- 1. Add the folder path to your repo's root .gitignore file. path_to_your_folder/
- 2. Remove the folder from your local git tracking, but keep it on your disk.
```
git rm -r --cached path_to_your_folder/
```
- 3. Push your changes to your git repo.

# Setup multiple git accounts on MacOs
1. Create SSH keys for two GitHub accounts

```
$ cd ~/.ssh
$ ssh-keygen -t rsa -b 4096 -C "personal@email.com"
$ ssh-keygen -t rsa -b 4096 -C "work@work.com"
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/username/.ssh/id_rsa): /Users/dusername/.ssh/id_rsa_company
```
2. check, and register ssh 

```
$ ls -al ~/.ssh

id_rsa_company.pub   
id_rsa_company      
id_rsa_personal.pub
id_rsa_personal


# Register ssh
$ ssh-add id_rsa_personal
$ ssh-add id-rsa_company
```

3. Add SSH keys to GitHub accounts
- Github:: settings -> SSH and GPG keys -> New SSH key
- paste id_rsa.pub key


4. Setup  ~/.ssh/config file

```
$ sudo nano ~/.ssh/config
```

```
# company
Host company-github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_company

# personal
Host personal-github.com
HostName github.com
User git
IdentityFile ~/.ssh/id_rsa_personal

```

5. Make sure it's connected - ssh -T [host] 
```
$ ssh -T company-github.com
Hi username! You've successfully authenticated, but GitHub does not provide shell access.

$ ssh -T personal-github.com 
Hi username! You've successfully authenticated, but GitHub does not provide shell access.

```

6. Setup gitconfig
 - global config
 - set the condition by using includeIf. (projects under Sites/company will use gitconfig-company.)
 ```
 $ sudo nano ~/.gitconfig
 ```
 ```
 [user]
  email = personal@email.com
  name = personal
[includeIf "gitdir:~/Sites/company/"]
  path = .gitconfig-company
 ```
 - company config
 ```
$ sudo nano ~/.gitconfig-company
 ```
```
[user]
  email = work@email.com
  name = work
```

7. How to use
- check the list at Sites/company/project1
```
~Sites/company/project1
$ git config --list
``` 
-  when you clone the project, git clone [host]:[githubId]/[repositoryName]
 - [host] is what you setup in ~/.ssh/config 
```
# normal way (clone with SSH) -- X
$ git clone git@gitlab.com:user/project1.git

$ git clone work-gitlab.com:user/project1.git
```