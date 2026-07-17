# Project: Journal with an Attitude

Step 0 : Project set-up

```bash
# Ensure we works with the last stable release of the rust programming language
rustup update
info: checking for self-update (current version: 1.29.0)

#Creating the project with Cargo, the go to Rust package Manager
# We use Cargo cause its a tool that allows Rust packages to declare their various dependencies and ensure that you’ll always get a repeatable build
# https://doc.rust-lang.org/cargo/guide/why-cargo-exists.html

cargo new jwaa
```
From Here I ad : 

- A readme.md who briefly explain the project purposes
- A Documentation folder who contais this logbook.md you are actually reading. It will allow me to keep track of my project

Step 0.1 : Puting the project on github

- I akways put project on github throught VSCode integrated GUI, I want to try the full CLI way

```bash
# Checking if git is properly installed
git --version
git version 2.43.0

# Browse to github Doc 
# https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github

git init -b main

warning: re-init: ignored --initial-branch=main
Reinitialized existing Git repository in /home/agschwind/jwaa/.git/

# Makes sense " cargo new already created a git repository"
```

