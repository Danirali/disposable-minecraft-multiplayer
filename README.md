# Run Server
`docker run --name mc-paper-server --net=host -v ./data:/data -e TYPE=PAPER -e VERSION=LATEST -e EULA=TRUE -e MEMORY=1520M -e INNIT=MEMORY=128M itzg/minecraft-server`

# Download Latest Plugins
```
curl -o /plugins/ViaVersion.jar "https://hangar.papermc.io/api/v1/projects/ViaVersion/ViaVersion/versions/latest/download" && \
curl -o /plugins/Geyser-Spigot.jar "https://download.geysermc.org/v2/projects/geyser/versions/latest/builds/latest/downloads/spigot" && \
curl -o /plugins/Floodgate-Spigot.jar "https://download.geysermc.org/v2/projects/floodgate/versions/latest/builds/latest/downloads/spigot"
```
