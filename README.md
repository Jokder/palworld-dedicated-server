PalWorld dedicated server deployment
====================================
This document describes how to deploy a PalWorld dedicated server.

## for arm64 (ubuntu above 20.04)
### 1. Install FEX-Emu
```bash
curl --silent https://raw.githubusercontent.com/FEX-Emu/FEX/main/Scripts/InstallFEX.py --output /tmp/InstallFEX.py && python3 /tmp/InstallFEX.py && rm /tmp/InstallFEX.py
```

### 2. Install SteamCMD
```bash
mkdir ~/steamcmd && cd ~/steamcmd
curl -sqL "https://steamcdn-a.akamaihd.net/client/installer/steamcmd_linux.tar.gz" | tar zxvf -
```

### 3. Test steamcmd
```bash
cd ~/steamcmd
./steamcmd.sh +login anonymous +quit
```

### 3. Install PalWorld dedicated server
```bash
#don't run steamcmd as root
cd ~/steamcmd
./steamcmd.sh +login anonymous +app_update 2394010 validate +quit
```
after install, you can find the server files in `~/Steam/steamapps/common/PalServer`

### 4. Run PalWorld dedicated server
```bash
#don't run PalWorld as root
~/Steam/steamapps/common/PalServer/PalServer.sh
```

### 5. Create a systemd service
```bash
sudo touch /etc/systemd/system/pal-server.service
sudo vim /etc/systemd/system/pal-server.service
```
paste the following content into the file

```ini
[Unit]
Description=PalWorld server

[Service]
ExecStart= </my/path/to/PalServer.sh>
WorkingDirectory=</my/path/to/PalServer>
User=<your user name>
Group=<your group name>
Restart=always

[Install]
WantedBy=multi-user.target
```
then run
```bash
sudo systemctl daemon-reload
sudo systemctl enable pal-server.service
sudo systemctl start pal-server.service
```

## Dedicated server configuration
### 1. Server configuration
The server configuration file is located in `/home/ubuntu/Steam/steamapps/common/PalServer/Pal/Saved/Config/LinuxServer`

more details please see official guide: https://tech.palworldgame.com/optimize-game-balance
