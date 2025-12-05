# SQL-Uprawnienia

Zadanie 1
```
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
do tabeli Zadania,
z możliwością dalszego przekazywania tych uprawnień (WITH GRANT OPTION).

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
