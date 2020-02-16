# Installatie van PHP als onderdeel van Apache2

```bash
  root@risksec: apt install php7.3 php7.3-ldap libapache2-mod-php
```
Wijzig de instellingen van PHP.ini. Deze kun je vinden op /etc/php/7.3/apache2

```bash
  root@risksec: nano /etc/php/7.3/apache2/php.ini
```

Als je in Developer modus werkt zijn onderstaande instellingen handig.
 1. short_open_tag=On
 1. display_errors=On

Voor een productie omgeving zijn onderstaande aan te raden ("Server Hardening"):
 1. short_open_tag=On
 1. display_errors=Off
 
