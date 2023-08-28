# Abaixo os códigos-fonte para você copiar e colar

## Executando um contêiner

'''
docker pull ubuntu
docker run ubuntu
docker run ubuntu sleep 10
docker run ubuntu sleep 1500
docker stop [id]
docker run --help
docker run -it ubuntu
'''

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

```
CREATE TABLE alunos (
    AlunoID int,
    Nome varchar(50),
    Sobrenome varchar(50),
    Endereco varchar(150),
    Cidade varchar(50)
);
```



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



```
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
```


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

## Primeiro Dockerfile
 docker run -dti --name ubuntu-python ubuntu

 docker exec -ti ubuntu-python bash


 apt install python3 nano
2# apt clean


	nome = input("Qual é o seu nome? ")
	print (nome)



nano dockerfile

FROM ubuntu

RUN apt update && apt install -y python3 && apt clean

COPY app.py /opt/app.py

CMD python3 /opt/app.py

docker build . -t python-ubuntu

dc


## Criando uma imagem personzalizada do Apache


wget http://site1368633667.hospedagemdesites.ws/site1.zip

Dockerfile

==================

FROM debian

RUN apt-get update && apt-get install -y apache2 && apt-get clean

ADD site.tar /var/www/html

LABEL description="Apache Webserver 1.0"

VOLUME /var/www/html/

EXPOSE 80


ENTRYPOINT ["/usr/sbin/apachectl"]

CMD ["-D", "FOREGROUND"]


======================

docker image build -t debian-apache:1.0 .

docker run  -dti -p 80:80 --name meu-apache debian-apache:1.0


## Criando imagens personalizadas a partir de imagens de linguagens de programacao

```
nome = input("Qual é o seu nome? ")
	print (nome)
```


=====================================

FROM python

WORKDIR /usr/src/app

COPY app.py /usr/src/app

CMD [ "python", "./app.py" ]


## Criando uma imagem multistage

docker pull golang

docker pull alpine

============================================================

```
package main
import (
    "fmt"
)

func main() {
  fmt.Println("Qual é o seu nome:? ")
  var name string
  fmt.Scanln(&name)
  fmt.Printf("Oi, %s! Eu sou a linguagem Go! ", name)
}
```


=============================================================

FROM golang as exec

COPY app.go /go/src/app/

ENV GO111MODULE=auto

WORKDIR /go/src/app

RUN go build -o app.go .

FROM alpine

WORKDIR /appexec

COPY --from=exec /go/src/app/ /appexec

RUN chmod -R 755 /appexec

ENTRYPOINT ./app.go

==============================================================

docker image build -t app-go:1.0 .

docker run -ti  app-go:1.0


## Realizando o upload de imagens para o hub do Docker

docker login

docker build . -t nome-de-usuário/my-go=app:1.0

docker push nome-deu-usuário/my-go=app:1.0


## Registry, criando um servidor de imagens

docker run -d -p 5000:5000 --restart=always --name registry registry:2

docker logout

docker image tag [id] localhost:5000/meu-apache:1.0


curl localhost:5000/v2/_catalog


docker push  localhost:5000/my-go-app:1.0


nano /etc/docker/daemon.json

```
	{ "insecure-registries":["10.0.0.189:5000"] }
```	

systemctl restart docker

docker push  localhost:5000/my-go-app:1.0


## Docker compose exemplo pratico

docker-compose.yml

====================

```
version: '3.8'

services:

  mysqlsrv:
  
    image: mysql:5.7
    
    environment:
    
      MYSQL_ROOT_PASSWORD: "Senha123"
      
      MYSQL_DATABASE: "testedb"
      
    ports:
    
      - "3306:3306"
      
    volumes:
    
      - /data/mysql-C:/var/lib/mysql
      
    networks:
    
      - minha-rede

  adminer:
  
    image: adminer
    
    ports:
    
      - 8080:8080
      
    networks:
    
      - minha-rede

networks: 

  minha-rede:
  
    driver: bridge
```    

docker-compose up -d


## Exemplo PHP-APACHE-MYSQL

===============================================

docker-compose.yml

===============================================

```
version: "3.7"

services:
  web:
    image: webdevops/php-apache:alpine-php7
    ports:
      - "4500:80"
    volumes:
      - /data/php/:/app

    networks:
      - minha-rede

  db:
    image: mysql:5.7
    environment:
      MYSQL_ROOT_PASSWORD: "Senha123"
      MYSQL_DATABASE: "testedb"
    ports:
      - "3306:3306"
    volumes:
      - /data/mysql-C:/var/lib/mysql

    networks:
      - minha-rede

  phpmyadmin:
    image: phpmyadmin/phpmyadmin
    environment:
      MYSQL_ROOT_PASSWORD: "Senha123"
    ports:
      - "8080:80"
    volumes:
      - /data/php/admin/uploads.ini:/usr/local/etc/php/conf.d/php-phpmyadmin.ini

    networks:
      - minha-rede

networks:
   minha-rede:
     driver: bridge

```

=============================================================

 uploads.ini
 
=============================================================
 
file_uploads = On

memory_limit = 500M

upload_max_filesize = 500M

post_max_size = 500M

max_execution_time = 600

max_file_uploads = 50000

max_execution_time = 5000

max_input_time = 5000

=============================================================

index.php

=============================================================

```
<html>

<head>
<title>Exemplo PHP</title>
</head>


<?php
ini_set("display_errors", 1);
header('Content-Type: text/html; charset=iso-8859-1');



echo 'Versao Atual do PHP: ' . phpversion() . '<br>';

$servername = "db";
$username = "root";
$password = "Senha123";
$database = "testedb";

// Criar conexão


$link = new mysqli($servername, $username, $password, $database);

/* check connection */
if (mysqli_connect_errno()) {
    printf("Connect failed: %s\n", mysqli_connect_error());
    exit();
}

$query = "SELECT * FROM tabela_exemplo";

if ($result = mysqli_query($link, $query)) {

    
    while ($row = mysqli_fetch_assoc($result)) {
        printf ("%s %s %s <br>", $row["nome"], $row["cidade"], $row["salario"]);
    }

    
    mysqli_free_result($result);
}


mysqli_close($link);

?>

</html>
```
