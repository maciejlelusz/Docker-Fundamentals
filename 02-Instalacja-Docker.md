Aktualizacja systemu i instalacja:
```
uname -a
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce
sudo docker info
```
Zmiany w firewall (jeżeli jest):
```
sudo vi /etc/default/ufw
  DEFAULT_FORWARD_POLICY="DROP" -> DEFAULT_FORWARD_POLICY="ACCEPT"
sudo ufw reload
```
Startowanie demona docker:
```
sudo service docker start
netstat -npea | grep -i docker
ps aux | grep -i docker
```
