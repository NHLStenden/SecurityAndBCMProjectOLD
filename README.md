# SecurityAndRiskProject

In dit project gaan we aan de slag met Linux (bijv. Ubuntu), Apache en LDAP. Het hoofdproduct van dit project is een website die we gaan beveiligen op meerdere manieren:
  * Operating Systeem
  * File Systeem
  * Database
  * Authenticatie / Autorisatie

We gebruiken hiervoor de volgende producten:
  * Debian 
  * OpenLDAP
  * Apache 2 webserver
  
VOor het beheren van de LDAP-entiteiten gebruiken we Apache Directory Studio. 

De verschillende handleidingen voor installatie en configuratie zijn te vinden in onderstaande locaties. Deze kunnen
het beste in onderstaande volgorde doorlopen worden:
  1. [Installatiehandleiding Debian]( ./DebianInstall/README.md )
  1. [Ophalen van bestanden](./GIT/README.md)
  1. [Installatie OpenLDAP](./OpenLDAP/README.md)
  1. [Installatie Apache Directory Studio](./ApacheLDAPStudio/README.md)
  1. [Configuratie gebruikers en groepen in LDAP](./ConfigLDAP/README.md)
  1. [Installatie Apache Webserver](./ApacheWebServer/README.md)
  1. [Installatie PHP](./php/README.md)
  1. [Netwerk instellen](./Netwerk/README.md)
  
Uiteindelijk kun je onderstaande landschap inrichten:

![Landschap](images/VirtueleMachine.png)  

---

# Achtergrond en onderbouwing
Bij het bouwen van een software systeem zijn we vaak primair gericht op het ontwikkelen van functionaliteit. Er is vaak
minder aandacht voor het platform waarmee of waarop we die functionaliteit uiteindelijk aanbieden. Als je wilt dat jouw
software in een bedrijfsomgeving lang zijn werk kan doen is er ook aandacht nodig voor niet functionele eisen, waaronder
security, als ook componenten die je typisch aan kunt treffen in een zakelijke omgeving.

In deze module gaan we daarom ook aandacht schenken aan deze aspecten: je zorgt er voor dat niet alleen je eigen software
maar ook het platform waar op je werkt veilig is. Daarnaast leer je gebruik te maken van een component voor de 
administratie van gebruikersgegevens. 

Nu kun je je afvragen waarom we voor deze opzet hebben gekozen. Waarom niet gewoon die account opnemen in je eigen database?
Gewoon het wachtwoord hashen, beetje *salt* er bij en dan is het toch ook goed? En dan nog 'even' de rollen administratie
er bij en dan werkt het toch ook? Waarom al die moeite om dat LDAP aan de praat te krijgen?

Kijk eens naar onderstaande landschap. Hierin wordt een ICT-landschap beschreven van een fictief bedrijf. 

![Afbeelding landschap](images/Rol%20van%20LDAP-Mijn%20Applicatie.png)

Het draait in deze tekening natuurlijk om jouw applicatie die weergegeven is in het grote oranje vlak ("Mijn applicatie").
Hierin zit je filesysteem waarop de website draait die gehost wordt via Apache webserver. 

Wat je links onderin ziet is dat er in dit fictieve bedrijf gebruik wordt gemaakt van een personeelssysteem (HRM) waar in
gegevens van werknemers worden opgeslagen. Daarnaast is er een LDAP-oplossing die gebruikt wordt om de accounts in op 
te slaan. Naast deze accounts worden ook rollen van de gebruikers hier opgeslagen.

Het idee is nu dat meerdere applicaties gebruik kunnen maken van deze LDAP-gegevens om zo hun applicatie te beveiligen
(authenticatie + autorisatie). Dit heeft meerdere voordelen, waaronder dat gebruikers niet per applicatie weer een nieuwe
account hoeven op te onthouden. 

Daarnaast wordt gebruik gemaakt van een software oplossing (IAM Tooling). Dergelijke tooling wordt ingezet om de informatie
die in het HRM-systeem zit om te zetten in Identity & Access Management informatie zoals het beheren van accounts en autorisaties.
Je kunt dan denken aan het geautomatiseerd (en dus gestandaardiseerd) verzorgen van:
  * het aanmaken van accounts als je in dienst komt
  * het intrekken van een account als je uit dienst gaat
  * het toekennen van autorisaties op basis van je rol of functie (het zogenaamde *Role based access control*, RBAC)
  * het intrekken van autorisaties als je rol of functie veranderd
  * het kunnen verzorgen van rapportages voor compliance/attestation
  * het verzorgen van in-sync houden van bijvoorbeeld rollen in een Identity Provider (LDAP/Active Directory)

Je zou dan onderstaande afbeelding kunnen krijgen waarbij een aantal zaken vereenvoudigd zijn ten opzichte van de vorige
afbeelding.

![Afbeelding uitgebreid landschap](images/Rol%20van%20LDAP-Meer%20apps.png)

In deze afbeelding zien we dat meer applicaties gebruik maken van de LDAP en eventueel ook leunen op de mogelijkheden
van de IAM-tooling om bijvoorbeeld rollen te synchroniseren. Het voordeel van de LDAP-oplossing is nu duidelijker: meerdere
applicaties maken gebruik van deze oplossing zodat ze zelf geen account- en wachtwoord beheer hoeven te doen. De rollen
die door de IAM-tooling worden bijgehouden worden door elke applicatie zelf vertaald naar permissies. Daarmee geven ze 
invulling aan de vraag

> "Wat betekent het voor deze gebruiker in **mijn applicatie** als ze rol X of Y hebben voor het wel of niet kunnen uitvoeren
van een bepaalde functie, handeling of proces?" 

Als de rol van de *medewerker* verandert doordat hij bijvoorbeeld een andere functie krijgt, dan zal deze door de IAM-tooling
doorvertaald worden naar technische rollen in het *account* van de LDAP omgeving. Doordat jouw applicatie die rollen 
opvraagt (na authenticatie) bij de LDAP-oplossing is meteen duidelijk dat bepaalde permissies wel of niet meer van 
toepassing zijn.

Door de combinatie van LDAP en je eigen administratie krijg je dan bijvoorbeeld onderstaande proces:

![Proces](images/Rol%20van%20LDAP-Bepalen%20permissies.png)

## Conclusies
Hoewel het configeren en gebruiken van LDAP voor deze onderwijs situatie dus lastig is en complexiteit toe lijkt te voegen,
is het voor bedrijven erg nuttig om dergelijke functionaliteit in te zetten voor meerdere applicaties. Dit heeft als 
bijkomend voordeel dat de kroonjuwelen (accounts/wachtwoorden) maar op één plek beveiligd hoeven te worden. Voor de 
medewerkers is het voordeel dat ze meerdere applicaties kunnen gebruiken met één account/wachtwoord combinatie. Mogelijk
ontstaat zelfs een Single Sign-On situatie waardoor medewerkers automatisch veilig ingelogd worden. 

---

Martin Molema

mail: [martin.molema@nhlstenden.com](mailto:martin.molema@nhlstenden.com)

Laatste update op 10-maart-2021
  
