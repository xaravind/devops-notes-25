
 **Challenge 2: Use `git cherry-pick` to apply a specific commit from another branch to your current branch**

**Steps** 

1. **Create a directory**
```bash
   mkdir challange2
   ```
   
2. **Go to the directory**
   ```bash
   cd challange2
   ```

3. **Initialize Git**
   ```bash
   git init
   ```

4. **Create two files**
   ```bash
   touch master1 master2
   ```

5. **Add and commit files**
   ```bash
   git add master1
   git commit -m "master1"
   git add master2
   git commit -m "master2"
   ```

6. **Create and switch to `dev` branch**
   ```bash
   git checkout -b "dev"
   ```

7. **Create two files**
   ```bash
   touch master3 dev1
   ```

8. **Add and commit the two files**
   ```bash
   git add master3
   git commit -m "master3_dev"
   git add dev1
   git commit -m "dev1"
   ```

9. **Check Git log**
    ```bash
    $ git log --oneline
    731be94 (HEAD -> dev) dev1
    1eecaca master3_dev
    61bcfaa (master) master2
    613e0df master1
    ```

10. **Switch to `master` branch**
   ```bash
   git checkout master
   ```
11. **Check Git log**
    ```bash
    61bcfaa (master) master2
    613e0df master1
    ```

12. **Use `git cherry-pick`**
    ```bash
    aravi@Aravind MINGW64 ~/devops/challange1 (master)
    $ ll
    total 0
    -rw-r--r-- 1 aravi 197609 0 Apr  4 12:21 master1
    -rw-r--r-- 1 aravi 197609 0 Apr  4 12:21 master2

    aravi@Aravind MINGW64 ~/devops/challange1 (master)
    $ git cherry-pick 1eecaca
    [master fde6e87] master3_dev
     Date: Fri Apr 4 12:23:14 2025 +0530
     1 file changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 master3

    aravi@Aravind MINGW64 ~/devops/challange1 (master)
    $ ll
    total 0
    -rw-r--r-- 1 aravi 197609 0 Apr  4 12:21 master1
    -rw-r--r-- 1 aravi 197609 0 Apr  4 12:21 master2
    -rw-r--r-- 1 aravi 197609 0 Apr  4 12:30 master3
    ```
12. **Check `git log --oneline`**
    ```bash
    $ git log --oneline
    b884bc6 (HEAD -> master) master3_dev
    61bcfaa master2
    613e0df master1
    ```

successfully used `git cherry-pick` to apply the commit `1eecaca` from the `dev` branch to your current `master` branch.

The file `master3` was added to the `master` branch which is mistakenly added in `dev` branch, reflecting the changes from the `dev` branch.

It Added one more commit in `master` branch `b884bc6` with the same commit msg from `dev` branch, `master3_dev`.

---
**Challenge 3: Create a merge conflict scenario and manually resolve it using git merge and git rebase.**

1. **Create a directory**
   ```bash
   mkdir challange3
   ```

2. **Go to the directory**
   ```bash
   cd challange3
   ```

3. **Initialize Git**
   ```bash
   git init
   ```

4. **Create two files**
   ```bash
   touch master1 master2
   ```

5. **Add and commit files**
   ```bash
   git add master1
   git commit -m "master1"
   git add master2
   git commit -m "master2"
   ```    

6. **check `git` log**
   ```bash
   git log --oneline
   ```
   ```bash
   6bf2bcf (HEAD -> master) master2
   febe8cc master1
   ```


7. **Create and switch to `dev` branch**
   ```bash
   git checkout -b "dev"
   ```

8. **Create two files**
   ```bash
   touch dev1 dev2
   ```

9. **Add and commit the two files**
   ```bash
   git add dev1
   git commit -m "dev1"
   git add dev2
   git commit -m "dev2"
   ```

10. **Add content in master1**
   ```bash
    vi master1
   ```
11. **Add and commit file**
   ```bash
   git add master1 ; git commit -m "added three lines from dev"
   ```

12. **check `git` log**
   ```bash
   git log --oneline
   ```
   ```bash
   e3afe62 (HEAD -> dev) dev2
   f75b94b dev1
   6bf2bcf (master) master2
   febe8cc master1
   ------------
   $ cat master1
   added from dev
   merge
   conflict
   $ ll
   total 1
   -rw-r--r-- 1 aravi 197609  0 Apr  5 11:38 dev1
   -rw-r--r-- 1 aravi 197609  0 Apr  5 11:38 dev2
   -rw-r--r-- 1 aravi 197609 30 Apr  5 11:38 master1
   -rw-r--r-- 1 aravi 197609  0 Apr  5 11:34 master2
   ```

13. **switch to `master` branch**
   ```bash
   git checkout master
   ```

14. **before merging `dev` branch add content in master1, add and commit**
    ```bash
    echo "added from master" >> master1
    git add master1 ; git commit -m "added line from master"
    ```
    ```bash
      $ git log --oneline
      cf40f64 (HEAD -> master) added three line from master
      eb2a0aa master2
      97ce171 master1 
      $ ll
      total 1
      -rw-r--r-- 1 aravi 197609 18 Apr  5 10:17 master1
      -rw-r--r-- 1 aravi 197609  0 Apr  5 09:47 master2
    ```
15. **now merge `dev` branch, now we will get conflict a conflict since there are two different contents on the same line.**

    ```bash
    git merge dev
    ```

    ```bash
    we will get below output
    Auto-merging master1
    CONFLICT (content): Merge conflict in master1
    Automatic merge failed; fix conflicts and then commit the result.   
    ```

16. **Open and check `master1` file**
    ```bash
    cat master1
    ```

    ```bash
    <<<<<<< HEAD
    added from master
    =======
    added from dev
    merge
    conflict
    >>>>>>> dev
    ```

17. **manually resolve the conflicts and remove all unnecessary lines**
    ```bash
    vi master1
    ```

    ```bash
    aravi@Aravind MINGW64 ~/devops/challange3 (master|MERGING)
    $ cat master1
    added from master
    added from dev
    merge
    conflict  
    ```
18. **Add and commit file**
   ```bash
   git add master1 ; git commit -m "conflict resolved"
   ```

19. **check `git` log, successfully merged branch with merge**

   ```bash
   aravi@Aravind MINGW64 ~/devops/challange3 (master)
    $ git log --oneline
    68004a1 (HEAD -> master) conflict resolved
    cf40f64 added line from master # merge commit id
    f1c5d3b (dev) added three lines from dev
    f2c0b8e dev2
    360a2b3 dev1
    eb2a0aa master2
    97ce171 master1
   ```

 **Observe that the commit history is not linear; commits from both branches, dev and master, have overlapped**
