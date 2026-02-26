# Linux Command

## Niveau 1

```bash
# Navigation et fichiers
pwd                          # Où suis-je ?
ls -la                       # Liste TOUT (même les cachés) avec détails
cd /etc                      # Se déplacer
cp -r source/ destination/   # Copier récursivement
mv ancien nouveau            # Déplacer/Renommer
rm -rf dossier/              # SUPPRIMER SANS CONFIRMATION (dangereux !)
mkdir -p chemin/vers/dossier # Créer des dossiers (même l'arborescence)

# Visualisation
cat fichier                  # Afficher le contenu (pour petits fichiers)
less fichier                 # Lire page par page (q pour quitter)
head -n 20 fichier           # Voir les 20 premières lignes
tail -f /var/log/syslog      # Suivre les logs en temps réel

# Recherche
grep "erreur" /var/log/*     # Chercher "erreur" dans tous les logs
find / -name "*.conf"        # Trouver tous les fichiers .conf depuis la racine
```

## Niveau 2

```bash
# Le pipe : chaîner les commandes
ls -la /etc | grep "conf"    # Lister /etc et ne garder que les lignes contenant "conf"
ps aux | grep "apache"       # Trouver le processus Apache
dmesg | less                 # Lire les messages du noyau tranquillement

# Redirections
echo "texte" > fichier.txt   # Écraser fichier.txt avec "texte"
echo "autre" >> fichier.txt  # Ajouter "autre" à la fin du fichier
ls /dossier-inexistant 2> erreur.log  # Rediriger les erreurs (stderr) vers un fichier
```

## Niveau 3

```bash
# Gestion des utilisateurs
whoami                       # Qui suis-je ?
id                           # Mon UID, GID, groupes
useradd -m -G sudo newuser   # Créer un user avec home (-m) et dans groupe sudo
passwd newuser               # Changer son mot de passe
su - newuser                 # Switch user (se connecter en tant que)
sudo commande                # Exécuter en tant que root (si autorisé)

# Les permissions
ls -l fichier
# -rw-r--r-- 1 user group 1024 Jan 1 00:00 fichier
# ^^^^???
# 1er char: - (fichier), d (dossier), l (lien)
# Ensuite: r (read), w (write), x (execute)
# 3 groupes: [user (owner)] [group] [others]
# rw- (user peut lire/écrire), r-- (group peut lire), r-- (others peut lire)

chmod 755 script.sh          # Change mode: 7=rwx (user), 5=r-x (group), 5=r-x (others)
chown user:group fichier     # Change owner:group
```

## Niveau 4

```bash
# Processus
ps aux                        # Liste tous les processus
top                           # Moniteur interactif (htop est mieux, mais à installer)
kill -9 PID                   # Forcer la mort d'un processus (SIGKILL)
kill -15 PID                  # Demander poliment de s'arrêter (SIGTERM)
jobs                          # Voir les tâches en arrière-plan
fg %1                         # Ramener la tâche 1 au premier plan

# Services (systemd - le standard moderne)
systemctl status ssh          # Voir l'état du service SSH
systemctl start ssh           # Démarrer
systemctl enable ssh          # Activer au démarrage
journalctl -u ssh -f          # Voir les logs du service SSH en temps réel

# Réseau (Les bases)
ip a                          # Voir les interfaces et adresses IP (remplace ifconfig)
ip r                          # Voir la table de routage (remplace route)
ss -tulpn                     # Voir les ports ouverts et les processus (remplace netstat)
ping google.com               # Tester la connectivité
traceroute google.com         # Voir le chemin des paquets
```

