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
