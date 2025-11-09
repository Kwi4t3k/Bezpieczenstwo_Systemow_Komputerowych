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
