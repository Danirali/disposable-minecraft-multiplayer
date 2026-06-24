# Run Server
`docker run --name disposable-mc-server -p 19132/udp -p 25565:25565 -p 7777:7777 -e MEMORY_MIN="128M" -e MEMORY_MAX="1G" -v ./data:/minecraft/data danirali2007/disposable-minecraft-multiplayer`

# View Dashboard
To view the dashboard, access the port `7777` on the IP address the container is running on. For example: `192.168.0.100:7777`

# Ports
This container relies on three ports to run smoothly;
1) Java port (25565)
2) Bedrock port (19132)
3) Dashboard (7777)
