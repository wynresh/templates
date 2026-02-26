# Windows Command

Si Linux est un atelier de menuiserie où tout est visible et modifiable, Windows est une usine high-tech avec des chaînes d'assemblage complexes (Active Directory) et un panneau de contrôle centralisé (le Registre).

## Le Cœur de Windows - Le Registre

Le Registre est une base de données hiérarchique qui contient TOUS les paramètres de Windows : configuration matérielle, logiciels, préférences utilisateurs, politique de sécurité.

C'est l'équivalent de /etc sur Linux, mais en base de données et bien plus critique.

### 1 Structure du Registre

5 ruches principales (hives) :

|Ruche |Abréviation	|Rôle|
|:----:|:----------:|:--:|
|HKEY_CLASSES_ROOT	|HKCR	|Associations de fichiers (quel programme ouvre .txt), objets COM.|
|HKEY_CURRENT_USER	|HKCU	|Configuration de l'utilisateur actuellement connecté (thème, variables d'environnement).|
|HKEY_LOCAL_MACHINE	|HKLM	|Configuration de la machine (matériel, logiciels installés pour tous, drivers).|
|HKEY_USERS	|HKU	|Contient les profils de TOUS les utilisateurs. HKCU est un lien vers l'un d'eux.|
|HKEY_CURRENT_CONFIG |HKCC	|Informations sur le profil matériel actuel (rarement utilisé).|

### 2 Outils pour le Registre

regedit.exe : L'éditeur graphique. À utiliser avec une PRÉCAUTION EXTRÊME. Une mauvaise modification peut rendre Windows non bootable.

reg.exe : L'outil en ligne de commande (pour les scripts).

```cmd
reg query HKLM\Software\Microsoft\Windows\CurrentVersion /v ProgramFilesDir
reg add HKCU\Control Panel\Desktop /v Wallpaper /t REG_SZ /d "C:\image.jpg" /f
```

### 3 Emplacements Clés à Connaître

```cmd
# Démarrage automatique des programmes (classique pour les malwares)
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
HKCU\SOFTWARE\Microsoft\Windows\CurrentVersion\Run

# Configuration réseau
HKLM\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters\Interfaces

# Historique (à nettoyer pour la vie privée)
HKCU\Software\Microsoft\Internet Explorer\TypedURLs
HKCU\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
```

## L'Âme du Réseau Windows - Active Directory (AD)

Active Directory (AD) est l'annuaire centralisé de Microsoft. C'est le cerveau de tout réseau d'entreprise sous Windows. Il utilise LDAP (Lightweight Directory Access Protocol) comme langage et Kerberos pour l'authentification.

Imaginez : Un nouvel employé arrive. L'admin crée un utilisateur DANS L'AD. En 5 minutes, cet employé peut se connecter à n'importe quel ordinateur de l'entreprise avec son mot de passe, et ses dossiers, logiciels, et droits le suivent.

### 1 Concepts Clés

|Concept	|Rôle	|Analogue |
|:---------:|:-----:|:-------:|
|Domaine	|Un groupe d'objets (utilisateurs, ordinateurs) qui partagent une base de données commune.	|Une "entreprise" dans l'annuaire.
|Contrôleur de Domaine (DC)	 |Le serveur qui héberge la base de données AD et authentifie les utilisateurs.	|Le "gardien" et le "bibliothécaire".
|Forêt	|Un ensemble de domaines qui se font confiance.	|Un groupe d'entreprises liées.
|Unité d'Organisation (OU)	|Conteneur pour organiser les objets (par service, par ville).	|Un dossier dans l'annuaire.
|GPO (Group Policy Object)	|RÈGLE LA PLUS PUISSANTE. Ensemble de paramètres appliqués aux utilisateurs/ordinateurs d'une OU.	|Le "contrôle parental" ultime pour tout le parc.

### 2 Outils d'Administration AD

- Utilisateurs et ordinateurs Active Directory (dsa.msc) : Gérer les comptes.
- Sites et services Active Directory (dssite.msc) : Gérer la réplication entre bureaux.
- Gestion des stratégies de groupe (gpmc.msc) : Créer et lier les GPO.

### 3 Commandes PowerShell pour l'AD (Le terminal de l'admin Windows)

```powershell
# Charger le module AD (sur un contrôleur de domaine ou avec RSAT installé)
Import-Module ActiveDirectory

# Créer un utilisateur
New-ADUser -Name "Jean Dupont" -GivenName "Jean" -Surname "Dupont" `
           -SamAccountName "jdupont" -UserPrincipalName "jdupont@monentreprise.local" `
           -Enabled $true -AccountPassword (ConvertTo-SecureString "Mot2P@sse" -AsPlainText -Force)

# Chercher tous les utilisateurs du service "Comptabilité"
Get-ADUser -Filter {Department -eq "Comptabilité"} -Properties Department, Title

# Déplacer un ordinateur dans une OU
Move-ADObject -Identity "CN=PC123,CN=Computers,DC=monentreprise,DC=local" `
              -TargetPath "OU=Paris,OU=Ordinateurs,DC=monentreprise,DC=local"

# Voir les GPO appliquées à un utilisateur
Get-ADUser jdupont -Properties MemberOf, Description
gpresult /r /user jdupont   # Commande classique (non PowerShell)
```

## Les Outils Système Indispensables sur Windows

Un ninja doit connaître ses armes, que ce soit en GUI ou en CLI.

### 1 Outils Graphiques (MMC - Microsoft Management Console)

- compmgmt.msc : Gestion de l'ordinateur (disques, services, utilisateurs locaux).
- devmgmt.msc : Gestionnaire de périphériques (drivers).
- services.msc : Gestion des services (équivalent de systemctl).
- eventvwr.msc : Observateur d'événements (les logs Windows, votre meilleur ami pour le forensic).
- secpol.msc : Stratégie de sécurité locale (si pas de GPO).
- lusrmgr.msc : Utilisateurs et groupes locaux (pour machine non-domaine).

### 2 La Ligne de Commande Windows (CMD et PowerShell)

```cmd
:: CMD - L'ancienne école
ipconfig /all          # Voir la config IP (plus détaillé que ip a)
nslookup google.com    # Interroger le DNS
netstat -an            # Voir les connexions et ports ouverts (ss -tulpn)
tasklist               # Liste des processus (ps aux)
taskkill /F /IM notepad.exe  # Tuer un processus (kill -9)
net user jdupont /domain     # Voir un utilisateur du domaine
net share              # Voir les dossiers partagés
```

```powershell
# PowerShell - Le vrai terminal moderne (objet-oriented)
Get-Process | Where-Object {$_.CPU -gt 10}  # Processus qui utilisent + de 10% CPU
Get-Service | Where-Object {$_.Status -eq "Stopped"}  # Services arrêtés
Get-EventLog -LogName System -Newest 50 | Format-Table -AutoSize  # Les 50 derniers logs système
Test-Connection google.com -Count 2  # Ping amélioré
Get-WmiObject -Class Win32_BIOS  # Infos BIOS (équivalent de dmidecode)
```

