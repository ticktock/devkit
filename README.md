# DevKit: Mac Setup for Development with Claude Code

A straightforward guide to getting your Mac ready for development with Claude Code.

## Prerequisites

- A Mac running macOS Sonoma (14) or later
- Admin access to your machine
- A working internet connection

## Step 1: Create Your Accounts

Before installing anything, sign up for the three services you'll use. This is all browser work — no terminal needed yet.

### GitHub

Go to [github.com](https://github.com) and create an account. GitHub is where your code lives and where you collaborate with others.

> **Stuck somewhere in this guide?** Once you have a GitHub account, you can [open an issue on this repo](https://github.com/ticktock/devkit/issues/new) describing where you got stuck, and someone will help.

### Docker Hub

Go to [hub.docker.com](https://hub.docker.com) and create an account. Docker Hub is where you'll pull pre-built container images (like databases) from.

### Anthropic (Claude)

Go to [console.anthropic.com](https://console.anthropic.com) and create an account. This is the account you'll use to sign in to Claude Code.

---

Keep the usernames and passwords for these three accounts handy — you'll need them in Step 6.

## Step 2: Open Terminal

Every step from here on happens in the terminal — a text-based interface for running commands on your Mac. You already have one installed.

To open it:

1. Press `Cmd + Space` to open Spotlight Search
2. Type **Terminal**
3. Hit **Enter**

You'll see a window with a blinking cursor — that's where you'll paste the commands from this guide. You can also find Terminal in **Applications > Utilities > Terminal**.

> After Step 4 you'll have Ghostty, a nicer terminal app. You can switch to that once it's installed.

## Step 3: Install Homebrew

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

## Step 4: Install Apps with Homebrew

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

## Step 5: Install Claude Code

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

Open the `.dmg`, drag Claude to your Applications folder, and launch it. Sign in with your Anthropic account and click the **Code** tab to start coding.

> The desktop app and the terminal CLI share the same settings, memory, and configuration — you can use both interchangeably.

## Step 6: Sign In

Now connect each CLI tool to the accounts you created in Step 1.

### GitHub CLI

Log in with the GitHub CLI:

```sh
gh auth login
```

It will ask you a series of questions. Choose these options:

- **Where do you use GitHub?** → `GitHub.com`
- **What is your preferred protocol for Git operations?** → `HTTPS`
- **How would you like to authenticate GitHub CLI?** → `Login with a web browser`

It will give you a one-time code and open your browser. Paste the code, authorize the app, and you're done.

Verify it worked:

```sh
gh auth status
```

You should see "Logged in to github.com".

### Docker Hub

Open Docker Desktop and sign in with your Docker Hub account. You can also log in from the terminal:

```sh
docker login
```

### Anthropic (Claude)

Launch Claude Code for the first time:

```sh
claude
```

It will open your browser and ask you to log in to your Anthropic account. Once you authorize it, the terminal will confirm you're authenticated and drop you into an interactive session. Type `/exit` to leave the session for now.

> **What just happened?** Claude Code stored an auth token on your machine so you won't need to log in again. If you ever need to re-authenticate, run `claude` and it will prompt you.

## Step 7: Run Claude Code Safely with Docker Sandboxes (Recommended)

Claude Code is powerful — it can read, write, and delete files, run shell commands, install packages, and modify your system. Most of the time that's exactly what you want. But **if you aren't sure you exactly understand what you're trying to do, do it in a sandbox.** That way, if Claude (or a prompt you paste in) does something unexpected, your actual Mac stays untouched.

**Docker Sandboxes** solve this. They run Claude Code inside an isolated microVM — a disposable copy of a dev environment. Claude can do whatever it wants inside the sandbox, and your real files, settings, and system stay untouched.

> **Requirements:** Apple Silicon Mac (M1/M2/M3/M4). Intel Macs are not supported.

### Install the Sandbox CLI

```sh
brew install docker/tap/sbx
```

### Log In and Choose a Network Policy

```sh
sbx login
```

This opens your browser to authenticate with Docker. On first login, you'll be asked to pick a default **network policy** — this controls what the sandbox is allowed to connect to on the internet:

- **Open** — all network traffic allowed (least safe)
- **Balanced** — default deny, but common dev sites (GitHub, npm, PyPI, etc.) are allowed **(recommended)**
- **Locked Down** — everything blocked unless you explicitly allow it

Pick **Balanced** to start. You can change it later.

### Run Claude Code in a Sandbox

From inside a project directory:

```sh
cd your-repo-name
sbx run claude
```

The first run takes a minute to download the sandbox image. Subsequent runs start in seconds.

You'll land in a familiar Claude Code session — same prompt, same capabilities — except now everything Claude does happens inside the sandbox. When you're done and exit, the sandbox is thrown away.

> **Heads up:** Inside a sandbox, Claude Code runs with `--dangerously-skip-permissions` by default. That flag is *only* safe because the sandbox is throwaway and isolated — never use it outside a sandbox.

### When to use a sandbox vs. running Claude directly

- **Use a sandbox** when trying unfamiliar prompts, experimenting, running agents unattended, or any time you're not 100% sure what Claude is about to do. If in doubt — sandbox it.
- **Run Claude directly** (just `claude`) when you want it to actually modify files in your real project and you understand the changes you're asking for.

Learn more in the [Docker Sandboxes docs](https://docs.docker.com/ai/sandboxes/).

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

3. Start Claude Code (using a sandbox is recommended — see Step 7):

   ```sh
   sbx run claude
   ```

   Or, if you'd rather run Claude directly against your real filesystem:

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
