1.1 Exercice : Connexion SSH root (reprise fin TP-01)
Objectif : Configurer SSH pour permettre les connexions root distantes avec mot de passe.

Étapes à suivre :

apt install openssh-server
Modification de la configuration SSH :

Ouvrez le fichier de configuration SSH :

nano /etc/ssh/sshd_config
Recherchez et modifiez les lignes suivantes :

PermitRootLogin yes

systemctl restart ssh

ssh root@ip_serveur -p 2222

1.2 Exercice : Authentification par clé / Génération de clés
Objectif : Générer une paire de clés SSH pour une authentification sécurisée sans mot de passe.

Étapes à suivre :

Génération d’une paire de clés SSH :

Sur votre machine hôte, ouvrez un terminal et exécutez :



ssh-keygen -t rsa -b 4096 -C "votre_email@example.com"
La clé privée est sauvegardée par défaut dans ~/.ssh/id_rsa.
La clé publique est sauvegardée dans ~/.ssh/id_rsa.pub.
Si vous utilisez votre machine personnelle :

Outils utilisés : ssh-keygen est l’outil standard pour générer des clés SSH. Les clés sont généralement gérées via le répertoire ~/.ssh/.
1.3 Exercice : Authentification par clé / Connexion serveur
Objectif : Déposer votre clé publique sur le serveur pour permettre une authentification par clé.

Étapes à suivre :

Connexion au serveur en tant que root via SSH :


ssh root@ip_serveur
Remplacez ip_serveur par l’adresse IP de votre serveur.
Création du dossier .ssh si nécessaire :


mkdir -p /root/.ssh/authorized_keys

chmod 600 root/.ssh/authorized_keys



Copiez le contenu affiché.
Sur le serveur, ouvrez le fichier authorized_keys :

nano /root/.ssh/authorized_keys

Collez la clé publique copiée.
Sauvegardez et fermez l’éditeur.
Définissez les permissions correctes :

Redémarrage du service SSH (si nécessaire) :


systemctl restart ssh
Remarque : Assurez-vous que le dossier .ssh et le fichier authorized_keys ont les bonnes permissions pour éviter les problèmes de sécurité.

1.4 Exercice : Authentification par clé depuis la machine hôte
Objectif : Se connecter au serveur en utilisant la clé privée sans mot de passe.

Étapes à suivre :

Connexion au serveur en utilisant la clé privée :


ssh -i ~/.ssh/id_rsa root@ip_serveur
Remplacez ~/.ssh/id_rsa par le chemin de votre clé privée si différent.
Remplacez ip_serveur par l’adresse IP de votre serveur.
Vérification :

Si tout est correctement configuré, vous devriez vous connecter sans avoir à entrer de mot de passe.



1.5 Exercice : Sécurisez
Objectif : Sécuriser l’accès SSH pour root en utilisant uniquement des clés, afin d’éviter les tentatives d’authentification par brute force.

Étapes à suivre :

Modifier la configuration SSH pour désactiver l’authentification par mot de passe :

Ouvrez le fichier de configuration SSH :


nano /etc/ssh/sshd_config
Modifier les lignes suivantes :
 


PermitRootLogin prohibit-password


PasswordAuthentication no


PermitRootLogin prohibit-password permet les connexions root uniquement via des clés SSH.

PasswordAuthentication no désactive l’authentification par mot de passe pour tous les utilisateurs.


Redémarrer le service SSH pour appliquer les modifications :


systemctl restart ssh
Procédure pour sécuriser l’accès SSH :

Utilisation des clés SSH uniquement : Assurez-vous que tous les utilisateurs autorisés ont leurs clés publiques déposées sur le serveur.
Désactivation de l’authentification par mot de passe : Réduit les risques de force brute.
Utilisation d’un pare-feu : Limitez l’accès SSH aux adresses IP de confiance.
Changer le port SSH par défaut : Réduit les tentatives automatiques sur le port standard (22).
Explication des attaques de type brute-force SSH :

Description : Les attaques par brute-force SSH consistent à essayer de multiples combinaisons de noms d’utilisateur et de mots de passe pour accéder au serveur.
Risques : Si des mots de passe faibles sont utilisés, un attaquant peut réussir à se connecter et compromettre le système.
Techniques supplémentaires pour se protéger :

Utiliser Fail2ban :

Installation :
bash
Copier le code
apt install fail2ban
Activation :
bash
Copier le code
systemctl enable fail2ban
systemctl start fail2ban
Fonctionnement : Bloque automatiquement les adresses IP après un certain nombre d’échecs de connexion.
Changer le port SSH par défaut :

Modification :
plaintext
Copier le code
Port 2222
Ajoutez ou modifiez cette ligne dans /etc/ssh/sshd_config.
Redémarrage SSH :
bash
Copier le code
systemctl restart ssh
Connexion via le nouveau port :
bash
Copier le code
ssh -p 2222 root@ip_serveur
Limiter les utilisateurs pouvant se connecter via SSH :

Ajouter dans /etc/ssh/sshd_config :
plaintext
Copier le code
AllowUsers user1 user2
Redémarrer SSH :
bash
Copier le code
systemctl restart ssh
Utiliser des clés SSH avec une passphrase :

Avantage : Ajoute une couche de sécurité supplémentaire en chiffrant la clé privée.
Inconvénient : Nécessite de saisir la passphrase lors de chaque connexion, sauf si vous utilisez un agent SSH (ssh-agent).
2. Étude des processus UNIX
2.1 Exercice : Étude des processus UNIX
Objectif : Afficher et analyser les processus en cours sur la machine.

Étapes à suivre :

Partie 1 : Afficher les processus avec ps
Afficher la liste de tous les processus :

bash
Copier le code
ps aux
Description des colonnes :
USER : Nom de l’utilisateur propriétaire du processus.
PID : Numéro d’identification du processus.
%CPU : Pourcentage d’utilisation du processeur.
%MEM : Pourcentage d’utilisation de la mémoire.
STAT : État du processus.
START : Date de début du processus.
TIME : Temps CPU utilisé.
COMMAND : Commande utilisée pour lancer le processus.
Questions :

À quoi correspond l’information TIME ?

Réponse : TIME indique le temps total de CPU utilisé par le processus depuis son lancement.
Quel est le processus ayant le plus utilisé le processeur sur votre machine ?

Réponse : Regardez la colonne %CPU et identifiez le processus avec la valeur la plus élevée. Par exemple, si vous voyez un processus firefox avec %CPU de 30%, c’est probablement le plus gourmand.
Quel a été le premier processus lancé après le démarrage du système ?

Réponse : Utilisez la commande suivante pour trier les processus par date de début :
bash
Copier le code
ps -e --sort=start_time | head -n 2
Le premier processus est généralement init ou systemd.
À quelle heure votre machine a-t-elle démarré ? Trouvez une autre commande pour le temps depuis lequel le serveur tourne :

Commande pour l'heure de démarrage :
bash
Copier le code
who -b
Autre commande pour le temps de fonctionnement :
bash
Copier le code
uptime
ou
bash
Copier le code
uptime -s
Nombre approximatif de processus créés depuis le démarrage :

Réponse : Il est difficile de déterminer le nombre exact de processus créés depuis le démarrage. Cependant, pour obtenir le nombre actuel de processus :
bash
Copier le code
ps -e | wc -l
Partie 2 : Relations de parenté des processus
Option de ps pour afficher le PPID :

bash
Copier le code
ps -eo pid,ppid,cmd
Explication : Affiche le PID, le PPID (Parent PID) et la commande associée à chaque processus.
Liste ordonnée de tous les processus ancêtres de la commande ps en cours d’exécution :

bash
Copier le code
pstree -s $$
Explication : pstree -s $$ affiche l’arborescence des processus ancêtres du shell courant.
Partie 3 : Utilisation de pstree
Installer pstree si nécessaire :

bash
Copier le code
apt update
apt install psmisc
pstree fait partie du paquet psmisc.
Afficher l’arborescence des processus :

bash
Copier le code
pstree -p
Explication : Affiche tous les processus sous forme d’arborescence avec les PID.
Afficher les ancêtres d’un processus spécifique (par exemple, ps) :

bash
Copier le code
pstree -s $(pgrep ps)
Partie 4 : Utilisation de top
Lancer top :

bash
Copier le code
top
Afficher les processus triés par utilisation mémoire décroissante :

Dans top, appuyez sur la touche M (majuscule) pour trier par %MEM.
Questions :

Quel est le processus le plus gourmand sur votre machine ? À quoi correspond-il ?

Réponse : Le processus en haut de la liste après avoir trié par mémoire est le plus gourmand. Par exemple, firefox peut être un processus gourmand en mémoire.
Commandes interactives dans top :

Passer l’affichage en couleur : Appuyez sur z.
Mettre en avant la colonne de tri : Appuyez sur f puis utilisez les flèches pour sélectionner la colonne souhaitée.
Changer la colonne de tri : Appuyez sur O (lettre O majuscule) puis sélectionnez la colonne.
Utilisation de htop :

Installation de htop :

bash
Copier le code
apt install htop
Lancer htop :

bash
Copier le code
htop
Avantages et inconvénients de htop par rapport à top :

Avantages :
Interface plus intuitive et colorée.
Navigation facile avec les touches fléchées.
Possibilité de tuer des processus directement depuis l’interface.
Inconvénients :
Peut consommer légèrement plus de ressources.
Moins universellement présent sur tous les systèmes par défaut.
2. Exercice 2 : Arrêt d’un processus
Objectif : Apprendre à gérer et arrêter des processus via des scripts et des commandes.

Étapes à suivre :

Créer deux scripts shell :

Script date.sh :

bash
Copier le code
#!/bin/sh
while true; do
    sleep 1
    echo -n 'date '
    date +%T
done
Script date-toto.sh :

bash
Copier le code
#!/bin/sh
while true; do
    sleep 1
    echo -n 'toto '
    date --date '5 hour ago' +%T
done
Rendre les scripts exécutables :

bash
Copier le code
chmod +x date.sh
chmod +x date-toto.sh
Lancer le premier script et le mettre en arrière-plan :

bash
Copier le code
./date.sh
Pendant l’exécution, appuyez sur CTRL+Z pour suspendre le processus.
Mettre en arrière-plan :
bash
Copier le code
bg
Lancer le deuxième script et le mettre en arrière-plan :

bash
Copier le code
./date-toto.sh
Pendant l’exécution, appuyez sur CTRL+Z pour suspendre le processus.
Mettre en arrière-plan :
bash
Copier le code
bg
Arrêter les scripts en utilisant jobs, fg et CTRL-C :

Liste des jobs :

bash
Copier le code
jobs
Ramener un job au premier plan et l’arrêter :

bash
Copier le code
fg %1
Puis appuyez sur CTRL-C pour arrêter.
bash
Copier le code
fg %2
Puis appuyez sur CTRL-C pour arrêter.
Arrêter les scripts en utilisant ps et kill :

Trouver les PID des scripts :

bash
Copier le code
ps aux | grep date.sh
ps aux | grep date-toto.sh
Utiliser kill pour arrêter les processus :

bash
Copier le code
kill <PID_date.sh>
kill <PID_date-toto.sh>
Si les processus ne s’arrêtent pas, utilisez :
bash
Copier le code
kill -9 <PID_date.sh>
kill -9 <PID_date-toto.sh>
Explication des scripts :

date.sh : Ce script exécute une boucle infinie qui, toutes les secondes, affiche le mot "date" suivi de l’heure actuelle au format HH:MM
.
date-toto.sh : Ce script exécute une boucle infinie qui, toutes les secondes, affiche le mot "toto" suivi de l’heure actuelle moins 5 heures au format HH:MM
.
3. Exercice 3 : Les tubes
Objectif : Comprendre l’utilisation des pipes (|), tee et cat.

Questions et explications :

Quelle est la différence entre tee et cat ?

cat : Affiche le contenu des fichiers sur la sortie standard (terminal).
tee : Lit de l’entrée standard et écrit simultanément sur la sortie standard et dans un ou plusieurs fichiers.
Que font les commandes suivantes :

$ ls | cat :

Explication : Liste les fichiers et dossiers dans le répertoire courant (ls) et passe la liste à cat, qui l’affiche sur le terminal. Cela n’a pas d’effet visible différent de ls seul.
$ ls -l | cat > liste :

Explication : Liste détaillée des fichiers (ls -l), passe cette liste à cat, qui redirige la sortie vers le fichier liste, écrasant son contenu précédent.
$ ls -l | tee liste :

Explication : Liste détaillée des fichiers (ls -l), passe cette liste à tee, qui l’écrit dans le fichier liste tout en l’affichant sur le terminal.
$ ls -l | tee liste | wc -l :

Explication : Liste détaillée des fichiers (ls -l), passe cette liste à tee qui l’écrit dans le fichier liste et la passe ensuite à wc -l qui compte le nombre de lignes, affichant ainsi le nombre total d’éléments listés.
4. Journal système rsyslog
Objectif : Comprendre le fonctionnement du service rsyslog et ses configurations.

Étapes à suivre :

Vérifier si le service rsyslog est lancé et obtenir son PID :

bash
Copier le code
systemctl status rsyslog
Si actif, vous verrez une ligne indiquant "active (running)".

Obtenir le PID :

bash
Copier le code
pidof rsyslogd
Cela affichera le PID du démon rsyslog.
Fichier de configuration principal et fichiers de destination des messages :

Fichier principal : /etc/rsyslog.conf

Fichiers de destination :

Messages issus des services standards : Généralement /var/log/syslog ou /var/log/messages.
Autres messages : Peuvent être dirigés vers des fichiers spécifiques comme /var/log/auth.log, /var/log/kern.log, etc.
Vérification du contenu :

bash
Copier le code
cat /var/log/syslog
cat /var/log/messages
À quoi sert le service cron ?

Réponse : cron est un service qui permet de planifier l’exécution automatique de tâches (scripts, commandes) à des intervalles réguliers (minute, heure, jour, semaine, mois).
Que fait la commande tail -f ?

Description : tail -f affiche les dernières lignes d’un fichier et continue de suivre les nouvelles lignes ajoutées en temps réel.

Commande demandée :

bash
Copier le code
tail -f /var/log/messages
Cela affichera en temps réel les nouveaux messages ajoutés au fichier /var/log/messages.
Que voyez-vous si vous redémarrez le service cron depuis un autre shell ?

Vous verrez des messages dans le terminal où tail -f est exécuté, indiquant le redémarrage du service cron, comme des messages de systemd ou des logs spécifiques à cron.
À quoi sert le fichier /etc/logrotate.conf ?

Réponse : Le fichier /etc/logrotate.conf configure la rotation des fichiers de logs, c’est-à-dire la gestion automatique de la taille des fichiers de logs en les archivant, les compressant et les supprimant pour éviter qu’ils ne deviennent trop volumineux.
Examiner la sortie de la commande dmesg :

Commande :

bash
Copier le code
dmesg
Questions :

Quel modèle de processeur Linux détecte-t-il sur votre machine ?
Recherchez les lignes contenant "CPU" ou "processor".
bash
Copier le code
dmesg | grep -i cpu
Quels modèles de cartes réseaux détecte-t-il ?
Recherchez les lignes contenant "Ethernet" ou le nom du fabricant de la carte réseau.
bash
Copier le code
dmesg | grep -i ethernet
dmesg | grep -i network
Exemple :

bash
Copier le code
dmesg | grep -i cpu
dmesg | grep -i ethernet

