# Polecenia konsoli Linux

## Operacje na plikach i katalogach
* **pwd** - aktualny katalog
* **ls -la** - listuje wszystkie dostępne pliki
* **ls -lat** - listuje wszystkie dostępne pliki posortowane według czasu modyfikacji
* **cp -r {nazwa pliku/katalogu} {miejsce docelowe}** - kopiuje plik lub cały katalog z zawartością we wskazane miejsce
* **mv {nazwa pliku/katalogu} {miejsce docelowe}** - przenosi plik lub cały katalog z zawartością we wskazane miejsce (jeśli lokalizacja pliku docelowego jest taka sama jak pliku do przeniesienia, zmienia nazwę)
* **du -sh** - pokazuje zajętość całego katalogu
* __du -sh *__ - pokazuje rozmiar poszczególnych plików i katalogów
* __du -sh \`ls -A\`__ - pokazuje rozmiar wszystkich (w tym ukrytych) plików i katalogów (można też użyć `$(ls -A)`)
* __du -sh .[!.]*__ - jak powyżej
* __du -hs .[^.]*__ - pokazuje rozmiar wszystkich ukrytych plików
* **du -ah** - pokazuje zajętość wszystkich plików w katalogu
* __ls -ld .*__ - wyświetla wszystkie ukryte pliki i katalogi
* **ls -li /etc | sort -n** - sortuje listę plików względem ich węzłów
* **df -h** - pokazuje zajętość wszystkich dysków
* **df -a** - pokazuje zajętość wszystkich urządzeń
* **df -hT** - pokazuje zajętość punktu montowania wraz z systemem plików
* **chmod 0755 {nazwa pliku/katalogu}** - zmiana praw dostępu (4-read, 2-write, 1-execute)
* **chmod ugo-x {nazwa pliku/katalogu}** - odbiera prawa wykonywania wszystkim użytkownikom (user, group, other)
  * **chmod g=-w;o=-w {plik}**
  * **chmod go=-w {plik}**
  * **chmod u+x {plik}**
  * **u+s** - ustawia setuid _(lub chmod 4755 -> rwsr-xr-x)_
  * **g+s** - ustawia setgid _(lub chmod 2644 -> rw-r-s-r--)_
  * **o+t** - ustawia sticky _(lub chmod 1777 -> rwxrwxrwt)_
* **chown user:group {nazwa pliku/katalogu}** - właściciela i grupy
* **ls | wc -l** - liczba plików i katalogów w aktualnym katalogu
* **tree** - drzewo katalogów i plików
* **tree -ugphD** - drzewo katalogów i plików wraz z informacjami o plikach i katalogach
* **find -iname {nazwa pliku}** - szuka podanego pliku bez uwzględnienia wielkości liter
* **find -size +100M** - szuka plików powyżej 100mb
* **find / {pattern}** - wyszukuja podanego wzorca w głównym katalogu i podkatalogach
* __find . -maxdepth 1 -name '*.json' -delete__ - usuwa dużą ilość plików z rozszerzeniem `*.json`
* **find . -type f -ctime -2 -print0** - pokazuje pliki utworzone w ciągu 2 dni i wyświetla jako linia (+1 starsze niz 2 dni)
  * **f** - plik
  * **d** - katalog
  * **c** - specjalny plik znakowy
  * **p** - plik fifo (named pipe)
  * **l** - link
  * **s** - socket
  * **-user** - użytkownik (-group)
  * **-atime -mtime -ctime** - czasy modyfikacjo, dostępu etc
* **find {dir} -type f -mtime +10d -exec rm {} \;** - kasuje pliki starsze niż 10 dni (rm {} - nawiasy zastępowane nazwą pliku)
* **find {katalog}/ -size +1G -exec bash -c "ls -lh '{}'" \;** - wyszukuje pliki większe niż 1GB i listuje je w outpucie
* **find {dir} -type d -exec sh -c '{program} $0/*' {} \; - wykonuje program dla wszystkich plików w katalogu
  * **-ok rm {} \;** - pyta czy usunąć plik
  * **find . -inum {węzeł} -ok rm {} \;** - kasowanie pliku z uszkodzoną nazwą (`ls -i` - zwróci numer węzła pliku)
  * **find / –perm –4000** - znajduje pliki z ustawionym _setuid_
* **cksum {nazwa pliku}** - pokazuje sumę kontrolną CRC i rozmiar pliku
* **ln -s {cel} {nazwa}** - tworzy link do pliku lub katalogu
* **"$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)/$(basename "${BASH_SOURCE[0]}")"** - zwraca ścieżkę w której znajduje się uruchomiony skrypt
* **ls -R | sort -R | tail -1** - wybiera losowy katalog lub plik z drzewa katalogu
* **rename -v 's/\.pdf$/\.doc/' *.pdf** - zmienia nazwy wszystkich plików *.pdf
* **(cd /{some}/{dir}/ && ls -l)** - zmienia katalog, wykonuje komendę i powraca do aktualnego katalogu
* **chgrp** - zmienia grupę dla plików/katalogów
* **echo {pliki/katalogi} | xargs -n 1 cp -vr {katalog}** - kopiuje pliki/katalogi do katalogu (-n ile przyjmuje argumentów, 1-każdy element z echo)
* **lsattr {plik/katalog}** - lista rozszerzonych atrybutów (tylko partycje ext) (chattr - ustawia atrybut)
* **chattr {plik/katalog}** - ustawia rozszerzone atrybuty
* **find . -maxdepth 1 -type d \( ! -name . \) -exec bash -c "cd '{}' && {polecenie}" \;** - wykonuje polecenie w każdym podkatalogu obecnej lokalizacji

### Pliki
* **cat {plik 1} {plik 2} {plik n} >> {plik docelowy}** - dodaje content z plików 1, 2 i 3 do pliku docelowego
* __cat {prefix}* >> {plik docelowy}__ - łączy pliki z podanym prefixem w jeden (np `cat baza_*.sql >> baza.sql`)
* **split -v 5M {plik} {prefix}** - dzieli podany plik na części po 5MB i dodaje prefix
  * **split -l 1000** - dzieli po 1000 linii
  * **split -b 1000** - dzieli po 1000 bajtw (b|k|m)
  * **split -`expr \`wc -l {plik} | awk '{print $1}'\` /$k` {plik} {prefix}** - dzieli plik na $k równych części
  * **csplit -k {plik} 100 {99}** - dzieli plik na części po 100 linii (ostatnia może być dłuższa)
* **nl {nazwa pliku}** - pokazuje zawartość pliku wraz numerami linii
* **dd** - kopiowanie pliku wraz z konwersją typu (dd if=/home/user/Downloads/debian.iso of=/dev/sdb1 bs=512M; sync)
* **dd id=/dev/device of=~/device.img** - kopiuje każdy bajt z urządzenia device do pliku
* **rm {nazwa pliku}** - kasuje plik
* **touch {nazwa pliku}** - tworzy pusty plik
* **ls -la | grep ^- | wc -l** - pokazuje liczbę plików w katalogu (-lar - + podkatalogi)
* **mv \`ls | head -n 2000\` test/** - przenosi 2000 plików do katalogu test
* **less** - wyświetla zawartość pliku (podobne: `more`)
* **less {plik} | grep "wyrażenie regularne"** - szuka w wybranym pliku podanej wartości
* **tail -f {nazwa pliku}** - pokazuje zmiany na żywo w podanym pliku
* **tail -2 {plik}** - wyświetla 2 ostatnie linie w pliku
* **stat {nawza pliku}** - rozszerzone informacje o pliku
* **file {nazwa pliku}** - pokazuje typ pliku
* **head -2 {plik}** - pokazuje 2 wiersze z podanego pliku
* **head -c5 {plik}** - wyświetli 5 pierwszych liter
* **md5sum {plik}** - oblicza sumę md5 
* **sha1sum {plik}** oblicza sumę sha1
* **wc -l {plik}** - liczba linii w pliku
* **find . -type f | wc -l** - liczba samych plików w katalogu
* __grep -ri "{pattern}" /jakiś/katalog/*__ - wyszukuje podany pattern we wszystkich plikach w katalogu i podkatalogach, bez względu na wielkość liter
* **grep –color -rw "{pattern}" .** - szuka tylko całych słów, zwraca pokolorowany otput
* **grep -c "{pattern}" .** - zwraca ilość znalezionych wzorców w plikach
* **grep -v "{pattern}" .** - zwraca wszystko z pominięciem wzorca
  * **grep -v "^git\|docker"**
* **grep -C 2 {pattern}** - zwraca 2 linie otaczające znaleziony fragment (2 z góry i 2 z dołu)
* **grep --include={plik} -A 1 -rn {ścieżka} -e {wzorzec}** - szuka wzorca w ścieżce i podanym pliku, wyświetla ścieżkę, numer linii + linia poniżej znalezionej
* **grep -o {wzorzec} {plik} | sort --unique | wc -l** - ilość unikalnych wystąpień wzorca
* **grep -o {wzorzec} {plik} | sort | uniqu -c** - pokazuje posortowane wzorce + ilość ich wystąpień
* **grep -rcw** - szuka rekursywnie wzorca w postaci całych słów i wyświetla tylko ilość znalezionych
* **diff {plik1} {plik2}** - porównuje ze sobą 2 pliki
  * **-y** - pokazuje w kolumnach (2 pliki, 2 kolumny)
  * **-a** - traktuja jako tekst
  * **-n** - format rcs
  * **-w** - ignoruje biale spacje
  * **diff <(sed -n '1p' {plik}) <(sed -n '1p' {plik})**
  * **diff -Nyrw dir1 dir2** - pokazuje różnice w plikach między dwoma katalogami
* **find {pattern} | xargs rm** - szuka plików i kasuje wszystkie znalezione
* **ls -laR | grep ^- | wc -l** - zlicza ilość plików w katalogu i podkatalogach
* **find . -type f | sort -R | tail -1** - wyszukuje losowy plik
* **locate {file}** - znajduje wszystkie pliki o podanej nazwie
* **cmp {plik1} {plik2}** - pokazuje różnicę między plikami
* **cmp --verbose {pliki}** - pokazuje kody różnic (bajt różnicy, kod znaku 1, kod znaku 2)
* **cmp -bl <(sed -n '1p' {plik}) <(sed -n '1p' {plik})** - porównuje pierwsze linie z plików
* **lsof** - pokazuje wszystkie otwarte pliki
  * **lsof -p {pid}** - pliki otwarte przez podany proces
* **fdupes -rnS {dir} >> duplicated** - znajduje i zapisuje do pliku listę zduplikowanych plików
* **qemu-img convert -f raw -O qcow2 image.img image.qcow2** - konwertuje pliki obrazów (np VM)
* **foremost -v -i {image} -o {out}** - odzyskiwanie skasowanych plików z obrazu do katalogu out
* **flock -xn {test.lock} -c "{script}"** - uruchamia skrypt i ustawia locka na pliku {test.lock}, do zakończenia nie da się ponownie uruchomić
* **echo '{string}' | cat - {plik} > temp && mv temp {plik}** - dodaje string na początek pliku
* **cat << EOF > {plik}** - uruchamia wpisywanie danych do pliku, linie oddzielone enterem
  * **EOF** - kończy wpisywanie do pliku
* **uuencode {plik} > {plik ze stringiem}** - konwertuje blik binarny na wartość tekstową (`uudecode` - proces odwrotny)

### Media
* **identify -format '%Q' {plik}** - zwraca informacje na temat kompresji
* **convert -quality 70 {źródło} {cel}** - zmienia stopień kompresji pliku np jpg
* **identify -verbose {img}** - odczytuje metadane z obrazu
* **exiftool {img}** - jw

### Katalogi
* **rm -fr {nazwa katalogu lub pliku}** - umożliwia kasowanie katalogu i zawartości wraz z wymuszeniem
* **mkdir {nazwa pliku}** - tworzy katalog (`mkdir {katalog} && cd {katalog}` - po utworzeniu od razu otwiera katalog)
* **touch {nazwa pliku}** - tworzy pusty plik
* **ls -la | grep ^- | wc -l** - pokazuje liczbę plików w katalogu
* **mv \`ls | head -n 2000\` test/**
* **du -sckx * | sort -nr** - sortuje katalogi według rozmiaru
* **cd -** - powraca do poprzedniego katalogu (przed wykonaniem polecenia `cd {katalog}`)
* **watch "ls -lrt | tail -10"** - obserwowanie zmian na katalogu
* **mkdir -p ~/katalog/{bin,src,pkg}** - tworzy katalog i w nim bin, src, pkg na tym samym poziomie
  * **take ~/katalog/{bin,src,pkg}** - j/w

### Archiwa
* **tar -xvzf {nazwa pliku}** - rozpakowuje archiwum `*.tar.gz`
* **tar -cvzf {nazwa archiwum} {nazwa pliku/katalogu}** - archiwizuje wskazany plik/katalog do podanego pliku jako `tar.gz`
* **tar -tvf {nazwa archiwum}** - lista zarchiwizowanych elementów
* **gzip {nazwa}** - kompresuje pliki jako `.gz`
* **gunzip {nazwa}** - rozpakowuje archiwum `*.gz`
* **zip -r0 {plik.zip} {katalog}** - zapisuje zawartość katalogu do pliku `zip` bez kompresji

---

## Użytkownicy
* **last** - pokazuje listę ostatnich logowań użytkowników (-n {liczba})
* **whoami** - podaje nazwę aktualnego użytkownika
* **users** - zalogowani użytkownicy
* **users | wc -w** - liczba zalogowanych użytkowników
* **id** - pokazuje id użytkownika, grupy, oraz do jakich grup należy\
* **sudo su - {nazwa użytkownika}** - przełącza na konto innego użytkownika
* **groups** - pokazuje grupy do których należy użytkownik
* **w** - pokazuje kto jest zalogowany i co robi
* **who** - pokazuje kto jest zalogowany
* **wall {wiadomość}** - wysyła wiadomość do użytkowników
* **mesg** - status wyświetlania wiadomości w terminalu (mesg y - włącza, mesg n - wyłącza)
* **write {użytkownik}** - włącza tryb pisania wiadomości do użytkownika
* **passwd** - zmiana własnego hasła
* **adduser {user} {grupa}** - dodaje nowego użytkownika
* **userdel {nazwa}** - usuwa użytkownika
* **addgroup {nazwa}** - dodaje grupę
* **groupdel {nazwa}** - usuwa grupę
* **sudo runuser -l {nazwa użytkownika} -c '{polecenie}'** - uruchamia poprzez innego użytkownika {polecenie}
* **sudo su - {nazwa użytkownika} -c "{polecenie}"** - jak powyżej
* **su - {user}** - przełącza usera
* **usermod** - modyfikuje ustawienia użytkownika
* **usermod -a -G {grupa} {user}** - dodaje usera do grupy
* **gpasswd -a {user} {grupa}** - j/w
* **htpasswd {user} {pass}** - tworzy plik htpasswd (-c nowy plik o wskazanej nazwie)

---
## Data i czas
* **cal** - pokazuje kalendarz aktualnego miesiąca
* **date** - pokazuje aktualną datę
* **date +"%T : %Y"** - pokazuje datę w formacie *HH:MM:SS : YYYY*
  * **date  --date="2 days ago"**
  * **date "+%Y-%m-%d"**
  * **date "+%H:%M:%S"**
  * **date +"%D %T"** - 05/18/18 12:16:46
  * **date +%N** - 563339704
  * **date +%s** - 1526638758
  * **date -R** - Fri, 18 May 2018 12:20:07 +0200
  * **date -jnu** - Fri May 18 10:21:13 UTC 2018
  * **date -v -1d '+%m-%d-%y'** - 05-17-18
  * **date -d @{timestamp}** - konwertuje timestamp na datę
* **sleep {sec} && {polecenie}** - uruchamia polecenie po określonej liczbie sekund (45m - minuty)

---
## System
* **uname -a** - pokazuje informację o systemie Linux
* **uname -snrvm** - j/w tylko bardziej szczegółowo
* **reboot** - urucahmia ponownie
* **shutdown -h {now | liczba minut | godzina:minuta}** - zamyka system natychmiast lub po podanej liczbie minut, lub czasie
* **init 0** - zabija cały system
* **uptime -p && uptime -s** - pokazuje czas pracy systemu, oraz datę uruchomienia
* **getconf LONG_BIT** lub **arch** pokazuje architekturę systemu
* **dstat** - listat statystyk systemowych
* **free** pokazuje ilość dostępnej pamięci
* **hostname** - pokazuje nazwę komputera w sieci
* **who -r** - pokazuje czas uruchomienia systemu oraz jego poziom
* **runlevel** - pokazuje poziom na którym zainicjalizowano system
* **top -bn1 | grep -i "%Cpu" | head -1 | sed "s/.*, *\([0-9]*,.\)%* \(id\|be\).*/\1/;s/,/./" | awk '{print 100 - $1}'** - pokazuje zużycie procesora
* **free | grep Mem | awk '{print $4/$2 * 100.0}'** - ilość wolnej pamięci w procentach
* **free | grep Mem | awk '{print $3/$2 * 100.0}'** - ilość zajętej pamięci w procentach
* **iostat -d /dev/sda | sed -n "4p"** - pokazuje zużycie dysku (/dev/sda)
* **cat /proc/loadavg | awk '{print $1,$2,$3}'** - pokazuje load systemowy
* **printenv** - pokazuje wszystkie zmienne środowiskowe i ich wartości
* **sudo dmidecode -t system** - informacje o systemie (w tym o urządzeniu)
* **top -l 1 -s 0 | grep PhysMem** - ilość zużytej i wolnej pamięci
* **lsmod** - pokazuje status modułów w jądrze linuxa
* **lsb_release -a** - informacje o dystrybucji (-s skrócone)
  * **cat /proc/version** - j/w
  * **cat /etc/*-release** - j/w
  * **cat /etc/system-release** - j/w
* **lslk** - lista locków
* **modprobe -a {modul1} {modul2}** - ładuje podane moduły które pasują do wzorca
  * **-c** - lista załadowanych modułów
* **crontab -e** - 
* **crontab -l** - lista wpisów w contab
* **crontab -l -u {user}** - 
* **vim /etc/crontab** - systemowy crontab (/etc/cron.hourly/)
* **for user in $(cut -f1 -d: /etc/passwd); do echo $user; crontab -u $user -l 2>/dev/null | grep -v '^#; done** - lista wpisów dla wszystkich userów

### Urządzenia
* **mount {urządzenie} {ścieżka docelowa}** - montuje urządzenie
* **unmount {urządzenie**} - odmontowuje urządzenie
* **mount -l** - lista podłączonych urządzeń
* **lsblk** - lista urządzeń blokowych ( -l)
* **sudo mount -r -o loop {obraz.iso} {/katalog}** - montuje obraz.iso do katalogu /katalog (-r tylko do odczytu)
* **sudo mount -t auto -o loop {name}.img {dir}** - montuje obraz (-t automatycznie wybiera system plików)
* **blkid** - id urządzeń blokowych
* **sudo fsck -Cyv {/dev/sdx}** - skanuje i naprawia system plików na dysku
* **lsusb** - lista urządzeń usb
* **sudo fdisk -l** - pokazuje informacje o systemie plików
* **sudo lshw** - pełna lista urządzeń systemowych
  * **-short** - jak wyżej, wersja skrócona (`-html` zapisuje w wersji html)
  * **-class disk** - informacje o dyskach
* **lscpu** - szczegółowe informacje na temat procesora
* **lsblk** - szczegółowe informacje na temat urządzeń blokowych
* **lspci** - urządzenia podpięte pod szynę PCI
* **watch "dmesg | tail -20"** - podgląd na żywo logów systemowych
* **hciconfig -a** - Bluetooth info
* **awk '/sd/ {print $3"\t"$10 / 2 / 1024}' /proc/diskstats** - pokazuje statystyki dysków zaczynających sie na `sd`
* **udevadm info -q all -n /dev/sda1** - szczegółowe informacje na temat dysku
* **udevadm monitor** - monitoruje urządzenia
* **lsinput** - lista urządzeń wejściowych
* **lshal** - informacja o urządzeniach HAL
* **lpq** - status podłączonych drukarek i drukowania
* **lpq -P {nazwa drukarki} {nazwa pliku}** - drukuje plik na wskazanej drukarce
  * **lpstat** - j/w (-d domyślna drukarka)
* **lp {plik}** - dodaje plik do kolejki drukowania
* **lprm {id}** - usuwa z kolejki do druku
* **sudo service bluetooth status** - status bluetooth na maszynie
* **hciconfig scan** - skanuje bluetooth
* **hcitool dev** - informacje o urządzeniu bluetooth na maszynie
* **hcitool inq** - znalezione i nie podłączone urządzenia
* **sudo hcitool info {mac}** - informacje o urządzeniu
* **hcitool con** - podłączone urządzenia bluetooth
* **inxi -Fxz** - szczegółowe/iso informacje na temat systemu
* **file -sL {/dev/...}** - zwraca szczegółowe informacje o dysku (np typ plików)
* **sudo parted -l** - jw, ale dla wszystkich dysków
* **sudo lsblk -f** - jw
* **rfkill** - zarządzanie urządzeniami radiowymi
  * **list** - lista urządzeń
  * **-J** - output jako json
* **sudo hdparm -r 0 /dev/{device}** - usuwa flagę tylko do odczytu
* **sudo dd if=/dev/zero of=/dev/{device} status=progress** - niszczy dane na nośniku
* **sudo dd if=/dev/zero of={plik}.img bs=1M count=1200 status=progress** - tworzy pusty obraz (1.2GB) (*.iso*, *.image*, */dev/urandom*)
  * **dd if=/dev/mem | hexdump -C | less** - przeglądanie pamięci
  * **dcfldd** - bardziej rozbudowana wersja **dd** [github](https://github.com/adulau/dcfldd)
* **sudo mkfs -t ext4 {name}.img** - tworzy partycję na podanym obrazie
* **fdisk -l** - wyświetlenie partycji i informacji na ich temat
* **lsblk -o NAME,FSTYPE,MOUNTPOINT,PARTLABEL,SIZE,RO** - lista urządzeń blokowych z listą informacji (nazwa, system plików, punkt montowania, label, rozmiar, tylko do odczytu)
* **sudo fdisk /dev/sdg** - operacje na dysku

### Procesy
* **pstree** - pokazuje drzewo procesów
* **kill -9 {numer procesu}** - zabija wskazany proces (kill)
* **kill -3 {numer procesu}** - zakończenie procesu (quit)
* **kill -15 {numer procesu}** - zakończenie procesu (terminate)
* **killall {nazwa}** - zabija wszystkie procesy o podanej nazwie
* **pidof {nazwa}** - znajduje id podanego procesu
* **sudo update-rc.d {skrypt w init.d} defaults** - dodaje skrypt do autostartu na domyślnym poziomie
* **sudo lsof** - pokazuje otwarte pliki i procesy ich używające
* **ps -Af --no-headers | wc -l** - liczba uruchomionych procesów
* **ps u** - pokazuje polecenia uruchomione w konsolach
* **ps aux | head -1 && ps aux | grep {program}** - szuka podanego programu, wraz z opisem kolumn z wyniku
* **ps aux | grep {nazwa}** - szuka procesów o podanym wyrażeniu
* **ps -LF -u {nazwa użytkownika}** - pokazuje procesy użytkownika
* **ps -p {id} -o %cpu,%mem** - pokazuje zużycie ramu i procesora dla podanego procesu
  * **watch -n 1 -d "ps -p {id} -o %cpu,%mem"**
* **nohup ./{nazwa}.sh > {nazwa}.log &** - uruchamia proces ze skryptu z zapisem do logu, tak aby działał dalej po wyjściu z konsoli
* **bg** - pokazuje procesy w tle
  * **fg {nazwa}** - przywraca podany proces
* **fg {proces}** - przenosi podany proces w tło
* **pidof {program}** - zwraca id procesu o podanej nazwie
* **watch -n 1 -d "{polecenie}"** - uruchamia polecenie i obserwuje wynik co sekundę z zaznaczeniem zmian
* **iotop** - pokazuje operacje i/o
* **jobs** - pokazuje polecenia uruchomione w terminalu i przeniesione w tło
* **kill %1** - zabija proces w tle
* **fg %2** - przywraca proces z tła (bez % ostatni)
* **bg** - przenosi proces w tło, po zatrzymaniu przez `CTRL-Z`
* **top -U {user} -o cpu -p {id}** - statystyki zużycia procesu przez użytkownika o id (na żywo)
* **pgrep {program}** - zwraca id procesu dla podanej nazwy programu
* **renice {numer} {pid}** - zmienia priorytet procesu o podanym id, im niższy tym wyższy priorytet (np -19, default: 0)
* **pkill {program}** - zabija podany program
  * **-p $(pgrep {program})** - dla podanego programu
* **xkill** - umożliwia zabijanie okienkowych procesów, należy wskazać kursorem proces do zabicia
* **pidstat -hruvp {pid1}{pid2} 5** - monitoruje 2 procesy i pokazuje co 5s
* **pmap -x {proces id}** - podaje szczegółowe informacje na temat zużycia pamięci przez proces i jego zależności
* **ps --no-headers -o "rss,cmd" -C {nazwa procesu} | awk '{ sum+=$1 } END { printf ("%d%s\n", sum/NR/1024,"Mb") }'** - sumaryczne zużycie pamięci dla procesu

---

## Internet i sieć
* **curl ifconfig.me** - pokazuje zewnętrzne ip komputera
* **wget http://ipinfo.io/ip -qO -** - pokazuje zewnętrzne ip komputera
* **ifconfig** - pokazuje ustawienia sieci
* **ifconfig {nazwa sieci}** - pokazuje informacje o wskazanej sieci
* **ifconfig {nazwa sieci} | grep 'inet addr:' | cut -d: -f2 | awk '{ print $1}'** - zwraca adres lokalny komputera
* **ifconfig {nazwa sieci} {down|up}** - wyłącza lub włącza sieć
* **ssh-keygen** - generuje nowy klucz ssh
* **mtr {ip lub domena}** - połączenie ping i traceroute
* **scp -r {źródło} {cel}** - kopiuje plik poprzez ssh (user@192.128.0.1:/some/path/file) razem z podkatalogami
  * **-l {val}** - limit transferu w Kbit/s (8000 -> 1MB)
* **curl ipinfo.io** - bardziej szczegółowe informacje o komputerze
* **netstat** - wyświetla listę aktywnych połączeń TCP i UDP
* **netstat -at** - lista portów TCP
* **netstat -nr | awk '{ if ($1 ~/default/) { print $6} }'** - nazwa aktualnie używanej sieci
* **netstat -anp tcp | grep -i "listen"** - pokazuje nasłuchiwane połączenia tcp
* **netstat -nr** - pokazuje tablicę routingu
* **netstat -tuln** - lista otwartych połączeń
 * **netstat -tulnp tcp** - tylko tcp
* **sudo lsof -i -P -n | grep LISTEN** - lista otwartych portów i procesów ich nasłuchujących
* **sudo lsof -i -nP** - pokazuje używane porty oraz powiązane z nimi procesy
* **iostat -d /dev/sda | sed -n "4p"** - pokazuje zużycie sieci
* **ip route get 8.8.8.8 | awk '{print $NF; exit}'** - pokazuje ip komputera wewnątrz sieci
* **hostname --ip-address** - lokalne ip komputera
* **nmap -sn {ip}/24** - skanuje adresy w poszukiwaniu hostów (/24,16,8 - mask adresu np /16 -> 127.0.x.x, 24 -> 127.0.0.x)
* **nmap {hostname}** - pokazuje otwarte porty i serwisy na podanym hoście
  * **nmap -p 1-1000 {hostname}** - pokazuje otwarte porty i serwisy na podanym hoście od 1 do 1000
  * **nmap -sn 192.168.1.0/24** - skanuje w poszukiwaniu działających ip
  * **arp-scan 192.168.0.0/24** - j/w
* **hostname -I** - pokazuje lokalne ip komputera
* **hostname -f** - nazwa hosta (długa, -s - krótka)
* **cat /etc/services | less** - podgląd wszystkich portów i programów je wykorzystujących
* **tcpdump -i any port {port}** - pokazuje całą komunikację na wskazanym porcie
  * **tcpdump -i {nazwa sieci}** - pokazuje całą komunikację na wskazanej sieci (np z ifconfig)
* **curl -D {plik dla nagłówków} {url} > /dev/null** - zapisuje nagłówki do pliku (nie generuje outputu)
* **curl -o- {url}.sh | bash** - pobiera skrypt i wykonuje go na komputerze
* **curl -si -H "Host: host.com" "http://server-name/"** - testowanie serwera za load balancerem (host - główna strona, server-name - docelowy serwer za load balancerem)
* **curl -sS -d key=val {url}** - wysyła dane POST, -sS pokazuje tylko błędy jeśli krytyczne (-G - GET)
  * **-d {key=val}** - post data
  * **-G {key=val}** - get data
  * **-H {Name: value}** - header data
  * **-i** - pokazuje headery w output
  * **-I** - pokazuje tylko headery w output
  * **-L** - podążaj za relokacją ze strony
  * **-N** - bez bufora, pokazuje na bieżąco dane przychodzące
  * **-o** - zapisuje output do pliku (-o- - force)
  * **-O | --remote-name** - nazywa plik tak samo jak ten z serwera
  * **-s** - nie pokazuje błędów i progresu
  * **-S** - pokazuje błędy
  * **-sS** - pokazuje błędy tylko jeśli zapytanie się nie udało
  * **-v --ipv4 -I** - pokazuje krok po kroku co dzieje się z zapytaniem
  * **-k** - pozwala na połączenie jeśli są problemy z certyfikatem ssl
* **for ((i=0;i<=30;i++)); do curl -I "{url}/$i"; done;** - odpala 30 url-i z dopiskiem numeru na końcu i zwraca tylko headery
* **whois {domena}** - podaje informacje o domenie internetowej
* **dig {domena}** - informacje o DNS
* **wget -r {url}** - pobiera rekursywnie z podanego url-a
  * **-O {plik}** - zapisuje output do pliku
  * **-c** - wznawia ściąganie częściowo ściągniętego pliku
* **lsof -n -i:80 | grep LISTEN** - lista programów nasłuchujących port 80 (-iTCP:80 - tylko TCP)
* **dsh -vMm {serwer, ...} -c {polecenie}** - uruchamia polecenia na zewnętrznych maszynach
* **if=`netstat -nr | awk '{ if ($1 ~/default/) { print $6} }'` && ifconfig ${if}** - szczegóły aktualnie używanej sieci
* **if=`netstat -nr | awk '{ if ($1 ~/default/) { print $6} }'` && ifconfig ${if} | awk '{ if ($1 ~/inet$/) { print $2} }'** - zwraca ip
* **ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'** - j/w
* __ifconfig | sed -En 's/127.0.0.1//;s/.*inet (addr:)?(([0-9]*\.){3}[0-9]*).*/\2/p'__ - j/w
* __ifconfig | grep -Eo 'inet (addr:)?([0-9]*\.){3}[0-9]*' | grep -Eo '([0-9]*\.){3}[0-9]*' | grep -v '127.0.0.1'__ - j/w
* **nc {ip} {port}** - ustawa połączenie tcp & udp z serwerem
* **nc -z -v 127.0.0.1 1-1000** - skanowanie portów od 1 do 1000
  * **nc -z 127.0.0.1 1-100** - tylko te z którymi udało się połączyć
  * **nc -z -n -v 127.0.0.1 1-1000 2>&1 | grep succeeded** - j/w
* **nc -l {port}** - nasłuchuje na porcie
  * **nc -l {port}| tar xzvf -** - zapisuje output do spakowanego pliku
  * **tar -czf - * | nc {ip} {port}** - wysyła spakowane pliki przez netcata
* **while true; do printf 'HTTP/1.1 200 OK\n\n%s' "$(cat index.html)" | nc -l {port}; done** - tworzy prosty serwer pokazujący plik index.html (while - żeby działało cały czas)
* **iptables -I INPUT -p tcp -m tcp --dport {port} -j ACCEPT** - dodaje port do wpisu w+ iptables
* **iptables -A INPUT -s {ip} -j DROP** - blokuje adres IP
* **iptables -D INPUT -s {ip} -j DROP** - usuwa z listy blokowanych
* **iptables --list** - lista wpisów
* **iptables-save** - zapisuje ustawienia
* **iptables -A INPUT -s {ip} -p tcp --destination-port {port} -j DROP** - blokuje wskazany port

---

## Inne
* **reset** - inicjalizuje ponownie konsolę
* **clear** - czyści wpisy w konsoli
* **which {nazwa polecenia}** - podaje ścieżkę do polecenia
* **whereis {nazwa polecenia}** - podaje ścieżkę do polecenia, źródeł binarnych, bibliotek i pakietów
* **alias fuck='sudo $(history -p \!\!)'** - fuck command
* **alias fuck='sudo $(fc -ln -1)'** - fuck command 2
* **history** - pokazuje historię poleceń konsoli
* **{polecenie} | column -t** - formatuje wynik polecenia w kolumny
* **expr {wyrażenie matematyczne}** - rozwiązuje wyrażenia matematyczne
  * **expr {arg1} = {arg2}** - czy oba argumenty są identyczne (|, &, \<, \>, <=, >=, \!=, +, -, *, /, %)
  * **expr {string} : {regexp}** - sprawdza czy string pasuje do wzorca
  * **expr substr {string} {start} {długoś}** - wycina cześć stringa
  * **expr index {string} {chars}** - zwraca index znaku w stringu
  * **expr length {string}** - długość tekstu
* **disown -a && exit {polecenie}** - uruchamia polecenie w tle nawet po zamknięciu konsoli
* **bind -p** - pokazuje skróty dostępne w bash
* **time {polecenie}** - mierzy czas podanego polecenia
* **while sleep 1;do tput sc;tput cup 0 $(($(tput cols)-29));date;tput rc;done &** - pokazuje datę i czas w prawym górnym rogu terminala
* **{polecenie} | tee {plik}** - wyświetla wyjście oraz zapisuje do pliku wyjście polecenia
* **tail -f jakaś/ścieżka/\`ls --format=single-column -t /jakaś/ścieżka | head -1\`** - uruchamia taila na najnowszym pliku w podanym katalogu
* **{polecenie} | awk '{ print $1 }** - wyświetla pierwszy wyraz z wyniku polecenia
* **{polecenie} | tr '\ ' '\n'** - wyświetla wynik polecenia w pojedynczych liniach (zamienia spacje na nowy wiersz)
  * **echo $PATH | tr ':' '\n'** - pokazuje ścieżki ze zmiennej `$PATH` w osobnych liniach
* **{polecenie} | xargs -n1** - działa jak powyżej
* **xxd -c 1 {plik}** - wyświetla wartości hex dla każdego znaku linia po linii
* **rsync -vrpogthl --progress {katalog} {katalog backupu}** - wykonuje backup katalogu, aktualizuje jedynie to co się zmieniło (opcje zapewniają backup praw, własności, czasów modyfikacji plików i symlinki jako symlinki)
  * **-e 'ssh -p {port}'** - połączenie na innym niż domyślny port
  * **-L** - Podąża za symlinkami
  * **-a** - pomija symlinki
  * **-z** - kompresuje pliki do przesyłania
  * **-P** - --progress and --partial
  * **-v** - zwiększa ilość informacji
  * **-r** - rekursywnie
  * **-p** - zachowuje uprawnienia
  * **-o** - zachowuje właściciela
  * **-g** - zachowuje grupę
  * **-t** - zachowuje czas modyfikacji
  * **-h** - numery w formacie łatwym do odczytania
  * **-l** - kopiuje symlinki jako symlinki
  * **-q** - nie pokazuje wiadomości (tylko błędy)
  * **--del** - (--delete-during) operacja usuwania podczas synchronizacji
  * **--delete-after** - operacja usuwania po synchronizacji
  * **--log-file** - zapisuje informacje do loga
* **for i in {1..10}; do {polecenie}; done** - uruchamia {polecenie} 10x
* **look {wyraz}** - podpowiada składnię wyrazu
* **md5sum {plik}** - oblicza skrót md5
* **mkpasswd -l 10** - generuje hasło 10 znakowe trudne do złamania
* **makepasswd --cahrs 10** - generuje hasło 10 znakowe trudne do złamania
* **while true; do {polecenie lub skrypt do wykonania} ; sleep 100 ; done** - wykonuje skrypt co określony czas
* **mplayer -vo caca {film}** - odtwarzanie filmów w konsoli
* **multitail -l 'ssh user@host "tail -f /var/log/log.log"' -l 'ssh user@host2 "tail -f /var/log/log.log"'** - podgląd logów z 2 serwerów na raz
* **echo -n "Hello" | od -A n -t x1** - wyświetla tekst jako wartości hex
* **echo -n "Hello" | hd** - wyświetla tekst jako wartości hex + oryginalny string (hd, xxd)
* **od {opcja} {plik}** - wyświetla zawartość pliku jako wartości danego znaku
  * **-c** - pokazuje znaki niedrukowane
  * **-x** - jako wartości hex
  * **-b** - jako wartości octalne
* **history | cut -c 8- | grep -Eo "^{name}.*" | sort --uniq** - wyszukuje w historii unikalne komendy zaczynające się od wyrażenia
* **history | cut -c 8- | grep -v "^{name}\|{name2}.*" | sort --uniq** - wyszukuje w historii unikalne komendy nie zaczynające się od wyrażenia name lub name2
* **history | awk '{ $1=""; print }'** - wyświetla tylko komendy z historii (print substr($0,2) - bez spacji na początku)
* **history | fc -ln** - j/w
* **for ((i=32;i<=127;i++)); do printf '%03o\t' "$i"; done;echo "\n""** - liczby od 32-127 przedstawione w notacji ósemkowej
* **mysqldump --host -u  -p --no-create-info --single-transaction -d {db_name} > sb.sql** - eksport danych bez info
  * **--no-create-db --where="date_time>'2015-08-01'"**
  * **mysqluc -e "help utilities"**
* **tig** **grv** - wizualna reprezentacja git-a
* **curl -N tty.zone/\[0-2\]\?auto\&cols=$((COLUMNS))** - demo narzędzi graficznych pod terminal
* **wget -O- -q wttr.in/Poland+{miasto}** - pogoda
* **type {alias}** - pokazuje komendę powiązaną z podanym aliasem
* **alias {alias}** - j/w
* **xfd -fa "{czcionka}"** - pokazuje pełną listę znaków dla podanej czcionki
* **sudo sshfs -p {port} -o allow_other,IdentityFile={id_rsa} {domena}:{katalog} /mnt/{punk montowania}** - montuje jako dysk katalog na zewnętrznym serwerze

---

## Stream
* **awk '{print $4/$2}'** - wyświetla parameter 4 i 2 z wejścia podzielone przez siebie (oddzielone spacja) ($NF - liczba pol, $NR - liczba rekordów)
* **awk 'BEGIN{for(i=32;i<128;i++)printf "%c",i}'** - wyświetla listę znaków (32-128)
* **awk -F'/' '{print $3}'** - dzieli input względem znaku `/` i wyświetla 3 element
* **awk '{s+=$1} END {print s}** - 
* **awk '{gsub(/{pattern}/,"{replace}")}' {plik}** - Szuka i zastępuje wszystkie wzorce znalezione w pliku
  * **awk '{gsub(/[0-9]+\.216\.104\.10/,"10.216.104.1")}' php.ini**
* **sed -n '44,92p' {plik}** - pokazuje linie 44-92 z pliku (samo 92p - 92 linia)
* **sed -i --'s/{pattern}/{replace}/g' {plik}** - zastępuje znaleziony wzorzec w pliku
* **... | cut -d' ' -f1** - tnie po spacji i podaje pierwszy element (-f 2- - od 2 i dalej; -2 do 2)
* **sed -i '1s/^/{string}\n/' {plik}** - dodaje string na początek pliku

---

## Terminal
* **{command} 1> {file|program}** - Redirect stdout to file or program
* **{command} 1>> {file|program}** - Redirect and append stdout to file or program
* **{command} 2> {file|program}** - Redirect stderr to file or program
* **{command} 2>> {file|program}** - Redirect and append stderr to file or program (to samo co {command} >> {plik} 2>&1)
* **{command} &> {file|program}** - Redirect both stdout and stderr to file or program
* **{command} 2>&1** - Redirect stderr to stdout
* **{command} > /dev/null 2>&1** - Redirect whole output to /dev/null
* **{command} &> /dev/null** - Redirect whole output to /dev/null
* **echo -n "{tekst}"** - wyświetla tekst bez znaku nowej linii
* **echo "\u9c93"** - 鲓
* **echo "\ue0b0 \u00b1 \ue0a0 \u27a6 \u2718 \u26a1 \u2699"** -  ±  ➦ ✘ ⚡ ⚙
* **echo "\xe2\x9c\x93"** - ✓
* **tty** - zwraca nazwę terminala
* **ttylog -d /dev/pts/{numer}** - wyświetla informacje z innego terminala
* **script -f /dev/tty3** - zapisuje wszystkie informacje z terminala (np zapis wszystkich działań w terminalu) do innego terminala, lub pliku (-f {plik})
* **retty $({id procesu})** - przełącza proces z innego terminala na obecny
* **... | bash -s {param}** - uruchamia skrypt w bash i podaje na standardowe wejście parametr
* **fc** - history manager
* **echo $(( 2#101011 ))** - zamienia binary na decimal (101011 -> 43)
* **echo "obase=2; {liczba}" | bc** - decimal do binary
* **echo "obase=8; {liczba}" | bc** - decimal do octal
  * **printf "%on" {octal}**
* **echo "obase=16; {liczba}" | bc** - decimal do hex
  * **printf "%xn" {liczba}**
* **echo "ibase=2; {liczba}" | bc** - binary do decimal
* **echo "ibase=8; {liczba}" | bc** - octal do decimal
* **echo "ibase=16; {liczba}" | bc** - hex do decimal
  * **echo $((0x{hex}))**
* **echo "ibase=2;obase=8; {liczba}" | bc** - binary do octal

---

## Skróty
* **ctrl+c** - zatrzymuje aktualny proces
* **ctrl+z** - przenosi proces w tło
* **ctrl+u** - kasuje wpisaną linię
* **ctrl+w** - kasuje jedno słowo z linii
* **ctrl+r** - szuka poprzedniej komendy
* **ctrl+d** - wylogowuje z obecnej sesji
* **!!** - powtarza poprzednie polecenie