Sieć za pomocą bridge:
```
sudo docker network ls
sudo docker run -dit --name alpine1 alpine ash
sudo docker run -dit --name alpine2 alpine ash
sudo docker container ls
sudo docker network inspect bridge
sudo docker attach alpine1
  ip addr show
  ping -c 2 google.com
  ping -c 2 [alpine2-IP]
  exit

sudo docker attach alpine2
  ip addr show
  ping -c 2 google.com
  ping -c 2 [alpine1-IP]
  ping -c 2 alpine1
  exit
```
Różne interfejsy bridge i komunikacja pomiędzy nimi:
```
sudo docker container stop alpine1 alpine2 
sudo docker container rm alpine1 alpine2
sudo docker network create --driver bridge alpine-net
sudo docker network ls
sudo docker network inspect alpine-net
sudo docker run -dit --name alpine1 --network alpine-net alpine ash
sudo docker run -dit --name alpine2 --network alpine-net alpine ash
sudo docker run -dit --name alpine3 alpine ash
sudo docker run -dit --name alpine4 --network alpine-net alpine ash
sudo docker network connect bridge alpine4
sudo docker container ls
sudo docker network inspect bridge
sudo docker network inspect alpine-net
sudo docker container attach alpine1
  ping -c 2 alpine2
  ping -c 2 alpine4
  ping -c 2 alpine1
  ping -c 2 alpine3
```
Sprzątanie:
```
sudo docker container stop alpine1 alpine2 alpine3 alpine4
sudo docker container rm alpine1 alpine2 alpine3 alpine4
sudo docker network rm alpine-net
```
