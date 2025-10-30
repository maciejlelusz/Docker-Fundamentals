# Docker Fundamentals — Moduł: Pierwsze kroki z kontenerami

W tym module rozpoczniesz praktyczną pracę z kontenerami Docker.  
Dowiesz się, jak uruchamiać kontenery, nadawać im nazwy, przeglądać logi, wykonywać operacje administracyjne oraz utrzymywać czystość w środowisku.

---

## 1. Uruchomienie pierwszego kontenera

Sprawdź nazwę swojego hosta (systemu macierzystego):

```bash
hostname
```

Następnie uruchom swój **pierwszy kontener** na bazie obrazu Ubuntu:

```bash
sudo docker run -i -t ubuntu /bin/bash
```

Po wejściu do kontenera możesz wykonać kilka poleceń diagnostycznych:

```bash
hostname
cat /etc/hosts
hostname -I
ps -aux
```

Zaktualizuj system wewnątrz kontenera i zainstaluj edytor `vim`:

```bash
apt-get update && apt-get upgrade && apt-get install vim
vim
exit
```

> 💡 **Wskazówka:** Polecenie `exit` zakończy sesję w kontenerze.

---

## 2. Tworzenie kontenera z nazwą

Sprawdź istniejące kontenery:

```bash
sudo docker ps -a
```

Uruchom nowy kontener z nazwą własną:

```bash
sudo docker run --name chochlik_1 -i -t ubuntu /bin/bash
exit
```

Nazwane kontenery ułatwiają późniejszą identyfikację i zarządzanie.

---

## 3. Operacje na kontenerach

Lista wszystkich kontenerów (działających i zatrzymanych):

```bash
sudo docker ps -a
```

Uruchamianie istniejącego kontenera:

```bash
sudo docker start chochlik_1
sudo docker start [ID]
```

Dołączanie się do uruchomionego kontenera:

```bash
sudo docker attach chochlik_1
sudo docker attach [ID]
```

---

## 4. Kontener z uruchomionym zadaniem w tle

Utwórz kontener działający w tle (daemon), który co sekundę wypisuje komunikat:

```bash
sudo docker run --name chochlik_2 -d ubuntu /bin/sh -c "while true; do echo praca praca; sleep 1; done"
```

Sprawdź działające kontenery:

```bash
sudo docker ps
```

Przeglądaj logi kontenera:

```bash
sudo docker logs chochlik_2
sudo docker logs -f chochlik_2
```

> `-f` pozwala na „śledzenie” logów w czasie rzeczywistym.

---

## 5. Obsługa logów z użyciem systemowego sysloga

Możesz skonfigurować kontener, aby zapisywał logi do **sysloga** systemowego:

```bash
sudo docker run --log-driver="syslog" --name chochlik_3 -d ubuntu /bin/sh -c "while true; do echo praca praca; sleep 1; done"
sudo docker logs -f chochlik_3
tail -f /var/log/syslog
```

Dzięki temu logi z kontenera są dostępne również z poziomu systemu.

---

## 6. Podstawowy monitoring kontenerów

Docker udostępnia narzędzia do monitorowania procesów i wykorzystania zasobów przez kontenery:

```bash
sudo docker top chochlik_3
sudo docker stats chochlik_2 chochlik_3
```

- `top` — pokazuje procesy działające w kontenerze.  
- `stats` — wyświetla zużycie CPU, pamięci i sieci w czasie rzeczywistym.

---

## 7. Tworzenie plików w kontenerze

Za pomocą polecenia `docker exec` możesz wykonać polecenie w już działającym kontenerze:

```bash
sudo docker exec -d chochlik_2 touch /etc/bomba
sudo docker exec -t -i chochlik_2 /bin/bash
```

Wewnątrz kontenera sprawdź, czy plik został utworzony:

```bash
ls -alh /etc/bomba
exit
```

---

## 8. Zatrzymywanie kontenerów

Aby zatrzymać kontener, użyj polecenia:

```bash
sudo docker ps -a
sudo docker stop chochlik_2
sudo docker stop [ID]
```

---

## 9. Inspekcja środowiska („piaskownicy”)

Polecenie `inspect` umożliwia podgląd szczegółowych informacji o kontenerach:

```bash
sudo docker inspect chochlik_3
sudo docker inspect --format='{{ .State.Running }}' chochlik_3
sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' chochlik_3
sudo docker inspect --format '{{.Name}} {{.State.Running}}' chochlik_2 chochlik_3
sudo ls -alh /var/lib/docker/containers
```

> 💡 Dzięki `--format` możesz precyzyjnie wyciągać konkretne informacje z wyników inspekcji.

---

## 10. Ubijanie kontenerów

Jeśli kontener nie reaguje, możesz go „zabić” (natychmiast zatrzymać):

```bash
sudo docker ps -a
sudo docker kill chochlik_3
sudo docker ps -a
sudo docker start chochlik_3
sudo ps aux | grep docker
sudo kill -9 [PID]
sudo docker ps -a
```

---

## 11. Usuwanie kontenerów

Aby usunąć pojedynczy kontener:

```bash
sudo docker rm [ID]
```

Lub usunąć **wszystkie** zatrzymane kontenery:

```bash
sudo docker rm -f `sudo docker ps -a -q`
```

---

## 12. Utrzymanie czystości w środowisku Docker

Docker może z czasem gromadzić nieużywane zasoby. Wyczyść środowisko:

```bash
sudo docker container prune
sudo docker system prune --volumes
```

> ⚠️ Te polecenia usuwają **nieużywane kontenery, obrazy i wolumeny**. Używaj ich ostrożnie!

---

## 13. Podsumowanie

W tym module nauczyłeś się:
- Uruchamiać i zarządzać kontenerami.
- Nadawać im nazwy oraz pracować z logami i procesami.
- Inspekcjonować, zatrzymywać i usuwać kontenery.
- Czyścić środowisko Docker z nieużywanych zasobów.

Jesteś teraz gotów, by przejść do kolejnych etapów — pracy z obrazami Docker i tworzenia własnych aplikacji kontenerowych.
