Obrazy Docker:
```
sudo docker images
sudo docker pull ubuntu:16.04
sudo docker images
sudo docker run -t -i --name chochlik_4 ubuntu:16.04 /bin/bash
  cat /var/log/bootstrap.log
  exit
```
Różne wersje:
```
sudo docker pull fedora:21
sudo docker images
sudo docker pull fedora:20
sudo docker images fedora
sudo docker run -t -i --name chochlik_5 fedora:21 /bin/bash
  cat /var/log/anaconda/syslog | head
  exit
```
Wyszukiwanie:
```
sudo docker search apache
```
Zakładamy repozytorium na stronie https://hub.docker.com, następnie:
```
docker login
```
Przygotowanie pierwszego obrazu:
```
sudo docker run -i -t ubuntu /bin/bash
  apt-get -y update
  apt-get -y install apache2
  service apache2 status
  exit
sudo docker ps -a
sudo docker commit -m "Moj pierwszy obraz" -a "Imie Nazwisko" [ID] [user]/apache2:webserver
sudo docker images
sudo docker inspect [user]/apache2:webserver | grep -i author
sudo docker push [user]/apache2:webserver
```
