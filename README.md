# Clean IP Blacklist

## Opis

Workflow GitHub Actions automatycznie utrzymuje plik `blacklist.txt` w uporządkowanej postaci. Po każdej zmianie w pliku wykonywane jest czyszczenie listy, usuwanie duplikatów oraz pustych linii, a następnie zapisanie uporządkowanej wersji do repozytorium.

Rozwiązanie eliminuje konieczność ręcznego porządkowania list adresów IP wykorzystywanej np. do blokad sieciowych, reguł zapory lub systemów bezpieczeństwa.

## Funkcjonalności

Workflow automatycznie:

* usuwa puste linie,
* usuwa nadmiarowe spacje i tabulatory na początku oraz końcu wierszy,
* sortuje wpisy alfabetycznie,
* usuwa duplikaty adresów IP,
* zapisuje oczyszczoną listę do pliku `blacklist.txt`,
* wykonuje automatyczny commit zmian do repozytorium.

## Wyzwalanie

Workflow uruchamia się w dwóch przypadkach:

### Automatycznie po zmianie pliku

```yaml
on:
  push:
    paths:
      - "blacklist.txt"
```

Każda modyfikacja pliku `blacklist.txt` powoduje automatyczne uruchomienie procesu czyszczenia.

### Ręcznie

```yaml
workflow_dispatch:
```

Workflow można uruchomić ręcznie z poziomu zakładki **Actions** w GitHub.

## Proces działania

1. Pobranie aktualnej zawartości repozytorium.
2. Odczyt pliku `blacklist.txt`.
3. Usunięcie pustych linii.
4. Usunięcie zbędnych spacji i tabulatorów.
5. Posortowanie wpisów.
6. Usunięcie duplikatów.
7. Zapis oczyszczonej listy.
8. Automatyczny commit zmian (jeżeli wykryto modyfikacje).
9. Wysłanie zmian do repozytorium.

## Przykład

### Przed czyszczeniem

```text
192.168.1.10

10.10.10.5
192.168.1.10

172.16.1.1
```

### Po czyszczeniu

```text
10.10.10.5
172.16.1.1
192.168.1.10
```

## Wymagania

* Repozytorium GitHub
* Włączone GitHub Actions
* Uprawnienia `contents: write`

```yaml
permissions:
  contents: write
```

## Automatyczny commit

Jeżeli workflow wykryje zmiany po czyszczeniu pliku, utworzy commit:

```text
Auto-clean blacklist (dedup + remove empty lines)
```

Jeżeli nie zostaną wykryte żadne zmiany, workflow zakończy działanie bez tworzenia nowego commita.

## Zastosowania

* listy blokowanych adresów IP,
* blacklisty wykorzystywane przez zapory sieciowe,
* systemy IDS/IPS,
* automatyzacja zarządzania regułami bezpieczeństwa,
* centralne utrzymywanie list adresów w repozytorium Git.

## Korzyści

* spójny format danych,
* eliminacja duplikatów,
* łatwiejsze zarządzanie dużymi listami adresów,
* automatyczne utrzymanie jakości danych,
* brak konieczności ręcznej kontroli pliku.
