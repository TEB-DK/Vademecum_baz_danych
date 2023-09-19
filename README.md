<div align="center"><h1>🛠️ Zapytania SQL (DDL) 🛠️</h1></div>
    
  ``DDL (Data Definition Language)`` jest to język, który służy do definiowania struktury bazy danych, czyli do tworzenia, modyfikowania i usuwania obiektów bazodanowych takich jak tabele, widoki, procedury składowane, indeksy, ograniczenia i inne.

  Jego głównym celem jest umożliwienie precyzyjnego definiowania schematu bazy danych, który zapewni spójność danych w całym systemie. DDL umożliwia określenie typu danych, długości i innych właściwości kolumn tabeli, definiowanie relacji między tabelami, określanie kluczy głównych i obcych, a także inne istotne aspekty projektowania bazy danych.

  W praktyce, DDL jest używane w celu utworzenia struktury bazy danych podczas tworzenia aplikacji lub w przypadku wprowadzania zmian do istniejącej bazy danych. DDL może być również używane do automatyzacji procesu instalacji bazy danych lub jej migracji między różnymi środowiskami.

  W przypadku systemów zarządzania bazami danych, takich jak MySQL, DDL jest integralną częścią języka SQL, który jest standardowym językiem używanym w większości relacyjnych systemów zarządzania bazami danych.

---

  ## 🏗️ Tworzenie struktur - ``CREATE``
  > Instrukcja ``CREATE`` wykorzystywana jest do tworzenia konkretnych struktur, takich jak: baza danych, table czy widoki, a nawet indeksów.

  ### ``DATABASE``
  Dzięki poleceniu ``CREATE DATABASE`` możemy zdefiniować nową bazę danych.
  
  > <div align="right"><sub>2.1 Listing - Przykładowe utworzenie bazy danych o nazwie ``Przykladowa_baza``</sub></div>
  ```sql
  CREATE DATABASE Przykladowa_baza;
  ```

  #### Klauzula ``IF NOT EXISTS``
  Jeśli nie jesteśmy pewni, czy dana baza danych o konkretnej nazwie już istnieje, warto użyć (a wręcz zawsze powinna być użyta) klauzuli ``IF NOT EXISTS``, przed utworzeniem bazy danych dzięki temu zapytaniu zostanie sprawdzone czy baza danych o konkretnej nazwie już widnieje.

  > <div align="right"><sub>2.2 Listing - Przykładowe utworzenie bazy danych o nazwie ``Przykladowa_baza``</sub></div>
  ```sql
  CREATE DATABASE IF NOT EXISTS Przykladowa_baza;
  ```

  ### ``TABLE``

  > <div align="right"><sub>2.3 Listing - Przykładowe utworzenie tabeli o nazwie ``Przykladowa_tabela``</sub></div>
  ```sql
  CREATE TABLE Przykladowa_tabela (
    ID INT,
    Atrybut1 VARCHAR(40),
    Atrybut2 DATETIME,
    Atrybut3 BLOB
  )
  ```

  Atrybuty definują nazwę kolumny w bazie danych, po których następuje określenie typu atrybutu.

  Aby utworzyć tabelę taką samą jak już istniejąca, należy wykorzystać słowa kluczowego wykorzystywanego do aliasu w zapytaniach DML - ``AS``.

  > <div align="right"><sub>2.4 Listing - Utworzenie kopii tabeli</sub></div>
  ```sql
  CREATE TABLE Nowa_tabela AS
  SELECT Atrybut1, Atrybut2
  FROM Przykladowa_tabela
  ```
  
  #### Ograniczenia w tabeli - (``Constraints``)
  > Ograniczenia służą do ograniczenia typu danych, które można umieścić w tabeli. Zapewnia to dokładność i wiarygodność danych w tabeli. Jeśli wystąpi jakiekolwiek naruszenie między ograniczeniem a działaniem dotyczącym danych, działanie zostanie przerwane.

  Ograniczenia mogą dotyczyć poziomu kolumny lub poziomu tabeli. Ograniczenia na poziomie kolumny mają zastosowanie do kolumny, a ograniczenia na poziomie tabeli dotyczą całej tabeli.
  
  > <div align="right"><sub>2.5 Tabela - Ograniczenia atrybutów</sub></div>

  <div align="center">

  | Typ ograniczenia | Opis |
  |:---:|:---:|
  |NOT NULL| Zapewnia, że kolumna nie będzie posiadała wartości NULL|
  |UNIQUE| Zapewnia, że wartości w kolumnie będą unikalne (różne od siebie)|
  |PRIMARY KEY| W prawdzie połączenie NOT NULL i UNIQUE - Jednoznaczny identyfikator wiersza w tabeli|
  | FOREIGN KEY | Klucz obcy |
  | CHECK | Zapewnia, że wartości w kolumnie spełniają dany warunek|
  | DEFAULT| Ustalona jest domyślna wartość dla wiersza z pustą komórką|
  | CREATE INDEX | Służy do indeksowania w celu szybszego wyszukiwania danych |

  </div>

  > <div align="right"><sub>2.6 Listing - Przykładowe utworzenie tabeli z ograniczeniem NOT NULL</sub></div>
  ```sql
  CREATE TABLE Czlowieki (
    CzlowiekiID int NOT NULL,
    Imie varchar(255) NOT NULL,
    Nazwisko varchar(255) NOT NULL,
    Wiek int
  ); 
  ```
  > <div align="right"><sub>2.7 Listing - Zastosowanie ograniczenia UNIQUE</sub></div>
  ```sql
  CREATE TABLE Czlowieki (
    CzlowiekiID int NOT NULL,
    Imie varchar(255) NOT NULL,
    Nazwisko varchar(255),
    Wiek int,
    UNIQUE (CzlowiekiID)
  );
  ```
  
  Natomiast aby zdefiniować ``UNIQUE`` ograniczenie dla wielu atrybutów w tabeli trzeba określić osobne ograniczenie.

  > <div align="right"><sub>2.8 Listing - Zastosowanie ograniczenia UNIQUE z przydzieloną nazwą</sub></div>
  ```sql
  CREATE TABLE Czlowieki (
    CzlowiekiID int NOT NULL,
    Imie varchar(255) NOT NULL,
    Nazwisko varchar(255),
    Wiek int,
    CONSTRAINT Nazwa_ograniczenia UNIQUE (CzlowiekiID, Imie)
  );
  ```

  W prosty sposób możemy zastosować składnię aby określić klucz główny, jednakże znacznie łatwiej jest zapisać to ograniczenie zaraz po typie atrybutu.

  > <div align="right"><sub>2.9 Listing - Zastosowanie ograniczenia PRIMARY KEY</sub></div>
  ```sql
  CREATE TABLE Czlowieki (
    CzlowiekiID int NOT NULL,
    Imie varchar(255) NOT NULL,
    Nazwisko varchar(255),
    Wiek int,
    PRIMARY KEY (CzlowiekiID)
  );
  ```

  Określenie klucza obcego wymaga zastosowania poniższej składni odwołującej się do tabli, z którą zostanie połączona.

  > <div align="right"><sub>2.10 Listing - Zastosowanie PRIMARY KEY oraz FOREIGN KEY w odniesieniu do tabeli Czlowieki</sub></div>
  ```sql
  CREATE TABLE Zwierze (
    ZwierzeID int NOT NULL,
    Imie varchar(255) NOT NULL,
    Wiek int,
    CzlowiekiID int,

    PRIMARY KEY (ZwierzeID),
    FOREIGN KEY (CzlowiekiID) REFERENCES Czlowieki(CzlowiekiID)
  );
  ```

  By natomiast zdefiniować alternatywną nazwę dla klucza obcego, wystarczy zapisać pełną forme zapisu.

  > <div align="right"><sub>2.11 Listing - Zastosowanie pełnej syntaktyki FOREIGN KEY w odniesieniu do tabeli Czlowieki</sub></div>
  ```sql
  ...
  CONSTRAINT FK_CzlowiekiZwierze FOREIGN KEY (CzlowiekiID)
  REFERENCES Czlowieki(CzlowiekiID)
  ``` 

  Ciekawą opcją jest ograniczenie ``CHECK``, które dosłownie pozwala wprowadzić nam ``instrukcję warunkową``, za pomocą której będzie mozliwe dodanie rekordu do tabeli, lub też nie.

  > <div align="right"><sub>2.12 Listing - Przykład zastosowania ograniczenia CHECK</sub></div>
  ```sql
  CREATE TABLE Zwierze (
    ZwierzeID int NOT NULL,
    Imie varchar(255) NOT NULL,
    Wiek int,
    CzlowiekiID int,

    PRIMARY KEY (ZwierzeID),
    CONSTRAINT WiekCheck_Zwierze CHECK (Wiek >= 0)
  );
  ```

  Cały warunek odbywa się w nawiasach okrągłych, dzięki czemu możemy zastosować ``operatory logiczne`` wykorzystywane w zapytaniach ``DML``.

  #### Typy atrybutów

  > <div align="right"><sub>2.13 Tabela - Najważniejsze typy danych</sub></div>

  <div align="center">

  | Typ | Opis |
  |:---:|:---: |
  | CHAR(rozmiar) | Ciąg znaków o określonej długości (może zawierać litery, cyfry i specjalne znaki). Parametr ``rozmiar`` definiuje długość kolumny w znakach (od 0 do 255). |
  | VARCHAR(rozmiar) | Ciąg znaków o zmiennej długości. Parametr ``rozmiar`` definiuje długość kolumny w znakach (od 0 do 65535) |
  | BINARY(rozmiar) | Podobnie jak CHAR(), z tą różnicą, że przechowuje binarnie bity ciągów. Parametr ``rozmiar`` definiuje długość kolumny w bajtów |
  | TEXT(rozmiar) | Przechowuje ciąg znaków z maksymalną długością 65,535 bajtów |
  | BLOB(rozmiar) | Binary Large OBjects - przechowuje do 65,535 bajtów danych |
  | BIT(rozmiar) | Typ bitowy, liczba bitów na wartość jest zdefiniowana w parametrze ``rozmiar``. Może przechowywać od 1 do 64 bitów. |
  | TINYINT(rozmiar) | Bardzo mała liczba całkowita, od -128 do 127. |
  | INT(rozmiar)| Średnia liczba całkowita, może przechowywać od -2147483648 do 2147483647, parametr ``rozmiar`` definiuje maksymalną szerokość (czyli 255) |
  | FLOAT(punkt) | Liczba zmienno-przecinkowa, MySQL za pomocą parametru ``punkt`` definiuje czy liczba jest typu FLOAT(od 0 do 24), czy DOUBLE (od 25 do 53)|
  | DOUBLE(rozmiar, ilosc_liczb_po_przecinku) | Liczba zmienno-przecinkowa normalnej wielkości, całkowita liczba cyfr określona jest w ``rozmiar``, natomiast liczba cyfr po przecinku w ``ilosc_liczb_po_przecinku`` |
  | DECIMAL(rozmiar, ilosc_liczb_po_przecinku) | Dokładnie określony punkt przecinkowy, liczba cyfr po przecinku zdefiniowana jest w ``ilosc_liczb_po_przecinku``, maksymalna wartość tego parametru to 30, a dla ``rozmiar`` - 65. |
  | DATE | Format daty: YYYY-MM-DD, wspiera zakres od ``1000-01-01`` do ``9999-12-31``|
  | TIME | Format czasu: hh:mm:ss, wspiera zakres od ``-838:59:59`` do ``838:59:59`` |
  | YEAR | 4 cyfrowy format roku |

  </div>

  ## 🧬 Modyfikowanie struktur - ``ALTER``
  > Po utworzeniu konkretnej struktury, może zdarzyć się potrzeba modyfikacji jej, czy to będzie tabela czy nawet baza danych. Służy ku temu deklaracja ``ALTER``. Za przykład posłuży wcześniej wspomniania encja ``Czlowieki``.

  ### Dodawanie atrybutów

  > <div align="right"><sub>2.14 Listing - Modyfikacja tabeli, dodanie atrybutu</sub></div>
  ```sql
  ALTER TABLE Czlowieki
  ADD Drugie_imie VARCHAR(69); 
  ```

  W podobny sposób możemy dodawać ograniczenia (constraints) czy chociażby indeksowanie.

  ### Usuwanie atrybutów
  Aby usunąć atrybut, należy jedynie podać jego konkretną nazwę, poprzedzone słowem ``COLUMN``, aby MySQL wiedział, że usuwa kolumne, a nie na przykład powiązanie.
  
  > <div align="right"><sub>2.15 Listing - Modyfikacja tabeli, usunięcie atrybutu</sub></div>
  ```sql
  ALTER TABLE Czlowieki
  DROP COLUMN Wiek;
  ```

  ### Modyfikowanie atrybutów

  > <div align="right"><sub>2.16 Listing - Modyfikacja tabeli, zmodyfikowanie atrybutu i jego typu</sub></div>
  ```sql
  ALTER TABLE Czlowieki
  MODIFY COLUMN Drugie_imie VARCHAR(100); 
  ```

  Modyfikować również można w taki sposób aby dodać ograniczenia.

  > <div align="right"><sub>2.17 Listing - Modyfikacja tabeli, zdefiniowanie ograniczenia</sub></div>
  ```sql
  ALTER TABLE Czlowieki
  MODIFY Wiek int NOT NULL;
  ```

  ## 📇 Indeksy
  Indeksy zostały zostawione na koniec ze względu na brak ich klarowności.

  W wszelakich źródłach możemy wyczytać, że indeksy są po to by:

  > Indeksy są wykorzystywane do zwracania danych z bazy danych szybciej niż zwykle. Użytkownicy nie widzą indeksów, natomiast istnieją one po to by przyśpieszyć wyszukiwanie / zapytania. ~ ``W3Schools.com, dostęp do zasobu z dnia 27.03.2023``

  > Indeksy można tworzyć na jednej lub kilku kolumnach, które zamierzamy sprawdzić w zapytaniu. Indeksy to struktury danych na dysku umożliwiające szybkie wyszukiwanie danych w bazie danych na podstawie wartości kluczy wyszukiwania. ~ ``Krysinski.eu, dostęp z dnia 21.02.2013``

  > Indexing is the way to get an unordered table into an order that will maximize the query’s efficiency while searching. When a table is unindexed, the order of the rows will likely not be discernible by the query as optimized in any way, and your query will therefore have to search through the rows linearly. In other words, the queries will have to search through every row to find the rows matching the conditions. As you can imagine, this can take a long time. Looking through every single row is not very efficient. ~ ``Chartio.com, dostęp z dnia 27.03.2023``

  Jaśniejsze wyjaśnienie tego zagadnienia

  > W gruncie rzeczy indeks w tabeli działa dokładnie jak indeks w książce:

  > Powiedzmy, że jest książka o bazach danych i chcemy znaleźć jakąś informacje o na przykład "magazynowaniu". Bez indeksu trzeba przejść po każdej ze stron jedna po drugiej, do momentu aż znajdziemy temat który nas interesuje (to był by skan całej tabeli). Z drugiej strony, indeks ma liste słów kluczowych, więc jeśli chcielibyśmy odnieść się do informacji o "magazynowaniu", które wskazywałby indeks na stronach 100-213,700 i 862-giej to przerzucilibyśmy szybko strony na te konkretne bez przekładania jednej po drugiej (i stąd bierze się cały zysk szybkości indeksowania).

  > Oczywiście, od tego jak indeks może być przydany, zależy wiele rzeczy - kilka przykładów:

  > Jeśli mielibyśmy książke o bazach danych i zindeksowane słowo "baza danych" wystąpiło by na stronach 1-69, 71-333 i od 335 do 700, to w tym przypadku indeks nie jest zbyt pomocny, ze względu na to, że przeszukanie stron jedna po drugiej okazałoby się szybsze, co jest określane mianem ``słabej selektywności``.
  
  > Dla 10 stronnicowej książki, indeksy są bez sensu, ponieważ jest to zbyt mała książka, by indeksowanie pozostawiło znaczący "ślad" w szybkości wyszukiwania wyników. ~ ``Stackoverflow.com, zasób z dnia 27.03.2023, tłum. Damian K.``
    

  ### Tworzenie indeksów

  Składnia indeksu pozwalająca na duplikaty wartości

  > <div align="right"><sub>2.18 Listing - Modyfikacja tabeli, zdefiniowanie ograniczenia</sub></div>
  ```sql
  CREATE INDEX nazwa_indeksu
  ON nazwa_tabeli (atrybut1, atrybut2, ...);
  ```

  Składnia indeksu nie pozwalająca na duplikaty wartości, tzw. "unikalny indeks".

  > <div align="right"><sub>2.19 Listing - Modyfikacja tabeli, zdefiniowanie ograniczenia</sub></div>
  ```sql
  CREATE UNIQUE INDEX nazwa_indeksu
  ON nazwa_tabeli (atrybut1, atrybut2, ...);
  ```

  Przykładowe zastosowanie indeksu na opisywanej wcześniej tabeli ``Czlowieki``. Aby zastosować indeks do kilku atrybutów należy oddzielić je przecinkiem i stosownie nazwać indeks.

  > <div align="right"><sub>2.20 Listing - Modyfikacja tabeli, zdefiniowanie ograniczenia</sub></div>
  ```sql
  CREATE INDEX idx_nazwisko
  ON Czlowieki (Nazwisko);
  ```

  Usunięcie indeksu z tabeli odbywa się za pomocą modyfikacji tabeli

  > <div align="right"><sub>2.21 Listing - Modyfikacja tabeli, zdefiniowanie ograniczenia</sub></div>
  ```sql
  ALTER TABLE Czlowieki
  DROP INDEX idx_nazwisko;
  ```

  ###### ❗ Należy pamietać, że aktualizacja encji z indeksami zajmie znacznie więcej czasu niż encja bez. Powodem tego będą indeksy, które również potrzebują dodatkowego czasu na aktualizację. Dobrą praktyką zatem jest tworzenie indeksów na kolumnach, które są często wyszukiwane.
  
  ### 🦖 Wyzwalacze
  Wyzwalacz (trigger) to obiekt bazy danych, który reaguje na zmiany w tabeli (np. wstawienie, aktualizacja lub usunięcie rekordu) i wykonuje określone akcje w odpowiedzi na te zmiany.

  Aby utworzyć wyzwalacz w MySQL, należy użyć polecenia CREATE TRIGGER, które przyjmuje następujące argumenty:

  - ``nazwa_wyzwalacza`` - nazwa wyzwalacza, która powinna być unikalna w ramach bazy danych.
  - ``BEFORE/AFTER`` - określa, czy wyzwalacz ma być uruchamiany przed (BEFORE) lub po (AFTER) wykonaniu operacji na tabeli.
    INSERT/UPDATE/DELETE - określa, na której operacji na tabeli wyzwalacz ma reagować.
  - ``ON nazwa_tabeli`` - określa, na której tabeli wyzwalacz ma działać.
  - ``FOR EACH ROW`` - oznacza, że wyzwalacz ma działać na każdym wierszu tabeli, który spełnia warunek.
  - ``BEGIN ... END`` - definiuje blok kodu, który ma być wykonany po spełnieniu warunku.

  Na przykład, poniższy kod tworzy wyzwalacz, który zwiększa wartość licznika po każdym wstawieniu wiersza do tabeli ``Customers``
  
  > <div align="right"><sub>2.22 Listing - Przykład wykorzystania wyzwalacza</sub></div>
  ```sql
  CREATE TRIGGER zliczacz_licznika
  AFTER INSERT ON Customers
  FOR EACH ROW
  BEGIN
    UPDATE licznik SET wartosc = wartosc + 1 WHERE name = 'Customers';
  END;
  ```

  Przykład wyzwalacza dla aktualizacji sumy zamówienia

  Załóżmy, że w bazie danych istnieje tabela ``orders``, w której przechowywane są informacje o zamówieniach. W tabeli tej znajdują się kolumny ``order_id``, ``customer_id`` oraz ``total_amount``. W celu automatycznego aktualizowania wartości kolumny ``total_amount`` po dodaniu nowego wiersza do tabeli ``order_items`` (która przechowuje szczegóły dotyczące poszczególnych pozycji w zamówieniu), można użyć wyzwalacza o następującej treści:
  
  > <div align="right"><sub>2.23 Listing - Kolejny przykład użycia wyzwalacza</sub></div>
  ```sql
  CREATE TRIGGER update_order_total 
  AFTER INSERT ON order_items
  FOR EACH ROW
  BEGIN
    UPDATE orders SET total_amount = total_amount + NEW.price * NEW.quantity
    WHERE order_id = NEW.order_id;
  END;
  ```

  Wyzwalacz ten jest uruchamiany po każdym wstawieniu nowego wiersza do tabeli ``order_items``. Dla każdego nowego wiersza wyzwalacz pobiera wartość kolumn ``price`` i ``quantity`` z tego wiersza i używa ich do aktualizacji wartości kolumny ``total_amount`` w tabeli ``orders``. Wyzwalacz jest ustawiony tak, aby działał na każdym wierszu (klauzula ``FOR EACH ROW``) i korzysta z identyfikatora zamówienia, aby zaktualizować tylko odpowiedni wiersz w tabeli ``orders``.


  ### ⚙️ Procedura
  Procedura (``stored procedure``) to zbiór instrukcji SQL, który może być przechowywany w bazie danych i wywoływany wielokrotnie. Procedury są bardzo przydatne w aplikacjach, ponieważ pozwalają na zdefiniowanie złożonych operacji i odseparowanie ich od logiki aplikacji. W MySQL tworzenie procedur jest stosunkowo proste.

  Oto przykład tworzenia procedury, która dodaje nowego użytkownika do tabeli ``users``:

  > <div align="right"><sub>2.24 Listing - Przykładowa procedura</sub></div>
  ```sql
  CREATE PROCEDURE add_user(IN username VARCHAR(50), IN password VARCHAR(50), IN email VARCHAR(50))
  BEGIN
    INSERT INTO users (username, password, email) VALUES (username, password, email);
  END;
  ```
  W powyższym przykładzie, definiujemy nową procedurę ``add_user``, która przyjmuje trzy argumenty: ``username``, ``password`` i ``email``. Wewnątrz bloku kodu procedury używamy instrukcji ``INSERT``, aby dodać nowy wiersz do tabeli ``users``, korzystając z wartości argumentów jako danych.

  Słowo kluczowe ``IN`` w parametrze procedury oznacza, że parametr jest ``wejściowy`` - czyli, że wartość dla tego parametru będzie przekazywana do procedury z zewnątrz. Innymi słowy, jeśli definiujemy parametr ``IN`` w procedurze, musimy przekazać wartość dla tego parametru przy wywoływaniu procedury.

  Aby wywołać procedurę, należy użyć instrukcji ``CALL``
  
  > <div align="right"><sub>2.25 Listing - Wywołanie procedury</sub></div>
  ```sql
  CALL add_user('johnpaul', 'password123', 'john2@vatican.com');
  ```

  W tym przykładzie, wartości argumentów ``username``, ``password`` i ``email`` są przekazywane do procedury, która następnie dodaje nowego użytkownika do tabeli ``users``.

  ###### ❗ Ważne jest, aby pamiętać, że procedury muszą być tworzone w kontekście konkretnej bazy danych i mogą być wywoływane tylko w tej samej bazie danych, w której zostały utworzone.

  ### 🌟 Zadania do wykonania

  <details>
  <summary> 🔧 Zadania 1-10 </summary>

  1. Utwórz tabelę o nazwie ``users`` z kolumnami ``id`` (typ INT, klucz główny), ``username`` (typ VARCHAR) i ``email`` (typ VARCHAR).

      ```sql
      CREATE TABLE users (
        id ___ _______ ___,
        username _______(255),
        email _______(255)
      );
      ```

  1. Zmień nazwę kolumny ``username`` na ``user_name`` w tabeli ``users``.
  
  1. Dodaj nową kolumnę o nazwie ``age`` (typ INT) do tabeli ``users``.

  1. Utwórz nowy indeks o nazwie ``idx_email`` na kolumnie ``email`` w tabeli ``users``.

  1. Dodaj ograniczenie ``UNIQUE`` na kolumnie ``email`` w tabeli ``users``.

  1. Zmodyfikuj typ danych kolumny ``age`` z ``INT`` na ``BIGINT`` w tabeli ``users``.

  1. Dodaj klucz obcy o nazwie ``fk_user_id`` z kolumny ``id`` tabeli ``users`` do kolumny ``user_id`` tabeli ``posts``.

  1. Usuń indeks o nazwie ``idx_email`` z tabeli ``users``.

  1. Zmień nazwę tabeli ``posts`` na ``articles``.
  
  1. Usuń tabelę ``users``.

  </details>

  <details>
  <summary> 📷 Wyzwalacze i procedury 📜</summary>

  <details>
  <summary>Baza danych do wykonania zadania</summary>

  ```sql    
  CREATE DATABASE IF NOT EXISTS baza_do_zadan;
  USE baza_do_zadan;

  CREATE TABLE IF NOT EXISTS users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL,
    password VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
  );

  CREATE TABLE IF NOT EXISTS products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
  );

  CREATE TABLE IF NOT EXISTS orders (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    product_id INT NOT NULL,
    quantity INT NOT NULL,
    status ENUM('placed', 'shipped', 'delivered', 'cancelled') DEFAULT 'placed' NOT NULL,
    order_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE ON UPDATE CASCADE
  );

  CREATE TABLE IF NOT EXISTS reviews (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    product_id INT NOT NULL,
    rating INT NOT NULL,
    comment TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE ON UPDATE CASCADE
  );

  CREATE TABLE IF NOT EXISTS categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    description TEXT,
    parent_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (parent_id) REFERENCES categories(id) ON DELETE CASCADE ON UPDATE CASCADE
  );

  CREATE TABLE IF NOT EXISTS product_categories (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_id INT NOT NULL,
    category_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE ON UPDATE CASCADE,
    FOREIGN KEY (category_id) REFERENCES categories(id) ON DELETE CASCADE ON UPDATE CASCADE
  );

  CREATE TABLE IF NOT EXISTS images (
    id INT AUTO_INCREMENT PRIMARY KEY,
    filename VARCHAR(255) NOT NULL,
    filepath VARCHAR(255) NOT NULL,
    product_id INT NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE ON UPDATE CASCADE
  );

  CREATE TABLE IF NOT EXISTS sessions (
    id INT AUTO_INCREMENT PRIMARY KEY,
    user_id INT NOT NULL,
    token VARCHAR(255) NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE ON UPDATE CASCADE
  );
  ```

  </details>

  1. Aktualizuj stan magazynowy po dodaniu nowego zamówienia
  - 📜 Procedura:
    - Utwórz procedurę o nazwie ``update_stock`` z parametrami ``product_id`` i ``quantity``.
    
    - W ciele procedury, zaktualizuj stan magazynowy produktu o danym ``product_id`` poprzez odjęcie quantity od aktualnej wartości stanu magazynowego.
    
    - Zapisz procedurę.

  - 📷 Wyzwalacz:
    - Utwórz wyzwalacz o nazwie ``new_order_trg`` na tabeli ``orders``.
  
    - Ustaw wyzwalacz na reakcję na wstawienie nowego wiersza do tabeli ``orders``.
    
    - W akcji wyzwalacza, wywołaj procedurę ``update_stock`` z odpowiednimi wartościami parametrów ``product_id`` i ``quantity``.

  2. Usuń zamówienia starsze niż 30 dni
  - 📜 Procedura:

    - Utwórz procedurę o nazwie ``delete_old_orders``.

    - W ciele procedury, użyj polecenia ``DELETE`` w celu usunięcia zamówień, które zostały złożone więcej niż ``30 dni temu``.
    
    - Zapisz procedurę.

  - 📷 Wyzwalacz:

    - Utwórz wyzwalacz o nazwie ``delete_old_orders_trg`` na tabeli orders.
    
    - Ustaw wyzwalacz na reakcję na wstawienie, aktualizację lub usunięcie wiersza z tabeli ``orders``.
    
    - W akcji wyzwalacza, wywołaj procedurę ``delete_old_orders``.
  </details>

  ---
  
<div align='center'>
    
## Sushi bar 🍣

#### 🍣🍣🍣🍣🍣🍣

![image](https://github.com/TEB-DK/Vademecum_baz_danych/assets/125214141/b68e6d16-2bf8-4a72-8abc-5808f5939054)

</div>

#### **Polecenie (MySQL) 📑**
- Utwórz bazę danych na podstawie przedstawionego schematu za pomocą `DDL`, pamietając o zasadzie optymalizacji atrybutów.
- Następnie dokonaj wprowadzenia danych do bazy
    - Każda encji ma posiadać co najmniej 10 różnorodnych krotek
#### **Polecenie (PHP) 🐘**
- Utwórz prosty panel logowania składający się z pól do wprowadzenia loginu i hasła
    - Login pracownika będzie następujący: `[pierwsze_imie][id_pracownika]`
    - Hasło pracownika będzie następujące: `[nazwisko][data_zatrudnienia]`
- Po poprawnym zalogowaniu, użytkownik będzie przeniesiony na stronę panelu administracyjnego restauracji
    - `Panel_1` -> będzie składał się z prostej listy bloków zawierających informacje o rezerwacji i pracowniku, który do rezerwacji jest przydzielony.
    - `Panel_2` -> będzie składał się z listy zamówień do odpowiedniej rezerwacji, ma posiadać ten sam kolor co bloczek rezerwacji.
    - `Panel_3` -> będzie wyświetlał produkty, ich ceny i kategorie produktów, które są dostępne.
