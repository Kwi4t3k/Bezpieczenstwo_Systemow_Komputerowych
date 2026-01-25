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

## Wprowadzenie do protokołu HTTP / testowania web aplikacji - TEORIA
> Protokół HTTP (Hypertext Transfer Protocol, RFC 2616 oraz od 7230 do 7235) - protokół warstwy aplikacji, wykorzystujący na niższej warstwie (zazwyczaj) gniazda TCP/IP oraz 2 domyślne porty: port niezabezpieczony 80 i port zabezpieczony: 443.

Podstawowe informacje: 
- Protokół HTTP jest protokołem wykorzystywanym do przesyłania plików (ogólnie mówiąc: zasobów) w sieci WWW (World Wide Web), bez względu na to, czy zasobem jest plik HTML, plik graficzny, wynik zapytania, czy cokolwiek innego 
- Protokół HTTP, do wersji 1.1, jest protokołem tekstowym, gdzie komendy protokołu, podobnie jak w SMTP, POP3 czy IMAP są komendami tekstowymi, zrozumiałymi dla człowieka 
- HTTP to protokół typu zapytanie-odpowiedź. Zapytanie, wysyłane przez klienta, zawiera informację o żądanym zasobie. Odpowiedź, wysyłana przez serwer, zawiera treść zasobu. Jeśli serwer nie jest w stanie zwrócić odpytywanego zasobu, odpowiedź zawiera kod reprezentujący powód, dla którego zasób nie mógł być wysłany (np. zasób nie istnieje) 
- Formaty zapytania i odpowiedzi HTTP są do siebie podobne; zarówno zapytanie, jak i odpowiedź HTTP zawierają (linia początkowa i nagłówki powinny się kończyć parą znaków CRLF, czyli \r\n): 
	- linię początkową 
	- 0 lub więcej nagłówków 
	- pustą linię (CRLF, czyli \r\n) 
	- opcjonalne ciało wiadomości

Przykład:
>linia poczatkowa, inna dla zadania, inna dla odpowiedzi \r\n
>naglowek1: wartosc1 \r\n
>naglowek2: wartosc2 \r\n
>naglowek3: wartosc3 \r\n
>\r\n
>cialo wiadomosci, moze sie skladac z 1 lub wielu linii, lub moze byc puste

- Nagłówki HTTP to wszelkie komendy używane do komunikacji między przeglądarką WWW (klientem) a serwerem. Nagłówki są to właściwości żądania i odpowiedzi przesyłane wraz z samą wiadomością. Służą one przede wszystkim do sterowania zachowaniem serwera oraz przeglądarki przez nadawcę wiadomości.
- Jeśli klient wysyła żądanie do serwera HTTP, żądanie powinno zawsze być zakończone parą znaków CRLF (czyli \r\n)
- Serwer odsyłając odpowiedź HTTP nie określa za pomocą żadnych specjalnych znaków końca odsyłanej odpowiedzi. W przypadku, gdy chcemy mieć pewność, że odebraliśmy całą odpowiedź serwera HTTP, musimy parsować odebrane nagłówki (Content-Length lub Transfer-Encoding), w których może znajdować się informacja o tym, jaki jest rozmiar odpowiedzi serwera, i użyć tej inforamcji do odebrania całej wiadomości. W przypadku, gdy serwer w odpowiedzi HTTP nie odeśle żadnego z powyższych nagłówków, aby mieć pewność odebrania całej odpowiedzi od serwera, musimy odbierać dane, dopóki serwer nie zakończy / zamknie połącznia. Zgodnie z formatem żądania i odpowiedzi HTTP, nagłówki od ciała oddzielają znaki CRLF CRLF (czyli \r\n \r\n).

### Podstawowa komunikacja HTTP
![[Pasted image 20260125135412.png]]

### Żądania HTTP

- Ogólny format żądania HTTP (pola oddzielone spacjami):
	>Method Request-URI HTTP-Version \r\n
HEADER1: VALUE1 \r\n 
HEADER2: VALUE2 \r\n 
... 
HEADERX: VALUEX \r\n
\r\n 
BODY 
\r\n

	gdzie: 
	- Method - to metoda żądania, dozwolone metody HTTP:
		- GET – pobranie zasobu wskazanego przez Request-URI
		- HEAD – pobiera informacje o zasobie, stosowane do sprawdzania dostępności zasobu
		- PUT – przyjęcie danych przesyłanych od klienta do serwera, najczęściej aby zaktualizować wartość zasobu,
		- POST – przyjęcie danych przesyłanych od klienta do serwera (np. wysyłanie zawartości formularzy),
		- DELETE – żądanie usunięcia zasobu,
		- OPTIONS – informacje o opcjach i wymaganiach dotyczących zasobu,
		- TRACE – diagnostyka, analiza kanału komunikacyjnego,
		- CONNECT – żądanie przeznaczone dla serwerów pośredniczących pełniących funkcje tunelowania,
		- PATCH – aktualizacja części zasobu (np. jednego pola).
	- Request-URI - to ścieżka do zasobu na serwerze, która może zawierać dodatkowo parametry HTTP oraz fragment (za znakiem #),
	- HTTP-Version - wersja protokołu HTTP, np. HTTP/1.0, HTTP/1.1, HTTP/2.0
	- HEADER1, HEADER2, ..., HEADERX - nagłówki HTTP, VALLUE1, VALUE2, ..., VALUEX - wartości konkretnych nagłówków
	- BODY - opcjonalne ciało żądania
---
#### Metody HTTP (co robią i po co) - INNY OPIS

- **GET** – pobranie zasobu (np. strona, JSON). Nie powinno zmieniać danych.
    
- **POST** – utworzenie zasobu / wysłanie danych (np. formularz, logowanie).
    
- **PUT** – wstawienie lub pełna aktualizacja zasobu (zwykle “cały obiekt”).
    
- **PATCH** – częściowa aktualizacja (np. zmiana jednego pola).
    
- **DELETE** – usunięcie zasobu.
    
- **HEAD** – jak GET, ale bez body (tylko nagłówki) – np. sprawdzanie, czy plik istnieje.
    
- **OPTIONS** – jakie metody są dozwolone; używane m.in. w CORS (przeglądarki robią “preflight”).
    
- **CONNECT** – tunel (np. HTTPS przez proxy).
    
- **TRACE** – diagnostyka (rzadko, często wyłączane).
---
#### Jak wygląda żądanie HTTP (request)
Przykład:

```
GET /api/products?search=apple HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 ...
Accept: application/json
Cookie: session=abc123; theme=dark

```

**Elementy:**

1. **Linia startowa**: metoda + ścieżka + wersja HTTP
    
2. **Nagłówki** (Headers): meta-informacje
    
3. **Pusta linia**
    
4. **Body** (opcjonalnie): dane (np. JSON w POST/PUT/PATCH)

### Odpowiedzi HTTP

- Ogólny format odpowiedzi HTTP (pola oddzielone spacjami):
>HTTP-Version Status-Code Reason-Phrase \r\n 
>HEADER1: VALUE1 \r\n 
>HEADER2: VALUE2 \r\n 
>... 
>HEADERX: VALUEX \r\n 
>\r\n 
>BODY 
>\r\n

gdzie: 

	- HTTP-Version - wersja protokołu HTTP, np. HTTP/1.0, HTTP/1.1, HTTP/2.0
	- Status-Code - kod odpowiedzi, który informuje klienta, w jaki sposób żądanie zostało lub nie zostało obsłużone, kody odpowiedzi to liczby trzycyfrowe, gdzie pierwsza z nich określa grupę odpowiedzi:
		- 1xx - to kody informacyjne
		- 2xx - to kody powodzenia
		- 3xx - to kody przekierowania
		- 4xx - to kody błędu aplikacji klienta
		- 5xx - to kody błędu serwera
	- Reason-Phrase - wiadomość powiązana z danym kodem odpowiedzi
	- HEADER1, HEADER2, ..., HEADERX - nagłówki HTTP, VALLUE1, VALUE2, ..., VALUEX - wartości konkretnych nagłówków
	- BODY - opcjonalne ciało żądania
---
#### Kody odpowiedzi HTTP (status codes)
Status to 3 cyfry, które mówią, co stało się z żądaniem.

**Grupy:**

- **1xx** Informacyjne (rzadko widoczne): np. 101 Switching Protocols
    
- **2xx** Sukces:
    
    - **200 OK** – wszystko OK
        
    - **201 Created** – utworzono zasób (np. po POST)
        
    - **204 No Content** – OK, ale bez treści (np. po DELETE)
        
- **3xx** Przekierowania:
    
    - **301 Moved Permanently** – stałe przekierowanie
        
    - **302 Found** – tymczasowe przekierowanie
        
    - **304 Not Modified** – użyj cache (brak zmian)
        
- **4xx** Błąd po stronie klienta (Twoje żądanie jest złe/nieuprawnione):
    
    - **400 Bad Request** – serwer nie rozumie żądania (zła składnia, brak danych, zły format)
        
    - **401 Unauthorized** – brak uwierzytelnienia (np. brak/niepoprawny token)
        
    - **403 Forbidden** – rozpoznano, ale brak dostępu
        
    - **404 Not Found** – zasób nie istnieje
        
    - **405 Method Not Allowed** – metoda niedozwolona dla endpointu
        
    - **429 Too Many Requests** – limit żądań (rate limit)
        
- **5xx** Błąd po stronie serwera:
    
    - **500 Internal Server Error** – ogólny błąd aplikacji/serwera
        
    - **502 Bad Gateway** – brama/proxy dostała złą odpowiedź od serwera „za nią” (np. od aplikacji/upstream)
        
    - **503 Service Unavailable** – usługa niedostępna (przeciążenie/maintenance)
        
    - **504 Gateway Timeout** – upstream nie odpowiedział na czas
        

**Co się dzieje gdy serwer zwraca…**

- **400**: klient zwykle powinien poprawić żądanie (payload, format JSON, nagłówki).
    
- **404**: zły URL albo zasób nie istnieje.
    
- **500**: bug lub błąd po stronie aplikacji/serwera.
    
- **502/504**: problem po drodze (proxy/load balancer) albo z serwerem backendowym.

### Dozwolone nagłówki HTTP

- **Pola nagłówków ogólnych (General Header Fields)** – to kilka pól nagłówków, które mają ogólne zastosowanie zarówno dla komunikatów żądania, jak i odpowiedzi, ale nie dotyczą bezpośrednio przesyłanej treści (encji).
    
- **Pola nagłówków encji (Entity Header Fields)** – definiują metainformacje o treści encji (entity-body) albo, jeśli treść nie występuje, o zasobie wskazanym przez żądanie.
    
- **Pola nagłówków żądania (Request Header Fields)** – pozwalają klientowi przekazać serwerowi dodatkowe informacje o żądaniu oraz o samym kliencie.
    
- **Pola nagłówków odpowiedzi (Response Header Fields)** – pozwalają serwerowi przekazać dodatkowe informacje o odpowiedzi, których nie da się umieścić w linii statusu (Status-Line). Te pola dostarczają informacji o serwerze oraz o dalszym dostępie do zasobu wskazanego przez URI żądania (Request-URI).
---
#### Najważniejsze nagłówki (Headers)

**Częste w żądaniu (Request):**

- **Host** – domena (w HTTP/1.1 obowiązkowe)
    
- **User-Agent** – opis klienta (przeglądarka/aplikacja)
    
- **Accept** – jakie formaty klient akceptuje (np. `application/json`)
    
- **Content-Type** – format wysyłanych danych (np. `application/json`, `application/x-www-form-urlencoded`)
    
- **Authorization** – dane uwierzytelnienia (np. Bearer token)
    
- **Cookie** – ciasteczka wysyłane do serwera
    

**Częste w odpowiedzi (Response):**

- **Content-Type** – format odpowiedzi
    
- **Set-Cookie** – ustawienie ciasteczka po stronie klienta
    
- **Location** – adres przekierowania (np. przy 301/302)
    
- **Cache-Control** – reguły cache
    
- **Server** – informacja o serwerze (czasem ukrywana)
- **X-Forwarded-For** (nagłówek żądania) – wyjątkowo ciekawy nagłówek z potencjałem naruszania bezpieczeństwa
- **Strict-Transport-Security** (nagłówek odpowiedzi) – jeden z nagłówków mogących wprost zwiększyć bezpieczeństwo aplikacji
- **Referer**(nagłówek żądania) - jego wartością jest adres URL strony poprzednio odwiedzanej przez użytkownika (aby przeglądarka nie wysyłała nagłówków Referer* z naszej domeny, można użyć nagłówka odpowiedzi: Referrer-Policy: no-referrer)
---
#### Cookie i nagłówki cookie

**Cookie (Request header)** – klient wysyła:

`Cookie: session=abc123; lang=pl`

**Set-Cookie (Response header)** – serwer ustawia:

`Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Lax; Path=/`

Znaczenie atrybutów:

- **HttpOnly** – JS nie odczyta cookie (ochrona przed kradzieżą przez XSS)
    
- **Secure** – cookie tylko po HTTPS
    
- **SameSite** – ogranicza wysyłanie między stronami (ochrona przed CSRF)
    
- **Path/Domain** – gdzie cookie obowiązuje
    
- **Expires/Max-Age** – czas ważności
---
#### User-Agent

Nagłówek **User-Agent** identyfikuje klienta (np. przeglądarkę, system).  
Bywa używany do statystyk, dopasowania widoku, czasem do blokowania botów (nie jest to silne zabezpieczenie, bo łatwo go podrobić).

### Przykłady żądań i odpowiedzi HTTP

Żądanie: 
```
GET /index.html HTTP/1.1 
HOST: 212.182.24.27
```

Odpowiedź:
```
HTTP/1.1 200 OK
Date: Thu, 13 Apr 2017 14:25:38 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Thu, 13 Apr 2017 13:57:13 GMT
ETag: "2c39-54d0cb3af4405"
Accept-Ranges: bytes
Content-Length: 11321
Vary: Accept-Encoding
Content-Type: text/html

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
"http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
	<head>
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" />
		<title>Apache2 Ubuntu Default Page: It works</title>
	</head>
	<body>
	...
	</body>
</html>
```
---
Żądanie:
```
TRACE / HTTP/1.1
HOST: 212.182.24.27
```

Odpowiedź:
```
HTTP/1.1 405 Method Not Allowed
Date: Thu, 13 Apr 2017 14:31:22 GMT
Server: Apache/2.4.18 (Ubuntu)
Allow:
Content-Length: 302
Content-Type: text/html; charset=iso-8859-1

<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html>
<head>
<title>405 Method Not Allowed</title>
</head><body>
<h1>Method Not Allowed</h1>
<p>The requested method TRACE is not allowed for the URL /.</p>
<hr>
<address>Apache/2.4.18 (Ubuntu) Server at 212.182.24.27 Port 80</address>
</body>
</html>

```
---
Żądanie:
```
OPTIONS /index.html HTTP/1.1
HOST: 212.182.24.27
```

Odpowiedź:
```
HTTP/1.1 200 OK
Date: Thu, 13 Apr 2017 14:52:31 GMT
Server: Apache/2.4.18 (Ubuntu)
Allow: OPTIONS,GET,HEAD,POST
Content-Length: 0
Content-Type: text/html
```
---
Żądanie:
```
HEAD /index.html HTTP/1.1
HOST: 212.182.24.27
```

Odpowiedź:
```
HTTP/1.1 200 OK
Date: Thu, 13 Apr 2017 14:53:06 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Thu, 13 Apr 2017 13:57:13 GMT
ETag: "2c39-54d0cb3af4405"
Accept-Ranges: bytes
Content-Length: 11321
Vary: Accept-Encoding
Content-Type: text/html
```
---
### Kodowanie procentowe (kodowanie URL)
Działa ono w prosty sposób: kod ASCII znaku & to szesnastkowo 26. W kodowaniu procentowym %26. Jeśli z kolei chcemy użyć spacji w URL, musimy ją zakodować jako %20 (lub jako +). Z tego powodu użycie znaku „plus” też musi być zakodowane (jako %2b), w przeciwnym wypadku oznaczałoby zakodowaną spację.

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

## CROSS SITE SCRIPTING (XSS) - TEORIA


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

