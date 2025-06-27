Rôle Ansible WordPress - Documentation

Vue d'ensemble du projet Transformation d'un script shell Install_wordpress.sh en rôle Ansible complet pour automatiser le déploiement de WordPress avec MariaDB sur Ubuntu et Rocky Linux dans des environnements conteneurisés.

Analyse du script original Le script Install_wordpress.sh effectue :

Installation des paquets : apache2, php, libapache2-mod-php, php-mysql, mariadb-server, wget, unzip

Démarrage des services avec service apache2 restart et mysqld_safe --datadir=/var/lib/mysql &

Configuration MariaDB avec utilisateur example / mot de passe examplePW

Téléchargement WordPress depuis https://wordpress.org/latest.zip

Configuration automatique de wp-config.php

Configuration Apache avec VirtualHost

Architecture du rôle text wordpress-deploy/ ├── defaults/main.yml # Variables configurables ├── vars/main.yml # Variables fixes et paquets par OS ├── tasks/ │ ├── main.yml # Point d'entrée │ ├── debian.yml # Tâches Ubuntu/Debian │ ├── redhat.yml # Tâches Rocky Linux │ └── common.yml # Workflow principal ├── handlers/main.yml # Gestionnaires de services ├── tests/ │ ├── inventory # Tests locaux │ └── test.yml # Playbook de test └── meta/main.yml # Métadonnées Galaxy

Stratégie d'adaptation Gestion multi-OS Debian/Ubuntu : Reproduction exacte du script avec apt et service apache2

Rocky Linux : Adaptation avec dnf, httpd et initialisation MariaDB spécifique

Défis résolus Services conteneurisés : Utilisation de service au lieu de systemd

MariaDB Rocky Linux : Ajout de mysql_install_db et permissions

Variables cohérentes : Respect des valeurs du script (example/examplePW)

Idempotence : Conditions creates et when appropriées

Workflow du rôle :

    Installation paquets selon l'OS (debian.yml/redhat.yml)

    Démarrage services adapté par OS

    Configuration MariaDB avec commandes exactes du script

    Téléchargement WordPress avec wget et unzip

    Configuration wp-config.php avec sed comme le script

    Configuration Apache avec VirtualHost et modules

Tests et validation bash
Test syntaxe

ansible-playbook tests/test.yml --syntax-check
Exécution

ansible-playbook -i tests/inventory tests/test.yml
Vérification WordPress

docker inspect client1 # Récupérer IP curl http://IP_CONTAINER # Accès WordPress

http//localhost:IP_PORT # pour accéder à la page web de wordpress.

Le rôle transforme avec succès le script shell en solution Ansible professionnelle, maintenable et publiable sur Galaxy.


