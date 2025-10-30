# Docker Fundamentals — Moduł: Instalacja i konfiguracja Dockera

W tym module poznasz proces przygotowania systemu operacyjnego Ubuntu do pracy z Dockerem.  
Dowiesz się, jak zaktualizować system, zainstalować niezbędne pakiety, skonfigurować repozytorium Dockera oraz uruchomić jego usługę.

---

## 1. Sprawdzenie wersji systemu

Na początek sprawdź podstawowe informacje o systemie operacyjnym i jądrze Linuxa, aby upewnić się, że środowisko spełnia wymagania Dockera:

```bash
uname -a
```

To polecenie wyświetli informacje o wersji kernela, architekturze procesora oraz dystrybucji systemu.

---

## 2. Aktualizacja systemu i pakietów

Przed instalacją nowych pakietów warto upewnić się, że system jest aktualny. Wykonaj poniższe polecenia:

```bash
sudo apt-get -y update
sudo apt-get -y upgrade
```

- `update` – aktualizuje listę dostępnych pakietów.
- `upgrade` – instaluje najnowsze wersje już zainstalowanych pakietów.

> 💡 **Wskazówka:** Parametr `-y` automatycznie potwierdza wszystkie pytania systemu o zgodę na instalację.

---

## 3. Instalacja wymaganych komponentów

Docker wymaga kilku dodatkowych pakietów systemowych, które odpowiadają m.in. za obsługę połączeń HTTPS i repozytoriów zewnętrznych:

```bash
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
```

- `apt-transport-https` — umożliwia pobieranie pakietów przez HTTPS.  
- `ca-certificates` — zestaw certyfikatów bezpieczeństwa.  
- `curl` — narzędzie do pobierania danych z sieci.  
- `software-properties-common` — zapewnia obsługę polecenia `add-apt-repository`.

---

## 4. Dodanie klucza i repozytorium Dockera

Aby zainstalować najnowszą wersję Dockera z oficjalnego źródła, należy dodać jego klucz GPG oraz repozytorium do systemu:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Po dodaniu repozytorium odśwież ponownie listę pakietów:

```bash
sudo apt-get -y update
```

---

## 5. Instalacja Dockera

Zainstaluj silnik Docker Community Edition (CE):

```bash
sudo apt-get -y install docker-ce
```

Po zakończeniu instalacji możesz sprawdzić, czy Docker działa poprawnie:

```bash
sudo docker info
```

Jeżeli widzisz informacje o wersji i stanie silnika, instalacja przebiegła pomyślnie.

---

## 6. Konfiguracja zapory sieciowej (Firewall)

Jeśli w systemie działa zapora UFW (**Uncomplicated Firewall**), konieczne może być wprowadzenie drobnych zmian w konfiguracji, aby umożliwić Dockerowi poprawną komunikację sieciową.

Otwórz plik konfiguracyjny:

```bash
sudo vi /etc/default/ufw
```

W pliku znajdź linię:

```
DEFAULT_FORWARD_POLICY="DROP"
```

i zmień ją na:

```
DEFAULT_FORWARD_POLICY="ACCEPT"
```

Następnie zapisz zmiany i przeładuj zaporę:

```bash
sudo ufw reload
```

---

## 7. Uruchomienie demona Dockera

Docker działa jako usługa systemowa (demon). Możesz uruchomić go ręcznie poleceniem:

```bash
sudo service docker start
```

Aby upewnić się, że demon działa, możesz wykonać:

```bash
netstat -npea | grep -i docker
ps aux | grep -i docker
```

- `netstat` – pokazuje aktywne połączenia sieciowe.  
- `ps aux` – wyświetla listę aktywnych procesów (w tym proces Dockera).

Jeśli demon jest aktywny, Docker jest gotowy do użycia! 🎉

---

## 8. Podsumowanie

W tym module nauczyłeś się:
- Aktualizować system i pakiety.
- Instalować wymagane komponenty i repozytorium Dockera.
- Uruchamiać i weryfikować usługę Docker Engine.
- Dostosowywać ustawienia firewalla dla poprawnej komunikacji Dockera.

W kolejnym kroku rozpoczniesz pracę z kontenerami – tworzenie, uruchamianie i zarządzanie nimi.
