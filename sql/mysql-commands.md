### Połączenie i podstawy

* **mysql -u {użytkownik} -p** - łączy się z konsolą MySQL (zapyta o hasło)
* **mysql -u {użytkownik} -p {baza}** - łączy się od razu z wybraną bazą
* **exit;** - wychodzi z konsoli
* **show databases;** - lista wszystkich baz
* **create database {baza};** - tworzy nową bazę
* **use {baza};** - wybiera bazę, na której pracujemy
* **select database();** - pokazuje aktualnie wybraną bazę
* **show tables;** - lista tabel w aktualnej bazie
* **describe {tabela};** - struktura tabeli
* **show index from {tabela};** - lista indeksów tabeli
* **SHOW TABLE STATUS FROM `{baza}` WHERE `name` LIKE '{tabela}';** - szczegóły tabeli (silnik, liczba rekordów, rozmiar, kodowanie)
* **SHOW TABLE STATUS LIKE '{tabela}'\G;** - to samo, wypisane pionowo (jedno pole na linię)
* **SHOW VARIABLES LIKE "%version%";** - informacje o wersji serwera
* **SHOW VARIABLES WHERE Variable_name = 'hostname';** - adres IP / nazwa hosta serwera MySQL
* **SELECT @@global.time_zone, @@session.time_zone; SELECT @@system_time_zone;** - strefa czasowa serwera, sesji i systemu

### Import i eksport

* **mysqldump -u {użytkownik} -p {baza} > dump.sql** - zrzuca bazę do pliku
  * **--lock-tables=false** - zrzut bez blokowania tabel
  * **-p{password} {baza} {tabela}** - z podanym hasłem i konkretnątabelą
* **mysqldump -u {użytkownik} -p -h 127.0.0.1 {baza} | pv -W > dump.sql** - zrzut po TCP z paskiem postępu
* ** kubectl exec -it mysql-677567d476-7fqp5 mysqldump {baza} > plik** - zrzut z kubernetesa
* **docker exec -it local-mysql mysqldump {baza} > plik.sql** - zrzut z dockera
* **mysql -u {użytkownik} -p {baza} < dump.sql** - wgrywa dumpa
* **pv dump.sql | mysql -u {użytkownik} -p {baza}** - wgrywa dumpa z paskiem postępu
* **cat dump.sql | docker exec -i {kontener} mysql -u root -p{hasło} {baza}** - wgrywa dumpa do bazy działającej w kontenerze Dockera

### Monitorowanie

* **show processlist;** - lista aktualnie wykonywanych zapytań
* **watch -n 5 'echo "show processlist;" | mysql -u {użytkownik} -p{hasło}'** - odświeża listę procesów co 5 sekund
* **mysqladmin -h {host} -u root -p{hasło} extended -r -i 10 | grep 'row'** - statystyki serwera na poziomie rekordów, odświeżane co 10 sekund
* **SELECT table_schema "baza", sum(data_length + index_length)/1024/1024 "rozmiar w MB" FROM information_schema.TABLES GROUP BY table_schema;** - rozmiar każdej bazy w MB
* **SELECT table_schema "baza", sum(data_length + index_length)/1024/1024 "rozmiar w MB", sum(data_free)/1024/1024 "wolne w MB" FROM information_schema.TABLES GROUP BY table_schema;** - rozmiary baz razem z miejscem do odzyskania

### Tabele i kolumny

* **CREATE TABLE {tabela} ({kolumna} VARCHAR(120), {inna_kolumna} DATETIME);** - tworzy tabelę z kolumnami
* **ALTER TABLE {tabela} ADD COLUMN {kolumna} VARCHAR(120);** - dodaje kolumnę
* **ALTER TABLE {tabela} ADD COLUMN {kolumna} int NOT NULL AUTO_INCREMENT PRIMARY KEY;** - dodaje kolumnę z unikalnym, automatycznie zwiększanym id
* **ALTER TABLE {tabela} DROP COLUMN {kolumna};** - usuwa kolumnę
* **DROP TABLE {tabela};** - kasuje tabelę
* **DROP DATABASE {baza};** - kasuje bazę

### Zapytania

* **SELECT * FROM {tabela};** - pobiera wszystkie rekordy
* **SELECT {kolumna}, {inna_kolumna} FROM {tabela};** - pobiera tylko wybrane kolumny
* **SELECT {kolumna} AS {alias} FROM {tabela};** - własna nazwa kolumny w wyniku
* **SELECT DISTINCT {kolumna} FROM {tabela};** - pobiera bez duplikatów
* **EXPLAIN SELECT * FROM {tabela};** - pokazuje plan wykonania zapytania
* **SELECT COUNT({kolumna}) FROM {tabela};** - liczy rekordy
* **SELECT *, (SELECT COUNT({kolumna}) FROM {tabela}) AS count FROM {tabela} GROUP BY {kolumna};** - liczy i pobiera rekordy pogrupowane
* **SELECT * FROM {tabela} WHERE {kolumna} = {wartość};** - pobiera wskazane rekordy (także `<`, `>`, `!=`; łączenie przez `AND`, `OR`)
* **SELECT * FROM {tabela} WHERE {kolumna} LIKE '%{wartość}%';** - rekordy zawierające wartość
* **SELECT * FROM {tabela} WHERE {kolumna} LIKE '{wartość}%';** - rekordy zaczynające się od wartości
* **SELECT * FROM {tabela} WHERE {kolumna} LIKE 'war_tość';** - rekordy pasujące do wzorca (`_` to jeden dowolny znak)
* **SELECT * FROM {tabela} WHERE {kolumna} BETWEEN {wartość1} AND {wartość2};** - pobiera zakres
* **SELECT * FROM {tabela} ORDER BY {kolumna} ASC LIMIT {wartość};** - własna kolejność i limit rekordów (`ASC`, `DESC`)

### Modyfikacja danych

* **INSERT INTO {tabela} ({kolumna}, {inna_kolumna}) VALUES ('{wartość}', '{wartość}');** - dodaje rekord
* **NOW()** - aktualna data i godzina, do kolumn typu datetime
* **UPDATE {tabela} SET {kolumna} = '{wartość}' WHERE {kolumna} = {wartość};** - aktualizuje rekordy
* **DELETE FROM {tabela} WHERE {kolumna} = {wartość};** - kasuje pasujące rekordy
* **DELETE FROM {tabela};** - kasuje wszystkie rekordy, zostawiając tabelę (zeruje też licznik auto increment)
* **TRUNCATE TABLE {tabela};** - kasuje wszystkie rekordy, szybciej niż DELETE

### Funkcje agregujące

* **SELECT SUM({kolumna}) FROM {tabela};** - suma kolumny
* **SELECT {kolumna_kategorii}, SUM({kolumna}) FROM {tabela} GROUP BY {kolumna_kategorii};** - suma pogrupowana po kategorii
* **SELECT MAX({kolumna}) FROM {tabela};** - największa wartość
* **SELECT MIN({kolumna}) FROM {tabela};** - najmniejsza wartość
* **SELECT AVG({kolumna}) FROM {tabela};** - średnia wartość
* **SELECT {kolumna_kategorii}, ROUND(AVG({kolumna}), 2) FROM {tabela} GROUP BY {kolumna_kategorii};** - zaokrąglona średnia pogrupowana po kategorii

### Wiele tabel

* **SELECT {tabela1}.{kolumna}, {tabela2}.{kolumna} FROM {tabela1}, {tabela2};** - pobiera z wielu tabel
* **SELECT * FROM {tabela1} INNER JOIN {tabela2} ON {tabela1}.{kolumna} = {tabela2}.{kolumna};** - łączy rekordy pasujące w obu tabelach
* **SELECT * FROM {tabela1} LEFT OUTER JOIN {tabela2} ON {tabela1}.{kolumna} = {tabela2}.{kolumna};** - łączy rekordy, zostawiając wszystkie rekordy lewej (pierwszej) tabeli
* **SELECT {tabela1}.{kolumna} AS '{alias}', {tabela2}.{kolumna} AS '{alias}' FROM {tabela1}, {tabela2};** - zmienia nazwy kolumn na aliasy

### Użytkownicy i uprawnienia

* **SELECT User, Host FROM mysql.user;** - lista wszystkich użytkowników
* **CREATE USER '{użytkownik}'@'localhost' IDENTIFIED BY '{hasło}';** - tworzy nowego użytkownika
* **GRANT ALL ON {baza}.* TO '{użytkownik}'@'localhost';** - nadaje pełny dostęp do wszystkich tabel bazy
* **SHOW GRANTS FOR CURRENT_USER;** - uprawnienia aktualnego użytkownika
* **SHOW GRANTS FOR '{użytkownik}'@'localhost';** - uprawnienia wskazanego użytkownika
* **ALTER USER '{użytkownik}'@'localhost' IDENTIFIED BY '{hasło}';** - zmienia hasło użytkownika
* **SET PASSWORD FOR '{użytkownik}'@'localhost' = PASSWORD('{hasło}');** - zmienia hasło użytkownika (starsze wersje MySQL)
