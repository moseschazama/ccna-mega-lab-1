# 📘 How to Create Your GitHub Repository – Step by Step

This guide is written for someone who is new to GitHub. Follow each step carefully.

---

## Step 1 – Create a GitHub Account

1. Go to [https://github.com](https://github.com)
2. Click **Sign up**
3. Enter your email, create a password, and choose a username
4. Verify your email address

---

## Step 2 – Create a New Repository

1. After logging in, click the **+** icon in the top-right corner
2. Select **New repository**
3. Fill in the form:
   - **Repository name**: `ccna-mega-lab`
   - **Description**: `Jeremy's IT Lab CCNA Mega Lab – Full Packet Tracer configuration`
   - **Visibility**: Public ✅ (so others can see your work)
   - ✅ Check **Add a README file** — but we will replace it with our own
4. Click **Create repository**

---

## Step 3 – Install Git on Your Computer

### Windows
1. Go to [https://git-scm.com/download/win](https://git-scm.com/download/win)
2. Download and install Git (use default settings)
3. Open **Git Bash** from the Start menu

### Mac
1. Open Terminal
2. Run: `git --version`
3. If not installed, macOS will prompt you to install it

### Linux
```bash
sudo apt install git    # Ubuntu/Debian
sudo dnf install git    # Fedora
```

---

## Step 4 – Configure Git (First Time Only)

Open Git Bash (Windows) or Terminal (Mac/Linux) and run:

```bash
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```

---

## Step 5 – Upload Your Files to GitHub

### Option A – Using the GitHub Website (Easiest for Beginners)

1. Go to your repository on GitHub: `https://github.com/YOUR_USERNAME/ccna-mega-lab`
2. Click **Add file** → **Upload files**
3. Drag and drop the files from this repo (README.md, configs folder, etc.)
4. At the bottom, type a commit message: `Initial commit – full CCNA Mega Lab documentation`
5. Click **Commit changes**

### Option B – Using Git on Your Computer (Recommended)

1. **Clone** your empty repo to your computer:
   ```bash
   git clone https://github.com/YOUR_USERNAME/ccna-mega-lab.git
   cd ccna-mega-lab
   ```

2. **Copy** all the files from this documentation into that folder.

3. **Add** all files to Git tracking:
   ```bash
   git add .
   ```

4. **Commit** with a message:
   ```bash
   git commit -m "Initial commit: full CCNA Mega Lab documentation"
   ```

5. **Push** to GitHub:
   ```bash
   git push origin main
   ```

---

## Step 6 – Add Your Device Configs

After each device is configured in Packet Tracer:

1. On the device CLI, run:
   ```
   show running-config
   ```
2. Copy everything from the terminal output.
3. Open the file `configs/<DeviceName>/running-config.txt` in any text editor.
4. Paste the config and save the file.
5. Commit and push:
   ```bash
   git add configs/R1/running-config.txt
   git commit -m "Add R1 running config"
   git push
   ```

---

## Step 7 – Folder Structure to Upload

Your repository should look like this:

```
ccna-mega-lab/
├── README.md                    ← Main documentation (already written)
├── docs/
│   └── github-setup.md         ← This file
├── configs/
│   ├── R1/
│   │   ├── notes.md
│   │   └── running-config.txt   ← Paste your R1 config here
│   ├── CSW1/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── CSW2/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── DSW-A1/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── DSW-A2/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── DSW-B1/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── DSW-B2/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── ASW-A1/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── ASW-A2/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── ASW-A3/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── ASW-B1/
│   │   ├── notes.md
│   │   └── running-config.txt
│   ├── ASW-B2/
│   │   ├── notes.md
│   │   └── running-config.txt
│   └── ASW-B3/
│       ├── notes.md
│       └── running-config.txt
```

---

## Tips for Good Git Commits

Write clear, short commit messages that explain **what** you changed:

| ✅ Good | ❌ Bad |
|--------|--------|
| `Add R1 NAT and DHCP config` | `update` |
| `Fix DSW-A1 HSRP priority` | `stuff` |
| `Complete Part 6 – Network Services` | `done` |

---

## Useful Git Commands Reference

| Command | What it does |
|---------|-------------|
| `git status` | See which files have changed |
| `git add .` | Stage all changed files |
| `git add <file>` | Stage a specific file |
| `git commit -m "message"` | Save a snapshot with a message |
| `git push` | Upload commits to GitHub |
| `git pull` | Download latest changes from GitHub |
| `git log` | See history of commits |

---

You're all set! Once uploaded, your repository URL will be:  
**`https://github.com/YOUR_USERNAME/ccna-mega-lab`**
