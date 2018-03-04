Pierwszy kontener:
```
hostname
sudo docker run -i -t ubuntu /bin/bash
  hostname
  cat /etc/hosts
  hostname -I
  ps -aux
  apt-get update && apt-get upgrade && apt-get install vim
  vim
  exit
```
Kontener z nazwą:
```
sudo docker ps -a
sudo docker run --name chochlik_1 -i -t ubuntu /bin/bash
  exit
```
Operacje na kontenerze:
```
sudo docker ps -a
sudo docker start chochlik_1
sudo docker start [ID]
sudo docker attach chochlik_1
sudo docker attach [ID]
```
Kontener z uruchomionym zadaniem:
```
sudo docker run --name chochlik_2 -d ubuntu /bin/sh -c "while true; do echo praca praca; sleep 1; done"
sudo docker ps
sudo docker logs chochlik_2
sudo docker logs -f chochlik_2
```
Obsługa logów:
```
sudo docker run --log-driver="syslog" --name chochlik_3 -d ubuntu /bin/sh -c "while true; do echo praca praca; sleep 1; done"
sudo docker logs -f chochlik_3
tail -f /var/log/syslog 
```
Podstawowy monitoring i testy:
```
sudo docker top chochlik_3
sudo docker stats chochlik_2 chochlik_3
```
Tworzenie plików w kontenerze:
```
sudo docker exec -d chochlik_2 touch /etc/bomba
sudo docker exec -t -i chochlik_2 /bin/bash
  ls -alh /etc/bomba
  exit
```
Zatrzymywanie kontenera:
```
sudo docker ps -a
sudo docker stop chochlik_2
sudo docker stop [ID]
```
Inspekcja naszej piaskownicy:
```
sudo docker inspect chochlik_3
sudo docker inspect --format='{{ .State.Running }}' chochlik_3
sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}' chochlik_3
sudo docker inspect --format '{{.Name}} {{.State.Running}}' chochlik_2 chochlik_3
sudo ls -alh /var/lib/docker/containers
```
Ubijanie kontenerów:
```
sudo docker ps -a
sudo docker kill chochlik_3
sudo docker ps -a
sudo docker start chochlik_3
sudo ps aux | grep docker
sudo kill -9 [PID]
sudo docker ps -a
```
Usuwanie kontnerów:
```
sudo docker rm [ID]
sudo docker rm -f `sudo docker ps -a -q`
```
