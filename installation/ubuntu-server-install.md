# Instalacja Ubuntu Server 24.04 LTS

System zainstalowany na SSD 120 GB.

## Kroki instalacji

1. Przygotowanie bootowalnego pendrive (Rufus).
2. Instalacja Ubuntu Server 24.04 LTS.
3. Wybranie instalacji minimalnej.
4. Utworzenie użytkownika `serwir`.
5. Instalacja OpenSSH podczas instalacji systemu.

## Po instalacji

Aktualizacja systemu:

```bash
sudo apt update
sudo apt upgrade -y
```
Instalacja podstawowych narzędzi:

```bash
sudo apt install -y curl htop git ufw
```