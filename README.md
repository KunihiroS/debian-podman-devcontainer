# Debian-based Dev Container for Podman

This repository provides a reusable, high-performance development environment template for VS Code Dev Containers. It is built on Debian 12 (Bookworm) and optimized for use with Podman, while maintaining compatibility with Docker.

The core philosophy is to provide a clean, lightweight, and fast environment equipped with a suite of modern, productivity-enhancing CLI tools.

## ✨ Key Features

- **Base OS**: Debian 12 (Bookworm) Slim - A minimal and stable foundation.
- **Container Engine**: Optimized for Podman, the rootless, daemonless container engine.
- **Core Runtimes**: Comes with Git, Node.js (LTS), and Python 3.11 (via OS).
- **Modern CLI Arsenal**: A curated collection of fast and intuitive tools to supercharge your workflow.
- **Extensible**: Easily customize tools, VS Code extensions, and settings via the `.devcontainer/` directory.

## 🛠️ Included Tools

### 📦 Containerfile

Check the [Containerfile](.devcontainer/Containerfile) for a list of included tools.

### 📦 devcontainer.json

Check the [devcontainer.json](.devcontainer/devcontainer.json) for a list of included tools.

### 📦 post_attach.sh

Post-attach script that runs every time the container is attached to.
- .env setup and git fetch when the container is attached to.
- Check the status of GitHub authentication and run `gh auth login` if not logged in.

### 📦 setup_gish.sh

Setup script that runs only once when the container is first created.
- Set up [gishscript](https://github.com/KunihiroS/gishscript) to make available to use `gish` command from inside the container.

## 🚀 Getting Started

There are two primary ways to use this template.

### Option 1: For a New Project (Recommended)

Use this repository as a GitHub template to start a new project with this development environment.

1.  Click the green **"Use this template"** button on this repository's GitHub page and select **"Create a new repository"**.
2.  Give your new repository a name and description.
3.  Clone your newly created repository to your local machine.
4.  Open the cloned repository folder in Visual Studio Code.
5.  VS Code will automatically detect the `.devcontainer` configuration and ask if you want to "Reopen in Container". Click it to build and launch your new development environment.

### Option 2: For an Existing Project

Add this development environment to a project you're already working on.

1.  Copy the entire `.devcontainer` directory from this repository into the root of your existing project folder.
2.  Open your project folder in Visual Studio Code.
3.  VS Code will detect the new `.devcontainer` configuration and prompt you to "Reopen in Container". Click it to get started.

## 🔧 Customization Guide

### Basic Customization

- **Add more tools**: Edit the `RUN apt-get install` command in `.devcontainer/Containerfile`.
- **Add VS Code extensions**: Add extension IDs to the `customizations.vscode.extensions` list in `.devcontainer/devcontainer.json`.
- **Change runtime versions**: Modify the `features` section in `.devcontainer/devcontainer.json` to specify different versions for Node.js, or to add other runtimes like Go, Rust, etc.

### Adapting to Your Project

When applying this template to a specific project, consider the following customizations:

1. **Project-Specific Tools**
   - Update the list of tools in `.devcontainer/Containerfile` to match your project's requirements
   - Add any language-specific tools or runtimes needed for your project

2. **Environment Variables**
   - Review and update environment variables in `.devcontainer/devcontainer.json`
   - Add any project-specific environment variables to `containerEnv`
   - Consider using `initializeCommand` to set up sensitive environment variables

3. **GitHub Integration**
   - The template includes GitHub CLI setup. Ensure it meets your authentication needs
   - Update the `postAttachCommand` if you have custom Git hooks or repository setup requirements

4. **gish Integration**
   - If you're using `gish` (GitHub Issue Shell), update the repository URL in `.devcontainer/setup_gish.sh`
   - Modify any gish-specific configurations as needed

5. **Post-Creation Scripts**
   - Review and customize `.devcontainer/post_attach.sh` for any project-specific setup that should run when attaching to the container
   - Update `.devcontainer/setup_gish.sh` if you have custom setup requirements

6. **Dev Container Features**
   - The template includes Node.js and common utilities. Add or remove features in `devcontainer.json` as needed
   - Consider adding database services or other required services to `docker-compose.yml` if needed

7. **VS Code Settings**
   - Customize `.vscode/settings.json` with project-specific settings
   - Add recommended extensions for your project's technology stack

8. **Documentation**
   - Update this README with project-specific information
   - Document any additional setup steps required for your project
