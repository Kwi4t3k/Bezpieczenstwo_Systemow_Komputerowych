
## Przydatne curle
curl -X GET http://localhost:port/encrypt -H 'accept: application/json'

curl -X POST http://localhost:port/submit -H 'accept: \*/\*' -H 'Content-Type: application/json' -d '{"session_id":"id_sesji", "encrypted_b64":"zaszyfrowane_słowo_base64"}'

curl -s -o data.txt -X GET 'http://localhost:port/encrypt' -> wysłanie do pliku outputu komendy
curl -s -O -X GET 'http://localhost:port/encrypt'

curl -X POST 'http://localhost:port/submit' -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"session_id":"id_sesji", "encrypted_data":" ' "$(cat image.enc.b64)" ' "}'

---

## Do sprawdzenia listy algorytmów szyfrujących

```
openssl list -cipher-commands
```

![[Pasted image 20251121115944.png]]

## lub

```
openssl list -cipher-algorithms
```

---

## Rodzaje algorytmów i co im trzeba do szczęścia
ecb -> klucz szyfrujący
cbc -> klucz szyfrujący, wektor inicjalizujący (IV)

---

### OPENSSL SZYFROWANIE używając algorytmu AES-256 w trybie ECB wykorzystując odebrany od serwera klucz szyfrujący
openssl enc -AES-256-ecb -in plik_ze_słowem.txt -K klucz_szesnastwowy_szyfrujący -out zaszyfrowany_plik_wyjściowy.enc -base64

`-base64 / -a -> jeśli wymaga tego zadanie`

`zamiast pliku ze słowem można wpisać -> <(echo -n slowo)`

`zamiast wypisania klucza można odczytać z pliku -> $(cat key)`

### OPENSSL SZYFROWANIE + iv + brak soli
openssl enc -AES-256-cbc -in plik_ze_słowem.txt -out zaszyfrowany_plik_wyjściowy.enc -K klucz_szesnastwowy_szyfrujący -iv wektor_inicjalizujący -nosalt -base64

### OPENSSL DESZYFROWANIE używając algorytmu AES-256 w trybie ECB 
openssl enc -d -aes-256-ecb -in zaszyfrowany_plik_base64 -out odszyfrowany_plik_wyjściowy.dec -K klucz_szesnastwowy_szyfrujący -base64

`musi być na końcu -base64 bo zaszyfrowany plik jest w tym formacie`

### OPENSSL DESZYFROWANIE pbkdf + pass + iv + brak soli
openssl enc -d -aes-256-cbc -pbkdf2 -pass pass:hasło -in zaszyfrowany_plik_base64 -out odszyfrowany_plik_wyjściowy.dec -base64 -iv wektor_inicjalizujący -nosalt

`pod -in wchodzi -> <(echo zawartość_encrypted_b64)

### OPENSSL DESZYFROWANIE pass + pbkdf2 + iter + nosalt
openssl enc -d -aes-256-ecb -in zaszyfrowany_plik_base64.bin -out odszyfrowany_plik_wyjściowy.dec -pass pass:hasło -pbkdf2 -iter liczba_iteracji -nosalt

`pod -in wchodzi -> echo -n "zaszyfrowany_ciąg_base64" | base64 -d > encrytped.bin`
### generowanie własnego klucza / iv
openssl rand -hex liczba > key/iv

liczba zależy od ilości bitów z polecenia, gdy klucz musi mieć 192 bity to -> 192/8 = 24,
czyli -> openssl rand -hex 24 > key

### OPENSSL SZYFROWANIE zdjęć
echo -n "ciąg_base64" | base64 -d > image.png

openssl enc -aria-128-ctr -K key -iv IV -in image.png -out image.enc

base64 image.enc > image.enc.b64 -> w tym pliku trzeba usunąć ręcznie entery bo nie przejdzie w serwerze

lub -> base64 -w 0 image.enc > image.enc.b64

### OPENSSL DESZYFROWANIE zdjęć
echo -n "ciąg_base64" | base64 -d > image.enc

openssl enc -d -aria-128-cfb -K key -iv IV -in image.enc -out image.png

base64 -w 0 image.png > image.b64