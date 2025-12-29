# WSL2 & Remote Server Setup Guide
## Nginx + Conda + Flask Development Environment

## Overview
This guide covers setting up identical development (WSL2 localhost) and production (remote server) environments using Nginx, Conda, and Flask. The localhost is the source of truth, pushing to GitHub, with the server pulling updates.

---

## 1. Configuration Files (Similar to package.json)

### `environment.yml` - Conda Environment Specification
```yaml
name: myproject
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.11
  - flask
  - gunicorn
  - pip
  - pip:
    - flask-cors
    - python-dotenv
```

### `requirements.txt` - Python Dependencies
```txt
Flask==3.0.0
gunicorn==21.2.0
flask-cors==4.0.0
python-dotenv==1.0.0
```

### `package.json` - Frontend Dependencies (Optional)
```json
{
  "name": "myproject-admin",
  "version": "1.0.0",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "devDependencies": {
    "vite": "^5.0.0"
  }
}
```

---

## 2. Project Structure with public_html

### Recommended Structure (Security Best Practice)

**KEEP OUTSIDE public_html (Private - Not Web Accessible):**
- Application code (Flask/Python)
- Configuration files
- Environment files
- Scripts
- Source code

**KEEP INSIDE public_html (Public - Web Accessible):**
- Static assets only (CSS, JS, images)
- Built frontend files
- Uploaded user files (if any)

```
~/project/                          # Outside public_html - PRIVATE
├── environment.yml                 # Conda environment (private)
├── requirements.txt                # Python packages (private)
├── package.json                    # Frontend dependencies (private)
├── .env                            # Environment variables (private, not committed)
├── .env.example                    # Template (committed)
├── .gitignore
├── app/                            # Flask application (private)
│   ├── app.py                      # Main Flask app
│   ├── wsgi.py                     # WSGI entry point
│   ├── __init__.py
│   ├── models.py                   # Database models
│   ├── routes.py                   # Route handlers
│   └── utils.py                    # Utility functions
├── admin/                          # Frontend admin source (private)
│   ├── src/                        # Source files
│   │   ├── index.html
│   │   ├── main.js
│   │   └── style.css
│   └── package.json
├── nginx/
│   └── site.conf                   # Nginx configuration
├── scripts/
│   ├── setup.sh
│   ├── deploy.sh
│   ├── flask.service
│   └── build-admin.sh              # Build script that outputs to public_html
└── public_html/                    # Web root - PUBLIC (Nginx serves from here)
    ├── index.html                  # Optional: static homepage
    ├── static/                     # Static assets
    │   ├── css/
    │   │   └── style.css
    │   ├── js/
    │   │   └── main.js
    │   └── images/
    │       └── logo.png
    ├── admin/                      # Built admin app (from ~/project/admin/src)
    │   ├── index.html
    │   ├── assets/
    │   │   ├── index-abc123.js
    │   │   └── index-def456.css
    │   └── favicon.ico
    └── uploads/                    # User uploaded files (if needed)
        └── .gitkeep
```

### Why This Structure?

**Security Benefits:**
1. **Application code is NOT accessible via web** - Flask app, configs, .env files stay private
2. **Only static files are served by Nginx** - Reduces attack surface
3. **Python files can't be downloaded** - Source code protected
4. **Credentials stay private** - .env, database configs not in web root

### Build Process

Frontend admin app gets **built** from source and output to `public_html/admin`:

```bash
# Build admin app
cd ~/project/admin
npm install
npm run build -- --outDir ../public_html/admin
```


---

## 3. Phase A: Localhost WSL2 Setup

### Step 1: Install Prerequisites
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx git curl build-essential -y
```

### Step 2: Install Miniconda
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
# Follow prompts, then restart terminal or run:
source ~/.bashrc
```

### Step 3: Create Project Directory
```bash
mkdir -p ~/project
mkdir -p ~/project/public_html/{static/{css,js,images},admin,uploads}
mkdir -p ~/project/app
mkdir -p ~/project/admin/src
mkdir -p ~/project/nginx
mkdir -p ~/project/scripts
cd ~/project
```

### Step 4: Create Configuration Files

#### `environment.yml`
```yaml
name: myproject
channels:
  - defaults
  - conda-forge
dependencies:
  - python=3.11
  - flask
  - gunicorn
  - pip
  - pip:
    - flask-cors
    - python-dotenv
```

#### `requirements.txt`
```txt
Flask==3.0.0
gunicorn==21.2.0
flask-cors==4.0.0
python-dotenv==1.0.0
```

#### `.gitignore`
```txt
__pycache__/
*.pyc
*.pyo
*.pyd
.env
.venv
venv/
node_modules/
dist/
.DS_Store
*.log
```

### Step 5: Setup Conda Environment
```bash
conda env create -f environment.yml
conda activate myproject
```

### Step 6: Create Flask Application

#### `app/app.py`
```python
from flask import Flask, jsonify, send_from_directory
from flask_cors import CORS
import os

app = Flask(__name__)
CORS(app)

@app.route('/api/health')
def health():
    return jsonify({'status': 'ok', 'message': 'Flask is running'})

@app.route('/api/data')
def get_data():
    return jsonify({'data': 'Sample data from Flask'})

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=5000)
```

#### `app/wsgi.py`
```python
from app import app

if __name__ == "__main__":
    app.run()
```

### Step 7: Configure Nginx

#### `nginx/site.conf`
```nginx
server {
    listen 80;
    server_name localhost;
    
    # Web root - serves public_html directory
    root /home/YOUR_USERNAME/project/public_html;
    index index.html;

    # Static files (served directly from public_html)
    location /static {
        # Files already in /public_html/static
        expires 30d;
        add_header Cache-Control "public, immutable";
    }

    # Admin frontend (served directly from public_html)
    location /admin {
        # Files already in /public_html/admin
        try_files $uri $uri/ /admin/index.html;
    }

    # Flask API (proxied to Flask app running outside web root)
    location /api {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Default location - can serve static homepage or proxy to Flask
    location / {
        # Try to serve static file first, then proxy to Flask
        try_files $uri $uri/ @flask;
    }

    # Flask proxy (for dynamic content)
    location @flask {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }

    # Security: Deny access to sensitive files
    location ~ /\. {
        deny all;
    }
    
    location ~ \.(py|pyc|pyo|pyd|env|yml|yaml|sh|md|git)$ {
        deny all;
    }
}
```

#### Install Nginx Configuration
```bash
# Copy config (replace YOUR_USERNAME)
sudo cp nginx/site.conf /etc/nginx/sites-available/myproject

# Enable site
sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled/

# Remove default site if present
sudo rm -f /etc/nginx/sites-enabled/default

# Test configuration
sudo nginx -t

# Restart Nginx
sudo systemctl restart nginx
```

### Step 8: Test Locally
```bash
# Activate environment
conda activate myproject

# Run Flask
python app/app.py

# In another terminal, test:
curl http://localhost/api/health
```

### Step 9: Initialize Git
```bash
git init
git add .
git commit -m "Initial setup: Flask + Nginx + Conda"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO.git
git push -u origin main
```

---

## 4. Phase B: Remote Server Setup

### Step 1: Create Setup Script

#### `scripts/setup.sh`
```bash
#!/bin/bash
set -e  # Exit on error

echo "=== Starting setup ==="

# Update system
echo "Updating system packages..."
sudo apt update && sudo apt upgrade -y
sudo apt install nginx git curl build-essential -y

# Install Miniconda (if not present)
if ! command -v conda &> /dev/null; then
    echo "Installing Miniconda..."
    cd ~
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    bash Miniconda3-latest-Linux-x86_64.sh -b -p ~/miniconda3
    ~/miniconda3/bin/conda init bash
    source ~/.bashrc
    echo "Miniconda installed"
fi

# Get project directory
PROJECT_DIR="${1:-$HOME/project}"
echo "Project directory: $PROJECT_DIR"

# Clone or pull repository
if [ ! -d "$PROJECT_DIR" ]; then
    echo "Cloning repository..."
    git clone https://github.com/USERNAME/REPO.git "$PROJECT_DIR"
else
    echo "Updating repository..."
    cd "$PROJECT_DIR"
    git pull origin main
fi

cd "$PROJECT_DIR"

# Setup conda environment
echo "Setting up conda environment..."
source ~/miniconda3/etc/profile.d/conda.sh
conda env create -f environment.yml --force
conda activate myproject

# Install Python dependencies
pip install -r requirements.txt

# Install Node.js and build admin frontend (if admin/package.json exists)
if [ -f admin/package.json ]; then
    echo "Installing Node.js..."
    curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
    sudo apt install nodejs -y
    
    echo "Building admin frontend..."
    cd admin
    npm install
    npm run build -- --outDir ../public_html/admin
    cd ..
    echo "Admin frontend built to public_html/admin"
fi

# Ensure public_html directories exist
mkdir -p public_html/{static/{css,js,images},admin,uploads}

# Setup Nginx
echo "Configuring Nginx..."
sudo cp nginx/site.conf /etc/nginx/sites-available/myproject
sudo ln -sf /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t && sudo systemctl restart nginx

# Setup systemd service for Flask
echo "Setting up Flask service..."
sudo cp scripts/flask.service /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl enable flask
sudo systemctl start flask

echo "=== Setup complete ==="
echo "Flask service status:"
sudo systemctl status flask --no-pager
```

### Step 2: Create Deployment Script

#### `scripts/deploy.sh`
```bash
#!/bin/bash
set -e

PROJECT_DIR="${1:-$HOME/project}"
cd "$PROJECT_DIR"

echo "=== Starting deployment ==="

# Pull latest changes
echo "Pulling from GitHub..."
git pull origin main

# Activate conda environment
source ~/miniconda3/etc/profile.d/conda.sh
conda activate myproject

# Update conda environment
echo "Updating conda environment..."
conda env update -f environment.yml --prune

# Update pip packages
echo "Updating pip packages..."
pip install -r requirements.txt

# Rebuild frontend (if applicable)
if [ -f admin/package.json ]; then
    echo "Rebuilding admin frontend..."
    cd admin
    npm install
    npm run build -- --outDir ../public_html/admin
    cd ..
    echo "Admin frontend rebuilt to public_html/admin"
fi

# Restart services
echo "Restarting services..."
sudo systemctl restart flask
sudo systemctl reload nginx

echo "=== Deployment complete ==="
echo "Flask service status:"
sudo systemctl status flask --no-pager -l
```

### Step 3: Create Systemd Service

#### `scripts/flask.service`
```ini
[Unit]
Description=Flask Application
After=network.target

[Service]
User=YOUR_USERNAME
Group=YOUR_USERNAME
WorkingDirectory=/home/YOUR_USERNAME/project
Environment="PATH=/home/YOUR_USERNAME/miniconda3/envs/myproject/bin"
ExecStart=/home/YOUR_USERNAME/miniconda3/envs/myproject/bin/gunicorn -w 4 -b 127.0.0.1:5000 app.wsgi:app
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

### Step 4: Run on Remote Server
```bash
# SSH into server
ssh user@remote-server

# Download and run setup script
curl -o setup.sh https://raw.githubusercontent.com/USERNAME/REPO/main/scripts/setup.sh
bash setup.sh

# Or if repo is already cloned:
cd ~/project
bash scripts/setup.sh
```

---

## 5. Phase C: Deployment Workflow

### Option 1: GitHub Pull (Recommended)

#### On Localhost (Development):
```bash
# Make changes
vim app/app.py

# Test locally
python app/app.py

# Commit and push
git add .
git commit -m "Update: description of changes"
git push origin main
```

#### On Remote Server (Production):
```bash
# SSH to server
ssh user@remote-server

# Run deployment script
cd ~/project
bash scripts/deploy.sh

# Or manually:
git pull origin main
conda activate myproject
sudo systemctl restart flask
sudo systemctl reload nginx
```

### Option 2: Rsync Direct Sync

#### `scripts/sync-to-server.sh` (Run from localhost)
```bash
#!/bin/bash
REMOTE_USER="username"
REMOTE_HOST="remote-server-ip"
REMOTE_PATH="~/project"

rsync -avz --delete \
  --exclude '.git' \
  --exclude 'node_modules' \
  --exclude '__pycache__' \
  --exclude '*.pyc' \
  --exclude '.env' \
  --exclude 'venv' \
  ~/project/ ${REMOTE_USER}@${REMOTE_HOST}:${REMOTE_PATH}/

# Restart services on remote
ssh ${REMOTE_USER}@${REMOTE_HOST} << 'EOF'
cd ~/project
source ~/miniconda3/etc/profile.d/conda.sh
conda activate myproject
sudo systemctl restart flask
sudo systemctl reload nginx
EOF

echo "Sync complete!"
```

---

## 6. Environment Variables

### `.env.example` (Commit this)
```env
FLASK_ENV=development
SECRET_KEY=change-this-in-production
DATABASE_URL=sqlite:///app.db
```

### `.env` (Don't commit - local/production specific)
```env
FLASK_ENV=production
SECRET_KEY=your-secure-random-key-here
DATABASE_URL=postgresql://user:pass@localhost/dbname
```

Load in Flask:
```python
from dotenv import load_dotenv
import os

load_dotenv()

app.config['SECRET_KEY'] = os.getenv('SECRET_KEY')
```

---

## 7. Useful Commands

### Conda
```bash
# Activate environment
conda activate myproject

# Update environment
conda env update -f environment.yml --prune

# Export environment
conda env export > environment.yml

# Remove environment
conda env remove -n myproject
```

### Systemd (Remote Server)
```bash
# Check status
sudo systemctl status flask

# View logs
sudo journalctl -u flask -f

# Restart
sudo systemctl restart flask

# Stop/Start
sudo systemctl stop flask
sudo systemctl start flask
```

### Nginx
```bash
# Test configuration
sudo nginx -t

# Reload (without downtime)
sudo systemctl reload nginx

# Restart
sudo systemctl restart nginx

# View logs
sudo tail -f /var/log/nginx/error.log
sudo tail -f /var/log/nginx/access.log
```

### Git
```bash
# Check status
git status

# View changes
git diff

# Commit
git add .
git commit -m "message"
git push origin main

# Pull updates
git pull origin main
```

---

## 8. Testing Checklist

### Localhost Testing
- [ ] Flask app runs: `python app/app.py`
- [ ] API accessible: `curl http://localhost/api/health`
- [ ] Static files serve: Check `http://localhost/static/`
- [ ] Nginx proxies correctly
- [ ] Conda environment activates

### Remote Server Testing
- [ ] SSH access works
- [ ] Flask service running: `sudo systemctl status flask`
- [ ] Nginx running: `sudo systemctl status nginx`
- [ ] API accessible: `curl http://your-domain.com/api/health`
- [ ] Static files serve correctly
- [ ] Deployment script works: `bash scripts/deploy.sh`

---

## 9. Security Checklist (Production)

```bash
# Setup firewall
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

# Install fail2ban
sudo apt install fail2ban -y
sudo systemctl enable fail2ban

# Setup SSL with Let's Encrypt
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d your-domain.com
```

---

## 10. Quick Reference

### Initial Setup (Localhost)
```bash
conda env create -f environment.yml
conda activate myproject
python app/app.py
```

### Initial Setup (Remote Server)
```bash
bash scripts/setup.sh
```

### Deploy Changes
```bash
# Localhost: push
git push origin main

# Server: pull
bash scripts/deploy.sh
```

### Troubleshooting
```bash
# Check Flask logs
sudo journalctl -u flask -f

# Check Nginx logs
sudo tail -f /var/log/nginx/error.log

# Test Nginx config
sudo nginx -t

# Check listening ports
sudo netstat -tulpn | grep :5000
sudo netstat -tulpn | grep :80
```

what would the command be to rsync from abv@abvchorus.org /home/abv/public_html/*.html to localhost /home/abv/public_html

```bash
rsync -avz abv@abvchorus.org:/home/abv/public_html/*.html /home/abv/public_html/
```

if you push to github a repository with a .gitignore, when you pull it from github to the server, will it honor that .gitignore -ans:NO

How would I modify or recreate a repository on localhost so that when it is pulled to the production server, it will not pull and overwrite the files/folders listed in the .gitignore

What would that git rm command look like if I no longer wanted to track

public_html/*.html
public_html/resources/
public_html/assets/

what do you need in rsnc so that it deletes files/folders on the destination that no longer exist in the source