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

### 📦 setup_gish.sh (optional)

Setup script that runs only once when the container is first created.
- Set up [gishscript](https://github.com/KunihiroS/gishscript) to make available to use `gish` command from inside the container.

### 📦 post_start.sh (optional)

Script that runs every time the container starts.
- Automatically tags the current container image with both `latest` and current date (YYYYMMDD) tags
- Ensures proper Podman socket connection to the host
- Runs after the container is fully initialized

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

4.  **Automatic Image Tagging**
    This template automatically tags your development container images with both `latest` and the current date (e.g., `20250629`). This happens automatically when the container starts.
    - **Configuration**:
      - Base image name is set via `IMAGE_NAME_BASE` in `.devcontainer/devcontainer.json`
      - Date format: `YYYYMMDD` (e.g., `20250629`)
    - **Example**:
      ```bash
      # Resulting tags for an image named 'my-project' on June 29, 2025
      ghcr.io/your-username/my-project:latest
      ghcr.io/your-username/my-project:20250629
      ```
    - **Customization**: Edit `.devcontainer/post_start.sh` to modify the tagging logic

5.  **Review `gish` Integration (Optional but Recommended)**
    This template includes a setup for a tool called `gish`. If you do not use this tool, you should remove its integration.
    -   **File**: `.devcontainer/devcontainer.json`
    -   **Action**: Remove or comment out the `onCreateCommand` and `postCreateCommand` lines related to `setup_gish.sh` and `gish --help`.
    -   **Action**: You can also delete the `.devcontainer/setup_gish.sh` file.

### Optional Customizations

-   **Add System Packages:** If your project needs other system libraries (e.g., `libpq-dev`), add them to the `apt-get install` command in the `.devcontainer/Containerfile`.
-   **Add Project Dependencies:** Modify the `postCreateCommand` in `.devcontainer/devcontainer.json` to automatically install project dependencies (e.g., `npm install`, `uv pip install -r requirements.txt`).

Remember to **Rebuild Container** from the Command Palette after making changes for them to take effect.

## 🐳 Using Podman Inside the Container

This template is designed to connect to and use the **host machine's Podman daemon** from within the container.

### Basic Usage

When running Podman commands inside the container, always use one of the following methods:

```bash
# Method 1: Explicitly specify the environment variable
sudo podman --url "$PODMAN_HOST" images

# Method 2: Set up an alias (recommended)
echo 'alias p="sudo podman --url \"\$PODMAN_HOST\""' >> ~/.bashrc
source ~/.bashrc
p images  # Use the alias
```

### Common Errors and Their Causes

If you see the following error, it means the container failed to connect to the host's Podman:

```bash
$ podman images  # Example of what will fail
ERRO[0000] running `/usr/bin/newuidmap 1234 0 1000 1 1 100000 65536`: 
newuidmap: write to uid_map failed: Operation not permitted
```

**Reason**: This occurs when trying to start rootless Podman inside the container, which fails due to user namespace mapping restrictions.

### Technical Details

- **Connection Method**: The host's Podman socket (`/run/user/1000/podman/podman.sock`) is mounted into the container and specified via the `PODMAN_HOST` environment variable
- **Permissions**: Uses `sudo` to interact with the host's Podman
- **Security Note**: This allows container operations on the host - only use in trusted environments

### Advanced: Docker-in-Docker Alternatives

For completely isolated container environments (without using the host's Podman), consider these alternatives:

1. **Using Docker inside the container**:
   ```bash
   # Install Docker inside the container
   sudo apt-get update && sudo apt-get install -y docker.io
   sudo systemctl enable --now docker
   ```

2. **Using Rootless Podman inside the container**:
   ```bash
   # Set up rootless Podman inside the container
   sudo apt-get install -y podman uidmap
   podman system migrate
   ```

### Troubleshooting

- **If you see "permission denied"**:
  ```bash
  # Check socket permissions on the host
  ls -l /run/user/1000/podman/podman.sock
  
  # If needed, adjust permissions (run on host)
  sudo chmod 666 /run/user/1000/podman/podman.sock
  ```

- **If you see "Connection refused"**:
  ```bash
  # Verify Podman socket service is running on host
  systemctl --user status podman.socket
  ```

## Prerequisites

### Host System Requirements

- **Operating System**: Linux with systemd (tested on Ubuntu 22.04/24.04)
- **User Account**: Standard non-root user with sudo privileges
- **Podman**: Latest stable version installed and configured for rootless operation

### Podman Configuration

Before using this template, ensure your host system has the following Podman configuration:

1. **Enable Podman Socket**:
   ```bash
   systemctl --user enable --now podman.socket
   ```

2. **Verify Socket Permissions**:
   ```bash
   ls -l /run/user/$(id -u)/podman/podman.sock
   # Should show: srw-rw---- 1 user user 0 ...
   ```

3. **Configure Sudo (Optional)**:
   If you want to avoid password prompts, add the following to `/etc/sudoers`:
   ```
   username ALL=(root) NOPASSWD: /usr/bin/podman
   ```
   Replace `username` with your actual username.

## Known Limitations

- Assumes default user UID 1000
- Requires rootless Podman configuration
- Socket path is hardcoded to `/run/user/1000/podman/podman.sock`

## Future Improvements (Optional)

### Making the Configuration More Robust

1. **Dynamic UID Detection**
   In `.devcontainer/devcontainer.json`:
   ```json
   "containerEnv": {
     "HOST_UID": "${localEnv:UID}",
     "PODMAN_SOCKET": "/run/user/${localEnv:UID}/podman/podman.sock"
   },
   "mounts": [
     "source=/run/user/${localEnv:UID}/podman/podman.sock,target=/run/user/${localEnv:UID}/podman/podman.sock,type=bind"
   ]
   ```

2. **Multiple Socket Path Fallback**
   In `post_start.sh`:
   ```bash
   # Try multiple common socket paths
   SOCKET_PATHS=(
     "/run/user/$(id -u)/podman/podman.sock"
     "${XDG_RUNTIME_DIR}/podman/podman.sock"
     "/run/podman/podman.sock"
   )
   
   for SOCKET in "${SOCKET_PATHS[@]}"; do
     if [ -S "$SOCKET" ]; then
       export PODMAN_HOST="unix://$SOCKET"
       break
     fi
   done
   
   if [ -z "$PODMAN_HOST" ]; then
     echo "Error: Could not find Podman socket" >&2
     exit 1
   fi
   ```

3. **Better Error Messages**
   ```bash
   if ! command -v podman &> /dev/null; then
     echo "Error: Podman is not installed on the host" >&2
     exit 1
   fi
   
   if ! podman info &> /dev/null; then
     echo "Error: Cannot connect to Podman. Please ensure the Podman service is running" >&2
     echo "Hint: Run 'systemctl --user start podman.socket'" >&2
     exit 1
   fi
   ```

## Troubleshooting

### Common Issues & Fixes

| Issue | Symptom | Quick Fix |
|-------|---------|-----------|
| Permission denied | `permission denied` when accessing the socket | `sudo chmod 666 /run/user/$(id -u)/podman/podman.sock` |
| User-namespace error | `newuidmap: write to uid_map failed` | Ensure subuid/subgid mapping exists in `/etc/subuid`, `/etc/subgid` (e.g., `sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 $(whoami)`) |
| Socket not found | `cannot connect to "unix:///...podman.sock"` | `systemctl --user enable --now podman.socket` on host |
| Podman not installed | `podman: command not found` | Install Podman on host (`apt install podman` or similar) |
| Podman service down | `Error: Cannot connect to Podman` | `systemctl --user restart podman.socket` |

> **Tip**: Run `podman --log-level=debug info` inside the container (with `sudo` + `--url "$PODMAN_HOST"`) for more verbose diagnostics.

   ```bash
   # On the host machine:
   sudo chmod 666 /run/user/$(id -u)/podman/podman.sock
   ```

2. **User Namespace Issues**
   Ensure `/etc/subuid` and `/etc/subgid` are properly configured:
   ```bash
   # Check current subuid mapping
   grep $(whoami) /etc/subuid
   grep $(whoami) /etc/subgid
   
   # If not set, add your user (requires root)
   sudo usermod --add-subuids 100000-165535 --add-subgids 100000-165535 $(whoami)
   ```

3. **Socket Not Found**
   Verify the Podman socket service is running:
   ```bash
   systemctl --user status podman.socket
   # If not running:
   systemctl --user enable --now podman.socket
   ```