Tworzymy wolumen:
```
sudo docker volume ls
sudo docker volume create my-vol
sudo docker volume inspect my-vol
```
Startowanie kontenera z wolumenem przy pomocy -mount:
```
sudo docker run -d --name devtest --mount source=my-vol,target=/app nginx:latest
docker container inspect devtest
docker exec -t -i devtest /bin/sh
  touch /app/test2
  exit
sudo ls /var/lib/docker/volumes/my-vol/_data
```
Startowanie kontenera z wolumenem przy pomocy -v:
```
docker run -d --name devtest2 -v my-vol:/app nginx:latest
docker container inspect devtest2
docker exec -t -i devtest2 /bin/sh
  touch /app/test2
  exit
sudo ls /var/lib/docker/volumes/my-vol/_data
```
Sprzątanko:
```
docker container stop devtest devtest2
docker container rm devtest devtest2
docker volume rm my-vol
```
Dostęp do plików Docker Engine za pomocą mount typu bind:
```
docker run -d -it --name devtest --mount type=bind,source=/tmp,target=/app nginx:latest
docker exec -t -i devtest /bin/sh
  ls -alh /app
  exit
```
