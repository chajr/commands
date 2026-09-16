# Przydatne polecenia kubectl

### Informacje o klastrze

* **kubectl cluster-info** - adresy master node i usług klastra
* **kubectl version --client** - wersja samego klienta, bez łączenia się z klastrem
* **kubectl config view** - aktualny kubeconfig: klastry, konteksty, użytkownicy
* **kubectl config get-contexts** - lista kontekstów z zaznaczonym aktualnym
* **kubectl config use-context {kontekst}** - przełącza kontekst bez `kubectx`
* **kubectl explain {zasób}** - opis pól zasobu wprost z API klastra (`kubectl explain job`)
  * **--recursive** - całe drzewo pól, nie tylko pierwszy poziom
  * ścieżkę pola podaje się kropkami: `kubectl explain job.spec.template.spec.containers.envFrom`
* **kubectl top pod -A** - zużycie CPU i pamięci przez pody ze wszystkich namespace
* **kubectl top node** - zużycie zasobów per node
* **kubectl proxy** - lokalne proxy do API klastra (domyślnie `127.0.0.1:8001`)
* **kubectl get all** - wszystkie podstawowe zasoby w namespace naraz (pody, usługi, deploymenty, replicasety)
* **kubectl api-resources** - lista typów zasobów w klastrze wraz ze skrótami (`po`, `deploy`, `ing`)
* **kubectl completion zsh > "${fpath[1]}/_kubectl"** - podpowiadanie poleceń w zsh (dla bash: `kubectl completion bash`)

### Konteksty i namespace

* **kubectl get ns** - lista namespace
* **kubectx** - przełącza kontekst, czyli klaster (bez argumentu - lista kontekstów)
* **kubens** - przełącza domyślny namespace w aktualnym kontekście
* **-n {namespace}** - działa dla większości poleceń, wymusza namespace jednorazowo
* **-A** / **--all-namespaces** - zamiast jednego namespace bierze wszystkie

### Pody

* **kubectl get pods** - lista podów w aktualnym namespace
  * **-o wide** - dodatkowo node i IP poda
  * **-w** - zostaje i pokazuje zmiany na bieżąco (przydatne przy deployu)
  * **--no-headers -o custom-columns=":metadata.name"** - tylko nazwy, bez nagłówka (do podstawiania w skryptach)
* **kubectl get pods --no-headers -o custom-columns=":metadata.name" | grep {fragment} | head -1** - nazwa pierwszego poda pasującego do fragmentu nazwy
* **kubectl get pods --show-labels** - pody wraz z ich labelkami
* **kubectl get pods --selector {label}={wartość}** - tylko pody z daną labelką (`-l` to skrót)
* **kubectl get pod {pod} -o jsonpath="{.spec.containers[*].image}"** - obrazy, na których faktycznie stoi pod
* **kubectl get pod {pod} -o go-template="{{range .status.containerStatuses}}{{.lastState.terminated.message}}{{end}}"** - komunikat, z którym kontener padł (np. przy OOMKilled)
* **kubectl exec {pod} -- printenv | grep {fragment}** - zmienne środowiskowe widziane przez kontener
* **kubectl attach {pod}** - podpina się do głównego procesu poda (nie odpala nowej powłoki, jak `exec`)
* **kubectl describe pod {pod}** - szczegóły poda: obrazy, eventy, powód restartu
* **kubectl delete pod {pod}** - kasuje poda (deployment odtworzy go od nowa)
  * **--force** - gdy pod wisi w `Terminating` i nie chce zniknąć
* **kubectl get pod | grep Evicted | awk '{print $1}' | xargs kubectl delete pod** - sprząta pody wyrzucone przez brak zasobów na node

### Logi

* **kubectl logs {pod}** - logi poda
  * **-f** - śledzi logi na bieżąco (jak `tail -f`)
  * **--previous** - logi poprzedniej instancji, po restarcie/crashu
  * **--since=5m** - tylko ostatnie 5 minut (`10s`, `2h`)
  * **{container}** - nazwa kontenera jako drugi argument, gdy pod ma ich kilka
* **kubectl logs -l {label}={wartość}** - logi ze wszystkich podów pasujących do labelki, bez szukania nazw
  * `kubectl logs -n kube-system -l app.kubernetes.io/name=karpenter --since=5m`

### Wejście do kontenera i przekierowanie portów

* **kubectl exec -it {pod} -- bash** - wchodzi do kontenera (`sh`, jeśli obraz nie ma basha)
* **kubectl exec {pod} -- {polecenie}** - jednorazowe polecenie w kontenerze, bez interaktywnej konsoli
* **kubectl exec -it {pod} --container {kontener} -- {polecenie}** - polecenie we wskazanym kontenerze poda
* **kubectl port-forward pods/{pod} {port lokalny}:{port poda}** - tunel z localhosta do poda
  * **-n {namespace}** - pod z innego namespace (np. dashboard w `kube-system`)
* **kubectl cp {pod}:{ścieżka w kontenerze} {ścieżka lokalna}** - kopiuje plik z kontenera
* **kubectl cp {ścieżka lokalna} {pod}:{ścieżka w kontenerze}** - kopiuje plik do kontenera
  * **-r** - kopiuje cały katalog
  * **--no-preserve=true** - nie przenosi uprawnień i właściciela pliku (gdy docelowy kontener ma innego użytkownika)
* **kubectl run {nazwa} --image=busybox:1.28 --rm -it --restart=Never -- sh** - jednorazowy pod do debugowania, kasuje się po wyjściu
* **kubectl run {nazwa} --image=busybox:1.28 --rm -it --restart=Never --command -- nslookup {usługa}** - sprawdza DNS klastra z wnętrza sieci podów
* **kubectl run {nazwa} --image=busybox:1.28 --rm -it --restart=Never --command -- wget -qO- http://{usługa}:{port}** - odpytuje usługę spod poda, z pominięciem ingressa

### Deploymenty i skalowanie

* **kubectl get deployments** - lista deploymentów
* **kubectl apply -f {plik.yaml}** - tworzy lub aktualizuje zasoby z pliku
  * **--validate=false** - pomija walidację schematu, gdy CRD nie ma jeszcze w klastrze
* **kubectl delete -f {plik.yaml}** - kasuje zasoby zdefiniowane w pliku
* **cat <<EOF | kubectl apply -f -** - manifest wprost z terminala, bez tworzenia pliku (`EOF` zamyka)
* **kubectl rollout restart deployment/{deployment}** - restartuje pody deploymentu bez zmiany manifestu
* **kubectl scale deployment {deployment} --replicas={liczba}** - zmienia liczbę replik (`0` wyłącza usługę, nie kasując deploymentu)
* **kubectl get hpa** - autoscalery: aktualne i docelowe obciążenie, min/max replik
* **kubectl set image deployment/{deployment} {kontener}={obraz}:{tag}** - podmienia obraz bez edycji manifestu
* **kubectl set env deployment {deployment} DEPLOY_DATE="$(date)"** - zmiana zmiennej wymusza nowy rollout (sposób na restart sprzed `rollout restart`)
* **kubectl edit deployment {deployment}** - edycja manifestu wprost w edytorze, zmiany wchodzą po zapisaniu
* **kubectl delete all --selector {label}={wartość}** - kasuje wszystkie zasoby z daną labelką
* **kubectl get job {job} -o json | jq 'del(.spec.selector)' | jq 'del(.spec.template.metadata.labels)' | kubectl replace --force -f -** - odpala joba jeszcze raz (bez usunięcia wygenerowanych selektorów `replace` odbija się o niezmienne pola)
* **kubectl apply -f {url}** - manifest wprost z sieci, bez pobierania pliku
* **kubectl convert -f {plik.yaml} --output-version {grupa}/{wersja}** - przepisuje manifest na nowszą wersję API

### Nody, sieć i dyski

* **kubectl get nodes -o wide** - nody z wersją kubeleta, systemem i IP
* **kubectl describe node {node}** - zasoby, warunki i pody działające na node
* **kubectl get node --selector='!node-role.kubernetes.io/master'** - tylko nody robocze, bez control plane
* **kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="ExternalIP")].address}'** - publiczne adresy nodów (np. do whitelisty)
* **kubectl drain {node}** - przenosi pody na inne nody i blokuje nowe (przed pracami na node)
  * **--ignore-daemonsets** - bez tego przerywa, bo daemonsetów nie da się wyprowadzić
* **kubectl uncordon {node}** - odblokowuje node po `drain`
* **kubectl taint node {node} {klucz}:NoSchedule-** - zdejmuje taint (końcowy minus oznacza usunięcie)
* **kubectl get sc** - klasy dyskowe dostępne w klastrze
* **kubectl get endpoints** - pody faktycznie stojące za usługą (pusta lista = selektor usługi nie łapie podów)
* **kubectl get services** - usługi z ich typem, cluster IP i portami
* **kubectl get ing** - ingressy: hosty, ścieżki, adresy
* **kubectl describe ingress {ingress}** - reguły ingressa i backendy, z eventami kontrolera
* **kubectl get pv** / **kubectl get pvc** - wolumeny i roszczenia do wolumenów
* **kubectl describe pvc {pvc}** - dlaczego PVC wisi w `Pending`

### Eventy

* **kubectl get events -A** - eventy ze wszystkich namespace
* **kubectl get events -A --sort-by='.metadata.creationTimestamp'** - to samo, chronologicznie (domyślna kolejność jest losowa)

### Sekrety i konfiguracja

* **kubectl get secret** - lista sekretów
* **kubectl get secret {secret} -o jsonpath='{.data}'** - zawartość sekretu (wartości zakodowane base64)
* **kubectl get secret {secret} -o jsonpath='{.data.{klucz}}' | base64 -d** - odkodowana wartość jednego klucza
* **kubectl get secret {secret} -o jsonpath='{.data}' | jq .{klucz}** - wyciąga jeden klucz, gdy w nazwie są kropki i `jsonpath` się gubi
* **kubectl create secret tls {nazwa} --key {klucz.key} --cert {certyfikat.crt}** - sekret z certyfikatem pod ingressa
* **kubectl get configmap {configmap} -o yaml** - zawartość configmapy (w `-o yaml` wartości są czytelne, nie base64 jak w sekretach)
* **kubectl get crd** - definicje własnych zasobów (cert-manager, elastic operator)
* **kubectl get resourcequotas -n {namespace}** - limity zasobów nałożone na namespace
* **kubectl create namespace {namespace}** - nowy namespace (np. pod cert-managera)

### Przykłady użycia

* **kubectl exec {pod} -- mysql -u {user} -p{hasło} {baza} <<< "{polecenie sql}"** - podaje polecenie SQL wprost do kontenera z bazą
* **kubectl exec -i {pod} -- mysql -u {user} -p{hasło} -h {host bazy} {baza} < dump.sql** - wgrywa dumpa do bazy w klastrze (`-i` bez `-t`, inaczej stdin nie przejdzie)
* **kubectl exec {pod} -- mysqldump --no-create-info -u {user} -p{hasło} {baza} > dump.sql** - zrzut samych danych, bez `CREATE TABLE`
* **kubectl exec {pod} -- mysqldump --skip-comments --no-create-info -u {user} -p{hasło} -h {host bazy} {baza} {tabela} > dump.sql** - zrzut jednej tabeli, bez komentarzy
* **kubectl exec -it $(kubectl get pods --no-headers -o custom-columns=":metadata.name" | grep redis) -- redis-cli FLUSHALL** - czyści cache Redisa bez szukania nazwy poda
* **kubectl exec -it $(kubectl get pods --no-headers -o custom-columns=":metadata.name" | grep rabbit) -- watch -n 5 -d 'rabbitmqctl list_queues'** - podgląd kolejek RabbitMQ odświeżany co 5 sekund

### Karpenter

* **kubectl get nodeclaim {nodeclaim} -w** - śledzi, czy Karpenter wystawił node pod czekającego poda
* **kubectl logs -n kube-system -l app.kubernetes.io/name=karpenter --since=5m** - logi kontrolera, gdy node nie chce się pojawić
