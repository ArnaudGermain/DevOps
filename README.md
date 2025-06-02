# DevOps

TP1 :
a) commande pour récup l'image : docker pull httpd
b) vérfifier que l'image est en local : docker images
c) Fichier dans le repo local : mkdir -p ./html
                                echo "Hello World" > ./html/index.html
d) Création du container avec Nginx : docker run -d -p 8080:80 -v $(pwd)/html:/usr/share/nginx/html --name my-web nginx
e) Suppression du container : docker stop my-web
                              docker rm my-web
f) Relancer le container sans l'option -v : docker run -d -p 8080:80 --name my-web nginx
copier le fichier dans le container : docker cp ./html/index.html my-web:/usr/share/nginx/html/index.html




 
