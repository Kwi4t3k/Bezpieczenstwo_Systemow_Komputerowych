![[Pasted image 20251110191759.png]]

![[Pasted image 20251111141347.png]]
![[Pasted image 20251111141417.png]]
![[Pasted image 20251111141501.png]]
![[Pasted image 20251111141535.png]]

---

![[Pasted image 20251110191811.png]]

![[Pasted image 20251111142310.png]]
![[Pasted image 20251111142321.png]]
![[Pasted image 20251111142345.png]]
![[Pasted image 20251111142411.png]]

---

![[Pasted image 20251110191824.png]]

![[Pasted image 20251111143602.png]]
![[Pasted image 20251111143624.png]]
![[Pasted image 20251111143646.png]]

### Notatka
Nie trzeba w żaden sposób edytować plików

---

![[Pasted image 20251110191837.png]]

![[Pasted image 20251111200412.png]]
![[Pasted image 20251111200424.png]]
![[Pasted image 20251111200448.png]]
![[Pasted image 20251111200511.png]]
![[Pasted image 20251111200526.png]]

---

![[Pasted image 20251110191850.png]]

![[Pasted image 20251111200913.png]]
![[Pasted image 20251111200927.png]]

---

![[Pasted image 20251110191902.png]]
![[Pasted image 20251110191909.png]]

DOKOŃCZYĆ bo idk czemu nie działa

![[Pasted image 20251111205456.png]]
![[Pasted image 20251111205509.png]]

## Przykładowy przebieg rozwiązywania zadania z chata

## 🔹 KROK 1 — URUCHOMIENIE SERWERA

Uruchom serwer (jeden z dwóch wariantów):

```bash
docker run -p 3006:3006 --name ex6 docker.io/mazurkatarzyna/asymmetric-enc-ex6:latest
```

💡 Jeśli Docker Hub nie działa:

```bash
docker run -p 3006:3006 --name ex6 ghcr.io/mazurkatarzynaumcs/asymmetric-enc-ex6:latest
```

---

## 🔹 KROK 2 — POBRANIE DANYCH Z SERWERA

Serwer działa lokalnie, więc wyślij zapytanie GET:

```bash
curl -i -X GET http://127.0.0.1:3006/encrypt -H "accept: application/json"
```

Przykładowa odpowiedź z serwera (z nagłówkami):

```
HTTP/1.1 200 OK
X-Session-ID: 123abc456def
X-Word: student2025
Content-Type: application/json

{
  "public_key_pem": "-----BEGIN PUBLIC KEY-----\nMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAmW5...\n-----END PUBLIC KEY-----\n"
}
```

📘 Z tej odpowiedzi:

- **Session-ID:** `123abc456def`
    
- **Słowo (Word):** `student2025`
    
- **Klucz publiczny:** `public_key_pem`
    

---

## 🔹 KROK 3 — ZAPISZ KLUCZ PUBLICZNY DO PLIKU

Skopiuj zawartość pola `public_key_pem` (łącznie z `-----BEGIN PUBLIC KEY-----` i `-----END PUBLIC KEY-----`)  
i zapisz do pliku `pubkey.pem`:

```bash
nano pubkey.pem
```

➡️ Wklej zawartość klucza RSA publicznego i zapisz (`CTRL+O`, `ENTER`, `CTRL+X`).

---

## 🔹 KROK 4 — ZASZYFRUJ SŁOWO RSA-2048 + OAEP

Użyj narzędzia `openssl pkeyutl` z trybem paddingu OAEP.

```bash
echo -n "student2025" | openssl pkeyutl -encrypt -pubin -inkey pubkey.pem -pkeyopt rsa_padding_mode:oaep -out encrypted.bin
```

💡 Co tu się dzieje:

- `echo -n "student2025"` — wprowadza słowo do szyfrowania
    
- `-encrypt` — tryb szyfrowania
    
- `-pubin` — klucz publiczny
    
- `-inkey pubkey.pem` — ścieżka do klucza publicznego
    
- `-pkeyopt rsa_padding_mode:oaep` — tryb paddingu OAEP
    
- `-out encrypted.bin` — wynik zaszyfrowany zapisany binarnie
    

---

## 🔹 KROK 5 — ZAKODUJ WYNIK DO BASE64

Serwer wymaga, aby plik był zakodowany w base64, więc:

```bash
base64 encrypted.bin > encrypted.b64
```

Możesz sprawdzić wynik:

```bash
cat encrypted.b64
```

Wynik przykładowy:

```
Z9pJmLkbUqH34d2iKsH7I0LZP4G6svX1oWPS9JfJ7fX2lXqv8+T0Hg==
```

---

## 🔹 KROK 6 — WYSŁANIE ODPOWIEDZI NA /submit

Teraz musisz wysłać POST z identyfikatorem sesji (`X-Session-ID`) i zaszyfrowanym plikiem.

```bash
curl -X POST http://127.0.0.1:3006/submit \
-H "accept: application/json" \
-H "Content-Type: application/json" \
-d '{
  "session_id": "123abc456def",
  "encrypted_file": "Z9pJmLkbUqH34d2iKsH7I0LZP4G6svX1oWPS9JfJ7fX2lXqv8+T0Hg=="
}'
```

---

## 🔹 KROK 7 — WERYFIKACJA

Serwer odpowie np.:

```
{"result": "Success"}
```

lub, jeśli coś nie tak:

```
{"result": "Incorrect encryption"}
```

---

## 📘 PEŁNY SKRÓT KOMEND

Dla ułatwienia — cały przebieg w kolejności do skopiowania:

```bash
# 1. Uruchom serwer
docker run -p 3006:3006 --name ex6 docker.io/mazurkatarzyna/asymmetric-enc-ex6:latest

# 2. Pobierz dane
curl -i -X GET http://127.0.0.1:3006/encrypt -H "accept: application/json"

# 3. Zapisz klucz publiczny do pliku pubkey.pem
nano pubkey.pem

# 4. Zaszyfruj słowo
echo -n "student2025" | openssl pkeyutl -encrypt -pubin -inkey pubkey.pem -pkeyopt rsa_padding_mode:oaep -out encrypted.bin

# 5. Zakoduj base64
base64 encrypted.bin > encrypted.b64

# 6. Wyślij odpowiedź do serwera
curl -X POST http://127.0.0.1:3006/submit \
-H "accept: application/json" \
-H "Content-Type: application/json" \
-d '{
  "session_id": "123abc456def",
  "encrypted_file": "WARTOŚĆ_Z_PLIKU_encrypted.b64"
}'
```

---

## 💡 PODSUMOWANIE

|Etap|Cel|Komenda|
|---|---|---|
|a|Uruchomienie serwera|`docker run -p 3006:3006 …`|
|b|Pobranie danych|`curl -i -X GET …`|
|c|Szyfrowanie RSA+OAEP|`openssl pkeyutl -encrypt …`|
|d|Base64|`base64 encrypted.bin > encrypted.b64`|
|e|Wysłanie wyniku|`curl -X POST …`|

---

![[Pasted image 20251110191919.png]]

## 🔹 Krok 1 — Uruchom serwer

```bash
docker run -p 3007:3007 --name ex7 docker.io/mazurkatarzyna/asymmetric-enc-ex7:latest
```

lub:

```bash
podman run -p 3007:3007 --name ex7 docker.io/mazurkatarzynaumcs/asymmetric-enc-ex7:latest
```

---

## 🔹 Krok 2 — Pobierz dane z serwera (`/decrypt`)

### Jedno polecenie, które pobiera ZIP **i** zapisuje nagłówki (z `session_id`)

```bash
curl -s -D headers.txt -o response.zip -X GET 'http://127.0.0.1:3007/decrypt' -H 'accept: application/json'
```

🔹 Co robi każda opcja:

- `-s` — tryb “silent” (bez pasków postępu),
    
- `-D headers.txt` — **zapisuje wszystkie nagłówki HTTP** do pliku `headers.txt`,
    
- `-o response.zip` — **zapisuje treść odpowiedzi (plik ZIP)**,
    
- reszta — Twój standardowy request.
    

![[Pasted image 20251111155358.png]]

---

### 📁 Po tym poleceniu masz dwa pliki:

```
headers.txt
response.zip
```

![[Pasted image 20251111155430.png]]

---

### 🔍 Odczytaj `session_id` z nagłówków:

```bash
grep -i X-Session-Id headers.txt
```

Przykład wyniku:

```
X-Session-Id: 4dbd46f4265e5e03
```

➡️ to jest Twój `session_id` do późniejszego POST-a.

![[Pasted image 20251111155510.png]]
![[Pasted image 20251111155536.png]]

---

## 🔹 Krok 3 — Rozpakuj plik ZIP

```bash
unzip response.zip -d zad7
```

Po rozpakowaniu zobaczysz coś takiego:

```bash
ls zad7
```

Wynik:

```
encrypted.txt
private_key.pem
```

![[Pasted image 20251111155604.png]]
![[Pasted image 20251111155619.png]]

---

## 🔹 Krok 4 — Sprawdź, co jest w pliku `encrypted.txt`

```bash
cat zad7/encrypted.txt
```

Zobaczysz długi ciąg Base64 — to zaszyfrowane dane (RSA4096 + OAEP).

![[Pasted image 20251111155811.png]]

### Dodatkowo private_key.pem

![[Pasted image 20251111155859.png]]
![[Pasted image 20251111155926.png]]

---

## 🔹 Krok 5 — Odszyfruj słowo RSA-4096 OAEP

Użyj **OpenSSL** z argumentem `pkeyutl` (jak w poleceniu z uwag):

```bash
openssl pkeyutl -decrypt -inkey zad7/private_key.pem -in <(base64 -d zad7/encrypted.txt) -pkeyopt rsa_padding_mode:oaep -out decrypted.txt
```

💡 Co się tu dzieje:

- `-decrypt` — tryb odszyfrowania
    
- `-inkey zad7/private_key.pem` — klucz prywatny RSA 4096
    
- `base64 -d` — dekoduje dane z base64
    
- `-pkeyopt rsa_padding_mode:oaep` — wymusza tryb OAEP
    
- `-out decrypted.txt` — zapisuje wynik do pliku
    

![[Pasted image 20251111160535.png]]
### lub

**Krok 5 (odszyfrowanie RSA-4096 z OAEP)** można zrobić **prościej**, rozbijając go na kilka bardziej intuicyjnych komend (zamiast jednego długiego z `process substitution`).  
Zrobimy to w **3 prostych krokach**, które łatwiej zrozumieć i debugować 👇

---

## 🔹 Wersja uproszczona odszyfrowania (zamiast jednej skomplikowanej komendy):

### 🧩 1️⃣ Dekoduj dane Base64 do pliku binarnego:

```bash
base64 -d zad7/encrypted.txt > encrypted.bin
```

🔍 Teraz masz „surowe” zaszyfrowane dane binarne w pliku `encrypted.bin`.

![[Pasted image 20251111160038.png]]

---

### 🧩 2️⃣ Użyj klucza prywatnego do odszyfrowania RSA-4096 z OAEP:

```bash
openssl pkeyutl -decrypt -inkey zad7/private_key.pem -in encrypted.bin -pkeyopt rsa_padding_mode:oaep -out decrypted.txt
```

![[Pasted image 20251111160122.png]]

---

## 🔹 Podsumowanie krótszej wersji:

```bash
# Dekodowanie Base64
base64 -d zad7/encrypted.txt > encrypted.bin

# Odszyfrowanie RSA-4096 z OAEP
openssl pkeyutl -decrypt -inkey zad7/private_key.pem -in encrypted.bin -pkeyopt rsa_padding_mode:oaep -out decrypted.txt
```

---

## 🔹 Krok 6 — Wyświetl odszyfrowane słowo

```bash
cat decrypted.txt
```

Wynik będzie czymś takim:

```
network2025
```

Zachowaj to słowo — to **decrypted_word**.

![[Pasted image 20251111160144.png]]

---

## 🔹 Krok 7 — Wyślij rozwiązanie na `/submit`

Z odpowiedzi `curl` z `/decrypt` (tej pierwszej) zapisz również **Session-ID** z nagłówka `X-Session-ID`.  
Załóżmy, że był to np. `7d2f9b88c31e`.

Wyślij wynik na `/submit`:

```bash
curl -X POST http://127.0.0.1:3007/submit \
-H "accept: application/json" \
-H "Content-Type: application/json" \
-d '{
  "session_id": "7d2f9b88c31e",
  "decrypted_word": "network2025"
}'
```

![[Pasted image 20251111160322.png]]

---

## 📘 PEŁNE PODSUMOWANIE KOMEND

```bash
# 1. Uruchom serwer
docker run -p 3007:3007 --name ex7 docker.io/mazurkatarzyna/asymmetric-enc-ex7:latest

# 2. Pobierz dane
curl -i -X GET http://127.0.0.1:3007/decrypt -H "accept: application/json" -o response.zip

# 3. Rozpakuj ZIP
unzip response.zip -d zad7

# 4. Sprawdź pliki
ls zad7
cat zad7/encrypted.txt

# 5. Odszyfruj (RSA-4096 OAEP)
openssl pkeyutl -decrypt -inkey zad7/private_key.pem -in <(base64 -d zad7/encrypted.txt) -pkeyopt rsa_padding_mode:oaep -out decrypted.txt

# 6. Wyświetl odszyfrowane słowo
cat decrypted.txt

# 7. Wyślij odpowiedź
curl -X POST http://127.0.0.1:3007/submit \
-H "accept: application/json" \
-H "Content-Type: application/json" \
-d '{
  "session_id": "TUTAJ_SESSION_ID",
  "decrypted_word": "TUTAJ_TEKST"
}'
```

---

![[Pasted image 20251110191930.png]]
![[Pasted image 20251110191944.png]]

![[Pasted image 20251111212048.png]]
![[Pasted image 20251111212948.png]]
![[Pasted image 20251111213007.png]]

---

![[Pasted image 20251110191956.png]]

---

![[Pasted image 20251110192007.png]]
![[Pasted image 20251110192017.png]]

