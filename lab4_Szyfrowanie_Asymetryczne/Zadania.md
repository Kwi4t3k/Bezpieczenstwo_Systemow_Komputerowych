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
### Notatka
Nie ma id sesji?

![[Pasted image 20251111144816.png]]
![[Pasted image 20251111144834.png]]

---

![[Pasted image 20251110191850.png]]

### Notatka 
Nie ma id sesji

![[Pasted image 20251111145352.png]]
![[Pasted image 20251111145404.png]]
![[Pasted image 20251111145417.png]]

---

![[Pasted image 20251110191902.png]]
![[Pasted image 20251110191909.png]]

### Notatka
Nie ma danych z serwera

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

---

![[Pasted image 20251110191930.png]]
![[Pasted image 20251110191944.png]]

---

![[Pasted image 20251110191956.png]]

---

![[Pasted image 20251110192007.png]]
![[Pasted image 20251110192017.png]]

