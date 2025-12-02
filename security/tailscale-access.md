Zdalny dostęp do serwera przez Tailscale — opis konfiguracji z przykładami

Na domowym serwerze z Ubuntu 24.04 wdrożyłem Tailscale jako rozwiązanie do zdalnego dostępu oraz bezpiecznego tunelowania ruchu w publicznych sieciach. Tailscale tworzy prywatną, szyfrowaną sieć opartą na WireGuardzie, dzięki czemu serwer staje się dostępny z dowolnego miejsca bez przekierowywania portów ani używania publicznego IP.

Instalacja i uruchomienie usługi

Tailscale został zainstalowany i aktywowany poprzez:

curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up


Po zalogowaniu urządzenie pojawiło się w mojej sieci Tailnet jako serwer domowy.

Stan klienta i przydzielone adresy można sprawdzić:

tailscale status
tailscale ip -4


Usługa działa jako jednostka systemowa:
sudo systemctl status tailscaled

Konfiguracja MagicDNS i nazwy hosta

W panelu Tailscale nadałem urządzeniu nazwę homeserver oraz włączyłem MagicDNS. Dzięki temu mogę łączyć się z serwerem przez SSH:

ssh serwir@homeserver

lub bezpośrednio po adresie z puli Tailscale:
ssh serwer@100.x.x.x


Klucz dostępu urządzenia został ustawiony na never expires, aby uniknąć ponownej autoryzacji.

Exit Node i trasy sieciowe

Serwer został skonfigurowany jako exit node, co umożliwia tunelowanie mojego ruchu internetowego przez domowe łącze — przydatne np. w publicznych Wi-Fi:
sudo tailscale up --advertise-exit-node

Do tego konieczne było włączenie routingu IPv4, poprzez edycję systemowej konfiguracji:

Plik:
/etc/sysctl.conf

Zmieniona linia:
net.ipv4.ip_forward=1

Zastosowanie ustawień:
sudo sysctl -p


Na innych urządzeniach akceptuję trasy:
sudo tailscale up --accept-routes

Zastosowania i efekty konfiguracji

Po wdrożeniu Tailscale uzyskałem:

- stały zdalny dostęp do serwera z dowolnej sieci,

- brak konieczności otwierania portu 22 na routerze,

- zabezpieczone, szyfrowane połączenia SSH,

- prywatną przestrzeń adresową dostępną na wszystkich moich urządzeniach,

- możliwość korzystania z domowego IP jako exit node (np. w hotelach, na uczelni, w hotspotach).

Konfiguracja jest nieinwazyjna dla systemu — Tailscale nie modyfikuje krytycznych plików poza użyciem własnej usługi tailscaled.

Dodatkowe komendy administracyjne

Restart klienta:

sudo tailscale down
sudo tailscale up


Wyłączenie exit node / reset opcji:
sudo tailscale up --reset


Wylistowanie urządzeń w sieci:
tailscale status