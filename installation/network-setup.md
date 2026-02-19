# Network Setup

Serwer działa w sieci lokalnej z przypisanym stałym adresem IP.

## Konfiguracja Netplan

Plik:
`/etc/netplan/00-installer-config.yaml`

Przykład:

```yaml
network:
  version: 2
  ethernets:
    enp3s0:
      dhcp4: no
      addresses:
        - 192.168.1.100/24
      gateway4: 192.168.1.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 1.1.1.1
Zastosowanie zmian:

```bash
netplan apply
```
## Dodatkowo:
Tailscale do zdalnego dostępu
Cloudflare Tunnel do publikacji usług