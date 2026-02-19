# Home Server za 100 zł – od złomu z OLX do pełnoprawnego serwera

Ten projekt dokumentuje proces stworzenia domowego serwera z taniego komputera kupionego za 100 zł na OLX.  
Celem jest zbudowanie stabilnego serwera działającego 24/7, który w przyszłości będzie hostował:

- serwer Minecraft Forge,
- prywatną chmurę (Nextcloud),
- bota Telegram,
- stronę WWW,
- inne usługi działające w Dockerze.

Projekt ma pokazać, że tani komputer z drugiej ręki może stać się w pełni funkcjonalnym serwerem domowym.

<p align="center">
  <img src="hardware/photos/IMG_5915.jpg" width="500" alt="Komputer kupiony za 100 zł">
</p>

---

## Sprzęt

Szczegółowy opis i zdjęcia znajdują się w `hardware/hardware.md`.

Konfiguracja początkowa (100 zł):
- HP Compaq Elite 8300 SFF  
- Intel Core i5-3470  
- 8 GB RAM DDR3  
- HDD 500 GB  
- Windows 7 (do usunięcia)

### Modyfikacje / Upgrade:

#### Dodanie RAM (16 GB)
<p align="center">
  <img src="hardware/photos/IMG_5912.jpg" width="500" alt="Rozbudowa RAM do 16 GB">
</p>

#### Instalacja SSD 120 GB na system
<p align="center">
  <img src="hardware/photos/IMG_5914.jpg" width="500" alt="Instalacja SSD">
</p>

#### Pozostałe modyfikacje:
- wymiana pasty termicznej  
- czyszczenie wnętrza obudowy  
- wymiana wentylatora na cichszy (Arctic F9)  
- test stabilności i temperatur 
---

## Instalacja oprogramowania

Opis instalacji znajduje się w folderze `installation/`.

Etapy instalacji:
1. Przygotowanie pendrive z Ubuntu Server 24.04.3 LTS  
2. Konfiguracja statycznego adresu IP  
3. Instalacja OpenSSH  
4. Podział dysku (system na SSD, HDD jako przestrzeń danych)  
5. Pierwsze logowanie przez SSH z innego komputera

---

## Docker i usługi

Dokumentacja konfiguracji Dockera oraz kontenerów będzie znajdować się w folderze `services/`.

Planowane usługi:
- serwer Minecraft (Forge / Fabric / Paper)
- prywatna chmura Nextcloud
- bot Telegram
- reverse proxy (Traefik lub NGINX Proxy Manager)
- monitoring (np. Netdata)
- dodatkowe usługi działające w kontenerach

---

## Cel projektu

Celem jest przedstawienie procesu budowy domowego serwera za niewielkie pieniądze oraz zdobycie doświadczenia z zakresu:

- administracji Linux,
- konfiguracji i utrzymania serwerów,
- Dockera i konteneryzacji,
- zarządzania usługami przy użyciu narzędzi DevOps,
- automatyzacji środowiska i wdrożeń (Docker Compose, planowany CI/CD),
- hostowania usług 24/7 i monitoringu.


---

## Postęp prac

Aktualne zdjęcia i etapy instalacji znajdują się w `hardware/photos/`.

---

## Status projektu (stan na 2026-02-19)

- **Minecraft Forge** (Docker) — dostęp tylko lokalnie/Tailscale
- **deal-monitor** (Spring Boot + PostgreSQL) — działa w Docker Compose
- **mobilfix** (PHP/Apache w Docker) — działa (formularz `send.php`)
- **Cloudflare Tunnel** — wystawianie usług na zewnątrz bez otwierania portów

---

## Licencja

MIT License.
