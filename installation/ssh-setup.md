# SSH Setup

SSH używane do zdalnego zarządzania serwerem.

## Instalacja

```bash
sudo apt install openssh-server
```
Status
```bash
sudo systemctl status ssh
```
## Zabezpieczenia
- Logowanie wyłącznie przez klucze SSH
- Wyłączone logowanie root
- Wyłączone hasło

Plik:
/etc/ssh/sshd_config

- PermitRootLogin no
- PasswordAuthentication no
Restart:

```bash
sudo systemctl restart ssh
```