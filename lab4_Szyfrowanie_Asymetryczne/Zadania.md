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
tutaj \\/        openssl ec -in privkey.pem -pubout -out ecpub.pem
![[Pasted image 20251111200927.png]]

---

![[Pasted image 20251110191902.png]]
![[Pasted image 20251110191909.png]]

![[Pasted image 20251121173248.png]]
![[Pasted image 20251121173312.png]]

## 🔹 ZASZYFROWANIE SŁOWA RSA-2048 + OAEP

Użyj narzędzia `openssl pkeyutl` z trybem paddingu OAEP.

```bash
echo -n "slowo" | openssl pkeyutl -encrypt -pubin -inkey pubkey.pem -pkeyopt rsa_padding_mode:oaep -out encrypted.bin
```

💡 Co tu się dzieje:

- `echo -n "student2025"` — wprowadza słowo do szyfrowania
    
- `-encrypt` — tryb szyfrowania
    
- `-pubin` — klucz publiczny
    
- `-inkey pubkey.pem` — ścieżka do klucza publicznego
    
- `-pkeyopt rsa_padding_mode:oaep` — tryb paddingu OAEP
    
- `-out encrypted.bin` — wynik zaszyfrowany zapisany binarnie
    
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
## 🧰 **KROK 1 — uruchomienie serwera**

Uruchom kontener (jeden z poniższych, w zależności od tego czy masz Docker Hub czy GHCR):

```bash
docker run -p 3009:3009 --name ex9 docker.io/mazurkatarzyna/asymmetric-enc-ex9:latest
```

lub

```bash
docker run -p 3009:3009 --name ex9 ghcr.io/mazurkatarzynaumcs/asymmetric-enc-ex9:latest
```

---

## 🧠 **KROK 2 — pobranie danych od serwera**

Pobierasz klucz prywatny (`private_key.pem`), identyfikator sesji (`session_id`) i słowo (`word`).

```bash
curl -i -X GET 'http://127.0.0.1:3009/sign' \
     -H 'accept: application/json' \
     -o response.pem > headers.txt
```

Teraz zobacz nagłówki, żeby poznać ID sesji i słowo:

```bash
cat headers.txt | grep -E 'Session|Word'
```

🔹 W nagłówkach znajdziesz:

- `X-Session-Id:` → identyfikator sesji
    
- `X-Word:` → słowo do podpisania
    

🔹 A w pliku `response.pem` masz:

- Twój klucz prywatny RSA w formacie PEM (`-----BEGIN PRIVATE KEY-----` …)
    

![[Pasted image 20251112110225.png]]
![[Pasted image 20251112110238.png]]
![[Pasted image 20251112105709.png]]


---

## 💾 **KROK 3 — przygotowanie plików**

Zapisz słowo do pliku:

```bash
echo -n "TUTAJ_WSTAW_SŁOWO" > word.txt
```

![[Pasted image 20251112110342.png]]
Sprawdź:

```bash
cat word.txt
```

---

## ✍️ **KROK 4 — utworzenie skrótu SHA-256**

Tworzymy skrót SHA-256 (binarne dane):

```bash
openssl dgst -sha256 -binary -out word.sha256 word.txt
```

![[Pasted image 20251112110353.png]]

Sprawdź długość (powinno być 32 bajty):

```bash
ls -l word.sha256
```

![[Pasted image 20251112110403.png]]

---

## 🔐 **KROK 5 — podpisanie słowa RSA-PSS**

Użyj klucza prywatnego do podpisania skrótu:

```bash
openssl pkeyutl -sign -in word.sha256 -inkey response.pem \
    -pkeyopt digest:sha256 \
    -pkeyopt rsa_padding_mode:pss \
    -pkeyopt rsa_pss_saltlen:32 \
    -out signature.bin
```

![[Pasted image 20251112110448.png]]

Teraz zakoduj podpis do base64 (żeby można go było wysłać HTTP-em):

```bash
base64 signature.bin > signature.b64
```

![[Pasted image 20251112110458.png]]

Sprawdź zawartość:

```bash
cat signature.b64
```

![[Pasted image 20251112110513.png]]

---

## 🚀 **KROK 6 — wysłanie podpisu do serwera**

Użyj `curl` do przesłania podpisu (jako base64) i ID sesji:

```bash
curl -X POST 'http://127.0.0.1:3009/submit' \
     -H 'accept: application/json' \
     -H 'Content-Type: application/json' \
     -d '{
           "session_id": "TU_WSTAW_SESSION_ID",
           "signature_b64": "TU_WKLEJ_ZAWARTOŚĆ_signature.b64"
         }'
```

![[Pasted image 20251112105926.png]]

---

![[Pasted image 20251110192007.png]]
![[Pasted image 20251110192017.png]]

![[Pasted image 20251121185936.png]]
### Żeby przechwycić wynik polecenia do weryfikacji trzeba zrobić zmienną z wynikiem w formie 'true' lub 'false'
![[Pasted image 20251121190118.png]]
### Należy pamiętać o "" przy zmiennej w user_verified bo gdy podajemy zmienną w tym zadaniu musi być przekazany string a nie wartość true/false !!!

![[Pasted image 20251121190137.png]]
