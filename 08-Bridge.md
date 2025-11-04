# Docker Fundamentals — Moduł: Sieci i komunikacja między kontenerami

W tym module poznasz podstawy **sieci Docker** opartej na typie *bridge*, nauczysz się tworzyć własne mosty sieciowe oraz sprawdzisz, jak kontenery mogą się ze sobą komunikować w ramach tej samej lub różnych sieci.

---

## 1. Sieć domyślna typu *bridge*

Docker tworzy automatycznie sieć `bridge` dla kontenerów uruchamianych bez jawnego przypisania do innej sieci.  
Zacznijmy od sprawdzenia, jakie sieci są dostępne:

```bash
sudo docker network ls
```

---

## 2. Uruchamianie kontenerów w sieci bridge

Uruchom dwa lekkie kontenery bazujące na obrazie **Alpine Linux**:

```bash
sudo docker run -dit --name alpine1 alpine ash
sudo docker run -dit --name alpine2 alpine ash
```

Sprawdź aktywne kontenery:

```bash
sudo docker container ls
```

A następnie przeanalizuj szczegóły sieci `bridge`:

```bash
sudo docker network inspect bridge
```

Wejdź do kontenera `alpine1` i wykonaj testy sieciowe:

```bash
sudo docker attach alpine1
  ip addr show
  ping -c 2 google.com
  ping -c 2 [alpine2-IP]
  exit
```

Następnie sprawdź drugi kontener `alpine2`:

```bash
sudo docker attach alpine2
  ip addr show
  ping -c 2 google.com
  ping -c 2 [alpine1-IP]
  ping -c 2 alpine1
  exit
```

### Co się dzieje?
- Kontenery w sieci `bridge` otrzymują adresy IP z podsieci zarządzanej przez Dockera.  
- Mogą komunikować się ze sobą za pomocą IP lub nazwy kontenera, jeśli są w tej samej sieci.

---

## 3. Tworzenie własnej sieci typu *bridge*

Zatrzymaj i usuń istniejące kontenery:

```bash
sudo docker container stop alpine1 alpine2 
sudo docker container rm alpine1 alpine2
```

Następnie utwórz nową sieć o nazwie `alpine-net`:

```bash
sudo docker network create --driver bridge alpine-net
```

Sprawdź dostępne sieci i szczegóły nowej:

```bash
sudo docker network ls
sudo docker network inspect alpine-net
```

---

## 4. Uruchamianie kontenerów w różnych sieciach

Uruchom kilka kontenerów, przypisując je do różnych sieci:

```bash
sudo docker run -dit --name alpine1 --network alpine-net alpine ash
sudo docker run -dit --name alpine2 --network alpine-net alpine ash
sudo docker run -dit --name alpine3 alpine ash
sudo docker run -dit --name alpine4 --network alpine-net alpine ash
```

Teraz połącz kontener `alpine4` z drugą siecią `bridge`, aby miał dostęp do obu:

```bash
sudo docker network connect bridge alpine4
```

Wyświetl listę kontenerów i szczegóły obu sieci:

```bash
sudo docker container ls
sudo docker network inspect bridge
sudo docker network inspect alpine-net
```

Wejdź do `alpine1` i przetestuj komunikację:

```bash
sudo docker container attach alpine1
  ping -c 2 alpine2
  ping -c 2 alpine4
  ping -c 2 alpine1
  ping -c 2 alpine3
```

### Wnioski:
- Kontenery w tej samej sieci `alpine-net` mogą się komunikować po nazwie.  
- Kontenery z różnych sieci (np. `bridge` i `alpine-net`) **nie komunikują się ze sobą**, chyba że zostaną połączone wspólną siecią.  
- `alpine4` ma dostęp do obu sieci i może działać jako „most” komunikacyjny.

---

## 5. Sprzątanie po ćwiczeniu

Na koniec zatrzymaj i usuń wszystkie kontenery oraz dodatkową sieć:

```bash
sudo docker container stop alpine1 alpine2 alpine3 alpine4
sudo docker container rm alpine1 alpine2 alpine3 alpine4
sudo docker network rm alpine-net
```

---

## 6. Podsumowanie

W tym module nauczyłeś się:
- Jak działa domyślna sieć *bridge* w Dockerze,  
- Jak tworzyć własne sieci mostowe i przypisywać do nich kontenery,  
- Jak analizować sieci i komunikację między kontenerami,  
- Jak łączyć kontenery z wieloma sieciami.

W kolejnym module zajmiemy się **zaawansowanym zarządzaniem sieciami i izolacją usług** w Dockerze.
