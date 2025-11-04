0.1 Wyświetl wszystkie informacje użytkownika Alice.

curl -X 'GET' 'http://127.0.0.1:5000/user/alice' -H accept:'*/*' | jq

![[Pasted image 20251104195338.png]]

0.2 Wyświetl imię i nazwisko użytkownika Alice. 

curl -X 'GET' 'http://127.0.0.1:5000/user/alice' -H accept:'*/*' | jq -r ".name"

![[Pasted image 20251104195527.png]]

0.3 Wyświetl adres e-mail użytkownika Bob. 

curl -X 'GET' 'http://127.0.0.1:5000/user/bob' -H accept:'*/*' | jq -r ".email"

![[Pasted image 20251104195604.png]]

0.4 Wyświetl wiek użytkownika Alice. 

curl -X 'GET' 'http://127.0.0.1:5000/user/alice' -H accept:'*/*' | jq -r ".age"

![[Pasted image 20251104195628.png]]

0.5 Wyświetl nazwę miasta, w którym mieszka użytkownik Bob. 

curl -X 'GET' 'http://127.0.0.1:5000/user/bob' -H accept:'*/*' | jq -r ".city"

![[Pasted image 20251104195649.png]]

0.6 Wyświetl hobby użytkownika Alice. 

0.7 Sprawdź, czy jednym z hobby użytkownika Bob są gry.

0.8 Wyświetl pierwsze hobby użytkownika Alice. 

0.9 Sprawdź, ile hobby ma użytkownik Bob, a ile użytkownik Alice. 

0.10 Wyświetl nazwę użytkownika i miasto jako jeden ciąg znaków, np. "Alice Smith (Warsaw)". 
Zmodyfikuj polecenie, aby tekst był wyświetlany jako "Alice Smith from Warsaw". 

0.11 Wyświetl wszystkie przedmioty. Wyświetl nazwy wszystkich przedmiotów.

0.12 Wyświetl przedmioty droższe niż 20 pln. 

0.13 Wyświetl przedmioty tańsze niż 30 pln. 

0.14 Posortuj przedmioty według ceny rosnąco, a następnie malejąco. 

0.15 Pobierz sumę cen wszystkich przedmiotów. 

0.16 Wyświetl przedmioty, których nazwa zawiera "item". 

0.17 Wyświetl przedmioty w przedziale cenowym 10-30 pln. 

0.18 Wyświetl użytkowników, którzy mają więcej niż 2 hobby. 

0.19 Wyślij do serwera request typu POST (endpoint /echo) i wyświetl odpowiedź. 

0.20 Wyślij do serwera request typu POST, który doda nowy przedmiot (użyj endpointa /items). Wyświetl wszystkie przedmioty po dodaniu. 

0.21 Za pomocą requestu typu PUT, aktualizuj cenę przedmiotu o numerze 1. Wyświetl wszystkie przedmioty. 

0.22 Za pomocą requestu typu DELETE, usuń przedmiot o numerze 3. Wyświetl wszystkie przedmioty. 

0.23 Wyświetl przedmiot o największej cenie. 

0.24 Wyświetl wszystkie przedmioty z ceną podwyższoną o 10%. 

0.25 Pobierz nazwy wszystkich użytkowników i ich wiek. 

0.26 Wyświetl hobby Alice jako string połączony przecinkami. 

0.27 Pobierz pierwszy i ostatni przedmiot z listy. 

0.28 Sprawdź, czy są przedmioty droższe niż 100 pln. 

0.29 Utwórz nowy przedmiot za pomocą POST /items. Wyświetl odpowiedź serwera i następnie listę wszystkich przedmiotów. 

0.30 Zaktualizuj przedmiot id=2. Wyświetl odpowiedź serwera i następnie listę wszystkich przedmiotów.