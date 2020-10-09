# Installatie van Debian binnen Virtual Box (v6)

Download een image bestand van Debian Net Installation :
[Debian Website](https://www.debian.org/CD/netinst/). De huidige versie ten tijde van dit schrijven is *10.6.0*. Hoewel
er steeds nieuwe versies uitgegeven worden is de procedure hetzelfde. 

Kies het juiste image voor jouw architectuur (vermoedelijk AMD64).

Als deze klaar is met downloaden kun je deze gebruiken om in (bijv.) VirtualBox een configuratie
aan te maken. 

## Aanmaken nieuwe VM configuratie in VirtualBox
Start VirtualBox en start het maken van een nieuwe configuratie voor een Virtuele Machine met Debian:

![VirtualBox-001](./images/VirtualBox-10.png)

Klik op de button 'Nieuw' en de wizard start. 


Kies eerst een locatie (map) waar de machine opgeslagen gaat worden. Een locatie op een snelle disk heeft de voorkeur.
Kies vervolgens het type en Versie: Linux, Debian 64 bit. 

![VirtualBox-001](./images/VirtualBox-01.png)

### Geheugen

De nieuwe VM heeft geheugen nodig om te kunnen werken. De keuze voor de hoeveelheid geheugen is erg afhankelijk van 
de hoeveelheid van de hoeveelheid geheugen in jouw laptop/PC, maar ook van wat je VM gaat doen. Als je VM
een Desktop-omgeving (Grafische User Interface) zoals XFCE4, GNome of Mate gaat gebruiken dan heb je meer 
geheugen nodig. Een paar handreikingen:
 * Headless: 1GB of 2GB is meer dan voldoende; 1 vCPU
 * Eenvoudige GUI (XFCE4, Mate): 2GB ; let ook op je instellingen voor videogeheugen; 1vCPU
 * Complexe GUI (Unbuntu 18 default desktop): 4GB minimaal  ;2 vCPU

![VirtualBox-001](./images/VirtualBox-02a.png)

### Harde schijf harde schijf    

In de volgende stap maak je keuzes voor het type harde schijf dat je wilt gebruiken. Dit is een virtuele
schijf die wordt gesimuleerd door één bestand op je de echte harde schijf van je laptop/PC. De omvang
is de maximale omvang, en is later eenvoudig aan te passen mocht dat nodig zijn. We kiezen hier
voor 12GB wat meer dan voldoende moet zijn. 

![VirtualBox-001](./images/VirtualBox-02.png)

De volgende keuze gaat over het type schijf. Kies hier voor VDI. 

![VirtualBox-001](./images/VirtualBox-03.png)

Het is verstandig om je schijf dynamisch op te zetten. Het bestand op de harddisk van de laptop/PC 
wordt vanzelf groter als je VM meer ruimte nodig heeft. De VM denkt echter de hele tijd dat de harddisk
gewoon 12GB is. Het voordeel hiervan is dat je makkelijker backups kunt maken of makkelijker je VM kunt
delen met anderen: deze is misschien in werkelijkheid maar 2GB in omvang op je laptop/PC.

![VirtualBox-001](./images/VirtualBox-04.png)


![VirtualBox-001](./images/VirtualBox-05.png)

### Verdere instellingen
Het kan nuttig zijn om wat standaard systeeminstellingen even na te lopen. Bijvoorbeeld de hoeveelheid
virtuele CPU's (vCPU) die je wilt toekennen, de hoeveelheid video geheugen en eventuele video versnelling
en aanvullende opstart instellingen.

In het eerste scherm kun je instellen hoeveel geheugen je wilt toekennen, mocht dat aanpassing nodig hebben. 
 
![VirtualBox-001](./images/VirtualBox-21.png)

Op het tweede tabblad (bovenin) kun je instellen hoeveel CPU's (cores) je wilt afstaan aan je VM.

![VirtualBox-001](./images/VirtualBox-22.png)

Bij het tabblad "Video" kun je instellen hoeveel geheugen je voor video wilt toewijzen en of je 
2D/3D versnelling wilt gebruiken. 
![VirtualBox-001](./images/VirtualBox-20.png)

### Opstartinstellingen
Nu we alles hebben ingesteld qua resources voor de VM, moeten we zorgen dat de machine kan opstarten. De eerste
keer moeten we opstarten van een ander device dan de nieuwe virtuele harddisk: we hebben immers nog niks
geinstalleerd op onze virtuele harddisk. 
We doen dit door de gedownloade ISO als CD te koppelen. Dit wordt in onderstaande schermen uitgelegd.

![VirtualBox-001](./images/VirtualBox-06.png)

![VirtualBox-001](./images/VirtualBox-07.png)

![VirtualBox-001](./images/VirtualBox-08.png)

![VirtualBox-001](./images/VirtualBox-08a.png)

![VirtualBox-001](./images/VirtualBox-09.png)

### Eerste opstart.
Als de configuratie klaar is, kun je de installatie starten. Klik in het hoofdscherm van VirtualBox
op de groene 'Start'-knop om de machine te starten.  

![VirtualBox-001](./images/VirtualBox-10.png)

Na het opstarten verschijnt kort het startscherm van Virtual Box. 


Als de ISO/CD goed start krijg je een keuze of je de setup van Debian via een grafische interface wilt. Dit verdient
vaak wel de voorkeur zodat je met de muis en toetsenbord kunt werken.

![DebianInstall-001](./images/debian-install-001.png)

Je bent hier vrij in het kiezen van een taal. In dit geval kies ik voor Engels.

![DebianInstall-002](./images/debian-install-002.png)

Daarna stel je in een aantal stappen de regio in waar je je bevindt. Kies voor Europae/Netherlands.
 
![DebianInstall-003](./images/debian-install-003.png)

![DebianInstall-004](./images/debian-install-004.png)

![DebianInstall-005](./images/debian-install-005.png)

Vervolgens kies je instellingen voor bijvoorbeeld je toetsenbordindeling.
![DebianInstall-006](./images/debian-install-006.png)

![DebianInstall-007](./images/debian-install-007.png)

Vervolgens begint deel 1 van de installatie.
![DebianInstall-008](./images/debian-install-008.png)

De volgende stap is het detecteren van hardware zoals de klok en je netwerkkaart.
![DebianInstall-009](./images/debian-install-009.png)

Het netwerk wordt automatisch ingesteld op DHCP: het verkrijgen van een automatisch adres.
![DebianInstall-010](./images/debian-install-010.png)

Nu moet je de naam van de virtuele machine instellen. Dit is onder andere de naam die bijv. in het
netwerk gebruikt kan worden.
![DebianInstall-011](./images/debian-install-011.png)

Het domein van je netwerk mag je leeglaten.
![DebianInstall-013](./images/debian-install-013.png)

Vervolgens moet je gebruikers aanmaken. Stap 1 is het opvoeren van een wachtwoord voor de 
`root` gebruiker. Tip: schrijf dit wachtwoord op! Als je dit kwijtraakt kun je niks meer met je VM beginnen.
![DebianInstall-001](./images/debian-install-014.png)

Voer daarna een naam in voor de tweede gebruiker. Je mag een volledige naam invoeren. Ik gebruik
hier alleen mijn voornaam.
![DebianInstall-001](./images/debian-install-015.png)

Van de volledige naam wordt vervolgens een gebruikersnaam afgeleid die je hier nog kunt veranderen.
![DebianInstall-001](./images/debian-install-016.png)

Voer vervolgens voor deze gebruiker ook een wachtwoord in. 
![DebianInstall-001](./images/debian-install-017.png)

De installatie gaat verder met deel 2: het configureren van de schijf. Deze wordt het beste
ingesteld op één partitie voor alle onderdelen.
![DebianInstall-001](./images/debian-install-018.png)

Let op: het gaat hier om een virtuele schijf! Dus je maakt geen wijzigingen op je echte harddisk van 
je computer. De 'schijf' die de installatieprocedure hier ziet is in wezen één groot bestand op je
harddisk. Virtual Box zorgt er voor dat deze door Debian als een gewone schijf wordt gezien.

![DebianInstall-001](./images/debian-install-019.png)

![DebianInstall-001](./images/debian-install-020.png)

![DebianInstall-001](./images/debian-install-021.png)

![DebianInstall-001](./images/debian-install-022.png)

Kies hier voor 'YES' om de wijzigingen door te voeren en verder te gaan. 

![DebianInstall-001](./images/debian-install-023.png)

![DebianInstall-001](./images/debian-install-024.png)

![DebianInstall-001](./images/debian-install-025.png)

![DebianInstall-001](./images/debian-install-026.png)

De packagemanager wil nu aanvullende pakketten ophalen en vraagt je om een locatie te kiezen
die dichtbij is, zodat het downloaden sneller gaat.
![DebianInstall-001](./images/debian-install-027.png)

Je kunt ook kiezen voor bijvoorbeeld de Universiteit van Twente (debian.snt.utwente.nl).
![DebianInstall-001](./images/debian-install-028.png)

Mocht je achter een proxy zitten dan kun je deze hier configureren.
![DebianInstall-001](./images/debian-install-029.png)

Nu wordt dan daadwerkelijk de rest van het OS geinstalleerd.
![DebianInstall-001](./images/debian-install-030.png)

Maak hier je eigen keuze.
![DebianInstall-001](./images/debian-install-031.png)

De installatie start...
![DebianInstall-001](./images/debian-install-032.png)

Na de basisinstallatie kunnen veel gebruikte pakketten geselecteerd worden. Voor een headless server gebruik
je geen desktop omgeving, dus zorg dat het bovenste pakket en de pakketten die met `...` beginnen
niet geselecteerd worden.

Zorg dat SSH *wel* aan staat! Dan kun je je server straks via SSH in ieder geval ook benaderen. 
![DebianInstall-001](./images/debian-install-033.png)

![DebianInstall-001](./images/debian-install-034.png)

Na het installatieproces moet de boot-loader geinstalleerd worden. Deze zorgt er voor dat Virtual Box
je OS daadwerkelijk kan vinden en opstarten.
![DebianInstall-001](./images/debian-install-035.png)

Installeer op de enige schijf. 
![DebianInstall-001](./images/debian-install-036.png)

![DebianInstall-001](./images/debian-install-037.png)

De installatie is klaar en we kunnen opnieuw opstarten.

LET OP! Als je de configuratie van de 'CD' met het installatie bestand op een andere manier hebt gekoppeld
dan kan het zijn dat je installatie proces opnieuw begint. In dat geval ga je in de configuratie
van je VM in Virtual box eerst je CD ontkoppelen voordat je opnieuw je VM opstart.
![DebianInstall-001](./images/debian-install-038.png)

![DebianInstall-001](./images/debian-install-039.png)

Nu wordt je VM daadwerkelijk voor de eerste keer opgestart.

![DebianInstall-001](./images/debian-install-040.png)

Eerst krijg je het schermv an GRUB (de bootloader) te zien. Je kunt hier op <enter> drukken om door te gaan.  
![DebianInstall-001](./images/debian-install-041.png)

Daarna start het boot-proces van de VM daadwerkelijk.
![DebianInstall-001](./images/debian-install-043.png)

Na verloop van tijd kun je inloggen. Login met `root` en je zelfgekozen wachtwoord.
![DebianInstall-001](./images/debian-install-044.png)

Na het inloggen krijg je wat informatie over de gekozen distributie / versie.
![DebianInstall-001](./images/debian-startup-004.png)
