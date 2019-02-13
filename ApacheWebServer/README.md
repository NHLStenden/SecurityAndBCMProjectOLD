# Configuratie Apache WebServer

Hier gaan we aan de slag met het configureren van de Apache WebServer. Het doel is 
  * Maak een virtuele host
  * Zorg voor een leesbare URL
  * Koppel de URL aan de virtuele host
  
In de taal van de Apache WebServer is een `virtuele host` niks meer dan een configuratie-bestand (`.conf`) 
die er voor zorgt dat je allerlei aspecten van een website kunt instellen. Denk daarbij aan:
  * naar welke URL moet ik 'luisteren'?
  * waar staan de bestanden?
  * wat moet ik allemaal toestaan of weigeren ?
    * gebruikers
    * aanpassingen via .htaccess
  * wat moet ik loggen ?
  * waar moet ik logbestanden opslaan?
  * custom error pagina's 
  * ....
  
We installeren de webserver via een bash-commando:
```bash
  $ sudo apt install apache2
```

Accepteer alle gesuggereerde pakketten en start de installatie. 

## Basis configuratie
Na de installatie is er slechts een basis-site. Hier gaan we wat aan doen. Open als `root` een editor, zoals 
nano (Terminal), Scite (GUI) of gedit (GUI). In deze editor gaan we een nieuw bestand maken genaamd `samenfit.conf`:

```bash
  $ sudo gedit /etc/apache2/sites-available/samenfit.conf
```

De meest basis configuratie ziet er ongeveer zo uit:
```apacheconf
 <VirtualHost  *:80>
   DocumentRoot /home/martin/samenfit/
   ServerName samenfit.local
   
   ErrorLog "/var/log/apache2/error.samenfit.log"
   CustomLog "/var/log/apache2/access.samenfit.log" common
   loglevel error

   <Directory "/home/martin/samenfit/">
      Require all granted
   </Directory>
  </VirtualHost>
```

Van boven naar beneden enige uitleg. De configuratie is opgeslagen tussen `<VirtualHost>` en `</VirtualHost>`.
De stijl van schrijven lijkt een beetje op XML, maar is verder nogal losjes. 

Bij de `<VirtualHost>` staat een poort waar naar geluisterd wordt. Poort nummer 80 is de standaard. Het `*`
betekent dat er naar alle IP-adressen wordt geluisterd. Zie bij de [Referenties](#referenties) onderaan deze pagina voor meer informatie 
rondom configuratie van virtuele hosts.

De `DocumentRoot` geeft aan op welk pad (absoluut) de bestanden van de webserver gevonden kunnen worden. 

De `ServerName` is belangrijk! De waarde van deze instelling is de URL die de gebruiker in moet typen. 

**Let op**: Deze URL is niet magisch beschikbaar als je deze configuratie activeert! Je moet zelf de URL via
DNS of hosts (`/etc/hosts` ) inregelen!! Zie onderaan deze instructie bij de [Referenties](#referenties).

De instelling `ErrorLog` geeft de locatie van de Error-log: in dit bestand worden de fouten gelogd. Wat er wel
of niet gelogd moet worden kun je instellen met `loglevel` . Tijdens ontwikkelen van een website kun je deze 
bijvoorbeld op `debug` zetten voor zeer uitgebreide informatie (let op je disk ruimte!).

De instelling `CustomLog` geeft aan waar de toegangsverzoeken worden gelogd. Zo kun je elke URL zien die
de webserver behandeld, wat het resultaat is en hoe snel het verzoek is afgehandeld.

Vervolgens worden van een specifieke directory (map) van het bestandssysteem aangegeven wat de instellingen
voor die map zijn. Dit doe je met het directive `<Directory>`. In deze simpele variant wordt alle verzoeken / gebruikers
toegestaan. Zie [Referenties](#referenties) (Apache toegangsbeveiliging) voor meer informatie.

## Configuratie activeren
Nu we een bestand hebben gemaakt, moeten we deze configuratie nog activeren. Dat doe je met het commando `a2ensite` (
Apache2 enable site). Je kunt het bestand dat je wilt activeren opgeven, of interactief werken:

```bash
  $ sudo a2ensite samenfit.conf
  Enabling site samenfit.
  To activate the new configuration, you need to run:
    service apache2 reload
  $ 
```
of interactief:
```bash
  $ sudo a2ensite 
  Your choices are: 000-default default-ssl samenfit.conf 
  Which site(s) do you want to enable (wildcards ok)?
  samenfit*
  Enabling site samenfit.
  To activate the new configuration, you need to run:
    service apache2 reload
  $
```

Na afloop moet je nog de webserver herladen (`reload`) of herstarten (`restart`):
```bash
  $ sudo service apache2 restart
```

## Fouten bij herstarten webserver
Soms krijg je fouten bij het starten/herladen van de configuratie van de webserver. Een typische foutmelding is dan:
```bash
Job for apache2.service failed. See 'systemctl status apache2.service' and 'journalctl -xn' for details.
```
Voer in dat geval het gesuggeerde commando uit:
```bash
 $ systemctl status apache2.service
 ● apache2.service - LSB: Apache2 web server
    Loaded: loaded (/etc/init.d/apache2)
   Drop-In: /lib/systemd/system/apache2.service.d
            └─forking.conf
    Active: failed (Result: exit-code) since Fri 2019-02-08 15:17:05 CET; 1min 30s ago
   Process: 6657 ExecStop=/etc/init.d/apache2 stop (code=exited, status=0/SUCCESS)
   Process: 6442 ExecReload=/etc/init.d/apache2 reload (code=exited, status=0/SUCCESS)
   Process: 6686 ExecStart=/etc/init.d/apache2 start (code=exited, status=1/FAILURE)
 
 Feb 08 15:17:05 audioplayer apache2[6686]: Starting web server: apache2 failed!
 Feb 08 15:17:05 audioplayer apache2[6686]: The apache2 configtest failed. ... (warning).
 Feb 08 15:17:05 audioplayer apache2[6686]: Output of config test was:
 Feb 08 15:17:05 audioplayer apache2[6686]: AH00526: Syntax error on line 24 of /etc/apache2/sites-enabled/samenfit.conf:
 Feb 08 15:17:05 audioplayer apache2[6686]: AuthLDAPBindDN takes one argument, DN to use to bind to LDAP server. If not provided, will do an anonymous bind.
 Feb 08 15:17:05 audioplayer apache2[6686]: Action 'configtest' failed.
 Feb 08 15:17:05 audioplayer apache2[6686]: The Apache error log may have more information.
 Feb 08 15:17:05 audioplayer systemd[1]: apache2.service: control process exited, code=exited status=1
 Feb 08 15:17:05 audioplayer systemd[1]: Failed to start LSB: Apache2 web server.
 Feb 08 15:17:05 audioplayer systemd[1]: Unit apache2.service entered failed state.
```


## Advanced configuration
De volgende stap is het opnemen van beperkingen in de toegang: we willen niet dat alle gebruikers zomaar
op bepaalde beveiligde mappen mogen kijken. We gebruiken hiervoor de LDAP-identity store die we eerder 
hebben geinstalleerd. 

Het komt er op neer dat een gebruiker 
  * geauthenticeerd moet zijn (bewijs van wie je bent)
  * geautoriseerd zijn (wat mag je?)

Dat betekent dat we toegang tot een submap `intranet` in onze webserver omgeving gaan beveiligen. Een gebruiker
moet eerst inloggen zodat we na het inloggen kunnen controleren of die gebruiker ook in een bepaalde 
autorisatiegroep zit. Zo niet, dan krijgt de webserver domweg geen toegang tot de bestanden in die map.

**Merk op dat** je hiervoor dus geen enkele regel in je website-code voor hoeft op te nemen! Als de webserver de pagina
laadt, dan zijn de identiteit & autorisatieniveau reeds vastgesteld.

We passen de configuratie aan. Deze nieuwe configuratie opgeslagen in een los bestand, omdat deze nogal 
uitgebreid is. Het bestand is [hier](files/samenfit.conf) te vinden. 

Een paar opvallende zaken:
  * Om een gebruiker te kunnen zoeken is toegang nodig tot de LDAP-service. Hiervoor zijn een gebruikersnaam
  en wachtwoord nodig. Om te voorkomen dat je een plain-tekst wachtwoord moet opnemen in configuratie 
  gebruik je onderstaande instellingen:
  
```apacheconf
    # Which user is used to do an initial BIND to the LDAP Provider (hardly ever anonymous access is granted)
    AuthLDAPBindDN "cn=webuserldap,ou=users,ou=SamenFit,dc=samenfit,dc=local"
    AuthLDAPBindPassword "exec:/bin/cat /root/website/passwd"
```
Je zet het wachtwoord dus in een aparte file `/root/website/passwd`, die je goed beveiligt. Er zijn ook 
andere veiligere methodes om zo'n wachtwoord kenbaar te maken op deze manier. Kijk 
[hier](https://httpd.apache.org/docs/2.4/mod/mod_authnz_ldap.html#authldapurl) voor meer informatie.   

**Let op**: er wordt bij elke inlogpoging van een gebruiker 2x toegang gevraagd richting de LDAP-service:
  1. De webserver heeft toegang nodig om de gebruiker te zoeken
  2. nadat de gebruiker is gevonden, wordt gepoogd een `ldap bind` te doen met de gevonden gebruiker en het wachtwoord
  dat de gebruiker heeft ingevoerd.
  
In de logfiles is dat verschil niet altijd duidelijk. Als je een foutmelding krijgt dat de gebruiker niet gevonden kan
worden, heeft dat vaak ook met toegang van **stap 1** te maken!

## Testen en testen en testen en...
Het werkend krijgen van deze configuratie is niet eenvoudig. Een paar tips:
  * Zet het loglevel van de LDAP-module op DEBUG: `loglevel error authnz_ldap:debug`
  * als je een configuratie wijzigt, herstart dan de webserver : `sudo service apache2 restart`.
  dit zorgt er voor dat de cache van LDAP geleegd wordt en je weer met schone wachtwoorden begint.
  * monitor de logfile met `tail -f /var/log/apache2/error.samenfit.log`
  * als je Apache2 herstart, en je krijgt een foutmelding dan staat er een commando dat je kunt uitvoeren
  om problemen te onderzoeken. Maak daar gebruik van en lees zorgvuldig de output van dat commando!
  * test eventueel je configuratie van Apache met `apache2 -S `
  
Veel voorkomende problemen:
  * encryptie van wachtwoorden is onjuist
  * wachtwoord fouten
  * locaties van bestanden / mappen deugen niet (DocumentRoot)


# Referenties <a name="referenties">
  * [Apache Virtual Hosts config](https://httpd.apache.org/docs/2.4/vhosts/examples.html)
  * [Linux: How to edit hosts file](https://manpages.ubuntu.com/manpages/bionic/en/man5/hosts.5.html)
  * [Apache 2.4 Core](https://httpd.apache.org/docs/2.4/mod/core.html)
  * [Apache 2.4 logging](http://httpd.apache.org/docs/current/mod/mod_log_config.html)
  * [Apache toegangsbeveiliging](https://httpd.apache.org/docs/2.4/howto/access.html)