# Installatie Samba - delen van bestanden
Als je bestanden die in je VM zitten wilt gebruiken op je laptop/PC dan moet je Samba gebruiken. 
Samba is een naam die gebaseerd is het SMB-protocol: Server Message Block. Dit protocol loopt over poort
445 en is eenvoudig te installeren via de command-line in Linux.

Als de software eenmaal geinstalleerd is moet je nog wel wat kleine zaken regelen in de welbekende .conf-bestanden.

## Installatie

```bash
  root@risksec:~# apt install samba
```

## Configuratie
De configuratie voor Samba is opgeslagen in het bestand `/etc/samba/smb.conf`. Je moet de volgende aanpassingen doen
om je HOME-drive (bijv `/home/martin`) te kunnen gebruiken met lezen+schrijven.

 1. Zoek de sectie  `[homes]`
 1. blader door totdat je ziet staan `read only = yes`
 1. verander deze naar `read only = no`
 1. blader eventueel door naar de entry `valid users`
 1. verander de standaard instelling eventueel naar de namen van gebruikers, gescheiden door een spatie.
 
Verlaat je editor met opslaan van de wijzigingen en herstart de service voor Samba (zorg dat je root bent). Controleer
vervolgens de instellingen met `testparm -L`. Je kunt hiermee alle instellingen en gedeelde mappen zien.
```bash
  root@risksec:~# service smbd restart
  root@risksec:~# testparm -L
.........snipped............
[homes]
	comment = Home Directories
	create mask = 0775
	read only = No
	valid users = martin 
.........snipped............

```
## Gebruikers toestaan in te loggen
Als een gebruiker toegang wil hebben tot Samba, dan moet er voor de gebruiker een apart wachtwoord opgegeven worden.
Dat doe je met het commando `smbpassword`. In onderstaande commando wordt voor de gebruiker `martin` een nieuw 
wachtwoord voor Samba aangemaakt. De optie `-a` zorgt voor deze toevoeging ('add'). Je kunt later hiermee ook 
wachtwoorden aanpassen. Zie manual page voor dit commando voor meer informatie (`man smbpasswd`).

```bash
  root@risksec:~# smbpasswd -a martin
```

## Eigen share toevoegen voor website
Als je website niet in je home-directory staat, dan kan het nuttig zijn om een aparte share te maken. Voeg eenvoudig
aan het einde van het bestand `/etc/samba/smb.conf` een nieuwe sectie toe. Bijvoorbeeld:

```text
  [website]
  browseable = yes
  path = /var/www/mijnwebsite
  valid users = martin
  force group = www-data
  create mask = 660
  directory mask = 770
```

Eén van de problemen waar je tegen aan loopt bij het op afstand maken van bestanden, is dat deze zonder extra maatregelen 
waarschijnlijk niet leesbaar zijn voor de webserver Apache2. Dit proces maakt namelijk gebruik van een user met naam
`www-data`. Daarom zie je in bovenstaande daarvoor een drietal maatregelen:

  1. `force group = www-data` : doe net alsof de ingelogd gebruiker lid is van de groep `www-data`. Nieuwe bestanden
  worden dan aangemaakt met als groep `www-data`. Op deze manier kun je zorgen dat de webserver de bestanden 
  wel mag gebruiken, mits je de rechten goed instelt. Dat regelen de volgende twee items.
  1. `create mask` = 660. Dit zorgt er voor dat aangemaakte 'normale' bestanden de file-toegang goed heeft staan: `-rw-rw----`
      1. eerste set `rw-` de eigenaar mag lezen, schrijven en uitvoeren
      1. tweede set `rw-` de groep `www-data` mag lezen, schrijven en uitvoeren
      1. derde set `---` anderen (zijnde niet de eigenaar en groep) hebben geen rechten
  1. `directory mask = 770` als een directory aangemaakt wordt, dan krijgt deze de rechten `drwxwrx---`.
      1. eerste set `rwx` de eigenaar mag lezen, schrijven en de map betreden (inhoud zien)
      1. tweede set `rwx` de groep `www-data` mag lezen, schrijven en de map betreden (inhoud zien)
      1. derde set `---` anderen (zijnde niet de eigenaar en groep) hebben geen rechten


Vergeet niet je instellingen te controleren!
```bash
  root@risksec:~# service smbd restart
  root@risksec:~# testparm -L

```
