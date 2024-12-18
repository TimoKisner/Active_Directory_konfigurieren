<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory - Installation und Konfiguration</h1>
Diese Anleitung dreht sich um die Implementierung einer lokalen Active Directory-Infrastruktur innerhalb Virtueller Maschinen in Microsoft Azure.
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Verwendete Technologien und Umgebungen</h2>

- Microsoft Azure (Virtuelle Maschinen, Virtuelles Netzwerk)
- Remotedesktopverbindungen 
- Active Directory Domain Services
- PowerShell



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Verwendete Betriebssysteme</h2>

- Windows Server 2022
- Windows 10 Pro (22H2)



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>High-Level Übersicht der Schritte</h2>

- Vorbereitungen
  - Umgebung erstellen (Ressourcengruppe, Virtuelles Netzwerk)
  - Virtuelle Maschine "dc-1" erstellen
- Installation Active Directory (AD)
  - Installation "Active Directory Domain Services" (AD DS)
  - Domain-Admin erstellen
- Einrichtung Active Directory
  - Rechner (PCs) zur Domain hinzufügen
  - Domain-Benutzerkontos erstellen
  - Zugriff auf Rechner für nicht-adminstrative Domain-Benutzerkontos



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Installation und Konfiguration von Active Directory</h1>



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Vorbereitungen</h2>

<p>
Zuerst erstellen wir die Umgebung für unser Projekt. Heißt, wir erstellen zuerst eine Ressourcengruppe und ein Virtuelles Netzwerk in der Azure-Cloud. Achte Hierbei darauf, das virtuelle Netzwerk der von uns erstellten Ressourcengruppe zuzuordnen. Darüber hinaus sollen beide der sich innerhalb der selben Region befinden (die, die Ihnen am nächsten liegt). Für mich sieht es wie folgt aus: mein virtuelles Netzwerk "TestVnet" befindet sich in meiner Ressourcengruppe "RGroup". Beides in der Region "(Europe) Germany West Central".
</p>
<p>
<img src="1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Im Verlaufe dieser Anleitung werden wir in Azure zwei virtuellen Maschinen erstellen. Die zweite wird aber erst im letzten Kapitel, der Einrichtung von Active Directory, erstellt. Um die erste kümmern wir uns jetzt. In dieser virtuellen Maschine mit dem Namen "dc-1" werden wir Active Directory installieren und verwalten. Das "dc" in "dc-1" steht für "Domain Controller", welcher dc-1 sein wird. Das ist ad und ein dc: jabsdfuasbuvbauh...............(). Achte beim Erstellen auf folgendes: die Ressourcengruppe muss unsere vorhin erstellte sein, sowie das virtuelle Netzwerk; die Region soll die muss die selbe sein; als Image wählen wir "Windows Server 2022 Datacenter"; für die Größe reicht eine Rechenleistung von 2vcpus (ich wähle 4 vcpus); Benutzername und Passwort stehen Ihnen frei; unten bei der Lizenzierung die Häckchen nicht vergessen. Der Rest kann unberührt bleiben.
</p>
<p>
Als Prävention für mögliche Missverständnisse in der Zukunft: mein Benutzername für den Account in meiner Virtuellen Maschine "dc-1" lautet "test_user".
</p>
<p>
<img src="2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Vor dem Start der Installation von AD, wichtig die IP auf statisch zu setzen, damit sie sich nicht ändert und immer die selbe bleibt. Das machen wir weil: client-1 kontext kjdanvfabv....(). Und client-1 DNS == ip dc-1 machen wir weil: habsdhvbabdjsabfcvabsjvbasvbSV...........(). Folge den kommenden Bildern um dich durch die Einstellungen zu navigieren. Die vorgeschlagene IP gleicht der zuvor benutzen IP, also belassen wir es dabei und drücken auf "Speichern" um die Änderung zu Bestätigen. Nun müssten Sie in der Zeile mit dem blau markierten Text "ipconfig1" neben der IP-Addresse "(Statisch)" sehen.
</p>
<p>
<img src="3" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Starten Sie zur Absicherung die Virtuelle Maschine neu um die Änderung effektiv zu machen.
</p>
<p>
<img src="5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Installation Active Directory</h2>
<!-- Install AD -->
<p>
remote desktop into dc-1. öffne server manager (sollte sich automatisch öffnen bei log in). klick auf "Add roles and Features" :: auf "next"; "Installation Type" : "Role-based or feature-based installation" dann "next"; "Server Selection" wähle dc-1 dann "next"; "Server Roles" das häckchen für "active directory domain services" klicken, auf "Add Features" drücken und "next"; "Features" auf "next" ; "AD DS" auf "next"; "Confirmation" das häckchen oben setzen und "Install".             {{{promote to DC                     {{{relog into dc-1 with domain context
</p>
<p>
<img src="1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="3" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Sobald die Installation abgeschlossen ist, drücken wir auf "close". Jetzt machen den Rechner, dc-1, zu einem tatsächlichen DC, Domain Controller. Hierzu müssen wir erneut in den Server Manager. Oben rechts befindet sich eine Fahne. Diese anklicken und auf "Promote this server to a domain controller" drücken. Anschließend öffnet sich ein Fenster zur Einrichtung der Domäne über die der Controller verwalten soll. Wir erschaffen eine komplett neue. Dafür fügen wir einen neuen "forest" hinzu. DAS ist ein forest: djfbajusbvjaufsd.........(). Er kann heißen wie Sie wünschen. Ich nenne meinen "uga.buga". Dieser "Root Domain name" ist nichts anderes als: jdabjvabfsdvba.......(). Anschließend müssen Sie ein ein Passwort eingeben zur Wiederherstellung der Domain (diesen werden wir nicht brauchen). Anschließend auf "next" drücken bis wir zum "Prerequisites Check"-Fenster kommen. Nachdem der Rechner erfolgreich geprüft wurde auf "Install" klicken. Im Anschluss der Installation wird Ihre Verbindung mit dem Rechner getrennt, weil dieser sich neu startet um die installierten Änderungen effektiv zu machen.
</p>
<p>
<img src="4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Von nun an, wenn wir uns einloggen wollen in unsere Rechner (sowohl unser gerade erstellter Domain Controller als auch zukünftige Rechner, die wir der Domain hinzufügen), verwenden wir den Kontext der Domain beim einloggen. Anstatt in Remotedesktopverbindungen den einfachen Benutzernamen des Accounts mit dem wir uns einloggen wollen einzugeben, geben wir ihn im folgendem Format ein: "[domain]\Benutzername". In meinem Beispiel heißt meine Domain uge.buga und der Benutzername lautet test_user, also gebe ich "uga.buga\test_user" ein. Das Passwort ist das gleiche wie zuvor.
</p>
<p>
<img src="6" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<hr>

<!-- Create Admin_User -->
<p>
TEXT ghghghghghg
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Einrichtung Active Directory</h2>

<p>
TEXT
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
