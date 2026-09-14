# Współtworzenie

To repozytorium trzyma wiedzę i nic więcej. Nie ma budowania, CI ani linterów — konwencje poniżej pilnujemy ręcznie.

## Co tu wchodzi

Tylko pliki `.md` z wiedzą. Dwa wyjątki to `.gitignore` i `LICENSE`.

Brudnopisy, zrzuty historii shella i wszystko niedokończone trafiają do `notes/`, który jest ignorowany przez gita i zostaje na twojej maszynie.

Nigdy nie commitujemy: wewnętrznych nazw hostów, nazw klientów, danych logowania, ścieżek zawierających nazwę użytkownika (piszemy `~/`), plików IDE ani śmieci systemowych.

## Gdzie trafia wpis

Jeden katalog na temat, jeden plik na rodzaj treści:

- `{temat}/commands.md` — ściąga poleceń (`sql/` używa `mysql-commands.md`)
- `{temat}/links.md` — dokumentacja, kursy, narzędzia
- wszystko inne dostaje opisową nazwę: `php/changes.md`, `magento/magento.md`

Nowy temat to nowy katalog. Każdy nowy plik dopisujemy do tabeli w `README.md` — nic jej nie generuje.

## Język

Wszystko po polsku: opisy, nagłówki sekcji, README. Polecenia, nazwy flag i kod zostają oczywiście w oryginale.

## Format: polecenia

Nagłówek `###` na sekcję, pod nim lista punktowana, jedno polecenie na linię. Polecenie pogrubione, potem ` - `, potem opis:

```markdown
### Polecenia docker

* **docker start {nazwa}** - uruchamia kontener
* **docker run -id --name {kontener} {obraz}** - tworzy i uruchamia kontener z obrazu
  * **-u www-data** - uruchamia z podanym użytkownikiem
  * **--rm** - kasuje kontener po zamknięciu
```

- Zmienne fragmenty w nawiasach klamrowych: `{nazwa}`, `{kontener}`.
- Flagi i warianty jako zagnieżdżona lista pod poleceniem.

## Format: linki

Gołe URL-e, jeden na linię. Bez składni linku Markdown, bez opisów:

```
https://reactphp.org/
https://github.com/vlucas/phpdotenv
```

Przed dodaniem linku obcinamy parametry śledzące (`?fbclid=...`, `?utm_source=...`).

## Nie wszystko się trzyma konwencji

`magento/magento.md` powstał przed tymi konwencjami i nadal jest surową listą poleceń. Poprawiamy przy okazji, gdy i tak w nim grzebiemy; nie przepisujemy dla samego przepisywania.
