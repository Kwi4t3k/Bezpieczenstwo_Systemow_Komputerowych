### ODPOWIEDZI - Student 1 ###

=== ZADANIE 1 - OpenSSL === zapisać
1. openssl genpkey -algorithm RSA -out privkey.pem -pkeyopt rsa_keygen_bits:2048
openssl pkey -in privkey.pem -pubout -out pubkey.pem

2. openssl dgst -sha256 -sign privkey.pem -hex message.txt

=== ZADANIE 2 - GnuPG ===
1. gpg --import instructor_key.asc

2. gpg --verify message_to_verify.sig

3. // gpg --armor --export-secret-key id_klucza_instructor_key > priv.key
gpg --encrypt --recipient id_klucza_instructor_key --armor --output encrypted.b64 <(echo -n "Szyfrowanie_1_91huSS6AZPsK20FKcpXz")

=== ZADANIE 3 - OpenSSL ===
1. echo -n "Wiadomosc_1_w2WTfPgk0wTDt0MJOkoa" | openssl dgst -sha224

=== ZADANIE 4 - OpenSSL ===
1. stworzyć plik IV, który ma w sobie 24 jedynki i zapisać jako IV ?
openssl rand -hex 24 > key ?

2. openssl enc -aes-192-cbc -in <(echo -n "Wiadomosc_1_i0VpEBOWfbZAVaBSo63b") -out zaszyfrowany_plik_wyjściowy.enc -K cec3dee4fed149ea38a7c64af3bed003f2b430a38972559f -iv "'"$(cat IV)"'" -base64

=== ZADANIE 5 - Certyfikat ===
1. openssl x509 -in cert.pem -text -noout

=== ZADANIE 6 - Łamanie hasha
1. echo -n "b0f40e2bace115cb1a24e26ac4759cf7ab9ed0466a54f9ce7742a1b6da3c77b7" > hash.txt

2. hashcat -m 1400 -a 3 hash.txt ?d?d?d?d

3. hashcat -m 1400 hash.txt --show

### KONIEC ODPOWIEDZI ###