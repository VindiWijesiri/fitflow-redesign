# GitHub Repository Setup Guide

This guide will help you push the FitFlow redesign project to GitHub.

## Step 1: Create GitHub Repository

1. Go to [GitHub](https://github.com) and login
2. Click the **"+"** icon in the top right corner
3. Select **"New repository"**
4. Configure the repository:
   - **Repository name**: `fitflow-redesign`
   - **Description**: `FitFlow Fitness App Redesign - AI-Powered Cross-Platform Mobile Application`
   - **Visibility**: Choose Public or Private
   - **DO NOT** initialize with README, .gitignore, or license (we already have these)
5. Click **"Create repository"**

## Step 2: Configure Git Remote

```bash
# Add GitHub repository as remote
git remote add origin https://github.com/YOUR_USERNAME/fitflow-redesign.git

# Verify remote was added
git remote -v
```

Replace `YOUR_USERNAME` with your actual GitHub username.

## Step 3: Push to GitHub

```bash
# Push to main branch
git push -u origin main
```

If you encounter authentication issues:

### Option A: Personal Access Token (Recommended)
1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token with `repo` scope
3. Use token as password when pushing

### Option B: SSH Key
```bash
# Generate SSH key
ssh-keygen -t ed25519 -C "your_email@example.com"

# Add SSH key to GitHub account (Settings → SSH and GPG keys)
# Change remote to SSH
git remote set-url origin git@github.com:YOUR_USERNAME/fitflow-redesign.git
```

## Step 4: Configure Repository Settings

### Branch Protection

1. Go to repository **Settings** → **Branches**
2. Click **"Add branch protection rule"**
3. Configure:
   - **Branch name pattern**: `main`
   - ✅ Require pull request reviews before merging
   - ✅ Require status checks to pass before merging
   - Select: `backend-test`, `frontend-test`, `build-check`
   - ✅ Require branches to be up to date before merging
   - ✅ Include administrators
4. Click **"Create"**

### Repository Settings

1. Go to **Settings** → **General**
2. Set description:
   ```
   FitFlow Fitness App Redesign - AI-Powered Cross-Platform Mobile Application with React Native, Node.js, Firebase, TensorFlow Lite, and ML Kit
   ```
3. Add topics (tags):
   - `react-native`
   - `fitness-app`
   - `ai-ml`
   - `tensorflow-lite`
   - `firebase`
   - `nodejs`
   - `typescript`
   - `mobile-app`
   - `cross-platform`
   - `hci-project`

### Enable Issues

1. Go to **Settings** → **General**
2. Under **Features**, ensure **Issues** is checked
3. Create issue templates:
   - Bug Report
   - Feature Request
   - Documentation Update

### Enable GitHub Pages (Optional - for documentation)

1. Go to **Settings** → **Pages**
2. Source: Deploy from branch `main`, folder `/docs`
3. Save

### Set Up GitHub Actions

The CI/CD workflow is already configured in `.github/workflows/ci.yml`.

To enable:
1. Go to **Actions** tab
2. Enable workflows
3. First push will trigger the workflow automatically

## Step 5: Create Development Branch

```bash
# Create and switch to develop branch
git checkout -b develop

# Push develop branch
git push -u origin develop
```

## Step 6: Add Collaborators (if team project)

1. Go to **Settings** → **Collaborators and teams**
2. Click **"Add people"**
3. Enter GitHub usernames of team members
4. Select permission level (Write or Admin)
5. Send invitations

## Step 7: Repository Labels

Create useful labels for issues and PRs:

1. Go to **Issues** → **Labels**
2. Create these labels:
   - `frontend` - Frontend related issues
   - `backend` - Backend related issues
   - `ai-ml` - AI/ML related issues
   - `documentation` - Documentation updates
   - `bug` - Bug reports
   - `enhancement` - New features
   - `good first issue` - Good for newcomers
   - `high priority` - High priority items
   - `in progress` - Currently being worked on

## Step 8: Create README Badge (Optional)

Add CI/CD status badge to README.md:

```markdown
![CI/CD](https://github.com/YOUR_USERNAME/fitflow-redesign/workflows/FitFlow%20CI%2FCD%20Pipeline/badge.svg)
```

## Step 9: Project Board (Optional)

Create a project board for task management:

1. Go to **Projects** tab
2. Click **"New project"**
3. Choose **"Board"** template
4. Name it "FitFlow Development"
5. Create columns:
   - 📋 To Do
   - 🏃 In Progress
   - 👀 In Review
   - ✅ Done

## Step 10: Wiki (Optional)

Enable and set up wiki for additional documentation:

1. Go to **Settings** → **General** → **Features**
2. Enable **Wiki**
3. Create wiki pages:
   - Home
   - Development Setup
   - API Documentation
   - Deployment Guide
   - Contributing Guidelines

## Verification Checklist

After setup, verify these items:

- ✅ Repository created and code pushed
- ✅ README.md displays correctly
- ✅ All documentation files visible in `docs/` folder
- ✅ CI/CD workflow runs successfully
- ✅ Branch protection enabled on `main`
- ✅ Repository description and topics set
- ✅ .gitignore working (no `node_modules/`, `.env` files visible)
- ✅ License file present (MIT)
- ✅ Architecture diagram displays correctly

## Working with the Repository

### Creating a Feature Branch

```bash
# Sync with main
git checkout main
git pull origin main

# Create feature branch
git checkout -b feature/your-feature-name

# Make changes, commit, and push
git add .
git commit -m "Add your feature"
git push -u origin feature/your-feature-name
```

### Creating a Pull Request

1. Go to repository on GitHub
2. Click **"Pull requests"** → **"New pull request"**
3. Select base branch (usually `main` or `develop`)
4. Select compare branch (your feature branch)
5. Fill in PR template:
   - **Title**: Clear description of changes
   - **Description**: What, why, and how
   - **Related issues**: Link any related issues
6. Request reviewers
7. Assign yourself
8. Add labels
9. Click **"Create pull request"**

### Merging a Pull Request

1. Ensure all CI checks pass
2. Get required reviews
3. Resolve any conflicts
4. Click **"Merge pull request"**
5. Choose merge method:
   - **Merge commit**: Preserves all commits
   - **Squash and merge**: Combines all commits (recommended for clean history)
   - **Rebase and merge**: Rebases commits onto base branch
6. Delete branch after merge

## Recommended GitHub Settings

### Security

1. Enable **Dependabot alerts**:
   - Go to **Settings** → **Security & analysis**
   - Enable Dependabot alerts
   - Enable Dependabot security updates

2. Set up **Code scanning**:
   - Go to **Security** → **Code scanning**
   - Set up CodeQL analysis

### Notifications

Configure notification preferences:
- **Settings** → **Notifications**
- Customize which events trigger notifications

## Common Issues and Solutions

### Push Rejected

```bash
# Fetch latest changes
git pull origin main --rebase

# Force push (use carefully)
git push -f origin main
```

### Large Files

If you accidentally commit large files:

```bash
# Remove from history
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch PATH_TO_FILE" \
  --prune-empty --tag-name-filter cat -- --all

# Force push
git push origin --force --all
```

Better: Use `.gitignore` to prevent this.

### Merge Conflicts

```bash
# Pull latest changes
git pull origin main

# Resolve conflicts manually in your editor
# Look for <<<<<<< HEAD markers

# After resolving
git add .
git commit -m "Resolve merge conflicts"
git push origin your-branch
```

## Team Workflow

### Recommended Git Flow

```
main (production-ready)
  └── develop (integration branch)
       ├── feature/ai-recommendations
       ├── feature/social-feed
       ├── feature/nutrition-tracking
       └── bugfix/login-issue
```

### Commit Message Convention

Use conventional commits:

```
feat: Add workout recommendation algorithm
fix: Resolve authentication token expiration
docs: Update API documentation
style: Format code with prettier
refactor: Restructure workout service
test: Add unit tests for nutrition service
chore: Update dependencies
```

## Resources

- [GitHub Documentation](https://docs.github.com/)
- [Git Cheat Sheet](https://education.github.com/git-cheat-sheet-education.pdf)
- [Conventional Commits](https://www.conventionalcommits.org/)
- [GitHub Flow](https://guides.github.com/introduction/flow/)

---

## Next Steps

After pushing to GitHub:

1. **Share repository URL** with team members and stakeholders
2. **Set up local development environments** following README.md
3. **Create first issues** for upcoming features
4. **Start development** on feature branches
5. **Submit Pull Requests** for review

## Support

For questions about Git/GitHub:
- [GitHub Community Forum](https://github.community/)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/git)
- Team communication channels

---

**Created**: September 12, 2026  
**Repository**: fitflow-redesign  
**Team**: FitFlow Development Team
