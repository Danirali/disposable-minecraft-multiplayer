# disposable-minecraft-multiplayer
A disposable multiplayer Minecraft server which retains only settings.


To run:
1) Navigate to folder of choice (powershell on windows) `cd ~/Documents/minecraft-server`
2) Run server using command `docker run -p 19132:19132 -p 25565:25565 --name=disposable-mc-server -v ./data:/data -e MEMORY=2G danirali2007/disposable-minecraft-multiplayer`

This server returns world data and settings, you just have to delete and re-run the docker command to update to last paper version.

To ensure ease-of-use, geyser and floodgate are pre-installed for multiplayer and crossplatform play.
