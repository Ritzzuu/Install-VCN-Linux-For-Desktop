# Secure VNC Setup on Linux VPS with XFCE and Google Chrome
This guide provides step-by-step instructions for setting up a secure VNC server on a Linux VPS, installing the lightweight XFCE desktop environment, and integrating Google Chrome.

## 1. Install Main Packages
```bash
sudo apt install curl iptables build-essential git wget lz4 jq make gcc nano automake autoconf tmux htop nvme-cli libgbm1 pkg-config libssl-dev libleveldb-dev tar clang bsdmainutils ncdu unzip libleveldb-dev  -y
```
## 2. Install XFCE Desktop Environment
First, install the XFCE graphical user interface:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install xfce4 xfce4-goodies -y
```

## 3. Install VNC Server
Install VNC Server to allow remote access to the graphical interface of your VPS:
```bash
sudo apt install tightvncserver -y

# Activate clipoboard in VNC
sudo apt install autocutsel
```

## 4. Config VNC Server
**1. Start VNC Server for the first time to set up a password:**
```bash
vncserver
```
* This will run a VNC screen on `:1` port

**2. Stop VNC Server to config it:**
```bash
vncserver -kill :1
```

**3. Configure VNC to use XFCE and automatically open Terminal**

Open the xstartup configuration file and edit it to start XFCE along with the Terminal:
```bash
nano ~/.vnc/xstartup
```

Replace the content of the file with:
```bash
#!/bin/bash
xrdb $HOME/.Xresources
startxfce4 &
xfce4-terminal &
autocutsel &
-geometry 1920x1080 -depth 24
```

**4. Make the xstartup file executable:**
```bash
chmod +x ~/.vnc/xstartup
```

## 5. Start a VNC session
**1. Start a VNC session on display `:1` and port `5901`**
```bash
vncserver :1 -geometry 1920x1080 -depth 24
```
* If needed, you can run multiple sessions by entering `:2` or more

## 6. Connect VNC from a Windows Machine
**1. Download VNC Viewer from [RealVNC](https://www.realvnc.com/en/connect/download/viewer/) and install it on your Windows machine.**

**2. Open VNC Viewer and connect to `IP_VPS:5901` or `IP_VPS:1`, then enter the VNC password when prompted.**

**3. Now, you should see the desktop interface of your VPS, with the Terminal automatically opened upon login.**

* To copy & paste anything in the VNC xefc terminal, you can use `shift+ctrl+c` & `shift+ctrl+v`

#

### Optional. Enable ports for your VNC seasons
If you had problem connecting the VNC, you can try openning the ports

* You might better activate your firewall for security reasons first:
```bash
sudo ufw allow ssh
sudo ufw enable
```

* Allow VNC connections:
```bash
sudo ufw allow 5901/tcp
```

* For multiple VNC sessions, allow each port:
```bash
sudo ufw allow 5901/tcp
sudo ufw allow 5902/tcp
```

**Optional: To kill VNC sessions if needed**
```bash
vncserver -kill :1
```

#

## 7. Install Google Chrome on the VPS
**1. Open Terminal (if it hasn't automatically opened in the VNC session).**

**2.Download Google Chrome:**
```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
```

**3. Install Google Chrome:**
```bash
sudo apt install ./google-chrome-stable_current_amd64.deb
```

If you encounter dependency errors, run:
```bash
sudo apt --fix-broken install
```

**4. Launch Chrome in VNC (add --no-sandbox if necessary):**
```bash
google-chrome --no-sandbox
```
# Install-VCN-Linux-For-Desktop
