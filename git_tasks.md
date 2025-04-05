
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

Sure, here's the modified version without the word "you":

---

### **Git Merge vs Git Rebase: Practice Notes**

---

### **Step-by-Step Breakdown**

#### **1. Initialize Git Repository**

A new Git repository was created:

```bash
mkdir challange3
cd challange3
git init
```

This initializes an empty Git repository.

---

#### **2. Create and Commit Files on `master` Branch**

Two files (`file.txt` and `file2.txt`) were added on the `master` branch and committed:

```bash
echo "from master file 1" > file.txt
echo "from master in file2" > file2.txt
git add file.txt ; git commit -m "added file"
git add file2.txt ; git commit -m "added second file"
```

Two files were committed on the `master` branch.

---

#### **3. Create and Switch to a New `dev` Branch**

A new branch `dev` was created and switched to:

```bash
git checkout -b "dev"
```

This created a new branch called `dev` and switched to it.

---

#### **4. Modify `file.txt` on `dev` Branch**

On the `dev` branch, `file.txt` was modified and committed:

```bash
echo "added second line in file from dev" >> file.txt
git add file.txt ; git commit -m "dev commit in file"
```

At this point, the `dev` branch contains changes to `file.txt`.

---

#### **5. Switch Back to `master` and Modify `file.txt`**

Switched back to the `master` branch and modified `file.txt`:

```bash
git checkout master
echo "added second line in file from master, will get conflict" >> file.txt
git add file.txt ; git commit -m "master commit second line in file"
```

Now, both `master` and `dev` branches have different changes to the same line in `file.txt`, setting up for a merge conflict.

---

#### **6. Switch Between Branches and Make More Changes**

Switched between branches, making additional changes on both `master` and `dev`:

1. On `dev`:

```bash
git checkout dev
echo "added third line in file from dev" >> file.txt
git add file.txt ; git commit -m "dev commit third line in file"
```

2. On `master`:

```bash
git checkout master
echo "added third line in file from master, will get conflict" >> file.txt
git add file.txt ; git commit -m "master commit third line in file"
```

Now, both branches have conflicting changes at multiple points in `file.txt`.

---

#### **7. Compare Commit Histories**

Commit history was checked on both `master` and `dev` branches:

- **On `master`**:

```bash
git log --oneline
```

The commits made on the `master` branch:

```
723f5a1 master commit third line in file
a427be2 master commit second line in file
1c61bbb added second file
fd928fd added file
```

- **On `dev`**:

```bash
git log --oneline
```

The commits made on the `dev` branch:

```
5b69953 dev commit third line in file
ccf6884 dev commit in file
1c61bbb added second file
fd928fd added file
```

---

#### **8. Performing a Git Merge**

Attempted to merge `dev` into `master`:

```bash
git checkout master
git merge dev
```

This caused a **merge conflict** in `file.txt` because both branches had changes to the same lines in the file:

```
CONFLICT (content): Merge conflict in file.txt
```

The conflict was resolved manually and committed:

```bash
git add file.txt ; git commit -m "merge conflict resolved"
```

After resolving the conflict, the history looked like this:

```
7e7d569 merge conflict resolved
723f5a1 master commit third line in file
5b69953 dev commit third line in file
a427be2 master commit second line in file
ccf6884 dev commit in file
1c61bbb added second file
fd928fd added file
```

---

#### **9. Performing a Git Rebase**

The branch was reset to before the merge commit and a rebase was performed instead of a merge:

```bash
git reset --hard HEAD~1  # Go back before merge commit
git rebase dev
```

This also resulted in a **rebase conflict** because the changes in `dev` and `master` conflicted at the same points in `file.txt`:

```
CONFLICT (content): Merge conflict in file.txt
```

The conflict was resolved manually:

```bash
git add file.txt ; git commit -m "rebase conflict resolved"
git rebase --continue
```

After resolving the conflict, the history looked like this:

```
02881bf master commit third line in file
9888f56 rebase conflict resolved
5b69953 dev commit third line in file
ccf6884 dev commit in file
1c61bbb added second file
fd928fd added file
```

---

### **Summary of Differences Between Git Merge and Git Rebase**

1. **Commit History Structure**:
   - **Merge**: Keeps the **branched structure** of the history, with a **merge commit** combining the branches.
   - **Rebase**: **Rewrites** the commit history to make it **linear**, as if the changes in `dev` were always part of `master`.

2. **Merge Commit**:
   - **Merge**: Creates a **merge commit** that ties the two branches together.
   - **Rebase**: **No merge commit** is created; the changes are re-applied on top of the branch.

3. **Conflict Resolution**:
   - **Merge**: Conflicts are resolved once during the merge, and the merge commit is created afterward.
   - **Rebase**: Conflicts occur multiple times during the rebase, requiring resolution at each step before continuing the rebase.

4. **Commit Hashes**:
   - **Merge**: The commit hashes remain the same for the commits in the branches.
   - **Rebase**: The commit hashes are **rewritten** as the commits are re-applied on top of the target branch.

---

### **Key Takeaways**
- Use `git merge` when preserving the context of the branches is important, showing how they came together.
- Use `git rebase` for a clean, linear history, as though the commits in the feature branch were directly made on the main branch.

This exercise helps in understanding how each operation handles conflicts and commit history differently.
 
