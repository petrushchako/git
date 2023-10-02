
## Squash merge
#### At this point you have **master** (m1,m2,m3) and **feature1** (m2,f1,f2)

```
git checkout master
git merge --squash feature1
git commit -m "Squash and merge msg"
```

## Rebase
#### At this point you have **master** (m1,m2,m3) and **feature1** (m2,f1)
```
git checkout feature1
git rebase master
git log
git checkout master
git rebase feature1
```
#### This command will bring **m3** commit from **master** to the **feature1** branch. 
At the backend:
* git checks the last commit two branches had in common.
* Store the additional changes made to **feature1** branch in temp file.
* Brings the additional change from **master** to **feature1** branch.
* Finally, the backed up changes from **feature1** are applied back to **feature1** branch.
* Note: After this operation is completed, you can run the **git log** command in **feature1** branch, so see, that the base branch now has a record of **m3** commit, rather than **m2** as it was previously. 

