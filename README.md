# HomeLab Dockerized GitOps Deployment

This repository is designed for managing and deploying containerized applications using GitOps principles. It supports **HomeLab** setups by providing Docker-based service configurations and seamless Git-based automation using tools like [Komodo](https://komo.do/docs/intro). Each branch in this repository represents a self-contained deployment for a specific service or stack.

---

## 🚀 Features

- Declarative Docker configurations for individual services or stacks.
- GitOps-powered continuous deployment.
- Support for orphan-branch model to isolate deployments.
- Works across different HomeLab environments.
- Bash helpers to streamline branch and deployment workflows.

---

## 📦 Requirements

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/) and Docker Compose
- A GitOps tool like [Komodo](https://komo.do/docs/intro) configured with webhook or polling
- A target server (HomeLab device) with Docker runtime

---

## 📁 Repository Structure

Each **branch** represents an independent service (e.g., `Nginx`, `Nextcloud`, `Jellyfin`) and contains:

```
.
├── docker-compose.yml     # Main service definition
├── .env                   # Optional environment variables
├── setup.sh               # Optional initialization scripts
└── README.md              # Service-specific notes
```

> ✅ Using orphan branches keeps service deployments clean, focused, and version-controlled.

---

## ⚙️ Getting Started

### 1. Clone the Repository

```bash
git clone https://{{git-source}}/{{user-name}}/{{repo-name}}.git
cd {{repo-name}}
```

### 2. Create a New Orphan Branch

```bash
git checkout --orphan <ServiceName>
```

> 📛 **Branch Naming Convention:**  
> Use a simple service name like `Nginx`, `Jellyfin`, `Plex`, etc.

---

### 3. Configure Your Service

- Edit `docker-compose.yml` and supporting files.
- Add `.env` for custom environment values if needed.
- Optionally include setup scripts (e.g., `setup.sh`).

Then:

```bash
git add .
git commit -m "Add deployment for <ServiceName>"
```

---

### 4. Push the Branch

```bash
git push origin <ServiceName>
```

---

### 5. Set Up GitOps Tool

I recommend [Komodo](https://komo.do/docs/intro), but any GitOps-compatible tool will work.

**Steps:**

1. Configure the GitOps tool to monitor your repo or a specific branch.
2. Set up a webhook or enable polling for automated triggers.
3. Choose deployment mode: auto-sync or manual approvals.
4. Test deployment by pushing a small change.

🧪 **Verify:** Ensure the GitOps tool detects changes and updates the deployment automatically.

---

## 🧰 Bash Helpers

Useful aliases and functions for managing Git branches and deployments. Add to `.bashrc`, `.zshrc`, or `.bash_aliases`.

### Pull All Branches and Sync

```bash
alias pullall='
git fetch --prune --all && \
current_branch=$(git branch --show-current); \
for local_branch in $(git branch --format="%(refname:short)"); do \
  if ! git show-ref --verify --quiet refs/remotes/origin/$local_branch; then \
    echo "🧹 Deleting local branch $local_branch (no longer exists on origin)..."; \
    git branch -D $local_branch; \
  fi; \
done; \
for remote_branch in $(git branch -r | grep -v "\->" | sed "s|origin/||"); do \
  if git show-ref --verify --quiet refs/heads/$remote_branch; then \
    echo "🔍 Checking branch $remote_branch (already exists locally)..."; \
    ahead=$(git rev-list --left-right --count $remote_branch...origin/$remote_branch | awk "{print \$1}"); \
    behind=$(git rev-list --left-right --count $remote_branch...origin/$remote_branch | awk "{print \$2}"); \
    if [ "$ahead" = "0" ] && [ "$behind" != "0" ]; then \
      echo "  ⏩ Fast-forwarding $remote_branch..."; \
      git update-ref refs/heads/$remote_branch refs/remotes/origin/$remote_branch; \
    elif [ "$ahead" = "0" ] && [ "$behind" = "0" ]; then \
      echo "  ✅ $remote_branch is already up-to-date."; \
    else \
      echo "  ⚠️ $remote_branch has diverged! Needs manual pull/merge."; \
    fi; \
  else \
    echo "🌱 Creating missing local branch $remote_branch tracking origin/$remote_branch..."; \
    git branch $remote_branch origin/$remote_branch; \
  fi; \
done; \
git checkout $current_branch'
```

### Orphan Branch Creator

```bash
orphan() {
    if [ -z "$1" ]; then
        echo "Usage: orphan <branch-name>"
        return 1
    fi
    git checkout --orphan "$1"
}
```

### Enable Git Autocompletion

```bash
source /usr/share/bash-completion/completions/git
```

---

## 🔁 Deployment Flow (Diagram)

```
  Git Branch (e.g., Nginx)
           ↓
     Git Commit/Push
           ↓
   GitOps Tool (e.g., Komodo)
           ↓
   Docker Compose Deployment
           ↓
        Target Host
```

---

## 📝 Notes

- You can create multiple branches for different services and deploy them independently.
- Avoid using `main` unless it contains meta or shared information.
- Keep branches clean — remove unused files not relevant to the specific service.
- Periodically review and test your GitOps configuration to ensure deployments stay consistent.

---
**THIS REPOSITORY IS ENCRYPTED. IF YOU'RE HERE, YOU'RE EITHER VERY BRAVE OR VERY LOST. EITHER WAY, GOOD LUCK!**
