![[Pasted image 20251110192108.png]]

![[Pasted image 20251115112643.png]]
![[Pasted image 20251116112817.png]]
![[Pasted image 20251115112736.png]]

---

![[Pasted image 20251110192118.png]]

![[Pasted image 20251115113247.png]]
![[Pasted image 20251115113258.png]]
![[Pasted image 20251115113317.png]]

---

![[Pasted image 20251110192618.png]]

## Musi być jq -r, bo bez -r tworzy się w pliku "xyz" zamiast xyz

![[Pasted image 20251115115839.png]]
![[Pasted image 20251115115848.png]]
![[Pasted image 20251115115922.png]]

---

![[Pasted image 20251110192635.png]]

![[Pasted image 20251115120811.png]]
![[Pasted image 20251115120823.png]]
![[Pasted image 20251115121615.png]]

---

![[Pasted image 20251110192717.png]]

![[Pasted image 20251115122020.png]]
![[Pasted image 20251115122033.png]]
![[Pasted image 20251115122314.png]]

---

![[Pasted image 20251110192816.png]]

![[Pasted image 20251115123550.png]]
![[Pasted image 20251115123558.png]]
![[Pasted image 20251115123614.png]]

---

![[Pasted image 20251110193002.png]]

![[Pasted image 20251115124443.png]]
![[Pasted image 20251115124502.png]]
![[Pasted image 20251115124524.png]]
![[Pasted image 20251115124547.png]]

---

![[Pasted image 20251110193030.png]]

![[Pasted image 20251115125306.png]]
![[Pasted image 20251115125319.png]]

---

![[Pasted image 20251110193449.png]]

DOKOŃCZYĆ bo se nie chce działać kij wie dlaczegoXD

![[Pasted image 20251115131545.png]]
![[Pasted image 20251115131559.png]]

---

![[Pasted image 20251110193459.png]]

![[Pasted image 20251115132346.png]]
![[Pasted image 20251115132417.png]]
![[Pasted image 20251115132437.png]]

---

![[Pasted image 20251110193514.png]]

## Więcej informacji o komendach można znaleźć pod __man__ (poniżej - man crunch)
![[Pasted image 20251115133323.png]]

![[Pasted image 20251115133431.png]]
![[Pasted image 20251115133451.png]]
![[Pasted image 20251115133521.png]]

---

![[Pasted image 20251110193527.png]]
![[Pasted image 20251110193536.png]]

![[Pasted image 20251115134112.png]]
![[Pasted image 20251115134128.png]]

---

![[Pasted image 20251110193547.png]]

![[Pasted image 20251115135153.png]]
![[Pasted image 20251115135203.png]]
![[Pasted image 20251115135216.png]]

---

![[Pasted image 20251110193558.png]]
![[Pasted image 20251110193607.png]]

![[Pasted image 20251115135607.png]]
![[Pasted image 20251115135618.png]]

---

![[Pasted image 20251110193622.png]]

![[Pasted image 20251115144359.png]]
![[Pasted image 20251115144419.png]]
![[Pasted image 20251115144433.png]]

---

![[Pasted image 20251110193634.png]]

![[Pasted image 20251116100149.png]]
![[Pasted image 20251116100203.png]]

---

![[Pasted image 20251110193647.png]]
![[Pasted image 20251110193655.png]]

![[Pasted image 20251116100946.png]]
![[Pasted image 20251116101051.png]]

---

![[Pasted image 20251110193706.png]]

## Zrobienie zasady w pliku rule.txt

sa@se3si1
tłumaczenie: 
- s - do zamiany
- sa@ - zamień 'a' na '@'
- se3 - zamień 'e' na '3'
- si1 - zamień 'i' na '1'

![[Pasted image 20251116110033.png]]
![[Pasted image 20251116110056.png]]

---

![[Pasted image 20251110193718.png]]
![[Pasted image 20251110193726.png]]

![[Pasted image 20251116112712.png]]
![[Pasted image 20251116112729.png]]

---

![[Pasted image 20251110193737.png]]

![[Pasted image 20251116142342.png]]
![[Pasted image 20251116142637.png]]

---

![[Pasted image 20251110193754.png]]

## Sposób 1 - fcrackzip

![[Pasted image 20251116161135.png]]

## Sposób 2 - hashcat + zip2john

![[Pasted image 20251116163336.png]]
![[Pasted image 20251116163356.png]]
## można wyczyścić plik ręcznie lub

![[Pasted image 20251116163520.png]]

![[Pasted image 20251116163548.png]]
![[Pasted image 20251116163606.png]]

---

![[Pasted image 20251110193807.png]]
![[Pasted image 20251110193815.png]]

## Metoda1 - hashcat + zip2john + crunch

![[Pasted image 20251116164753.png]]
![[Pasted image 20251116164813.png]]
![[Pasted image 20251116164825.png]]

## Metoda2 - hashcat (brute-force) + zip2john

![[Pasted image 20251116165344.png]]

## użyć attack mode 3

![[Pasted image 20251116165409.png]]
jak nie wyjdzie to:

![[Pasted image 20251116165428.png]]
![[Pasted image 20251116165447.png]]

## Metoda3 - fcrackzip

Chat mówi że tutaj się nie da

---

![[Pasted image 20251110193834.png]]
![[Pasted image 20251110193844.png]]

## Metoda 1 - pdfcrack

![[Pasted image 20251116175911.png]]

## Generator PESEL w Python

```
from datetime import date, timedelta   # Importujemy klasę date i timedelta do pracy z datami

# Definiujemy zakres dat wymagany w zadaniu:
# od 1 lipca 1992 do 31 grudnia 1992
start = date(1992, 7, 1)                # Data początkowa
end   = date(1992, 12, 31)              # Data końcowa

# Wagi wymagane do obliczenia cyfry kontrolnej PESEL
weights = [1, 3, 7, 9, 1, 3, 7, 9, 1, 3]

# Funkcja obliczająca cyfrę kontrolną PESEL
def checksum(core):
    # core – pierwsze 10 cyfr PESEL jako string
    s = sum(int(digit) * weight for digit, weight in zip(core, weights))  
    # Obliczamy sumę ważoną zgodnie ze wzorem
    return str((10 - (s % 10)) % 10)    
    # Wynik (cyfra kontrolna) – ostatnia cyfra PESEL

# Otwieramy plik wyjściowy, do którego zapisany zostanie słownik PESEL
with open("peselist.txt", "w") as f:
    
    d = start                            # Zaczynamy od pierwszej daty zakresu
    
    while d <= end:                      # Iterujemy po dniach aż do końca zakresu
        
        y = d.year % 100                 # Dwie ostatnie cyfry roku
        m = d.month                      # Miesiąc (1–12)
        dd = d.day                       # Dzień miesiąca (1–31)
        
        # Seria trzycyfrowa (000–999)
        for ser in range(1000):
            
            # Cyfry oznaczające płeć — parzyste dla kobiet
            for sex in (0, 2, 4, 6, 8):
                
                # Składamy pierwsze 10 cyfr PESEL:
                core = f"{y:02d}{m:02d}{dd:02d}{ser:03d}{sex}"
                
                # Obliczamy cyfrę kontrolną
                last = checksum(core)
                
                # Zapisujemy pełny PESEL (11 cyfr) do pliku
                f.write(core + last + "\n")
        
        # Przechodzimy do następnego dnia
        d += timedelta(days=1)

# Informacja dla użytkownika
print("Wygenerowano slownik peselist.txt")
```

![[Pasted image 20251116180237.png]]
## Metoda 2 - hashcat + pdf2john

![[Pasted image 20251116180628.png]]
![[Pasted image 20251116180653.png]]