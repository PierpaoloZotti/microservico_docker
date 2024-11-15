# Docker: Utilização prática no cenário de Microsserviços
## Curso Azure - Digital Innovation One

---

### 1. Criação do volumes para armazenamento de arquivos

```
sudo docker volume create app
```
```
sudo docker volume create data
```

### 2. Criar um container do MySQL

   ```
   sudo docker run -e MYSQL_ROOT_PASSWORD=minhasenha -e MYSQL_DATABASE=mercado --name mysql-A -d -p 3306:3306 --mount type=volume,src=data,dst=/var/lib/mysql/ mysql:5.7
   ```

### 4. Criação de um container Apache com PHP 7

   ```
   sudo docker run --name web-server -dt -p 8080:80 --mount type=volume,src=app,dst=/app/ webdevops/php-apache:alpine-php7
   ```

### 6. Criação de um Cluster Swarm e inclusão de novos nós

   ```
   sudo docker swarm init sudo docker swarm join sudo docker node ls
   ```

### 7. Criar um serviço "clusterizado"
##### Executar esses comandos no nó gerenciador:

   ```
   sudo docker service create --name web-server --replicas 10 -dt -p 80:80 --mount type=volume,src=app,dst=/app/ webdevops/php-apache:alpine-php7
   ```

   ```
   sudo docker service ps web-server
   ```

### 8. Instalação do NFS para replicação dos volumes entre os nós

```
sudo apt-get install nfs-server
```

```
sudo vim /etc/exports
```

Copy this into the exports file:
```
/var/lib/docker/volumes/app/_data *(rw,sync,subtree_check)
```

```
sudo exportfs -ar
```

##### Os seguentes comandos tem que ser rodados nos nós de trabalho:

```
sudo apt-get install nfs-common
```

```
sudo mount -o v3 172.31.0.54:/var/lib/docker/volumes/app/_data /var/lib/docker/volumes/app/_data
```

### 9. Criação da imagem do proxy nginx e execução do container

```
docker build -t proxy-app
```

```
docker container run --name my-proxy-app -dti -p 4500:4500 proxy-app
```
