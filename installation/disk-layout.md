# Disk Layout

## Dyski

- SSD 120 GB – system (Ubuntu)
- HDD 500 GB – dane

## Schemat

SSD:
- `/` – system

HDD:
- `/data` – przechowywanie danych usług
- Docker volumes
- Minecraft world

## Sprawdzenie partycji:

```bash
lsblk
df -h
```