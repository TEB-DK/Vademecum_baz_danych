<div align="center"><h1>📝 Zapytania SQL (DML) 📝</h1></div>

  ## 🧠 Przypomnienie

  ### Język manipulacji danych (ang. ``Data Manipulation Language``)
  > zbiór instrukcji języka zapytań używanych do przetwarzania danych z bazy danych. Są to instrukcje takie jak: ``SELECT``, ``INSERT``, ``UPDATE``, ``DELETE``.

  > <div align="right"><sub>1.1 Listing - Przykładowa składnia instrukcji SELECT</sub></div>
  ```sql
  SELECT atrybut FROM nazwa_tabeli  
  WHERE [warunki_wyszukiwania] 
  ORDER BY atrybut [ASC / DESC], 
  LIMIT ilość_wierszy;
  ```

  > <div align="right"><sub>1.2 Listing - Przykładowa składnia instrukcji INSERT</sub></div>
  ```sql
  INSERT INTO nazwa_tabeli VALUES ('Przykład', 69); 
  INSERT INTO nazwa_tabeli (atrybut, atrybut...) VALUES ('Przykład', 69); 
  ```

  > <div align="right"><sub>1.3 Listing - Przykładowa składnia instrukcji UPDATE</sub></div>
  ```sql
  UPDATE nazwa_tabeli SET atrybut = 'nowa_wartość' 
  WHERE [warunek_wyszukania];
  ```

  > <div align="right"><sub>1.4 Listing - Przykładowa składnia instrukcji DELETE</sub></div>
  ```sql
  DELETE FROM nazwa_tabeli WHERE [warunek_wyszukania];
  ```
  
---

  ## 📽️ Zapytania ``SELECT``
  > Zapytanie SELECT jest wykorzystywane do wybierania (``projekcji``) danych z bazy. Z pozoru łatwe zagadnienie zapytania może jednak przysporzyć wiele kłopotów jeśli zgłebimy je dokładnie, szczególnie używając ``podzapytań SELECT``, ``złożonych warunków filtrowania WHERE``, czy chociażby funkcji obliczeniowych - grupujących (agregujących), takich jak ``COUNT``,``AVG``,``SUM``,``MIN``,``MAX``.

  #### ORDER BY
  > Słowo kluczowe (syntaktyczne) wykorzystywane do sortowania wyników rosnąco lub malejąco na podstawie konkretnego atrybutu tabeli.
  
  > <div align="right"><sub>1.4 Listing - Przykładowe sortowanie wyników rosnąco i malejąco</sub></div>

  ```sql
  SELECT FirstName FROM Employees ORDER BY EmployeeID ASC

  SELECT FirstName FROM Employees ORDER BY EmployeeID DESC
  ```
  ``ASC`` - Sortowanie rosnące (z ang. ``ascending``)

  ``DESC`` - Sortowanie malejące (z ang. ``descending``)

  Listing 1.4 przedstawia wybranie atrybutu ``FirstName`` z tabeli ``Employees`` sortując je najpierw rosnąco po atrybucie ``EmployeeID``. Sortowania można dokonać po przez dowolny atrybut, nawet typu ``VARCHAR``.

  #### DISTINCT
  > Polecenie następujące po ``SELECT`` oznajmiające w zapytaniu, aby zwróciło tylko różniące się od siebie wyniki, innymi słowy redukuje ilość wyników do takich, które się nie powtarzają.

  > <div align="right"><sub>1.5 Listing - Przykładowe zastosowanie DISTINCT</sub></div>
  ```sql
  SELECT OrderID FROM OrderDetails
  ```
  ![Bez distinct](https://user-images.githubusercontent.com/125214141/221034358-0e52ab3b-7ac1-460f-b226-99b9e0cc8c1e.png)


  ```sql
  SELECT DISTINCT OrderID FROM OrderDetails
  ```
  ![Z distinct](https://user-images.githubusercontent.com/125214141/221034552-62c2b5b0-27bc-425e-840d-2a990632c07a.png)

  #### NOT, AND i OR, IN
  > Operatory służące głównie do klauzuli ``WHERE``, dzięki którym możliwe jest zdefiniowanie więcej niż jednego warunku wyszukiwania.
  
  > <div align="right"><sub>1.6 Listing - Zastosowanie operatora AND i OR</sub></div>
  ```sql
  SELECT * FROM Customers 
  WHERE Country = "Germany" AND City = "Berlin" OR City = "Aachen"
  ```

  W wolnym tłumaczeniu możemy przeczytać warunek logiczny wyszukiwania w ten sposób: Znajdz wszystkich klientów ``GDZIE ich kraj to Germany I miasto to Berlin ORAZ miasto to Aachen``.

  > <div align="right"><sub>1.7 Listing - Zastosowanie operatora NOT</sub></div>
  ```sql
  SELECT * FROM Customers
  WHERE NOT Country = "UK"
  ```

  Natomiast operator ``NOT`` powoduje wykluczenie ze zbioru pasujących elementów jednego konkretnego elementu, bądź całej "listy" elementów, którą można zastosować za pomocą operatora ``IN``.

  Jak możemy zauważyć na listingach 1.6 oraz 1.7, operatory logiczne są całkiem elastyczne jeśli chodzi o zastosowanie, niektóre z tych logik możemy zastosować za pomocą operatora ``IN``. Na przykład listing 1.6 możemy zapisać w ten sposób:

  > <div align="right"><sub>1.8 Listing - Wykorzystanie operatora IN</sub></div>
  ```sql
  SELECT * FROM Customers
  WHERE Country = "Germany" AND City IN ("Berlin", "Aachen")
  ```

  Operator ten wykorzystywany jest do zastosowania listy elementów, które można albo uwzględnić w wyszukiwaniu, albo je wykluczyć.
  
  > <div align="right"><sub>1.9 Listing - Wykorzystanie operatora NOT wraz z operatorem IN</sub></div>
  ```sql
  SELECT * FROM Customers
  WHERE Country = "Germany" AND City NOT IN ("Berlin", "Aachen")
  ```
  
  Zapytanie zwróci nam wszystkich klientów z kraju Germany oraz miast wszystkich poza Berlinem oraz Aachen.

  #### GROUP BY
  > Deklaracja oznaczająca grupowanie krotek z tym samym typem wartości w podsumowujący wiersz. Często również słowo kluczowe ``GROUP BY`` wykorzystywane jest do funkcji agregujących takich jak ``COUNT``,``MAX``,``MIN``,``SUM`` czy ``AVG``.

  Przykładowym zastosowaniem GROUP BY jest grupowanie konkretnej kolumny wzgledem drugiej. Na przykład: Zliczenie ilości zamówień danego dnia, by to uczynić musimy użyć funkcji COUNT oraz GROUP BY. Projekcja wyników będzie sensowna tylko wtedy gdy uwzględnimy liczbę zamówień oraz datę.
  > <div align="right"><sub>1.10 Listing - Wykorzystanie GROUP BY wraz z funkcją agregującą COUNT</sub></div>
  ```sql
  SELECT COUNT(OrderID), OrderDate FROM Orders 
  GROUP BY OrderDate
  ```
  Wyniki są pogrupowane względem daty zamówienia, więc funkcja COUNT będzie oznaczać jedynie zsumowanie ilości ID do danego dnia, co po przetłumaczeniu na normalny język będzie znaczyło: Danego dnia ilość zamówień wynosiła [tutaj wartość z COUNT(OrderID)].

  Aby nasza projekcja była bardziej czytelna możemy zastosować ``alias``, deklaracją ``as`` zaraz po atrybucie.

  > <div align="right"><sub>1.11 Listing - Zastosowanie aliasu w zapytaniu agregującym</sub></div>
  ```sql
  SELECT 
  COUNT(OrderID) as Ilosc_zamowien, 
  OrderDate as Data_zamowienia
  FROM Orders 
  GROUP BY Data_zamowienia
  ```

  Dzięki zastosowaniu ``aliasu`` czyli nazwy zastępczej możemy również wykorzystać ją w dowolnym miejscu zapytania, tak jak ma to miejsce w ``GROUP BY``. Nazwy atrybutów przyjmują wtedy nazwy aliasowe więc i wykorzystanie tych nazw jest jak najbardziej poprawne.


  #### COUNT, AVG, SUM
  > Zbiór funkcji agregujących.

  - ``COUNT`` - Zliczanie wszystkich krotek danego atrybutu.

    > <div align="right"><sub>1.12 Listing - Użycie funkcji COUNT do zliczenia wszystkich krotek atrybutu Price</sub></div>
    ```sql
    SELECT COUNT(Price) FROM Products
    ```

  - ``AVG`` - Zliczenie średniej wartości krotek atrybutu numerycznego.

    > <div align="right"><sub>1.13 Listing - Użycie funkcji AVG do wyliczenia średniej wartości ceny produktu ze wszystkich krotek</sub></div>
    ```sql
    SELECT AVG(Price) FROM Products
    ```

  - ``SUM`` - Zliczenie sumy krotek atrybutu numerycznego.
    
    > <div align="right"><sub>1.14 Listing - Użycie funkcji SUM do wyliczenia sumy całej kolumny Price</sub></div>
    ```sql
    SELECT SUM(Price) FROM Products      
    ```


  #### BETWEEN
  > Operator zakresu, pozwala na wybranie wartości ``pomiędzy`` zadeklarowanymi.

  Wykorzystwanie operatora zakresu następuje w polu warunku wyszukiwania. Wartości zakresu mogą być reprezentowane przez ``wartości liczbowe``, ``tekstowe`` lub ``wartości daty``.

  > <div align="right"><sub>1.15 Listing - Wyświetlenie nazwy produktów gdzie id kategorii jest z zakresu [2,5]</sub></div>
  ```sql
  SELECT ProductName FROM Products 
  WHERE CategoryID BETWEEN 2 AND 5
  ```

  #### LIKE
  > Operator podobieństwa, stosowany w klauzuli ``WHERE`` i wykorzystywany do wyszukiwania po przez określony schemat w atrybucie.
  
  > <div align="right"><sub>1.16 Listing - Symbole schematu operatora</sub></div>
  | Symbol schematu operatora | Wyjaśnienie |
  |:---:|:---:|
  | % | Przedstawia zero lub więcej znaków |
  | _ | Przedstawia dokładnie jeden dowolny znak |

  > <div align="right"><sub>1.17 Listing - Przykładowe zastosowanie schematów operatora LIKE</sub></div>
  | Schemat operatora | Wyjaśnienie |
  |:---:  |:---:      |
  |"tekst%" | Wyszukanie wszystkich wartości ``zaczynających`` się od "tekst"|
  |"%tekst" | Wyszukanie wszystkich wartości ``kończących`` się na "tekst" |
  |"tekst%xd" | Wyszukanie wszystkich wartości ``zaczynających`` się na "tekst" i ``kończących`` na "xd" |
  |"%ess%"| Wyszukanie wszystkich wartości, które posiadają "ess" w ``dowolnym`` miejscu |
  |"____era% | Wyszukanie wszystkich wartości, które posiadają "era" na 5 pozycji, słowem pasującym będzie np. "chillera" |
  |"O_az%" | Wyszukanie wszystkich wartości, które zaczynają się na wyraz O_az z jedną dowolną literą na drugim miejscu, wynikami wyszukiwania będą na np. "Okaz", a także i "Oraz".
  |"P_%_%" | Wyszukanie słowa, które zaczyna się na ``P`` i jest conajmniej ``3 literowe``, wynikami wyszukiwania mogą być np. Piwo, Paw"

  Zastosowanie ``LIKE`` jest zazwyczaj związane z wyszukiwaniem konkretnego schematu tekstu, np. zaczynającego się na konkretne litery, bądź literę.

  > <div align="right"><sub>1.18 Listing - Przykładowa składnia z wykorzystaniem operatora LIKE</sub></div>
  ```sql
  SELECT * FROM Customers
  WHERE City LIKE "L_%_%_%_%_%"
  ```

  Na listingu 1.18 zaprezentowany został przykład wyszukujący klientów których nazwa miasta zaczyna się na literę ``L`` i jest conajmniej 6 literowa (5 dowolnych i zaczynająca się na L).


  #### MIN i MAX
  > Zbiór funkcji wybierających najmniejszą oraz największą wartość z kolumny.
  
  - ``MIN`` - funkcja zwracająca najmniejszą wartość w kolumnie.
    
    > <div align="right"><sub>1.19 Listing - Przykładowe użycie funkcji MIN</sub></div>
    ```sql
    SELECT MIN(Quantity) FROM OrderDetails      
    ```
  
  - ``MAX`` - funkcja zwracająca największą wartość w kolumnie.
    
    > <div align="right"><sub>1.20 Listing - Przykładowe użycie funkcji MAX</sub></div>
    ```sql
    SELECT 
    MAX(Quantity) as Najwieksza_ilosc,
    ProductID 
    FROM OrderDetails
    GROUP BY ProductID ORDER BY Najwieksza_ilosc DESC LIMIT 1     
    ```
  

  #### LIMIT
  > Klauzula ograniczająca ilość wyników do projekcji. Szczególnie użyteczne dla dużych encji z tysiącami krotek, które są wybierane przez użytkowników.
  
  Zastosowanie tej klauzuli jest umiejscowione zawsze na samym końcu zapytania.

  > <div align="right"><sub>1.21 Listing - Wykorzystanie klauzuli LIMIT w celu ograniczenia ilości wyników</sub></div>
  ```sql
  SELECT * FROM OrderDetails LIMIT 10
  ```


  ### 🌟 Zadania do wykonania
  > Do wykonania tego poddziału wykorzystamy gotową bazę do nauki W3Schools, link poniżej.

  [ 🔗 Przenieś mnie do krainy MySQL!](https://www.w3schools.com/mysql/trymysql.asp?filename=trysql_select_all)
  
  1. Wybierz wszystkie krotki z tabeli ``Customers``, którzy są z miasta ``Walla``.
        
     - Zmodyfikuj powyższy podpunkt w następujący sposób: 

       - Wybierz ``imiona klientów`` oraz ich ``adresy``.
       - Zamień wyszukiwanie przez miasto, na wyszukiwanie przez kraj i wyszukaj klientów zamieszkujących ``Venezuele``.
       - Posortuj to wyszukiwanie malejąco.
       - Zlicz wszystkich klientów zamieszkujących ``Venezuele`` za pomocą funkcji ``COUNT``.

  2. Wybierz wszystkie ``ID zamówień`` w tabeli ``OrderDetails``, których ilość przekracza ``21``.
     
     - Zmodyfikuj powyższy podpunkt w następujący sposób:

       - Dodaj górne ograniczenie ilości w postaci nie przekraczania liczby ``37``.
       - Wybierz tylko ``nie powtarzające`` się zamówienia.
       
  3. Wyszukaj nazwę produktu, jednostkę oraz cene w tabeli ``Products``, których cena przekracza średnią cene wszystkich produktów. (W przykładzie występuje podzapytanie).
     
     - Zmodyfikuj powyższy podpunkt w następujący sposób:

       - Dodatkowo muszą być to produkty, których jednostka to ``bottles`` (butelki) lub ``bags`` (worki).
  
  4. Zlicz liczbę zamówień dla każdego klienta w tabeli ``Orders``, wyniki posortuj malejąco względem liczby zamówień dokonanych przez klienta.

  ### Podzapytania
  > Podzapytanie to zapytanie SQL, które umieszczone jest wewnątrz innego zapytania. Podzapytanie zawsze otoczone jest parą nawiasów ().

  > Podzapytanie może występować praktycznie wszędzie wewnątrz zapytania SQL. To gdzie podzapytanie może być użyte uzależnione jest od tego ile wartości zwraca. Jeśli podzapytanie zwraca pojedynczą wartość może być użyte jako część wyrażenia – na przykład w porównaniach, czy zwracanych atrybutach.




  ## Zapytania ``INSERT``
  > in progress


  ## Zapytania ``UPDATE``
  > in progress



  ## Zapytania ``DELETE``
  > in progress
