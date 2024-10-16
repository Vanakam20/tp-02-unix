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
**systemctl restart ssh**

is ont se connect avec la clef publique :
C:\Users\Auguste-Alain\.ssh>ssh root@ip_serveur -p 2222 -i maclef.pub
**@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for 'id_rsa.pub' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "id_rsa.pub": bad permissions**

il vaut se connecter avec la clef privé: **ssh root@ip_server -p 2222 -i clefpriver**

Procédure pour sécuriser l’accès SSH :

Utilisation des clés SSH uniquement : Assurez-vous que tous les utilisateurs autorisés ont leurs clés publiques déposées sur le serveur.
Désactivation de l’authentification par mot de passe : Réduit les risques de force brute.
Utilisation d’un pare-feu : Limitez l’accès SSH aux adresses IP de confiance.
Changer le port SSH par défaut : Réduit les tentatives automatiques sur le port standard (22).
Explication des attaques de type brute-force SSH :

Description : Les attaques par brute-force SSH consistent à essayer de multiples combinaisons de noms d’utilisateur et de mots de passe pour accéder au serveur.
Risques : Si des mots de passe faibles sont utilisés, un attaquant peut réussir à se connecter et compromettre le système.
Techniques supplémentaires pour se protéger :

Changer le port SSH par défaut :

Port 2222
Ajoutez ou modifiez cette ligne dans /etc/ssh/sshd_config.
Redémarrage SSH :
systemctl restart ssh
Connexion via le nouveau port :
bash
Copier le code
ssh -p 2222 root@ip_serveur
Limiter les utilisateurs pouvant se connecter via SSH :

Utiliser des clés SSH avec une passphrase :

Avantage : Ajoute une couche de sécurité supplémentaire en chiffrant la clé privée.
Inconvénient : Nécessite de saisir la passphrase lors de chaque connexion, sauf si vous utilisez un agent SSH (ssh-agent).

2. Étude des processus UNIX
2.1 Exercice : Étude des processus UNIX

Étapes à suivre :

Partie 1 : Afficher les processus avec ps
Afficher la liste de tous les processus :


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

le plus de CPU:
<table border="1">
  <tr>
    <th>USER</th>
    <th>PID</th>
    <th>%CPU</th>
    <th>%MEM</th>
    <th>STAT</th>
    <th>STARTED</th>
    <th>TIME</th>
    <th>COMMAND</th>
  </tr>
  <tr>
    <td>root</td>
    <td>596</td>
    <td>0.1</td>
    <td>0.0</td>
    <td>I</td>
    <td>12:25:09</td>
    <td>00:00:04</td>
    <td>kworker/0:1-events_power_efficient</td>
  </tr>
</table>

Quel a été le premier processus lancé après le démarrage du système ?
<table border="1">
  <tr>
    <th>PID</th>
    <th>TTY</th>
    <th>TIME</th>
    <th>CMD</th>
  </tr>
  <tr>
    <td>1</td>
    <td>?</td>
    <td>00:00:00</td>
    <td>systemd</td>
  </tr>
</table>

Commande pour l'heure de démarrage :

**who -b**
         <table border="1">
  <tr>
    <th>Event</th>
    <th>Date</th>
    <th>Time</th>
  </tr>
  <tr>
    <td>System Boot</td>
    <td>2024-10-15</td>
    <td>10:23</td>
  </tr>
</table>


**uptime**
<table border="1">
  <tr>
    <th>Time</th>
    <th>Uptime</th>
    <th>Users</th>
    <th>Load Average (1 min)</th>
    <th>Load Average (5 min)</th>
    <th>Load Average (15 min)</th>
  </tr>
  <tr>
    <td>13:10:06</td>
    <td>1:03</td>
    <td>2 users</td>
    <td>0.00</td>
    <td>0.00</td>
    <td>0.00</td>
  </tr>
</table>


Nombre approximatif de processus créés depuis le démarrage :
**cat /proc/stat | grep processes**
processes 809

Partie 2 : Relations de parenté des processus
Option de ps pour afficher le PPID :
**ps -eo pid,ppid,cmd**

<table border="1">
  <tr>
    <th>PID</th>
    <th>PPID</th>
    <th>CMD</th>
  </tr>
  <tr>
    <td>1</td>
    <td>0</td>
    <td>/sbin/init</td>
  </tr>
  <tr>
    <td>2</td>
    <td>0</td>
    <td>[kthreadd]</td>
  </tr>
  <tr>
    <td>3</td>
    <td>2</td>
    <td>[rcu_gp]</td>
  </tr>
  <tr>
    <td>4</td>
    <td>2</td>
    <td>[rcu_par_gp]</td>
  </tr>
  <tr>
    <td>5</td>
    <td>2</td>
    <td>[slub_flushwq]</td>
  </tr>
  <tr>
    <td>6</td>
    <td>2</td>
    <td>[netns]</td>
  </tr>
  <tr>
    <td>8</td>
    <td>2</td>
    <td>[kworker/0:0H-events_highpri]</td>
  </tr>
  <tr>
    <td>10</td>
    <td>2</td>
    <td>[mm_percpu_wq]</td>
  </tr>
  <tr>
    <td>11</td>
    <td>2</td>
    <td>[rcu_tasks_kthread]</td>
  </tr>
  <tr>
    <td>12</td>
    <td>2</td>
    <td>[rcu_tasks_rude_kthread]</td>
  </tr>
  <tr>
    <td>13</td>
    <td>2</td>
    <td>[rcu_tasks_trace_kthread]</td>
  </tr>
  <tr>
    <td>14</td>
    <td>2</td>
    <td>[ksoftirqd/0]</td>
  </tr>
  <tr>
    <td>15</td>
    <td>2</td>
    <td>[rcu_preempt]</td>
  </tr>
  <tr>
    <td>16</td>
    <td>2</td>
    <td>[migration/0]</td>
  </tr>
  <tr>
    <td>18</td>
    <td>2</td>
    <td>[cpuhp/0]</td>
  </tr>
  <tr>
    <td>19</td>
    <td>2</td>
    <td>[kdevtmpfs]</td>
  </tr>
  <tr>
    <td>20</td>
    <td>2</td>
    <td>[inet_frag_wq]</td>
  </tr>
  <tr>
    <td>21</td>
    <td>2</td>
    <td>[kauditd]</td>
  </tr>
  <tr>
    <td>22</td>
    <td>2</td>
    <td>[khungtaskd]</td>
  </tr>
  <tr>
    <td>23</td>
    <td>2</td>
    <td>[oom_reaper]</td>
  </tr>
  <tr>
    <td>26</td>
    <td>2</td>
    <td>[writeback]</td>
  </tr>
  <tr>
    <td>27</td>
    <td>2</td>
    <td>[kcompactd0]</td>
  </tr>
  <tr>
    <td>28</td>
    <td>2</td>
    <td>[ksmd]</td>
  </tr>
  <tr>
    <td>29</td>
    <td>2</td>
    <td>[khugepaged]</td>
  </tr>
  <tr>
    <td>30</td>
    <td>2</td>
    <td>[kintegrityd]</td>
  </tr>
  <tr>
    <td>31</td>
    <td>2</td>
    <td>[kblockd]</td>
  </tr>
  <tr>
    <td>32</td>
    <td>2</td>
    <td>[blkcg_punt_bio]</td>
  </tr>
  <tr>
    <td>33</td>
    <td>2</td>
    <td>[tpm_dev_wq]</td>
  </tr>
  <tr>
    <td>34</td>
    <td>2</td>
    <td>[edac-poller]</td>
  </tr>
  <tr>
    <td>35</td>
    <td>2</td>
    <td>[devfreq_wq]</td>
  </tr>
  <tr>
    <td>36</td>
    <td>2</td>
    <td>[kworker/0:1H-kblockd]</td>
  </tr>
  <tr>
    <td>37</td>
    <td>2</td>
    <td>[kswapd0]</td>
  </tr>
  <tr>
    <td>43</td>
    <td>2</td>
    <td>[kthrotld]</td>
  </tr>
  <tr>
    <td>45</td>
    <td>2</td>
    <td>[acpi_thermal_pm]</td>
  </tr>
  <tr>
    <td>47</td>
    <td>2</td>
    <td>[mld]</td>
  </tr>
  <tr>
    <td>48</td>
    <td>2</td>
    <td>[ipv6_addrconf]</td>
  </tr>
  <tr>
    <td>53</td>
    <td>2</td>
    <td>[kstrp]</td>
  </tr>
  <tr>
    <td>58</td>
    <td>2</td>
    <td>[zswap-shrink]</td>
  </tr>
  <tr>
    <td>59</td>
    <td>2</td>
    <td>[kworker/u3:0]</td>
  </tr>
  <tr>
    <td>123</td>
    <td>2</td>
    <td>[ata_sff]</td>
  </tr>
  <tr>
    <td>124</td>
    <td>2</td>
    <td>[scsi_eh_0]</td>
  </tr>
  <tr>
    <td>125</td>
    <td>2</td>
    <td>[scsi_tmf_0]</td>
  </tr>
  <tr>
    <td>126</td>
    <td>2</td>
    <td>[scsi_eh_1]</td>
  </tr>
  <tr>
    <td>127</td>
    <td>2</td>
    <td>[scsi_eh_2]</td>
  </tr>
  <tr>
    <td>128</td>
    <td>2</td>
    <td>[scsi_tmf_2]</td>
  </tr>
  <tr>
    <td>129</td>
    <td>2</td>
    <td>[scsi_tmf_1]</td>
  </tr>
  <tr>
    <td>158</td>
    <td>2</td>
    <td>[jbd2/sda1-8]</td>
  </tr>
  <tr>
    <td>159</td>
    <td>2</td>
    <td>[ext4-rsv-conver]</td>
  </tr>
  <tr>
    <td>199</td>
    <td>1</td>
    <td>/lib/systemd/systemd-journald</td>
  </tr>
  <tr>
    <td>228</td>
    <td>1</td>
    <td>/lib/systemd/systemd-udevd</td>
  </tr>
  <tr>
    <td>244</td>
    <td>1</td>
    <td>/lib/systemd/systemd-timesyncd</td>
  </tr>
  <tr>
    <td>276</td>
    <td>2</td>
    <td>[cryptd]</td>
  </tr>
  <tr>
    <td>297</td>
    <td>2</td>
    <td>[irq/18-vmwgfx]</td>
  </tr>
  <tr>
    <td>362</td>
    <td>1</td>
    <td>dhclient -4 -v -i -pf /run/dhclient.enp0s3.pid -lf /var/lib/dhcp/dhclient.enp0s3.leases -I -df /var/lib/dhcp</td>
  </tr>
  <tr>
    <td>413</td>
    <td>1</td>
    <td>/usr/sbin/cron -f</td>
  </tr>
  <tr>
    <td>414</td>
    <td>1</td>
    <td>/usr/bin/dbus-daemon --system --address=systemd: --nofork --nopidfile --systemd-activation --syslog-only</td>
  </tr>
  <tr>
    <td>416</td>
    <td>1</td>
    <td>/lib/systemd/systemd-logind</td>
  </tr>
  <tr>
    <td>418</td>
    <td>1</td>
    <td>/sbin/wpa_supplicant -u -s -O DIR=/run/wpa_supplicant GROUP=netdev</td>
  </tr>
  <tr>
    <td>422</td>
    <td>1</td>
    <td>/bin/login -p --</td>
  </tr>
  <tr>
    <td>435</td>
    <td>1</td>
    <td>/lib/systemd/systemd --user</td>
  </tr>
  <tr>
    <td>436</td>
    <td>435</td>
    <td>(sd-pam)</td>
  </tr>
  <tr>
    <td>442</td>
    <td>422</td>
    <td>-bash</td>
  </tr>
  <tr>
    <td>596</td>
    <td>2</td>
    <td>[kworker/0:1-events]</td>
  </tr>
  <tr>
    <td>627</td>
    <td>1</td>
    <td>sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups</td>
  </tr>
  <tr>
    <td>646</td>
    <td>627</td>
    <td>sshd: root@pts/0</td>
  </tr>
  <tr>
    <td>652</td>
    <td>646</td>
    <td>-bash</td>
  </tr>
  <tr>
    <td>746</td>
    <td>2</td>
    <td>[kworker/u2:1-flush-8:0]</td>
  </tr>
  <tr>
    <td>825</td>
    <td>2</td>
    <td>[kworker/0:0-ata_sff]</td>
  </tr>
  <tr>
    <td>908</td>
    <td>2</td>
    <td>[kworker/u2:0-events_unbound]</td>
  </tr>
  <tr>
    <td>974</td>
    <td>2</td>
    <td>[kworker/0:2-ata_sff]</td>
  </tr>
  <tr>
    <td>978</td>
    <td>652</td>
    <td>ps -eo pid,ppid,cmd</td>
  </tr>
</table>

Explication : Affiche le PID, le PPID (Parent PID) et la commande associée à chaque processus.
Liste ordonnée de tous les processus ancêtres de la commande ps en cours d’exécution :
**ps -o pid,ppid,comm**

<table border="1">
    <thead>
        <tr>
            <th>PID</th>
            <th>PPID</th>
            <th>COMMAND</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>652</td>
            <td>646</td>
            <td>bash</td>
        </tr>
        <tr>
            <td>981</td>
            <td>652</td>
            <td>ps</td>
        </tr>
    </tbody>
</table>


Partie 3 : Utilisation de pstree
Installer pstree si nécessaire :

apt update
apt install psmisc
pstree fait partie du paquet psmisc.
Afficher l’arborescence des processus :


pstree -p
Explication : Affiche tous les processus sous forme d’arborescence avec les PID.
Afficher les ancêtres d’un processus spécifique (par exemple, ps) :

bash
Copier le code
pstree -s $(pgrep ps)
Partie 4 : Utilisation de top
Lancer top :

Afficher les processus triés par utilisation mémoire décroissante :

Dans top, appuyez sur la touche M (majuscule) pour trier par %MEM.
<table border="1">
    <thead>
        <tr>
            <th>PID</th>
            <th>USER</th>
            <th>PR</th>
            <th>NI</th>
            <th>VIRT</th>
            <th>RES</th>
            <th>SHR</th>
            <th>S</th>
            <th>%CPU</th>
            <th>%MEM</th>
            <th>TIME+</th>
            <th>COMMAND</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>199</td>
            <td>root</td>
            <td>20</td>
            <td>0</td>
            <td>33452</td>
            <td>11672</td>
            <td>10888</td>
            <td>S</td>
            <td>0.0</td>
            <td>0.6</td>
            <td>0:00.30</td>
            <td>systemd-journal</td>
        </tr>
        <tr>
            <td>1</td>
            <td>root</td>
            <td>20</td>
            <td>0</td>
            <td>37472</td>
            <td>10564</td>
            <td>8564</td>
            <td>S</td>
            <td>0.0</td>
            <td>0.5</td>
            <td>0:00.98</td>
            <td>systemd</td>
        </tr>
        <tr>
            <td>646</td>
            <td>root</td>
            <td>20</td>
            <td>0</td>
            <td>18008</td>
            <td>10008</td>
            <td>8664</td>
            <td>S</td>
            <td>0.0</td>
            <td>0.5</td>
            <td>0:00.58</td>
            <td>sshd</td>
        </tr>
    </tbody>
</table>


Quel est le processus le plus gourmand sur votre machine ? À quoi correspond-il ?
"systemd-journal"

Passer l’affichage en couleur : Appuyez sur z.
Mettre en avant la colonne de tri : Appuyez sur f puis utilisez les flèches pour sélectionner la colonne souhaitée.
Changer la colonne de tri : Appuyez sur O (lettre O majuscule) puis sélectionnez la colonne.


Installation de htop :

apt install htop
Lancer htop :

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
bg
Lancer le deuxième script et le mettre en arrière-plan :

./date-toto.sh
Pendant l’exécution, appuyez sur CTRL+Z pour suspendre le processus.
bg
Arrêter les scripts en utilisant jobs, fg et CTRL-C :

Liste des jobs :

Ramener un job au premier plan et l’arrêter :

fg %1
Puis appuyez sur CTRL-C pour arrêter.
fg %2
Puis appuyez sur CTRL-C pour arrêter.

root@vbox:~# jobs<br>
[1]-  Stopped                 sleep 1<br>
[2]+  Stopped                 sleep 1<br>


root@vbox:~# fg %1<br>
sleep 1

root@vbox:~# jobs<br>
[2]+  Stopped                 sleep 1<br>


root@vbox:~# fg %2<br>
sleep 1

root@vbox:~# jobs

Arrêter les scripts en utilisant ps et kill :

Trouver les PID des scripts :

bash
Copier le code
ps all | grep date.sh
0     0   657   548  20   0   8248  2132 pipe_r S+   pts/0      0:00 grep date.sh
ps all | grep date-toto.sh
0     0   675   548  20   0   8248  2148 pipe_r S+   pts/0      0:00 grep date-toto.sh
Utiliser kill pour arrêter les processus :

kill <PID_date.sh>
**kill 657**
kill <PID_date-toto.sh>
**kill 675**

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

Explication : Liste les fichiers et dossiers dans le répertoire courant (ls) et passe la liste à cat, qui l’affiche sur le terminal. La seul différent a ls est que la liste est mise en colonne .
Résultat:
**authorized_keys
liste
ok
ok.pub**

$ ls -l | cat > liste :

Explication : Liste détaillée des fichiers (ls -l), passe cette liste à cat, qui redirige la sortie vers le fichier liste, écrasant son contenu précédent.
Contenu de liste: 
 <table border="1">
    <thead>
        <tr>
            <th>Permissions</th>
            <th>Links</th>
            <th>User</th>
            <th>Group</th>
            <th>Size</th>
            <th>Date</th>
            <th>Time</th>
            <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-rw-r--r--</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>0</td>
            <td>Oct 9</td>
            <td>14:17</td>
            <td>authorized_keys</td>
        </tr>
        <tr>
            <td>-rw-r--r--</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>0</td>
            <td>Oct 15</td>
            <td>17:58</td>
            <td>liste</td>
        </tr>
        <tr>
            <td>-rw-------</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>2590</td>
            <td>Oct 2</td>
            <td>12:33</td>
            <td>ok</td>
        </tr>
        <tr>
            <td>-rw-r--r--</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>563</td>
            <td>Oct 2</td>
            <td>12:33</td>
            <td>ok.pub</td>
        </tr>
    </tbody>
</table>



$ ls -l | tee liste :
Explication : Liste détaillée(permission sur le ficher, qui la crée le ficher, la taille, date de création) des fichiers (ls -l), passe cette liste à tee, qui l’écrit dans le fichier liste tout en l’affichant sur le terminal.
Résultat: <table border="1">
    <thead>
        <tr>
            <th>Permissions</th>
            <th>Links</th>
            <th>User</th>
            <th>Group</th>
            <th>Size</th>
            <th>Date</th>
            <th>Time</th>
            <th>Name</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>-rw-r--r--</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>0</td>
            <td>Oct 9</td>
            <td>14:17</td>
            <td>authorized_keys</td>
        </tr>
        <tr>
            <td>-rw-r--r--</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>0</td>
            <td>Oct 15</td>
            <td>17:58</td>
            <td>liste</td>
        </tr>
        <tr>
            <td>-rw-------</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>2590</td>
            <td>Oct 2</td>
            <td>12:33</td>
            <td>ok</td>
        </tr>
        <tr>
            <td>-rw-r--r--</td>
            <td>1</td>
            <td>root</td>
            <td>root</td>
            <td>563</td>
            <td>Oct 2</td>
            <td>12:33</td>
            <td>ok.pub</td>
        </tr>
    </tbody>
</table>



$ ls -l | tee liste | wc -l :

Explication : Liste détaillée des fichiers (ls -l), passe cette liste à tee qui l’écrit dans le fichier liste et la passe ensuite à wc -l qui compte le nombre de lignes, affichant ainsi le nombre total d’éléments listés.
Résultat: 5

4. Journal système rsyslog
Objectif : Comprendre le fonctionnement du service rsyslog et ses configurations.

Étapes à suivre :

Vérifier si le service rsyslog est lancé et obtenir son PID :

ps aux | grep rsyslog
<table border="1">
    <thead>
        <tr>
            <th>USER</th>
            <th>PID</th>
            <th>%CPU</th>
            <th>%MEM</th>
            <th>VSZ</th>
            <th>RSS</th>
            <th>TTY</th>
            <th>STAT</th>
            <th>START</th>
            <th>TIME</th>
            <th>COMMAND</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>root</td>
            <td>576</td>
            <td>0.0</td>
            <td>0.1</td>
            <td>8248</td>
            <td>2120</td>
            <td>pts/0</td>
            <td>S+</td>
            <td>18:29</td>
            <td>0:00</td>
            <td>grep rsyslog</td>
        </tr>
    </tbody>
</table>
le PID de rsyslog est 576
Fichier de configuration principal et fichiers de destination des messages :

Fichier principal : /etc/rsyslog.conf

Fichiers de destination :

Messages issus des services standards : Généralement /var/log/syslog ou /var/log/messages.
Autres messages : Peuvent être dirigés vers des fichiers spécifiques comme /var/log/auth.log, /var/log/kern.log, etc.

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
Questions :
Quel modèle de processeur Linux détecte-t-il sur votre machine ?
Recherchez les lignes contenant "CPU" ou "processor".
**dmesg | grep -i cpu**
<table border="1">
    <thead>
        <tr>
            <th>Time</th>
            <th>Component</th>
            <th>Message</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>0.217773</td>
            <td>smpboot</td>
            <td>CPU0: 11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz (family: 0x6, model: 0x8c, stepping: 0x1)</td>
        </tr>
    </tbody>
</table>

Quels modèles de cartes réseaux détecte-t-il ?
Recherchez les lignes contenant "Ethernet" ou le nom du fabricant de la carte réseau.
**dmesg | grep -i network**
<table border="1">
    <thead>
        <tr>
            <th>Time</th>
            <th>Component</th>
            <th>Message</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>1.421180</td>
            <td>e1000</td>
            <td>Intel(R) PRO/1000 Network Driver</td>
        </tr>
    </tbody>
</table>


