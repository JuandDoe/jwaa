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

## Step 0.2: Set up of git pre commits & watchdogs

- Before going any further into the chore project I would like to ad some watchdogs to avoid pushing mistakes on GitHub (Secrets, typos..etc), will read a bit about it

- Found a gitignore generator for rust project : [text](https://www.git-tower.com/free-tools/gitignore/rust)
- I will use their template for now. I didn't really found an article, blog or doc about what would be the prefect gitignore for rust so i make the choice that this company no better than me for now

- [text](https://github.com/j178/prek)
Prek seems very promising, a faster version of pre-commit framework, built in commons hook, no dependencies needed (single binary)

- I install
```bash
cargo install --locked prek
```

- Added prek.toml at the root of project. This file will contain our hook

- Let's allow hooks to run every time I commit, with prek’s Git shim integration
```bash
prek install
```
- I added some built I understood and found useful in hook in my prek.toml
- Regarding the protection against secret leaks especially I wanted to firstly go for Gitleaks, but the author considers it feature complete and shifted recently to Betterleaks to build the future of secret scanning. As i'm in a new project I will go for Betterleaks

- Found usage example : [
](https://github.com/betterleaks/betterleaks/blob/main/.pre-commit-hooks.yaml)

- I chose the Golang-based hook instead of the system or Docker variants because it allows prek to manage Betterleaks installation in an isolated environment. This avoids requiring contributors to manually install Betterleaks or maintain a local version, reducing onboarding friction when sharing the project.

I pinned the rev field (syntax from pre-commit, used by prek also) to the latest release tag instead of relying on a floating tag such as latest, ensuring deterministic and reproducible hook execution across environments. I will dig into the config when it will need by the use of a secret


- Let's check before commit

```bash
- prek run --all-files
trim trailing whitespace.................................................Passed
check for case conflicts.................................................Passed
check illegal windows names..........................(no files to check)Skipped
fix end of files.........................................................Failed
- hook id: end-of-file-fixer
- exit code: 1
- files were modified by this hook

  Fixing .gitignore
  Fixing Documentation/logbook.md
  Fixing readme.md
fix utf-8 byte order marker..............................................Passed
check json...........................................(no files to check)Skipped
check toml...............................................................Passed
check vcs permalinks.....................................................Passed
check yaml...........................................(no files to check)Skipped
check for broken symlinks............................(no files to check)Skipped
detect private key.......................................................Passed
Detect hardcoded secrets.................................................Passed
ant@127:~/bullshit/jwaa$ prek run --all-files
trim trailing whitespace.................................................Passed
check for case conflicts.................................................Passed
check illegal windows names..........................(no files to check)Skipped
fix end of files.........................................................Passed
fix utf-8 byte order marker..............................................Passed
check json...........................................(no files to check)Skipped
check toml...............................................................Passed
check vcs permalinks.....................................................Passed
check yaml...........................................(no files to check)Skipped
check for broken symlinks............................(no files to check)Skipped
detect private key.......................................................Passed
Detect hardcoded secrets.................................................Passed
```
## Step 1 — The Recorder (make Rust hear you)

- We will firstly go for fixed duration. I will follow advices : [`cpal`](https://crates.io/crates/cpal) crate for microphone capture and [`hound`](https://crates.io/crates/hound) to write the WAV file.

```bash
cargo add cpal
cargo add hound
```

- Project build and run

- I found a recording file example with cpal and Hound
[text](https://github.com/RustAudio/cpal/blob/0ecb418b7b74da871984287e9f0c5a41900c8d52/examples/record_wav.rs)

- I had firstly a fear here : when you have a problem to solve with a lib you go to examples and docs, but when its about 2 docs, and you don't see clearly how to split it down into distinct pieces I feel easily myself sinking. I ctrl+f hound here and found. Lucky. Claude explained that usually a low level library bring upper level lib in their examples as the low lib itself didn't have meaningful use case by itself most of the time or at least, if you want a nice overview of its ability you should include what upper lib leveraging include in your examples

- Claude gave a meaningful example here when I asked how to proceed if lib A or Lib B doesn't give examples including each other :
   When no obvious example links the two crates: GitHub code search with both names (cpal hound language:Rust)"

   - I added needed crates and features needed by reading the toml file of the repo then replaced the example duration (3 seconds) to 60 seconds
   - Software compile. I get a clear wav audio file, the recording stop automatically when file duration reach 60 seconds
   - I can read the file and heard the record as needed


```bash
fix end of files.........................................................Failed
- hook id: end-of-file-fixer
- exit code: 1
- files were modified by this hook
```
- Weird ! At least it had I thought. My idea was "Okay after first fail, an automatic modification/fix was applied. It was ! But I didn't see first that i had to re-stage the file in my IDE.
- Commit & Push successfully now
