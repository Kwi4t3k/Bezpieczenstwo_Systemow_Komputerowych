![[Pasted image 20260122174406.png]]

a)
![[Pasted image 20260122174753.png]]
![[Pasted image 20260122174804.png]]

b)
![[Pasted image 20260122174943.png]]
![[Pasted image 20260122175001.png]]
![[Pasted image 20260122175017.png]]

c)
• Dla jakiej domeny wystawiony jest certyfikat? 
![[Pasted image 20260122175425.png]]

• Kto wystawił certyfikat (CA)? 
![[Pasted image 20260122175449.png]]

• Kiedy certyfikat został wystawiony i kiedy wygasa?
![[Pasted image 20260122175512.png]]

• Jakie dodatkowe domeny są obsługiwane?
![[Pasted image 20260122175604.png]]

• Jaki algorytm i długość klucza?
![[Pasted image 20260122175706.png]]

• Do czego może być użyty certyfikat?
![[Pasted image 20260122180222.png]]

• Ile certyfikatów znajduje się w łańcuchu zaufania?


• Kto jest głównym CA (root CA) w łańcuchu?


d)
![[Pasted image 20260122180053.png]]

![[Pasted image 20260122174414.png]]

![[Pasted image 20260122181220.png]]

![[Pasted image 20260122174425.png]]

b)


![[Pasted image 20260122174432.png]]

![[Pasted image 20260122174442.png]]

![[Pasted image 20260122174449.png]]

![[Pasted image 20260122174502.png]]



Pewnie — poniżej masz **rozpisane krok po kroku zadania 6.1–6.7** dokładnie wg kartki, z gotowymi komendami + krótką instrukcją “co z tego przepisać do sprawozdania”.

---

## 6.1 Analiza certyfikatu SSL/TLS witryny (github.com)

### 6.1(a) Pobierz certyfikat do pliku

```bash
echo | openssl s_client -servername github.com -connect github.com:443 2>/dev/null \
| openssl x509 -out github.crt
```

### 6.1(b) Wyświetl certyfikat czytelnie

```bash
openssl x509 -in github.crt -text -noout
```

### 6.1(c) Odczytaj wymagane informacje (OpenSSL)

**Dla jakiej domeny (CN + SAN):**

```bash
openssl x509 -in github.crt -noout -subject
openssl x509 -in github.crt -noout -text | grep -A1 "Subject Alternative Name"
```

**Kto wystawił (Issuer / CA):**

```bash
openssl x509 -in github.crt -noout -issuer
```

**Kiedy wydany i kiedy wygasa:**

```bash
openssl x509 -in github.crt -noout -dates
```

**Dodatkowe domeny (SAN):**

```bash
openssl x509 -in github.crt -noout -text | grep -A1 "Subject Alternative Name"
```

**Algorytm i długość klucza:**

```bash
openssl x509 -in github.crt -noout -text | grep -E "Public Key Algorithm|Public-Key"
```

**Do czego może być użyty certyfikat (najprościej):**

```bash
openssl x509 -in github.crt -noout -purpose
```

(alternatywnie “ręcznie” z KU/EKU)

```bash
openssl x509 -in github.crt -noout -text | grep -A2 "Key Usage"
openssl x509 -in github.crt -noout -text | grep -A2 "Extended Key Usage"
```

**Ile certyfikatów w łańcuchu zaufania + kto jest root CA**  
(Tego zwykle nie ma w samym `github.crt`, tylko w połączeniu TLS.)

- liczba certyfikatów wysłanych przez serwer:
    

```bash
echo | openssl s_client -servername github.com -connect github.com:443 -showcerts 2>/dev/null \
| grep -c "BEGIN CERTIFICATE"
```

- root CA (największy `depth=`):  
    **Debian/Ubuntu:**
    

```bash
echo | openssl s_client -servername github.com -connect github.com:443 -verify 10 \
-CAfile /etc/ssl/certs/ca-certificates.crt 2>&1 | grep "^depth="
```

**Fedora/RHEL:**

```bash
echo | openssl s_client -servername github.com -connect github.com:443 -verify 10 \
-CAfile /etc/pki/tls/certs/ca-bundle.crt 2>&1 | grep "^depth="
```

W sprawozdaniu: `depth=0` = cert serwera, największy `depth=` = **root CA**.

### 6.1(d) Wyodrębnij klucz publiczny do pliku

```bash
openssl x509 -in github.crt -pubkey -noout > github_pubkey.pem
```

---

## 6.2 Pobierz cały łańcuch z github.com i wskaż serwer vs CA

### 6.2(a) Pobierz łańcuch do `gh.txt`

```bash
openssl s_client -connect github.com:443 -showcerts </dev/null > gh.txt 2>/dev/null
```

### 6.2(b) Sprawdź liczbę certyfikatów w łańcuchu

```bash
grep -c "BEGIN CERTIFICATE" gh.txt
```

### 6.2(c) Wskaż: który cert jest serwera, które CA (bez rozbijania na pliki)

Wyświetl sekcję „Certificate chain” z `gh.txt`:

```bash
sed -n '/Certificate chain/,/---/p' gh.txt
```

Interpretacja:

- wpis **`0 s:`** → certyfikat **serwera** (leaf)
    
- wpisy **`1 s:`**, **`2 s:`** → certyfikaty **CA (intermediate)**
    

---

## 6.3 Analiza certyfikatu lokalnego serwera (self-signed)

### 6.3(a) Uruchom serwer (Docker/Podman)

Docker:

```bash
docker run -p 8443:443 --name tls3 docker.io/mazurkatarzyna/tls-ex3:latest
```

Podman:

```bash
podman run -p 8443:443 --name tls3 docker.io/mazurkatarzyna/tls-ex3:latest
```

Zapasowy obraz:

```bash
docker run -p 8443:443 --name tls3 ghcr.io/mazurkatarzynaumcs/tls-ex3:latest
```

Serwer będzie pod: `https://127.0.0.1:8443`.

### 6.3(b) Pobierz cert serwera i wyświetl czytelnie

Pobranie leaf cert do pliku:

```bash
echo | openssl s_client -connect 127.0.0.1:8443 -servername 127.0.0.1 2>/dev/null \
| openssl x509 -out local8443.crt
```

Wyświetlenie:

```bash
openssl x509 -in local8443.crt -text -noout
```

### 6.3(c) Analiza – te same pola co w 6.1

Użyj dokładnie tych samych komend co w 6.1(c), tylko na `local8443.crt` (subject/issuer/dates/SAN/algorytm/purpose/chain/root).

### 6.3(d) Klucz publiczny do pliku

```bash
openssl x509 -in local8443.crt -pubkey -noout > local8443_pubkey.pem
```

---

## 6.4 Łańcuch certyfikatów z lokalnego serwera (127.0.0.1:8443)

### 6.4(a) Pobierz łańcuch do `gh.txt`

(Uwaga: w PDF znowu nazwa pliku `gh.txt` — możesz dać np. `local_chain.txt`, ale jak chcesz trzymać się polecenia, zostaw `gh.txt`.)

```bash
openssl s_client -connect 127.0.0.1:8443 -showcerts </dev/null > gh.txt 2>/dev/null
```

### 6.4(b) Policz certyfikaty

```bash
grep -c "BEGIN CERTIFICATE" gh.txt
```

### 6.4(c) Wskaż serwer vs CA

```bash
sed -n '/Certificate chain/,/---/p' gh.txt
```

Przy self-signed często wyjdzie 1 cert (serwer) i brak CA w łańcuchu.

---

## 6.5 Serwer testowy certyfikatów (127.0.0.1:8447) + walidacja odpowiedzi

### 6.5(a) Uruchom serwer

```bash
docker run -p 8447:443 --name tls6 docker.io/mazurkatarzyna/tls-ex6:latest
# albo:
docker run -p 8447:443 --name tls6 ghcr.io/mazurkatarzynaumcs/tls-ex6:latest
```

Serwer: `https://127.0.0.1:8447`, endpointy: `/cert`, `/validate`, (i `/public`) + `/apidocs`.

### 6.5(b) Pobierz certyfikat z endpointa `/cert` do `cert.pem`

Najprościej curl (dla lokalnego/self-signed dodaj `-k`):

```bash
curl -k https://127.0.0.1:8447/cert -o cert.pem
```

### 6.5(c) Wyodrębnij klucz publiczny i wyślij na `/public`

Wyodrębnij:

```bash
openssl x509 -in cert.pem -pubkey -noout > public_key.pem
```

Wyślij jako plik (nazwa pola wg PDF: `public_key`):

```bash
curl -k -F "public_key=@public_key.pem" https://127.0.0.1:8447/public
```

W odpowiedzi serwer powie, czy klucz pasuje do certyfikatu.

### 6.5(d) Przeanalizuj cert.pem i przygotuj odpowiedzi

- expiry_date, issued_date:
    

```bash
openssl x509 -in cert.pem -noout -dates
```

- issuer (Root CA w sensie “Issuer” podany w certyfikacie):
    

```bash
openssl x509 -in cert.pem -noout -issuer
```

- key_algorithm + key_size:
    

```bash
openssl x509 -in cert.pem -noout -text | grep -E "Public Key Algorithm|Public-Key"
```

- primary_domain + san_domains:
    

```bash
openssl x509 -in cert.pem -noout -subject
openssl x509 -in cert.pem -noout -text | grep -A1 "Subject Alternative Name"
```

- key_usage:
    

```bash
openssl x509 -in cert.pem -noout -purpose
# albo:
openssl x509 -in cert.pem -noout -text | grep -A2 "Key Usage"
openssl x509 -in cert.pem -noout -text | grep -A2 "Extended Key Usage"
```

Lista pól, które masz odesłać: `expiry_date`, `issued_date`, `issuer`, `key_algorithm`, `key_size`, `primary_domain`, `san_domains`, `key_usage`.

### 6.5(e) Wyślij odpowiedzi na `/validate` (POST)

Najpierw podejrzyj wymagany format w `/apidocs`:

- w przeglądarce: `https://127.0.0.1:8447/apidocs` (może być ostrzeżenie certyfikatu)
    
- albo:
    

```bash
curl -k https://127.0.0.1:8447/apidocs
```

Potem wysyłka (przykład – uzupełnij swoimi wartościami wg /apidocs):

```bash
curl -k -H "Content-Type: application/json" \
-d '{"expiry_date":"YYYY-MM-DD","issued_date":"YYYY-MM-DD","issuer":"...","key_algorithm":"...","key_size":2048,"primary_domain":"...","san_domains":["..."],"key_usage":["..."]}' \
https://127.0.0.1:8447/validate
```

---

## 6.6 Deszyfrowanie TLS mając klucz prywatny serwera (TLS_RSA)

Tu chodzi o sytuację, gdzie da się odszyfrować sesję TLS z PCAP **posiadając klucz prywatny serwera**, ale tylko gdy użyto wymiany kluczy typu RSA (TLS_RSA).

### 6.6(a–b) Uruchom serwer (wg PDF)

```bash
docker run -p 8445:443 --name tls4 docker.io/mazurkatarzyna/tls-ex4:latest
# albo obraz zapasowy z PDF (jeśli zadziała):
docker run -p 8445:443 --name tls4 ghcr.io/mazurkatarzynaumcs/tls-ex4:latest
```

### 6.6(c) Otwórz w Wireshark plik `nettraffic1.pcapng`

Sprawdź, czy da się odczytać treść (na start zwykle nie).

### 6.6(d) Dodaj klucz prywatny serwera w Wireshark

Wireshark: **Edit → Preferences → Protocols → TLS → RSA keys list**  
Dodaj wpis (wg PDF):

- Address: `0.0.0.0`
    
- Port: `8443` _(jeśli Twój ruch był na innym porcie, wpisz port z przechwyconej sesji)_
    
- Protocol: `http`
    
- Key file: `/ścieżka/do/server.key`
    

> Skąd wziąć `server.key`? Jeśli masz go w materiałach do labów – użyj. Jeśli masz tylko kontener i musisz go znaleźć, typowo działa:

```bash
docker exec -it tls4 sh -lc "find / -maxdepth 4 -name '*.key' -o -name 'server.key' 2>/dev/null"
# a potem np.:
docker cp tls4:/sciezka/z/kontenera/server.key ./server.key
```

### 6.6(e) Sprawdź czy ruch się odszyfrował

Po dodaniu klucza, w Wireshark powinny pojawić się zdekodowane warstwy HTTP (jeśli to naprawdę TLS_RSA).

### 6.6(f) Przechwyć własny ruch do serwera i odszyfruj

PDF mówi: przechwyć ruch do serwera pod `https://127.0.0.1:8447` i odszyfruj kluczem prywatnym. (Tu robisz analogicznie: capture → dodajesz RSA key → sprawdzasz dekodowanie).

---

## 6.7 Deszyfrowanie TLS przy użyciu SSLKEYLOGFILE (działa też z ECDHE)

Cel: odszyfrować ruch mając **plik SSLKEYLOGFILE** (klucze sesyjne), nawet przy ECDHE/forward secrecy.

### 6.7(b) Uruchom serwer

```bash
docker run -p 8446:443 --name tls5 docker.io/mazurkatarzyna/tls-ex5:latest
# albo:
docker run -p 8446:443 --name tls5 ghcr.io/mazurkatarzynaumcs/tls-ex5:latest
```

Serwer: `https://127.0.0.1:8446`.

### 6.7(c) Ustaw SSLKEYLOGFILE w terminalu (nie zamykaj tego okna)

```bash
export SSLKEYLOGFILE=/tmp/sslkeys.log
```

### 6.7(d) Włącz przechwytywanie w Wireshark/tcpdump

Filtruj ruch do `127.0.0.1:8446` (albo po porcie `8446`).

### 6.7(e) Uruchom przeglądarkę z TEGO SAMEGO terminala

```bash
firefox &
```

(ważne: musi wystartować z terminala, gdzie jest `SSLKEYLOGFILE`).

### 6.7(f) Sprawdź, czy plik z kluczami powstał

```bash
file /tmp/sslkeys.log
```

### 6.7(g) Wejdź na [https://127.0.0.1:8446](https://127.0.0.1:8446/) i “naklikaj” coś do przechwycenia

Potem zatrzymaj capture.

### 6.7(h) W Wireshark podaj plik sslkeys.log do TLS i odczytaj ruch

Wireshark: **Edit → Preferences → Protocols → TLS**  
Pole: **(Pre)-Master-Secret log filename** → wskaż `/tmp/sslkeys.log` → OK.  
Powinieneś zobaczyć odszyfrowany ruch.

---

Jeśli chcesz, napisz na jakim systemie pracujesz (Docker czy Podman, Linux/WSL) i czy masz Wireshark — a ja dopasuję Ci **dokładne filtry w Wireshark** i gotowy “szablon odpowiedzi do sprawozdania” (punkt po punkcie) dla 6.1/6.3/6.5.