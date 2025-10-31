# Docker Fundamentals — Moduł: Praca z wolumenami (Volumes)

W tym module nauczysz się, jak zarządzać wolumenami w Dockerze — specjalnymi przestrzeniami, które pozwalają na trwałe przechowywanie danych kontenerów.  
Poznasz różne sposoby montowania wolumenów oraz zobaczysz, jak uzyskać dostęp do plików przechowywanych przez Docker Engine.

---

## 1. Tworzenie wolumenu

Wolumeny są używane do przechowywania danych poza cyklem życia kontenera. Możesz je tworzyć i przeglądać przy użyciu poniższych poleceń:

```bash
sudo docker volume ls
sudo docker volume create my-vol
sudo docker volume inspect my-vol
```

- `docker volume ls` — wyświetla listę istniejących wolumenów.  
- `docker volume create` — tworzy nowy wolumen o podanej nazwie.  
- `docker volume inspect` — pokazuje szczegóły dotyczące ścieżki i konfiguracji wolumenu.

---

## 2. Uruchomienie kontenera z wolumenem przy użyciu `--mount`

Metoda `--mount` jest zalecanym sposobem montowania wolumenów, ponieważ jest bardziej czytelna i elastyczna.

```bash
sudo docker run -d --name devtest --mount source=my-vol,target=/app nginx:latest
docker container inspect devtest
docker exec -t -i devtest /bin/sh
  touch /app/test2
  exit
sudo ls /var/lib/docker/volumes/my-vol/_data
```

Powyższy zestaw poleceń:
1. Uruchamia kontener `devtest` z wolumenem `my-vol` zamontowanym do katalogu `/app` wewnątrz kontenera.  
2. Tworzy plik `test2` w katalogu `/app`.  
3. Pokazuje, że plik ten jest przechowywany fizycznie w lokalizacji:
   `/var/lib/docker/volumes/my-vol/_data`.

> 💡 **Wskazówka:** Dane w wolumenie nie są kasowane po usunięciu kontenera — dzięki temu możesz zachować dane między uruchomieniami.

---

## 3. Uruchomienie kontenera z wolumenem przy użyciu `-v`

Alternatywny, krótszy sposób montowania wolumenów wykorzystuje opcję `-v`.  
Działa podobnie jak `--mount`, ale ma mniej czytelną składnię.

```bash
docker run -d --name devtest2 -v my-vol:/app nginx:latest
docker container inspect devtest2
docker exec -t -i devtest2 /bin/sh
  touch /app/test2
  exit
sudo ls /var/lib/docker/volumes/my-vol/_data
```

Efekt działania jest identyczny – plik zapisany w `/app` w kontenerze znajdziesz również w lokalnym katalogu danych Dockera.

> ⚠️ **Uwaga:** W nowszych wersjach Dockera zaleca się używanie `--mount`, ponieważ zapewnia większą kontrolę i czytelność konfiguracji.

---

## 4. Dostęp do plików Dockera za pomocą mount typu **bind**

Oprócz wolumenów można również montować lokalne katalogi systemu hosta bezpośrednio do kontenera.  
To tzw. **bind mount**, często używany w środowiskach deweloperskich.

```bash
docker run -d -it --name devtest3 --mount type=bind,source=/tmp,target=/app nginx:latest
docker exec -t -i devtest3 /bin/sh
  ls -alh /app
  exit
```

Tutaj lokalny katalog `/tmp` został zamontowany do `/app` wewnątrz kontenera.  
Wszystkie pliki tworzone w `/app` pojawią się również w `/tmp` na maszynie hosta.

> 💡 **Przykład zastosowania:** Używanie `bind` pozwala deweloperom edytować kod na hoście i od razu widzieć zmiany w kontenerze bez konieczności ponownego budowania obrazu.

---

## 5. Sprzątanie po ćwiczeniach

Po zakończeniu pracy z wolumenami warto usunąć testowe kontenery i dane:

```bash
docker container stop devtest devtest2 devtest3
docker container rm devtest devtest2 devtest3
docker volume rm my-vol
```

> ⚠️ **Uwaga:** Usunięcie wolumenu (`docker volume rm`) spowoduje trwałe usunięcie wszystkich danych zapisanych w nim.

---

## 6. Podsumowanie

W tym module nauczyłeś się:
- Tworzyć i przeglądać wolumeny w Dockerze.  
- Montować je do kontenerów za pomocą `--mount` i `-v`.  
- Używać bind mountów do pracy z lokalnymi katalogami.  
- Zarządzać i czyścić środowisko po zakończeniu pracy.

W kolejnym module poznasz **obrazy Dockera (Docker images)** i dowiesz się, jak budować własne środowiska aplikacyjne.
