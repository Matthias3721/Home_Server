UFW Firewall – Konfiguracja zabezpieczeń serwera (wersja skondensowana)

Dokument opisuje zastosowaną konfigurację zapory UFW na serwerze, której celem jest ograniczenie powierzchni ataku, kontrola dostępu do usług oraz logowanie prób połączeń.

1. Instalacja (jeśli brak)
sudo apt update
sudo apt install -y ufw

2. Domyślne polityki bezpieczeństwa

Cały ruch przychodzący jest blokowany, a ruch wychodzący dozwolony:

sudo ufw default deny incoming
sudo ufw default allow outgoing

3. Dostęp do SSH

Domyślny port:

sudo ufw allow 22/tcp


Jeśli SSH działa na innym porcie (np. 2222):

sudo ufw allow 2222/tcp

4. Dostęp do dodatkowych usług (wg potrzeb)

Najczęściej używane reguły:

HTTP/HTTPS
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp

DNS (własny serwer DNS)
sudo ufw allow 53

Serwer Minecraft
sudo ufw allow 25565/tcp

Reverse proxy (np. Traefik, NPM)
sudo ufw allow 8080/tcp

5. Włączenie zapory
sudo ufw enable


Status i szczegółowe reguły:

sudo ufw status verbose

6. Logowanie ruchu

Aktywacja logów:

sudo ufw logging on


Logi dostępne są w:

/var/log/ufw.log

7. Dodatkowe zabezpieczenia systemowe (opcjonalne)

Reguły minimalizujące ryzyko skanowania i floodu SYN znajdują się w:

/etc/ufw/sysctl.conf


Dodane ustawienia:

net/ipv4/tcp_syn_retries=2
net/ipv4/conf/all/rp_filter=1
net/ipv4/conf/default/rp_filter=1


Zastosowanie zmian:

sudo ufw reload

8. Reset UFW (w razie problemów)
sudo ufw disable
sudo ufw reset
sudo ufw enable

Podsumowanie konfiguracji

Po wdrożeniu ustawień:

zapora blokuje cały nieautoryzowany ruch przychodzący,

dostępne są tylko określone porty i usługi,

aktywne logowanie pozwala monitorować próby połączeń,

UFW startuje automatycznie z systemem,

konfiguracja jest prosta do modyfikacji i rozszerzenia.