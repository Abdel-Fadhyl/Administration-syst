# Administration des systèmes UNIX
Configuration d’un serveur Debian, programmation des tâches avec CRON, sécurisation des infiltrations avec le framework Fail2ban, gestion des groupes d’utilisateurs et définition des droits partagés.

## Etape 1
Assertion du port 22 accessible par SSH.

## Etape 2
Crêation d'user ainsi que création de groupe avec des commandes UNIX.

## Etape 3
Octroiement de droit par les commandes UNIX.

## Etape 4

    Tout d'abord, avec les configurations actuelles je ne peux entrer dans le dossier compta donc pour cela je me suis octroyé le droit de lire, écrire et exécuter avec cette commande sudo setfacl -m u:djoi_a:rwx compta.
    Ensuite, les droits d'accès des fichiers com_marc, com_jean et com_tata sont lire et écrire.
    Pour finir, oui Marc peut aller dans le fichier jean_com tout simplement parce qu' il fait partie du groupe commercial.
    
## Etape 6
    
Afin d'afficher la valeur des variables d'environnement on doit appliquer la commande env dans l'user.

## Etape 7
    
### Apache 2.4
    Mettre à jour le terminal : sudo apt-get update
    Pour installer Apache 2.4 : sudo apt-get install apache2
    
### Php 7.4
    Vérifier que mon système est à jour : sudo apt-get update && apt-get –yes –force-yes –fix-missing –auto-remove
                                          sudo apt list --upgradable
    
    Télécharger la clé GPG : sudo apt -y install lsb-release apt-transport-https ca-certificates
                             sudo wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
    
    Ajoutez le dépôt PPA : echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | sudo tee /etc/apt/sources.list.d/php.list
    
    Installation Php 7.4 : sudo apt update
                           sudo apt -y install php7.4

Pour finir, pour pouvoir bloquez les versions de ces paquets pour empêcher leur mise à jour j'ai fait la commande : apt-mark hold apache2
                                                                                                                    apt-mark hold php 7.4
                                                                                                                    
## Etape 8
    
### VM A(la mienne)
      Pour effectuer cette étape je suis entrer dans l'utilisateur bob et j'ai généré une clé ssh public avec ces commandes : ssh-keygen -t rsa -b 2048
    Puis j'ai fait cette commande afin de pouvoir envoyer ma clé ssh à un de mes camarades :  sudo ssh-copy-id -i ~/.ssh/id_rsa.pub bob@172.16.234.43 -p2242
    Ensuite je me suis reconnecter a mon compte utilisateur et je suis aller dans le répertoire .ssh afin de pouvoir faire un : cat authorized_keys pour voir la clé de mon camarade à qui j'ai envoyer ma clé
    Après j'ai fait un : chmod 400 ~/.ssh/id_rsa pour changer le droit de lecture du fichier et j'ai refait un : ssh-copy-id bob@172.16.234.43 -p2242
    Pour finir j'ai fait plusieurs chmod :
                                                        -chmod 700 ~/.ssh (Pour s'assurer que les droits sont bien configurés sur le serveur)
                                                        -chmod 600 ~/.ssh/authorized_keys (Pour s'assurer que les droits sont bien configurés sur le serveur)
                                                        vérifier que les droits du dossier d’origine de l’utilisateur distant
                                                        -sudo chown bob /home/bob (il doit lui appartenir)
                                                        -sudo chmod 750 /home/bob(les droits ne doivent pas être trop ouverts)
    et j'ai essayé de voir si sa marchais et du coup oui ça marchait pour ma VM

### VM B(la VM de mon camarade)
    Mon camarade pour cette étape à fait de son coté il a effectuer plusieurs chmod :
                                                        -chmod 700 ~/.ssh (Pour s'assurer que les droits sont bien configurés sur le serveur)
                                                        -chmod 600 ~/.ssh/authorized_keys (Pour s'assurer que les droits sont bien configurés sur le serveur)
                                                         vérifier que les droits du dossier d’origine de l’utilisateur distant
                                                        -chown bob /home/bob (il doit lui appartenir)
                                                        -chmod 750 /home/bob (les droits ne doivent pas être trop ouverts)

## Etape 9
   
   Afin de réaliser cette étape, j'ai fait un répertoire configurations puis j'ai créé quelques fichiers.
    Ensuite, j'ai effectué cette commande afin de faire la synchronisation ajout d'un ou plusieurs fichier : rsync -az -r -rve 'ssh -p2242' /data/admin/configurations bob@172.16.234.43:/data/admin/
    Enfin pour finir, j'ai effectué cette commande pour synchroniser la suppression d'un ou plusieurs fichier : rsync --delete -az -r -rve 'ssh -p2242' /data/admin/configurations bob@172.16.234.43:/data/admin/  

