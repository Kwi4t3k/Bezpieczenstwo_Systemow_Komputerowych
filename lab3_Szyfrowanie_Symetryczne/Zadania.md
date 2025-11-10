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
	w openssl rand -hex 24 > key
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

DOKOŃCZYĆ

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

DOKOŃCZYĆ

---

![[Pasted image 20251110134309.png]]

DOKOŃCZYĆ

---

![[Pasted image 20251110134320.png]]
![[Pasted image 20251110134327.png]]

DOKOŃCZYĆ