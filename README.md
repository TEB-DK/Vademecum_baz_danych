<div align="center">

# 🎒 Projekt Szkoła 🏫

</div>

## 🚧 Encje projektu:

1. ``Nauczyciele`` - tabela zawierająca informacje o nauczycielach, np. imię, nazwisko, data urodzenia, przedmiot nauczany, adres, numer telefonu, adres e-mail.
2. ``Uczniowie`` - tabela zawierająca informacje o uczniach, np. imię, nazwisko, klasa, adres, numer telefonu, adres e-mail, dane rodziców.
3. ``Przedmioty`` - tabela zawierająca informacje o przedmiotach, np. nazwa, opis, nauczyciel prowadzący.
4. ``Klasy`` - tabela zawierająca informacje o klasach, np. nazwa, rok szkolny, wychowawca klasy.
5. ``Oceny`` - tabela zawierająca informacje o ocenach, np. data wystawienia, przedmiot, uczeń, wartość oceny.

### 🔗 Powiązania między encjami:

- Jeden nauczyciel może prowadzić jeden przedmiot. W tabeli przedmioty znajduje się klucz obcy do tabeli nauczyciele.
- Każda klasa może mieć jednego wychowawcę, ale jeden nauczyciel może być wychowawcą wielu klas. W tabeli klasy znajduje się klucz obcy do tabeli nauczyciele.
- Każdy uczeń jest przypisany do jednej klasy, ale jedna klasa może mieć wielu uczniów. W tabeli uczniowie znajduje się klucz obcy do tabeli klasy.
- Każda ocena jest przypisana do jednego ucznia i jednego przedmiotu. W tabeli oceny znajdują się klucze obce do tabeli uczniowie i przedmioty.

> Taki schemat bazy danych pozwoli na przechowywanie i zarządzanie informacjami o nauczycielach, uczniach, przedmiotach, klasach oraz ocenach w sposób uporządkowany i efektywny.

## 🌟 Zadania do wykonania

> Zapytania DDL oraz DML muszą znajdować się w oddzielnych plikach tekstowych o nazwie ``[nazwisko]_DDL.txt`` i ``[nazwisko]_DML.txt``.
 
###### ❗ W przeciwnym wypadku nie zostaną poddane ocenie pozytywnej.

### 🏗️ Zapytania DDL (30% + wypełnienie)
<sup><sup><sup>(rogal to beztalencie)</sup></sup></sup>

###### ❗ Wartości typów danych powinny być ZOPTYMALIZOWANE, tj. jeżeli wystepuje atrybut nazwisko to należy wyszukać najdłuższe europejskie/polskie nazwisko i wprowadzić odpowiednią liczbę znaków w typie VARCHAR.

- Encja ``Nauczyciele``:

    - Utwórz tabelę ``nauczyciele`` z polami: id (klucz główny, typ danych INT, auto inkrementacja), imie (typ danych VARCHAR), nazwisko (typ danych VARCHAR), data urodzenia (typ danych DATE), adres (typ danych VARCHAR), numer_telefonu (typ danych VARCHAR), email (typ danych VARCHAR), przedmiot_nauczany (typ danych VARCHAR).

    - Ustaw wartość ``id`` jako klucz główny tabeli ``nauczyciele``.

- Encja ``Uczniowie``:

    - Utwórz tabelę ``uczniowie`` z polami: id (klucz główny, typ danych INT, auto inkrementacja), imie (typ danych VARCHAR), nazwisko (typ danych VARCHAR), klasa (typ danych VARCHAR), data urodzenia (typ danych DATE), adres (typ danych VARCHAR), numer_telefonu (typ danych VARCHAR), email (typ danych VARCHAR), imiona_rodzicow (typ danych VARCHAR), srednia (typ danych FLOAT).

    - Ustaw wartość ``id`` jako klucz główny tabeli ``uczniowie``.

- Encja ``Przedmioty``:

    - Utwórz tabelę ``przedmioty`` z polami: id (klucz główny, typ danych INT, auto inkrementacja), nazwa (typ danych VARCHAR), opis (typ danych VARCHAR), nauczyciel_prowadzacy (klucz obcy, typ danych INT).
    Ustaw wartość ``id`` jako klucz główny tabeli ``przedmioty``.

    - Utwórz klucz obcy ``nauczyciel_prowadzacy`` w tabeli ``przedmioty`` odwołujący się do pola ``id`` w tabeli ``nauczyciele``.

- Encja ``Klasy``:

    - Utwórz tabelę ``klasy`` z polami: id (klucz główny, typ danych INT, auto inkrementacja), nazwa (typ danych VARCHAR), rok_szkolny (typ danych VARCHAR), wychowawca (klucz obcy, typ danych INT).

    - Utwórz klucz obcy ``wychowawca`` w tabeli ``klasy`` odwołujący się do pola ``id`` w tabeli ``nauczyciele``.

- Encja ``Oceny``:

    - Utwórz tabelę ``oceny`` z polami: id (klucz główny, typ danych INT, auto inkrementacja), data_wystawienia (typ danych DATE), przedmiot (klucz obcy, typ danych INT), uczen (klucz obcy, typ danych INT), wartosc (typ danych INT).

    - Ustaw wartość ``id`` jako klucz główny tabeli ``oceny``.

    - Utwórz klucz obcy ``przedmiot`` w tabeli ``oceny`` odwołujący się do pola ``id`` w tabeli ``przedmioty``.

    - Utwórz klucz obcy ``uczen`` w tabeli ``oceny`` odwołujący się do pola ``id`` w tabeli ``uczniowie``.

### 🔧 Zapytania DML ( 70% + gwiazdki)


- Encja ``Nauczyciele``:

    - Dodaj ``10`` nowych krotek z danymi spełniającymi wymogi struktury tabeli.
    
    - Zmodyfikuj dane ``3`` dowolnych nauczycieli.

    - Usuń dowolnego nauczyciela nauczającego ``Geografii``.

    - Wyświetl imie, nazwisko oraz adres e-mail nauczycieli, których wiek przekracza 30 lat.

    - Wybierz wszystkich nauczycieli, którzy uczą przedmiotu ``Matematyka`` lub ``Fizyka``.

- Encja ``Uczniowie``:

    - Dodaj ``10`` nowych krotek z danymi spełniającymi wymogi struktury tabeli.

    - Wyświetl 3 najlepszych uczniów z najwyższą średnią.

    - Zmień klasę ucznia o dowolnym id na ``3B``.

    - Wybierz wszystkich uczniów, którzy uzyskali ocenę wyższą niż 4 z przedmiotu matematyka.

    - Wybierz imiona i nazwiska uczniów oraz średnią ich ocen z przedmiotu język polski, posortowane malejąco według średniej ocen.

- Encja ``Przedmioty``:

    - Dodaj ``10`` nowych krotek z danymi spełniającymi wymogi struktury tabeli.

    - Wybierz wszystkie przedmioty, których opis zawiera słowo ``programowanie``.

    - Wybierz przedmioty, których nazwa zaczyna się na literę ``M`` lub ``P``.

    - 🌟 Stwórz zapytanie wybierające nazwy przedmiotów, które mają średnią ocen powyżej ``4.5`` oraz prowadzone są przez nauczyciela o nazwisku zaczynającym się na ``Ko``.

- Encja ``Klasy``:

    - Dodaj ``10`` nowych krotek z danymi spełniającymi wymogi struktury tabeli.

    - Znajdź wszystkie klasy, które nie mają wychowawcy.

    - Wybierz wszystkie klasy, których wychowawca jest nauczycielem matematyki.

    - 🌟 Wybierz wszystkie klasy, których nazwa zaczyna się od litery ``A`` i które mają co najmniej ``30`` uczniów.


- Encja ``Oceny``:

    - 🌟 Utwórz wyzwalacz zliczający średnią z ocen dla każdego ucznia. 

    - Dodaj ``20`` nowych krotek z danymi spełniającymi wymogi struktury tabeli.

    - Zaktualizuj ``5`` dowolnych ocen. 

    - Wybierz średnią wartość ocen z przedmiotu o id ``3``.

    - Wybierz wszystkie oceny wystawione uczniowi o id ``5`` wraz z nazwą przedmiotu, datą wystawienia i wartością oceny.

    - Wybierz listę uczniów, którzy otrzymali ocenę niższą niż ``3`` z przedmiotu o nazwie ``Matematyka``.
