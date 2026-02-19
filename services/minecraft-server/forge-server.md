# Minecraft Server (Forge)

Serwer Minecraft działa w Dockerze.

## Lokalizacja na serwerze
~/docker/minecraft/

## Port
25565 (dostęp tylko LAN / Tailscale)

## Uruchamianie
```bash
cd ~/docker/minecraft
docker compose up -d
```
## Backup

Wykorzystuje skrypt:

~/mc_backup.sh