# HomeLab Docker GitOps Deployment

This repository is designed to streamline the deployment and management of Docker containers in a HomeLab environment using GitOps principles. It provides a collection of Docker-based configurations and scripts, enabling automation and version control for containerized applications. The repository integrates seamlessly with GitOps tools, allowing for continuous deployment directly from the Git repository.

## Features

- Docker-based configurations for various applications and services.
- GitOps-powered automation and version control for deployments.
- Seamless integration with GitOps tools for continuous deployment.
- Simple process for managing and scaling containerized applications in a HomeLab.

## Requirements

- Git installed
- Docker and Docker Compose
- A GitOps tool like [Komodo](https://komo.do/docs/intro) configured and running

## Repository Structure

Each branch contains the main configuration and templates at its root directory.

## Getting Started

To start using this repository, follow the steps below:

### 1. Clone the Repository

```bash
git clone https://{{git-source}}/homelab10400/dockerized-homelab.git
cd dockerized-homelab
```

### 2. Create a New Branch

To create a new branch for your HomeLab containerized application, use the following naming convention:  
**Branch Naming Convention:**

```yaml
ServiceName
```

**Example:**  
`Nginx`

Create the orphan branch:

```bash
git checkout --orphan Nginx
```

### 3. Make Changes and Commit

After creating your new branch, make any modifications to Docker configs, scripts, or services.

1. Stage the changes:

    ```bash
    git add .
    ```

2. Commit them with a descriptive message:

    ```bash
    git commit -m "Your commit message"
    ```

### 4. Push Your Branch

```bash
git push origin Nginx
```

### 5. Set Up Continuous Deployment

After pushing your branch, set up continuous deployment using Komodo. This ensures changes in the repository automatically trigger deployments.

#### Steps:

1. **Choose a GitOps Tool**  
   We are using [Komodo](https://komo.do/docs/intro) for GitOps operations.

2. **Configure Komodo to Watch Your Repository**  
   You can configure Komodo to monitor all branches or specific ones for deployment triggers.  
   Follow the guide: [Set up a webhook in Komodo](https://komo.do/docs/webhooks)

3. **Test Deployment**  
   Push a small change to your branch and verify that Komodo detects the change and deploys it as expected.

---

## Bash Helpers

You may find the following aliases and functions useful in your workflow.

### Add the following to `.bashrc` or `.bash_aliases`:

```bash
# Pull all local branches from remote
alias pullall="for branch in \$(git branch | sed 's/^\* //'); do git checkout \$branch && git pull; done"

# Enable Git autocompletion (if not already enabled)
source /usr/share/bash-completion/completions/git

# Create an orphan branch easily
orphan() {
  if [ -z "$1" ]; then
    echo "Usage: orphan <branch-name>"
    return 1
  fi
  git checkout --orphan "$1"
}
```

---

**Branch Naming Reminder:**  
Please follow the naming convention: `ServiceName`  
_(e.g., `Nginx`, `Nextcloud`, `Jellyfin`)_

---

## Deployment Flow (Diagram)

```
Git Branch (e.g., Nginx)
      ↓
  Git Commit/Push
      ↓
  Komodo GitOps Tool
      ↓
  Docker Deployment to Target Server/VM
```

---
**THIS REPOSITORY IS ENCRYPTED. IF YOU'RE HERE, YOU'RE EITHER VERY BRAVE OR VERY LOST. EITHER WAY, GOOD LUCK!**
