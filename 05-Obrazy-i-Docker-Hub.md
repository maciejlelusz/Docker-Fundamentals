# Docker Fundamentals — Moduł: Praca z obrazami Docker (Images)

W tym module nauczysz się pracować z **obrazami Docker** – podstawowym elementem, z którego tworzone są kontenery.  
Dowiesz się, jak pobierać obrazy z repozytoriów, uruchamiać różne wersje systemów oraz tworzyć i publikować własne obrazy.

---

## 1. Przegląd istniejących obrazów

Aby wyświetlić listę wszystkich obrazów dostępnych lokalnie w Twoim systemie:

```bash
sudo docker images
```

To polecenie pokaże m.in. nazwę obrazu, jego wersję (tag), identyfikator (IMAGE ID) oraz rozmiar.

---

## 2. Pobieranie obrazu z repozytorium

Pobierz obraz systemu **Ubuntu 16.04** z oficjalnego repozytorium Docker Hub:

```bash
sudo docker pull ubuntu:16.04
sudo docker images
```

Teraz na Twojej maszynie lokalnej powinien być dostępny nowy obraz.

Uruchom kontener z tego obrazu:

```bash
sudo docker run -t -i --name chochlik_4 ubuntu:16.04 /bin/bash
  cat /var/log/bootstrap.log
  exit
```

Wewnątrz kontenera możesz analizować pliki logów i strukturę systemu.

---

## 3. Różne wersje tego samego obrazu

Docker pozwala pobierać różne **wersje (tagi)** tego samego obrazu.  
Spróbujmy to na przykładzie **Fedory**:

```bash
sudo docker pull fedora:21
sudo docker images
sudo docker pull fedora:20
sudo docker images fedora
```

Zauważ, że obrazy mają różne identyfikatory — oznacza to, że są to niezależne wersje środowiska.

Uruchom kontener z wybraną wersją:

```bash
sudo docker run -t -i --name chochlik_5 fedora:21 /bin/bash
  cat /var/log/anaconda/syslog | head
  exit
```

> 💡 **Wskazówka:** Używaj tagów (`:21`, `:20`, `:latest`) aby dokładnie kontrolować wersję środowiska w swoich projektach.

---

## 4. Wyszukiwanie obrazów w Docker Hub

Docker umożliwia wyszukiwanie dostępnych obrazów bezpośrednio z linii poleceń.  
Przykład wyszukiwania obrazów związanych z Apache:

```bash
sudo docker search apache
```

Polecenie zwraca listę obrazów wraz z krótkim opisem i liczbą gwiazdek (ocen).  
Oficjalne obrazy mają w kolumnie `OFFICIAL` wartość `OK`.

---

## 5. Zakładanie konta i logowanie do Docker Hub

Przejdź do strony [https://hub.docker.com](https://hub.docker.com) i utwórz swoje konto.  
Po założeniu konta zaloguj się do Docker Hub z poziomu terminala:

```bash
sudo docker login
```

> Po zalogowaniu możesz przesyłać (push) własne obrazy do repozytorium.

---

## 6. Tworzenie własnego obrazu

Utworzymy teraz własny obraz Dockera, który będzie zawierał serwer Apache.

Uruchom kontener na bazie obrazu **Ubuntu**:

```bash
sudo docker run -i -t ubuntu /bin/bash
  apt-get -y update
  apt-get -y install apache2
  service apache2 status
  exit
```

Wyjdź z kontenera i sprawdź jego identyfikator (ID):

```bash
sudo docker ps -a
```

Następnie utwórz nowy obraz z tego kontenera:

```bash
sudo docker commit -m "Moj pierwszy obraz" -a "Imie Nazwisko" [ID] [user]/apache2:webserver
```

- `-m` – dodaje komentarz (np. opis zmian).
- `-a` – określa autora obrazu.
- `[ID]` – identyfikator kontenera.
- `[user]/apache2:webserver` – nazwa i tag nowego obrazu.

Sprawdź, czy obraz został utworzony:

```bash
sudo docker images
```

I sprawdź jego autora:

```bash
sudo docker inspect [user]/apache2:webserver | grep -i author
```

---

## 7. Publikacja obrazu w Docker Hub

Aby udostępnić swój obraz w chmurze Docker Hub:

```bash
sudo docker push [user]/apache2:webserver
```

Po przesłaniu obraz będzie dostępny publicznie (lub prywatnie – w zależności od ustawień repozytorium).

> ⚠️ **Uwaga:** Przed publikacją upewnij się, że nie zawiera on żadnych wrażliwych danych.

---

## 8. Podsumowanie

W tym module nauczyłeś się:
- Pobierać obrazy z Docker Hub i uruchamiać kontenery z różnych wersji systemów.  
- Wyszukiwać dostępne obrazy w publicznym repozytorium.  
- Tworzyć własne obrazy z istniejących kontenerów.  
- Publikować obrazy w Docker Hub, aby dzielić się nimi z innymi użytkownikami.

W kolejnym module poznasz proces **automatyzacji budowy obrazów** za pomocą plików `Dockerfile`.
