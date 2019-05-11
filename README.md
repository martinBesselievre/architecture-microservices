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
* Responsabilités :
        * Soulager le backend WordPress en servant les ressources statiques
	* Transmettre les requêtes PHP vers un backend WordPress
	
### Les deux backend WordPress
* Responsabilités:
	* Gestion de contenu (CMS)
	
### La base de données MariaDB
* __Responsabilités:__
	* Persistance des données.
* __Mise en oeuvre:__
	* Utilisation d'une image de base  mariadb/server:10.3
	* Le service MariaDB expose le port 3306.
	* Customisation de l'image mariadb/server:10.3 
		* Nécessaire pour pouvoir accéder à Mariadb depuis l'extérieur du container mariadb.
		* Surcharge du my.cnf (bind-address        = 0.0.0.0)
	* Utilisation d'un volume
		* Permet de ne pas perdre les données si on perd le container mariadb.
		* Le volume est associé à l'emplacement /var/lib/mysql
		
	

		
		
