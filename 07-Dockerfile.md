# Docker Fundamentals — Moduł: Tworzenie i publikacja własnego obrazu Docker

W tym module nauczysz się **tworzyć własny obraz Dockera** przy użyciu pliku `Dockerfile`, budować go lokalnie oraz przesyłać do rejestru.  
Poznasz też podstawowe mechanizmy mapowania portów i uruchamiania kontenerów z własnych obrazów.

---

## 1. Przygotowanie środowiska pracy

Zacznij od utworzenia katalogu roboczego:

```bash
mkdir static
cd static
sudo vi Dockerfile
```

---

## 2. Tworzenie pliku `Dockerfile`

Plik `Dockerfile` zawiera instrukcje definiujące nowy obraz Dockera.  
Otwórz go w edytorze i wklej poniższą zawartość:

```dockerfile
# Version: 0.1
FROM ubuntu:16.04
MAINTAINER Imie Nazwisko "imie.nazwisko@cementarnapolka.com"
RUN apt-get update && apt-get install -y nginx
RUN echo 'Wujek Vernon, wujek Vernon.' > /var/www/html/index.html
EXPOSE 80
```

### Wyjaśnienie poleceń:
- `FROM ubuntu:16.04` – bazuje na obrazie systemu Ubuntu 16.04,  
- `MAINTAINER` – dodaje informację o autorze obrazu,  
- `RUN apt-get update && apt-get install -y nginx` – instaluje serwer WWW **nginx**,  
- `RUN echo ... > /var/www/html/index.html` – dodaje prostą stronę testową,  
- `EXPOSE 80` – wskazuje port, który kontener będzie nasłuchiwał (HTTP).

---

## 3. Budowanie obrazu

Zbuduj obraz lokalnie, korzystając z utworzonego pliku `Dockerfile`:

```bash
sudo docker build -t "[user]/static" .
```

Po zakończeniu procesu Docker utworzy nowy obraz widoczny na liście po wykonaniu:

```bash
sudo docker images
```

---

## 4. Budowa obrazu z repozytorium GitHub

Utwórz konto na [GitHub](https://github.com) i załóż nowe repozytorium.  
Umieść w nim plik `Dockerfile`, a następnie zbuduj obraz bezpośrednio z repozytorium:

```bash
sudo docker build -t "[user]/static:v1" github.com/[user]/[repo]
```

> 💡 **Wskazówka:** Docker automatycznie pobierze plik `Dockerfile` z repozytorium i zbuduje obraz zgodnie z jego zawartością.

---

## 5. Analiza historii obrazu

Aby prześledzić, jak został zbudowany obraz i jakie ma warstwy, użyj:

```bash
sudo docker images [user]/static
sudo docker history [ID]
```

Polecenie `docker history` pokazuje listę poleceń `RUN`, `COPY`, `ADD` i innych, które zostały wykonane w trakcie budowy obrazu.

---

## 6. Uruchamianie kontenera z własnego obrazu

Uruchom kontener z zbudowanego przez Ciebie obrazu:

```bash
sudo docker run -d -p 80 --name chochlik_6 [user]/static nginx -g "daemon off;"
sudo docker ps -l
sudo docker port chochlik_6 80
curl localhost:[Port]
```

Wyjaśnienie:
- `-d` – uruchamia kontener w tle,  
- `-p 80` – eksponuje port 80 kontenera,  
- `--name chochlik_6` – nadaje nazwę kontenerowi,  
- `nginx -g "daemon off;"` – uruchamia Nginx w trybie pierwszoplanowym (foreground).

Sprawdź działanie strony lokalnie za pomocą `curl` lub przeglądarki.

---

## 7. Wysyłanie obrazu do rejestru

Aby udostępnić swój obraz innym, prześlij go do repozytorium (np. Docker Hub):

```bash
sudo docker push [user]/static:v1
```

Upewnij się, że jesteś zalogowany (`sudo docker login`), zanim wykonasz to polecenie.

---

## 8. Mapowanie portów

Jeśli chcesz, aby kontener był dostępny z zewnątrz na innym porcie hosta, użyj opcji `-p host_port:container_port`:

```bash
sudo docker run -d -p 8080:80 --name chochlik_7 [user]/static nginx -g "daemon off;"
```

Oznacza to, że port **8080** na hoście zostanie zmapowany na port **80** w kontenerze.

Teraz otwórz przeglądarkę i przejdź pod adres:

```
http://docker01:8080
```

Powinieneś zobaczyć stronę z napisem *„Wujek Vernon, wujek Vernon.”*.

---

## 9. Podsumowanie

W tym module nauczyłeś się:
- Tworzyć plik `Dockerfile` i budować z niego obraz,  
- Budować obraz zdalnie z repozytorium GitHub,  
- Analizować historię budowy obrazu,  
- Uruchamiać kontenery i mapować porty,  
- Wysyłać własne obrazy do rejestru Docker.

W kolejnym module zajmiemy się **automatyzacją procesu budowania** przy użyciu `Dockerfile` i `docker-compose`.
