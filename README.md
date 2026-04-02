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
npm install -g @anthropic-ai/claude-code
```

> **Don't have npm?** Install Node.js first:
>
> ```sh
> brew install node
> ```
>
> Then re-run the Claude Code install command above.

Verify it worked:

```sh
claude --version
```

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

## You're Done

Your Mac is now set up for development with Claude Code. To start working in a project:

```sh
cd /path/to/your/project
claude
```

Claude Code will help you from there.
