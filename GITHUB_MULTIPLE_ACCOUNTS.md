# Managing Multiple GitHub Accounts

## Method 1: SSH Keys (Recommended)

### Step 1: Generate SSH Keys

```bash
# For Account 1 (e.g., personal)
ssh-keygen -t ed25519 -C "personal@example.com" -f ~/.ssh/id_ed25519_personal

# For Account 2 (e.g., work)
ssh-keygen -t ed25519 -C "work@example.com" -f ~/.ssh/id_ed25519_work
```

### Step 2: Add Keys to SSH Agent

```bash
# Start ssh-agent (if not running)
eval "$(ssh-agent -s)"

# Add both keys
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_work
```

### Step 3: Create SSH Config File

Create/edit `~/.ssh/config`:

```
# Personal GitHub Account
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal

# Work GitHub Account
Host github.com-work
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_work
```

### Step 4: Add SSH Keys to GitHub

```bash
# Copy personal key (paste in GitHub Settings → SSH Keys)
cat ~/.ssh/id_ed25519_personal.pub

# Copy work key (paste in your work GitHub account)
cat ~/.ssh/id_ed25519_work.pub
```

### Step 5: Clone/Push Using Different Accounts

```bash
# For PERSONAL account repos:
git clone git@github.com-personal:username/repo.git

# For WORK account repos:
git clone git@github.com-work:username/repo.git

# Or update existing repo remote:
git remote set-url origin git@github.com-personal:username/repo.git
```

### Step 6: Configure Git User Per Repo

```bash
# In each repository, set the correct user:
cd your-personal-repo
git config user.name "Your Personal Name"
git config user.email "personal@example.com"

cd your-work-repo
git config user.name "Your Work Name"
git config user.email "work@example.com"
```

---

## Method 2: HTTPS with Credential Manager

### For Windows (Git Credential Manager)

```bash
# In each repository:
cd your-repo

# Set user for this repo
git config user.name "Your Name"
git config user.email "your-email@example.com"

# Remove cached credentials
git credential-manager erase https://github.com

# Next push will ask for credentials
git push
```

### Using Personal Access Tokens

1. Generate PAT in GitHub: Settings → Developer settings → Personal access tokens
2. Use PAT as password when prompted
3. Store different tokens for different accounts

---

## Method 3: GitHub CLI (gh)

```bash
# Login to first account
gh auth login

# Switch accounts
gh auth logout
gh auth login  # Login with different account

# Or manage multiple accounts
gh auth status  # Check current account
```

---

## Quick Switching Commands

### Check Current Config
```bash
git config user.name
git config user.email
git config --list
```

### Switch Account for Current Repo
```bash
# Switch to personal
git config user.name "Personal Name"
git config user.email "personal@example.com"
git remote set-url origin git@github.com-personal:username/repo.git

# Switch to work
git config user.name "Work Name"
git config user.email "work@example.com"
git remote set-url origin git@github.com-work:username/repo.git
```

---

## Troubleshooting

### Test SSH Connection
```bash
ssh -T git@github.com-personal
ssh -T git@github.com-work
```

### List Stored Credentials (Windows)
```powershell
cmdkey /list | findstr github
```

### Remove Specific Credential (Windows)
```powershell
cmdkey /delete:git:https://github.com
```

### View Remote URL
```bash
git remote -v
```

---

## Best Practices

1. **Use SSH keys** - More secure and easier to manage
2. **Set user per repository** - Never set global config if using multiple accounts
3. **Use meaningful Host names** in SSH config (e.g., `github.com-personal`, `github.com-work`)
4. **Keep keys organized** - Use descriptive filenames
5. **Use different emails** - Helps track commits per account

---

## For Your Current Repo (landingiron)

```bash
# Set which account to use:
git config user.name "Your Name"
git config user.email "your-github-email@example.com"

# Update remote (if using SSH):
git remote set-url origin git@github.com-personal:yourusername/landingiron.git

# Then push:
git add .
git commit -m "Add IronUp landing page"
git push
```

