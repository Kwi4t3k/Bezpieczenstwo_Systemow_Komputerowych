## Przydatne curle
curl -X POST 'http://localhost:port/submit/public' -F "file=@pub.key"

curl -X POST 'http://localhost:port/submit/public' -F "file=tekst_z_pliku"

curl -X POST 'http://localhost:port/submit' -H 'Content-Type: application/json' -d '{"session_id":"id_sesji", "decrypted_text":" ' "$(cat decrypted.txt)" ' "}'

## Przydatne komendy
gpg --list-keys `wyświetlwnie wszystkich kluczy`
gpg --list-public-keys `wyświetlwnie publicznych kluczy`
gpg --list-secret-keys `wyświetlwnie prywatnych kluczy`
gpg --list-keys --fingerprint
gpg --list-keys --keyid-format long

gpg --delete-secret-keys id_klucza `usuwanie klucza prywatnego`
gpg --delete-keys id_klucza `usuwanie klucza publicznego`
gpg --delete-secret-and-public-key id_klucza `usuwanie obu kluczy za jednym razem`

### GENEROWANIE kluczy GPG
gpg --full-generate-key

### EKSPORT klucza publicznego do pliku
gpg --armor --export id_klucza > pub.key

### EKSPORT klucza prywatnego do pliku
gpg --armor --expor-secret-key id_klucza > priv.key

### IMPORT klucza publicznego / prywatnego do swojego zbioru
gpg --import key.asc

### ODSZYFROWANIE pliku używając algorytmu CAMELLIA128 i hasła z pliku
gpg --cipher-algo CAMELLIA128 --decrypt encrypted.txt > decrypted.txt
`wpisać hasło z passphrase.txt`

### SZYFROWANIE pliku ze słowem za pomocą algorytmu AES128 i pliku z hasłem + base64(armor)
gpg --symmetric --cipher-algo AES128 --armor --output encrypted.txt word_to_encrypt.txt

### SZYFROWANIE pliku ze słowem kluczem publicznym + base64 - bez hasła
gpg --encrypt --recipient id_klucza --armor --output encrypted.txt word.txt

### ODSZYFROWANIE pliku ze słowem za pomocą pobranego klucza prywatnego
gpg --decrypt --recipient id_klucza --output decrypted_word.txt encrypted_word.txt

### PODPIS pliku ze słowem za pomocą pobranego klucza publicznego
chodzi o import prywatnego klucza bo tylko prywatnym się podpisuje

gpg -u id_klucza --sign --output word.txt.gpg word.txt

gpg -u id_klucza --detach-sign --output word.txt.sig word.txt `utworzenie oddzielnego podpisu`

gpg -u id_klucza --clearsign --output word.txt.sig word.txt `utworzenie czytelnego podpisu`

### PODPIS importowanego klucza publicznego swoim prywatnym + eksport
1. gpg --full-generate-key
2. gpg --armor --export id_klucza > pub.key
3. gpg --import server_key.asc
4. gpg --local-user id_klucza_podpisującego --sign-key id_klucza_do_podpisania `podpis`
5. gpg --export --armor id_klucza_do_podpisania > signed_key.asc

### WERYFIKACJA PODPISU za pomocą importowanego klucza publicznego
gpg --verify word.txt.gpg
