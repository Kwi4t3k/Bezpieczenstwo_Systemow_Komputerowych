### ZAD1 
###### Zahaszuj słowo Linux za pomocą algorytmu MD5.
echo -n "Linux" | openssl dgst -md5 > word.txt

echo -n "Linux" | md5sum > word.txt

### ZAD2 
###### Zahaszuj słowo Linux za pomocą algorytmu SHA-256.
echo -n "Linux" | openssl dgst -SHA-256 > word.txt

echo -n "Linux" | sha256sum > word.txt

### ZAD3 
###### Zakoduj słowo Linux za pomocą kodowania base64.
echo -n "Linux" | base64 > word.b64

1. echo -n "Linux" > word.txt
2. base64 word.txt > word.b64

### ZAD4 
###### Odkoduj słowo VU1DUw== zakodowane za pomocą base64.
1. echo -n "VU1DUw\=\=" > encoded_word.b64
2. cat encoded_word.b64 | base64 -d > decoded_word.txt

echo -n "VU1DUw\=\=" | base64 -d > decoded_word.txt

### ZAD5 
###### Zaszyfruj słowo Linux za pomocą algorytmu AES-256-ECB i klucza e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e.
openssl enc -AES-256-ecb -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -in <(echo -n "Linux") -out word.enc

1. echo -n "Linux" > word.txt
2. openssl enc -AES-256-ecb -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -in word.txt -out word.enc

### ZAD6 
###### Zaszyfruj słowo Linux za pomocą AES-256-CFB, klucza e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e oraz IV 83c468dc477543a6906912b6a1344416.
openssl enc -AES-256-CFB -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc477543a6906912b6a1344416 -in <(echo -n "Linux") -out word.enc

### ZAD7 
###### Zaszyfruj słowo Linux za pomocą AES-256-CFB, klucza e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e oraz IV 83c468dc477543a6906912b6a1344416. Wynik szyfrowania zakoduj za pomocą kodowania base64.
openssl enc -AES-256-CFB -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc477543a6906912b6a1344416 -in <(echo -n "Linux") -out word.enc -base64

openssl enc -AES-256-CFB -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc477543a6906912b6a1344416 -in <(echo -n "Linux") -out word.enc -a


### ZAD8 
###### Słowo f6NB8w== zostało zaszyfrowane algorytmem AES-256-CFB, kluczem e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e oraz IV 83c468dc477543a6906912b6a1344416, a następnie zakodowane za pomocą kodowania base64. Odszyfruj i odkoduj słowo.
1. echo -n "f6NB8w\=\=" | base64 -d > word.enc
2. openssl enc -d -AES-256-CFB -in word.enc -out decrypted_word.txt -K e592bc9e5fa8618a02edf437bdf3ffd4fcdfbe9e588749db898a3662b461a80e -iv 83c468dc477543a6906912b6a1344416

### ZAD9 
###### Wygeneruj klucz szyfrujący o długości 128 bitów.
openssl rand -hex 16 > key

### ZAD10 
###### Zaszyfruj słowo Linux za pomocą AES-128-CBC, bez soli, hasło do generowania klucza to Ubuntu, funkcja generowania klucza to PBKDF2, z liczbą iteracji równą 256.
openssl enc -AES-128-CBC -in <(echo -n "Linux") -out word.enc -nosalt -pass pass:Ubuntu -pbkdf2 -iter 256 -base64

### ZAD11 
###### Wygeneruj parę kluczy (publiczny i prywatny), klucze mają 2048 bitów. Wyeksportuj klucze do pliku.
openssl genpkey -algorithm RSA -out privkey.pem -pkeyopt rsa_keygen_bits:2048 
openssl pkey -in privkey.pem -pubout -out pubkey.pem

### ZAD12 
###### Wygeneruj parę kluczy (publiczny i prywatny), klucze mają 2048 bitów. Wyeksportuj klucze do pliku. Zaszyfruj słowo Linux kluczem publicznym i odszyfruj prywatnym. Użyj paddingu OAEP.
1. openssl genpkey -algorithm RSA -out priv.pem -pkeyopt rsa_keygen_bits:2048
2. openssl pkey -in priv.pem -pubout -out pub.pem
3. openssl pkeyutl -encrypt -pubin -inkey pub.pem -pkeyopt rsa_padding_mode:oaep -in <(echo -n "Linux") -out word.enc
4. openssl pkeyutl -decrypt -inkey priv.pem -pkeyopt rsa_padding_mode:oaep -in word.enc -out word.txt

### ZAD13
###### Wygeneruj parę kluczy (publiczny i prywatny), klucze mają 2048 bitów. Wyeksportuj klucze do pliku. Zapisz do pliku słowo Linux. Podpisz plik używając klucza prywatnego, z paddingiem PSS: hash SHA-256, długość soli powinna odpowiadać długości funkcji skrótu. Zweryfikuj wygenerowany podpis.
1. openssl genpkey -algorithm RSA -out privkey.pem -pkeyopt rsa_keygen_bits:2048
2. openssl pkey -in privkey.pem -pubout -out pubkey.pem
3. echo -n "Linux" > word.txt
4. openssl dgst -sha256 -binary -out word.sha256 word.txt
5. openssl pkeyutl -sign -in word.sha256 -inkey privkey.pem -pkeyopt digest:sha256 -pkeyopt rsa_padding_mode:pss -pkeyopt rsa_pss_saltlen:32 -out signature.bin
6. openssl dgst -sha256 -verify pubkey.pem -sigopt rsa_padding_mode:pss -sigopt rsa_pss_saltlen:32 -signature signature.bin word.txt

### ZAD14
###### Złam hash MD5: e2fc714c4727ee9395f324cd2e7f331f za pomocą: 
1) ataku słownikowego, 
2) generując swój własny słownik, widząc, że hasło ma 4 znaki i składa się z małych liter a-z 
3) za pomocą maski, widząc, że hasło ma 4 znaki i składa się z małych liter a-z 

echo -n "e2fc714c4727ee9395f324cd2e7f331f" > hash.txt

crunch 4 4 -t @@@@ -o slownik.txt
hashcat -m 0 -a 0 hash.txt slownik.txt

hashcat -m 0 -a 3 hash.txt ?l?l?l?l
hashcat -m 0 hash.txt --show

### ZAD15 
###### Wygeneruj parę kluczy GPG dla maila student@umcs.pl. Klucz RSA, 1024 bity. Wyeksportuj klucz publiczny i prywatny do pliku.
1. gpg --full-generate-key (1 -> 1024 -> student@umcs.pl)
2. gpg --armor --export id_klucza > pub.key
3. gpg --armor --export-secret-key id_klucza > priv.key

### ZAD16 
###### Zaszyfruj słowo Linux algorytmem AES z kluczem 128 bitów, z hasłem Ubuntu używając GPG. Wynik zapisz do pliku. Odszyfruj plik.
1. gpg --symmetric --cipher-algo AES128 --armor --output word.enc <(echo -n "Linux")
2. wpisać hasło Ubuntu w okienku
3. gpg --decrypt --cipher-algo AES128 --armor --output wprd.txt word.enc

### ZAD17 
###### Wygeneruj parę kluczy RSA (GPG) z domyślnymi parametrami. Zaszyfruj słowo Linux przy użyciu klucza publicznego. Wynik szyfrowania zapisz do pliku. Odszyfruj plik.
1. gpg --full-generate-key
2. gpg --armor --export id_klucza > pub.key
3. gpg --armor --export-secret-key id_klucza > priv.key
4. gpg --encrypt --recipient id_klucza --armor --output word.enc <(echo -n "Linux")
5. gpg --decrypt --recipient id_klucza --output word.dec word.enc

### ZAD18 
###### Wygeneruj parę kluczy RSA (GPG) z domyślnymi parametrami. Zapisz słowo Linux do pliku, podpisz plik przy użyciu klucza publicznego. Użyj kodowania base64. Wynik podpisywania zapisz do pliku. Zweryfikuj podpisany plik.
1. gpg --full-generate-key
2. gpg --armor --export id_klucza > pub.key
3. gpg --armor --export-secret-key id_klucza > priv.key
4. echo -n "Linux" > word.txt
5. gpg -u id_klucza --armor --sign --output word.sig word.txt
6. gpg --verify word.sig