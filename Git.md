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


### see the all changes from specific commit 
```
git diff 54b79f2c0e6a04d8bd83d96e2465f853470028db...HEAD

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

//if it doesn't have remote url in there, edit the config file
vim .git/config 

// arrow down, and press 'o', then paste -> esc, :wq
```