# SQL-Uprawnienia

Zadanie 1
```
CREATE DATABASE Projekty;
USE Projekty;

CREATE TABLE Zadania (
    id INT PRIMARY KEY,
    nazwa VARCHAR(100),
    status VARCHAR(20)
);

```

1. Wykonaj następujące polecenia:

    Utwórz użytkownika:
    login: kierownik
    hasło: kier2025

2. Nadaj użytkownikowi kierownik uprawnienia:

    SELECT

    INSERT

    UPDATE
do tabeli Zadania, z możliwością dalszego przekazywania tych uprawnień (WITH GRANT OPTION).

3. Utwórz użytkownika:
login: asystent
hasło: asys2025

4. Zaloguj się jako kierownik i nadaj asystentowi takie same uprawnienia jak w punkcie 2, ale bez GRANT OPTION.

5. Zaloguj się jako asystent i sprawdź:

   czy możesz wykonać INSERT,

   czy możesz przekazywać uprawnienia dalej (powinien pojawić się błąd).

6. Odbierz użytkownikowi kierownik możliwość delegowania uprawnień (GRANT OPTION).

7. Sprawdź, co stało się z uprawnieniami użytkownika asystent (powinny zostać cofnięte).

```
-- Tworzenie użytkownika kierownik
CREATE USER 'kierownik'@'localhost' IDENTIFIED BY 'kier2025';

-- Nadanie uprawnień kierownikowi z możliwością dalszego delegowania
GRANT SELECT, INSERT, UPDATE 
ON Projekty.Zadania
TO 'kierownik'@'localhost'
WITH GRANT OPTION;

-- Tworzenie użytkownika asystent
CREATE USER 'asystent'@'localhost' IDENTIFIED BY 'asys2025';

-- Nadanie uprawnień asystentowi przez kierownika
GRANT SELECT, INSERT, UPDATE
ON Projekty.Zadania
TO 'asystent'@'localhost';

-- Test INSERT jako asystent (po zalogowaniu jako asystent)
INSERT INTO Projekty.Zadania (id, nazwa, status)
VALUES (1, 'Przygotowanie raportu', 'Nowe');

-- Próba nadania uprawnień przez asystenta (powinna zwrócić błąd)
GRANT SELECT ON Projekty.Zadania TO 'nowy_user'@'localhost';

-- Odebranie kierownikowi możliwości delegowania uprawnień
REVOKE GRANT OPTION ON Projekty.Zadania
FROM 'kierownik'@'localhost';

-- Sprawdzenie uprawnień asystenta (powinny zniknąć)
SHOW GRANTS FOR 'asystent'@'localhost';


```

Zadanie 2

```
CREATE DATABASE FinanseDB;
USE FinanseDB;

CREATE TABLE Finanse (
    id INT PRIMARY KEY,
    opis VARCHAR(100),
    kwota DECIMAL(10,2)
);


```

1. Wykonaj następujące polecenia:

2. Utwórz użytkownika:
    
    login: fin_admin
    
    hasło: fin2025

3. Nadaj użytkownikowi fin_admin uprawnienia do tabeli Finanse:

    SELECT
    
    INSERT
    
    UPDATE
    
    DELETE z możliwością dalszego delegowania (WITH GRANT OPTION).

4. Utwórz użytkownika:

    login: fin_viewer
    
    hasło: view2025

5. Zaloguj się jako fin_admin i nadaj użytkownikowi fin_viewer uprawnienie SELECT do tabeli Finanse.

6. Zaloguj się jako fin_viewer i sprawdź:

    czy możesz wykonać SELECT,
    
    czy możesz wykonać UPDATE (powinien pojawić się błąd).

7. Odbierz użytkownikowi fin_admin możliwość delegowania uprawnień.

8. Sprawdź, czy użytkownik fin_viewer nadal ma uprawnienie SELECT (powinno zostać cofnięte).

```
CREATE USER 'fin_admin'@'localhost' IDENTIFIED BY 'fin2025';

GRANT SELECT, INSERT, UPDATE, DELETE
ON FinanseDB.Finanse
TO 'fin_admin'@'localhost'
WITH GRANT OPTION;

CREATE USER 'fin_viewer'@'localhost' IDENTIFIED BY 'view2025';

GRANT SELECT
ON FinanseDB.Finanse
TO 'fin_viewer'@'localhost';

UPDATE FinanseDB.Finanse SET kwota = 100 WHERE id = 1;

REVOKE GRANT OPTION ON FinanseDB.Finanse
FROM 'fin_admin'@'localhost';

SHOW GRANTS FOR 'fin_viewer'@'localhost';


```

Zadanie 3.

```
CREATE DATABASE SklepDB;
USE SklepDB;

CREATE TABLE Produkty (
    id INT PRIMARY KEY,
    nazwa VARCHAR(50),
    cena DECIMAL(10,2)
);

CREATE TABLE Sprzedaz (
    id INT PRIMARY KEY,
    produkt_id INT,
    sztuk INT
);


```

1. Wykonaj następujące polecenia:

2. Utwórz użytkownika:

    login: ksiegowy
    
    hasło: ksieg2025

3. Nadaj użytkownikowi ksiegowy:

    SELECT do tabeli Produkty

    SELECT i INSERT do tabeli Sprzedaz
    z możliwością delegowania (WITH GRANT OPTION).

4. Utwórz użytkownika:
    
    login: praktykant
    
    hasło: prak2025

5. Zaloguj się jako ksiegowy i nadaj użytkownikowi praktykant uprawnienie SELECT do tabeli Produkty.

6. Zaloguj się jako praktykant i sprawdź:

    czy możesz wykonać SELECT z Produkty (powinno działać),
    
    czy możesz wykonać SELECT z Sprzedaz (błąd),
    
    czy możesz wykonać INSERT do Sprzedaz (błąd).

7. Odbierz użytkownikowi ksiegowy GRANT OPTION.

8. Sprawdź, czy użytkownik praktykant nadal ma dostęp (powinien go utracić).
