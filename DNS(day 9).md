Plik **`/etc/resolv.conf`** w systemach Linux i Unix przechowuje konfigurację klienta DNS, określając, z jakich serwerów DNS system powinien korzystać.  

### **Zawartość `resolv.conf`**  
Typowy plik wygląda tak:  
```bash
nameserver 8.8.8.8
nameserver 1.1.1.1
search localdomain
options ndots:5 timeout:2 attempts:3
```

### **Kluczowe wpisy w `resolv.conf`**  
1. **`nameserver <adres IP>`**  
   - Określa adresy IP serwerów DNS (maksymalnie 3 wpisy).  
   - System używa ich w kolejności, w jakiej są podane.  
   - Przykład:  
     ```bash
     nameserver 8.8.8.8
     nameserver 8.8.4.4
     ```

2. **`search <domena>`**  
   - Definiuje domyślną domenę do wyszukiwania niepełnych nazw.  
   - Przykład:  
     ```bash
     search example.com
     ```
   - Gdy wpiszesz `server1`, system spróbuje `server1.example.com`.  

3. **`options`**  
   - Dodatkowe parametry:  
     - `ndots:<n>` – ile kropek musi mieć nazwa, by system traktował ją jako pełną domenę (domyślnie `1`).  
     - `timeout:<n>` – czas oczekiwania na odpowiedź serwera DNS (domyślnie `5` sekund).  
     - `attempts:<n>` – liczba prób połączenia (domyślnie `2`).  
     - `rotate` – używa serwerów DNS naprzemiennie zamiast w kolejności.  

---

### **Jak sprawdzić aktualną konfigurację?**  
W terminalu:  
```bash
cat /etc/resolv.conf
```

### **Jak zmienić `resolv.conf`?**  
Plik można edytować ręcznie:  
```bash
sudo nano /etc/resolv.conf
```
Jednak wiele systemów (np. Ubuntu z NetworkManager) automatycznie nadpisuje `resolv.conf`.  
Aby uniknąć nadpisywania, można ustawić:  
```bash
sudo chattr +i /etc/resolv.conf  # Blokada zmian
```
Aby cofnąć blokadę:  
```bash
sudo chattr -i /etc/resolv.conf
```

Chcesz pomocy z trwałą konfiguracją DNS na Twoim systemie? 😊

Polecenie:  
```bash
curl -v 8.8.8.8
```
robi następujące rzeczy:  

### **Rozbicie składni:**
- **`curl`** – narzędzie do wykonywania żądań HTTP i pobierania zasobów z internetu.  
- **`-v` (verbose)** – włącza tryb szczegółowy, który wyświetla dodatkowe informacje o połączeniu.  
- **`8.8.8.8`** – adres IP serwera, do którego wykonywane jest żądanie HTTP (w tym przypadku Google Public DNS).  

### **Co się stanie po uruchomieniu?**
- **`curl` spróbuje połączyć się z `8.8.8.8` na porcie 80** (domyślny port HTTP).  
- **W trybie `-v` zobaczysz szczegółowe informacje** o połączeniu, np.:  
  - Próba nawiązania połączenia TCP.  
  - Rozpoznawanie DNS (w tym przypadku niepotrzebne, bo podano IP).  
  - Wysłane nagłówki HTTP.  
  - Otrzymane odpowiedzi serwera.  

### **Przykładowy wynik (`curl -v 8.8.8.8`)**
```
*   Trying 8.8.8.8:80...
* Connected to 8.8.8.8 (8.8.8.8) port 80 (#0)
> GET / HTTP/1.1
> Host: 8.8.8.8
> User-Agent: curl/7.81.0
> Accept: */*
> 
< HTTP/1.1 404 Not Found
< Content-Length: 156
< Content-Type: text/html; charset=UTF-8
```
### **Co oznacza wynik?**
- **`Trying 8.8.8.8:80...`** – Próbujemy połączyć się na port 80.  
- **`Connected to 8.8.8.8`** – Połączenie zostało nawiązane.  
- **`GET / HTTP/1.1`** – `curl` wysłał żądanie HTTP do serwera.  
- **`404 Not Found`** – Serwer (Google DNS) nie obsługuje zapytań HTTP i zwraca błąd.  

### **Czy `curl -v 8.8.8.8` ma sens?**
Nie bardzo, ponieważ `8.8.8.8` to serwer DNS, a nie serwer HTTP.  
Jeśli chcesz przetestować działanie DNS, użyj:  
```bash
nslookup google.com 8.8.8.8
dig @8.8.8.8 google.com
```

Chcesz przetestować coś konkretnego? 😊

Polecenie `dig www.google.com` używa narzędzia **`dig`** (Domain Information Groper) do wykonania zapytania DNS o adres IP przypisany do domeny `www.google.com`.  

### Co dokładnie robi?
1. **Zapytanie do serwera DNS**  
   `dig` wysyła zapytanie do serwera DNS w celu uzyskania rekordów powiązanych z `www.google.com`.
   
2. **Pobranie adresu IP**  
   Domyślnie zwracany jest rekord **A** (IPv4) lub **AAAA** (IPv6), czyli adres(y) IP, pod którym działa `www.google.com`.

3. **Wyświetlenie szczegółowych informacji**  
   Wynik zawiera:
   - **Nagłówek**: informacje o zapytaniu, np. kod odpowiedzi.
   - **Sekcję QUESTION**: zapytanie DNS (domena, typ rekordu).
   - **Sekcję ANSWER**: odpowiedź DNS (adresy IP).
   - **Sekcję AUTHORITY**: serwery autorytatywne (jeśli dotyczy).
   - **Sekcję ADDITIONAL**: dodatkowe informacje (np. inne adresy IP).

### Przykładowy wynik:
```
; <<>> DiG 9.16.1-Ubuntu <<>> www.google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 0

;; QUESTION SECTION:
;www.google.com.		IN	A

;; ANSWER SECTION:
www.google.com.	300	IN	A	142.250.180.4
www.google.com.	300	IN	A	142.250.180.5

;; Query time: 10 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Sat Feb 10 12:00:00 2025
;; MSG SIZE  rcvd: 60
```
### Przydatne opcje:
- `dig +short www.google.com` – zwraca tylko adresy IP.
- `dig www.google.com MX` – sprawdza rekordy MX (serwery pocztowe Google).
- `dig @8.8.8.8 www.google.com` – używa konkretnego serwera DNS (np. Google DNS).
- `dig -x 142.250.180.4` – odwrotne wyszukiwanie DNS (PTR).

Chcesz sprawdzić coś konkretnego w DNS? 🚀