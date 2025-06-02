TP1

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

a) On crée le fichier Dockerfile avec la commande nano :

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
