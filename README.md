# Disposable Minecraft Multiplayer Server ⛏️

### Planning on running multiple servers? [Click here](#running-multiple-servers-with-velocity)

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
  danirali2007/disposable-minecraft-multiplayer:amd64
```
X64 / X86 / Intel / AMD Systems:
```
docker run -d --name mc-server -p 25565:25565 -p 19132:19132/udp -p 7777:7777 -e MEMORY_MIN="128M" -e MEMORY_MAX="2G" -e VIA_VERSION_BUILD="5.11.0" -e MC_CONTAINER_NAME="mc-server" -v /var/run/docker.sock:/var/run/docker.sock -v ./data:/minecraft/data danirali2007/disposable-minecraft-multiplayer:amd64
```
ARM64 Systems:
```
docker run -d --name mc-server -p 25565:25565 -p 19132:19132/udp -p 7777:7777 -e MEMORY_MIN="128M" -e MEMORY_MAX="2G" -e VIA_VERSION_BUILD="5.11.0" -e MC_CONTAINER_NAME="mc-server" -v /var/run/docker.sock:/var/run/docker.sock -v ./data:/minecraft/data danirali2007/disposable-minecraft-multiplayer:arm64
```
Apple Silicon Macs:
```
docker run -d --name mc-server -p 25565:25565 -p 19132:19132/udp -p 7777:7777 -e MEMORY_MIN="128M" -e MEMORY_MAX="2G" -e VIA_VERSION_BUILD="5.11.0" -e MC_CONTAINER_NAME="mc-server" -v /var/run/docker.sock:/var/run/docker.sock -v ./data:/minecraft/data danirali2007/disposable-minecraft-multiplayer:apple-silicon
```

## 📊 Monitoring Dashboard
The container includes an integrated socket-query web management panel that does not rely on external APIs. It allows you to monitor live player sessions, execute online Paper JAR updates, change display IPs, and restart the container directly from the web interface.
To view the status of your Java engine and Bedrock proxy layer, access port 7777 on your browser:
<br>
👉 Local: http://localhost:7777
<br>
👉 Network: http://<YOUR_SERVER_IP>:7777 (e.g., http://192.168.0.100:7777)

<br>Default Admin Credentials:
```
Username: admin
Password: minercaft-admin
```

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
  -e TARGET_VERSION="26.2" \
  -e MC_CONTAINER_NAME="disposable-mc-server" \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v ./data:/minecraft/data \
  -v ./credentials:/minecraft/credentials \
  danirali2007/disposable-minecraft-multiplayer
```

## 💾 Data Persistence & Volume Mounts
The image is structurally disposable, meaning the application layer can be safely updated or deleted without losing your world or administrative login settings. However, the option to update the server without deleting the container is available from the UI.
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

---

# Running multiple servers with Velocity

Run as a docker compose:

`docker-compose.yml`
<br>
```
name: minecraft-servers
networks:
  mc-network:
    driver: bridge

services:
  mc-server-0:
    image: danirali2007/disposable-minecraft-multiplayer:amd64
    container_name: mc-server-0
    restart: unless-stopped
    ports:
       - "7777:7777"
       - "8100:8100"
    environment:
      - MEMORY_MIN=128M
      - MEMORY_MAX=2G
      - VIA_VERSION_BUILD=5.11.0
      - MC_CONTAINER_NAME=mc-server-0
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./survival/data:/minecraft/data
    networks:
      - mc-network
  
  mc-server-1:
    image: danirali2007/disposable-minecraft-multiplayer:amd64
    container_name: mc-server-1
    restart: unless-stopped
    ports:
       - "7778:7777"
       - "8101:8100"
    environment:
      - MEMORY_MIN=128M
      - MEMORY_MAX=2G
      - VIA_VERSION_BUILD=5.11.0
      - MC_CONTAINER_NAME=mc-server-1
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./city/data:/minecraft/data
    networks:
      - mc-network

  velocity-proxy:
    image: danirali2007/velocity-proxy:amd64
    container_name: velocity-proxy
    restart: unless-stopped
    ports:
      - "25565:25565"
      - "19132:19132/udp"
    environment:
      - MEMORY_MIN=64M
      - MEMORY_MAX=512M
    volumes:
      - ./velocity-config:/velocity/runtime
    networks:
      - mc-network
```

In the folder velocity-config, save latest velocity and save as velocity.jar, add file forwarding.secret with your password and velocity.toml with your config.

Example velocity.toml:
```
bind = "0.0.0.0:25565"
motd = "Welcome to the Network!"
show-max-players = 20
online-mode = true

player-info-forwarding-mode = "MODERN"
forwarding-secret-file = "forwarding.secret"

[servers]
  survival = "mc-server-0:25565"
  city = "mc-server-1:25565"

try = [
  "survival"
]

[forced-hosts]
  "sub.duckdns.org" = ["survival"]
  "city.sub.duckdns.org" = ["city"]
```

For each server enter their respective folders and append paper-world.yml:
```
proxies:
  velocity:
    enabled: true
    online-mode: true
    secret: "your-random-secret-key-123"
```

velocity.toml
```
#If you are using modern or BungeeGuard IP forwarding, configure a file that contains a unique secret here.
#The file is expected to be UTF-8 encoded and not empty.
forwarding-secret-file = "forwarding.secret"
config-version = "2.9"
#Should the proxy enforce the new public key security standard? By default, this is on.
force-key-authentication = true
# What should be the MOTD? This gets displayed when the player adds your server to
# their server list. Only MiniMessage format is accepted.
motd = "\\<#09add3>A Velocity Server"

[servers]
	survival = "mc-server-0:25565"
	city = "mc-server-1:25565"
	try = ["survival"]

[forced-hosts]
	"server.duckdns.org" = ["survival"]
	"server2.server.duckdns.org" = ["city"]

[advanced]
	accepts-transfers = true

[packet-limiter]
	#Size of the moving time window in seconds used to calculate average rates.
	#A larger window tolerates short bursts while still enforcing the configured limits over time.
	interval = 7
	#Maximum average number of packets per second a client may send. -1 disables this check.
	packets-per-second = -1
	#Maximum average number of compressed (on-wire) bytes per second a client may send. -1 disables this check.
	bytes-per-second = -1
	#Maximum average number of decompressed bytes per second a client may send.
	#Protects against compression bomb attacks where small packets expand to excessive sizes after decompression.
	#-1 disables this check.
	decompressed-bytes-per-second = 5242880

[ping-passthrough]
	# Should Velocity pass the version number from the backend server when responding to server list ping requests?
	version = false
	# Should Velocity pass the player count from the backend server when responding to server list ping requests?
	players = false
	# Should Velocity pass the description from the backend server when responding to server list ping requests?
	description = false
	# Should Velocity pass the favicon (also known as the server icon) from the backend server when responding to server list ping requests?
	favicon = false
	# Should Velocity pass the mod list from the backend server when responding to server list ping requests?
	modinfo = false
```
