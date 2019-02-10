# OpenLDAP installatie en configuratie.
Het OpenLDAP product geeft ons de mogelijkheid om gebruikers veilig te administreren. Deze gebruikers kunnen we dan 
autoriseren voor bijvoorbeeld onze webserver. 

## Installatie Apache & OpenLDAP  
```bash
$ sudo apt install slapd
```
Accepteer eventuele gesuggereerde pakketten en start de installatie. 

# Configureren OpenLDAP
Na afloop van de installatie moeten we de OpenLDAP nog wel verder configureren. Dat doen we met onderstaande commando:
```bash
$ sudo dpk-reconfigure slapd
```

Je krijgt een aantal vragen te zien:
![slapd01](images/slapd-reconfig-01.png)

![slapd01](images/slapd-reconfig-02.png)

![slapd01](images/slapd-reconfig-03.png)

![slapd01](images/slapd-reconfig-04.png)

![slapd01](images/slapd-reconfig-05.png)

![slapd01](images/slapd-reconfig-06.png)

![slapd01](images/slapd-reconfig-07.png)

## Testen OpenLDAP
Om wat testen te kunnen doen kunnen we zowel bash-commando's gebruiken, óf een GUI. In het eerste geval gebruik je 

```bash
$ sudo apt install ldap-utils
$ ldapsearch -s sub -x -b "" -s base supportedfeatures
# extended LDIF
#
# LDAPv3
# base <> with scope baseObject
# filter: (objectclass=*)
# requesting: supportedfeatures 
#

#
dn:
supportedFeatures: 1.3.6.1.1.14
supportedFeatures: 1.3.6.1.4.1.4203.1.5.1
supportedFeatures: 1.3.6.1.4.1.4203.1.5.2
supportedFeatures: 1.3.6.1.4.1.4203.1.5.3
supportedFeatures: 1.3.6.1.4.1.4203.1.5.4
supportedFeatures: 1.3.6.1.4.1.4203.1.5.5

# search result
search: 2
result: 0 Success

# numResponses: 2
# numEntries: 1

```

Testen door middel van een GUI gaat met de Apache Directory Studio. Je kunt deze op je Windows PC installeren of op je virtuele machine. Als je via je WIndows PC naar de LDAP-service wilt verbinden, zorg dan dat je Port-forwarding aanzet voor poort 389 op je VM-configuratie

[Apache Directory Studio](https://directory.apache.org/studio/downloads.html)

Voor een installatie handleiding klik je [hier](../ApacheLDAPStudio). 

## Opvoeren gebruikers en groepen - OrganizationalUnit
Het is niet gebruikelijk om de in root van je LDAP-tree meteen gebruikers en groepen te maken. Dus maak eerst OrganisationalUnits (OU) . Bijvoorbeeld een OU voor de interne organisatie. 

Ga naar het menu, dan `LDAP` en `New Entry`. Zorg dat de root-node `dc=Samenfit,dc=local` geselecteerd is.  Doorloop de wizard en vul de juiste gegevens in:
![newOU01](images/ldap-newOU-00.png)

![newOU01](images/ldap-newOU-01.png)

Type bij `Available object classes` de tekst `Org` en er verschijnt een verkorte lijst. Kies voor `organizationalUnit` en klik op de `Add` button. 

![newOU02](images/ldap-newOU-02.png)
![newOU02](images/ldap-newOU-03.png)

Het scherm ziet er dan uit zoals hier onder. Klik op `Next` om verder te gaan. We gaan nu de attributen invullen. Elk object heeft een attribuut dat bepaald hoe het in de tree geplaatst moet worden. We noemen dit de [RDN](https://ldap.com/ 


![newOU03](images/ldap-newOU-04.png)

## Opvoeren gebruikers en groepen - usergroups
Om rechten uit te geven is het vaak handig om gebruikers in groepen op te nemen. De volgende stap is dus het aanmaken van een gebruikersgroep.  Hiervoor maken we eerst op een wat slimmere manier een nieuwe OU. 

Selecteer eerst de eerder gemaakte OU en klik met de rechter muisknop op deze OU, en kies uit het lokale menu 'New Entry':
![newOU03](images/ldap-newOU2-00.png)
Merk op dat we nu een template gebruiken van de zojuist geselecteerde OU. Nu kunnen we sneller door de wizard heen. 
![newOU03](images/ldap-newOU2-01.png)
Vergeet niet de RDN aan te passen! Noem deze OU `groups`. 

![newOU03](images/ldap-newOU2-02.png)

![newOU03](images/ldap-newOU2-03.png)

Klik op `Finish` om de wizard af te ronden. 

## Aanmaken groep 
De volgende stap is het maken van een Groep. 
![newGroup03](images/ldap-newGroup-00.png)

![newGroup03](images/ldap-newGroup-01.png)
![newGroup03](images/ldap-newGroup-02.png)

In het laatste scherm laten we het attribuut `uniqueMember` voorlopig leeg.
![newGroup03](images/ldap-newGroup-03.png)

## Maken van een gebruiker
We gaan nu een nieuwe gebruiker aanmaken. We maken een gebruiker door een object te maken van de 
`objectClass = inetOrgPerson`. Belangrijk om te weten is dat we na afloop van het doorlopen van de wizard
nog een aantal extra attributen moeten aanmaken, te weten:
  * `userPassword` : het wachtwoord
  * `uid`: de gebruikersnaam
  
Als RDN gebruiken we het attribuut `CN` (CanonicalName). We krijgen dan de volgende informatie:
   

## Koppelen gebruikers aan groep





# Referenties
  * [LDAP](https://ldap.com]) 
  * [LDAP Password encryptie & Apache WebServer](https://httpd.apache.org/docs/2.4/misc/password_encryptions.html)
