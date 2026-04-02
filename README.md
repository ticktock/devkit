# DevKit: Mac Setup for Development with Claude Code

A straightforward guide to getting your Mac ready for development with Claude Code.

## Prerequisites

- A Mac running macOS Sonoma (14) or later
- Admin access to your machine
- A working internet connection

## Step 0: Open Terminal

Every step in this guide happens in the terminal — a text-based interface for running commands on your Mac. You already have one installed.

To open it:

1. Press `Cmd + Space` to open Spotlight Search
2. Type **Terminal**
3. Hit **Enter**

You'll see a window with a blinking cursor — that's where you'll paste the commands from this guide. You can also find Terminal in **Applications > Utilities > Terminal**.

> After Step 2 you'll have Ghostty, a nicer terminal app. You can switch to that once it's installed.

## Step 1: Install Homebrew

Homebrew is a package manager for macOS — it lets you install software from the terminal.

Paste this into your terminal and hit Enter:

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the on-screen prompts. When it finishes, **read the output carefully** — Homebrew will tell you to run a couple of commands to add it to your PATH. Run those commands.

Verify it worked:

```sh
brew --version
```

## Step 2: Install Apps with Homebrew

### Ghostty (Terminal Emulator)

Ghostty is a fast, modern terminal that you'll use instead of the built-in Terminal app.

```sh
brew install --cask ghostty
```

After installing, open Ghostty from your Applications folder. You can use it for the rest of this guide.

### Docker Desktop

Docker lets you run containers — isolated environments for databases, services, and tools.

```sh
brew install --cask docker
```

After installing, open **Docker Desktop** from your Applications folder and complete the setup wizard. Docker needs to be running for container-based workflows to work.

### GitHub CLI

The GitHub CLI (`gh`) lets you work with GitHub from the terminal — cloning repos, creating PRs, and more.

```sh
brew install gh
```

## Step 3: Install Claude Code

Claude Code is Anthropic's CLI tool for working with Claude directly in your terminal.

```sh
curl -fsSL https://claude.ai/install.sh | bash
```

This installs the `claude` command and automatically keeps it up to date in the background.

Verify it worked:

```sh
claude --version
```

### Optional: Claude Code Desktop App

If you prefer a standalone app over the terminal, Claude Code also has a desktop app with a visual interface for reviewing diffs, running multiple sessions side by side, and scheduling recurring tasks.

Download it here:

- [macOS (Intel and Apple Silicon)](https://claude.ai/api/desktop/darwin/universal/dmg/latest/redirect)

Open the `.dmg`, drag Claude to your Applications folder, and launch it. Sign in with your Anthropic account (see Step 4 below) and click the **Code** tab to start coding.

> The desktop app and the terminal CLI share the same settings, memory, and configuration — you can use both interchangeably.

## Step 4: Create Accounts

You'll need accounts on three services. Sign up for each one:

### GitHub

1. Go to [github.com](https://github.com) and create an account
2. Back in your terminal, log in with the GitHub CLI:

   ```sh
   gh auth login
   ```

3. It will ask you a series of questions. Choose these options:
   - **Where do you use GitHub?** → `GitHub.com`
   - **What is your preferred protocol for Git operations?** → `HTTPS`
   - **How would you like to authenticate GitHub CLI?** → `Login with a web browser`
4. It will give you a one-time code and open your browser. Paste the code, authorize the app, and you're done.

Verify it worked:

```sh
gh auth status
```

You should see "Logged in to github.com".

### Docker Hub

1. Go to [hub.docker.com](https://hub.docker.com) and create an account
2. Open Docker Desktop and sign in with your new account

### Anthropic (Claude)

1. Go to [console.anthropic.com](https://console.anthropic.com) and create an account
2. Launch Claude Code for the first time:

   ```sh
   claude
   ```

3. Claude Code will open your browser and ask you to log in to your Anthropic account
4. Once you authorize it, the terminal will confirm you're authenticated and drop you into an interactive session
5. Type `/exit` to leave the session for now

> **What just happened?** Claude Code stored an auth token on your machine so you won't need to log in again. If you ever need to re-authenticate, run `claude` and it will prompt you.

## What's Next

Your Mac is set up. Here's how to actually start building things.

### Start a Project

The typical way to start a new project is to create a GitHub repo and clone it to your machine:

1. Go to [github.com/new](https://github.com/new) and create a new repository (check "Add a README file" to make it non-empty)
2. Clone it to your machine:

   ```sh
   gh repo clone your-username/your-repo-name
   cd your-repo-name
   ```

3. Start Claude Code:

   ```sh
   claude
   ```

You're now in an interactive session where you can talk to Claude about your project.

### Working with Claude Code

Claude Code works best when you tell it what you want in plain language. Here are some things you can ask it to do:

**Build something:**

```
create a simple web app that shows a todo list
```

**Work on a branch** (good practice — keeps your main branch clean):

```
create a new branch called add-login-page, then build a login page
```

**Commit and push your work:**

```
commit my changes with a descriptive message and push to github
```

**Open a pull request** (how you propose changes for review):

```
create a pull request for this branch
```

**Fix something:**

```
the app crashes when I click submit — here's the error: <paste error>
```

You don't need to memorize commands. Just describe what you want and Claude will figure out the right git, shell, and code changes to make.

### Docker Basics

Make sure Docker Desktop is running (open it from your Applications folder). You can verify it's working:

```sh
docker info
```

Log in to Docker Hub from the terminal:

```sh
docker login
```

Enter the username and password from the Docker Hub account you created earlier.

**Run a database** — this is one of the most common things you'll use Docker for. For example, to start a PostgreSQL database:

```sh
docker run --name my-postgres -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
```

This runs Postgres in the background, accessible on `localhost:5432`. To stop it:

```sh
docker stop my-postgres
```

To start it again later:

```sh
docker start my-postgres
```

**Docker Compose** — when your project needs multiple services (a database, a web server, a cache, etc.), Docker Compose lets you define them all in a `docker-compose.yml` file and start everything with one command:

```sh
docker compose up
```

You can ask Claude Code to help you write a `docker-compose.yml` for your project.

### Further Learning

- [Claude Code documentation](https://code.claude.com/docs/en/overview) — the full guide to everything Claude Code can do
- [GitHub quickstart](https://docs.github.com/en/get-started/quickstart) — learn the basics of git and GitHub
- [Docker getting started](https://docs.docker.com/get-started/) — containers, images, and Compose
- [Homebrew documentation](https://docs.brew.sh) — installing and managing software on macOS
- [Ghostty documentation](https://ghostty.org/docs) — configuring your terminal
