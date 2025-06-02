# TP1 - Docker & DevOps

---
```bash
 Partie 3

 a) Commande pour récupérer l'image :

docker pull httpd
b) Vérifier que l'image est en local :
docker images
c) Fichier dans le repo local :
mkdir -p ./html
echo "Hello World" > ./html/index.html
d) Création du container avec Nginx :
docker run -d -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html --name my-web nginx
e) Suppression du container :
docker stop my-web
docker rm my-web
f) Relancer le container sans l'option -v :
docker run -d -p 8080:80 --name my-web nginx
docker cp ./html/index.html my-web:/usr/share/nginx/html/index.html
Partie 4 – Builder une image

a) Création du fichier Dockerfile :
FROM nginx:alpine
COPY ./html/index.html /usr/share/nginx/html/index.html
EXPOSE 80
b) Construction et exécution de l'image Docker :
docker build -t custom-nginx .
docker stop my-web
docker run -d -p 8080:80 custom-nginx
c) Différences entre -v et COPY :
-v permet un développement plus rapide et des modifications directes.
COPY s’appuie sur un Dockerfile pour inclure les fichiers dans l’image.
Comparatif -v vs COPY

-v (montage de volume)
Avantages :

Permet de modifier les fichiers en local sans reconstruire l’image.
Rapide pour tester des changements.
Inconvénients :

Peut causer des conflits de permission ou des problèmes de performance selon le système.
COPY dans le Dockerfile
Avantages :

Image autonome : tout est inclus au moment du build.
Plus simple et plus propre pour les déploiements en production.
Inconvénients :

Nécessite un docker build à chaque changement de fichier.
Moins pratique pour les ajustements rapides.
Partie 5

a) Récupération des images depuis Docker Hub :
docker pull mysql
docker pull phpmyadmin/phpmyadmin
b) Création des 2 containers
1) Container MySQL :

docker run -d --name my-mysql \
  --network my-network \
  -e MYSQL_ROOT_PASSWORD=rootpass \
  -e MYSQL_DATABASE=mydb \
  mysql
2) Lier le container à PhpMyAdmin :

docker run -d --name my-phpmyadmin \
  --network my-network \
  -e PMA_HOST=my-mysql \
  -p 8081:80 \
  phpmyadmin/phpmyadmin
Partie 6

a) À quoi sert docker-compose ?
docker-compose permet d'exécuter des applications multi-containers définies dans un fichier docker-compose.yml. Contrairement à docker run, il permet de gérer plusieurs services en une seule commande et rend le projet plus lisible et maintenable.

b) Commandes utiles :
Lancer les containers :
docker-compose up -d
Stopper les containers :
docker-compose down
c) Exemple de fichier docker-compose.yml :
version: '3.8'

services:
  db:
    image: mysql:latest
    container_name: mysql-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: my-secret-pw
    networks:
      - my-network

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    container_name: my-admin
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: db
      PMA_PORT: 3306
    networks:
      - my-network

networks:
  my-network:
    driver: bridge
