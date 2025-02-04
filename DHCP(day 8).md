1. ip adress dhcp

Model **TCP/IP** ma **4 warstwy**, ale jeśli chcesz 7 warstw, to zapewne masz na myśli **model OSI**.  

### **4-warstwowy model TCP/IP**:  
1. **Warstwa dostępu do sieci** (*Network Access Layer*)  
2. **Warstwa internetowa** (*Internet Layer*)  
3. **Warstwa transportowa** (*Transport Layer*)  
4. **Warstwa aplikacji** (*Application Layer*)  

### **7-warstwowy model OSI** (często używany do porównań z TCP/IP):  
1. **Warstwa fizyczna** (*Physical Layer*)  
2. **Warstwa łącza danych** (*Data Link Layer*)  
3. **Warstwa sieciowa** (*Network Layer*)  
4. **Warstwa transportowa** (*Transport Layer*)  
5. **Warstwa sesji** (*Session Layer*)  
6. **Warstwa prezentacji** (*Presentation Layer*)  
7. **Warstwa aplikacji** (*Application Layer*)  

TCP/IP łączy funkcje warstw sesji, prezentacji i aplikacji w jedną warstwę aplikacji.  

Jeśli potrzebujesz szczegółowego porównania TCP/IP vs OSI, daj znać! 😊

**Ramki (frames)** są odczytywane w **warstwie łącza danych** (**Data Link Layer**) w modelu **OSI**.  

### **Szczegóły**:  
- **Warstwa łącza danych (Layer 2)** odpowiada za tworzenie, przesyłanie i odczytywanie ramek.  
- Ramka to jednostka danych zawierająca adresy MAC, nagłówek, dane użytkownika oraz sumę kontrolną (FCS).  
- Protokóły warstwy 2, np. **Ethernet, Wi-Fi (802.11), PPP**, operują na ramkach.  

W modelu **TCP/IP** warstwa łącza danych jest częścią **warstwy dostępu do sieci (Network Access Layer)**.  

Masz konkretny przypadek użycia ramek, który chcesz przeanalizować? 😊

Ramki w sieci są identyfikowane na podstawie kilku kluczowych elementów, które znajdują się w ich nagłówku. W warstwie **łącza danych (Layer 2, OSI)** identyfikacja ramek odbywa się głównie poprzez **adresy MAC**, ale także inne pola pomagają w ich klasyfikacji i obsłudze.  

### **Jak ramki są identyfikowane?**  
1. **Adresy MAC (Media Access Control)**  
   - Każda ramka Ethernetowa zawiera:  
     - **Adres MAC źródłowy** (Source MAC) – unikalny adres urządzenia wysyłającego.  
     - **Adres MAC docelowy** (Destination MAC) – adres urządzenia, do którego ramka jest kierowana.  
   - Przełączniki (switches) używają tablicy MAC (CAM Table), aby kierować ramki do właściwego portu.  

2. **Typ EtherType lub pole długości**  
   - W ramkach Ethernetowych występuje pole **EtherType**, które określa, jaki protokół warstwy 3 jest przenoszony (np. **IPv4, IPv6, ARP**).  
   - Jeśli wartość jest mniejsza niż **1536 (0x0600)**, oznacza długość ramki.  

3. **Pole VLAN (opcjonalnie)**  
   - Jeśli sieć wykorzystuje VLAN-y (**IEEE 802.1Q**), w nagłówku ramki znajduje się znacznik VLAN, który określa, do jakiej sieci logicznej należy dana ramka.  

4. **Pole FCS (Frame Check Sequence)**  
   - Znajduje się na końcu ramki i służy do sprawdzenia poprawności transmisji (CRC – Cyclic Redundancy Check).  

5. **Protokół warstwy 2**  
   - Typy ramek mogą się różnić w zależności od zastosowanego protokołu warstwy łącza danych:  
     - **Ethernet** (najczęściej używany w sieciach LAN)  
     - **Wi-Fi (802.11)** – ramki zawierają dodatkowe informacje o sieci bezprzewodowej  
     - **PPP (Point-to-Point Protocol)** – używany np. w połączeniach VPN  
     - **Frame Relay, ATM** – starsze technologie WAN  

### **Podsumowanie**  
Główne metody identyfikacji ramek w sieci to **adresy MAC**, **typ EtherType**, **znaczniki VLAN**, a także **kontrola integralności (FCS)**. Przełączniki sieciowe wykorzystują adresy MAC, aby przesyłać ramki do odpowiednich urządzeń, natomiast routery analizują dane warstwy 3 (np. adresy IP) i nie operują bezpośrednio na ramkach warstwy 2.  

Chcesz przeanalizować konkretną sytuację lub zobaczyć przykładową strukturę ramki? 😊

Wędrówka pakietów IP przez urządzenia sieciowe zależy od tego, czy routing odbywa się statycznie (konfiguracja ręczna) czy dynamicznie (za pomocą protokołów routingu).  

---

## **1. Routing statyczny** (Statyczne trasy w tablicy routingu)
### **Opis:**
- Administrator ręcznie konfiguruje trasę na routerze, określając, przez który interfejs i do jakiego następnego skoku (**next-hop**) ma być kierowany ruch do danej sieci.
- Router nie uczy się tras automatycznie, a zmiany w topologii wymagają ręcznej aktualizacji.

### **Przykładowa wędrówka pakietu przy routingu statycznym:**
1. **Urządzenie źródłowe** (np. PC1: 192.168.1.10/24) chce wysłać pakiet do **serwera** (np. 10.0.0.5).
2. PC1 sprawdza, czy 10.0.0.5 jest w jego podsieci – nie jest, więc kieruje pakiet do swojej bramy domyślnej (np. 192.168.1.1 – router R1).
3. **Router R1** otrzymuje pakiet i sprawdza swoją tablicę routingu (`show ip route`):
   - Jeśli ma statyczną trasę do 10.0.0.0/24 (np. `ip route 10.0.0.0 255.255.255.0 192.168.2.2`), wysyła pakiet do routera R2.
4. **Router R2** odbiera pakiet, sprawdza tablicę routingu i znajduje statyczną trasę do 10.0.0.0/24.
5. Pakiet trafia do serwera **10.0.0.5**, który odpowiada w ten sam sposób (ale w odwrotnym kierunku).

**Zalety:**  
✅ Prosta konfiguracja w małych sieciach  
✅ Pełna kontrola nad trasami  

**Wady:**  
❌ Brak automatycznej adaptacji do zmian w topologii  
❌ Konieczność ręcznego zarządzania trasami  

---

## **2. Routing dynamiczny** (Trasy uczone automatycznie)
### **Opis:**
- Routery używają protokołów routingu (np. **RIP, OSPF, EIGRP, BGP**), aby wymieniać informacje o trasach i automatycznie aktualizować tablice routingu.
- Gdy pojawi się nowa trasa lub zmieni się topologia sieci, routery automatycznie dostosowują swoje wpisy.

### **Przykładowa wędrówka pakietu przy routingu dynamicznym (np. OSPF)**:
1. PC1 (192.168.1.10) wysyła pakiet do serwera 10.0.0.5.
2. Pakiet trafia do routera R1, który sprawdza swoją tablicę routingu (`show ip route`).
3. **Router R1 nie ma statycznych tras**, ale używa OSPF i wie, że sieć 10.0.0.0/24 jest osiągalna przez **192.168.2.2 (R2)**.
4. Pakiet jest przekazywany do R2, który sprawdza tablicę routingu i przekazuje pakiet dalej.
5. Serwer odbiera pakiet i odpowiada w ten sam sposób.

**Zalety:**  
✅ Automatyczna adaptacja do zmian w topologii  
✅ Skalowalność dla dużych sieci  

**Wady:**  
❌ Większe obciążenie procesora i pamięci routera  
❌ Może wymagać zaawansowanej konfiguracji  

---

### **Podsumowanie**
|  | **Routing statyczny** | **Routing dynamiczny** |
|---|----------------------|----------------------|
| **Konfiguracja** | Ręczna | Automatyczna |
| **Adaptacja do zmian** | Nie | Tak |
| **Zastosowanie** | Małe sieci | Średnie i duże sieci |
| **Zarządzanie** | Trudne w dużych sieciach | Łatwiejsze, bo trasy uczą się automatycznie |
| **Wydajność** | Brak obciążenia CPU | Obciążenie CPU w zależności od protokołu |

Wybór między routingiem statycznym a dynamicznym zależy od rozmiaru i złożoności sieci. **W małych sieciach domowych lub firmowych lepiej sprawdzi się routing statyczny**, natomiast **w większych środowiskach korporacyjnych dynamiczne protokoły routingu są koniecznością**.  

Masz konkretną sieć, w której chcesz skonfigurować routing? 😊

### **Jak DHCP i brama domyślna wpływają na wędrówkę pakietów w sieci?**  

**DHCP (Dynamic Host Configuration Protocol)** i **brama domyślna (default gateway)** odgrywają kluczową rolę w przesyłaniu pakietów przez sieć, niezależnie od tego, czy routing jest **statyczny** czy **dynamiczny**.  

---

## **1. DHCP – Automatyczna konfiguracja urządzeń**  
- **DHCP serwer** automatycznie przypisuje urządzeniom sieciowym (np. komputerom, telefonów, drukarkom) takie ustawienia jak:  
  - **Adres IP**  
  - **Maska podsieci**  
  - **Brama domyślna (default gateway)**  
  - **Adresy serwerów DNS**  

- **Jak to wpływa na routing?**  
  - Gdy urządzenie (np. komputer) chce wysłać pakiet do innej sieci, używa **bramy domyślnej**.  
  - Jeśli DHCP nie przypisałby poprawnej bramy domyślnej, urządzenie nie wiedziałoby, gdzie kierować pakiety poza swoją podsieć.  

### **Przykład – jak DHCP przyznaje IP i bramę domyślną**  
1. Nowy komputer podłącza się do sieci i wysyła żądanie DHCP (`DHCP Discover`).  
2. Serwer DHCP odpowiada (`DHCP Offer`), proponując adres **192.168.1.100** i informując, że brama domyślna to **192.168.1.1**.  
3. Komputer akceptuje (`DHCP Request`), a serwer potwierdza (`DHCP ACK`).  
4. Teraz komputer ma pełną konfigurację i wie, gdzie wysyłać ruch do innych sieci.  

---

## **2. Brama domyślna – Pierwszy krok w trasie pakietu**  
- **Brama domyślna** to router, który kieruje ruch poza lokalną sieć.  
- Jeśli urządzenie chce wysłać pakiet do innej podsieci, ale nie ma bezpośredniej trasy, wysyła go do bramy domyślnej.  
- Brama domyślna sprawdza swoją tablicę routingu i decyduje, gdzie przekazać pakiet dalej.

### **Przykład – jak brama domyślna przesyła pakiet**  
1. **Komputer (192.168.1.100)** chce wysłać pakiet do **10.0.0.5**.  
2. Sprawdza, czy 10.0.0.5 jest w jego podsieci – **nie jest**, więc kieruje pakiet do bramy domyślnej **192.168.1.1**.  
3. **Router (192.168.1.1)** sprawdza swoją tablicę routingu:  
   - Jeśli ma **trasę statyczną** (`ip route 10.0.0.0 255.255.255.0 192.168.2.2`), wysyła pakiet do routera R2.  
   - Jeśli używa **protokołu routingu (np. OSPF, RIP)**, może dynamicznie znaleźć najlepszą trasę do celu.  
4. Pakiet dociera do routera R2, który przesyła go do sieci 10.0.0.0/24.  
5. **Serwer (10.0.0.5)** odbiera pakiet i odsyła odpowiedź tą samą drogą.  

---

## **3. Jak to wygląda w praktyce – Routing statyczny i DHCP**  
📌 **Przykładowa konfiguracja routera z DHCP i statycznym routingiem:**  

### **Router R1 (brama domyślna dla LAN 192.168.1.0/24)**  
```cisco
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip dhcp pool LAN
  network 192.168.1.0 255.255.255.0
  default-router 192.168.1.1
  dns-server 8.8.8.8
exit
ip route 10.0.0.0 255.255.255.0 192.168.2.2
```
- **Router R1 działa jako serwer DHCP** i przypisuje klientom adresy IP oraz **bramę domyślną (192.168.1.1)**.  
- Pakiety wychodzące poza sieć **192.168.1.0/24** są kierowane na **192.168.2.2 (R2)** przez statyczną trasę.  

---

## **4. Jak to wygląda w praktyce – Routing dynamiczny i DHCP**  
📌 **Przykładowa konfiguracja routera z DHCP i dynamicznym routingiem (OSPF):**  

### **Router R1 (brama domyślna i DHCP)**
```cisco
interface GigabitEthernet0/0
 ip address 192.168.1.1 255.255.255.0
 ip dhcp pool LAN
  network 192.168.1.0 255.255.255.0
  default-router 192.168.1.1
  dns-server 8.8.8.8
exit
router ospf 1
 network 192.168.1.0 0.0.0.255 area 0
 network 192.168.2.0 0.0.0.255 area 0
```
- Router R1 działa jako **serwer DHCP** i przydziela urządzeniom adresy IP.  
- Zamiast statycznej trasy, używamy **OSPF**, który dynamicznie znajduje ścieżki do innych sieci.  

---

## **Podsumowanie**
| **Funkcja** | **Routing statyczny** | **Routing dynamiczny** |
|-------------|----------------------|----------------------|
| **DHCP** | Może przypisywać bramę domyślną, ale trasy muszą być ustawione ręcznie | Przypisuje bramę domyślną, a routery automatycznie uczą się tras |
| **Brama domyślna** | Wskazuje router, który przekierowuje ruch poza sieć lokalną | To samo, ale routery dynamicznie wymieniają informacje o trasach |
| **Zarządzanie** | Wymaga ręcznej konfiguracji tras | Automatyczna aktualizacja tras przez protokoły routingu |
| **Zastosowanie** | Małe sieci | Duże, zmienne sieci |

### 🔹 **Wniosek**  
- **DHCP** pozwala urządzeniom automatycznie uzyskać adres IP i bramę domyślną.  
- **Brama domyślna** to pierwszy punkt wyjścia dla ruchu poza podsieć.  
- **Routing statyczny** wymaga ręcznej konfiguracji tras, a **routing dynamiczny** automatycznie wymienia trasy między routerami.  

Masz jakiś konkretny przypadek do przeanalizowania? 😊

Komenda **`ip dhcp excluded-address`** w konfiguracji routera lub switcha Cisco **rezerwuje określone adresy IP**, aby nie były one przydzielane klientom przez serwer DHCP.  

### **Jak działa `ip dhcp excluded-address`?**  
- Gdy router działa jako **serwer DHCP**, domyślnie może przydzielić adresy z całej dostępnej puli.  
- **Komenda `ip dhcp excluded-address` zapobiega przydzielaniu określonych adresów**, np. dla routerów, drukarek lub serwerów, które mają mieć stałe IP.  

### **Przykład użycia:**
```cisco
ip dhcp excluded-address 192.168.1.1 192.168.1.10
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8
```
#### **Co się tutaj dzieje?**
✅ **Adresy 192.168.1.1 – 192.168.1.10 są wykluczone**, więc DHCP ich **nie przypisze klientom**.  
✅ Klienci dostaną adresy z zakresu **192.168.1.11 – 192.168.1.254**.  
✅ **Router 192.168.1.1 może być bramą domyślną**, a np. **192.168.1.2 – 192.168.1.10** mogą być używane przez serwery lub drukarki.  

### **Podsumowanie**  
📌 **`ip dhcp excluded-address`** to sposób na **zarezerwowanie IP dla ważnych urządzeń**, aby DHCP ich nie przypisywał dynamicznie.  

Chcesz zobaczyć pełną konfigurację DHCP na routerze Cisco? 😊

Komenda **`ip dhcp pool LAN`** w routerach Cisco tworzy **pulę adresów DHCP** i określa jej ustawienia.  

---

## **Jak działa `ip dhcp pool LAN`?**  
- `ip dhcp pool LAN` **tworzy pulę DHCP** o nazwie „LAN”.  
- W ramach tej puli można określić **zakres dostępnych adresów IP, bramę domyślną, serwer DNS** itp.  
- DHCP dynamicznie przydziela adresy IP urządzeniom klienckim w sieci lokalnej (LAN).  

---

### **Przykład konfiguracji DHCP na routerze Cisco**  
```cisco
ip dhcp excluded-address 192.168.1.1 192.168.1.10  ! Wyklucza IP (np. dla routera i serwerów)
ip dhcp pool LAN  ! Tworzy pulę DHCP o nazwie "LAN"
 network 192.168.1.0 255.255.255.0  ! Definiuje sieć DHCP
 default-router 192.168.1.1  ! Ustawia bramę domyślną (router)
 dns-server 8.8.8.8 1.1.1.1  ! Określa serwery DNS
 lease 7  ! Ustala czas dzierżawy IP na 7 dni
```

---

## **Co się tutaj dzieje?**
✅ **Adresy 192.168.1.1 – 192.168.1.10 są wykluczone** → DHCP ich nie przypisze.  
✅ Klienci dostaną **IP z zakresu 192.168.1.11 – 192.168.1.254**.  
✅ **Router 192.168.1.1 jest bramą domyślną**.  
✅ **Serwery DNS to 8.8.8.8 i 1.1.1.1** (Google i Cloudflare).  
✅ **Dzierżawa (lease) wynosi 7 dni** – po tym czasie klient musi odnowić IP.  

---

## **Podsumowanie**
📌 **`ip dhcp pool LAN` tworzy pulę DHCP**, a potem można określić sieć, bramę, DNS i inne parametry.  
📌 **`ip dhcp excluded-address`** zapobiega przydzielaniu niektórych IP.  
📌 **DHCP ułatwia zarządzanie IP w sieciach LAN**, automatycznie przypisując adresy urządzeniom.  

Chcesz więcej szczegółów lub przykład konfiguracji z VLAN-ami? 😊

Aby wejść do **konfiguracji DHCP (dhcp-config)** na routerze lub switchu Cisco, wykonaj następujące kroki:  

---

### **1️⃣ Wejście w tryb konfiguracji globalnej**  
Najpierw musisz przejść do trybu **konfiguracji globalnej**:  
```cisco
enable
configure terminal
```

---

### **2️⃣ Przejście do konfiguracji puli DHCP (dhcp-config)**  
Teraz tworzysz lub edytujesz pulę DHCP:  
```cisco
ip dhcp pool LAN
```
Po wpisaniu tej komendy router przechodzi do **(dhcp-config)**, gdzie możesz skonfigurować szczegóły puli DHCP.

---

### **3️⃣ Konfiguracja ustawień DHCP**  
W trybie **dhcp-config** możesz ustawić:  

✅ **Sieć i maskę podsieci:**  
```cisco
network 192.168.1.0 255.255.255.0
```
✅ **Bramę domyślną (routera dla klientów DHCP):**  
```cisco
default-router 192.168.1.1
```
✅ **Serwer(y) DNS:**  
```cisco
dns-server 8.8.8.8 1.1.1.1
```
✅ **Czas dzierżawy (lease) w dniach, godzinach i minutach:**  
```cisco
lease 7
```
✅ **Opcjonalnie: domena dla klientów DHCP:**  
```cisco
domain-name moja-siec.local
```

---

### **4️⃣ Wyjście z konfiguracji DHCP**  
Po skonfigurowaniu DHCP możesz wyjść:  
```cisco
exit
exit
write memory  ! (zapisywanie konfiguracji)
```

---

### **5️⃣ Sprawdzenie konfiguracji DHCP**  
Możesz sprawdzić ustawienia DHCP i przyznane adresy IP:  
```cisco
show ip dhcp pool
show ip dhcp binding
show running-config | section dhcp
```

---

### **🔹 Przykładowa pełna konfiguracja DHCP**  
```cisco
enable
configure terminal
ip dhcp excluded-address 192.168.1.1 192.168.1.10  ! Wykluczone adresy (np. dla routera, serwerów)
ip dhcp pool LAN
 network 192.168.1.0 255.255.255.0
 default-router 192.168.1.1
 dns-server 8.8.8.8 1.1.1.1
 lease 7
exit
write memory
```

---

## **Podsumowanie**  
🔹 **Wejście do DHCP-config:** `ip dhcp pool <nazwa>`  
🔹 **Konfiguracja sieci:** `network <IP> <maska>`  
🔹 **Ustawienie bramy:** `default-router <IP>`  
🔹 **Wyjście i zapis:** `exit`, `write memory`  

Masz konkretny scenariusz do skonfigurowania? 😊

