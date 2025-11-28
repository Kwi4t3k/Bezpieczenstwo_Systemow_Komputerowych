## Przydatne curle
curl -X POST 'http://localhost:port/checkkeys' -H 'accept: application/json' -H 'Content-Type: multipart/form-data' -F 'session_id=id_sesji' -F 'pub_key_file=@pubkey.pem' -F 'priv_key_file=@privkey.pem'

curl -s -D headers.txt -o privkey.pem -X GET 'http://localhost:port/getprivkey' -H 'accept: application/json' -> wysłanie nagłówków i outputu do pliku

curl -s -D headers.txt -o response.zip -X GET 'http://127.0.0.1:3007/decrypt' -H 'accept: application/json'

---

### Generowanie pary kluczy RSA (publiczny i prywatny) + eksport do plików + długość klucza 1024 bity
klucz prywatny -> openssl genpkey -algorithm RSA -out privkey.pem -pkeyopt rsa_keygen_bits:1024

klucz publiczny -> openssl pkey -in privkey.pem -pubout -out pubkey.pem

### Generowanie pary kluczy EC - krzywych eliptycznych (publiczny i prywatny) + krzywa eliptyczna prime256v1 + eksport do plików
klucz prywatny -> openssl genpkey -algorithm EC -out ecpriv.pem -pkeyopt ec_paramgen_curve:prime256v1

klucz publiczny -> openssl ec -in ecpriv.pem -pubout -out ecpub.pem

### Sprawdzenie ilu bitowy jest klucz
publiczny EC -> openssl ec -pubin -in ec_public_key.pem -text -noout
prywatny EC -> openssl ec -in ec_private_key.pem -text -noout

uniwersalne:
openssl pkey -in key.pem -text -noout
openssl pkey -pubin -in key.pub -text -noout

### Zaszyfrowanie słowa algorytmem RSA-2048 z kluczem publicznym i wybranym trybem paddingu: OAEP
openssl pkeyutl -encrypt -pubin -inkey key.pub -pkeyopt rsa_padding_mode:oaep -in plik_ze_słowem -out encrypted.bin

pod słowo można też dać -> <(echo -n słowo)

### Odkodowanie i odszyfrowanie słowa przy użyciu klucza prywatnego i algorytmu RSA-4096 z paddingiem OAEP
openssl pkeyutl -decrypt -inkey private_key.pem -in <(base64 -d enctypted.txt) -pkeyopt rsa_padding_mode:oaep -out decrypted.txt

lub 

base64 -d encrypted.txt > encrypted.bin
openssl pkeyutl -decrypt -inkey private_key.pem -in encrypted.bin -pkeyopt rsa_padding_mode:oaep -out decrypted.txt

### Podpisanie słowa kluczem prywatnym + przy podpisie parametr paddingu PSS + format base64 + używana funkcja skrótu SHA-256 + długość soli ma być równa z długością skrótu (32B dla SHA-256)
utworzenie skrótu -> openssl dgst -sha256 -binary -out word.sha256 word.txt
`żeby sprawdzić ile wynosi długość pliku .sha256 -> ls -l word.sha256`

podpisanie słowa -> openssl pkeyutl -sign -in word.sha256 -inkey private_key_pem -pkeyopt digest:sha256 -pkeyopt rsa_padding_mode:pss -pkeyopt rsa_pss_saltlen:32 -out signature.bin

base64 signature.bin > signature.b64

### Weryfikacja podpisu algorytmem RSA-2048 i trybem paddingu PSS + funkcja skrótu SHA-256 + długość soli dopasowana do skrótu
base64 -d signature.b64 > signature.bin

openssl dgst -sha256 -verify public_key.pem -sigopt rsa_padding_mode:pss -sigopt rsa_pss_saltlen:32 -signature signature.bin word.txt

`przechwycenie wyniku do zmiennej result -> result=$(openssl dgst -sha256 -verify public_key.pem -sigopt rsa_padding_mode:pss -sigopt rsa_pss_saltlen:32 -signature signature.bin word.txt | grep -q 'Verified OK' && echo true || echo false)`
`sprawdzenie czy działa -> echo $result`
`użycie w curl -> -F "user_verified=$result"`

---

Oczywiście! Aby odpowiedzieć na Twoje pytanie, wyjaśnię, w jakich sytuacjach i dla jakich plików OpenSSL wymaga formatu binarnego, a nie tekstowego (np. base64). Poniżej przedstawiam szczegółowe rozpisanie krok po kroku, kiedy OpenSSL wymaga plików binarnych:

### 1. **Podpis cyfrowy w formacie binarnym**

Kiedy weryfikujesz podpis cyfrowy w OpenSSL, podpis (czyli `signature`) zazwyczaj musi być w formacie **binarnym**, a nie w formacie tekstowym. Na przykład, plik z podpisem (`slowo.sig`) powinien być binarny, a nie zakodowany w base64.

- **Przykład weryfikacji podpisu z plikiem binarnym**:
    
    ```bash
    openssl dgst -sha256 -verify pub.pem -sigopt rsa_padding_mode:pss -sigopt rsa_pss_saltlen:32 -signature slowo.sig slowo.txt
    ```
    
    **Gdzie**:
    
    - `slowo.sig` musi być plikiem binarnym z podpisem (np. wyjście z procesu podpisywania w formacie binarnym).
        

**Dlaczego binarny?** Ponieważ podpisy cyfrowe są zazwyczaj generowane w postaci surowych danych binarnych, które zawierają zaszyfrowaną informację o oryginalnym dokumencie.

### 2. **Podpis cyfrowy w formacie base64**

Jeśli podpis jest zapisany w formacie base64 (np. w pliku tekstowym), przed weryfikacją musisz go **przekonwertować na format binarny**, ponieważ `openssl dgst` nie obsługuje podpisów w base64 bezpośrednio. Używasz wtedy narzędzia `base64`, aby zdekodować plik do formatu binarnego.

- **Przykład konwersji pliku z podpisem w base64 na plik binarny**:
    
    ```bash
    base64 -d slowo.sig > slowo.sig.bin
    ```
    
    A potem:
    
    ```bash
    openssl dgst -sha256 -verify pub.pem -sigopt rsa_padding_mode:pss -sigopt rsa_pss_saltlen:32 -signature slowo.sig.bin slowo.txt
    ```
    

**Dlaczego tak?** Ponieważ OpenSSL oczekuje binarnych danych do operacji weryfikacji podpisów (takich jak RSA). Dane w base64 są tylko kodowaniem tekstowym, a nie surową reprezentacją danych.

### 3. **Weryfikacja z plikami w formacie PEM**

Jeśli używasz klucza publicznego w formacie PEM (który jest standardowym formatem przechowywania kluczy w OpenSSL), nie musisz konwertować pliku PEM do formatu binarnego. OpenSSL obsługuje pliki PEM bez potrzeby zmiany formatu.

- **Przykład z kluczem publicznym w formacie PEM**:
    
    ```bash
    openssl dgst -sha256 -verify pub.pem -sigopt rsa_padding_mode:pss -sigopt rsa_pss_saltlen:32 -signature slowo.sig slowo.txt
    ```
    

W tym przypadku `pub.pem` jest plikiem w formacie PEM, który zawiera klucz publiczny RSA.

### 4. **Podpisy RSA z OpenSSL (weryfikacja przy użyciu klucza publicznego)**

Do weryfikacji podpisu RSA nie musisz przekształcać samego klucza publicznego (np. w formacie PEM) do binarnego, ponieważ OpenSSL obsługuje format PEM (zakodowany w ASCII). Jednak, jak wspomniano wcześniej, **podpis musi być w formacie binarnym**.

### 5. **Podsumowanie - pliki binarne vs tekstowe**:

- **Podpis cyfrowy (signature)**: Zawsze wymaga formatu binarnego, chyba że masz plik w base64. Jeśli masz plik w base64, musisz go przekonwertować na plik binarny.
    
- **Dokument do podpisania (message)**: Może być w formacie tekstowym lub binarnym. Zwykle jest to plik tekstowy (np. `.txt`), ale może też być plikiem binarnym (np. `.pdf`).
    
- **Klucz publiczny (public key)**: Plik PEM jest często stosowany w formacie tekstowym i nie wymaga konwersji na format binarny.
    

---

### Podsumowując, w procesie weryfikacji podpisu za pomocą OpenSSL musisz:

1. **Sprawdzić, czy podpis jest w formacie binarnym** — jeśli jest w formacie base64, musisz go zdekodować na binarny plik.
    
2. **Użyć klucza publicznego w formacie PEM** — nie musisz go konwertować do formatu binarnego.
    
3. **Weryfikować podpis, przekazując go w odpowiedniej postaci** — czyli zdekodowany w przypadku base64 i plik binarny w przypadku podpisów w innych formatach.
    

Mam nadzieję, że teraz wszystko jest jasne! Jeśli masz dodatkowe pytania, chętnie Ci pomogę.