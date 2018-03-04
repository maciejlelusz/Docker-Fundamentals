Uruchamianie własnego prywatnego rejestru:
```
sudo docker run -d -p 5000:5000 --name registry registry
sudo docker images
sudo docker tag [user]/apache2:webserver localhost:5000/[user]/apache2:webserver
sudo docker push localhost:5000/[user]/apache2:webserver
sudo docker run -t -i localhost:5000/[user]/apache2:webserver /bin/bash
```
