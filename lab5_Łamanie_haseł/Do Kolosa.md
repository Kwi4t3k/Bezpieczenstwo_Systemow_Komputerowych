## Przydatne curle
curl -X POST 'http://localhost:port/submit' -H 'accept: appplication/json' -H 'Content-Type: application/json' -d '{"word":"odkodowane_słowo"}'

## Przydatne komendy
man crunch
head -n 5 wordlist.txt

## Usuwanie wszystkich kontenerów na raz (usuwa działające i zatrzymane)
podman rm -f $(podman ps -aq)

## Rodzaje hashy z labów `po -m`
- MD5 -> 0
- SHA-1 -> 100
- SHA-256 -> 1400
- SHA-521 -> 1700
- bcrypt -> 3200

---
### hashcat - rodzaje ataków
hashcat -m rodzaj_hasha -a 0 hash.txt wordlist.txt -> podstawowy atak słownikowy

hashcat -m rodzaj_hasha -a 3 hash.txt wordlist.txt -> brute-force + maska

hashcat -m rodzaj_hasha -a 6 hash.txt wordlist.txt ?d?d -> słownik + maska

hashcat -m rodzaj_hasha -a 7 hash.txt -1 '\!\@\#\$\%\&\*' ?l?d?1 wordlist.txt -> maska + słownik

### hashcat sprawdzenie wyniku
hashcat -m rodzaj_hasha hash.txt --show

### crunch - generowanie słownika
crunch 3 3 0123456789 -o slownik.txt `3 znaki, każdy jest cyfrą od 0 do 9 (-a 0)`

crunch 4 4 ABCDEFGHIJKLMNOPQRSTUVWXYZ -o slownik.txt `4 znaki, każdy jest wielką literą (-a 0)`

crunch 8 8 -t pass%%%% -o wordlist.txt `słowo pass z sufiksem: 4 cyfry 0-9, czyli np. pass0001 (-a 3)`

crunch 1 3 -o wordlist.txt -p admin password 123 `permutacje słów admin, password i 123, czyli np. 123adminpassword, admin123password (-a 0)`

crunch 5 5 -t ,@%^^ -o wordlist.txt  `5 znaków -> wielka litera, mała litera, 0-9, 4 i 5 znak to znaki specjalne np. !,@,#,$ itp.`

### hashcat - bruteforce
hashcat -m rodzaj_hasha -a 3 hash.txt ?l?l?l?l `4 znaki -> a-z`

hashcat -m rodzaj_hasha -a 3 hash.txt ?u?d?d?d `4 znaki -> pierwszy A-Z, reszta 0-9`

hashcat -m rodzaj_hasha -a 3 hash.txt -1 abc -2 468 -3 \*%: '?1?2?3' `3 znaki -> pierwszy {a,b,c}, drugi {4,6,8}, trzeci {*,%,:}`

hashcat -m rodzaj_hasha -a 3 hash.txt '00:14:22:ff:ff:?h:?h' `MAC zaczynający się 00:14:22:ff:ff:xx`

hashcat -m rodzaj_hasha -a 6 hash.txt wordlist.txt ?d?d `to słowo ze słownika, do którego dodano dwie cyfry(0-9) na jego końcu`

hashcat -m rodzaj_hasha -a 7 hash.txt -1 '\!\@\#\$\%\&\*' ?l?d?1 wordlist.txt `to słowo ze słownika, do którego dodano na początku literę(a-z), cyfrę(0-9) i znak specjalny(!,@,#,$,%,&,*)`

### plik rule.txt do tworzenia własnych zasad
1. hasło ze słowami z pliku przekształcone jako (a->@, e->3, i->1)

sa@se3si1  
tłumaczenie:
- s - do zamiany
- sa@ - zamień 'a' na '@'
- se3 - zamień 'e' na '3'
- si1 - zamień 'i' na '1'

echo -n "sa@se3si1" > rule.txt
###### ZŁAMANIE HASŁA \\/
hashcat -m rodzaj_hasha -a 0 hash.txt wordlist.txt -r rule.txt

2. hasło ze słowami z pliku przekształcone jako (pierwsza litera wielka, zamienione litert a->4 e->3 o->0, na końcu słowa dodany znak \_ bieżący rok i znak \!)

echo -n "T0 sa4 se3 so0 \\$\_ \\\$2 \\\$0 \\\$2 \\\$5 \\\$!" > rule.txt

3. hasło ze słowami z dodanym na początku \! i \# na końcu

echo -n "\^\! \\\$\#" > rule.txt

### dodać łamanie haseł w \*.zip/\*.rar ?

---

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

