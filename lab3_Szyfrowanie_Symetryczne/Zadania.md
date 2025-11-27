

![[Pasted image 20251110134007.png]]

![[Pasted image 20251110140740.png]]

---

![[Pasted image 20251110134021.png]]
![[Pasted image 20251110134028.png]]

![[Pasted image 20251110143616.png]]

---

![[Pasted image 20251110134043.png]]

### Przy tym <(echo ...) DAĆ -n czyli    -->   <(echo -n ...)

![[Pasted image 20251110150941.png]]

---

![[Pasted image 20251110134056.png]]
![[Pasted image 20251110134103.png]]

![[Pasted image 20251110152915.png]]

---

![[Pasted image 20251110134122.png]]

### Informacja do obliczeń
	openssl rand -hex 24 > key
24, ponieważ --> 192 / 8 = 24

![[Pasted image 20251110153746.png]]

---

![[Pasted image 20251110134135.png]]

### Informacja do obliczeń
	w openssl rand -hex 16 > key
16, ponieważ --> 128 / 8 = 16

![[Pasted image 20251110154538.png]]

---

![[Pasted image 20251110134148.png]]
![[Pasted image 20251110134200.png]]

![[Pasted image 20251110181306.png]]
![[Pasted image 20251110181335.png]]
![[Pasted image 20251110181354.png]]

---

![[Pasted image 20251110134214.png]]

![[Pasted image 20251110182913.png]]
![[Pasted image 20251110182926.png]]
![[Pasted image 20251110182940.png]]

---

![[Pasted image 20251110134229.png]]

![[Pasted image 20251121121556.png]]
![[Pasted image 20251121121611.png]]
![[Pasted image 20251121121621.png]]
![[Pasted image 20251121121815.png]]
![[Pasted image 20251121121826.png]]
### Wyjaśnienie komendy openssl:

- `-d`: Określa, że operacja jest odszyfrowaniem.
    
- `-aes-256-ecb`: Określa użycie algorytmu AES z 256-bitowym kluczem w trybie ECB.
    
- `-in encrypted.bin`: Określa plik wejściowy, który zawiera zaszyfrowane dane.
    
- `-out decrypted.txt`: Określa plik wyjściowy, w którym zapisane będą odszyfrowane dane.
    
- `-pass pass:YOUR_PASSWORD`: Określa hasło używane do generowania klucza (może to być hasło podane w odpowiedzi).
    
- `-pbkdf2`: Określa, że klucz będzie generowany za pomocą funkcji **PBKDF2**.
    
- `-iter 356`: Określa liczbę iteracji funkcji PBKDF2 (w tym przypadku 356, jak wymagano).
    
- `-nosalt`: do szyfrowania NIE u»yto soli, wi¦c w deszyfrowaniu równie» NIE JEST potrzebna.

---

![[Pasted image 20251110134248.png]]
![[Pasted image 20251110134257.png]]

![[Pasted image 20251110185314.png]]

```
import requests

r = requests.get("http://127.0.0.1:2010/decrypt").json()

key_hex = r["key_hex"]
iv_hex = r.get("iv_hex", "")

key_len = len(key_hex)//2
iv_len = len(iv_hex)//2

print(f"key={key_len}B, iv={iv_len}B")

if iv_len == 0:
    print("ECB mode")
elif iv_len == 16:
    print("CBC AES")
elif iv_len == 8:
    print("CBC DES/3DES")

```

DOKOŃCZYĆ - nie mam pojęcia jak zrobić

---

![[Pasted image 20251110134309.png]]

![[Pasted image 20251121132549.png]]
![[Pasted image 20251121132651.png]]
![[Pasted image 20251121132749.png]]
![[Pasted image 20251121132810.png]]
![[Pasted image 20251121132833.png]]
Po tym w pliku image.enc.b64 trzeba usunąć sobie entery bo inaczej nie ogarnia (można ręcznie)
![[Pasted image 20251121132939.png]]

---

![[Pasted image 20251110134320.png]]
![[Pasted image 20251110134327.png]]

![[Pasted image 20251121135622.png]]
![[Pasted image 20251121135636.png]]
![[Pasted image 20251121135653.png]]
![[Pasted image 20251121135721.png]]
![[Pasted image 20251121135729.png]]
Gdy wykonamy to polecenie, plik image.b64 będzie miał nowe linie
![[Pasted image 20251121135808.png]]

Gdy wykonamy to polecenie, plik nie będzie miał nowych linii i przejdzie bez edytowania w serwerze
![[Pasted image 20251121140412.png]]

![[Pasted image 20251121140422.png]]