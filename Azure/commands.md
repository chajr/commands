# Przydatne polecenia Azure CLI

### Logowanie i subskrypcje

* **az login** - logowanie przez przeglądarkę
  * **--use-device-code** - gdy nie ma przeglądarki (serwer, kontener)
* **az account show** - aktualna subskrypcja i tenant
  * **--query id -o tsv** - samo ID subskrypcji do podstawienia w skrypcie
  * **--query tenantId -o tsv** - samo ID tenanta
* **az account list -o table** - lista dostępnych subskrypcji
* **az account set --subscription {id lub nazwa}** - przełącza aktywną subskrypcję
* **az account list-locations -o table** - lista regionów
* **az group show --name {grupa} --query "{name:name,location:location}" -o table** - region grupy zasobów
* **az config set extension.use_dynamic_install=yes_without_prompt** - doinstalowuje brakujące rozszerzenia bez pytania

### Formatowanie wyniku

* **-o table** - czytelna tabelka
* **-o tsv** - goła wartość, do podstawienia w `$(...)`
* **-o jsonc** - JSON z kolorowaniem
* **-o yaml** - YAML, wygodny przy zagnieżdżonych strukturach
* **-o none** - nic nie wypisuje (gdy wynik zawiera sekret)
* **--query "{a:pole.jedno, b:pole.drugie}"** - JMESPath, wybiera i zmienia nazwy pól
* **--query "[?name=='{nazwa}']"** - filtr po polu z listy
* **--query "[?contains(displayName, '{fragment}')]"** - filtr po fragmencie nazwy
* **--query "[0]"** - pierwszy element listy (`| [0]` po filtrze)
* **--ids {resource id}** - większość poleceń `show` przyjmuje pełne ID zamiast `-g` i `-n`

### Container Apps

* **az containerapp show -g {grupa} -n {aplikacja}** - pełna definicja aplikacji
  * **--query "properties.configuration.ingress.fqdn" -o tsv** - publiczny adres aplikacji
  * **--query "properties.template.containers[0].env" -o table** - zmienne środowiskowe
  * **--query "properties.configuration.secrets[].name" -o tsv** - nazwy sekretów (bez wartości)
  * **--query identity.principalId -o tsv** - ID tożsamości zarządzanej
  * **--query "properties.latestRevisionName" -o tsv** - nazwa ostatniej rewizji
* **az containerapp update -g {grupa} -n {aplikacja} --image {rejestr}.azurecr.io/{obraz}:{tag}** - wdraża nowy obraz
* **az containerapp update -g {grupa} -n {aplikacja} --set-env-vars {KLUCZ}={wartość}** - dopisuje lub nadpisuje pojedyncze zmienne
  * **--replace-env-vars** - podmienia cały zestaw zmiennych, reszta znika
  * wartość `secretref:{nazwa sekretu}` podstawia sekret aplikacji zamiast jawnego tekstu
* **az containerapp secret set -g {grupa} -n {aplikacja} --secrets {nazwa}={wartość}** - ustawia sekret aplikacji
* **az containerapp identity assign -g {grupa} -n {aplikacja} --system-assigned** - włącza tożsamość zarządzaną
* **az containerapp logs show -g {grupa} -n {aplikacja} --tail 200** - ostatnie logi aplikacji
  * **--follow** - śledzi na bieżąco
* **az containerapp exec -g {grupa} -n {aplikacja} --command "/bin/sh"** - wchodzi do kontenera
  * **--container {kontener}** - gdy pod ma kilka kontenerów
  * `--command "wget -qO- http://localhost:{port}/health"` - jednorazowy strzał do healthchecku od środka
* **az containerapp revision list -g {grupa} -n {aplikacja} -o table** - lista rewizji, od najnowszej

### Container Apps - ingress i środowisko

* **az containerapp ingress enable -g {grupa} -n {aplikacja} --type internal --target-port {port}** - włącza ingress dostępny tylko w środowisku
* **az containerapp ingress update -g {grupa} -n {aplikacja} --type external --target-port {port}** - wystawia na świat i zmienia port
* **az containerapp env show -g {grupa} -n {środowisko} --query "{internal:properties.vnetConfiguration.internal,defaultDomain:properties.defaultDomain,staticIp:properties.staticIp}" -o yaml** - czy środowisko jest wewnętrzne, jego domena i IP
* **az containerapp env show -g {grupa} -n {środowisko} --query "properties.appLogsConfiguration.logAnalyticsConfiguration.customerId" -o tsv** - ID workspace Log Analytics, potrzebne do zapytań o logi

### Container Apps Jobs

* **az containerapp job create -g {grupa} -n {job} --environment {środowisko} --trigger-type Manual --image {obraz} --cpu 0.5 --memory 1Gi --command {polecenie} --args {argumenty}** - tworzy joba uruchamianego ręcznie
  * **--replica-timeout 1800** - limit czasu na jedno uruchomienie (sekundy)
  * **--replica-retry-limit 0** - bez ponawiania po błędzie
  * **--parallelism 1 --replica-completion-count 1** - jedna replika, jedno zakończenie
* **az containerapp job update -g {grupa} -n {job} --image {obraz}** - podmienia obraz joba
  * **--command alembic --args "upgrade head"** - zmienia polecenie startowe (np. migracje)
* **az containerapp job start -g {grupa} -n {job} --query name -o tsv** - uruchamia joba i zwraca nazwę wykonania
* **az containerapp job execution show -g {grupa} -n {job} --job-execution-name {wykonanie} --query "properties.status" -o tsv** - status wykonania (`Running`, `Succeeded`, `Failed`)
* **az containerapp job logs show -g {grupa} -n {job} --execution {wykonanie} --container {kontener} --tail 200** - logi wykonania
  * nazwę kontenera daje `az containerapp job show ... --query "properties.template.containers[0].name" -o tsv`
  * **--follow** - śledzi na bieżąco
* **az containerapp job secret set -g {grupa} -n {job} --secrets {nazwa}={wartość}** - sekret joba
* **az containerapp job identity assign -g {grupa} -n {job} --system-assigned** - tożsamość zarządzana dla joba

Uruchomienie joba i podejrzenie logów jednym ciągiem:

```bash
EXEC=$(az containerapp job start -g "$RG" -n "$JOB" --query name -o tsv)
CONTAINER=$(az containerapp job show -g "$RG" -n "$JOB" \
  --query "properties.template.containers[0].name" -o tsv)
az containerapp job logs show -g "$RG" -n "$JOB" \
  --execution "$EXEC" --container "$CONTAINER" --tail 200
```

### Key Vault

* **az keyvault show -n {vault} -g {grupa} --query id -o tsv** - ID vaulta, do nadawania ról
* **az keyvault show -n {vault} -g {grupa} --query "properties.enableRbacAuthorization" -o tsv** - czy vault używa RBAC, czy starych access policies
* **az keyvault show -n {vault} -g {grupa} --query "properties.networkAcls" -o jsonc** - reguły sieciowe vaulta
* **az keyvault secret list --vault-name {vault} -o table** - nazwy sekretów
* **az keyvault secret set --vault-name {vault} --name {sekret} --value "{wartość}" -o none** - zapisuje sekret, `-o none` żeby nie wypisał go z powrotem
  * `--value "$(openssl rand -hex 32)"` - od razu generuje losowy klucz
* **az keyvault secret show --vault-name {vault} --name {sekret} --query "length(value)" -o tsv** - sprawdza, że sekret istnieje i ma sensowną długość, bez pokazywania go
* **az keyvault private-endpoint-connection list --vault-name {vault} -g {grupa} -o table** - prywatne endpointy vaulta

Sekret z Key Vaulta wstrzyknięty do Container App przez tożsamość zarządzaną, zamiast kopiowania wartości:

```bash
az containerapp secret set -g "$RG" -n "$APP" --secrets \
  "session-secret-key=keyvaultref:$KVURI/session-secret-key,identityref:system"
```

### Tożsamości, role i service principale

* **az role assignment list --assignee {id} --scope {zakres} --query "[].roleDefinitionName" -o tsv** - jakie role ma dana tożsamość na zasobie
  * **--assignee-object-id {id} --assignee-principal-type ServicePrincipal** - gdy tożsamość jest świeża i nie zdążyła się rozpropagować
* **az role assignment create --assignee-object-id {id} --assignee-principal-type ServicePrincipal --role {rola} --scope {zakres}** - nadaje rolę
  * typowe role: `AcrPull` (ciągnięcie obrazów), `Key Vault Secrets User` (czytanie sekretów), `Storage Blob Data Reader`
* **az ad sp create-for-rbac --name {nazwa} --role Contributor --scopes {zakres}** - tworzy service principal z sekretem; sekret pokazuje się **tylko raz**
* **az ad sp list --display-name {nazwa} --query "[].{name:displayName, appId:appId}" -o table** - szuka SP po nazwie
  * **--all --query "[?contains(displayName, '{fragment}')].{Name:displayName, AppId:appId, ObjectId:id}"** - szuka po fragmencie
* **az ad sp show --id {appId lub objectId} --query appRoleAssignmentRequired** - czy aplikacja wymaga jawnego przypisania użytkowników
* **az ad app credential reset --id {appId} --years 1 --append** - dokłada nowy sekret aplikacji, nie kasując starego

### Microsoft Graph przez az rest

* **az rest --method GET --uri "https://graph.microsoft.com/v1.0/servicePrincipals/{objectId}/appRoleAssignments" --query "value[].{Resource:resourceDisplayName, Permission:appRoleId}" -o table** - uprawnienia aplikacyjne nadane tożsamości
* **az rest --method GET --uri "https://graph.microsoft.com/v1.0/servicePrincipals/{objectId}/appRoleAssignedTo"** - kto ma przypisaną aplikację
* **az rest --method PATCH --uri "https://graph.microsoft.com/v1.0/servicePrincipals/{objectId}" --headers "Content-Type=application/json" --body '{"appRoleAssignmentRequired": true}'** - wymusza przypisanie użytkowników do aplikacji
* w URL-u trzeba escapować `$` z parametrów OData: `?\$select=principalDisplayName,appRoleId`

Nadanie tożsamości zarządzanej uprawnień do Graph API:

```bash
GRAPH_SP_ID=$(az ad sp list --filter "appId eq '00000003-0000-0000-c000-000000000000'" \
  --query "[0].id" -o tsv)
ROLE_ID=$(az ad sp show --id "$GRAPH_SP_ID" \
  --query "appRoles[?value=='User.Read.All' && contains(allowedMemberTypes, 'Application')].id | [0]" -o tsv)
```

`00000003-0000-0000-c000-000000000000` to stałe appId Microsoft Graph, wszędzie takie samo.

### Logi: Log Analytics

* **az monitor log-analytics query -w {workspace id} --analytics-query "{KQL}" -o table** - zapytanie KQL do logów
* **az monitor log-analytics workspace show -g {grupa} -n {workspace} --query customerId -o tsv** - ID workspace, którego oczekuje `-w`
* tabele Container Apps: `ContainerAppConsoleLogs_CL` (stdout aplikacji), `ContainerAppSystemLogs_CL` (zdarzenia platformy, powody niewystartowania)
* **column_ifexists('{kolumna}','')** - odwołanie do kolumny, która może nie istnieć, zamiast błędu zapytania

Logi konkretnego wykonania joba:

```bash
az monitor log-analytics query -w "$WS_ID" --analytics-query "\
ContainerAppConsoleLogs_CL \
| where TimeGenerated > ago(2h) \
| where ContainerJobName_s == '$JOB' \
| where ContainerGroupName_s contains '$EXEC' \
| project TimeGenerated, Stream_s, Log_s \
| order by TimeGenerated asc" -o table
```

Rozbicie logu w formacie JSON na kolumny:

```bash
... | where Log_s startswith '{' \
| extend payload = parse_json(Log_s) \
| project TimeGenerated, created = toint(payload.created), errors = tostring(payload.errors)
```

### Logi: activity log

* **az monitor activity-log list --resource-id {id zasobu} --status Failed --offset 2h -o table** - nieudane operacje na zasobie z ostatnich godzin
  * **--max-events 1 -o json** - pełne szczegóły ostatniego błędu, tam jest prawdziwy komunikat
* **az monitor activity-log list --resource-group {grupa} --offset 4h --status Failed --query "[?contains(resourceId, '{fragment}')]" -o table** - nieudane operacje w całej grupie
* **az monitor app-insights component show -g {grupa} -a {komponent} --query connectionString -o tsv** - connection string do Application Insights

### Container Registry

* **az acr show -g {grupa} -n {rejestr} --query id -o tsv** - ID rejestru, do nadania roli `AcrPull`
* **az acr repository show-tags -n {rejestr} --repository {obraz} --top 20 -o table** - tagi obrazu
  * **--top 1 --orderby time_desc --query "[0]" -o tsv** - najnowszy tag, do podstawienia we wdrożeniu

### Postgres Flexible Server

* **az postgres flexible-server show -g {grupa} -n {serwer} --query "{version:version,aad:authConfig.activeDirectoryAuth,password:authConfig.passwordAuth}" -o jsonc** - wersja i włączone metody uwierzytelniania
* **az postgres flexible-server update -g {grupa} -n {serwer} --public-network-access-enabled true** - otwiera dostęp publiczny (na czas migracji, potem wyłączyć)
* **az postgres flexible-server parameter show -g {grupa} -s {serwer} --name azure.extensions --query value -o tsv** - lista włączonych rozszerzeń
* **az postgres flexible-server parameter set -g {grupa} -s {serwer} --name azure.extensions --value "{lista}"** - włącza rozszerzenia; wartość podmienia całą listę, więc najpierw trzeba odczytać obecną i dopisać do niej

### Storage

* **az storage account network-rule add -g {grupa} --account-name {konto} --ip-address {ip}** - dopuszcza adres IP do konta storage
* **az storage container list --connection-string "$AZURE_STORAGE_CONNECTION_STRING" -o table** - lista kontenerów
* **az storage blob list --container-name {kontener} --connection-string "$AZURE_STORAGE_CONNECTION_STRING" -o table** - lista plików w kontenerze

### Sieć i Application Gateway

* **az network application-gateway list -g {grupa} --query "[].name" -o tsv** - nazwy bram
* **az network application-gateway show-backend-health -g {grupa} -n {brama} -o jsonc** - stan backendów, pierwsze miejsce do sprawdzenia przy 502
* **az network application-gateway http-listener list -g {grupa} --gateway-name {brama} --query "[?protocol=='Https'].name | [0]" -o tsv** - nazwa listenera HTTPS
* **az network application-gateway redirect-config create -g {grupa} --gateway-name {brama} --name {nazwa} --type Permanent --target-listener {listener} --include-path true --include-query-string true** - przekierowanie HTTP na HTTPS
* **az network public-ip show --ids {id} --query "{ip:ipAddress, fqdn:dnsSettings.fqdn}" -o yaml** - adres i nazwa publicznego IP
* **az network private-endpoint-connection list --id {id zasobu} -o jsonc** - prywatne endpointy dowolnego zasobu

### Maszyny wirtualne i nody AKS

* **az ssh vm --local-user {użytkownik} --ip {ip} --private-key-file ~/{klucz}.pem** - SSH do maszyny przez Azure CLI
* **az vm run-command invoke -g {grupa} --name {maszyna} --command-id RunShellScript --scripts "{polecenie}"** - wykonuje polecenie na maszynie bez SSH, przydatne przy nodach AKS bez publicznego IP
* **az vm identity show -g {grupa} --name {maszyna} -o json** - tożsamości przypisane do maszyny
* **az aks nodepool show --cluster-name {klaster} -g {grupa} --name {pula} --query "{Name:name, State:provisioningState}"** - stan puli nodów
* grupa zasobów nodów AKS to `MC_{grupa klastra}_{klaster}_{region}`, a nie grupa samego klastra

### Resource Graph

* **az graph query -q "{KQL}" -o json** - zapytanie po wszystkich subskrypcjach naraz, gdy nie wiadomo gdzie leży zasób

Znalezienie grupy i subskrypcji workspace'u Databricks po ID z URL-a:

```bash
az graph query -q "Resources \
| where type=='microsoft.databricks/workspaces' \
| where properties.workspaceId == '{id}' \
| project name, resourceGroup, subscriptionId, location, url=properties.workspaceUrl" -o json
```

### Zasoby ogólnie

* **az resource show --ids {resource id} --query "{...}" -o jsonc** - odczyt dowolnego zasobu po ID, gdy dedykowane polecenie nie pokazuje potrzebnego pola
  * **--api-version {wersja}** - wymusza wersję API, czasem konieczne dla nowszych pól
