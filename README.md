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
  - 1
  - 2
- Installation Active Directory (AD)
  - 1
  - 2
- Konfiguration AD
  - 1
  - 2



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
Im Verlaufe dieser Anleitung werden wir in Azure zwei virtuellen Maschinen erstellen. Die zweite wird aber erst im letzten Kapitel "?(Konfig oder Einr?)" erstellt. Um die erste kümmern wir uns jetzt. In dieser virtuellen Maschine mit dem Namen "dc-1" werden wir Active Directory installieren und verwalten. Das "dc" in "dc-1" steht für "Domain Controller", welcher dc-1 sein wird. Das ist ad und ein dc: jabsdfuasbuvbauh...............(). Achte beim Erstellen auf folgendes: die Ressourcengruppe muss unsere vorhin erstellte sein, sowie das virtuelle Netzwerk; die Region soll die muss die selbe sein; als Image wählen wir "Windows Server 2022 Datacenter"; für die Größe reicht eine Rechenleistung von 2vcpus (ich wähle 4 vcpus); Benutzername und Passwort stehen Ihnen frei; unten bei der Lizenzierung die Häckchen nicht vergessen. Der Rest kann unberührt bleiben.
</p>
<p>
<img src="2" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Installation Active Directory</h2>
<!-- Install AD -->
<p>
TEXT
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />

<hr>
<!-- Create Admin_User -->
<p>
TEXT
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />



<!-- NEW SECTION -->
<!-- NEW SECTION -->
<!-- NEW SECTION -->
<h2>Konfiguration Active Directory</h2>

<p>
TEXT
</p>
<p>
<img src="" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
<br />
