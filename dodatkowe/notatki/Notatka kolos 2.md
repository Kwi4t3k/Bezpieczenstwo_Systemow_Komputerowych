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

## CROSS SITE SCRIPTING (XSS) - ZADANIA
![[Pasted image 20260124140053.png]]

a)

![[Pasted image 20260124141435.png]]

Na stronie widać że tekst jest do nas odbijany przez serwer więc do ataku pasuje Reected XSS

![[Pasted image 20260124141757.png]]

![[Pasted image 20260124141907.png]]

kod strony:
![[Pasted image 20260124142140.png]]

to jest niebezpieczne ponieważ od pobrania inputu przez użytkownika do wyświetlenia nic nie robi się z tymi danymi

b)

funkcja escape robi to, że znaki są zamieniane w taki sposób że nie mogą się wykonać tylko są encjami

![[Pasted image 20260124142603.png]]

![[Pasted image 20260124142823.png]]

![[Pasted image 20260124140103.png]]

dane do logowania: hasło - password
![[Pasted image 20260124143239.png]]

zmiana poziomów:
![[Pasted image 20260124143454.png]]

- low
![[Pasted image 20260124143601.png]]
![[Pasted image 20260124143638.png]]
![[Pasted image 20260124143711.png]]

- medium
![[Pasted image 20260124143759.png]]

działa jak napiszemy `<SCRIPT>` capsem, bo jest zabezpieczenie na tylko `<script>` oraz nie działa funkcja rekurencyjnie więc zadziała `<scr<script>ipt>alert("a")</scr</script>ipt>`
![[Pasted image 20260124143819.png]]
![[Pasted image 20260124143840.png]]

- high
![[Pasted image 20260124162117.png]]
![[Pasted image 20260124162142.png]]
![[Pasted image 20260124162319.png]]
słowo script zostało zamienione na ciąg znaków

- impossible
![[Pasted image 20260124162734.png]]
wszystkie znaki zostają zmienione na encje

![[Pasted image 20260124140139.png]]

Jeśli mamy ograniczenie długości wiadomości to w firefox można `CTRL + SHIFT + K` i z narzędzi dla twórców witryn można zmienić kod strony i zmienić długość pola

- low (wszystkie inputy są od razu przekazywane do bazy danych)
![[Pasted image 20260124163420.png]]

- medium
![[Pasted image 20260124163713.png]]

- high
![[Pasted image 20260124163743.png]]

- impossible
![[Pasted image 20260124163815.png]]

![[Pasted image 20260124140155.png]]

- low
![[Pasted image 20260124164652.png]]
![[Pasted image 20260124165031.png]]

- medium
![[Pasted image 20260124165236.png]]
![[Pasted image 20260124165526.png]]

- high
![[Pasted image 20260124165325.png]]
![[Pasted image 20260124165504.png]]

- impossible
![[Pasted image 20260124165428.png]]
![[Pasted image 20260124165442.png]]

## SQL Injection - ZADANIA
![[Pasted image 20260124171408.png]]

![[Pasted image 20260124184224.png]]
![[Pasted image 20260124184313.png]]
![[Pasted image 20260124184333.png]]
![[Pasted image 20260124184416.png]]

![[Pasted image 20260124171421.png]]

![[Pasted image 20260124185642.png]]
![[Pasted image 20260124185755.png]]

![[Pasted image 20260124171430.png]]

`https://www.youtube.com/watch?v=0-D-e66U2Z0`

![[Pasted image 20260124192931.png]]
![[Pasted image 20260124193015.png]]

![[Pasted image 20260124171438.png]]

`https://www.youtube.com/watch?v=frymuDxKwmc&list=PL8j1j35M7wtKXpTBE6V1RlN_pBZ4StKZw&index=48`

trzeba dodać jakieś review
![[Pasted image 20260124194047.png]]
![[Pasted image 20260124194334.png]]
![[Pasted image 20260124194742.png]]

teraz wszystkie komentarze na stronie to `:D`

![[Pasted image 20260124194821.png]]

![[Pasted image 20260124171449.png]]

- low

![[Pasted image 20260124195438.png]]
![[Pasted image 20260124195450.png]]

to jest znak, że aplikacja jest podatna na tego typu atak

![[Pasted image 20260124195617.png]]

szukanie nazw kolumn `' UNION SELECT table_name, NULL FROM information_schema.tables #`

![[Pasted image 20260124201057.png]]

pobranie haseł `' UNION SELECT user, password FROM users #`

![[Pasted image 20260124201308.png]]

![[Pasted image 20260124195803.png]]

DODATKOWO sprawdzenie wersji bazy danych `' UNION SELECT version(), database() FROM users #`
![[Pasted image 20260124201517.png]]

- medium



- high



![[Pasted image 20260124171458.png]]

możemy użyć payload `1' and sleep(5) #` i jeżeli po pięciu sekundach to się wykona, a nie od razu to znaczy że jest podatność



![[Pasted image 20260124201726.png]]

![[Pasted image 20260124171508.png]]

