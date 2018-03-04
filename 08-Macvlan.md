Powołanie interjesu Macvlan:
```
sudo docker network create -d macvlan --subnet=172.16.86.0/24 --gateway=172.16.86.1 -o parent=ens2 my-macvlan-net
sudo docker run --rm -itd --network my-macvlan-net --name my-macvlan-alpine alpine:latest ash
sudo docker container inspect my-macvlan-alpine
sudo docker exec my-macvlan-alpine ip addr show eth0
sudo docker exec my-macvlan-alpine ip route
sudo docker container stop my-macvlan-alpine
sudo docker network rm my-macvlan-net
```
Powołanie interjesu Macvlan 821q:
```
sudo docker network create -d macvlan --subnet=172.16.86.0/24 --gateway=172.16.86.1 -o parent=ens2.10 my-8021q-macvlan-net
sudo docker run --rm -itd --network my-8021q-macvlan-net --name my-second-macvlan-alpine alpine:latest ash
sudo docker container inspect my-second-macvlan-alpine
sudo docker exec my-second-macvlan-alpine ip addr show eth0
sudo docker exec my-second-macvlan-alpine ip route
sudo docker container stop my-second-macvlan-alpine
sudo docker network rm my-8021q-macvlan-net
```
