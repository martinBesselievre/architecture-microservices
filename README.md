# architecture-microservices

## Architecture
L'architecture est construite à l'aide de six __micro-services__ containerisés. 

* Chaque service correspond à un processus.
* Chaque service porte une ou plusieurs responsabilités.

* Service de répartition de charge : Load Balancer
		Le service repose sur un container HAProxy.
                Le service est configuré pour recevoir des requêtes correspondant au hostname backdoor.monblog.etna.
                Seuls les requêtes en  HTTPS pourront arriver jusqu'aux deux serveurs WEB après vérification de la validité du certificat SSL.
                Pour cela le service HAProxy reécrit les URLs HTTP en URL HTTPS.
                Le service répartit la charge entre les deux serveurs WEB.
                Les requêtes sont envoyées successivement à l'un ou l'autre des deux serveurs WEB (Round-Robin)
                Le service est configuré pour gérer l'affinité de session.
		
		
