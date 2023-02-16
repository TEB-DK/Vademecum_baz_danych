<div align="center"><h1>📘 Vademecum baz danych 📘</h1></div>

podstawowe informacje z lokalnych baz danych o charakterze informacyjno-praktycznym. Znajomość tych zagadnień jest **_obowiązkowa_**. 
  ## Podstawowe pojęcia
  
  ### Baza danych
  > zbiór powiązanych ze sobą informacji, zorganizowany w określoną strukturę.
  
  ### Rodzaje baz danych
  > - ``Nierelacyjne`` – baza danych, która nie korzysta ze schematu tabelarycznego wierszy i kolumn znalezionych w większości tradycyjnych systemów baz danych. Zamiast tego nierelacyjne bazy danych używają modelu magazynu zoptymalizowanego pod kątem określonych wymagań dotyczących typu przechowywanych danych. Na przykład dane mogą być przechowywane jako proste pary klucz/wartość, dokumenty ``JSON`` lub jako graf składający się z krawędzi i wierzchołków.
  > - ``Relacyjne`` – dane gromadzone, przetwarzane i przechowywane za pomocą komputera. Dane umieszczone w tabelach (przynajmniej dwóch) pozostających w ścisłym związku z sobą.
  
  ### System zarządzania bazami danych
  > to program komputerowy, który służy do przechowywania i modyfikowania danych, np. Acces (Microsoft Office), Base (OpenOffice).
  
  ### Encja
  > to uporządkowany zbiór danych, przechowywanych w ujednolicony sposób. Dane w tabeli mogą być poddawane różnym operacjom: przeglądane, wyszukiwane, zamieniane, zaznaczane, kopiowane, usuwane.
  
  ### Krotka
  > wiersz w tabeli opisuje informacje o jednym obiekcie. Wiersz składa się z pól opisujących cechy obiektu (atrybutów).
  
  ### Atrybut
  > jest to kolumna zawierająca dane jednego określonego typu.
  
  ### Kwerenda
  > to zapytanie umożliwiające wyświetlenie pól i rekordów z tabel według kryterium ustalonego przez użytkownika. Kwerenda służy też do porządkowania danych, wykonywania obliczeń i aktualizacji danych.
  
  ### Formularz
  > to obiekt, który upraszcza proces wprowadzania i aktualizacji danych.
  
  ### Raport
  > to prezentacja wybranych informacji z bazy danych. Raporty wykonuje się zazwyczaj w formie wydruku.
  
  ### Relacje (związki)
  > to zależności między tabelami umożliwiające ich logiczne powiązanie ze sobą.

  > Typy relacji:
  > - ``Jeden-do-jednego`` - polega na tym, że jednemu rekordowi pierwszej tabeli jest przyporządkowany dokładnie jeden rekord drugiej tabeli, a jednemu rekordowi drugiej tabeli jest przyporządkowany dokładnie jeden rekord pierwszej tabeli, np. jeden przelew bankowy ma przyporządkowany jeden kod jednorazowy i odwrotnie.
  > - ``Jeden-do-wielu`` - polega na tym, że jednemu rekordowi pierwszej tabeli jest przyporządkowanych wiele rekordów drugiej tabeli, a jednemu rekordowi drugiej tabeli jest przyporządkowany dokładnie jeden rekord pierwszej tabeli, np. jeden wychowawca ma wielu uczniów, ale uczeń ma jednego wychowawcę.
  > - ``Wiele-do-wielu`` - polega na tym, że jednemu rekordowi pierwszej tabeli jest przyporządkowanych wiele rekordów drugiej tabeli, a jednemu rekordowi drugiej tabeli jest przyporządkowanych wiele rekordów pierwszej tabeli, np. jeden nauczyciel uczy wielu uczniów, a każdy uczeń ma wielu nauczycieli.
  
  ### Klucz główny (podstawowy)
  > to unikatowa nazwa pola (używa się nazwy ID – np. ID imię)- typ pola autonumerowanie. Każdy rekord w tabeli musi mieć swój unikatowy numer.
  
  ### Klucz obcy
  > atrybut jest kluczem obcym dla danej tabeli, jeśli nie jest jej kluczem podstawowym, ale jej wartości są wartościami klucza podstawowego innej tabeli.
 
  ### Typy danych
  > - Liczby całkowite - ``INT``, ``TINYINT``.
  > - Liczby rzeczywiste - ``FLOAT``, ``DECIMAL``
  > - Czasowe - ``DATETIME``, ``DATE``, ``TIME``.
  > - Ciągi znaków - ``CHAR``, ``VARCHAR``, ``BLOB``

  ### Język manipulacji danych (ang. ``Data Manipulation Language``)
  > zbiór instrukcji języka zapytań używanych do przetwarzania danych z bazy danych. Są to instrukcje takie jak: ``SELECT``, ``INSERT``, ``UPDATE``, ``DELETE``.

  > <div align="right"><sub>1.1 Listing - Przykładowa składnia instrukcji SELECT</sub></div>
  ```sql
  SELECT atrybut FROM nazwa_tabeli  
  WHERE [warunki_wyszukiwania] 
  ORDER BY [ASC / DESC], 
  LIMIT ilość_wierszy;
  ```

  > <div align="right"><sub>1.2 Listing - Przykładowa składnia instrukcji INSERT</sub></div>
  ```sql
  INSERT INTO nazwa_tabeli VALUES ('Przykład', 69); 
  INSERT INTO nazwa_tabeli (atrybut, atrybut...) VALUES ('Przykład', 69); 
  ```

  > <div align="right"><sub>1.3 Listing - Przykładowa składnia instrukcji UPDATE</sub></div>
  ```sql
  UPDATE nazwa_tabeli SET atrybut = 'nowa_wartość' WHERE [warunek_wyszukania];
  ```

  > <div align="right"><sub>1.4 Listing - Przykładowa składnia instrukcji DELETE</sub></div>
  ```sql
  DELETE FROM nazwa_tabeli WHERE [warunek_wyszukania];
  ```

  ### Język definicji danych (ang. ``Data Definition Language``)
  > zbiór instrukcji języka zapytań używanych do definiowania struktur danych. Możemy do nich zaliczyć polecenia takie jak ``CREATE``, ``ALTER``, ``DROP``. Za pomocą instrukcji ``DDL`` osoba nie manipuluje bezpośrednio danymi, a ich strukturą. Można zdefiniować kolumny tabel, zmienić typy danych, czy usunąć obiekt taki jak widok, czy tabela.

  > <div align="right"><sub>1.5 Listing - Przykładowa składnia instrukcji CREATE</sub></div>
  ```sql
  CREATE DATABASE nazwa_bazy;

  CREATE TABLE nazwa_tabeli (
    [ struktura_tabeli ]
  );
  ```

  > <div align="right"><sub>1.6 Listing - Przykładowa składnia instrukcji ALTER</sub></div> 
  ```sql
  ALTER TABLE nazwa_tabeli [ADD / MODIFY / RENAME / DROP] atrybut;
  ```

  > <div align="right"><sub>1.7 Listing - Przykładowa składnia instrukcji DROP</sub></div>
  ```sql
  DROP TABLE nazwa_tabeli;

  DROP DATABASE nazwa_bazy;
  ```

  ### Wyzwalacz
  > jest to skrypt (fragment kodu) wykonywany w przypadku zajścia jakiegoś zdarzenia w bazie danych (np. dodania danych, ich modyfikacji, czy usunięcia).
  
  > Typy wyzwalaczy:
  > - ``AFTER DELETE`` – wykonanie wyzwalacza po operacji usunięcia rekordu.
  > - ``AFTER INSERT`` – wykonanie wyzwalacza po dodaniu rekordu.
  > - ``AFTER UPDATE`` – wykonanie wyzwalacza po zmodyfikowaniu rekordu.

  > - ``BEFORE DELETE`` – wykonanie wyzwalacza przed operacji usunięcia rekordu.
  > - ``BEFORE INSERT``  – wykonanie wyzwalacza przed dodaniu rekordu.
  > - ``BEFORE UPDATE`` – wykonanie wyzwalacza przed zmodyfikowaniu rekordu.
  
  > <div align="right"><sub>1.8 Listing - Przykładowa składnia wyzwalacza</sub></div>
  ```sql
  CREATE TRIGGER nazwa_wyzwalacza
  BEFORE INSERT ON
  nazwa_tabeli 
  FOR EACH ROW BEGIN
  ...  
  END
  ```
  
  ### Procedura
  > pozwalają zdefiniować dowolne zapytanie i wywołać je za pomocą komendy EXEC nazwa_procedury. Zmienne lokalne, globalne i parametry funkcji i procedur oznaczamy poprzez ``@`` i typ zmiennej.
  
  > <div align="right"><sub>1.9 Listing - Przykładowa procedura</sub></div>
  ```sql
  CREATE PROCEDURE nazwa_procedury @zmienna int
  AS
  SELECT * FROM nazwa_tabeli WHERE id = @zmienna;

  EXEC nazwa_procedury 10
  ```
  
