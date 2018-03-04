Stworzenie środowiska pracy:
```
mkdir static
cd static
vi Dockerfile
```
W pliku umieszczamy:
```
# Version: 0.1
FROM ubuntu:16.04
MAINTAINER Imie Nazwisko "imie.nazwisko@cemntarnapolka.com"
RUN apt-get update && apt-get install -y nginx
RUN echo 'Wujek Vernon, wujek Vernon.' > /var/www/html/index.html
EXPOSE 80
```
Budujemy:
```
sudo docker build -t "[user]/static" .
sudo docker build -t "[user]/static:v1" .
```
Zakładamy GitHub (https://github.com) i tworzymy repo z plikiem Dockerfile, a następnie budujemy przy jego użyciu:
```
sudo docker build -t "[user]/static:v1" github.com/[user]/[repo]
```
Podgląd historii obrazu:
```
sudo docker images [user]/static
sudo docker history [ID]
```
Uruchomienie z zbudowanego przez nas obrazu kontenera:
```
sudo docker run -d -p 80 --name chochlik_6 [user]/static nginx -g "daemon off;"
sudo docker ps -l
sudo docker port chochlik_6 80
curl localhost:[Port]
```
Mapowanie portów:
```
sudo docker run -d -p 8080:80 --name chochlik_7 [user]/static nginx -g "daemon off;"
```
Teraz wejdz na adres serwera docker01 z lab na port 8080 poprzez przegladarkę internetową. Wyślij obraz do rejestru:
```
sudo docker push [user]/static:v1
```
