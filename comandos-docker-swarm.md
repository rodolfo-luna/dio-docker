## Iniciando um cluster swarm

```
hostnamectl hostname aws1

docker swarm init

docker swarm leave

docker swarm join --token [token]

docker node ls
```

## Cluster em infraestrutura local

```
vagrant init

code .
```

```
	config.vm.provision "shell", path: "instalar-docker.sh"
```

```
vagrant up

vagrant ssh

vagrant destroy -f
```

## Criando um servico em um cluster

```
docker service create --name web-server --replicas 10 -p 80:80 httpd

docker service ps web-server 

docker node update --availability drain aws1

docker service rm web-server
```

## Consistencia de dados em um cluster

```
cd /var/lib/docker/volumes/app/_data

docker service create --name meu-app --replicas 15 -dt -p 80:80 --mount type=volume,src=app,dst=/usr/local/apache2/htdocs/ httpd

  
sudo apt-get install nfs-server


nano /etc/exports 
	cd /var/lib/docker/volumes/app/_data *(rw,sync,subtree_check)

exportfs -ar
```
aws2

======

```
sudo apt-get install nfs-common

showmount -e [ip]

mount [ip]:/var/lib/docker/volumes/app/_data /var/lib/docker/volumes/app/_data
```


## Exemplo de aplicacao em cluster

```
docker volume create data

docker run -e MYSQL_ROOT_PASSWORD=Senha123 -e MYSQL_DATABASE=meubanco --name mysql-A -d -p 3306:3306 --mount type=volume,src=data,dst=/var/lib/mysql/ mysql:5.7


docker service create --name meu-app --replicas 15 -dt -p 80:80 --mount type=volume,src=app,dst=/app/ webdevops/php-apache:alpine-php7
```
