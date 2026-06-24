# Disposable Minecraft Multiplayer Server ⛏️

A lightweight, isolated, single-container Minecraft server featuring **Paper Spigot** with cross-play support (**Geyser/Floodgate**) and an integrated real-time **Flask Dashboard**. Perfect for setting up high-performance, temporary, or persistent multiplayer environments instantly.

---

## 🚀 Quick Start

To fire up the container with default allocations, run the following command in your terminal:

```
docker run --name disposable-mc-server \
  -p 25565:25565 \
  -p 19132:19132/udp \
  -p 7777:7777 \
  -e MEMORY_MIN="128M" \
  -e MEMORY_MAX="1G" \
  -v ./data:/minecraft/data \
  danirali2007/disposable-minecraft-multiplayer
```

## 📊 Monitoring Dashboard
The container includes an integrated socket-query web management panel that does not rely on external APIs.
To view the status of your Java engine and Bedrock proxy layer, access port 7777 on the host machine's IP address:
👉 http://localhost:7777 (Local)
👉 http://<YOUR_SERVER_IP>:7777 (e.g., http://192.168.0.100:7777)

## 🔌 Network Port Mappings
This container relies on three primary port allocations to handle connections smoothly:
Port	Protocol	Service	Description
25565	TCP	Java Edition	Standard connection port for desktop Minecraft clients.
19132	UDP	Bedrock Edition	Geyser translation tunnel port for mobile, console, and Windows 10/11 clients.
7777	TCP	Flask UI	Web infrastructure panel showing live player counts, performance metrics, and statuses.
## ⚙️ Advanced Configuration (Environment Variables)
You can scale the hardware resources allocated to the Java Virtual Machine dynamically at runtime using environment flags (-e):
•	MEMORY_MIN: The initial RAM allocation pool for the server core (Default: 128M).
•	MEMORY_MAX: The maximum allowed RAM boundary before container mitigation (Default: 1520M).
•	GC_THREADS: Number of parallel CPU threads dedicated to Java Garbage Collection (Default: 2).
•	ASYNC_THREADS: CPU threads allocated specifically for asynchronous chunk generation and loading (Default: 4).
Example with CPU Tuning and 4GB RAM:
```docker run --name disposable-mc-server \
  --cpus="4.0" \
  -e MEMORY_MIN="1G" \
  -e MEMORY_MAX="4G" \
  -e GC_THREADS="2" \
  -e ASYNC_THREADS="4" \
```
## 💾 Data Persistence
The image is structurally disposable, meaning the application layer can be safely updated or deleted. Your maps, player credentials, and configs remain isolated and secure on the host machine via the volume mount flag:
•	-v ./data:/minecraft/data: Maps your local directory to the internal environment data node.
### 💡 Tip: If you wish to run a background tunnel via playit.gg, ensure your playit.toml config file is saved inside this /minecraft/data path to keep your custom static domain mapped between container rebuilds.
