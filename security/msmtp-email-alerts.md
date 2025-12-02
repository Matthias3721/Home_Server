Konfiguracja powiadomień e-mail z Fail2Ban (msmtp + WP)

Dokument opisuje wdrożenie klienta SMTP (msmtp) z serwerem WP oraz integrację z Fail2Ban w celu automatycznego wysyłania szczegółowych alertów o zablokowanych adresach IP.

1. Instalacja wymaganych pakietów
sudo apt update
sudo apt install -y msmtp msmtp-mta mailutils whois geoip-bin

2. Konfiguracja globalna msmtp (/etc/msmtprc)

Plik zawiera ustawienia połączenia SMTP dla konta WP:

defaults
auth           on
tls            on
tls_starttls   on
tls_trust_file /etc/ssl/certs/ca-certificates.crt
logfile        /var/log/msmtp.log

account wp
host smtp.wp.pl
port 587
from "e-mail"
user "e-mail"
password "haslo do aplikacji"

account default : wp


Uprawnienia:

sudo chmod 600 /etc/msmtprc


Test działania:

echo "Test wiadomości" | msmtp "e-mail"

3. Niestandardowa akcja Fail2Ban (msmtp-extended.conf)

Plik:

/etc/fail2ban/action.d/msmtp-extended.conf


Akcja generuje pełny raport o banie (IP, hostname, kraj, ISP, logi):

[Definition]
actionstart =
actionstop =
actioncheck =

actionban = bash -c '
IP="<ip>";
COUNTRY=$(geoiplookup "$IP" | awk -F ": " "{print \$2}");
HOSTNAME=$(host "$IP" 2>/dev/null | awk "/domain name pointer/ {print \$5}");
ISP=$(whois "$IP" 2>/dev/null | grep -Ei "OrgName|organisation|owner" | head -n 1 | sed "s/.*: //");
LOGS=$(grep "$IP" /var/log/auth.log | tail -n 10);

COUNTRY=${COUNTRY:-unknown};
HOSTNAME=${HOSTNAME:-unknown};
ISP=${ISP:-unknown};

printf "Subject: [Fail2Ban] SSH BAN: $IP ($COUNTRY)\n";
printf "From: <sender>\n";
printf "To: <dest>\n\n";

printf "=== Fail2Ban Security Alert ===\n\n";
printf "IP Address:  $IP\n";
printf "Hostname:    $HOSTNAME\n";
printf "Country:     $COUNTRY\n";
printf "ISP:         $ISP\n";
printf "Date:        $(date)\n";
printf "Service:     SSH\n\n";

printf "--- Related log entries ---\n$LOGS\n\n";
' | msmtp -a default <dest>

actionunban = printf "Subject: [Fail2Ban] SSH UNBAN for <ip>\nFrom: <sender>\nTo: <dest>\n\nIP <ip> został odbanowany." | msmtp -a default <dest>

[Init]
sender = "e-mail"
dest = "e-mail"

4. Konfiguracja Fail2Ban (/etc/fail2ban/jail.local)
[DEFAULT]
bantime = 1h
findtime = 10m
maxretry = 5
backend = systemd

sender = "e-mail"
dest = "e-mail"
action = msmtp-extended[name=SSH Ban, dest="%(dest)s"]

[sshd]
enabled = true
port = 22
filter = sshd
logpath = /var/log/auth.log
maxretry = 5


Restart usługi:

sudo systemctl restart fail2ban

5. Test działania powiadomień

Ręczne wywołanie bana:

sudo fail2ban-client set sshd banip 8.8.8.8


Przykład danych w powiadomieniu:

IP: 8.8.8.8

Hostname: dns.google

Country: US

ISP: Google LLC

Logi: ostatnie wpisy z /var/log/auth.log

6. Logi systemowe

Logi SSH: /var/log/auth.log

Logi msmtp: /var/log/msmtp.log

Podsumowanie

Po wdrożeniu konfiguracji:

Fail2Ban automatycznie wysyła pełne raporty o zablokowanych IP,

e-mail zawiera szczegółowe dane (kraj, hostname, ISP, logi),

msmtp działa jako lekki klient SMTP zgodny z WP,

alerty można testować ręcznie poprzez fail2ban-client.