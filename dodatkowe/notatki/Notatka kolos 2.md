## Transport Layer Security (TLS) - TEORIA

> Certyfikaty TLS (Transport Layer Security) stanowią podstawowy mechanizm zapewniający poufność, integralność oraz uwierzytelnienie w komunikacji sieciowej. Wykorzystywane są m.in. przez protokół HTTPS, który chroni ruch między przeglądarką a serwerem.

### Infrastruktura klucza publicznego (PKI)
PKI (Public Key Infrastructure) to zestaw technologii, procedur oraz podmiotów, które umożliwiają bezpieczne zarządzanie kluczami publicznymi i certyfikatami cyfrowymi. PKI zapewnia: 
- wiarygodną identyfikację podmiotów, 
- mechanizm podpisywania i weryfikacji certyfikatów, 
- hierarchię zaufania między uczestnikami, 
- procedury unieważniania certyfikatów. 
 
 Główne elementy PKI to:
 - Root CA – główny urząd certyfikacji o najwyższym poziomie zaufania, 
 - Intermediate CA – pośrednie urzędy certyfikacji, 
 - RA (Registration Authority) – urząd rejestracji weryfikujący tożsamość podmiotów, 
 - Repozytoria certyfikatów i CRL – listy certyfikatów ważnych i unieważnionych, 
 - Podmioty końcowe – np. serwery HTTPS.

### Diagram hierarchii zaufania PKI
![[Pasted image 20260123131433.png]]

### Klucze publiczne i algorytmy
Certyfikat TLS zawiera klucz publiczny serwera wraz z informacjami identyfikacyjnymi. Najczęściej stosowane algorytmy kryptograficzne to RSA, ECDSA oraz Ed25519. Klucz publiczny służy klientowi do weryfikowania podpisu certyfikatu oraz do bezpiecznej wymiany kluczy sesyjnych.

### Hierarchia zaufania
System certyfikatów opiera się na hierarchii zaufania PKI. Root CA podpisuje certyfikaty pośrednie, a one certyfikaty końcowe. Przeglądarka ufa certyfikatowi serwera, jeśli może odtworzyć pełny łańcuch zaufania aż do zaufanego Root CA.

### Generowanie i podpisywanie certyfikatów
Proces tworzenia certyfikatu rozpoczyna się od wygenerowania pary kluczy: prywatnego oraz publicznego. Następnie tworzy się żądanie podpisania certyfikatu (CSR), które zawiera klucz publiczny i dane identyfikacyjne. CSR jest wysyłane do CA, które po weryfikacji informacji podpisuje certyfikat swoim kluczem prywatnym i zwraca gotowy certyfikat TLS.

### Zastosowanie certyfikatów
Głównym celem certyfikatów TLS jest zapewnienie, że użytkownik łączy się z właściwym serwerem, a komunikacja jest szyfrowana. Bez certyfikatów TLS możliwe byłyby ataki polegające na podszywaniu się pod serwer (np. man-in-the-middle).

### Certyfikaty self-signed
Certyfikat self-signed (samopodpisany) to certyfikat, który został podpisany własnym kluczem prywatnym właściciela certyfikatu, zamiast kluczem zaufanego urzędu certyfikacji (CA). Oznacza to, że wystawca oraz podmiot certyfikatu są tym samym bytem. Taki certyfikat nadal jest poprawny technicznie — zawiera klucz publiczny, dane oraz podpis — jednak nie należy do globalnej hierarchii zaufania PKI.

#### Po co stosuje się certyfikaty self-signed?
Certyfikaty self-signed stosuje się przede wszystkim: 
- w środowiskach testowych i deweloperskich, 
- w systemach wewnętrznych, gdzie administratorzy ręcznie dodają certyfikat do magazynu zaufanych, 
- jako certyfikat główny prywatnego CA (tzw. private PKI).

#### Dlaczego przeglądarki zgłaszają błąd?
Przeglądarki mają wbudowaną listę zaufanych Root CA. Ponieważ certyfikat self-signed nie został podpisany przez żaden z nich, nie można zbudować łańcucha zaufania. Przeglądarka nie ma podstaw, by zaufać wystawcy, więc wyświetla ostrzeżenie o możliwym zagrożeniu.

![[Pasted image 20260123132712.png]]

## Transport Layer Security (TLS) - ZADANIA
![[Pasted image 20260122174406.png]]

b) 

![[Pasted image 20260123133102.png]]

c) 
- Dla jakiej domeny wystawiony jest certyfikat? 

![[Pasted image 20260122175425.png]]
![[Pasted image 20260123133608.png]]

- Kto wystawił certyfikat (CA)? 

![[Pasted image 20260122175449.png]]
![[Pasted image 20260123133829.png]]

- Kiedy certyfikat został wystawiony i kiedy wygasa? 

![[Pasted image 20260122175512.png]]
![[Pasted image 20260123134029.png]]

- Jakie dodatkowe domeny są obsługiwane? 

![[Pasted image 20260122175604.png]]
![[Pasted image 20260123134209.png]]

- Jaki algorytm i długość klucza? 

![[Pasted image 20260122175706.png]]

- Do czego może być użyty certyfikat? 

![[Pasted image 20260122180222.png]]

- Ile certyfikatów znajduje się w łańcuchu zaufania? 

![[Pasted image 20260123134729.png]]

- Kto jest głównym CA (root CA) w łańcuchu?

![[Pasted image 20260123135228.png]]

d) 
![[Pasted image 20260122180053.png]]

![[Pasted image 20260122174414.png]]

- liczba certyfikatów
![[Pasted image 20260123135716.png]]

- który certyfikat należy do serwera
![[Pasted image 20260123135744.png]]

	Interpretacja:
	
	- wpis **`0 s:`** → certyfikat **serwera** (leaf)
	    
	- wpisy **`1 s:`**, **`2 s:`** → certyfikaty **CA (intermediate)**

![[Pasted image 20260122174425.png]]

b)
![[Pasted image 20260123140334.png]]

c)

- Dla jakiej domeny wystawiony jest certyfikat? 

![[Pasted image 20260123141000.png]]

- Kto wystawił certyfikat (CA)? 

![[Pasted image 20260123141009.png]]

- Kiedy certyfikat został wystawiony i kiedy wygasa? 

![[Pasted image 20260123141018.png]]

- Jakie dodatkowe domeny są obsługiwane? 

![[Pasted image 20260123141030.png]]

- Jaki algorytm i długość klucza?

![[Pasted image 20260123141044.png]]

- Do czego może być użyty certyfikat?

![[Pasted image 20260123141058.png]]

- Ile certyfikatów znajduje się w łańcuchu zaufania? 

![[Pasted image 20260123141109.png]]

- Kto jest głównym CA (root CA) w łańcuchu?

![[Pasted image 20260123141117.png]]

![[Pasted image 20260122174432.png]]

![[Pasted image 20260123141322.png]]

![[Pasted image 20260122174442.png]]

b)
![[Pasted image 20260123141650.png]]

c) 
![[Pasted image 20260123142436.png]]

d)
![[Pasted image 20260123144904.png]]

![[Pasted image 20260123145755.png]]

![[Pasted image 20260122174449.png]]

przed odszyfrowaniem

![[Pasted image 20260123171856.png]]

po odszyfrowaniu
![[Pasted image 20260123172011.png]]

## Wprowadzenie do protokołu HTTP / testowania web aplikacji - ZADANIA
![[Pasted image 20260123173907.png]]

1. narzędzia programistyczne przeglądarki
po odpaleniu serwera `CTRL + SHIFT + K` w przeglądarce firefox

![[Pasted image 20260123175102.png]]

widać:
- wersję php
- wersję openssl
- wersję serwera apache

To są informacje wrażliwe, ponieważ mogą one zostać wykorzystane przez atakującego. Może sobie sprawdzić jakie dla tych konkretnych wersji istnieją podatności.

2. curl
![[Pasted image 20260123175615.png]]

3.  Burp Suite

- wyłączenie pierw tego 
![[Pasted image 20260123175849.png]]

- później otworzyć przeglądarkę
![[Pasted image 20260123175943.png]]

- wchodzimy na stronę serwera z wbudowanej przeglądarki
![[Pasted image 20260123180010.png]]

- w HTTP history mamy wszystkie requesty, które do serwera zostały wysłane
![[Pasted image 20260123180136.png]]

- jeśli jest request, który chcemy edytować, klikamy sobie prawym na ten request i robimy `Send to Repeater`

`Repeater` to narzędzie, które powtarza nam request. Powtarza w taki sposób, że możemy edytować dowolne rzeczy.

![[Pasted image 20260123180303.png]]

- zostawiamy minimalne wymagane nagłówki do działania (Host i Connection)
![[Pasted image 20260123181144.png]]

- po kliknięciu przycisku SEND dostajemy odpowiedź
![[Pasted image 20260123181741.png]]

![[Pasted image 20260123182032.png]]

Zmieniamy User-Agent
![[Pasted image 20260123182553.png]]![[Pasted image 20260123182837.png]]
![[Pasted image 20260123183708.png]]

![[Pasted image 20260123183727.png]]

Intruder to narzędzie, które potrafi zmieniać request tak jak sobie zażyczymy i wysyłać go dowolną ilość razy.

![[Pasted image 20260123184053.png]]

![[Pasted image 20260123184151.png]]

![[Pasted image 20260123184635.png]]

trzeba szukać jakichś różnic na przykład w długości
![[Pasted image 20260123184800.png]]

![[Pasted image 20260123184813.png]]

![[Pasted image 20260123184854.png]]

![[Pasted image 20260123185125.png]]

![[Pasted image 20260123185338.png]]

![[Pasted image 20260123185434.png]]

![[Pasted image 20260123185445.png]]

![[Pasted image 20260123185524.png]]

![[Pasted image 20260123190153.png]]
![[Pasted image 20260123190206.png]]

![[Pasted image 20260123190221.png]]

![[Pasted image 20260123190334.png]]

![[Pasted image 20260123191316.png]]

![[Pasted image 20260123191424.png]]

![[Pasted image 20260123191439.png]]

![[Pasted image 20260123191616.png]]

![[Pasted image 20260123191812.png]]

![[Pasted image 20260123193833.png]]

![[Pasted image 20260123200205.png]]

![[Pasted image 20260123200238.png]]

![[Pasted image 20260123200302.png]]

![[Pasted image 20260123200319.png]]

![[Pasted image 20260123200342.png]]

![[Pasted image 20260123200404.png]]

![[Pasted image 20260123200501.png]]

![[Pasted image 20260123201013.png]]

![[Pasted image 20260123201040.png]]