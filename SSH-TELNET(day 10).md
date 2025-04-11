Hakerzy atakują przez przejmowanie pakietów.

Polecenie `crypto key generate rsa` w systemach Cisco IOS i podobnych służy do generowania klucza RSA, który jest wykorzystywany do zabezpieczonych połączeń, takich jak SSH (Secure Shell) oraz SSL/TLS.

### Składnia podstawowa:
```
crypto key generate rsa [modulus <size>] [label <name>] [usage-keys] [exportable]
```

### Kluczowe opcje:
- **`modulus <size>`** – Określa długość klucza w bitach (zwykle 1024, 2048 lub 4096). Im dłuższy klucz, tym wyższy poziom bezpieczeństwa, ale większe zużycie zasobów.
- **`label <name>`** – Nadaje nazwę kluczowi, co może być przydatne do identyfikacji.
- **`usage-keys`** – Tworzy osobne klucze dla podpisywania i szyfrowania (np. dla VPN).
- **`exportable`** – Umożliwia eksportowanie klucza do użycia na innym urządzeniu.

### Przykład użycia:
Aby wygenerować klucz RSA o długości 2048 bitów dla SSH:
```
Router(config)# crypto key generate rsa modulus 2048
```
Po uruchomieniu polecenia system może zapytać o potwierdzenie włączenia SSH.

Czy potrzebujesz dodatkowych informacji? 😊

Klucze **RSA** i **EC** (Elliptic Curve) różnią się pod względem algorytmu kryptograficznego, poziomu bezpieczeństwa i wydajności. Oto kluczowe różnice:

### 🔑 **1. Algorytm i matematyka**
- **RSA (Rivest-Shamir-Adleman)**: Opiera się na trudności faktoryzacji dużych liczb pierwszych.
- **EC (Elliptic Curve Cryptography - ECC)**: Bazuje na problemie dyskretnego logarytmu krzywych eliptycznych, który przy tej samej długości klucza jest znacznie trudniejszy do złamania niż RSA.

### ⚡ **2. Wydajność i bezpieczeństwo**
- **RSA**:
  - Wymaga dłuższych kluczy dla tego samego poziomu bezpieczeństwa (np. RSA 2048 ≈ EC 256).
  - Wolniejszy w generowaniu kluczy i operacjach podpisywania, ale szybszy w weryfikacji.
- **EC**:
  - Krótsze klucze przy tym samym poziomie bezpieczeństwa (np. EC 256-bit ≈ RSA 3072-bit).
  - Szybszy i mniej zasobożerny, co ma znaczenie w urządzeniach mobilnych i IoT.

### 📊 **3. Długości kluczy i poziom bezpieczeństwa**
| Poziom bezpieczeństwa | RSA (w bitach) | ECC (w bitach) |
|----------------------|--------------|--------------|
| 80-bit              | 1024         | 160          |
| 128-bit             | 3072         | 256          |
| 192-bit             | 7680         | 384          |
| 256-bit             | 15360        | 512          |

### 🔄 **4. Zastosowania**
- **RSA**:
  - Powszechnie stosowany w certyfikatach SSL/TLS (choć coraz częściej zastępowany przez ECC).
  - SSH, PGP, podpisy cyfrowe.
- **ECC**:
  - Używany w nowoczesnych systemach, takich jak TLS 1.3, Apple Secure Enclave, Bitcoin i inne kryptowaluty.
  - Popularny w mobilnych i wbudowanych systemach ze względu na niskie zużycie mocy.

### ✅ **Który wybrać?**
- Jeśli zależy Ci na **kompatybilności**, RSA jest szeroko obsługiwany.
- Jeśli ważna jest **wydajność i bezpieczeństwo**, ECC jest lepszym wyborem.

Czy masz jakieś konkretne zastosowanie na myśli? 😊

Polecenie:  
```
show run | sec line
```
jest używane w systemach Cisco IOS i służy do **filtrowania konfiguracji bieżącej (`show running-config`) w taki sposób, że wyświetlane są tylko sekcje zawierające słowo "line"**.

---

### 🔍 **Analiza składni**:
- **`show run`** – Wyświetla bieżącą konfigurację urządzenia.
- **`| sec line`** – Filtruje wynik tak, aby wyświetlać tylko sekcje konfiguracji, w których występuje słowo „line” oraz wszystko, co po nich następuje.

---

### 📌 **Co wyświetli to polecenie?**
Najczęściej sekcje dotyczące konfiguracji linii dostępu, np.:
```
line con 0
 password cisco
 login
!
line vty 0 4
 transport input ssh
 login local
!
line aux 0
```
Oznacza to, że polecenie **pokaże konfigurację konsoli (`line con`), linii wirtualnych (`line vty` - SSH/Telnet) i portu pomocniczego (`line aux`)**.

---

### 🛠 **Zastosowanie w praktyce**
1. **Szybkie sprawdzenie ustawień dostępu** (np. konfiguracja logowania, transportu SSH/Telnet).
2. **Sprawdzenie, czy linie wymagają uwierzytelnienia** (czy jest hasło i metoda logowania).
3. **Analiza dostępu zdalnego do urządzenia**.

Chcesz przetestować jakieś inne filtry? 😊

Polecenie:  
```
transport input telnet ssh
```
jest używane w konfiguracji linii **VTY (Virtual Teletype)** na urządzeniach Cisco i określa, jakie protokoły mogą być używane do zdalnego dostępu do urządzenia.

---

### 🔍 **Analiza składni**:
- **`transport input`** – Definiuje dozwolone protokoły transportowe dla linii VTY.
- **`telnet ssh`** – Zezwala zarówno na połączenia **Telnet**, jak i **SSH**.

---

### 📌 **Gdzie to polecenie jest stosowane?**
Wchodzi w skład konfiguracji linii VTY, np.:
```
Router(config)# line vty 0 4
Router(config-line)# transport input telnet ssh
```
Oznacza to, że urządzenie pozwala użytkownikom łączyć się zdalnie za pomocą **Telnetu lub SSH**.

---

### ⚠ **Bezpieczeństwo – czy to dobry pomysł?**
- **Telnet przesyła dane (w tym hasła) w postaci czystego tekstu**, co jest bardzo niebezpieczne.
- **SSH zapewnia szyfrowane połączenie i jest zalecanym protokołem**.
- Lepsza praktyka to **zezwolenie tylko na SSH**:
  ```
  transport input ssh
  ```

Chcesz sprawdzić, czy Twój router obsługuje SSH i czy jest poprawnie skonfigurowany? 😊

W systemach Cisco IOS występują różne typy **linii dostępu (line)**, które pozwalają na łączenie się z urządzeniem w różnych trybach. Oto ich kluczowe różnice:

---

### 🔹 **1. Line CTY (Console – `line con 0`)**
- **Typ połączenia:** Fizyczne, lokalne (port konsoli).
- **Opis:** 
  - Służy do konfiguracji urządzenia bezpośrednio przez port **console** (RJ-45, USB lub szeregowy).
  - Jest pierwszym miejscem, gdzie można uzyskać dostęp do routera/switcha.
  - Nie wymaga aktywnej sieci.
- **Typowe ustawienia:**
  ```
  line con 0
   password cisco
   login
  ```
- **Gdzie używać?**  
  - Podczas pierwszej konfiguracji urządzenia.
  - W sytuacjach awaryjnych (np. brak dostępu sieciowego).

---

### 🔹 **2. Line VTY (Virtual Terminal – `line vty 0 4`)**
- **Typ połączenia:** Zdalne (przez sieć).
- **Opis:**  
  - Umożliwia zdalny dostęp do urządzenia poprzez **Telnet lub SSH**.
  - Numeracja `line vty 0 4` oznacza, że można mieć **5 jednoczesnych sesji** (0,1,2,3,4).
  - Konieczna jest poprawna konfiguracja loginu i hasła.
- **Typowe ustawienia:**
  ```
  line vty 0 4
   transport input ssh
   login local
  ```
- **Gdzie używać?**  
  - Gdy chcesz zarządzać urządzeniem zdalnie, np. przez SSH.

---

### 🔹 **3. Line AUX (Auxiliary – `line aux 0`)**
- **Typ połączenia:** Fizyczne, zdalne (port AUX).
- **Opis:**  
  - Służy do **zdalnego zarządzania przez modem szeregowy** (np. przez dial-up).
  - Jest podobny do portu konsoli, ale pozwala na zdalne łączenie.
  - Może być używany jako dodatkowy dostęp w sytuacjach awaryjnych.
- **Typowe ustawienia:**
  ```
  line aux 0
   password cisco
   login
  ```
- **Gdzie używać?**  
  - W systemach, gdzie dostęp do sieci jest zawodny, a potrzebny jest modem zapasowy.

---

### 🔥 **Podsumowanie: kiedy używać jakiej linii?**
| Typ linii  | Połączenie | Fizyczne/Zdalne | Przykład użycia |
|------------|-----------|----------------|----------------|
| **CTY** (Console) | Port konsoli | Fizyczne | Pierwsza konfiguracja, awaryjny dostęp |
| **VTY** (Virtual Terminal) | Telnet / SSH | Zdalne | Standardowe zarządzanie przez sieć |
| **AUX** (Auxiliary) | Modem szeregowy | Fizyczne/Zdalne | Awaryjne połączenie przez modem |

Czy masz jakieś konkretne pytania dotyczące konfiguracji tych linii? 😊