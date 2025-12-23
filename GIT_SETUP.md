# Git Repository Setup Guide

## Reinitialize Git Repository

To start fresh with a new git repository, follow these steps:

### Step 1: Remove existing git repository (if starting fresh)
```powershell
# Navigate to your project directory
cd C:\Users\kumman\Documents\Devlaunch\chatbot

# Remove existing .git folder (this will delete all git history)
Remove-Item -Recurse -Force .git
```

### Step 2: Initialize new git repository
```powershell
# Initialize new git repository
git init
```

### Step 3: Add all files to staging
```powershell
# Add all files (except those in .gitignore)
git add .
```

### Step 4: Make initial commit
```powershell
# Create initial commit
git commit -m "Initial commit: Chatbot project with frontend and backend"
```

### Step 5: (Optional) Add remote repository
```powershell
# Add GitHub remote (replace with your repository URL)
git remote add origin https://github.com/your-username/your-repo-name.git

# Push to remote
git branch -M main
git push -u origin main
```

## Quick Setup (All in one)

If you want to do everything at once:

```powershell
cd C:\Users\kumman\Documents\Devlaunch\chatbot
Remove-Item -Recurse -Force .git -ErrorAction SilentlyContinue
git init
git add .
git commit -m "Initial commit: Chatbot project"
```

## Verify Setup

Check if everything is set up correctly:
```powershell
# Check git status
git status

# Check git log
git log

# Check remote (if added)
git remote -v
```

## Important Notes

- The `.gitignore` file is already configured to exclude:
  - `node_modules/`
  - `.env` files
  - `existing_assistant.json`
  - Other common build/cache files

- Before pushing to GitHub, make sure to:
  1. Review what files are being tracked: `git status`
  2. Never commit `.env` files with real API keys
  3. The `.gitignore` should prevent sensitive files from being committed

