# Abaixo os códigos-fonte para você copiar e colar

## Executando um contêiner

docker pull ubuntu
docker run ubuntu
docker run ubuntu sleep 10
docker run ubuntu sleep 1500
docker stop [id]
docker run --help
docker run -it ubuntu

## Executando aplicações no contêiner

docker run -dti  ubuntu 
docker exec -it [id ou nome]  /bin/bash

## Excluindo e nomeando contêineres

docker stop [id]
docker rm [id]
docker rmi [imagem]

docker run -dti --name Ubuntu-A ubuntu

## Copiando arquivos para o contêiner

docker exec -ti Ubuntu-A /bin/bash
docker exec Ubuntu-A mkdir /destino/
docker exec Ubuntu-A mkdir ls -l /
nano Arquivo.txt
docker cp arquivo.txt Ubuntu-A:/aula/

## Copiando arquivos do container

docker cp Ubuntu-A:/destino/Meuzip.zip  Zipcopia.zip

## Criando um contêiner do MySQL

docker cp Ubuntu-A:/destino/Meuzip.zip  Zipcopia.zip

## Acessando um container externamente

docker pull mysql
 
docker run -e MYSQL_ROOT_PASSWORD=Senha123 --name mysql-A -d -p 3306:3306 mysql

docker exec -it mysql-A bash

mysql -u root -p --protocol=tcp


CREATE DATABASE aula;
show databases;

docker inspect mysql-A

mysql -u root -p --protocol=tcp


## Montando um local de armazenamento

docker run -e MYSQL_ROOT_PASSWORD=Senha123 --name mysql-A -d -p 3306:3306 --volume=/data:/var/lib/mysql mysql

mysql -u root -p --protocol=tcp --port=3306

CREATE TABLE alunos (
    AlunoID int,
    Nome varchar(50),
    Sobrenome varchar(50),
    Endereco varchar(150),
    Cidade varchar(50)
);



INSERT INTO alunos (AlunoID, Nome, Sobrenome, Endereco, Cidade) VALUES (1, 'Carlos Alberto', 'da Silva', 'Av. que sobe e desce que ninguém conhece', 'Manaus');


## Exemplos de tipos de mount

****bind mount *****

docker run -dti --mount type=bind,src=/opt/teste,dst=/teste debian

docker run -dti --mount type=bind,src=/opt/teste,dst=/teste,ro debian

***volumes****

docker volume create teste

docker volume ls

	/var/lib/docker/volumes/teste/_data
	
docker run -dti --mount type=volume,src=teste,dst=teste debian

docker volume rm teste

## Apache container

docker run  --name apache-A -d -p 80:80 --volume=/data/apache-A:/usr/local/apache2/htdocs/ httpd

docker run  --name php-A -d -p 8080:80 --volume=/data/php-A:/var/www/html php:7.4-apache




<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8"/>
<title>Exemplo Apache</title>
</head>
<body>
<h1> OK !! Apache funcionando!!!!! </h1>
</body>
</html>




<?php
phpinfo();
?>


## Limitando memória e CPU

docker stats php-A

#docker update php-A -m 128M --cpus 0.2

docker run --name ubuntu-C -dti -m 128M --cpus 0.2 ubuntu

apt update && apt install stress

stress --cpu 1 --vm-bytes 50m --vm 1 --vm-bytes 50m


## Redes

apt-get install iputils-ping

docker network create minha-rede

docker run -dit --name Ubuntu-B --network minha-rede  ubuntu
