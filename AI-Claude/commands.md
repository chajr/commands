# Polecenia Claude Code

## Wbudowane

* **/usage** - pokazuje wykorzystanie limitów planu (sesyjnych i tygodniowych)
* **/cost** - pokazuje koszt i czas trwania bieżącej sesji
* **/context** - rozbicie zajętości okna kontekstowego (prompt systemowy, narzędzia, pliki, historia)
* **/clear** - kasuje historię rozmowy i zaczyna nową sesję od zera
* **/compact** - streszcza dotychczasową rozmowę, żeby zwolnić miejsce w kontekście (zmniejsza zużycie tokenów, ale nie usuwa historii)
  * **/compact {instrukcja}** - streszcza z naciskiem na wskazany wątek (np. `/compact skup się na zmianach w API`)

## Ponytail

Wymusza najprostsze rozwiązanie, które działa — mniej kodu, mniej zależności, mniej abstrakcji.

* **/ponytail** - włącza tryb „leniwego seniora” na resztę sesji
  * **lite | full | ultra** - siła trybu, domyślnie `full` (np. `/ponytail ultra`)
* **/ponytail-review** - przegląd zmian pod kątem nadmiarowego kodu: co wyrzucić i czym to zastąpić
* **/ponytail-audit** - to samo, ale dla całego repozytorium: lista rzeczy do usunięcia lub uproszczenia
* **/ponytail-debt** - zbiera komentarze `ponytail:` z kodu w jedną listę świadomie odpuszczonych skrótów
* **/ponytail-gain** - podsumowanie z benchmarków: ile kodu, czasu i kosztu oszczędza tryb ponytail
* **/ponytail-help** - ściąga ze wszystkich trybów i poleceń ponytail

## BMAD

Metodyka prowadzenia projektu od pomysłu do kodu, każdy etap ma własne polecenie.

* **/bmad-product-brief** - tworzy, aktualizuje lub waliduje brief produktowy
* **/bmad-prd {opis}** - tworzy, aktualizuje lub waliduje PRD (dokument wymagań produktowych)
* **/bmad-spec {opis}** - destyluje pomysł, notatki albo PRD do zwięzłego `SPEC.md`; potrafi też rozbić spec na stories
    * ** Zaplanuj architekturę...** - spisuje decyzje architektoniczne w dokumencie architektury
    * ** Zweryfikuj stan...** - uruchamia rolę weryfikatora
    * ** Wygeneruj listę zadań...** - uruchamia rolę planisty
    * ** Utwórz specyfikację...** - uruchamia rolę dokumentalisty
* **/bmad-architecture** - spisuje decyzje architektoniczne w dokumencie architektury
* **/bmad-ux** - tworzy specyfikację UX: `DESIGN.md` (wygląd) i `EXPERIENCE.md` (zachowanie)
* **/bmad-create-epics-and-stories** - rozbija wymagania na epiki i user stories
* **/bmad-sprint-planning** - sprawdza gotowość do implementacji i generuje plik statusu sprintu
* **/bmad-build** - implementuje story, feature albo poprawkę i weryfikuje wynik
* **/bmad-code-review** - kilku niezależnych recenzentów równolegle, potem triaż i zbiorcze wnioski
* **/bmad-walkthrough** - prowadzi przez przegląd zmiany: po co jest, na co patrzeć, jak przetestować
* **/bmad-correct-course** - ocenia wpływ dużej zmiany w trakcie sprintu na PRD, epiki, architekturę i UX
* **/bmad-retrospective** - retrospektywa epiku na podstawie commitów, diffów i statusu sprintu

Role (agenci), których można wywołać po imieniu:

* **/bmad-agent-analyst** - Mary, analityk biznesowy: badanie rynku, konkurencji, wymagań
* **/bmad-agent-pm** - John, product manager: PRD i zbieranie wymagań
* **/bmad-agent-architect** - Winston, architekt systemu
* **/bmad-agent-ux-designer** - Sally, projektantka UX/UI
* **/bmad-agent-dev** - Amelia, programistka: implementacja stories

## Superpowers

Zestaw „umiejętności” wymuszających konkretny sposób pracy zamiast improwizacji.

* **/using-superpowers** - zasady korzystania ze skilli: sprawdź i użyj pasującego, zanim cokolwiek zrobisz
* **/brainstorming** - obowiązkowe przed tworzeniem czegokolwiek: dopytuje o intencje, wymagania i projekt
* **/writing-plans** - zamienia wymagania na pisemny plan wdrożenia
* **/executing-plans** - realizuje gotowy plan z punktami kontrolnymi do przeglądu
* **/systematic-debugging** - uporządkowane szukanie przyczyny błędu, zanim padnie propozycja poprawki
* **/test-driven-development** - test przed implementacją
* **/requesting-code-review** - zamawia przegląd kodu po skończonym zadaniu
* **/receiving-code-review** - jak przyjąć uwagi z przeglądu: weryfikacja zamiast potakiwania
* **/verification-before-completion** - wymusza uruchomienie testów przed ogłoszeniem „zrobione”
* **/using-git-worktrees** - izolowany katalog roboczy na czas pracy nad zmianą
* **/finishing-a-development-branch** - domknięcie gałęzi, gdy wszystko przechodzi
* **/dispatching-parallel-agents** - rozdziela niezależne zadania między równoległych agentów
* **/subagent-driven-development** - realizacja planu przez podagentów w bieżącej sesji
* **/writing-skills** - tworzenie i edycja własnych skilli
