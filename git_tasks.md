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
mkdir challenge1 && cd challenge1
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
   mkdir challenge2
   ```
   
2. **Go to the directory**
   ```bash
   cd challenge2
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

#### 🧪 Objective:
- Create interleaved commits across `master` and `feature`
- Introduce merge conflict
- Resolve using:
  - 🔀 `git merge`
  - 🔁 `git rebase`
- Compare `git log` outputs

---

### 🗺️ Commit Timeline Plan

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

## 🔧 Part 1: Setup

#### 🔹 Step 1️⃣: Init
```bash
mkdir challenge3 && cd chellange3
git init
```

#### 🔹 Step 2️⃣: Base Commit `a1`
```bash
echo "Line A" > file.txt
git add file.txt
git commit -m "a1 Base: Add Line A"
```

#### 🔹 Step 3️⃣: Master Commits `m1`, `m2`
```bash
echo "Master Line T" >> file.txt
git commit -am "m1 Master: Add Line T"

echo "Master Line X" >> file.txt
git commit -am "m2 Master: Add Line X"
```

#### 🔹 Step 4️⃣: Create `feature`, Commit `b1`
```bash
git checkout -b feature
echo "Feature Line B" >> file.txt
git commit -am "b1 Feature: Add Line B"
```

#### 🔹 Step 5️⃣: Master Commit `m3`
```bash
git checkout master
echo "Master Line Y" >> file.txt
git commit -am "m3 Master: Add Line Y"
```

#### 🔹 Step 6️⃣: Feature Commit `f2`
```bash
git checkout feature
echo "Feature Line C" >> file.txt
git commit -am "f2 Feature: Add Line C"
```

#### 🔹 Step 7️⃣: Master Commit `m4`
```bash
git checkout master
echo "Master Line Z" >> file.txt
git commit -am "m4 Master: Add Line Z"
```

#### 🔹 Step 8️⃣: Feature Commit `f3`
```bash
git checkout feature
echo "Feature Line D" >> file.txt
git commit -am "f3 Feature: Add Line D"
```

---

#### 🔍 Step 9️⃣: Git Log Before Merge
```bash
git log --oneline --graph --all --date-order
```

```
As of now git logs from both branches
aravi@Aravind MINGW64 ~/devops/challange3 (master)
$ git log --oneline --graph  --date-order
* 4178923 (HEAD -> master) m4 Master: Add Line Z
* b5cb3aa m3 Master: Add Line Y
* f91879a m2 Master: Add Line X
* ac130f9 m1 Master: Add Line T
* bfb2dc8 a1 Base: Add Line A

aravi@Aravind MINGW64 ~/devops/challange3 (feature)
$ git log --oneline
a8165e0 (HEAD -> feature) f3 Feature: Add Line D
e541989 f2 Feature: Add Line C
894db87 b1 Feature: Add Line B
f91879a m2 Master: Add Line X
ac130f9 m1 Master: Add Line T
bfb2dc8 a1 Base: Add Line A

```

---

## 🔀 Part 2: Merge Conflict Flow

#### 🔹 🔟 Merge `feature` into `master`
```bash
git checkout master
git merge feature
```
```
you'll see like below

Switched to branch 'master'
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.

```

💥 Conflict in `file.txt`

#### 🔹 Step 1️⃣1️⃣: Resolve Merge Conflict
Edit `file.txt`:
```
aravi@Aravind MINGW64 ~/devops/challange3 (master|MERGING)
$ cat file.txt
Line A
Master Line T
Master Line X
Feature Line B
Master Line Y
Feature Line C
Master Line Z
Feature Line D

```
Then:
```bash
git add file.txt
git commit -m "m5 Master: Merge feature into master"
```

#### 🔍 Step 1️⃣2️⃣: Git Log After Merge
```bash
git log --oneline
```

```
aravi@Aravind MINGW64 ~/devops/challange3 (master)
$ git log --oneline
60ea1d4 (HEAD -> master) m5 Master: Merge feature into master
a8165e0 (feature) f3 Feature: Add Line D
4178923 m4 Master: Add Line Z
e541989 f2 Feature: Add Line C
b5cb3aa m3 Master: Add Line Y
894db87 b1 Feature: Add Line B
f91879a m2 Master: Add Line X
ac130f9 m1 Master: Add Line T
bfb2dc8 a1 Base: Add Line A

```
### 📝 Summary: Why Step 12 Log Looks Overlapped After Merge

- `git merge` creates a **merge commit** with **two parents**: one from `master`, one from `feature`.
- Running `git log --oneline` **flattens** the history — it doesn’t show the branch structure.
- This causes commits from both branches to appear **mixed and unordered**, making it hard to read.

✅ **Fix**: Use a structured log view:
```bash
git log --oneline --graph --all --decorate --date-order
```

```
aravi@Aravind MINGW64 ~/devops/challange3 (master)
$ git log --oneline --graph --all --decorate --date-order
*   60ea1d4 (HEAD -> master) m5 Master: Merge feature into master
|\
| * a8165e0 (feature) f3 Feature: Add Line D
* | 4178923 m4 Master: Add Line Z
| * e541989 f2 Feature: Add Line C
* | b5cb3aa m3 Master: Add Line Y
| * 894db87 b1 Feature: Add Line B
|/
* f91879a m2 Master: Add Line X
* ac130f9 m1 Master: Add Line T
* bfb2dc8 a1 Base: Add Line A
```

This shows:
- 🔀 True branching history
- 📌 Which commits came from `master` vs `feature`
- 🔁 How the merge commit connects them

Much easier to follow! 🙌

---

## 🔁 Part 3: Rebase Conflict Flow

#### 🔹 Step 1️⃣3️⃣: Reset & Checkout `master`
```bash
git reset --hard HEAD~1
git checkout master
```

#### 🔹 Step 1️⃣4️⃣: Rebase Onto `master`
```bash
git rebase feature
```

```
you'll see like below

Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
error: could not apply b5cb3aa... m3 Master: Add Line Y
hint: Resolve all conflicts manually, mark them as resolved with
hint: "git add/rm <conflicted_files>", then run "git rebase --continue".
hint: You can instead skip this commit: run "git rebase --skip".
hint: To abort and get back to the state before "git rebase", run "git rebase --abort".
hint: Disable this message with "git config advice.mergeConflict false"
Could not apply b5cb3aa... m3 Master: Add Line Y

aravi@Aravind MINGW64 ~/devops/challange3 (master|REBASE 1/2)
```

💥 Conflict in `file.txt`. Fix to:
```
Line A
Master Line T
Master Line X
Feature Line B
Master Line Y
Feature Line C
Master Line Z
Feature Line D
```
Then:
```bash
git commit -am "conflict resolved"
git rebase --continue
```

#### 🔹 Step 1️⃣5️⃣: Fast-Forward Merge
```bash
git checkout master
git merge feature --ff-only
```

#### 🔍 Step 1️⃣6️⃣: Git Log After Rebase
```bash
git log --oneline --graph --all
```
```
aravi@Aravind MINGW64 ~/devops/challange3 (master)
$  git log --oneline --graph --all --decorate --date-order
* d19ff4c (HEAD -> master) conflict resolved
* ffa1043 m3 Master: Add Line Y
* a8165e0 (feature) f3 Feature: Add Line D
* e541989 f2 Feature: Add Line C
* 894db87 b1 Feature: Add Line B
* f91879a m2 Master: Add Line X
* ac130f9 m1 Master: Add Line T
* bfb2dc8 a1 Base: Add Line A

```
---

## 🔀 **Merge Log (Branchy History)**

```
*   60ea1d4 (HEAD -> master) m5 Master: Merge feature into master
|\
| * a8165e0 (feature)       f3 Feature: Add Line D
* | 4178923                m4 Master: Add Line Z
| * e541989                f2 Feature: Add Line C
* | b5cb3aa                m3 Master: Add Line Y
| * 894db87                b1 Feature: Add Line B
|/
* f91879a                m2 Master: Add Line X
* ac130f9                m1 Master: Add Line T
* bfb2dc8                a1 Base: Add Line A
```

### 🧠 What It Shows:
- Git created a **merge commit (`m5`)** that combines both branches.
- The graph visually shows **parallel development**:
  - Left side → commits from `feature` branch
  - Right side → commits from `master`
- It's **real branch history**, not rewritten.
- Easy to **trace branch lines**, but **history is more complex**.

---

## 🔁 **Rebase Log (Linear History)**

```
* d19ff4c (HEAD -> master) conflict resolved
* ffa1043                m3 Master: Add Line Y
* a8165e0 (feature)       f3 Feature: Add Line D
* e541989                f2 Feature: Add Line C
* 894db87                b1 Feature: Add Line B
* f91879a                m2 Master: Add Line X
* ac130f9                m1 Master: Add Line T
* bfb2dc8                a1 Base: Add Line A
```

### 🧠 What It Shows:
- Feature commits (`b1`, `f2`, `f3`) were **replayed on top** of `master` via rebase.
- Then a **conflict was resolved** and committed (`d19ff4c`).
- The result is a **clean, linear history** — like the feature branch was developed on top of the latest `master` from the start.
- Looks like **everything happened in one straight line** — easier to read for small teams or clean project history.

---

## 🧩 Visual Difference: Summary Table

| Feature             | Merge                           | Rebase                          |
|---------------------|----------------------------------|---------------------------------|
| History Style       | Branching/Non-linear             | Linear                          |
| Merge Commit        | ✅ Yes (`m5`)                     | ❌ No (history rewritten)        |
| Conflict Handling   | During merge commit              | During rebase replay            |
| Commit Order        | Parallel (interleaved visually)  | Sequential (one timeline)       |
| Visual Graph        | Shows two lines merged           | Shows single straight path      |
| Use Case            | Preserve true history            | Clean up history before sharing |

---

# 💣 Challenge 4: Undo a Commit Using `git reset` & `git revert`

---

#### 🎯 Objective:
Understand how to undo commits using:
- `git reset` (`--soft`, `--mixed`, `--hard`)
- `git revert`

---

#### 1. **Initialize the Repo**

```bash
mkdir challenge4 && cd challenge4
git init
```

---

#### 2. **Create and Commit**

```bash
echo "line A" > file.txt
git add . && git commit -m "a1: Add line A"

echo "line B" >> file.txt
git add . && git commit -m "b1: Add line B"

echo "line C" >> file.txt
git add . && git commit -m "c1: Add line C"

echo "line D" >> file.txt
git add . && git commit -m "d1: Add line D"
```

---

#### 3. **Check the Commit History**

```bash
git log --oneline
```

```
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git log --oneline
b574b44 (HEAD -> master) d1: Add line D
dcf7632 c1: Add line C
276d87c b1: Add line B
e719ed0 a1: Add line A

```

---

#### 4. **Undo with `git reset --soft HEAD~1`**

```bash
git reset --soft HEAD~1
```

🧠 **What happened:**
- ✅ Commit `d1` removed from history
- 🟢 Changes from `d1` are still **staged**
- 📁 Working directory is untouched
💬 In simple terms:
> It just **removes the commit message**, and lets you **re-commit the same changes** with a new message.


```bash
git status
```

```
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   file.txt


```
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$  git log --oneline
dcf7632 (HEAD -> master) c1: Add line C
276d87c b1: Add line B
e719ed0 a1: Add line A

---

#### 5. **Undo with `git reset --mixed HEAD~1`**

```bash
git commit -m "d1: Add line D"
git reset --mixed HEAD~1
```

🧠 **What happened:**
- ✅ Commit `d1` removed
- 🔄 Changes moved to **unstaged**
- 📁 Working directory still contains the changes
💬 Easy version:  
The commit is removed, and your changes are still there — but **not staged anymore**.  
Now you need to **add and commit again** to stage them.

```bash
git status
```

```
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   file.txt

no changes added to commit (use "git add" and/or "git commit -a")

```

---

#### 6. **Undo with `git reset --hard HEAD~1`**

```bash
git add . && git commit -m "d1: Add line D"
git reset --hard HEAD~1
```
---

```bash
git status
```

```
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git status
On branch master
nothing to commit, working tree clean

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ cat file.txt
line A
line B
line C

```
🧠 **What happened:**
- 💣 Commit `d1` is erased
- 🔥 Changes to file are gone from disk
- 🧼 Clean working directory and staging area

> **Removes the commit, unstages the changes, and deletes the file content too.**

💬 Easy version:  
Just like `--mixed`, it **removes the commit** and **unstages the file** — but on top of that, it also **removes the changes from the file itself**.  
It’s like the commit and all its changes **never happened** — everything is moved back to the state of the previous commit.

🛑 Be super careful: You lose the commit **and** the code changes unless you recover using `git reflog`.

### ✅ **Bonus Tip: Recover with `git reflog`**

If you used `git reset --hard` and accidentally removed a commit or changes, **you can still recover it** using `git reflog`.


#### 🪜 Step-by-step Recovery:

##### 1. **Check the reflog (your recent Git actions)**
```bash
git reflog
```

This will show something like:
```
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git reflog
dcf7632 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
2085028 HEAD@{1}: commit: d1: Add line D
dcf7632 (HEAD -> master) HEAD@{2}: reset: moving to HEAD~1
2865bdd HEAD@{3}: commit: d1: Add line D
dcf7632 (HEAD -> master) HEAD@{4}: reset: moving to HEAD~1
b574b44 HEAD@{5}: commit: d1: Add line D
dcf7632 (HEAD -> master) HEAD@{6}: commit: c1: Add line C
276d87c HEAD@{7}: commit: b1: Add line B
e719ed0 HEAD@{8}: commit (initial): a1: Add line A

```

> 🔍 Look for the commit **before** the reset — usually labeled as `HEAD@{1}`, `HEAD@{2}`, etc.

---

##### 2. **Recover the lost commit**

Choose the commit you want to go back to:
```bash
git reset --hard <commit-hash>
```

🛠 Example:
```bash
git reset --hard 2085028
```
> ✅ This brings back the exact state of your project at that commit — including files and changes.

```bash
aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git reset --hard 2085028
HEAD is now at 2085028 d1: Add line D

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ cat file.txt
line A
line B
line C
line D

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git status
On branch master
nothing to commit, working tree clean

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git log --oneline
2085028 (HEAD -> master) d1: Add line D
dcf7632 c1: Add line C
276d87c b1: Add line B
e719ed0 a1: Add line A

```
agian moved 

🧠 **Pro Tip:**  
You can use `HEAD@{n}` directly too:
```bash
git reset --hard HEAD@{2}
```
💬 **Fun Note:**  
> With `reflog`, we went back… and then moved forward again — kinda cool, right? 😄  
> It’s like your personal time machine inside Git!

---

#### 7. **Undo with `git revert HEAD`**

```bash
git add . && git commit -m "d1: Add line D"
git revert HEAD
```
```bash

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git revert HEAD
[master 893a98b] Revert "d1: Add line D"
 1 file changed, 1 deletion(-)

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git status
On branch master
nothing to commit, working tree clean

aravi@Aravind MINGW64 ~/devops/challenge4 (master)
$ git log --oneline
893a98b (HEAD -> master) Revert "d1: Add line D"
2085028 d1: Add line D
dcf7632 c1: Add line C
276d87c b1: Add line B
e719ed0 a1: Add line A

```

🧠 **What happened:**
- 🆕 A new commit is created to undo `d1`
- ✅ No commits are removed
- 🔐 Safe for public/shared branches
- 
💬 **Easy version:**  
It works kinda like `--hard`:  
✅ It **removes the changes from the file**,  
✅ It **unstages the file**.

**BUT** (and this is important!) — it **does NOT delete or rewrite history**.  
Instead, it **adds a new commit** on top that says:

> “Hey, I’m reversing what the last guy did.” 😎

So the original commit is still there — **safe and untouched**!

💬 **Think of it like this:**

> 🔒 `git revert` is like a **police officer** —  
> It doesn’t erase the crime (commit), but it files a proper report to undo the damage — **clean, safe, and traceable**.

---
✅ **Use `revert` when:**
- You’ve already **pushed to GitHub**
- You're working with a **team**
- You want to **undo without rewriting history**
---

#### 8. **Summary Table**

| Command              | Removes Commit | Keeps Changes | Stages Changes | Safe to Share |
|----------------------|----------------|----------------|----------------|----------------|
| `reset --soft`       | ✅             | ✅              | ✅              | ❌             |
| `reset --mixed`      | ✅             | ✅              | ❌              | ❌             |
| `reset --hard`       | ✅             | ❌              | ❌              | ❌             |
| `revert`             | ❌             | ✅ (via new commit) | ✅         | ✅             |

---
#### 🎯 Summary (In Real Life Terms)

| Command           | Feels Like...                               |
|------------------|---------------------------------------------|
| `reset --soft`    | ✏️ Erasing a message but keeping the idea ready to rewrite |
| `reset --mixed`   | 📋 Erasing a message and putting the idea back on scratchpad |
| `reset --hard`    | 🧼 Wiping the board clean, like nothing happened |
| `revert`          | 👮 Writing a formal report to undo something without hiding it |

---



