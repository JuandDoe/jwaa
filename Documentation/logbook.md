# Project: Journal with an Attitude

## Step 0: Project setup

```bash
# Ensure we work with the latest stable release of the Rust programming language
rustup update
info: checking for self-update (current version: 1.29.0)

# Creating the project with Cargo, the go-to Rust package manager
# We use Cargo because it's a tool that allows Rust packages to declare their various dependencies and ensures that you'll always get a repeatable build
# https://doc.rust-lang.org/cargo/guide/why-cargo-exists.html

cargo new jwaa
```

From here I added:

- A `README.md` that briefly explains the project's purpose.
- A `Documentation` folder that contains this `logbook.md` you are currently reading. It will allow me to keep track of my project.

## Step 0.1: Putting the project on GitHub

- I always put projects on GitHub through VSCode's integrated GUI. I want to try doing it entirely through the CLI.

```bash
# Checking if Git is properly installed
git --version
git version 2.43.0

# Browse to the GitHub documentation
# https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github

git init -b main

warning: re-init: ignored --initial-branch=main
Reinitialized existing Git repository in /home/agschwind/jwaa/.git/

# Makes sense: "cargo new jwaa" already created a Git repository.

# Adds the files in the local repository and stages them for commit
git add .
```
I was about to commit...
```bash
# Commits the tracked changes and prepares them to be pushed to a remote repository
git commit -m "First commit"
```

But the fact I want to keep my commits consistent sparked me , so I will try to follow a clear commit syntax:
https://www.conventionalcommits.org/en/v1.0.0/#specification

Nothing is specified for the initial commit name. I discussed it with Claude and went with:

```bash
chore: initial commit

# Commits the tracked changes and prepares them to be pushed to a remote repository
git commit -m "chore: initial commit"

# Success
[master (root-commit) 85e79e6] chore: initial commit
 6 files changed, 60 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 Cargo.lock
 create mode 100644 Cargo.toml
 create mode 100644 Documentation/logbook.md
 create mode 100644 readme.md
 create mode 100644 src/main.rs

# Install GitHub CLI
sudo apt install gh

gh repo create
To get started with GitHub CLI, please run: gh auth login
Alternatively, populate the GH_TOKEN environment variable with a GitHub API authentication token.
agschwind@AOG25P004:~/jwaa$

# Login
gh auth login
```

This is very weird. First time I've seen this. I swear I didn't smoke anything. My Google 2FA code for GitHub is in the `(3) XXX XXX` format, and GitHub asks me for a `(4) XXXX XXXX` format.

- My Google Authenticator app is up-to-date.
- Re-scanned with my 2FA app: https://github.com/settings/security?type=app#two-factor-summary. It still gives me the `XXX XXX` code format.
- Searched a bit... found it: https://www.reddit.com/r/github/comments/127zzfn/visual_studio_is_asking_for_8_number_github_2fa/

- Tried again after a good full cheese pizza.

```bash
# I misread it; it was nothing about my GitHub 2FA.
First copy your one-time code: 1234-1234

✓ Authentication complete.
- gh config set -h github.com git_protocol https
✓ Configured git protocol
! Authentication credentials saved in plain text
✓ Logged in as JuandDoe

# Tried to push to Git

git remote add origin git@github.com:/JuandDoe/jwaa.git
agschwind@AOG25P004:~/jwaa$ git push -u origin master

The authenticity of host 'github.com (140.82.121.4)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.
```

- Saw nothing on my GitHub. Found `gh` CLI on the same page.

```bash
gh repo create
? What would you like to do? Push an existing local repository to GitHub
? Path to local repository .
? Repository name jwaa
? Description Journal with an Attitude
? Visibility Public
✓ Created repository JuandDoe/jwaa on GitHub
  https://github.com/JuandDoe/jwaa
? Add a remote? Yes
? What should the new remote be called? origin
X Unable to add remote "origin"
```

- It initialized an empty GitHub repository named `jwaa`, with no files.

- Okay, I'm probably mixing Git and GitHub at this point. We have to make it clearer.

```bash
git remote add origin https://github.com/JuandDoe/jwaa
error: remote origin already exists.

agschwind@AOG25P004:~/jwaa$ git remote -v
origin  git@github.com:/JuandDoe/jwaa.git (fetch)
origin  git@github.com:/JuandDoe/jwaa.git (push)

agschwind@AOG25P004:~/jwaa$ git push origin main
git@github.com: Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

- Seems like a problem with authorization or permission scope.

- Well, I'm Googling the error and found an interesting blog post:
https://oneuptime.com/blog/post/2026-01-24-git-could-not-read-remote/view

```bash
# Perform first diagnostic
git remote -v

origin  git@github.com:/JuandDoe/jwaa.git (fetch)
origin  git@github.com:/JuandDoe/jwaa.git (push)
```

- But my page is https://github.com/JuandDoe/jwaa

```bash
# Success
git remote set-url origin https://github.com/JuandDoe/jwaa

agschwind@AOG25P004:~/jwaa$ git push origin main

Enumerating objects: 10, done.
Counting objects: 100% (10/10), done.
Delta compression using up to 22 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (10/10), 1.83 KiB | 1.83 MiB/s, done.
Total 10 (delta 0), reused 0 (delta 0), pack-reused 0

To https://github.com/JuandDoe/jwaa
 * [new branch]      main -> main
```

- Since I do lot typo I added LTEX Extension for VS Code, a Grammar/Spell Checker Using LanguageTool which will speed up the English correction process of both code and documentation

- I added Markdown Preview Enhanced extension to check if I didn't have any broken Markdown syntax