### 🧪 **Challenge 1: Perform an interactive rebase to modify commit history (rename, squash, reorder commits).**

#### 📌 Objective:
1. Initialize a git repo.
2. Make 4 commits with dummy files.
3. Use interactive rebase to:
   - Reorder the commits.
   - Squash two commits together.
   - Rename a commit message.

---

### ✅ **Step-by-Step Instructions**

---

#### 1. **Initialize a Repo**
```bash
mkdir challange1 && cd challange1
git init
```
> 🎓 `git init` starts a new Git repo in the folder.

---

#### 2. **Create and Commit 4 Files**
```bash
echo "First" > file1.txt
git add .
git commit -m "Add file1"

echo "Second" > file2.txt
git add .
git commit -m "Add file2"

echo "Third" > file3.txt
git add .
git commit -m "Add file3"

echo "Fourth" > file4.txt
git add .
git commit -m "Add file4"
```
> 🎓 You’ve now got 4 commits in your repo.

---

#### 3. **Check the Commit History**
```bash
git log --oneline
```
> 🧠 You'll see something like:
```
aravi@Aravind MINGW64 ~/devops/challange1 (master)
$ git log --oneline
f21c85b (HEAD -> master) Add file4
7f15336 Add file3
bf1226e Add file2
7e7a158 Add file1
```

---

#### 4. **Start an Interactive Rebase**
We want to edit the last 4 commits:
```bash
git rebase -i HEAD~4
```
---

### 🧠 Bonus Tip

If you ever want to rebase all the way from the **first commit**, you can do this:

```bash
git rebase -i --root
```

> This shows *every commit since the beginning of the repo*. Super useful for rewriting the whole history (great for learning, too).

---

> 🛠 This opens a text editor with lines like:
```
pick 7e7a158 Add file1
pick bf1226e Add file2
pick 7f15336 Add file3
pick f21c85b Add file4
# Rebase f21c85b onto 91933da (4 commands)
```

Now let’s:
- ✅ **Rename** "Add file2" to "Second file added"
- ✅ **Squash** "Add file3" into "Add file2"
- ✅ **Reorder** so file2 comes after file4

Change the lines to:, and save it.
```
pick 7e7a158 Add file1
pick f21c85b Add file4
pick bf1226e Add file2
squash 7f15336 Add file3
```

> 🧠 `pick` means "keep this commit as is", `squash` means "combine with the previous commit".

---

#### 5. **Edit the Commit Message When Prompted**
You'll get a prompt like:
```
# This is a combination of 2 commits.
# This is the 1st commit message:

Add file2 and file3 together

# This is the commit message #2:

Add file3

```

Edit it to say:
```
Add file4 and file3 together
```

Save and exit.(:wq)

---

#### 6. **Done! View the New History**
```bash
git log --oneline
```

You should now see something like:
```
1425152 (HEAD -> master) Add file2 and file3 together
1e97c24 Add file4
7e7a158 Add file1
```

---

### ✅ Summary of What You Did
- ✔️ Used `git init` to start fresh
- ✔️ Created 4 commits
- ✔️ Reordered commits using interactive rebase
- ✔️ Squashed 2 commits into 1
- ✔️ Renamed a commit

---

🧪 **Challenge 2: Use `git cherry-pick` to apply a specific commit from another branch to your current branch**

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
### ✅ Summary 

- ✔️ successfully used `git cherry-pick` to apply the commit `1eecaca` from the `dev` branch to your current `master` branch.

- ✔️ The file `master3` was added to the `master` branch which is mistakenly added in `dev` branch, reflecting the changes from the `dev` branch.

- ✔️ It Added one more commit in `master` branch `b884bc6` with the same commit msg from `dev` branch, `master3_dev`.

---

### 🧨 Challenge 3: Create a Merge Conflict Scenario & Resolve It with `git merge` and `git rebase`.**

---

## 🧪 Goal:

Simulate a realistic scenario where:

- `master` and `feature` branches diverge
- Commits are **crossed**
- A merge conflict occurs
- You resolve it using:
  - 🔀 `git merge`
  - 🔁 `git rebase`
- You compare the two workflows using `git log`

---

## ✅ cross Commit Plan

Target commit history:

```
f3 Feature: Add Line D
m4 Master: Add Line Z
f2 Feature: Add Line C
m3 Master: Add Line Y
b1 Feature: Add Line B
m2 Master: Add Line X
m1 Master: Add Line T
a1 Base: Add Line A
```

---

## 🔧 Phase 1: Repository Setup

### 1️⃣ Initialize Repo & Base Commit
```bash
mkdir git-conflict-demo && cd git-conflict-demo
git init
echo "Line A" > file.txt
git add file.txt
git commit -m "a1 Base: Add Line A"
```

---

### 2️⃣ Master Commits: `m1`, `m2`
```bash
echo "Master Line T" >> file.txt
git commit -am "m1 Master: Add Line T"

echo "Master Line X" >> file.txt
git commit -am "m2 Master: Add Line X"
```

---

### 3️⃣ Feature Branch: Commit `b1`
```bash
git checkout -b feature
echo "Feature Line B" >> file.txt
git commit -am "b1 Feature: Add Line B"
```

---

### 4️⃣ Back to Master: Commit `m3`
```bash
git checkout master
echo "Master Line Y" >> file.txt
git commit -am "m3 Master: Add Line Y"
```

---

### 5️⃣ Feature Branch: Commit `f2`
```bash
git checkout feature
echo "Feature Line C" >> file.txt
git commit -am "f2 Feature: Add Line C"
```

---

### 6️⃣ Master: Commit `m4`
```bash
git checkout master
echo "Master Line Z" >> file.txt
git commit -am "m4 Master: Add Line Z"
```

---

### 7️⃣ Feature: Commit `f3`
```bash
git checkout feature
echo "Feature Line D" >> file.txt
git commit -am "f3 Feature: Add Line D"
```

---

### 🔍 8️⃣ View Interleaved Log
```bash
git log --oneline --graph --all --date-order
```

Expected output:

```
* f3 (feature) Feature: Add Line D
* m4 (master) Master: Add Line Z
* f2 Feature: Add Line C
* m3 Master: Add Line Y
* b1 Feature: Add Line B
* m2 Master: Add Line X
* m1 Master: Add Line T
* a1 Base: Add Line A
```

---

## 🔀 Phase 2: Merge with Conflict

### 🔨 Attempt Merge
```bash
git checkout master
git merge feature
```

💥 Conflict in `file.txt`.

---

### 🛠️ Resolve Conflict
Edit `file.txt` to resolve the conflict:

```txt
Line A
Master Line T
Master Line X
Feature Line B
Master Line Y
Feature Line C
Master Line Z
Feature Line D
```

Then commit:

```bash
git add file.txt
git commit -m "m5 Master: Merge feature into master"
```

---

### 📜 Git Log After Merge
```bash
git log --oneline --graph --all
```

```
*   m5 (HEAD -> master) Master: Merge feature into master
|\
| * f3 (feature) Feature: Add Line D
| * f2 Feature: Add Line C
| * b1 Feature: Add Line B
* | m4 Master: Add Line Z
* | m3 Master: Add Line Y
* | m2 Master: Add Line X
* | m1 Master: Add Line T
|/
* a1 Base: Add Line A
```

---

## 🔁 Phase 3: Rebase with Conflict

### 🔄 Reset to Pre-Merge
```bash
git reset --hard m4
git checkout feature
```

---

### 🔁 Rebase Feature onto Master
```bash
git rebase master
```

💥 Conflict will occur → edit `file.txt` as before.

```bash
git add file.txt
git rebase --continue
```

Repeat for each conflict until rebase completes.

---

### ✅ Final Fast-Forward
```bash
git checkout master
git merge feature --ff-only
```

---

### 📜 Git Log After Rebase
```bash
git log --oneline --graph --all
```

```
* f3 (HEAD -> master, feature) Feature: Add Line D
* f2 Feature: Add Line C
* b1 Feature: Add Line B
* m4 Master: Add Line Z
* m3 Master: Add Line Y
* m2 Master: Add Line X
* m1 Master: Add Line T
* a1 Base: Add Line A
```

---

## 🧠 Merge vs Rebase Summary

| Method | History Shape        | Pros                         | Cons                         |
|--------|-----------------------|------------------------------|------------------------------|
| Merge  | Graph with merge node | Preserves full history       | Can be messy for long devs   |
| Rebase | Clean linear commits  | Easy to read, clean timeline | Rewrites history (⚠️ careful) |

---
