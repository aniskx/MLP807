# [git-workshop-tutorial.md](https://github.com/user-attachments/files/23542956/git-workshop-tutorial.md)
# Git & CI/CD Workshop: From Basics to Snowflake Deployment

**Duration:** 2+ hours  
**Level:** Beginner to Intermediate  
**Prerequisites:** Basic command line knowledge

---

## Workshop Overview

This hands-on workshop will take you through:
1. **Part 1:** Essential Git commands and concepts (45 minutes)
2. **Part 2:** Git workflows and Pull Requests (45 minutes)
3. **Part 3:** CI/CD Pipeline - Deploy a Snowflake table (30+ minutes)

By the end of this workshop, you'll be able to manage code with Git, collaborate using modern workflows, and automate deployments to Snowflake.

---

## Table of Contents

- [Part 1: Essential Git Commands](#part-1-essential-git-commands)
  - [Git Configuration](#1-git-configuration)
  - [Repository Initialization](#2-repository-initialization)
  - [Basic Workflow Commands](#3-basic-workflow-commands)
  - [Branching and Merging](#4-branching-and-merging)
  - [Viewing History](#5-viewing-history)
  - [Undoing Changes](#6-undoing-changes)
  - [Remote Repositories](#7-remote-repositories)
  - [Stashing Changes](#8-stashing-changes)
- [Part 2: Git Workflows and Pull Requests](#part-2-git-workflows-and-pull-requests)
  - [Common Git Workflows](#common-git-workflows)
  - [Pull Request Best Practices](#pull-request-best-practices)
- [Part 3: CI/CD with Snowflake](#part-3-cicd-with-snowflake)
  - [Project Setup](#project-setup)
  - [GitHub Actions Configuration](#github-actions-configuration)
  - [Testing the Pipeline](#testing-the-pipeline)

---

## Part 1: Essential Git Commands

### Prerequisites Setup

Before we begin, ensure you have Git installed:

```bash
# Check Git version
git --version

# If not installed:
# - Windows: Download from https://git-scm.com/
# - Mac: brew install git
# - Linux: sudo apt-get install git
```

---

### 1. Git Configuration

Configure your Git identity and preferences:

```bash
# Set your name (appears in commits)
git config --global user.name "Your Name"

# Set your email
git config --global user.email "your.email@example.com"

# Set default branch name to 'main'
git config --global init.defaultBranch main

# Set default editor (optional)
git config --global core.editor "code --wait"  # VS Code
# OR
git config --global core.editor "nano"  # Nano editor

# Enable color output
git config --global color.ui auto

# View all configurations
git config --list

# View specific configuration
git config user.name
```

**💡 Tip:** The `--global` flag sets configuration for all repositories. Omit it to set config for the current repository only.

---

### 2. Repository Initialization

Two ways to start working with Git:

#### Option A: Create a new repository

```bash
# Create a new directory
mkdir my-project
cd my-project

# Initialize Git repository
git init

# Verify initialization
ls -la  # You should see a .git directory
```

#### Option B: Clone an existing repository

```bash
# Clone from GitHub
git clone https://github.com/username/repository.git

# Clone to a specific directory
git clone https://github.com/username/repository.git my-folder

# Clone a specific branch
git clone -b branch-name https://github.com/username/repository.git
```

---

### 3. Basic Workflow Commands

The fundamental Git workflow: **modify → stage → commit**

#### Check Repository Status

```bash
# Show working directory status
git status

# Short status format
git status -s
```

**Status Indicators:**
- `??` - Untracked files
- `A` - Added files (staged)
- `M` - Modified files
- `D` - Deleted files

#### Staging Changes

```bash
# Stage a specific file
git add filename.txt

# Stage multiple files
git add file1.txt file2.txt

# Stage all changes in current directory
git add .

# Stage all changes in repository
git add -A

# Stage only modified files (not new files)
git add -u

# Interactively stage parts of files
git add -p filename.txt
```

#### Committing Changes

```bash
# Commit staged changes
git commit -m "Your commit message"

# Commit with detailed message (opens editor)
git commit

# Stage and commit in one step (tracked files only)
git commit -am "Your commit message"

# Amend the last commit (fix message or add files)
git add forgotten-file.txt
git commit --amend -m "Updated commit message"
```

**✅ Good Commit Messages:**
```
Add user authentication feature
Fix bug in data processing pipeline
Update documentation for API endpoints
Refactor database connection logic
```

**❌ Bad Commit Messages:**
```
Fixed stuff
Update
asdfgh
More changes
```

---

### 4. Branching and Merging

Branches allow you to work on features independently.

#### Branch Basics

```bash
# List all local branches
git branch

# List all branches (local + remote)
git branch -a

# Create a new branch
git branch feature-login

# Switch to a branch
git checkout feature-login

# Create and switch to a new branch (shortcut)
git checkout -b feature-dashboard

# Using newer 'switch' command (Git 2.23+)
git switch feature-dashboard
git switch -c new-feature  # Create and switch
```

#### Merging Branches

```bash
# Switch to the branch you want to merge INTO
git checkout main

# Merge another branch into current branch
git merge feature-login

# Merge with a commit message
git merge feature-login -m "Merge login feature"

# Abort a merge if conflicts arise
git merge --abort
```

#### Handling Merge Conflicts

When Git can't automatically merge changes:

```bash
# After running git merge, if conflicts occur:
# 1. Check which files have conflicts
git status

# 2. Open conflicted files - you'll see markers:
<<<<<<< HEAD
Your current branch code
=======
Incoming branch code
>>>>>>> feature-branch

# 3. Edit files to resolve conflicts (remove markers)

# 4. Stage resolved files
git add resolved-file.txt

# 5. Complete the merge
git commit -m "Resolve merge conflicts"
```

#### Deleting Branches

```bash
# Delete a local branch (safe - won't delete unmerged branches)
git branch -d feature-login

# Force delete a branch
git branch -D feature-old

# Delete a remote branch
git push origin --delete feature-login
```

---

### 5. Viewing History

Understanding your project's history:

```bash
# View commit history
git log

# Compact one-line format
git log --oneline

# Show last 5 commits
git log -5

# Show commits with file changes
git log --stat

# Show commits with actual changes
git log -p

# Show graph of branches
git log --graph --oneline --all

# Search commits by author
git log --author="Anis"

# Search commits by message
git log --grep="bug fix"

# Show commits affecting a specific file
git log filename.txt

# Show changes in a commit
git show commit-hash

# Show changes between commits
git diff commit1 commit2
```

**🎯 Useful Alias:**
```bash
# Create a pretty log alias
git config --global alias.lg "log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

# Use it with:
git lg
```

---

### 6. Undoing Changes

Git provides multiple ways to undo changes:

#### Unstage Files

```bash
# Unstage a file (keep changes in working directory)
git reset filename.txt

# Unstage all files
git reset
```

#### Discard Changes

```bash
# Discard changes in working directory (DESTRUCTIVE)
git checkout -- filename.txt

# Using newer 'restore' command (Git 2.23+)
git restore filename.txt

# Discard all local changes (DESTRUCTIVE)
git checkout -- .
# OR
git restore .
```

#### Revert Commits

```bash
# Create a new commit that undoes a previous commit
git revert commit-hash

# Revert the last commit
git revert HEAD

# Revert multiple commits
git revert commit1 commit2
```

#### Reset Commits

```bash
# Move branch pointer back (keep changes staged)
git reset --soft HEAD~1

# Move branch pointer back (keep changes unstaged)
git reset HEAD~1
# OR
git reset --mixed HEAD~1

# Move branch pointer back and discard changes (DESTRUCTIVE)
git reset --hard HEAD~1

# Reset to a specific commit
git reset --hard commit-hash
```

**⚠️ Warning:** `git reset --hard` is destructive and will permanently delete uncommitted changes!

---

### 7. Remote Repositories

Working with remote repositories (GitHub, GitLab, Bitbucket):

#### Adding Remotes

```bash
# Add a remote repository
git remote add origin https://github.com/username/repo.git

# View remote repositories
git remote -v

# Show detailed remote info
git remote show origin

# Rename a remote
git remote rename origin upstream

# Remove a remote
git remote remove origin
```

#### Fetching and Pulling

```bash
# Download changes from remote (doesn't merge)
git fetch origin

# Download and merge changes
git pull origin main

# Pull with rebase instead of merge
git pull --rebase origin main

# Pull from specific remote and branch
git pull origin feature-branch
```

**Difference: Fetch vs Pull**
- `git fetch`: Downloads changes but doesn't modify your working directory
- `git pull`: Downloads changes AND merges them (`git fetch` + `git merge`)

#### Pushing Changes

```bash
# Push to remote repository
git push origin main

# Push and set upstream (first time)
git push -u origin main
# After this, you can just use: git push

# Push a new branch
git push origin feature-login

# Push all branches
git push --all origin

# Force push (DANGEROUS - use with caution)
git push --force origin main
# Safer alternative:
git push --force-with-lease origin main
```

---

### 8. Stashing Changes

Temporarily save changes without committing:

```bash
# Stash current changes
git stash

# Stash with a message
git stash save "Work in progress on login feature"

# List all stashes
git stash list

# Apply most recent stash (keep in stash list)
git stash apply

# Apply and remove most recent stash
git stash pop

# Apply a specific stash
git stash apply stash@{2}

# Show stash contents
git stash show -p stash@{0}

# Drop a specific stash
git stash drop stash@{1}

# Clear all stashes
git stash clear
```

**Use Case:** You're working on a feature but need to quickly switch branches to fix a bug:
```bash
git stash                    # Save current work
git checkout main           # Switch to main
git checkout -b bugfix      # Create bugfix branch
# ... fix the bug ...
git checkout feature-branch # Switch back
git stash pop              # Restore your work
```

---

### 🎓 Part 1 Practice Exercise

Let's practice what we've learned:

```bash
# 1. Create a new repository
mkdir git-practice
cd git-practice
git init

# 2. Create a file and make first commit
echo "# My Git Practice" > README.md
git add README.md
git commit -m "Initial commit"

# 3. Create a new branch and make changes
git checkout -b feature-about
echo "## About\nThis is a practice repository" >> README.md
git add README.md
git commit -m "Add about section"

# 4. Switch back to main and create another branch
git checkout main
git checkout -b feature-contact
echo "## Contact\nEmail: example@email.com" >> README.md
git add README.md
git commit -m "Add contact section"

# 5. Merge feature-about into main
git checkout main
git merge feature-about

# 6. Merge feature-contact into main (will cause a conflict!)
git merge feature-contact
# Resolve the conflict manually, then:
git add README.md
git commit -m "Merge contact feature"

# 7. View your work
git log --graph --oneline --all

# 8. Clean up
git branch -d feature-about
git branch -d feature-contact
```

---

## Part 2: Git Workflows and Pull Requests

### Common Git Workflows

Different teams use different workflows. Here are the most popular ones:

---

#### 1. Centralized Workflow

**Best for:** Small teams, simple projects

**How it works:**
- Everyone works on the `main` branch
- Pull before pushing
- Resolve conflicts locally

```bash
# Daily workflow
git pull origin main        # Get latest changes
# ... make your changes ...
git add .
git commit -m "Your changes"
git pull origin main        # Check for new changes again
git push origin main        # Push your changes
```

**Pros:**
- Simple and straightforward
- Easy to understand for beginners

**Cons:**
- No isolation for features
- Risk of breaking main branch
- Difficult to review code

---

#### 2. Feature Branch Workflow

**Best for:** Most teams, standard development

**How it works:**
- `main` branch is always stable
- Create a new branch for each feature
- Merge back when feature is complete

```bash
# Start new feature
git checkout main
git pull origin main
git checkout -b feature-user-profile

# Work on feature
# ... make changes ...
git add .
git commit -m "Add user profile page"
git commit -m "Add profile edit functionality"

# Keep feature branch updated with main
git checkout main
git pull origin main
git checkout feature-user-profile
git merge main  # OR: git rebase main

# Push feature branch
git push origin feature-user-profile

# After review and approval, merge to main
git checkout main
git merge feature-user-profile
git push origin main

# Clean up
git branch -d feature-user-profile
git push origin --delete feature-user-profile
```

**Naming Conventions:**
```
feature/user-authentication
feature/dashboard-widgets
bugfix/login-validation
hotfix/critical-security-patch
```

**Pros:**
- Isolates features
- Main branch stays stable
- Enables code review

**Cons:**
- Requires coordination
- Can have merge conflicts

---

#### 3. Gitflow Workflow

**Best for:** Projects with scheduled releases

**How it works:**
- Multiple long-lived branches: `main`, `develop`
- Feature branches merge to `develop`
- Release branches prepare for production
- Hotfix branches for urgent fixes

**Branch Structure:**
```
main        (production-ready code)
  |
develop     (integration branch)
  |
  ├─ feature/feature-1
  ├─ feature/feature-2
  └─ feature/feature-3
  |
release/1.0 (release preparation)
  |
hotfix/critical-bug (urgent fixes)
```

**Workflow Steps:**

```bash
# Setup
git checkout -b develop main

# Create feature
git checkout -b feature/payment develop
# ... work on feature ...
git checkout develop
git merge feature/payment
git branch -d feature/payment

# Create release branch
git checkout -b release/1.0 develop
# ... fix bugs, update version ...
git checkout main
git merge release/1.0
git tag -a v1.0 -m "Version 1.0"
git checkout develop
git merge release/1.0
git branch -d release/1.0

# Hotfix
git checkout -b hotfix/security-fix main
# ... fix critical bug ...
git checkout main
git merge hotfix/security-fix
git tag -a v1.0.1 -m "Hotfix 1.0.1"
git checkout develop
git merge hotfix/security-fix
git branch -d hotfix/security-fix
```

**Pros:**
- Clear separation of concerns
- Supports multiple versions
- Organized releases

**Cons:**
- Complex for small teams
- More branches to manage
- Longer cycle time

---

#### 4. GitHub Flow

**Best for:** Continuous deployment, web applications

**How it works:**
- Only `main` branch is permanent
- Create short-lived feature branches
- Use Pull Requests for review
- Deploy from `main` constantly

```bash
# 1. Create branch from main
git checkout main
git pull origin main
git checkout -b add-analytics

# 2. Make commits
git add .
git commit -m "Add Google Analytics tracking"
git push origin add-analytics

# 3. Open Pull Request on GitHub
# (done via GitHub UI)

# 4. After review and CI passes, merge to main
# (done via GitHub UI)

# 5. Deploy main to production
# (automated via CI/CD)

# 6. Delete branch
git checkout main
git pull origin main
git branch -d add-analytics
```

**Pros:**
- Simple and fast
- Encourages continuous deployment
- Quick feedback

**Cons:**
- Requires good CI/CD
- Less control over releases
- Main must always be deployable

---

#### 5. Trunk-Based Development

**Best for:** High-performance teams, continuous integration

**How it works:**
- Developers commit to `main` (trunk) daily
- Very short-lived feature branches (< 1 day)
- Feature flags for incomplete features
- Heavy reliance on automated testing

```bash
# Create short-lived branch
git checkout -b quick-fix
# ... work for a few hours ...
git add .
git commit -m "Implement quick fix"
git push origin quick-fix

# Immediately merge (after quick review)
git checkout main
git pull origin main
git merge quick-fix
git push origin main
git branch -d quick-fix

# For larger features, use feature flags
echo "if (featureFlags.newDashboard) { ... }" > code.js
git add code.js
git commit -m "Add new dashboard behind feature flag"
git push origin main
```

**Pros:**
- Fast integration
- Reduces merge conflicts
- Continuous delivery

**Cons:**
- Requires discipline
- Need strong testing
- Feature flags add complexity

---

### Pull Request Best Practices

Pull Requests (PRs) are essential for code review and collaboration.

#### Creating a Good Pull Request

**1. Before Creating PR:**

```bash
# Ensure your branch is up to date
git checkout main
git pull origin main
git checkout your-feature-branch
git merge main  # OR: git rebase main

# Run tests
npm test  # or your test command

# Push your branch
git push origin your-feature-branch
```

**2. PR Title and Description Template:**

```markdown
## Title
[Type] Brief description of changes

Examples:
- [Feature] Add user authentication
- [Bugfix] Fix null pointer in data pipeline
- [Refactor] Improve error handling
- [Docs] Update API documentation

## Description
### What does this PR do?
Brief summary of the changes

### Why are we making this change?
Context and motivation

### How was this tested?
- [ ] Unit tests added/updated
- [ ] Manual testing performed
- [ ] Integration tests pass

### Screenshots (if applicable)
[Add screenshots for UI changes]

### Related Issues
Closes #123
Related to #456

### Checklist
- [ ] Code follows project style guide
- [ ] Tests added for new functionality
- [ ] Documentation updated
- [ ] No sensitive data in code
- [ ] Ready for review
```

**3. PR Size Best Practices:**
- Keep PRs small (< 400 lines changed)
- One feature or fix per PR
- Break large features into multiple PRs

#### Reviewing Pull Requests

**As a Reviewer:**

```bash
# Fetch the PR branch locally
git fetch origin
git checkout -b pr-review origin/feature-branch

# Review changes
git diff main...feature-branch

# Test the changes locally
npm install
npm test
npm start

# Leave feedback on GitHub, then:
git checkout main
git branch -d pr-review
```

**Review Checklist:**
- [ ] Code is readable and maintainable
- [ ] Tests cover new functionality
- [ ] No obvious bugs or security issues
- [ ] Follows project conventions
- [ ] Documentation is updated
- [ ] Commit messages are clear

**Review Comments Examples:**

Good feedback:
```
✅ "Consider extracting this logic into a separate function for reusability"
✅ "This could be simplified using array.filter() instead of a loop"
✅ "Great improvement! One suggestion: add error handling here"
```

Avoid:
```
❌ "This is wrong"
❌ "Why didn't you do X?"
❌ "I would have done this differently"
```

#### Responding to Review Comments

```bash
# Make requested changes
# ... edit files ...
git add .
git commit -m "Address review comments"
git push origin your-feature-branch

# If you need to update commit history
git rebase -i HEAD~3  # Interactive rebase last 3 commits
# Edit, squash, or reword commits
git push --force-with-lease origin your-feature-branch
```

#### Merging Strategies

**1. Merge Commit (Default)**
```bash
git checkout main
git merge feature-branch
# Creates a merge commit
```
- Preserves complete history
- Shows when features were integrated
- Can clutter history

**2. Squash and Merge**
```bash
git checkout main
git merge --squash feature-branch
git commit -m "Add feature: user authentication"
```
- Combines all commits into one
- Clean main branch history
- Loses detailed commit history

**3. Rebase and Merge**
```bash
git checkout feature-branch
git rebase main
git checkout main
git merge feature-branch  # Fast-forward merge
```
- Linear history
- No merge commits
- Can rewrite history (requires force push)

**Recommendation for most teams:** Squash and Merge for feature branches, keeping main branch clean.

---

### 🎓 Part 2 Practice Exercise

**Scenario:** You're working on a data engineering team. Practice the feature branch workflow:

```bash
# 1. Setup: Fork or create a repository
mkdir data-pipeline
cd data-pipeline
git init
echo "# Data Pipeline Project" > README.md
git add README.md
git commit -m "Initial commit"

# Simulate remote
# (In real scenario, create repo on GitHub)

# 2. Create a feature branch
git checkout -b feature/add-etl-script

# 3. Create an ETL script
cat > etl.py << 'EOF'
def extract():
    """Extract data from source"""
    print("Extracting data...")
    return {"users": 100}

def transform(data):
    """Transform data"""
    print("Transforming data...")
    data["processed"] = True
    return data

def load(data):
    """Load data to destination"""
    print("Loading data...")
    print(f"Loaded: {data}")

if __name__ == "__main__":
    data = extract()
    data = transform(data)
    load(data)
EOF

git add etl.py
git commit -m "Add basic ETL script"

# 4. Add tests
cat > test_etl.py << 'EOF'
import etl

def test_extract():
    data = etl.extract()
    assert "users" in data

def test_transform():
    data = {"users": 100}
    result = etl.transform(data)
    assert result["processed"] == True
EOF

git add test_etl.py
git commit -m "Add unit tests for ETL"

# 5. Update documentation
cat >> README.md << 'EOF'

## ETL Pipeline

### Usage
```python
python etl.py
```

### Testing
```bash
pytest test_etl.py
```
EOF

git add README.md
git commit -m "Update documentation"

# 6. Review your changes
git log --oneline feature/add-etl-script

# 7. Merge to main (simulate approved PR)
git checkout main
git merge feature/add-etl-script

# 8. Clean up
git branch -d feature/add-etl-script
```

---

## Part 3: CI/CD with Snowflake

Now let's build a real CI/CD pipeline that deploys a table to Snowflake using GitHub Actions!

### Project Overview

We'll create a data pipeline that:
1. Defines a Snowflake table schema in SQL
2. Uses GitHub Actions for CI/CD
3. Automatically deploys to Snowflake on merge to main
4. Includes data quality tests

---

### Prerequisites

Before starting, ensure you have:

1. **GitHub Account** - [Sign up at github.com](https://github.com)
2. **Snowflake Account** - [Free trial available](https://signup.snowflake.com)
3. **Git installed locally**

---

### Project Setup

#### Step 1: Create Snowflake Resources

First, let's set up the Snowflake environment:

```sql
-- Login to Snowflake and run these commands in a worksheet

-- 1. Create a database for our project
CREATE DATABASE IF NOT EXISTS CICD_DEMO;

-- 2. Create schemas for different environments
CREATE SCHEMA IF NOT EXISTS CICD_DEMO.DEV;
CREATE SCHEMA IF NOT EXISTS CICD_DEMO.PROD;

-- 3. Create a warehouse
CREATE WAREHOUSE IF NOT EXISTS CICD_WH
  WAREHOUSE_SIZE = 'XSMALL'
  AUTO_SUSPEND = 60
  AUTO_RESUME = TRUE;

-- 4. Create a service user for CI/CD
CREATE USER IF NOT EXISTS CICD_USER
  PASSWORD = 'YourStrongPassword123!'
  DEFAULT_WAREHOUSE = CICD_WH
  DEFAULT_NAMESPACE = CICD_DEMO.DEV;

-- 5. Create a role for CI/CD
CREATE ROLE IF NOT EXISTS CICD_ROLE;

-- 6. Grant permissions
GRANT USAGE ON DATABASE CICD_DEMO TO ROLE CICD_ROLE;
GRANT USAGE ON SCHEMA CICD_DEMO.DEV TO ROLE CICD_ROLE;
GRANT USAGE ON SCHEMA CICD_DEMO.PROD TO ROLE CICD_ROLE;
GRANT CREATE TABLE ON SCHEMA CICD_DEMO.DEV TO ROLE CICD_ROLE;
GRANT CREATE TABLE ON SCHEMA CICD_DEMO.PROD TO ROLE CICD_ROLE;
GRANT USAGE ON WAREHOUSE CICD_WH TO ROLE CICD_ROLE;

-- 7. Assign role to user
GRANT ROLE CICD_ROLE TO USER CICD_USER;

-- 8. Verify setup
SHOW DATABASES;
SHOW SCHEMAS IN DATABASE CICD_DEMO;
```

#### Step 2: Create GitHub Repository

```bash
# Create project directory
mkdir snowflake-cicd-demo
cd snowflake-cicd-demo

# Initialize Git
git init

# Create .gitignore
cat > .gitignore << 'EOF'
# Python
__pycache__/
*.py[cod]
*$py.class
*.so
.Python
venv/
ENV/
.env

# IDE
.vscode/
.idea/
*.swp
*.swo

# OS
.DS_Store
Thumbs.db

# Snowflake
*_rsa_key*
EOF

git add .gitignore
git commit -m "Add .gitignore"
```

Now create a repository on GitHub:
1. Go to github.com and click "New Repository"
2. Name it: `snowflake-cicd-demo`
3. Don't initialize with README (we already have files)
4. Click "Create Repository"

```bash
# Link local repo to GitHub
git remote add origin https://github.com/YOUR_USERNAME/snowflake-cicd-demo.git
git branch -M main
git push -u origin main
```

---

### Step 3: Create Project Structure

```bash
# Create directory structure
mkdir -p sql/tables
mkdir -p sql/data
mkdir -p tests
mkdir -p .github/workflows

# Create SQL files
cat > sql/tables/customers.sql << 'EOF'
-- Customer dimension table
CREATE OR REPLACE TABLE CUSTOMERS (
    CUSTOMER_ID INTEGER AUTOINCREMENT PRIMARY KEY,
    CUSTOMER_NAME VARCHAR(100) NOT NULL,
    EMAIL VARCHAR(255) UNIQUE NOT NULL,
    PHONE VARCHAR(20),
    COUNTRY VARCHAR(50),
    CREATED_AT TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP(),
    UPDATED_AT TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP()
);

-- Add comment
COMMENT ON TABLE CUSTOMERS IS 'Customer master data table';
EOF

cat > sql/tables/orders.sql << 'EOF'
-- Orders fact table
CREATE OR REPLACE TABLE ORDERS (
    ORDER_ID INTEGER AUTOINCREMENT PRIMARY KEY,
    CUSTOMER_ID INTEGER NOT NULL,
    ORDER_DATE DATE NOT NULL,
    ORDER_AMOUNT DECIMAL(10,2) NOT NULL,
    ORDER_STATUS VARCHAR(20) NOT NULL,
    CREATED_AT TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP(),
    FOREIGN KEY (CUSTOMER_ID) REFERENCES CUSTOMERS(CUSTOMER_ID)
);

-- Add comments
COMMENT ON TABLE ORDERS IS 'Orders fact table';
COMMENT ON COLUMN ORDERS.ORDER_STATUS IS 'Values: PENDING, PROCESSING, COMPLETED, CANCELLED';
EOF

# Create sample data
cat > sql/data/sample_customers.sql << 'EOF'
-- Insert sample customer data
INSERT INTO CUSTOMERS (CUSTOMER_NAME, EMAIL, PHONE, COUNTRY)
VALUES 
    ('Alice Martin', 'alice.martin@email.com', '+33-1-2345-6789', 'France'),
    ('Bob Smith', 'bob.smith@email.com', '+44-20-1234-5678', 'UK'),
    ('Claire Dubois', 'claire.dubois@email.com', '+33-4-8765-4321', 'France'),
    ('David Johnson', 'david.johnson@email.com', '+1-555-0123', 'USA'),
    ('Emma Wilson', 'emma.wilson@email.com', '+44-20-8765-4321', 'UK');
EOF

cat > sql/data/sample_orders.sql << 'EOF'
-- Insert sample order data
INSERT INTO ORDERS (CUSTOMER_ID, ORDER_DATE, ORDER_AMOUNT, ORDER_STATUS)
SELECT 
    CUSTOMER_ID,
    DATEADD(day, -UNIFORM(1, 30, RANDOM()), CURRENT_DATE()) AS ORDER_DATE,
    UNIFORM(10, 1000, RANDOM()) AS ORDER_AMOUNT,
    CASE UNIFORM(1, 4, RANDOM())
        WHEN 1 THEN 'PENDING'
        WHEN 2 THEN 'PROCESSING'
        WHEN 3 THEN 'COMPLETED'
        ELSE 'CANCELLED'
    END AS ORDER_STATUS
FROM CUSTOMERS
CROSS JOIN TABLE(GENERATOR(ROWCOUNT => 3));
EOF
```

---

### Step 4: Create Python Deployment Script

```bash
# Create requirements.txt
cat > requirements.txt << 'EOF'
snowflake-connector-python==3.7.0
python-dotenv==1.0.1
pytest==8.0.0
EOF

# Create deployment script
cat > deploy.py << 'EOF'
#!/usr/bin/env python3
"""
Snowflake deployment script for CI/CD pipeline
"""
import os
import sys
import snowflake.connector
from pathlib import Path
from dotenv import load_dotenv

# Load environment variables
load_dotenv()

def get_snowflake_connection():
    """Create Snowflake connection using environment variables"""
    try:
        conn = snowflake.connector.connect(
            account=os.getenv('SNOWFLAKE_ACCOUNT'),
            user=os.getenv('SNOWFLAKE_USER'),
            password=os.getenv('SNOWFLAKE_PASSWORD'),
            warehouse=os.getenv('SNOWFLAKE_WAREHOUSE'),
            database=os.getenv('SNOWFLAKE_DATABASE'),
            schema=os.getenv('SNOWFLAKE_SCHEMA'),
            role=os.getenv('SNOWFLAKE_ROLE')
        )
        print("✓ Successfully connected to Snowflake")
        return conn
    except Exception as e:
        print(f"✗ Failed to connect to Snowflake: {e}")
        sys.exit(1)

def execute_sql_file(cursor, file_path):
    """Execute SQL from a file"""
    try:
        with open(file_path, 'r') as f:
            sql_content = f.read()
        
        # Split by semicolon and execute each statement
        statements = [s.strip() for s in sql_content.split(';') if s.strip()]
        
        for statement in statements:
            cursor.execute(statement)
        
        print(f"✓ Executed: {file_path.name}")
        return True
    except Exception as e:
        print(f"✗ Error executing {file_path.name}: {e}")
        return False

def deploy_tables(cursor, tables_dir):
    """Deploy all table definitions"""
    print("\n=== Deploying Tables ===")
    tables_dir = Path(tables_dir)
    sql_files = sorted(tables_dir.glob('*.sql'))
    
    success_count = 0
    for sql_file in sql_files:
        if execute_sql_file(cursor, sql_file):
            success_count += 1
    
    print(f"Deployed {success_count}/{len(sql_files)} tables")
    return success_count == len(sql_files)

def load_sample_data(cursor, data_dir, load_data=False):
    """Load sample data if requested"""
    if not load_data:
        print("\n=== Skipping sample data load ===")
        return True
    
    print("\n=== Loading Sample Data ===")
    data_dir = Path(data_dir)
    sql_files = sorted(data_dir.glob('*.sql'))
    
    success_count = 0
    for sql_file in sql_files:
        if execute_sql_file(cursor, sql_file):
            success_count += 1
    
    print(f"Loaded {success_count}/{len(sql_files)} data files")
    return success_count == len(sql_files)

def verify_deployment(cursor):
    """Verify tables were created successfully"""
    print("\n=== Verifying Deployment ===")
    
    try:
        cursor.execute("SHOW TABLES")
        tables = cursor.fetchall()
        
        print(f"Found {len(tables)} tables:")
        for table in tables:
            print(f"  - {table[1]}")  # table name is in second column
        
        return len(tables) > 0
    except Exception as e:
        print(f"✗ Verification failed: {e}")
        return False

def main():
    """Main deployment function"""
    print("=" * 50)
    print("Snowflake CI/CD Deployment")
    print("=" * 50)
    
    # Check required environment variables
    required_vars = [
        'SNOWFLAKE_ACCOUNT',
        'SNOWFLAKE_USER',
        'SNOWFLAKE_PASSWORD',
        'SNOWFLAKE_WAREHOUSE',
        'SNOWFLAKE_DATABASE',
        'SNOWFLAKE_SCHEMA'
    ]
    
    missing_vars = [var for var in required_vars if not os.getenv(var)]
    if missing_vars:
        print(f"✗ Missing required environment variables: {', '.join(missing_vars)}")
        sys.exit(1)
    
    # Get optional parameters
    load_data = os.getenv('LOAD_SAMPLE_DATA', 'false').lower() == 'true'
    
    # Connect to Snowflake
    conn = get_snowflake_connection()
    cursor = conn.cursor()
    
    try:
        # Deploy tables
        if not deploy_tables(cursor, 'sql/tables'):
            print("\n✗ Table deployment failed")
            sys.exit(1)
        
        # Load sample data if requested
        if not load_sample_data(cursor, 'sql/data', load_data):
            print("\n✗ Data loading failed")
            sys.exit(1)
        
        # Verify deployment
        if not verify_deployment(cursor):
            print("\n✗ Verification failed")
            sys.exit(1)
        
        print("\n" + "=" * 50)
        print("✓ Deployment completed successfully!")
        print("=" * 50)
        
    finally:
        cursor.close()
        conn.close()

if __name__ == "__main__":
    main()
EOF

chmod +x deploy.py
```

---

### Step 5: Create Tests

```bash
cat > tests/test_deployment.py << 'EOF'
"""
Tests for Snowflake deployment
"""
import os
import pytest
import snowflake.connector
from dotenv import load_dotenv

load_dotenv()

@pytest.fixture
def snowflake_connection():
    """Create Snowflake connection for testing"""
    conn = snowflake.connector.connect(
        account=os.getenv('SNOWFLAKE_ACCOUNT'),
        user=os.getenv('SNOWFLAKE_USER'),
        password=os.getenv('SNOWFLAKE_PASSWORD'),
        warehouse=os.getenv('SNOWFLAKE_WAREHOUSE'),
        database=os.getenv('SNOWFLAKE_DATABASE'),
        schema=os.getenv('SNOWFLAKE_SCHEMA'),
        role=os.getenv('SNOWFLAKE_ROLE')
    )
    yield conn
    conn.close()

def test_customers_table_exists(snowflake_connection):
    """Test that CUSTOMERS table exists"""
    cursor = snowflake_connection.cursor()
    cursor.execute("SHOW TABLES LIKE 'CUSTOMERS'")
    result = cursor.fetchone()
    assert result is not None, "CUSTOMERS table does not exist"
    cursor.close()

def test_orders_table_exists(snowflake_connection):
    """Test that ORDERS table exists"""
    cursor = snowflake_connection.cursor()
    cursor.execute("SHOW TABLES LIKE 'ORDERS'")
    result = cursor.fetchone()
    assert result is not None, "ORDERS table does not exist"
    cursor.close()

def test_customers_table_structure(snowflake_connection):
    """Test CUSTOMERS table has expected columns"""
    cursor = snowflake_connection.cursor()
    cursor.execute("DESCRIBE TABLE CUSTOMERS")
    columns = {row[0] for row in cursor.fetchall()}
    
    expected_columns = {
        'CUSTOMER_ID', 'CUSTOMER_NAME', 'EMAIL', 
        'PHONE', 'COUNTRY', 'CREATED_AT', 'UPDATED_AT'
    }
    
    assert expected_columns.issubset(columns), \
        f"Missing columns: {expected_columns - columns}"
    cursor.close()

def test_orders_table_structure(snowflake_connection):
    """Test ORDERS table has expected columns"""
    cursor = snowflake_connection.cursor()
    cursor.execute("DESCRIBE TABLE ORDERS")
    columns = {row[0] for row in cursor.fetchall()}
    
    expected_columns = {
        'ORDER_ID', 'CUSTOMER_ID', 'ORDER_DATE',
        'ORDER_AMOUNT', 'ORDER_STATUS', 'CREATED_AT'
    }
    
    assert expected_columns.issubset(columns), \
        f"Missing columns: {expected_columns - columns}"
    cursor.close()

def test_sample_data_loaded(snowflake_connection):
    """Test that sample data exists (if loaded)"""
    cursor = snowflake_connection.cursor()
    
    # Check customers
    cursor.execute("SELECT COUNT(*) FROM CUSTOMERS")
    customer_count = cursor.fetchone()[0]
    
    # Only test if data was loaded
    if customer_count > 0:
        assert customer_count >= 5, "Expected at least 5 customers"
        
        # Check orders
        cursor.execute("SELECT COUNT(*) FROM ORDERS")
        order_count = cursor.fetchone()[0]
        assert order_count > 0, "Expected orders data"
    
    cursor.close()

def test_foreign_key_constraint(snowflake_connection):
    """Test foreign key relationship between tables"""
    cursor = snowflake_connection.cursor()
    
    # This should fail if foreign key is properly enforced
    # (In Snowflake, FK is informational, so we test the relationship exists)
    cursor.execute("""
        SELECT COUNT(*) 
        FROM ORDERS o
        LEFT JOIN CUSTOMERS c ON o.CUSTOMER_ID = c.CUSTOMER_ID
        WHERE c.CUSTOMER_ID IS NULL
    """)
    
    orphaned_orders = cursor.fetchone()[0]
    assert orphaned_orders == 0, f"Found {orphaned_orders} orders without customers"
    
    cursor.close()
EOF

# Create pytest configuration
cat > pytest.ini << 'EOF'
[pytest]
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
EOF
```

---

### Step 6: GitHub Actions Configuration

```bash
# Create GitHub Actions workflow
cat > .github/workflows/deploy.yml << 'EOF'
name: Deploy to Snowflake

on:
  push:
    branches:
      - main
      - develop
  pull_request:
    branches:
      - main

env:
  PYTHON_VERSION: '3.11'

jobs:
  validate:
    name: Validate SQL
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Check SQL files syntax
        run: |
          echo "Validating SQL files..."
          for file in sql/tables/*.sql sql/data/*.sql; do
            if [ -f "$file" ]; then
              echo "Checking $file"
              # Basic syntax check (looking for obvious errors)
              if grep -q "CREATE\|INSERT\|SELECT" "$file"; then
                echo "✓ $file looks valid"
              else
                echo "✗ $file might be invalid"
                exit 1
              fi
            fi
          done

  deploy-dev:
    name: Deploy to DEV
    runs-on: ubuntu-latest
    needs: validate
    if: github.event_name == 'push' && github.ref == 'refs/heads/develop'
    environment: development
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}
      
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      
      - name: Deploy to Snowflake DEV
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: DEV
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
          LOAD_SAMPLE_DATA: 'true'
        run: |
          python deploy.py
      
      - name: Run tests
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: DEV
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
        run: |
          pytest tests/ -v

  deploy-prod:
    name: Deploy to PROD
    runs-on: ubuntu-latest
    needs: validate
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    environment: production
    
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: ${{ env.PYTHON_VERSION }}
      
      - name: Install dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt
      
      - name: Deploy to Snowflake PROD
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: PROD
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
          LOAD_SAMPLE_DATA: 'false'
        run: |
          python deploy.py
      
      - name: Run tests
        env:
          SNOWFLAKE_ACCOUNT: ${{ secrets.SNOWFLAKE_ACCOUNT }}
          SNOWFLAKE_USER: ${{ secrets.SNOWFLAKE_USER }}
          SNOWFLAKE_PASSWORD: ${{ secrets.SNOWFLAKE_PASSWORD }}
          SNOWFLAKE_WAREHOUSE: ${{ secrets.SNOWFLAKE_WAREHOUSE }}
          SNOWFLAKE_DATABASE: ${{ secrets.SNOWFLAKE_DATABASE }}
          SNOWFLAKE_SCHEMA: PROD
          SNOWFLAKE_ROLE: ${{ secrets.SNOWFLAKE_ROLE }}
        run: |
          pytest tests/ -v
      
      - name: Notify success
        if: success()
        run: |
          echo "🎉 Production deployment successful!"
EOF
```

---

### Step 7: Create README

```bash
cat > README.md << 'EOF'
# Snowflake CI/CD Demo

Automated deployment pipeline for Snowflake tables using GitHub Actions.

## Architecture

```
┌─────────────┐     ┌──────────────┐     ┌────────────────┐
│   Developer │────▶│    GitHub    │────▶│   Snowflake    │
│             │     │    Actions   │     │   DEV/PROD     │
└─────────────┘     └──────────────┘     └────────────────┘
```

## Project Structure

```
snowflake-cicd-demo/
├── .github/
│   └── workflows/
│       └── deploy.yml          # CI/CD pipeline
├── sql/
│   ├── tables/
│   │   ├── customers.sql       # Customer table DDL
│   │   └── orders.sql          # Orders table DDL
│   └── data/
│       ├── sample_customers.sql
│       └── sample_orders.sql
├── tests/
│   └── test_deployment.py      # Automated tests
├── deploy.py                   # Deployment script
├── requirements.txt
└── README.md
```

## Setup Instructions

### 1. Configure GitHub Secrets

Go to your GitHub repository → Settings → Secrets and variables → Actions → New repository secret

Add the following secrets:
- `SNOWFLAKE_ACCOUNT` - Your Snowflake account identifier
- `SNOWFLAKE_USER` - CI/CD service user
- `SNOWFLAKE_PASSWORD` - Service user password
- `SNOWFLAKE_WAREHOUSE` - Warehouse name
- `SNOWFLAKE_DATABASE` - Database name
- `SNOWFLAKE_ROLE` - Role name

### 2. Local Development

```bash
# Clone repository
git clone https://github.com/YOUR_USERNAME/snowflake-cicd-demo.git
cd snowflake-cicd-demo

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Create .env file for local testing
cat > .env << 'EOL'
SNOWFLAKE_ACCOUNT=your_account
SNOWFLAKE_USER=your_user
SNOWFLAKE_PASSWORD=your_password
SNOWFLAKE_WAREHOUSE=CICD_WH
SNOWFLAKE_DATABASE=CICD_DEMO
SNOWFLAKE_SCHEMA=DEV
SNOWFLAKE_ROLE=CICD_ROLE
LOAD_SAMPLE_DATA=true
EOL

# Run deployment locally
python deploy.py

# Run tests
pytest tests/ -v
```

### 3. Deployment Workflow

**DEV Environment:**
1. Create feature branch: `git checkout -b feature/my-feature`
2. Make changes to SQL files
3. Commit and push: `git push origin feature/my-feature`
4. Create PR to `develop` branch
5. Merge to `develop` → Automatically deploys to DEV

**PROD Environment:**
1. Create PR from `develop` to `main`
2. Review and approve PR
3. Merge to `main` → Automatically deploys to PROD

## CI/CD Pipeline

The GitHub Actions pipeline includes:

1. **Validate** - SQL syntax validation
2. **Deploy to DEV** - Deploy to development environment (on develop branch)
3. **Deploy to PROD** - Deploy to production environment (on main branch)
4. **Test** - Run automated tests after deployment

## Tables

### CUSTOMERS
- Customer master data
- Primary key: CUSTOMER_ID
- Contains: name, email, phone, country

### ORDERS
- Orders fact table
- Primary key: ORDER_ID
- Foreign key: CUSTOMER_ID → CUSTOMERS
- Contains: order date, amount, status

## Testing

Tests include:
- Table existence verification
- Column structure validation
- Data integrity checks
- Foreign key relationship validation

```bash
pytest tests/ -v --tb=short
```

## Troubleshooting

**Connection Issues:**
- Verify Snowflake account identifier format
- Check user permissions in Snowflake
- Ensure warehouse is running

**Deployment Failures:**
- Check GitHub Actions logs
- Verify all secrets are configured
- Test SQL files locally first

## Contributing

1. Fork the repository
2. Create feature branch
3. Make changes and test locally
4. Submit pull request to `develop`

## License

MIT License
EOF
```

---

### Step 8: Commit and Push Everything

```bash
# Check status
git status

# Add all files
git add .

# Commit
git commit -m "Add Snowflake CI/CD pipeline with GitHub Actions

- Add customer and orders table definitions
- Create deployment script with Python
- Add automated tests with pytest
- Configure GitHub Actions for DEV and PROD deployments
- Include sample data for development"

# Push to GitHub
git push origin main
```

---

### Step 9: Configure GitHub Secrets

Now configure your GitHub repository secrets:

1. Go to your GitHub repository
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Add each of these secrets:

| Secret Name | Value |
|------------|-------|
| `SNOWFLAKE_ACCOUNT` | `your_account.region` (e.g., `xy12345.eu-west-1`) |
| `SNOWFLAKE_USER` | `CICD_USER` |
| `SNOWFLAKE_PASSWORD` | `YourStrongPassword123!` |
| `SNOWFLAKE_WAREHOUSE` | `CICD_WH` |
| `SNOWFLAKE_DATABASE` | `CICD_DEMO` |
| `SNOWFLAKE_ROLE` | `CICD_ROLE` |

**Finding your Snowflake Account:**
```sql
-- Run in Snowflake worksheet
SELECT CURRENT_ACCOUNT();
SELECT CURRENT_REGION();

-- Your account identifier format: <account>.<region>
-- Example: xy12345.eu-west-1
```

---

### Testing the Pipeline

#### Test 1: Manual Trigger

```bash
# Trigger deployment by pushing to main
git commit --allow-empty -m "Trigger deployment"
git push origin main
```

Go to your GitHub repository → **Actions** tab to watch the deployment.

#### Test 2: Feature Branch Workflow

```bash
# Create develop branch
git checkout -b develop
git push origin develop

# Create feature branch
git checkout -b feature/add-product-table

# Add new table
cat > sql/tables/products.sql << 'EOF'
-- Products table
CREATE OR REPLACE TABLE PRODUCTS (
    PRODUCT_ID INTEGER AUTOINCREMENT PRIMARY KEY,
    PRODUCT_NAME VARCHAR(200) NOT NULL,
    CATEGORY VARCHAR(50),
    PRICE DECIMAL(10,2) NOT NULL,
    STOCK_QUANTITY INTEGER DEFAULT 0,
    CREATED_AT TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP()
);

COMMENT ON TABLE PRODUCTS IS 'Product catalog table';
EOF

# Commit and push
git add sql/tables/products.sql
git commit -m "Add products table"
git push origin feature/add-product-table
```

1. Go to GitHub and create a Pull Request from `feature/add-product-table` to `develop`
2. Review the PR
3. Merge it → This deploys to DEV environment
4. Create another PR from `develop` to `main`
5. Merge it → This deploys to PROD environment

---

### Step 10: Verify Deployment in Snowflake

```sql
-- Switch to DEV schema
USE SCHEMA CICD_DEMO.DEV;

-- Check deployed tables
SHOW TABLES;

-- Verify data
SELECT * FROM CUSTOMERS LIMIT 5;
SELECT * FROM ORDERS LIMIT 10;

-- Check table details
DESCRIBE TABLE CUSTOMERS;
DESCRIBE TABLE ORDERS;

-- Verify relationships
SELECT 
    c.CUSTOMER_NAME,
    COUNT(o.ORDER_ID) as TOTAL_ORDERS,
    SUM(o.ORDER_AMOUNT) as TOTAL_SPENT
FROM CUSTOMERS c
LEFT JOIN ORDERS o ON c.CUSTOMER_ID = o.CUSTOMER_ID
GROUP BY c.CUSTOMER_NAME
ORDER BY TOTAL_SPENT DESC;

-- Switch to PROD and verify
USE SCHEMA CICD_DEMO.PROD;
SHOW TABLES;
```

---

### Advanced Enhancements

#### 1. Add Slack Notifications

```yaml
# Add to .github/workflows/deploy.yml after deploy-prod job

- name: Notify Slack
  if: always()
  uses: slackapi/slack-github-action@v1.24.0
  with:
    webhook: ${{ secrets.SLACK_WEBHOOK_URL }}
    payload: |
      {
        "text": "Snowflake Deployment ${{ job.status }}",
        "blocks": [
          {
            "type": "section",
            "text": {
              "type": "mrkdwn",
              "text": "*Snowflake PROD Deployment*\nStatus: ${{ job.status }}\nCommit: ${{ github.sha }}\nAuthor: ${{ github.actor }}"
            }
          }
        ]
      }
```

#### 2. Add Data Quality Checks

```python
# Add to tests/test_deployment.py

def test_data_quality_no_duplicates(snowflake_connection):
    """Test that emails are unique in CUSTOMERS"""
    cursor = snowflake_connection.cursor()
    cursor.execute("""
        SELECT EMAIL, COUNT(*) as cnt
        FROM CUSTOMERS
        GROUP BY EMAIL
        HAVING COUNT(*) > 1
    """)
    duplicates = cursor.fetchall()
    assert len(duplicates) == 0, f"Found duplicate emails: {duplicates}"
    cursor.close()

def test_data_quality_valid_status(snowflake_connection):
    """Test that order statuses are valid"""
    cursor = snowflake_connection.cursor()
    cursor.execute("""
        SELECT DISTINCT ORDER_STATUS
        FROM ORDERS
        WHERE ORDER_STATUS NOT IN ('PENDING', 'PROCESSING', 'COMPLETED', 'CANCELLED')
    """)
    invalid_statuses = cursor.fetchall()
    assert len(invalid_statuses) == 0, f"Found invalid statuses: {invalid_statuses}"
    cursor.close()
```

#### 3. Add Rollback Capability

```python
# Create rollback.py

#!/usr/bin/env python3
"""Rollback script for Snowflake deployments"""

import os
import snowflake.connector
from dotenv import load_dotenv

load_dotenv()

def rollback():
    conn = snowflake.connector.connect(
        account=os.getenv('SNOWFLAKE_ACCOUNT'),
        user=os.getenv('SNOWFLAKE_USER'),
        password=os.getenv('SNOWFLAKE_PASSWORD'),
        warehouse=os.getenv('SNOWFLAKE_WAREHOUSE'),
        database=os.getenv('SNOWFLAKE_DATABASE'),
        schema=os.getenv('SNOWFLAKE_SCHEMA')
    )
    
    cursor = conn.cursor()
    
    # Example: Restore from backup
    cursor.execute("""
        CREATE OR REPLACE TABLE CUSTOMERS CLONE CUSTOMERS_BACKUP;
    """)
    
    cursor.execute("""
        CREATE OR REPLACE TABLE ORDERS CLONE ORDERS_BACKUP;
    """)
    
    print("✓ Rollback completed")
    
    cursor.close()
    conn.close()

if __name__ == "__main__":
    rollback()
```

#### 4. Add Schema Version Control

```python
# Create migrations/001_initial_schema.sql

-- Migration: 001_initial_schema
-- Description: Create initial customers and orders tables
-- Date: 2024-01-15

CREATE TABLE IF NOT EXISTS SCHEMA_VERSIONS (
    VERSION INTEGER PRIMARY KEY,
    DESCRIPTION VARCHAR(500),
    APPLIED_AT TIMESTAMP_NTZ DEFAULT CURRENT_TIMESTAMP()
);

INSERT INTO SCHEMA_VERSIONS (VERSION, DESCRIPTION)
VALUES (1, 'Initial schema with customers and orders');
```

---

### Common Issues and Solutions

#### Issue 1: Authentication Fails

```bash
# Check your Snowflake account format
# Should be: account.region.cloud
# Example: xy12345.eu-west-1.aws
```

#### Issue 2: Permission Denied

```sql
-- Run in Snowflake as ACCOUNTADMIN
GRANT USAGE ON DATABASE CICD_DEMO TO ROLE CICD_ROLE;
GRANT ALL ON SCHEMA CICD_DEMO.DEV TO ROLE CICD_ROLE;
GRANT ALL ON SCHEMA CICD_DEMO.PROD TO ROLE CICD_ROLE;
```

#### Issue 3: Pipeline Fails

1. Check GitHub Actions logs
2. Verify secrets are set correctly
3. Test deployment script locally first:

```bash
# Create .env file with your credentials
python deploy.py
```

---

## Summary

### What We Built

1. ✅ **Git Basics** - Essential commands and workflows
2. ✅ **Git Workflows** - Feature branch workflow, Gitflow, GitHub Flow
3. ✅ **Pull Requests** - Best practices for collaboration
4. ✅ **CI/CD Pipeline** - Automated Snowflake deployments
5. ✅ **Testing** - Automated validation of deployments
6. ✅ **Multi-Environment** - DEV and PROD environments

### Key Takeaways

- **Git** is essential for modern software development
- **Workflows** prevent conflicts and enable collaboration
- **Pull Requests** facilitate code review and quality
- **CI/CD** automates deployments and reduces errors
- **Testing** ensures quality before production

### Next Steps

1. **Practice** these commands daily
2. **Contribute** to open source projects
3. **Explore** advanced Git features (rebase, cherry-pick, bisect)
4. **Automate** more with CI/CD (testing, monitoring, rollbacks)
5. **Learn** infrastructure as code (Terraform for Snowflake)

---

## Resources

### Git Learning
- [Git Official Documentation](https://git-scm.com/doc)
- [Pro Git Book](https://git-scm.com/book/en/v2) (Free)
- [GitHub Learning Lab](https://lab.github.com/)

### GitHub Actions
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Workflow Syntax](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions)

### Snowflake
- [Snowflake Documentation](https://docs.snowflake.com/)
- [Python Connector](https://docs.snowflake.com/en/user-guide/python-connector.html)

### Best Practices
- [Git Commit Best Practices](https://www.conventionalcommits.org/)
- [Trunk-Based Development](https://trunkbaseddevelopment.com/)
- [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

---

## Questions?

Feel free to open an issue in this repository for questions or suggestions!

---

**Workshop Complete! 🎉**

You now have a solid foundation in Git, collaborative workflows, and CI/CD with Snowflake. Keep practicing and building!
