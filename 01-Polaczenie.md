# Docker Fundamentals — Moduł: Połączenie ze środowiskiem szkoleniowym

W tym module nauczysz się, jak połączyć się ze zdalnym środowiskiem szkoleniowym, w którym będziesz realizować dalsze ćwiczenia praktyczne z Dockera.  
Połączenie odbywa się za pomocą protokołu **SSH (Secure Shell)**, który umożliwia bezpieczną komunikację z serwerem w systemie Linux.

---

## 1. Pobranie klucza dostępowego

Aby uzyskać dostęp do zdalnej maszyny, potrzebujesz klucza prywatnego w formacie PEM. Klucz ten działa jak Twoja „cyfrowa przepustka” — pozwala Ci uwierzytelnić się bez użycia hasła.

Wykonaj poniższe polecenie, aby pobrać klucz:

```bash
wget https://raw.githubusercontent.com/inleo-pl/Warsztaty-Docker-Fundamentals/master/Docker_Fundamentals.pem
```

Po pobraniu plik **Docker_Fundamentals.pem** znajdzie się w bieżącym katalogu.

> 💡 **Wskazówka:**  
> Możesz sprawdzić zawartość katalogu poleceniem:
> ```bash
> ls -l
> ```

---

## 2. Nadanie uprawnień do pliku klucza

Ze względów bezpieczeństwa system SSH wymaga, aby klucz prywatny miał **ograniczone uprawnienia** – tylko właściciel pliku może go odczytać.

Nadaj odpowiednie uprawnienia, wykonując:

```bash
chmod 600 /ścieżka/do/pliku/Docker_Fundamentals.pem
```

Gdzie:
- `/ścieżka/do/pliku/` to folder, w którym zapisałeś klucz PEM (np. `/home/ubuntu/` lub `~/Pobrane/`).

> ⚠️ **Uwaga:**  
> Jeśli klucz ma zbyt szerokie uprawnienia (np. może być czytany przez innych użytkowników), SSH odmówi użycia go do logowania.

---

## 3. Połączenie z maszyną zdalną

Po przygotowaniu klucza możesz nawiązać połączenie SSH z maszyną szkoleniową.  
Użyj poniższego polecenia, podstawiając właściwą ścieżkę i adres hosta:

```bash
ssh -i /ścieżka/do/pliku/Docker_Fundamentals.pem ubuntu@adreshosta.com
```

Gdzie:
- `-i` — wskazuje plik klucza prywatnego,
- `ubuntu` — to użytkownik, którym logujesz się do systemu (typowy w instancjach Ubuntu),
- `adreshosta.com` — to adres lub IP maszyny w chmurze (otrzymasz go od prowadzącego szkolenie).

Po poprawnym połączeniu zobaczysz komunikat w stylu:

```
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-123-generic x86_64)
ubuntu@docker-lab:~$
```

Oznacza to, że jesteś zalogowany i możesz rozpocząć pracę w środowisku Dockera.

---

## 4. Weryfikacja połączenia

Aby upewnić się, że połączenie działa poprawnie i masz dostęp do Dockera, wpisz:

```bash
docker --version
```

Powinieneś zobaczyć wynik podobny do:

```
Docker version 26.1.0, build 0123456
```

Jeśli tak — gratulacje! 🎉  
Jesteś gotów do dalszych ćwiczeń praktycznych w ramach kursu **Docker Fundamentals**.

---

## 5. Podsumowanie

W tym module nauczyłeś się:
- Pobierać i konfigurować klucz PEM.
- Nadawać właściwe uprawnienia plikowi.
- Łączyć się z maszyną zdalną za pomocą SSH.
- Weryfikować poprawność połączenia i dostępność Dockera.

W kolejnym kroku przejdziesz do pracy z kontenerami — uruchamiania, zatrzymywania oraz eksplorowania ich działania w praktyce.
