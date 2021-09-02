### Docker commands

* **docker start {name}** - Start docker container
* **docker start -i {name}** - Start docker container with execute information
* **docker stop {name}** - Stop docker container
* **docker restart {name}** - Restart docker container
* **docker exec -it {name} {command}** - Execute command inside of container (/bin/bash | bash, phpunit, /usr/sbin/nginx, redis-cli)
* **docker images** - Display all created images
* **docker ps -a** - Display all containers (also exited)
* **docker system df** - Show docker disk usage
* **docker system info** - Display docker environment information
* **docker system df --format "{{.Type}} {{.Size}}"** - Docker disk usage with filters (show type and size)
* **docker build -t {image name} {docker conf file}** - Build image from docker configuration file
* **docker rm {name/hash}** - Remove container (must be killed before)
* **docker rmi {name/hash}** - Remove image (firs must be killed or removed container)
* **docker run -id --name {container} {image}** - Create container from image
* **docker history {image}** - Show image history
* **docker run -id -v local/file:/container/file -p 3307:3306 --name {container} {image}** - Create container with file link and port forwarding
* **docker network inspect {network name}** - Display information about docker internal network
* **docker network inspect $(docker network ls -q)** - Information about all docker networks
* **docker network ls** - List of docker networks
* **docker logs {container}** - Display docker container logs (with `-f` live like tail -f)
* **docker kill {container}** - Kill working container
* **docker search {name}** - Search existing images on docker community
* **docker stats** - Docker containers system resource usage (docker stats --no-stream - display only once)
* **docker top {name}** - List of launched process inside of container
* **docker inspect {name}** - Container details
* **docker update {name} --restart always** - aktualizuje kontener i ustawia uruchamiania po każdym restarcie (`no` - wyłącza restart)
* **docker run -it --name {nazwa kontenera} --rm --entrypoint /bin/bash {skrypt}** - ustawiamy własny entrypoint
* **docker exec -i {nazwa kontenera} mysql -u {login} -p{hasło} {tabela} <<< "polecenie sql"** - bezpośrednie podanie polecenia sql do kontenera
* **nohup docker exec -i {kontener} {polecenie} > {log} 2>&1 & echo $! > {pid}** - uruchamia proces w kontenerze, tak aby działał w tle i zapisuje id procesu do pliku
* **docker volume ls** - lista volumenów
* **docker volume rm {nazwa}** - kasowanie volumenów
* **docker system prune** - 

### Docker compose commands

* **docker-compose top** - Display details of docker containers launched by docker compose (inside of docker env directory)
* **docker-compose -f {config.yml} up -d** - Start docker containers defined in compose
* **docker-compose build --verbose** - Build or rebuild containers
* **docker-compose ps** - List of started by docker compose containers
* **docker-compose -f {config.yml} up -d --build redis** - Build single container from docker compose

### Some useful commands usage

* **docker cp {plik} {kontener}:{docelowa ścierzka}** - kopiuje plik do kontenera
* **docker cp {kontener}:{plik} {docelowa ścierzka}** - kopiuje plik z kontenera
  * **-a** - zachowuje atrybuty
* **docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")** - remove broken images
* **docker rmi $(docker images --filter "dangling=true" -q --no-trunc)** - remove unused images
* __docker exec -it {container} mysql -u root --password=* -e "CREATE USER '{user_name}'@'%' IDENTIFIED BY '{pass};"__
* __docker exec -it {container} mysql -u root --password=* -e "GRANT ALL PRIVILEGES ON *.* TO '{user_name}'@'%' IDENTIFIED BY '{pass}' WITH GRANT OPTION;";__

### Other

* **screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty** - dostęp do virtualki z dockerem na macku
  * **Ctrl+a d** - wyjście
* **docker rm $(docker ps -a -q)** - kasuje wszystkie kontenery
* **docker rm $(docker volume ls -q)** - kasuje wszystkie volumeny