# setup


## adduser w keys
```bash
adduser abv
sudo usermod -aG sudo abv # add abv to sudoers
su - abv
sudo chmod -R 755 /home/abv
ssh-keygen -t ed25519 -f /home/abv/.ssh/id_ed25519
mkdir public_html
```
Anytime vscode has created new files owned by root run this
``` bash
sudo chown -R abv:abv /home/abv
```
## opening vscode in WSL as /home/user
It sucks that you cannot do it from RemotExplore/WSL_Targets. You have to do it through ssh.

From WSL
1. put  "C:\Users\mcken\.ssh\id_ed25519.pub" the windows root key and "\\wsl.localhost\Ubuntu-22.04\root\.ssh\id_ed25519.pub" in /home/user/.ssh/authorized_keys
2. find hostname ```hostname -I``` # currently 192.168.193.234
   

From Win
1. ctrl+shift+p Remote-SSH: Add new SSH host user@192.168.193.234 or whatever WSL's ip might have been changed to 
2. open "C:\Users\mcken\.ssh\config" and futz with the first line of the group (the alias) that's what will show up in vscode 
3. use RemoteExplorer/SSH to login as /home/user. Now creating files with the UI will have the correct owner/group

### localhost setup

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install nginx git curl build-essential -ya


# Follow prompts, then restart terminal or run:
source ~/.bashrc
```
### abv-beta setup
```bash
sudo usermod -aG sudo abv-beta # add abv to sudoers
groups abv-beta
su - abv-beta
sudo chmod 700 /home/abv-beta 
ssh-keygen -t ed25519 -f /home/abv-beta/.ssh/id_ed25519
mkdir public_html
sudo chmod -R 755 /home/abv-beta
```


#### accessing /home/abv/miniconda3/ from /home/abv-beta
login abv-beta then
```bash
export PATH="/home/abv/miniconda3/bin:$PATH"
```
### conda 
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
#### conda install abvenv
```bash
conda init
conda update conda
conda install ipython
conda update ipython 
conda create -n abvenv python=3.13.9 flask gunicorn 
conda activate abvenv
conda install bs4
pip install docx2pdf
conda install markdown
conda install markdown2
pip install --upgrade jupyter
pip install --upgrade ipywidgets
```
### git
```bash
git config --global user.name "mckennatim"
git config --global user.email "mckenna.tim@gmail.com"
rm -rf .git
git init
git remote add origin https://github.com/mckennatim/abv.git
git branch -M main
git add -A
git commit -m"initial commit"
git rm -r --cached miniconda3/
git commit -m "Remove miniconda3 directory from repository"
git push -u origin main
```

### creating `environment.yml` from existing env
```bash
conda list 
 # or with channels
conda list --explicit 
conda env export --from history > environment.yml
 # or for no platform specific builds
conda env export --no-builds > environment.yml 
conda info --envs
```

## directory structure

- public_html/
    - index.html

## nginx mods for localhost (see /home/setup.md or)

sudo chown -R $USER:www-data /home/abv/public_html
