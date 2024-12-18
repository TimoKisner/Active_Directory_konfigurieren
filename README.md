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
<h1>Vorbereitungen</h1>
<!-- XXX -->
<h2>Umgebung erstellen</h2>

<p>
Zuerst erstellen wir die Umgebung für unser Projekt. Heißt, wir erstellen zuerst eine Ressourcengruppe und ein Virtuelles Netzwerk in der Azure-Cloud. Achte Hierbei darauf, das virtuelle Netzwerk der von uns erstellten Ressourcengruppe zuzuordnen. Darüber hinaus sollen beide der sich innerhalb der selben Region befinden (die, die Ihnen am nächsten liegt). Für mich sieht es wie folgt aus: mein virtuelles Netzwerk "TestVnet" befindet sich in meiner Ressourcengruppe "RGroup". Beides in der Region "(Europe) Germany West Central".
</p>
<p>
<img src="1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<!-- XXX -->
<h2>Virtuelle Maschine "dc-1"</h2>

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
Vor dem Start der Installation von AD, wichtig die IP auf statisch zu setzen, damit sie sich nicht ändert und immer die selbe bleibt. Das machen wir weil: habsdhvbabdjsabfcvabsjvbasvbSV...........(). Folge den kommenden Bildern um dich durch die Einstellungen zu navigieren. Die vorgeschlagene IP gleicht der zuvor benutzen IP, also belassen wir es dabei und drücken auf "Speichern" um die Änderung zu Bestätigen. Nun müssten Sie in der Zeile mit dem blau markierten Text "ipconfig1" neben der IP-Addresse "(Statisch)" sehen.
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
<h1>Installation Active Directory</h1>
<!-- XXX -->
<h2>Istallation Active Directory Domain Servives</h2>

<p>
remote desktop into dc-1. öffne server manager (sollte sich automatisch öffnen bei log in). klick auf "Add roles and Features" :: auf "next"; "Installation Type" : "Role-based or feature-based installation" dann "next"; "Server Selection" wähle dc-1 dann "next"; "Server Roles" das häckchen für "active directory domain services" klicken, auf "Add Features" drücken und "next"; "Features" auf "next" ; "AD DS" auf "next"; "Confirmation" das häckchen oben setzen und "Install". 
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
<!-- XXX -->
<h2>Domain-Admin</h2>

<p>
Der nächste Schritt bezieht sich auf das Erstellen von Instanzen innerhalb unserer Domain. Genauer werden wir zunächst einen Benutzer mit Adminstartor-Berechtigungen über die Domain erstellen, kurz einen Domain-Admin. Öffnen tun wir eine Anwendung namens "Active Directory Users and Computers". Hier können wir genannte Instanzen erstellen. Zur besseren Übersicht erstellen wir eine "Organizational Unit" namens "_ADMINS". Eine "Organizational Unit" (OU) bezeichnet, für unsere Zwecke, nichts anderes als einen Ordner mit bestimmten Attributen. Der Name kann sein was auch immer Ihr Herz begehrt, da wir aber in diesem Ordner vor haben all unsere Admin-Benutzer zu verwalten, nenne ich ihn dementsprechend "_ADMINS" (das "_" dient zur Sortierung: ist wegen alphabetischer Anordnung der Ordner als erstes angezeigt). Rechtklicken Sie auf ihre Domain, dann auf "New" und dann auf "Organizational Units". 
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
Während wir schon dabei sind, erstellen wir zwei weitere OUs. Nämlich "_CLIENTS" und "_EMPLOYEES". Beide benutzen wir später im Verlauf der Einrichtung. Achte bei der OU "_EMPLOYEES" es genau so zu schreiben, da wir später mit einem script arbeiten, um uns mehrere zufällig generierte Benutzer zu erstellen (oder ändere das script, dass es auf den Namen deiner OU zutrifft). Fürs erste spielen diese zwei OUs aber keine Rolle. 
</p>
<p>
<img src="4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zurück zur Organizational Unit "_ADMINS". Innerhalb dieser erstellen wir einen "User". KLicke auf "_ADMINS", dann rechtklicke die Ansicht rechts und drücke "New", dann "User". Alle relevanten Informationen ausfüllen, den logon-Namen sich merken und auf "Next" drücken. Diesen verwenden wir zum einloggen in den Account. Es ist der Benutzername des Benutzer-Accounts, den wir eingeben in Remotedesktopverbindung. Das selbe gilt für das Passwort, welches Sie im Anschluss eingeben. !Achtung: lese dir die Checkboxen durch beim eingeben des Passwortes und setzen/entfernen sie Häckchen nach Ihrem Belieben. Da dies lediglich eine Anleitung ist und ich meine virtuelle Maschine lösche, habe ich folgende Häckchen gesetzt (s. Bild).
</p>
<p>
Mein logon-Name/Benutzername dieses Admin Accounts lautet "admin_barack". 
</p>
<p>
<img src="5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="6" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zuletzt müssen wir "admin_barack" auch wirklich zum Admin machen, denn nur weil er sich in der von uns erstellten "_ADMINS" OU befindet, macht ihn das nicht automatisch zu einem Admin. Um das zu realisieren müssen wir ihn der Sicherheitsgruppe der Domain-Admins hinzufügen. Öffne "_ADMINS", rechtklicke auf Barack und drücke auf "Properties". Navigiere zu "Member Of", drücke "Add" und schreibe "Domain Admins" in die Box. Sicherheitshalber drücken Sie auf "Check Names" und erst dann auf "OK" (folge den Pfeilen auf dem Bild).
</p>
<p>
<img src="8" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="9" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Abschließend bestätigen wir, dass Barack in den Rängen der Domain Admins angenommen wird, klicken auf "Apply" und dann auf "OK". Nun besitzt Barack die Berechtigungen eines Admins innerhalb der Domain uga.buga. Logge dich neu ein als "[domain-name]\[admin_user]". Von nun an loggen wir uns in dc-1 nur noch mit unserem Adminkonto ein.
</p>
<p>
<img src="10" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Einrichtung Active Directory</h1>
<!-- XXX -->
<h2>Rechner zur Domain hinzufügen</h2>

<p>
Was benötigt man um einen Rechner, gedacht für Benutzer, einer Domain hinuzufügen? Richtig, einen Rechner! Wir erschaffen uns eine weitere Virtuelle Maschine in Azure, die, bezogen auf die Einstellungen (zugeordnete Ressourcengruppe, Virtuelles Netzwerk, etc.), gleichgesetzt ist mit dc-1. So befinden sie sich in der selben Umgebung. Der einzige Unterschied ist folgender: an der Stelle von Windows Server 2022 benutzen wir Windows 10 Pro als Image. Als Namen für die Virtuelle Maschine suggeriere ich "client-1". Falls Sie sich noch erinnern, haben wir eine Organizational Unit namens "_CLIENTS" angelegt, mit der Intention darin unsere Rechner innerhalb der Domain zu verwalten. Der Benutzername und das Passwort des Kontos steht Ihnen frei. Meiner lautet "original_user".
</p>
<p>
<img src="1" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Eine andere Sache, die wir zuvor getan haben, war es die private-IP-Addresse von dc-1 auf statisch zu setzen, sodass diese sich nicht ändert. Warum wir das getan haben, habe ich bereits erläutert. Jetzt ändern wir die DNS-Einstellungen von unserer gerade erstellten Maschine "client-1" und lassen diese zum Domain Controller, dc-1, zeigen. Dastun wir(/müssen wir??). weil: jdbvchavsbhfvb...............(). Dafür navigieren wir zur selben Stelle in Azure wo wir auch die IP-Adresse von dc-1 auf statisch gesetzt haben. Diesmal klicken wir auf "DNS-Server", auf "Benutzerdefiniert", geben als DNS-Server die private-IP-Adresse von dc-1 ein und "Speichern".
</p>
<p>
<img src="3" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Abchließend starten wir die VM neu und betsätigen die Änderung des DNS-Einstellungen. Das Neustarten der Maschine wird in Azure erledigt. Zum Bestätigen des DNS-Servers loggen wir uns in client-1 ein und öffnen Powershell. Hier angekommen geben wir "ipconfig /all" ein und suchen nach "". Wenn rechts daneben die private-IP von dc-1 zu finden ist, dann ist alles nach Plan verlaufen.
<p>
<img src="4" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="5" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Endlich kommen wir zum Thema! Um diesen Rechner jetzt zu unserer Domain hinzuzufügen, öffnen wir die Systemseinstellungen (rechtsklick auf Windowssymbol unten links und auf "System" drücken). Als nächstes auf "Rename this PC (advanced)", auf "Change..." und dann als "Member of" "Domain" anwählen und ihren Domainn-Namen eingeben (s. Bild). Die Rechner fragt als Reaktion nach einem Benutzer mit der Berechtigung diese Aktion auszuführen. Wir geben die Daten vom lieben Barack an (Ihrem Domain-Admin). Der Rechner fordert uns an ihn neu zu starten, damit die Änderungen in Effekt treten. Diesem Wunsch gehen wir nach.
<p>
<img src="6" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Zusammenfassend bestätigen wir noch die Aufnahme von client-1 in unsere Domain. Öffne Active Directory Users and Computers erneut und schaue unter dem Ordner Computers", ob du client-1 siehst. Ziehe client-1 in "_CLIENTS".
</p>
<p>
<img src="7" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Et Voila! Wir haben einen Rechner erfolgreich unserer Domain hinzugefügt. Zeit, ein paar Benutzer unserer Domain hinzuzufügen.
</p>
<br />
<!-- XXX -->
<h2>Domain-Benutzerkontos</h2>

<p>
TEXT hghghghghghg
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<!-- XXX -->
<h2>Zugriff für nicht-adminstrative Domain-Benutzerkontos</h2>

<p>
TEXT hghghghghghg
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
