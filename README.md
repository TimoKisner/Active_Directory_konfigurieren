<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>Active Directory - Installation und Konfiguration</h1>
Diese Anleitung dreht sich um die Implementierung einer lokalen Active Directory-Infrastruktur innerhalb virtueller Maschinen in Microsoft Azure.
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Verwendete Technologien und Umgebungen</h2>

- Microsoft Azure (virtuelle Maschinen, virtuelles Netzwerk)
- Remotedesktopverbindungen 
- Active Directory Domain Services
- Active Directory Users and Computers
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
  - Umgebung erstellen (Ressourcengruppe, virtuelles Netzwerk)
  - Virtuelle Maschine "dc-1" erstellen
- Installation Active Directory (AD)
  - Installation "Active Directory Domain Services" (AD DS)
  - Domain-Admin erstellen
- Einrichtung Active Directory
  - Rechner (PCs) der Domain hinzufügen
  - Domain-Benutzerkontos erstellen
  - Zugriff auf Rechner für nicht-administrative Domain-Benutzerkontos
  - Abschließende Worte



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Vorbereitungen</h1>
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Umgebung erstellen</h2>

<p>
Zuerst erstellen wir die Umgebung für unser Projekt. Heißt, wir erstellen zuerst eine Ressourcengruppe und ein virtuelles Netzwerk in der Azure-Cloud. Achte hierbei darauf, das virtuelle Netzwerk der von uns erstellten Ressourcengruppe zuzuordnen. Darüber hinaus sollen beide der sich innerhalb derselben Region befinden (die, die Ihnen am nächsten liegt). Bei mir sieht es wie folgt aus: Mein virtuelles Netzwerk "TestVnet" befindet sich in meiner Ressourcengruppe "RGroup". Beides in der Region "(Europe) Germany West Central".
</p>
<p>
<img src="https://i.imgur.com/r5piKWD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Virtuelle Maschine "dc-1"</h2>

<p>
Im Verlaufe dieser Anleitung werden wir in Azure zwei virtuellen Maschinen erstellen. Die zweite wird aber erst im letzten Kapitel, der Einrichtung von Active Directory, erstellt. Um die erste kümmern wir uns jetzt. In dieser virtuellen Maschine mit dem Namen "dc-1" werden wir Active Directory installieren und verwalten. Das "dc" in "dc-1" steht für "Domain Controller", welcher dc-1 sein wird. Aber was ist ein Domain-Controller? Was ist überhaupt Active Directory? Active Directory (AD) ist ein Verzeichnisdienst von Microsoft, der verwendet wird, um Netzwerke zentral zu verwalten, einschließlich Benutzern, Computern und Ressourcen wie Druckern. Ein Domain-Controller (DC) ist ein Server, der Active Directory hostet und als zentrale Authentifizierungsinstanz für alle Benutzer und Geräte im Netzwerk dient. Mit einem Domain-Controller können Administratoren Benutzerdaten, Berechtigungen und Sicherheitsrichtlinien zentral verwalten. Achte beim Erstellen auf folgendes: die Ressourcengruppe muss unsere vorhin erstellte sein, sowie das virtuelle Netzwerk; die Region muss dieselbe sein; als Image wählen wir "Windows Server 2022 Datacenter"; für die Größe reicht eine Rechenleistung von 2vcpus (ich wähle 4 vcpus); Benutzername und Passwort stehen Ihnen frei; unten bei der Lizenzierung die Häkchen nicht vergessen. Der Rest kann unberührt bleiben.
</p>
<p>
Als Prävention für mögliche Missverständnisse in der Zukunft: mein Benutzername für den Account in meiner virtuellen Maschine "dc-1" lautet "test_user".
</p>
<p>
<img src="https://i.imgur.com/mJEYLsi.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Vor dem Start der Installation von AD setzen wir die private-IP-Adresse von dc-1 von dynamisch auf statisch, damit sie sich nicht ändert und immer dieselbe bleibt. Das Setzen einer statischen privaten IP-Adresse für den Domain-Controller ist notwendig, da er eine zentrale Rolle im Netzwerk spielt und von anderen Geräten über eine feste IP-Adresse erreichbar sein muss. Eine dynamische IP-Adresse könnte sich ändern, was dazu führen würde, dass Geräte den Domain-Controller nicht mehr finden, wodurch Authentifizierungen und Netzwerkdienste gestört werden, bis hin zu nicht mehr möglich sind. Folge den kommenden Bildern um dich durch die Einstellungen zu navigieren. Die vorgeschlagene IP gleicht, der zuvor benutzen IP, also belassen wir es dabei und drücken auf "Speichern" um die Änderung zu bestätigen. Nun müssten Sie in der Zeile mit dem blau markierten Text "ipconfig1" neben der IP-Adresse "(Statisch)" sehen.
</p>
<p>
<img src="https://i.imgur.com/Ximghle.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/xucoVJ6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Starten Sie zur Absicherung die virtuelle Maschine neu um die Änderung effektiv zu machen.
</p>
<p>
<img src="https://i.imgur.com/R1zBMaW.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Installation Active Directory</h1>
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Installation Active Directory Domain Servives</h2>

<p>
Fortfahren tun wir innerhalb der virtuellen Maschine. Benutzen Sie Remotedesktopverbindungen, um sich in dc-1 einzuloggen und, falls es nicht schon automatisch geschieht, öffnen Sie "Server Manager". Klicke auf "Add roles and features". Hier haben Sie eine Auflistung, worauf sie bei jedem der folgenden Einrichtungsfenster achten sollen:
</p>
<p>
"Add roles and Features" :: auf "next"; "Installation Type" :: "Role-based or feature-based installation" dann "next"; "Server Selection" :: wähle dc-1 dann "next"; "Server Roles" :: das Häckchen für "Active Directory Domain Services" klicken, auf "Add Features" drücken und "next"; "Features" :: auf "next" ; "AD DS" :: auf "next"; "Confirmation" :: das Häkchen oben setzen und auf "Install". 
</p>
<p>
<img src="https://i.imgur.com/9v7xLxj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/qtV1s22.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DmEAzio.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Sobald die Installation abgeschlossen ist, drücken wir auf "close". Jetzt machen wir den Rechner, dc-1, zu einem tatsächlichen DC, Domain Controller. Hierzu müssen wir erneut in den Server Manager. Oben rechts befindet sich eine Fahne. Diese anklicken und auf "Promote this server to a domain controller" drücken. Anschließend öffnet sich ein Fenster zur Einrichtung der Domain, über die der Controller verwalten soll. Wir erschaffen eine komplett neue. Dafür fügen wir einen neuen "forest" hinzu. Ein Forest in Active Directory ist die oberste organisatorische Ebene, die alle Domänen und ihre Ressourcen (Benutzer, Rechner, etc.) unter sich zusammenfasst. Es stellt eine gemeinsame Sicherheits- und Verwaltungsstruktur für diese Domänen bereit. Unser Domain-Controller erstellt den Forest und die Domain. Ihr Forest kann heißen wie Sie wünschen. Ich nenne meinen "uga.buga". Dieser "Root domain name" ist nichts anderes als der Name der Domain, die innerhalb unseres Forests erstellt wird, und dient als Grundlage für die gesamte Active Directory-Umgebung. Er definiert die primäre Identität des Forests und legt den Namensraum fest, unter dem alle weiteren Domänen und Ressourcen organisiert werden. Anschließend müssen Sie ein Passwort eingeben zur Wiederherstellung der Domain (diesen werden wir nicht brauchen). Hinterher auf "next" drücken bis wir zum "Prerequisites Check"-Fenster kommen. Nachdem der Rechner erfolgreich geprüft wurde, auf "Install" klicken. Im Anschluss der Installation wird Ihre Verbindung mit dem Rechner getrennt, weil dieser sich neu startet, um die installierten Änderungen effektiv zu machen.
</p>
<p>
<img src="https://i.imgur.com/9qqRPJG.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/V7C6Ojp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<p>
Von nun an, wenn wir uns einloggen wollen in virtuellen Maschinen (sowohl unser gerade erstellter Domain-Controller als auch zukünftige Rechner, die wir der Domain hinzugefügt haben), verwenden wir den Kontext der Domain beim Einloggen. Anstatt in Remotedesktopverbindungen den einfachen Benutzernamen des Accounts, mit dem wir uns einloggen wollen, einzugeben, geben wir ihn im folgenden Format ein: "[domain]\Benutzername". In meinem Beispiel heißt meine Domain uge.buga und der Benutzername lautet test_user, also gebe ich "uga.buga\test_user" ein. Das Passwort ist das gleiche wie zuvor.
</p>
<p>
<img src="https://i.imgur.com/pRaXgY1.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Domain-Admin</h2>

<p>
Der nächste Schritt bezieht sich auf das Erstellen von Instanzen innerhalb unserer Domain. Genauer werden wir zunächst einen Benutzer mit Administrator-Berechtigungen über die Domain erstellen, kurz einen Domain-Admin. Öffnen tun wir eine Anwendung namens "Active Directory Users and Computers". Hier können wir genannte Instanzen erstellen. Zur besseren Übersicht erstellen wir eine "Organizational Unit" namens "_ADMINS". Eine "Organizational Unit" (OU) bezeichnet, für unsere Zwecke, nichts anderes als einen Ordner mit bestimmten Attributen. Der Name kann sein was auch immer Ihr Herz begehrt, da wir aber in diesem Ordner vorhaben all unsere Admin-Benutzer zu verwalten, nenne ich ihn dementsprechend "_ADMINS" (das "_" dient zur Sortierung: durch alphabetischer Anordnung wird der Ordner als erstes angezeigt). Rechtsklicken Sie auf ihre Domain, dann auf "New" und dann auf "Organizational Units".
</p>
<p>
<img src="https://i.imgur.com/LztRXEj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/Eovz7yl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/gg74Qrx.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Während wir schon dabei sind, erstellen wir zwei weitere OUs. Nämlich "_CLIENTS" und "_EMPLOYEES". Beide benutzen wir später im Verlauf der Einrichtung. Achte bei der OU "_EMPLOYEES", dass es genau so geschrieben ist, da wir später mit einem Script arbeiten, um uns mehrere zufällig generierte Benutzer zu erstellen (oder ändere das Script, dass es auf den Namen deiner OU zutrifft). Fürs erste spielen diese zwei OUs aber keine Rolle. 
</p>
<p>
<img src="https://i.imgur.com/1FMxHMj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zurück zur Organizational Unit "_ADMINS". Innerhalb dieser erstellen wir einen "User". Klicke auf "_ADMINS", dann Rechtsklicke die Ansicht rechts und drücke "New", dann "User". Alle relevanten Informationen ausfüllen, den logon-Namen sich merken und auf "Next" drücken. Diesen verwenden wir zum Einloggen in den Account. Es ist der Benutzername des Benutzer-Accounts, den wir eingeben in Remotedesktopverbindung. Dasselbe gilt für das Passwort, welches Sie im Anschluss eingeben. !Achtung: lesen Sie sich die Checkboxen durch beim Eingeben des Passwortes und setzen/entfernen sie Häkchen nach Ihrem Belieben. Da dies lediglich eine Anleitung ist und ich meine virtuelle Maschine am Ende lösche, habe ich folgende Häkchen gesetzt (s. Bild).
</p>
<p>
Mein logon-Name/Benutzername dieses Admin Accounts lautet "admin_barack". 
</p>
<p>
<img src="https://i.imgur.com/BGJtWNf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/tBipgfr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/FSoTmmr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zuletzt müssen wir "admin_barack" auch wirklich zum Admin machen, denn nur weil er sich in der von uns erstellten "_ADMINS" OU befindet, macht ihn das nicht automatisch zu einem Admin. Um das zu realisieren, müssen wir ihn der Sicherheitsgruppe der Domain-Admins hinzufügen. Öffne "_ADMINS", Rechtsklicke auf Barack und drücke auf "Properties". Navigiere zu "Member Of", drücke "Add" und schreibe "Domain Admins" in die Box. Sicherheitshalber drücken Sie auf "Check Names" und erst dann auf "OK" (folge den Pfeilen auf dem Bild).
</p>
<p>
<img src="https://i.imgur.com/2quI4fE.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/iHkv2ol.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Abschließend bestätigen wir, dass Barack in den Rängen der Domain Admins angenommen wurde, klicken auf "Apply" und dann auf "OK". Nun besitzt Barack die Berechtigungen eines Admins innerhalb der Domain uga.buga. Logge dich neu ein als "[domain-name]\[admin_user]". Von nun an loggen wir uns in dc-1 nur noch mit unserem Adminkonto ein.
</p>
<p>
<img src="https://i.imgur.com/lKR1NCp.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h1>Einrichtung Active Directory</h1>
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Rechner zur Domain hinzufügen</h2>

<p>
Was benötigt man um einen Rechner, gedacht für Benutzer, einer Domain hinzuzufügen? Richtig, einen Rechner! Wir erschaffen uns eine weitere virtuelle Maschine in Azure, die, bezogen auf die Einstellungen (zugeordnete Ressourcengruppe, Virtuelles Netzwerk, etc.), gleichgesetzt ist mit dc-1. So befinden sich diese in derselben Umgebung. Der einzige Unterschied ist folgender: an der Stelle von Windows Server 2022 benutzen wir Windows 10 Pro als Image. Als Namen für die virtuelle Maschine suggeriere ich "client-1". Falls Sie sich noch erinnern, haben wir eine Organizational Unit namens "_CLIENTS" angelegt, mit der Intention darin unsere Rechner innerhalb der Domain zu verwalten. Der Benutzername und das Passwort des Kontos steht Ihnen frei. Meiner lautet "original_user".
</p>
<p>
<img src="https://i.imgur.com/Tvzod6n.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/oSf01tD.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Eine andere Sache, die wir zuvor getan haben, war es die private-IP-Adresse von dc-1 auf statisch zu setzen, sodass diese sich nicht ändert. Warum wir das getan haben, habe ich bereits erläutert. Jetzt ändern wir die DNS-Einstellungen von unserer gerade erstellten Maschine "client-1" und lassen diese zum Domain-Controller, dc-1, zeigen. Indem wir die DNS-Einstellungen von client-1 auf die private IP-Adresse von dc-1 ändern, haben wir den Domain-Controller als primären DNS-Server festgelegt. Dies ist notwendig, da der Domain Controller mit Active Directory auch DNS-Dienste bereitstellt, die für die Namensauflösung und das Finden von Domainressourcen essenziell sind. Ohne diese Änderung könnte client-1 die Domain und den Domain-Controller nicht finden, was eine Anmeldung oder Integration in die Domain unmöglich machen würde. Dafür navigieren wir zur selben Stelle in Azure, wo wir auch die IP-Adresse von dc-1 auf statisch gesetzt haben. Diesmal klicken wir auf "DNS-Server", auf "Benutzerdefiniert", geben als DNS-Server die private-IP-Adresse von dc-1 ein und "Speichern".
<p>
<img src="https://i.imgur.com/huSDx7I.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Abschließend starten wir die VM neu und bestätigen die Änderung der DNS-Einstellungen. Das Neustarten der Maschine wird in Azure erledigt. Zum Bestätigen des DNS-Servers loggen wir uns in client-1 ein und öffnen PowerShell. Hier angekommen geben wir "ipconfig /all" ein und suchen nach "DNS-Server". Wenn rechts daneben die private-IP von dc-1 zu finden ist, dann läuft alles nach Plan.
<p>
<img src="https://i.imgur.com/Ub5Lusb.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/DoDettQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Endlich kommen wir zum Thema! Um diesen Rechner jetzt zu unserer Domain hinzuzufügen, öffnen wir die Systemeinstellungen (Rechtsklick auf Windowssymbol unten links und auf "System" drücken). Als Nächstes auf "Rename this PC (advanced)", auf "Change..." und dann bei "Member of" die Option "Domain" anwählen und ihren Domain-Namen eingeben (s. Bild). Der Rechner fragt, als Reaktion nach einem Benutzer mit der Berechtigung diese Aktion auszuführen. Wir geben die Daten vom lieben Barack an (Ihrem Domain-Admin). Der Rechner fordert uns an ihn neu zu starten, damit die Änderungen in Effekt treten. Diesem Wunsch gehen wir nach.
<p>
<img src="https://i.imgur.com/wNeyzNV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zusammenfassend bestätigen wir noch die Aufnahme von client-1 in unsere Domain. Öffne Active Directory Users and Computers erneut und schaue unter dem Ordner "Computers", ob du client-1 siehst. Ziehe client-1 in "_CLIENTS".
</p>
<p>
<img src="https://i.imgur.com/XESNonR.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Et voilà! Wir haben einen Rechner erfolgreich unserer Domain hinzugefügt. Zeit, ein paar Benutzer zu erstellen.
</p>
<br />
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Domain-Benutzerkontos</h2>

<p>
Tatsächlich haben wir diesen Schritt schon getan. Nämlich als wir unser Adminaccount erstellt haben. Hingegen des Adminaccounts erstellen wir unsere normalen, nicht-adminstrativen Benutzerkontos in der "_EMPLOYEES" Organizational Unit. Angesichts der Verwendung von Active Directory in der echtel Welt, sind diese Art von Benutzerkontos oft die der Mitarbeiter des Unternehmens, welche Besitz über die Domain hat. Dementsprechend ändern wir auch nichts an den Eigenschaten ("Properties") der Benutzerkonten innerhalb dieser Organizational Unit. Mein Beispiel eines nicht-admistrativen Benutzeraccounts taufe ich "hilli_billi". 
</p>
<p>
<img src="https://i.imgur.com/hv7U8aF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/MKMRJfw.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Gegenwärtig haben wir uns eine Testumgebung gebaut. Unser jetziges Zwischenprodukt ist nicht ausgelegt auf eine Verwendung in der realen Welt, sondern dient lediglich dem Erlangen des Grundverständnis und die Möglichkit für Experiemente in Bezug auf Active Directory als Verzeichnisdienst. Demnach wäre es nützlich mehrere Benutzerkonten für Mitarbeiter anzulegen, aber einen nach dem anderen hinzuzufügen ist mühselig und zeitintensiv. Aus diesem Grund lassen wir ein script[LINK EMBEDDED, machen] in Powershell ISE laufen. Wichtig, öffne Powershell ISE in dc-1 als Adminstrator (rechtklick auf Powershell ISE und drücke auf "Run as adminstrator"). Der Unterschied zu Powershell? PowerShell ist eine Befehlszeilenumgebung und Skriptsprache, mit der Befehle direkt ausgeführt und Aufgaben automatisiert werden können. PowerShell ISE (Integrated Scripting Environment) hingegen ist eine grafische Benutzeroberfläche, die speziell für das Schreiben, Bearbeiten und Testen von PowerShell-Skripten entwickelt wurde, um komplexere Aufgaben einfacher umzusetzen. Erstelle ein neues Fenster für das schreiben von Programmen und Scripten, indem du oben links auf das leere, weiße Blatt mit gelben Sternchen klickst.
</p>
<p>
<img src="https://i.imgur.com/W2wobF9.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/Tfj3m2M.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>

Kopiere das Script und füge es ein. Bevor du auf das script ausführst empfehle ich dir bei Bedarf folgende Variablen am script zu ändern: Anzahl der zu generierenden Benutzer (der Standardwert beträgt 10.000!!); das Password der Benutzer (wird für alle gleich sein) und, falls du deine Organizational Unit für Mitarbeiter nicht "_EMPLOYEES" genannt hast, den Weg der Erstellung.
</p>
<p>
<img src="https://i.imgur.com/TWBmtCA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/EC3AGNj.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Zum Ausführen des Scripts drücken wir oben den grünen Play-Button. Ich habe die Anzahl der Benutzerkonten auf 100 gesetzt und das Password beim Satndard-Password belassen. Im Anschluss überprüfen wir in Active Directory Users and Computers, ob die Benutzerkonten tatsächlich angelegt wurden. Schließe Powershell ISE und rücke vor zur nächsten Station.
</p>
<p>
<img src="https://i.imgur.com/Mwaw5cV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br/>
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Zugriff für nicht-adminstrative Domain-Benutzerkontos</h2>

<p>
Momentan haben wir mehrere Benutzerkonten für Mitarbeiter, ein Adminkonto für den Systemadminstrator und einen Rechner für Mitarbeiter. Aber beim Versuch uns mit einem zufällig gewählten Mitarbeiteraccount in client-1 anzumelden, scheitert es. Das liegt daran, dass wir den Zugang zu client-1 für nicht-adminstrative Benutzerkontos noch nicht genehmigt haben. Versucht man es mit einem zufälligen Mitarbeiteraccount ("fiko.fakuh"), erscheint folgendes Error-Fenster (s. Bild). Der Domain Controller bestätigt, dass ein Benutzerkonto namens "fiko.fakuh" existiert, Client-1 aber das genannte Vorhaben nicht genehmigt.
<p>
<img src="https://i.imgur.com/HeAtYGV.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<p>
<img src="https://i.imgur.com/1JX8Qtc.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<p>
Um unser Vorhaben möglich zu machen loggen wir uns zunächst mit unserem Adminaccount Barack in client-1 ein. In client-1 angekommen öffnen wir die Systemsteuerungen (rechtklick unten links auf das Windowssymbol und wähle "System" aus). Anschließend navigieren wir zu "Remote Desktop", klicken auf "Select users that can remotely access this PC", drücken auf "Add" und fügen unsere Benutzer hinzu. Anstatt einen einzelnen Benutzer einzugeben und diesen Prozess für jeden einzelnen Account zu wiederholen, schreiben wir "Domain Users" in die Box. Beim Erstellen von Usern in Active Directory Users and Computers werden diese automatisch als Mitglieder von "Domain Users" zugeordnet. Demzufolge sind all unsere Mitarbeiteraccounts Mitglied und alle erhalten Zugriff auf client-1.
</p>
<p>
<img src="https://i.imgur.com/X5v2q0G.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
<!-- XXX -->
<!-- XXX -->
<!-- XXX -->
<h2>Abschließende Worte</h2>

<p>
Im Rahmen dieses Projekts haben wir eine grundlegende Active Directory-Umgebung eingerichtet, bestehend aus zwei virtuellen Maschinen: einem Domain Controller und einem Client, der erfolgreich in die Domain integriert wurde. Neben der manuellen Erstellung eines Administratorkontos haben wir mithilfe eines PowerShell-Skripts zahlreiche Benutzerkonten effizient der Domain hinzugefügt. Diese grundlegende Einrichtung bildet die Basis für viele weitere Konfigurationsmöglichkeiten, wie etwa das Erstellen von Gruppenrichtlinien zur zentralen Verwaltung von Sicherheits- und Benutzerrechten, das Implementieren von weiteren Organisational Units (OU) zur Strukturierung von Ressourcen oder die Integration weiterer Dienste wie DHCP oder Zertifikatsdienste. Solche Erweiterungen ermöglichen eine optimierte Verwaltung und Skalierung der Umgebung, um sie individuell an die Anforderungen eines Netzwerks und dem Vorhaben anzupassen.
</p>
<p>
Viel Spaß beim weiteren Einrichten ihrer Active Directory-Umgebung!
</p>
