# harborprojet
Lucas, Nicolas et Raphael

Initialisation Git:
		$ mkdir TPGitlab
		$ cd TPGitlab
		$ git init
		$ git commit -m "first commit, init gitlab project"
		$ git checkout master	
		$ git add *
		$ git push origin master
		

Initialisation environnement:
		$ sudo apt-get update
	
Docker :
		$ sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose -$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
		
		$ sudo docker-compose --version
	
GITLAB-CI :
	- Nous allons ensuite créer le docker-compose.yml qu'on peut voir dans le fichier "plan d'action"
	- Il faudra par la suite modifier le fichier /etc/hosts avec la commande sudo nano /etc/hosts pour rajouter la ligne suivant :
		 <IP_VM>    gitlab.example.com
	- On exécute ensuite le docker compose avec la commande 
		$ sudo docker-compose up -d
	- On peut maintenant se connecter sur gitlab.example.com avec un nav privée. On récupère le mdp au chemin ‘/etc/gitlab/initial_root_password’ avev un username = root
	
Préapartion environnement Harbor :
	Récupération des packages necessaires qui sont : 
		- harbor-offline-installer-v2.3.4.5.tgz
		- harbor-offline-installer-v2.3.4.5.tgz.asc
	
	Obtention de la clé public, et on extrait le package
		$ gpg --keyserver hkps://keyserver.ubuntu.com --receive-keys 644FF454C0B4115C
		$ tar zxvf harbor-offline-installer-v*.tgz
	
	On va générer la clé privé de certificat CA : 
		$ openssl genrsa -out ca.key 4096
		$ openssl req -x509 -new -nodes -sha512 -days 3650 \ -subj “/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=yourdomain.com" \ -key ca.key \ -out ca.crt
	On génère ensuite le certificat
		$ openssl req -x509 -new -nodes -sha512 -days 3650 \ -subj /C=CN/ST=../L=../O=example/OU=../CN=yourdomain.com" \-key ca.key \ -out ca.crt
		
	Génération clé privée
		$  openssl genrsa -out yourdomain.com.key 4096
	
	Génération certificat
		$ openssl req -sha512 -new \ -subj “/C=CN/ST=Beijing/L=Beijing/O=example/OU=Personal/CN=yourdomain.com" \ -key yourdomain.com.key \ -out yourdomain.com.csr
	
	Création d'un fichier v3.ext ceux qui va permettre de générer le certificat Harbor
	
	On copie le certificat et la clé dans le dossier hôte de harbor
		$ cp yourdomain.com.crt /data/cert/
		$ cp yourdomain.com.key /data/cert/

	Préparation de l'environnement des clé et certificat
	
		$ cp yourdomain.com.cert /etc/docker/certs.d/yourdomain.com/
		$ cp yourdomain.com.key /etc/docker/certs.d/yourdomain.com/
		$ cp ca.crt /etc/docker/certs.d/yourdomain.com/

		
	Redemarrage de docker
		$ systemctl restart docker
		
		
Lancement des scripts pour Harbor :
	
	Modifié le harbor.yml en précisant le chemin du certificat et de la clè (ici /etc/docker/certs.d/yourdomain.com
	
	Lancement du script permettant de déployer Harbor, il va créer un docker-compose.yml
		$ ./prepare
		
		$ docker-compose down -v
		
		$ docker-compose up -d
		

	Ce qui va permettre à la suite de pouvoir se connecter à l'interface de Harbor grâce à l'adresse yourdomain.com avec un username = admin et le password = Harbor12345 par défault.
	
Alternative :
	On peux lancer le script ./install qui permet directement d'installer docker, docker-compose, de charger les images Harbor et de préparer l'environnement Harbor 
		
		
		
		
		
			
