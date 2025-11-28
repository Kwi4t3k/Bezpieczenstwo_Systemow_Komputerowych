![[Pasted image 20251122100303.png]]

## a)

0. Po wywołaniu tej komendy trzeba przejść przez klika kroków tworzenia klucza
![[Pasted image 20251122102036.png]]

1. Wybieramy jaki rodzaj klucza chcemy. W tym zadaniu RSA więc opcja 1
![[Pasted image 20251122102137.png]]

2. Wybieramy długość klucza. W poleceniu nie ma żadnych wytycznych więc można przykładowo 1024
![[Pasted image 20251122102239.png]]

3. Wybieramy jak długo klucz będzie aktywny. Ja daję jeden dzień bo nie ma rzadnych informacji w poleceniu o tym
![[Pasted image 20251122102334.png]]

4. Wpisujemy dane do klucza
![[Pasted image 20251122102423.png]]

5. Wpisujemy hasło do zabezpieczenia klucza (można zostawić puste hasło i kliknąć zatwierdzenie, czasem prosi o potwierdzenie 2 razy)

| ![[Pasted image 20251122103928.png]] | ![[Pasted image 20251122103956.png]] |
| ------------------------------------ | ------------------------------------ |

6. Na koniec dostajemy informacje związane z kluczem
![[Pasted image 20251122102529.png]]

## b)

Klucz możemy wyeksportować przez jego id lub email podany przy tworzeniu (przez email może się krzaczyć jak jest kilka bo wybiera sobie samo ten najbardziej aktualny i nie ma pewności że weźmie ten co chcemy)

Trzeba podać --armor bo w kluczu będą krzaki których serwer nie zaakceptuje

![[Pasted image 20251122102659.png]]
![[Pasted image 20251122102842.png]]

![[Pasted image 20251122102953.png]]

## c)

![[Pasted image 20251122103028.png]]

## d)

![[Pasted image 20251122103046.png]]

---

![[Pasted image 20251122100315.png]]

Wybieramy typ klucza 2 bo w poleceniu jest informacja o tym, że ma być DSA
![[Pasted image 20251122103620.png]]

![[Pasted image 20251122103718.png]]

![[Pasted image 20251122103737.png]]

---

![[Pasted image 20251122100325.png]]

![[Pasted image 20251122104204.png]]
![[Pasted image 20251122104212.png]]
![[Pasted image 20251122104224.png]]
![[Pasted image 20251122104237.png]]
![[Pasted image 20251122104306.png]]
![[Pasted image 20251122104319.png]]

---

![[Pasted image 20251122100335.png]]

![[Pasted image 20251122105136.png]]
![[Pasted image 20251122105156.png]]
![[Pasted image 20251122105801.png]]
![[Pasted image 20251122105554.png]]
![[Pasted image 20251122110630.png]]
![[Pasted image 20251122110544.png]]

---

![[Pasted image 20251122100347.png]]

![[Pasted image 20251122111400.png]]
![[Pasted image 20251122111420.png]]

---

![[Pasted image 20251122100405.png]]

![[Pasted image 20251122112120.png]]
![[Pasted image 20251122112359.png]]
![[Pasted image 20251122112435.png]]
![[Pasted image 20251122112445.png]]
![[Pasted image 20251122112455.png]]

---

![[Pasted image 20251122100416.png]]

![[Pasted image 20251122113637.png]]
![[Pasted image 20251122113740.png]]
![[Pasted image 20251122113811.png]]
![[Pasted image 20251122113829.png]]

---

![[Pasted image 20251122100437.png]]

![[Pasted image 20251122120202.png]]
![[Pasted image 20251122120219.png]]

---

![[Pasted image 20251122100448.png]]
!
![[Pasted image 20251122121443.png]]
![[Pasted image 20251122121455.png]]

---

![[Pasted image 20251122100503.png]]
!
![[Pasted image 20251122123130.png]]
![[Pasted image 20251122123201.png]]

---

![[Pasted image 20251122100516.png]]

![[Pasted image 20251122144721.png]]
![[Pasted image 20251122144745.png]]

---

![[Pasted image 20251122100528.png]]

![[Pasted image 20251122145505.png]]

---

![[Pasted image 20251122100548.png]]

![[Pasted image 20251122145747.png]]

---

![[Pasted image 20251122100600.png]]

![[Pasted image 20251122152209.png]]
![[Pasted image 20251122152235.png]]
![[Pasted image 20251122152259.png]]
![[Pasted image 20251122152314.png]]

---

![[Pasted image 20251122100615.png]]

### Wszystkie klucze (publiczne + prywatne)
```
gpg --list-keys
```
![[Pasted image 20251122153201.png]]
### Tylko klucze publiczne
```
gpg --list-public-keys
```

### Tylko klucze prywatne
```
gpg --list-secret-keys
```

### Jeśli chcesz zobaczyć klucze z fingerprintami:
```
gpg --list-keys --fingerprint
```
![[Pasted image 20251122153213.png]]
### Jeśli chcesz format „długi” (pokazuje fingerprint):
```
gpg --list-keys --keyid-format long
gpg --list-secret-keys --keyid-format long
```
![[Pasted image 20251122153227.png]]

---

![[Pasted image 20251122100623.png]]

### Komendy

1. Usuwanie klucza prywatnego
```
gpg --delete-secret-keys [id klucza / mail]
```

2. Usuwanie klucza publicznego
```
gpg --delete-keys [id klucza / mail]
```

### lub wszystko za jednym razem
```
gpg --delete-secret-and-public-key [id klucza / mail]
```

![[Pasted image 20251122153900.png]]
![[Pasted image 20251122153947.png]]
![[Pasted image 20251122154218.png]]
![[Pasted image 20251122154502.png]]

---

![[Pasted image 20251122100644.png]]

---

![[Pasted image 20251122100657.png]]

