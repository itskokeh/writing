<h1>Beginner’s Guide to Git, GitHub, and Visual Studio Code Setup For
Windows</h1>

- [Introduction](#introduction)
- [1. Installing Git](#1-installing-git)
  - [Step 1: Download Git](#step-1-download-git)
  - [Step 2: Run the Installer](#step-2-run-the-installer)
  - [Step 3: Follow the Installation Wizard](#step-3-follow-the-installation-wizard)
- [2. Open a GitHub Account](#2-open-a-github-account)
  - [Steps to Create a GitHub Account](#steps-to-create-a-github-account)
- [3. Installing Visual Studio Code](#3-installing-visual-studio-code)
  - [Step 1: Download VS Code](#step-1-download-vs-code)
  - [Step 2: Install VS Code](#step-2-install-vs-code)
  - [Step 3: Launch VS Code](#step-3-launch-vs-code)
  - [Step 4: Configure VS Code for Git](#step-4-configure-vs-code-for-git)
  - [Step 5: Install Recommended Extensions](#step-5-install-recommended-extensions)

## Introduction

Have you ever wanted to keep track of changes to your files or write and test
code easily? Two powerful tools, **Git** and **Visual Studio Code (VS Code)**,
can help you do exactly that—and more! Whether you're working on a personal
project, collaborating with others, or just exploring the world of coding,
these tools are essential for getting started.

Think of **Git** as a "time machine" for your files. It allows you to save
snapshots of your work, so you can always go back to a previous version if
something goes wrong. It’s especially useful when working on team projects, as
it helps everyone stay organized and avoid conflicts.

**Visual Studio Code (VS Code)**, on the other hand, is like a supercharged text
editor. It’s designed to make writing and editing code easier, with features
like syntax highlighting, auto-completion, and built-in tools for debugging.
Whether you're writing a simple script or building a complex application, VS
Code provides a comfortable and efficient workspace.

Together, Git and VS Code are a dynamic duo for anyone working with code. This
guide will walk you through the simple steps to install both tools on your
computer. No prior experience is needed—just follow along, and you’ll be ready
to start using these tools in no time!

## 1. Installing Git

In this section, we’ll walk through the process of installing Git on a Windows
computer. Don’t worry—it’s straightforward, and I’ll guide you through every
step.

### Step 1: Download Git

1. Open your web browser and go to the official Git website:
   [https://git-scm.com](https://git-scm.com/).

2. Click the **Download for Windows** button. The website will automatically
   detect your operating system and provide the correct version.

   ![Git Website Homepage](./assets/git-images/git-website-homepage.png)  
   _Git's Homepage_

### Step 2: Run the Installer

1. Once the download is complete, locate the installer file (usually in your
   `Downloads` folder) and double-click it to run the installer.

2. A security warning may pop up. Click **Yes** to allow the installer to make
   changes to your device.

### Step 3: Follow the Installation Wizard

1. The Git Setup Wizard will open. Click **Next** to begin the installation.

   ![Git Setup Wizard](./assets/git-images/git-setup-wizard.png)  
   _Git Setup Wizard_

2. **Select Components**: On the next screen, you’ll see options for additional
   components. The default selections are fine for most users, but you can
   customize them if you’d like. Click **Next** to continue.

   ![Git Components](./assets/git-images/git-components.png)

3. **Choose the Default Editor**: Git needs a text editor for certain tasks. By
   default, it suggests **Vim**, but if you’re not familiar with Vim, you can
   select **Notepad** or another editor you’re comfortable with but I recommend
   **Visual Studio Code** because I would show how to use it in this guide.
   Click **Next**.

   ![Git Default Editor](./assets/git-images/git-default-editor.png)  
    _Git Default Editor_

4. **Choose Initial Branch Name**: During the old times, the default was
   **master** but in recent times **main** has been the preferred option. So,
   select **Override default branch name for new repositories** and input
   **main** as the default. Click **Next**.

   ![Choose Initial Branch](./assets/git-images/git-initial-branch.png)

5. **Adjust Your PATH Environment**: This step determines how Git is accessed
   from the command line. Select the recommended option:
   **Git from the command line and also from 3rd-party software**. This ensures
   Git works seamlessly with tools like VS Code. Click **Next**.

   ![Git Path Environment](./assets/git-images/git-path-env.png)

6. **Choose SSH Executable**: Select **Use Bundled OpenSSH**. Click **Next**.

   ![Git SSH Executable](./assets/git-images/git-ssh-executable.png)

7. **Choose HTTPS Transport Backend**: Leave this setting at its default
   (**Use the OpenSSL library**) and click **Next**.

   ![Git HTTPS Transport Backend](./assets/git-images/git-transport-backend.png)

8. **Configure Line Ending Conversions**: This setting ensures compatibility
   between Windows and other operating systems. Select the default option:
   **Checkout Windows-style, commit Unix-style line endings**. Click **Next**.

   ![Git Line Ending](./assets/git-images/git-line-endings.png)

9. **Choose Terminal Emulator**: Select
   **Use MinTTY (the default terminal of MSYS2)** for a better terminal
   experience. Click **Next**.

   ![Git Terminal Emulator](./assets/git-images/git-terminal-emulator.png)

10. **Choose Git Pull Default Behavior**: Several options to pick from here but
   for beginners and non-technical people, picking **Fast Forward or Merge** is
   the best option. Click **Next**.

      ![Git Pull Behavior](./assets/git-images/git-pull-default-behaviour.png)

11. **Choose Credential Helper**: Select **Git Credential Manager**. Click
    **Next**.

      ![Git Credential Helper](./assets/git-images/git-credential-helper.png)

12. **Configure Extra Options**: Leave the default options selected and click
    **Install**.

      ![Git Extra Options](./assets/git-images/git-extra-options.png)

The installer will now install Git on your computer. This may take a few
moments.

Congratulations! You’ve successfully installed Git on your Windows computer.
In the next section, we’ll install Visual Studio Code and set it up to work
seamlessly with Git.

## 2. Open a GitHub Account

Before we move on to installing Visual Studio Code, it’s a good idea to set up
a **GitHub account**. GitHub is a platform that works seamlessly with Git,
allowing you to store your code online, collaborate with others, and contribute
to projects.

### Steps to Create a GitHub Account

1. Go to [https://github.com](https://github.com) and click **Sign up**.

2. Enter a username, your email address, and a secure password.

3. Verify your email address by clicking the link in the confirmation email sent
   by GitHub.

Now that your GitHub account is set up, let’s install **Visual Studio Code**
and get everything connected.

## 3. Installing Visual Studio Code

Visual Studio Code (VS Code) is a powerful, free code editor that works on
Windows, macOS, and Linux. In this section, we’ll walk you through installing
VS Code and setting it up for use with Git and GitHub.

### Step 1: Download VS Code

1. Open your web browser and go to the official VS Code website:
   [https://code.visualstudio.com/](https://code.visualstudio.com/).

2. Click the **Download** button for **Windows**.

   ![vscode homepage](./assets/vscode-images/vscode-website-homepage.png)

### Step 2: Install VS Code

1. Locate the downloaded installer (usually in your `Downloads` folder) and
   double-click it to run.

2. Follow the installation wizard:
   - Accept the license agreement.
   - Choose the default installation location.
   - Select **Add to PATH** (this makes it easier to open VS Code from the
     command line).

   ![vsc additional tasks](./assets/vscode-images/vscode-additional-task.png)

3. Click **Install** and wait for the installation to complete.

   ![vsc installation daemon](./assets/vscode-images/vscode-install-daemon.png)

### Step 3: Launch VS Code

1. Once installed, open VS Code from your applications menu

2. Take a moment to explore the interface:
   - **Sidebar**: Access files, extensions, and source control.
   - **Editor**: Write and edit your code.
   - **Terminal**: Run commands directly within VS Code.

### Step 4: Configure VS Code for Git

1. Open the **Accounts** tab in the sidebar and click **Turn On Cloud Changes**.

   ![vsc account cloud changes](./assets/vscode-images/vscode-cloud-changes.png)  
   _Turn On Cloud Changes on Account_

   ![vsc git account signin](./assets/vscode-images/vscode-signin.png)  
   _Sign in with GitHub_

   ![vsc authorize](./assets/vscode-images/vscode-git-authorize.png)  
   _Authorize the sign in request_

2. Now, click on **Source Control** tab and select **Clone Repository**. A
   **Clone from GitHub** popup will appear. Click on it and you will be taken to
   your browser to authorize the request for cloning a repository. Once
   authorized, you will see all your repositories on GitHub in your editor for
   you to select one. And you have successfully linked your GitHub account to
   VS Code. You can now use VS Code to commit, push, and pull changes to your
   Git repositories.

   ![vsc source control](./assets/vscode-images/vsc-source-control.png)  
   _Source Control tab and Clone Repository_

   ![vsc clone from github](./assets/vscode-images/vsc-clone-repository.png)  
   _Clone from GitHub_

   ![vsc display repos](./assets/vscode-images/vscode-display-repos.png)  
   _Display of repositories from GitHub_

### Step 5: Install Recommended Extensions

VS Code’s functionality can be extended with plugins. Here are a few essential
extensions for Git and general development or writing:

1. **GitLens**: Supercharges Git integration, providing detailed history and
   blame information for your code.
   - To install: Go to the Extensions view (`Ctrl+Shift+X`),
     search for **GitLens**, and click **Install**.

2. **Live Preview**: Launches a local development server with live reload for
   web development or a preview screen for your `.md` files.
   - Search for **Live Preview** and install it.

Now that you’ve installed VS Code and set it up for Git, you’re ready to start
writing code! In the next section, we’ll explore how to create your first
project and connect it to GitHub.

---

Congratulations! You’ve successfully installed Git, set up a GitHub account,
and installed Visual Studio Code.
<!-- ## Read my other posts

- [Essential Git Commands Every Beginner Should Know](https://kokeh.dev.blog)

- [Git for Non-Coders:
  Mastering Version Control with GUI Tools](https://kokeh.dev/blog)

- [Mastering Version Control:
  A Beginner’s Guide to Git and GitHub](https://kokeh.dev/blog) -->
