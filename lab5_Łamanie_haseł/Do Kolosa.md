Usuwanie wszystkich kontenerów na raz (usuwa działające i zatrzymane)
```
podman rm -f $(podman ps -aq)
```


# 🔥 **HASHCAT RULES — PEŁNA ŚCIĄGA**

Plik `.rule` składa się z **linijek**, gdzie każda linia to **jedna transformacja** lub **kombinacja transformacji**.

PRZED $ POWINNO BYĆ \
```
\$_
```
to nam da znak _ na końcu wyrazu

---

# 🟦 **1. Zmiany wielkości liter**

|Akcja|Reguła|Przykład|
|---|---|---|
|Pierwsza litera DUŻA|`T0`|`ania → Ania`|
|Pierwsza litera mała|`t0`|`ANIA → aNIA`|
|Ostatnia DUŻA|`T$`|`adam → adaM`|
|Ostatnia mała|`t$`|`ADA → ADa`|
|Wszystkie DUŻE|`TU`|`ania → ANIA`|
|Wszystkie małe|`TL`|`ANIA → ania`|
|Zamień litery na przemian|`Ta`|`test → TeSt`|

---

# 🟩 **2. Zamiana znaków**

Format: `sXY`

- `X` = co zamienić
    
- `Y` = na co zamienić
    

|Akcja|Reguła|Przykład|
|---|---|---|
|a → 0|`sa0`|`ania → 0ni0`|
|e → 3|`se3`|`ela → 3l0`|
|i → 1|`si1`|`iza → 1z0`|
|o → @|`so@`|`ola → @l0`|

**Zamiana tylko pierwszego wystąpienia:**

`rXY`  
(r = replace only first)

Przykład:

```
rza0     # zamień pierwsze 'a' na '0'
```

**Zamiana tylko ostatniego wystąpienia:**

```
lza0
```

---

# 🟧 **3. Dodawanie znaków**

### **Na początku**

```
^X      → dodaje X z przodu
```

Przykład:

```
^1   # 1admin
```

### **Na końcu**

```
$X      → dodaje X na końcu
```

Przykład:

```
$!   # admin!
```

### **Dodaj ciąg znaków**

Można łączyć:

```
^pass$123
```

---

# 🟥 **4. Usuwanie znaków**

|Akcja|Reguła|
|---|---|
|Usuń pierwszy znak|`D0`|
|Usuń drugi znak|`D1`|
|Usuń ostatni znak|`D$`|
|Skróć do długości N|`]N` (np. `]5`)|

Przykład:

```
D0    # 'ania' → 'nia'
D$    # 'ania' → 'ani'
```

---

# 🟪 **5. Odwracanie / duplikowanie**

|Akcja|Reguła|Przykład|
|---|---|---|
|Odwróć|`r`|`ania → aina`|
|Powtórz słowo|`pX`|`p1: ania → aniaania`|
|Duplikuj pierwszą literę|`d`|`ania → aania`|

---

# 🟨 **6. Wstawianie znaków**

Format:

```
iXY
```

| X = pozycja (0 = początek)  
| Y = znak do wstawienia |

Przykład:

```
i0!        # !ania
i1@        # a@nia
i$!        # ania!
```

---

# 🔵 **7. Przykłady złożonych reguł**

### **Leet speak + pierwsza litera DUŻA**

```
T0 sa0 se3 si1 so0
```

### **Dodanie roku na końcu + duża pierwsza litera**

```
T0 $2 $0 $2 $3   # dodaje "2023"
```

### **Imię + ! + liczba na końcu**

```
T0 $! $1
```

### **Pierwsza DUŻA + zamiany leet + wykrzyknik**

```
T0 sa0 se3 si1 $!
```

---

# 🟣 **8. Jak używać pliku z zasadami**

Tworzysz plik:

```
nano moje.rule
```

Wklejasz np.:

```
T0 sa0 se3 si1
T0 sa0
sa0 se3 si1 $!
```

Uruchamiasz:

```
hashcat -m 1400 -a 0 hash.txt wordlist.txt -r moje.rule
```

---

# 🔥 **9. Świetne techniki do zdań typu 4.18 (imiona → leet)**

**Plik: leet-polish.rule**

```
T0
T0sa0
T0se3
T0si1
T0sa0se3si1
T0sa0se3
T0sa0si1
T0se3si1
```

Daje np.:

|Wejście|Wyjście|
|---|---|
|ania|Ania, 0ni0, An10, 0n1@|
|iza|Iza, 1z0, 1z@|

---

# 🟢 **10. Najważniejsze skróty (mini-cheat sheet)**

```
⬆ Pierwsza duża:        T0
⬇ Pierwsza mała:        t0
Zmiana a→0:             sa0
Zmiana e→3:             se3
Zmiana i→1:             si1
Dodaj X na końcu:       $X
Dodaj X na początku:    ^X
Usuń pierwszy znak:     D0
Usuń ostatni znak:      D$
Odwróć:                 r
Wstaw X na pozycji n:   inX
Duplikuj słowo:         p1
```

