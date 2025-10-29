# Using MyLogger (as example) as a Private Package import use.

## 🎯 Overview

You want to use your custom logger across multiple projects without publishing to PyPI. Here are several methods to accomplish this, from simplest to most sophisticated.

---

## Method 1: Local Pip Install (Simplest) ⭐

**Best for:** Most use cases, easy to manage and update

### Setup Once

```bash
# 1. Navigate to your logger project
cd /path/to/mylogger

# 2. Install in editable mode (development)
pip install -e .

# Or install normally
pip install .
```

### Use in Any Project

```python
# In your other projects, just import normally
from mylogger import logger

logger.info("Hello from another project!")
```

### How It Works

- Installing with `pip install -e .` creates a link to your package
- Changes to mylogger are immediately available
- Works across all projects in the same virtual environment
- **When to use:** Solo developer, local development, quick setup

### Pros

✅ Clean and professional
✅ Works like any other package
✅ Easy to update
✅ Supports dependencies

### Cons

❌ Needs to be installed in each virtual environment
❌ Not automatically synced across environments

---

## Method 2: PYTHONPATH Environment Variable

**Best for:** Temporary use, testing, flexibility

### Setup

```bash
# Option A: Set temporarily (current session only)
export PYTHONPATH="/path/to/mylogger:$PYTHONPATH"

# Option B: Add to ~/.bashrc or ~/.zshrc (permanent)
echo 'export PYTHONPATH="/path/to/mylogger:$PYTHONPATH"' >> ~/.bashrc
source ~/.bashrc

# Windows (Command Prompt)
set PYTHONPATH=C:\path\to\mylogger;%PYTHONPATH%

# Windows (PowerShell)
$env:PYTHONPATH = "C:\path\to\mylogger;$env:PYTHONPATH"
```

### Use in Any Project

```python
from mylogger import logger

logger.info("Using PYTHONPATH method")
```

### Pros

✅ No installation needed
✅ One location, all projects can use
✅ Easy updates

### Cons

❌ Environment variable must be set
❌ Can cause conflicts with other packages
❌ Not portable across machines

---

## Method 3: Symbolic Links (Unix/Linux/Mac)

**Best for:** Local development, single machine

### Setup

```bash
# Create a directory for private packages
mkdir -p ~/my_python_packages

# Move your logger there
mv mylogger ~/my_python_packages/

# In each project's virtual environment
cd /path/to/your_project
source venv/bin/activate

# Create symbolic link
cd venv/lib/python3.x/site-packages/
ln -s ~/my_python_packages/mylogger_project/mylogger mylogger
```

### Use in Project

```python
from mylogger import logger

logger.info("Using symlink method")
```

### Pros

✅ Acts like installed package
✅ Changes reflect immediately
✅ No environment variables

### Cons

❌ Unix/Linux/Mac only
❌ Must create symlink for each venv
❌ Can break if source moves

---

## Method 4: Git Repository (Professional and Recommended) ⭐⭐⭐

**Best for:** Team projects, version control, multiple machines

### Setup Git Repository

```bash
# 1. Initialize git in your logger project
cd mylogger
git init
git add .
git commit -m "Initial commit"

# 2. Create a remote repository (GitHub, GitLab, Bitbucket, or local)
# For GitHub (private repo):
git remote add origin git@github.com:yourusername/mylogger.git
git push -u origin main
git tag v1.0.0
git push origin v1.0.0

# For local git server:
git remote add origin /path/to/git/repos/mylogger.git
git push -u origin main
```

### Install from Git in Other Projects

```bash
# Install directly from git
pip install git+https://github.com/yourusername/mylogger.git

# Or from local git repo
pip install git+file:///path/to/git/repos/mylogger.git

# Install specific branch
pip install git+https://github.com/yourusername/mylogger.git@develop

# Install specific tag/version
pip install git+https://github.com/yourusername/mylogger.git@v1.0.0

# In requirements.txt
git+https://github.com/yourusername/mylogger.git@main
```

### Use in Any Project

```python
from mylogger import logger

logger.info("Using git-installed logger")
```

### Pros

✅ Professional version control
✅ Works on multiple machines
✅ Easy to share with team
✅ Version management (tags/branches)
✅ Track changes

### Cons

❌ Requires git knowledge
❌ Need to commit/push changes
❌ Slightly more setup

**When to use:** Multiple machines, teams, version control needed

---

## Method 5: Direct Copy (Simple but not recommended)

**Best for:** Quick tests, one-off projects

### Setup

```bash
# Copy the mylogger folder to your project
cp -r /path/to/mylogger_project/mylogger /path/to/your_project/
```

### Project Structure

```
your_project/
├── mylogger/          # Copied folder
│   ├── __init__.py
│   ├── logger.py
│   └── ...
├── your_app.py
└── requirements.txt
```

### Use in Project

```python
from mylogger import logger

logger.info("Using copied logger")
```

### Pros

✅ Very simple
✅ Self-contained
✅ No external dependencies

### Cons

❌ Duplicate code in each project
❌ Hard to update
❌ No version control
❌ Increases project size

---

## Method 6: Local Package Server (Advanced)

**Best for:** Organizations, many projects, teams

### Setup with pypiserver

```bash
# 1. Install pypiserver
pip install pypiserver

# 2. Create packages directory
mkdir -p ~/pypi-packages

# 3. Build your package
cd mylogger_project
python setup.py sdist bdist_wheel
cp dist/* ~/pypi-packages/

# 4. Start the server
pypi-server -p 8080 ~/pypi-packages/
```

### Install in Other Projects

```bash
# Install from local server
pip install --index-url http://localhost:8080/simple/ mylogger

# Or configure pip permanently
# Create/edit ~/.pip/pip.conf
[global]
extra-index-url = http://localhost:8080/simple/
```

### Use in Any Project

```python
from mylogger import logger

logger.info("Using private PyPI server")
```

### Pros

✅ Professional solution
✅ Multiple packages
✅ Version management
✅ Team-friendly

### Cons

❌ Complex setup
❌ Requires running server
❌ Overkill for single package

---

## 🎯 Recommended Approach by Use Case

### Solo Developer, Single Machine

**Method 1: Local Pip Install (editable)**

```bash
cd mylogger_project
pip install -e .
```

### Solo Developer, Multiple Machines

**Method 4: Git Repository (private)**

```bash
git init
git remote add origin git@github.com:yourusername/mylogger.git
pip install git+ssh://git@github.com/yourusername/mylogger.git
```

### Small Team

**Method 4: Git Repository (shared private repo)**

```bash
# One person sets up
git init
git remote add origin git@github.com:yourteam/mylogger.git

# Everyone installs
pip install git+ssh://git@github.com/yourteam/mylogger.git
```

### Large Organization

**Method 6: Private Package Server**

- Set up internal PyPI server
- Use CI/CD to publish updates
- Version management

---

## 📝 Step-by-Step: Recommended Setup (Git + Editable Install)

### One-Time Setup

```bash
# 1. Set up git repository
cd mylogger_project
git init
git add .
git commit -m "Initial logger implementation"

# 2. Push to GitHub (private repository)
# Create private repo on GitHub first, then:
git remote add origin git@github.com:yourusername/mylogger.git
git branch -M main
git push -u origin main

# 3. Tag your first version
git tag v1.0.0
git push origin v1.0.0
```

### For Development (Your Main Project)

```bash
# Install in editable mode for active development
cd mylogger_project
pip install -e .

# Now you can edit mylogger and changes are immediate
```

### For Other Projects

```bash
# Install specific version
pip install git+https://github.com/yourusername/mylogger.git@v1.0.0

# Or in requirements.txt
echo "git+https://github.com/yourusername/mylogger.git@v1.0.0" >> requirements.txt
pip install -r requirements.txt
```

### Updating Your Logger

```bash
# 1. Make changes to mylogger
cd mylogger
# ... edit files ...

# 2. Test in development
pytest

# 3. Commit and tag new version
git add .
git commit -m "Add rotation feature"
git tag v1.1.0
git push origin main --tags

# 4. Update in other projects
pip install --upgrade git+https://github.com/yourusername/mylogger.git@v1.1.0
```

---

## 🔧 Practical Example: Complete Workflow

### Project Structure on Your System

```
~/projects/
├── mylogger_project/          # Your logger library
│   ├── mylogger/
│   ├── tests/
│   ├── setup.py
│   └── .git/
│
├── web_app/                   # Project 1
│   ├── app.py
│   ├── requirements.txt       # includes mylogger
│   └── venv/
│
├── data_processor/            # Project 2
│   ├── process.py
│   ├── requirements.txt       # includes mylogger
│   └── venv/
│
└── api_service/               # Project 3
    ├── main.py
    ├── requirements.txt       # includes mylogger
    └── venv/
```

### Workflow

```bash
# 1. Develop logger
cd ~/projects/mylogger_project
pip install -e .
# Make changes, test...
git commit -am "Improvements"
git tag v1.2.0
git push origin main --tags

# 2. Use in web_app
cd ~/projects/web_app
source venv/bin/activate
pip install git+https://github.com/yourusername/mylogger.git@v1.2.0

# 3. Use in data_processor
cd ~/projects/data_processor
source venv/bin/activate
pip install git+https://github.com/yourusername/mylogger.git@v1.2.0

# 4. Use in api_service
cd ~/projects/api_service
source venv/bin/activate
pip install git+https://github.com/yourusername/mylogger.git@v1.2.0
```

### In Each Project's Code

```python
# web_app/app.py
from mylogger import logger

logger.add("app.log", rotation="10 MB")
logger.info("Web app started")

# data_processor/process.py
from mylogger import logger

logger.add("processor.log")
logger.info("Processing data")

# api_service/main.py
from mylogger import logger

logger.bind(service="api").info("Service started")
```

---

## 📋 requirements.txt Examples

### Using Git (Recommended)

```txt
# requirements.txt

# Your logger from git
git+https://github.com/yourusername/mylogger.git@v1.0.0

# Other dependencies
flask==2.3.0
requests==2.31.0
```

### Using Local Path (Development)

```txt
# requirements.txt

# Your logger from local path (editable)
-e /path/to/mylogger_project

# Other dependencies
flask==2.3.0
requests==2.31.0
```

### Using File Path

```txt
# requirements.txt

# Your logger from local wheel
/path/to/mylogger_project/dist/mylogger-1.0.0-py3-none-any.whl

# Other dependencies
flask==2.3.0
requests==2.31.0
```

---

## 🚀 Quick Setup Script

Save this as `setup_mylogger.sh`:

```bash
#!/bin/bash

# Setup script for MyLogger in new projects

echo "🚀 Setting up MyLogger..."

# Check if venv exists
if [ ! -d "venv" ]; then
    echo "Creating virtual environment..."
    python3 -m venv venv
fi

# Activate venv
source venv/bin/activate

# Install mylogger
echo "Installing MyLogger..."
pip install git+https://github.com/yourusername/mylogger.git@main

# Verify installation
python -c "from mylogger import logger; print('✅ MyLogger installed successfully!')"

echo "🎉 Done! MyLogger is ready to use."
```

Usage:

```bash
chmod +x setup_mylogger.sh
./setup_mylogger.sh
```

---

## 🐍 Python Script to Auto-Import from Local

If you want to import from a local directory without installation:

```python
# add_to_path.py - Add this at the top of your scripts

import sys
from pathlib import Path

# Add mylogger to path
mylogger_path = Path.home() / "projects" / "mylogger_project"
if mylogger_path.exists():
    sys.path.insert(0, str(mylogger_path))

# Now you can import
from mylogger import logger
```

Then in your scripts:

```python
# your_script.py
import add_to_path  # This adds mylogger to path
from mylogger import logger

logger.info("Logger working!")
```

---

## 🔒 Security Considerations

### For Private Git Repos

```bash
# Use SSH keys for authentication
git remote add origin git@github.com:yourusername/mylogger.git

# Or use access tokens
git remote add origin https://<token>@github.com/yourusername/mylogger.git

# For pip install with private repos
pip install git+ssh://git@github.com/yourusername/mylogger.git
# or
pip install git+https://<token>@github.com/yourusername/mylogger.git
```

### For Team Access

```bash
# Add team members to private GitHub repo
# They can then install with:
pip install git+ssh://git@github.com/yourteam/mylogger.git
```

---

## ✅ Best Practices

1. **Version Your Logger**

   ```bash
   git tag -a v1.0.0 -m "First stable release"
   git push origin v1.0.0
   ```
2. **Pin Versions in Production**

   ```txt
   # requirements.txt
   git+https://github.com/yourusername/mylogger.git@v1.0.0
   ```
3. **Use Editable Install for Development**

   ```bash
   pip install -e /path/to/mylogger_project
   ```
4. **Document Installation**
   Create `INSTALL.md` in each project explaining how to set up
5. **Keep Logger Separate**
   Don't mix logger code with application code

---

## 🎯 Summary: Choose Your Method

| Method            | Complexity  | Best For      | Updates         |
| ----------------- | ----------- | ------------- | --------------- |
| Local Pip Install | ⭐ Easy     | Single dev    | Manual          |
| PYTHONPATH        | ⭐ Easy     | Testing       | Immediate       |
| Symlinks          | ⭐⭐ Medium | Local dev     | Immediate       |
| Git Repo          | ⭐⭐ Medium | Teams         | Version control |
| Direct Copy       | ⭐ Easy     | One-off       | Manual copy     |
| Package Server    | ⭐⭐⭐ Hard | Organizations | Automatic       |

**Our Recommendation: Git Repository + Local Pip Install** ⭐⭐⭐

---

## 🚀 Ready to Use!

Pick your method, set it up once, and then simply:

```python
from mylogger import logger

logger.info("Your custom logger, everywhere!")
```

**That's it! Your private logger library is now usable across all your projects.** 🎉
