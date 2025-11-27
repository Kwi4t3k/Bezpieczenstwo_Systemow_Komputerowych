## Przydatne curle
curl -X GET http://localhost:port/hash (wyświetla wynik w terminalu)

curl -X POST http://localhost:port/submit -H 'accept: application/json' -H 'Content-Type: application/json' -d '{"session_id":"id_sesji", "hash_hex":"przykładowy_hash_hex"}'

---

## Wyświetlenie listy algorytmów
openssl list -cipher-algorithms
opcjonalnie szukajka: openssl list -cipher-algorithms | grep "blowfish"

---

### Obliczenie skrótu MD5 / SHA-256 / SHA-512 otrzymanego słowa, kodując wynik w hex
echo -n "przykładowe_słowo" | openssl dgst `-md5` `-SHA-256` `-SHA-512`

### Obliczenie skrótu Argon
W kontenerze dockerowym

openssl kdf -keylen 24 -kdfopt pass:przykładowe_słowo -kdfopt salt:przykładowa_sól -kdfopt iter:przykładowa_iteracja -kdfopt memcost:przykładwa_pamięć ARGON2D | tr -d ':' | tr '[:upper]' '[:lower]'

### Obliczenie skrótu bcrypt
użyte narzędzie htpasswd

-n -> wyświetlanie wyniku w terminalu
-b -> użycie hasła z linii komend
-B -> wymuś bcrypt

podman run docker.io/mazurkatarzyna/htpasswd:lastest -nbB username przykładowe_słowo

### Użycie hash-identifier mając początkowy ciąg znaków i hash
1. podman run -it docker.io/mazurkatarzyna/hash-identifier:lastest
2. wklejamy hash do sprawdzenia
3. dostajemy wynik w possible hashs np. SHA-1
4. sprawdzamy wynik -> echo -n "przykładowe słowo" | openssl dgst -sha-1

### Użycie hashid (jak hash-identifier nie zadziała)
1. hashid
2. od nowej linii wklejamy hash
3. sprawdzenie -> echo -n "przykładowe_słowo" | openssl dgst -algorytm LUB hashcat

### hashcat użycie
1. tworzenie pliku z hashem (może być zwykłe txt)
2. tworzenie pliku ze słowem
3. hashcat -m (numerek konkternego algorytmu) -a 0 (zwykł atak słownikowy bez masek) hash.txt word.txt
4. wyświetlenie wyniku -> hashcat -m (numerek konkternego algorytmu) hash.txt (początkowy plik z hashem) --show