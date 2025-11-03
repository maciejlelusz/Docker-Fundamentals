# Docker Fundamentals — Moduł: Prywatny rejestr obrazów Docker (Private Registry)

W tym module nauczysz się, jak uruchomić **własny prywatny rejestr obrazów Docker**, który pozwala przechowywać i udostępniać obrazy lokalnie — bez korzystania z publicznego Docker Hub.  
To rozwiązanie często stosuje się w środowiskach firmowych i testowych.

---

## 1. Uruchomienie prywatnego rejestru

Docker udostępnia oficjalny obraz o nazwie `registry`, który pozwala błyskawicznie uruchomić lokalny rejestr obrazów.  
Aby go uruchomić, wykonaj:

```bash
sudo docker run -d -p 5000:5000 --name registry registry
```

Parametry:
- `-d` – uruchamia kontener w tle (detached mode),
- `-p 5000:5000` – mapuje port 5000 kontenera na port 5000 hosta,
- `--name registry` – nadaje kontenerowi nazwę `registry`,
- `registry` – to oficjalny obraz serwera rejestru.

Po uruchomieniu rejestru możesz sprawdzić jego działanie:

```bash
sudo docker ps
```

Powinieneś zobaczyć kontener `registry` działający na porcie 5000.

---

## 2. Przygotowanie obrazu do wysłania

Załóżmy, że masz już własny obraz o nazwie `[user]/apache2:webserver`.  
Aby umieścić go w swoim prywatnym rejestrze, należy go **otagować** w formacie `localhost:5000/nazwa`:

```bash
sudo docker images
sudo docker tag [user]/apache2:webserver localhost:5000/[user]/apache2:webserver
```

To polecenie tworzy nowy alias dla obrazu, który wskazuje na Twój lokalny rejestr.

---

## 3. Wysyłanie obrazu do prywatnego rejestru

Po otagowaniu obrazu możesz go przesłać (push) do rejestru działającego na Twojej maszynie:

```bash
sudo docker push localhost:5000/[user]/apache2:webserver
```

Po zakończeniu transferu obraz będzie dostępny w lokalnym repozytorium.

> 💡 **Wskazówka:** Domyślnie lokalny rejestr działa przez HTTP, bez szyfrowania.  
> W środowisku produkcyjnym zaleca się skonfigurowanie HTTPS i uwierzytelniania.

---

## 4. Uruchamianie kontenera z prywatnego rejestru

Aby upewnić się, że obraz został poprawnie przesłany, możesz spróbować uruchomić nowy kontener bezpośrednio z prywatnego rejestru:

```bash
sudo docker run -t -i localhost:5000/[user]/apache2:webserver /bin/bash
```

Docker pobierze obraz z Twojego lokalnego rejestru i uruchomi interaktywną sesję bash.

---

## 5. Sprawdzenie zawartości rejestru

Aby sprawdzić, jakie obrazy znajdują się w Twoim prywatnym rejestrze, otwórz w przeglądarce adres:

```bash
http://docker01:5000/v2/_catalog
```

Zwrócony zostanie wynik w formacie JSON, np.:

```json
{
  "repositories": [
    "user/apache2/webserver"
  ]
}
```

> 🔍 Możesz także użyć `curl` zamiast przeglądarki:
> ```bash
> curl http://localhost:5000/v2/_catalog
> ```

---

## 6. Podsumowanie

W tym module nauczyłeś się:
- Uruchamiać lokalny serwer rejestru Docker (`registry`),  
- Oznaczać obrazy i przesyłać je do prywatnego repozytorium,  
- Uruchamiać kontenery z obrazów przechowywanych w rejestrze,  
- Sprawdzać zawartość rejestru za pomocą interfejsu API.

W kolejnym module omówimy **konfigurację bezpieczeństwa i HTTPS** dla prywatnego rejestru.
