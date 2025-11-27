Flaga **`-r`** (czyli `--raw-output`) w `jq` **usuwa cudzysłowy z wyniku** i wypisuje czysty tekst zamiast JSON-owego stringa.

---

### 🔍 Porównanie:

#### Bez `-r`

```bash
curl -s http://localhost:5000/user/alice | jq '.hobbies[0]'
```

📘 Wynik:

```
"reading"
```

→ `jq` zwraca poprawny JSON (czyli string z cudzysłowami `"..."`).

---

#### Z `-r`

```bash
curl -s http://localhost:5000/user/alice | jq -r '.hobbies[0]'
```

📘 Wynik:

```
reading
```

→ `-r` usuwa cudzysłowy i wypisuje czysty tekst (czyli tzw. raw output).

---

### 💡 Po co to się przydaje?

- Gdy chcesz używać wyniku w **skryptach bashowych** (np. przypisać do zmiennej, porównać, przekazać dalej).
    
- Gdy chcesz po prostu ładnie wyświetlić wartość, a nie pełny JSON.
    

---

### 🧠 W skrócie:

|Opcja|Co robi|Przykład wyniku|
|---|---|---|
|`jq`|zwraca JSON|`"reading"`|
|`jq -r`|zwraca tekst|`reading`|

---


## Zadania
---

0.2 Wyświetl imię i nazwisko użytkownika Alice.

curl -X 'GET' '[http://127.0.0.1:5000/user/alice'](http://127.0.0.1:5000/user/alice') -H accept:'_/_' | jq -r ".name"

---

0.7 Sprawdź, czy jednym z hobby użytkownika Bob są gry.

curl -X 'GET' '[http://127.0.0.1:5000/user/bob'] -H accept:'_/_' | jq -r 'any(.hobbies[]; . == "games")'

---

0.8 Wyświetl pierwsze hobby użytkownika Alice.

curl -X 'GET' '[http://127.0.0.1:5000/user/alice'] -H accept:'_/_' | jq -r .hobbies[0]

---

0.9 Sprawdź, ile hobby ma użytkownik Bob.

curl -X 'GET' '[http://127.0.0.1:5000/user/bob'] -H accept:'_/_' | jq -r '.hobbies | length'

---

0.10 Wyświetl nazwę użytkownika i miasto jako jeden ciąg znaków, np. "Alice Smith (Warsaw)".  
Zmodyfikuj polecenie, aby tekst był wyświetlany jako "Alice Smith from Warsaw".

curl -X 'GET' '[http://127.0.0.1:5000/user/alice'] -H accept:'_/_' | jq -r '"\\(.name) (\\(.city))"'

curl -X 'GET' '[http://127.0.0.1:5000/user/alice'] -H accept:'_/_' | jq -r '"\\(.name) from \\(.city)"'

---

0.12 Wyświetl przedmioty droższe niż 20 pln.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r '.[] | select(.price > 20.00)'

---

0.14 Posortuj przedmioty według ceny rosnąco, a następnie malejąco.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r 'sort_by(.price)'

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r 'sort_by(.price) | reverse'

---

0.15 Pobierz sumę cen wszystkich przedmiotów.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r 'map(.price) | add'

---

0.16 Wyświetl przedmioty, których nazwa zawiera "item".

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r '.[] | select(.name | test("item))'

---

0.17 Wyświetl przedmioty w przedziale cenowym 10-30 pln.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r 'map(select(.price >= 10.00 and .price <= 30.00))'

---

0.23 Wyświetl przedmiot o największej cenie.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r 'sort_by(.price) | reverse | .[0]'

---

0.24 Wyświetl wszystkie przedmioty z ceną podwyższoną o 10%.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r '.[] | .price=(.price*1.1)'

---

0.26 Wyświetl hobby Alice jako string połączony przecinkami.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r '.hobbies | join(", ")'

---

0.27 Pobierz pierwszy i ostatni przedmiot z listy.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r '.[0], .[-1]'

---

0.28 Sprawdź, czy są przedmioty droższe niż 100 pln.

curl -X 'GET' '[http://127.0.0.1:5000/items'] -H accept:'_/_' | jq -r '.[] | (.price > 100)'