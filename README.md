# architecture-microservices

## Architecture
L'architecture est construite à l'aide de six __micro-services__ containerisés. Chaque __micro-service__ correspond à un __processus__. Chaque __micro-service__ porte une ou plusieurs __responsabilités__.

#### Le répartiteur de charge 
* Responsabilités :
	* Réécrire les URLs en HTTP en URLs HTTPS
	* Vérifierla validité du certificat SSL
	* Répartir les requêtes entre les deux serveurs WEB en Round-Robin.
	* Gérer l'affinité de session utilisateur. 
	
### Les deux serveurs Web
* __Responsabilités__:
	* Soulager le backend WordPress en servant les ressources statiques
	* Transmettre les requêtes PHP vers un backend WordPress
* __Mise en oeuvre__:
	* Utilisation d'une image Docker de base __nginx:latest__
	* Customisation de l'image de base
		* Surcharge du fichier de configuration __default.conf__
			* Customisation du server_name  backdoor.monblog.etna
			* Customisation des répertoires de logs (access_log, error_log)
			* Proxification des requêtes PHP vers le backend wordpress
	* Deux containers:
		* front-01 et front-02
		* Le frontend front-01 :
			* Dépend du backend wp-backend-01
			* Proxifie les requêtes PHP vers le backend 01
			* Ecrit ses logs dans /var/log/nginx/frontend01.access.log et /var/log/nginx/frontend01.error.log
		* Le frontend front-02:
			* Dépend du backend wp-backend-02
			* Proxifie les requêtes PHP vers le backend 02
			* Ecrit ses logs dans /var/log/nginx/frontend02.access.log et /var/log/nginx/frontend02.error.log
	* Utilisation de deux volumes :
		* Permet de ne pas perdre les données si on perd un container frontend
		* Un volume est associé au répertoire __/var/www/html__
		* Un volume associé au répertoire __/var/log/nginx__
	
### Les deux backend WordPress
* __Responsabilités:__
	* Gestion de contenu (CMS)
* __Mise en oeuvre:__
	* Utilisation d'une image Docker de base __wordpress:latest__
	* Customisation de l'image de base
		* Spécification des variables d'environnement
			* ENV WORDPRESS_DB_HOST 
			* ENV WORDPREES_DB_USER
			* ENV WORDPRESS_DB_PASSWORD
			* ENV WORDPRESS_DB_NAME
	* Deux containers 
		* wp-backend01 et wp-backend02
		* Chaque backend dépend du container database et expose le port __80__
	* Utilisation d'un volume:
		* Permet de ne pas perdre les données si on perd un container wp-backend
		* Le volume est associé au répertoire __/var/www/html__
		* Les deux backends Wordpress utilisent les mêmes données (répertoire de la machine Hote)
	
### La base de données MariaDB
* __Responsabilités:__
	* Persistance des données.
* __Mise en oeuvre:__
	* Utilisation d'une image de base  __mariadb/server:10.3__
	* Le service MariaDB expose le port __3306__.
	* Customisation de l'image de base
		* Nécessaire pour pouvoir accéder à Mariadb depuis l'extérieur du container mariadb.
		* Surcharge du __my.cnf__ (bind-address        = 0.0.0.0)
	* Utilisation d'un volume
		* Permet de ne pas perdre les données si on perd le container mariadb.
		* Le volume est associé au répertoire __/var/lib/mysql__
		
	

		
		
