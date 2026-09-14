### Polecenia docker

* **docker start {nazwa}** - uruchamia kontener
* **docker start -i {nazwa}** - uruchamia kontener i podłącza się do jego wyjścia
* **docker stop {nazwa}** - zatrzymuje kontener
* **docker restart {nazwa}** - restartuje kontener
* **docker exec -it {nazwa} {polecenie}** - wykonuje polecenie w kontenerze (`/bin/bash` | `bash`, `phpunit`, `/usr/sbin/nginx`, `redis-cli`)
* **docker images** - lista wszystkich obrazów
* **docker ps -a** - lista wszystkich kontenerów (także zatrzymanych)
* **docker system df** - zużycie dysku przez Dockera
* **docker system info** - informacje o środowisku Dockera
* **docker system df --format "{{.Type}} {{.Size}}"** - zużycie dysku z filtrem (pokazuje typ i rozmiar)
* **docker build -t {nazwa obrazu} {plik konfiguracyjny}** - buduje obraz z pliku konfiguracyjnego
* **docker rm {nazwa/hash}** - kasuje kontener (musi być wcześniej zatrzymany)
* **docker rmi {nazwa/hash}** - kasuje obraz (najpierw trzeba zatrzymać lub skasować kontener)
* **docker run -id --name {kontener} {obraz}** - tworzy i uruchamia kontener z obrazu
  * **-u www-data** - uruchamia z podanym użytkownikiem
  * **-it** - uruchamia polecenie w konsoli
  * **--rm** - kasuje kontener po zamknięciu
* **docker history {obraz}** - historia warstw obrazu
* **docker run -id -v lokalny/plik:/plik/w/kontenerze -p 3307:3306 --name {kontener} {obraz}** - tworzy kontener z podmontowanym plikiem i przekierowaniem portu
* **docker network inspect {nazwa sieci}** - informacje o wewnętrznej sieci Dockera
* **docker network inspect $(docker network ls -q)** - informacje o wszystkich sieciach Dockera
* **docker network ls** - lista sieci Dockera
* **docker logs {kontener}** - logi kontenera (z `-f` na bieżąco, jak `tail -f`)
* **docker kill {kontener}** - ubija działający kontener
* **docker search {nazwa}** - szuka obrazów w repozytorium Dockera
* **docker stats** - zużycie zasobów przez kontenery (`docker stats --no-stream` - pokazuje tylko raz)
* **docker top {nazwa}** - lista procesów działających w kontenerze
* **docker inspect {nazwa}** - szczegóły kontenera
* **docker update {nazwa} --restart always** - aktualizuje kontener i ustawia uruchamianie po każdym restarcie (`no` - wyłącza restart)
* **docker run -it --name {nazwa kontenera} --rm --entrypoint /bin/bash {obraz}** - ustawia własny entrypoint
* **docker exec -i {nazwa kontenera} mysql -u {login} -p{hasło} {baza} <<< "{polecenie sql}"** - bezpośrednie podanie polecenia SQL do kontenera
* **nohup docker exec -i {kontener} {polecenie} > {log} 2>&1 & echo $! > {pid}** - uruchamia proces w kontenerze w tle i zapisuje id procesu do pliku
* **docker volume ls** - lista volumenów
* **docker volume rm {nazwa}** - kasuje volumen
* **docker system prune** - sprząta nieużywane kontenery, sieci i obrazy
* **docker builder prune** - czyści cache buildera obrazów
* **docker tag {hash} {nazwa obrazu}:{tag}** - nadaje tag obrazowi

### Polecenia docker compose

* **docker-compose top** - szczegóły kontenerów uruchomionych przez compose (z katalogu ze środowiskiem)
* **docker-compose -f {config.yml} up -d** - uruchamia kontenery zdefiniowane w compose
* **docker-compose build --verbose** - buduje lub przebudowuje kontenery
* **docker-compose ps** - lista kontenerów uruchomionych przez compose
* **docker-compose -f {config.yml} up -d --build redis** - buduje pojedynczy kontener z compose

### Przykłady użycia

* **docker cp {plik} {kontener}:{docelowa ścieżka}** - kopiuje plik do kontenera
* **docker cp {kontener}:{plik} {docelowa ścieżka}** - kopiuje plik z kontenera
  * **-a** - zachowuje atrybuty
* **docker rmi -f $(docker images | grep "<none>" | awk "{print \$3}")** - kasuje uszkodzone obrazy
* **docker rmi $(docker images --filter "dangling=true" -q --no-trunc)** - kasuje nieużywane obrazy
* **docker exec -it {kontener} mysql -u root --password={hasło} -e "CREATE USER '{użytkownik}'@'%' IDENTIFIED BY '{hasło}';"** - tworzy użytkownika bazy bez wchodzenia do kontenera
* **docker exec -it {kontener} mysql -u root --password={hasło} -e "GRANT ALL PRIVILEGES ON *.* TO '{użytkownik}'@'%' IDENTIFIED BY '{hasło}' WITH GRANT OPTION;"** - nadaje użytkownikowi pełne uprawnienia

### Inne

* **screen ~/Library/Containers/com.docker.docker/Data/vms/0/tty** - dostęp do maszyny wirtualnej z Dockerem na macOS
  * **Ctrl+a d** - wyjście
* **docker rm $(docker ps -a -q)** - kasuje wszystkie kontenery
* **docker volume rm $(docker volume list -q)** - kasuje wszystkie volumeny
