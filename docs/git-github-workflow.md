# Git and GitHub Workflow

The standard basic workflow for Git and GitHub is:

```text
Working folder -> Staging area -> Commit -> GitHub
```

This is the normal way to work with Git locally and then publish your work to GitHub.

## 1. Working Folder

The working folder is the project folder on your computer.

This is where you create, edit, rename, or delete files. At this stage, Git can see that files have changed, but those changes have not been prepared for a commit yet.

Example:

```powershell
git status -s
```

If Git shows something like this:

```text
?? "docs/Social Media/"
```

it means Git sees a new folder, but it is not tracking it yet.

## 2. Staging Area

The staging area is where you choose which changes should be included in the next commit.

Staging does not save the version permanently yet. It only prepares the selected files.

Example:

```powershell
git add "docs/Social Media"
```

This tells Git:

```text
Include the files in this folder in the next commit.
```

Staging is useful because it lets you control exactly what goes into each commit. If you changed many files but only want to commit some of them, you stage only those files.

## 3. Commit

A commit is a saved snapshot of the staged changes.

Each commit has a message that explains what changed. This creates a version history for the project.

Example:

```powershell
git commit -m "Add social media article drafts"
```

This saves the staged files into the local Git history.

At this point, the commit exists on your computer, but it has not necessarily been uploaded to GitHub yet.

## 4. GitHub

GitHub is the remote copy of the repository online.

After committing locally, you push the commit to GitHub.

Example:

```powershell
git push
```

This uploads your local commit to the GitHub repository so it is backed up, visible online, and available to share.

## Full Example

```powershell
git status -s
git add "docs/Social Media"
git status -s
git commit -m "Add social media article drafts"
git push
```

## Why This Workflow Matters

This workflow gives you control and traceability:

- The working folder is where you make changes.
- The staging area is where you decide what belongs in the next version.
- The commit is the saved version.
- GitHub is where you publish and share the version.

This is one of the most common professional workflows for using Git and GitHub.
