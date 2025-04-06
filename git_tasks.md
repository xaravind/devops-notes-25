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
Love the enthusiasm! Let’s dive into **Challenge 3**, building off that same style and energy 💥

---

### 🧨 **Challenge 3: Create a Merge Conflict Scenario & Resolve It with `git merge` and `git rebase`**

---

#### 📌 Objective:
1. Create a repo with a main branch.
2. Create a second branch with diverging commits (make multiple switches).
3. Make conflicting changes on both branches.
4. First use `git merge` to resolve the conflict.
5. Then reset and try the same using `git rebase`.
6. Use `git log --graph` to visualize the difference.

---

### ✅ **Step-by-Step Instructions**

---

#### 1. **Set Up the Playground**
```bash
mkdir challange3 && cd challange3
git init
```

---

#### 2. **Create Base Commit on `main`**
```bash
echo "Line A" > file.txt
git add file.txt
git commit -m "Initial commit on main"
```

---

#### 3. **Create and Switch to `feature` Branch**
```bash
git checkout -b feature
```

---

#### 4. **Add Multiple Commits on `feature`**
```bash
echo "Line B" >> file.txt
git commit -am "Feature: Add Line B"

echo "Line C" >> file.txt
git commit -am "Feature: Add Line C"

echo "Line D" >> file.txt
git commit -am "Feature: Add Line D"
```
> 🧠 We're stacking up commits on the feature branch — this will make rebase more interesting!

---

#### 5. **Switch Back to `main` and Add Diverging Commits**
```bash
git checkout main
```

```bash
echo "Line X" >> file.txt
git commit -am "Main: Add Line X"

echo "Line Y" >> file.txt
git commit -am "Main: Add Line Y"
```
> ⚔️ Now both branches have changed the same file. Conflict guaranteed!

---

### 🔀 PART 1: **MERGE and Resolve Conflict**

#### 6. **Try Merging `feature` into `main`**
```bash
git merge feature
```

> 😬 You'll get a conflict:
```
Auto-merging file.txt
CONFLICT (content): Merge conflict in file.txt
```

---

#### 7. **Resolve the Conflict in `file.txt`**
Open `file.txt` and manually edit it. You'll see:
```
Line A
<<<<<<< HEAD
Line X
Line Y
=======
Line B
Line C
Line D
>>>>>>> feature
```

🛠 Change it to a clean version, e.g.:
```
Line A
Line X
Line Y
Line B
Line C
Line D
```

Then:
```bash
git add file.txt
git commit -m "Merge feature into main with conflict resolution"
```

---

#### 8. **View Log Graph**
```bash
git log --oneline --graph --all
```

> 🧠 You’ll see a **merge commit** showing both histories.

---

### 🔁 PART 2: **REBASE and Resolve Conflict**

#### 9. **Reset to Before Merge**
```bash
git reset --hard HEAD~3
git checkout feature
```

#### 10. **Rebase `feature` onto `main`**
```bash
git rebase main
```

> 😬 Conflict again!

---

#### 11. **Resolve the Conflict**
Edit `file.txt` the same way as before:
```
Line A
Line X
Line Y
Line B
Line C
Line D
```

Then:
```bash
git add file.txt
git rebase --continue
```

Repeat if there are more conflicts (you may need to resolve for each commit).

---

#### 12. **Rebase Done! Fast-Forward Merge**
Now rebase is clean. Switch to `main`:
```bash
git checkout main
git merge feature --ff-only
```

---

#### 13. **Final Git Graph**
```bash
git log --oneline --graph --all
```

> 🎯 You’ll see a **linear history** now — no merge commit!

---

### 🧠 Summary: Merge vs Rebase

| Action     | Result in Git Log               | Conflict Handling         |
|------------|----------------------------------|---------------------------|
| `merge`    | Creates a merge commit 🧩        | One-time conflict         |
| `rebase`   | Linear history 🧵                | Multiple conflict resolutions (one per commit) |

---

Want to expand this next by simulating collaboration with a remote (`origin`) and pushing with `--force-with-lease` after a rebase?
