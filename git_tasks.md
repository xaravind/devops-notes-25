
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

