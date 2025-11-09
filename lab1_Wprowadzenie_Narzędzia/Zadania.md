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

![[Pasted image 20251109143041.png]]

0.7 Sprawdź, czy jednym z hobby użytkownika Bob są gry.

![[Pasted image 20251109143556.png]]

0.8 Wyświetl pierwsze hobby użytkownika Alice. 

![[Pasted image 20251109143727.png]]

0.9 Sprawdź, ile hobby ma użytkownik Bob, a ile użytkownik Alice. 

![[Pasted image 20251109144214.png]]
![[Pasted image 20251109144133.png]]

0.10 Wyświetl nazwę użytkownika i miasto jako jeden ciąg znaków, np. "Alice Smith (Warsaw)". 
Zmodyfikuj polecenie, aby tekst był wyświetlany jako "Alice Smith from Warsaw". 

![[Pasted image 20251109144543.png]]
![[Pasted image 20251109144616.png]]

0.11 Wyświetl wszystkie przedmioty. Wyświetl nazwy wszystkich przedmiotów.

![[Pasted image 20251109144737.png]]
![[Pasted image 20251109144849.png]]

![[Pasted image 20251109144927.png]]

0.12 Wyświetl przedmioty droższe niż 20 pln. 

![[Pasted image 20251109145201.png]]

0.13 Wyświetl przedmioty tańsze niż 30 pln. 

![[Pasted image 20251109145225.png]]

0.14 Posortuj przedmioty według ceny rosnąco, a następnie malejąco. 

![[Pasted image 20251109145506.png]]
![[Pasted image 20251109145704.png]]

0.15 Pobierz sumę cen wszystkich przedmiotów. 

![[Pasted image 20251109150051.png]]

0.16 Wyświetl przedmioty, których nazwa zawiera "item". 

![[Pasted image 20251109150251.png]]

0.17 Wyświetl przedmioty w przedziale cenowym 10-30 pln. 

![[Pasted image 20251109150432.png]]

0.18 Wyświetl użytkowników, którzy mają więcej niż 2 hobby. 

0.19 Wyślij do serwera request typu POST (endpoint /echo) i wyświetl odpowiedź. 

![[Pasted image 20251109152835.png]]

0.20 Wyślij do serwera request typu POST, który doda nowy przedmiot (użyj endpointa /items). Wyświetl wszystkie przedmioty po dodaniu. 

![[Pasted image 20251109153224.png]]
![[Pasted image 20251109153239.png]]

0.21 Za pomocą requestu typu PUT, aktualizuj cenę przedmiotu o numerze 1. Wyświetl wszystkie przedmioty. 

![[Pasted image 20251109153520.png]]
![[Pasted image 20251109153534.png]]

0.22 Za pomocą requestu typu DELETE, usuń przedmiot o numerze 3. Wyświetl wszystkie przedmioty. 

![[Pasted image 20251109153628.png]]

0.23 Wyświetl przedmiot o największej cenie. 

![[Pasted image 20251109153946.png]]

0.24 Wyświetl wszystkie przedmioty z ceną podwyższoną o 10%. 

![[Pasted image 20251109154348.png]]

0.25 Pobierz nazwy wszystkich użytkowników i ich wiek. 

0.26 Wyświetl hobby Alice jako string połączony przecinkami. 

![[Pasted image 20251109154543.png]]

0.27 Pobierz pierwszy i ostatni przedmiot z listy. 

![[Pasted image 20251109154714.png]]

0.28 Sprawdź, czy są przedmioty droższe niż 100 pln. 

![[Pasted image 20251109154757.png]]

0.30 Zaktualizuj przedmiot id=2. Wyświetl odpowiedź serwera i następnie listę wszystkich przedmiotów.

![[Pasted image 20251109155409.png]]