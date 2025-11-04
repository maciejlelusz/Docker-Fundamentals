# Docker Fundamentals — Moduł: Sieci Macvlan i VLAN w Dockerze

W tym module nauczysz się korzystać z **interfejsów sieciowych typu Macvlan** w Dockerze.  
Pozwalają one na przypisanie kontenerom **unikalnych adresów MAC i IP** w tej samej sieci fizycznej co host, dzięki czemu kontenery mogą być widoczne w sieci tak jak zwykłe urządzenia.

---

## 1. Wprowadzenie do Macvlan

Sieć Macvlan w Dockerze umożliwia tworzenie wirtualnych interfejsów sieciowych (z osobnymi adresami MAC), które są przypisane do jednego fizycznego interfejsu sieciowego hosta.  
Dzięki temu kontenery mogą być bezpośrednio dostępne z sieci lokalnej bez użycia NAT (Network Address Translation).

### Zastosowania:
- Gdy kontenery muszą być widoczne w tej samej sieci co host,
- Gdy wymagane jest pełne rozdzielenie ruchu sieciowego,
- Dla integracji z VLAN (np. 802.1Q).

---

## 2. Tworzenie interfejsu Macvlan

Utwórz sieć typu **Macvlan** z określonym zakresem adresów i interfejsem fizycznym:

```bash
sudo docker network create -d macvlan   --subnet=172.16.86.0/24   --gateway=172.16.86.1   -o parent=ens2   my-macvlan-net
```

Następnie uruchom kontener, przypisując go do tej sieci:

```bash
sudo docker run --rm -itd --network my-macvlan-net --name my-macvlan-alpine alpine:latest ash
```

Sprawdź szczegóły kontenera i interfejsu sieciowego:

```bash
sudo docker container inspect my-macvlan-alpine
sudo docker exec my-macvlan-alpine ip addr show eth0
sudo docker exec my-macvlan-alpine ip route
```

Na koniec zatrzymaj kontener i usuń sieć:

```bash
sudo docker container stop my-macvlan-alpine
sudo docker network rm my-macvlan-net
```

### Co się dzieje?
- Docker tworzy nowy interfejs Macvlan przypisany do `ens2`,  
- Kontener otrzymuje własny adres IP w podsieci `172.16.86.0/24`,  
- Ruch sieciowy odbywa się **bez translacji adresów**, jak w przypadku tradycyjnych hostów.

---

## 3. Tworzenie interfejsu Macvlan z VLAN (802.1Q)

Można także utworzyć interfejs Macvlan oparty o VLAN — w tym przykładzie o identyfikatorze `10` (interfejs `ens2.10`).

```bash
sudo docker network create -d macvlan   --subnet=172.16.86.0/24   --gateway=172.16.86.1   -o parent=ens2.10   my-8021q-macvlan-net
```

Uruchom kontener korzystający z tej sieci VLAN:

```bash
sudo docker run --rm -itd --network my-8021q-macvlan-net --name my-second-macvlan-alpine alpine:latest ash
```

Sprawdź jego konfigurację sieciową:

```bash
sudo docker container inspect my-second-macvlan-alpine
sudo docker exec my-second-macvlan-alpine ip addr show eth0
sudo docker exec my-second-macvlan-alpine ip route
```

Po zakończeniu ćwiczenia zatrzymaj kontener i usuń sieć:

```bash
sudo docker container stop my-second-macvlan-alpine
sudo docker network rm my-8021q-macvlan-net
```

---

## 4. Wnioski

- Sieć **Macvlan** pozwala na pełną izolację ruchu między kontenerami i hostem,  
- Kontenery mogą komunikować się z urządzeniami fizycznymi w tej samej sieci,  
- Konfiguracja z VLAN (802.1Q) pozwala na logiczne separowanie ruchu w ramach jednej infrastruktury sieciowej,  
- Jest to idealne rozwiązanie dla środowisk produkcyjnych z kontrolą adresacji i segmentacją sieci.

---

## 5. Sprzątanie

Zawsze po testach usuń nieużywane kontenery i sieci, aby uniknąć konfliktów adresów IP i nazw interfejsów.

---

## 6. Podsumowanie

W tym module nauczyłeś się:
- Tworzyć sieć Macvlan w Dockerze,
- Przydzielać kontenery do tej sieci,
- Sprawdzać konfigurację i trasy sieciowe w kontenerach,
- Tworzyć sieć Macvlan z obsługą VLAN (802.1Q).

W kolejnym module poznasz **zaawansowane typy sieci Docker**, takie jak *overlay* i *host*, oraz sposoby ich praktycznego zastosowania w środowiskach wielokontenerowych.
