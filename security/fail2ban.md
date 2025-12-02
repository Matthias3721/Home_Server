Fail2Ban – zabezpieczenie serwera przed próbami brute-force (opis konfiguracji)

Na serwerze Ubuntu został wdrożony Fail2Ban jako warstwa ochronna monitorująca logi systemowe i automatycznie blokująca adresy IP powodujące liczne błędy logowania. Konfiguracja obejmuje również w pełni działające powiadomienia e-mail wysyłane przez msmtp.

Instalacja i włączenie usługi
```bash
sudo apt update
sudo apt install -y fail2ban
```


Fail2Ban działa jako usługa systemowa:

```bash
sudo systemctl status fail2ban
```

Konfiguracja główna – jail.local

Wszystkie ustawienia zostały wprowadzone w:

```bash
/etc/fail2ban/jail.local
```


Przykład zastosowanej konfiguracji z aktywnymi powiadomieniami:

```bash
[DEFAULT]
bantime  = 1h
findtime = 10m
maxretry = 5
backend  = systemd
```

# Integracja z msmtp – e-mail działa poprawnie
sender = "alerty@twojadomena.pl"
dest   = "mojemail@example.com"
```bash
action = msmtp-extended[name=SSH Ban, dest="%(dest)s", sender="%(sender)s"]
```

```bash
[sshd]
enabled  = true
port     = 22
filter   = sshd
logpath  = /var/log/auth.log
maxretry = 5
```


W tej konfiguracji Fail2Ban wysyła e-mail za każdym razem, gdy adres IP zostanie zbanowany w jailu SSH.

Monitorowanie działania

Sprawdzenie ogólnego statusu:

```bash
sudo fail2ban-client status
```


Sprawdzenie konkretnego jaila:

```bash
sudo fail2ban-client status sshd
```


Ręczny ban i unban:

```bash
sudo fail2ban-client set sshd banip 1.2.3.4
sudo fail2ban-client set sshd unbanip 1.2.3.4
```

Logi Fail2Ban

Główne logi znajdują się w:

```bash
/var/log/fail2ban.log
```


Logi SSH:

```bash
/var/log/auth.log
```


Podgląd:

```bash
sudo tail -f /var/log/fail2ban.log
```

Wprowadzanie zmian

Po edycji konfiguracji Fail2Ban:

```bash
sudo systemctl restart fail2ban
sudo systemctl status fail2ban
```

Zastosowane zabezpieczenia towarzyszące

Wraz z Fail2Ban używam również:

- logowania wyłącznie kluczami SSH,
- zapory UFW ograniczającej liczbę otwartych portów,
- możliwości zmiany portu SSH (opcjonalnie),
- powiadomień e-mail o banach (w pełni działających).

Dokumenty powiązane:
- msmtp-email-alerts.md — konfiguracja klienta pocztowego dla powiadomień
- ssh-keys.md — klucze SSH
- ufw-firewall.md — zapora UFW

Podsumowanie

Fail2Ban skutecznie monitoruje logi SSH i automatycznie blokuje podejrzane adresy IP. Wdrożona konfiguracja, połączona z wysyłaniem powiadomień e-mail przez msmtp, zapewnia bieżącą informację o wykrywanych atakach i podwyższa bezpieczeństwo całego środowiska.