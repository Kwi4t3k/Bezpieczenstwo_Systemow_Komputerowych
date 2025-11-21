![[Pasted image 20251109155719.png]]

![[Pasted image 20251109160339.png]]

![[Pasted image 20251109160807.png]]

---

![[Pasted image 20251109155734.png]]

![[Pasted image 20251109161657.png]]

---

![[Pasted image 20251109155745.png]]

![[Pasted image 20251109162144.png]]

---

![[Pasted image 20251109155757.png]]

![[Pasted image 20251109173537.png]]
![[Pasted image 20251109173601.png]]
![[Pasted image 20251109173621.png]]

---

![[Pasted image 20251109155809.png]]

![[Pasted image 20251120212027.png]]
![[Pasted image 20251120212100.png]]

![[Pasted image 20251120212358.png]]

---

![[Pasted image 20251109155820.png]]

![[Pasted image 20251110112700.png]]
![[Pasted image 20251110112732.png]]
![[Pasted image 20251110112756.png]]

---

![[Pasted image 20251109155829.png]]

![[Pasted image 20251110113640.png]]
![[Pasted image 20251110113651.png]]
![[Pasted image 20251110113708.png]]

---

![[Pasted image 20251109155837.png]]

![[Pasted image 20251110114012.png]]
![[Pasted image 20251110114038.png]]
![[Pasted image 20251110114024.png]]

---

![[Pasted image 20251109155848.png]]
![[Pasted image 20251109155858.png]]

![[Pasted image 20251110122812.png]]
![[Pasted image 20251110123147.png]]
![[Pasted image 20251110123205.png]]

---

![[Pasted image 20251109155908.png]]

![[Pasted image 20251110124147.png]]
![[Pasted image 20251110124200.png]]
![[Pasted image 20251110124439.png]]
![[Pasted image 20251110124451.png]]

---

![[Pasted image 20251109155916.png]]

![[Pasted image 20251110124949.png]]
![[Pasted image 20251110125003.png]]
![[Pasted image 20251110130805.png]]

### Sprawdzenie - hashcat

![[Pasted image 20251121114513.png]]

![[Pasted image 20251121114459.png]]

![[Pasted image 20251121114533.png]]

### hash-mode 3200 bo:
![[Pasted image 20251121114907.png]]

![[Pasted image 20251121114822.png]]
![[Pasted image 20251121114828.png]]

---

![[Pasted image 20251109155923.png]]

![[Pasted image 20251110131142.png]]

---

![[Pasted image 20251109155932.png]]

![[Pasted image 20251110131404.png]]

---

![[Pasted image 20251109155940.png]]

```
import hashlib

# Przykładowe dane
input_text = "R3iSrSNmg9SFHxVakUD"
target_hash = "48cab4b54bef42fddaa6335c68a20b369f4002d"

def find_matching_algorithms(text: str, target: str):
    matches = []
    
    # używamy algorithms_available – daje szerszy zestaw algorytmów dostępnych na systemie
    for algo in sorted(hashlib.algorithms_available):
        try:
            # Hashujemy tekst przy pomocy algorytmu
            h = hashlib.new(algo, text.encode()).hexdigest()
        except (ValueError, TypeError):
            # Nieobsługiwany algorytm w tej konfiguracji Python
            continue
        
        # Porównujemy obliczony hash z podanym hashem
        if h == target.lower():
            matches.append((algo, h))
    
    return matches

if __name__ == "__main__":
    # Wywołanie funkcji
    res = find_matching_algorithms(input_text, target_hash)
    
    if res:
        print("Znaleziono dopasowanie:")
        for algo, h in res:
            print(f"Algorytm: {algo} → {h}")
    else:
        print("Brak dopasowania w hashlib.algorithms_available.")
        print("Upewnij się, że używasz pełnej listy algorytmów (algorithms_available) lub zainstaluj dodatkowe algorytmy (np. RIPEMD-160) w Twojej wersji OpenSSL/Python.")

```

![[Pasted image 20251110133602.png]]