# Disposable Minecraft Multiplayer Server ⛏️

A lightweight, isolated, single-container Minecraft server featuring **Paper Spigot** with cross-play support (**Geyser/Floodgate**) and an integrated real-time **Flask Dashboard**. Perfect for setting up high-performance, temporary, or persistent multiplayer environments instantly.

---

## 🚀 Quick Start

To fire up the container with default allocations, credential persistence, and administrative control permissions, run the following command in your terminal:

```
docker run -d --name mc-server \
  -p 25565:25565 \
  -p 19132:19132/udp \
  -p 7777:7777 \
  -e MEMORY_MIN="128M" \
  -e MEMORY_MAX="2G" \
  -e VIA_VERSION_BUILD="5.11.0" \
  -e MC_CONTAINER_NAME="mc-server" \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ./data:/minecraft/data \
  danirali2007/disposable-minecraft-multiplayer
```
Windows:
```
docker run -d --name mc-server -p 25565:25565 -p 19132:19132/udp -p 7777:7777 -e MEMORY_MIN="128M" -e MEMORY_MAX="2G" -e VIA_VERSION_BUILD="5.11.0" -e MC_CONTAINER_NAME="mc-server" -v /var/run/docker.sock:/var/run/docker.sock -v ./data:/minecraft/data danirali2007/disposable-minecraft-multiplayer
```

## 📊 Monitoring Dashboard
The container includes an integrated socket-query web management panel that does not rely on external APIs. It allows you to monitor live player sessions, execute online Paper JAR updates, change display IPs, and restart the container directly from the web interface.
To view the status of your Java engine and Bedrock proxy layer, access port 7777 on your browser:
<br>
👉 Local: http://localhost:7777
<br>
👉 Network: http://<YOUR_SERVER_IP>:7777 (e.g., http://192.168.0.100:7777)

Default credentials:
Username: admin
Password: minrcaft-admin

## 🔌 Network Port Mappings
This container relies on three primary port allocations to handle connections smoothly:

Port	Protocol	Service	Description
25565	TCP	Java Edition	Standard connection port for desktop Minecraft clients.
19132	UDP	Bedrock Edition	Geyser translation tunnel port for mobile, console, and Windows 10/11 clients.
7777	TCP	Flask UI	Web infrastructure panel showing live player counts, administrative actions, and statuses.

## ⚙️ Advanced Configuration (Environment Variables)
You can scale hardware resources, target specific server versions, and enable admin features using environment flags (-e):
- MEMORY_MIN: The initial RAM allocation pool for the Java Virtual Machine (Default: 128M).
- MEMORY_MAX: The maximum allowed RAM boundary before container mitigation (Default: 1520M).
- GC_THREADS: Number of parallel CPU threads dedicated to Java Garbage Collection (Default: 2).
- ASYNC_THREADS: CPU threads allocated specifically for asynchronous chunk generation and loading (Default: 4).
- VIA_VERSION_BUILD: Specifies which build version of ViaVersion to download (Default: 5.11.0).
- TARGET_VERSION: Specifies which Paper Minecraft version to target (e.g., 1.21.1).
- MC_CONTAINER_NAME: The container name used by the Web UI to execute restart operations (Default: disposable-mc-server).
- FLASK_SECRET_KEY: Secret key used to sign web session authentication cookies.

Example with CPU Tuning, Docker Control, and 4GB RAM:

```
docker run -d --name disposable-mc-server \
  --cpus="4.0" \
  -p 25565:25565 \
  -p 19132:19132/udp \
  -p 7777:7777 \
  -e MEMORY_MIN="1G" \
  -e MEMORY_MAX="4G" \
  -e GC_THREADS="2" \
  -e ASYNC_THREADS="4" \
  -e VIA_VERSION_BUILD="5.11.0" \
  -e TARGET_VERSION="1.21.1" \
  -e MC_CONTAINER_NAME="disposable-mc-server" \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ./data:/minecraft/data \
  -v ./credentials:/minecraft/credentials \
  danirali2007/disposable-minecraft-multiplayer
```

## 💾 Data Persistence & Volume Mounts
The image is structurally disposable, meaning the application layer can be safely updated or deleted without losing your world or administrative login settings.
```
-v ./data:/minecraft/data: Maps your local directory to internal server storage for maps, player credentials, and plugin configurations.
-v ./credentials:/minecraft/credentials: Persists web dashboard admin login details and session secrets locally across container reinstalls.
-v /var/run/docker.sock:/var/run/docker.sock: Maps the host machine's Docker daemon socket so authenticated administrators can trigger container restarts directly from the Dashboard UI.
```
💡 Tip: If you wish to run a background tunnel via playit.gg, ensure your playit.toml config file is saved inside ./data (/minecraft/data) to keep your custom static domain mapped between container rebuilds.

## 📖 Guidance for Non-Technical Users
The main goal of this container is convenience: you get a fresh, optimized server installation with the latest plugin builds every time the container boots, while keeping your world data and logins completely safe.
Every restart automatically syncs the latest compatible plugin dependencies.
Server administration (restarts, IP display adjustments, Paper core upgrades) can be managed visually via the Dashboard at `http://localhost:7777` without interacting with the terminal.

<img width="460" height="693" alt="Screenshot 2026-09-06 at 14 02 42" src="https://github.com/user-attachments/assets/5047fbca-f97a-45ec-a9b6-50e7646a1c36" />

