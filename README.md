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

Follow these instructions to set up the development environment.

### Prerequisites

- [Visual Studio Code](https://code.visualstudio.com/)
- [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)
- [Podman](https://podman.io/getting-started/installation) (as a Docker alternative)

### Option 1: For a New Project

Use this method to start a brand new project with this environment.

1.  **Create a new repository from this template.**
    Click the **[Use this template](https://github.com/kunihiros/debian-podman-devcontainer/generate)** button on the repository's GitHub page and select **"Create a new repository"**.

2.  **Clone your new repository.**
    Replace `your-username/your-new-repo` with your actual repository details.
    ```bash
    git clone https://github.com/your-username/your-new-repo.git
    ```

3.  **Open in VS Code and start the container.**
    ```bash
    cd your-new-repo
    code .
    ```
    When VS Code opens, it will detect the `.devcontainer` configuration and prompt you to **"Reopen in Container"**. Click it to build and launch the environment.

### Option 2: For an Existing Project

Add this development environment to a project you are already working on.

1.  **Navigate to your project's root directory.**
    ```bash
    cd /path/to/your/project
    ```

2.  **Copy the `.devcontainer` directory.**
    The easiest way is to clone this template repository temporarily and copy the directory.
    ```bash
    # Clone the template to a temporary location
    git clone https://github.com/kunihiros/debian-podman-devcontainer.git /tmp/devcontainer-template
    
    # Copy the .devcontainer directory into your project
    cp -r /tmp/devcontainer-template/.devcontainer .
    
    # Clean up the temporary clone
    rm -rf /tmp/devcontainer-template
    ```
    
## 🔧 Customization Guide

After applying this template to your project using one of the methods above, it is highly recommended to perform the following customizations to tailor it to your needs.

### Essential Customizations

These changes are strongly recommended for every project to ensure clarity and avoid conflicts.

1.  **Change the Container Name**
    This name is displayed in the VS Code UI. Change it to something that reflects your project.
    -   **File**: `.devcontainer/devcontainer.json`
    -   **Property**: `"name"`
    -   **Example**: `"name": "Dev Container for My-Awesome-Project"`

2.  **Update the Image Tag Owner**
    The container image will be tagged with `kunihiros/...`. You should change `kunihiros` to your own Docker Hub username or another namespace.
    -   **File**: `.devcontainer/devcontainer.json`
    -   **Property**: `"build.tags"`
    -   **Example**: `"tags": ["your-username/${localWorkspaceFolderBasename}:latest"]`

3.  **Review VS Code Extensions**
    The default list of extensions is extensive. Remove any that are not relevant to your project's technology stack to keep the environment lightweight.
    -   **File**: `.devcontainer/devcontainer.json`
    -   **Property**: `"customizations.vscode.extensions"`

4.  **Review `gish` Integration (Optional but Recommended)**
    This template includes a setup for a tool called `gish`. If you do not use this tool, you should remove its integration.
    -   **File**: `.devcontainer/devcontainer.json`
    -   **Action**: Remove or comment out the `onCreateCommand` and `postCreateCommand` lines related to `setup_gish.sh` and `gish --help`.
    -   **Action**: You can also delete the `.devcontainer/setup_gish.sh` file.

### Optional Customizations

-   **Add System Packages:** If your project needs other system libraries (e.g., `libpq-dev`), add them to the `apt-get install` command in the `.devcontainer/Containerfile`.
-   **Add Project Dependencies:** Modify the `postCreateCommand` in `.devcontainer/devcontainer.json` to automatically install project dependencies (e.g., `npm install`, `uv pip install -r requirements.txt`).

Remember to **Rebuild Container** from the Command Palette after making changes for them to take effect.