# Gates
Gate opening control
Schema

<img src="https://github.com/teter08/Gates/blob/b727f3e660d7c0b8d49f36ed34c43dde3e6753d6/scheme1.jpg" width="500" />


##1. Установка Debian
2. Установка docker 
```yaml
sudo curl -fsSL get.docker.com | sh
```
3. Добавляем в группу docker пользователя
```yaml
sudo gpasswd -a $USER docker
newgrp docker
```
4. Установка OS-Agent. [Последний релиз](https://github.com/home-assistant/os-agent/releases/latest)    
Загружаем - `wget https://github.com/home-assistant/os-agent/releases/download/1.2.2/os-agent_1.2.2_linux_x86_64.deb` (номер меняем на актуальный)    
Установка - `sudo dpkg -i os-agent_1.2.2_linux_x86_64.deb`    
5. Установка Home Assisistant Supervised    
Загружаем - `wget https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb`    
Установка - `sudo dpkg -i homeassistant-supervised.deb`    
6. Установка Portainer - 
```yaml
docker pull portainer/portainer-ce
docker volume create portainer_data
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
Веб интерфейс Portainer - IP adress:9000    
Веб интерфейс Home Assistant - IP adress:8123    
7. Установка альтернативного брокера
```yaml
sudo apt-get install mosquitto mosquitto-clients
```
После установки брокера, необходимо защитить его от подписки кого бы то ни было при помощи связки логина пароля, для этого воспользуемся следующей командой:
```yaml
sudo mosquitto_passwd -c /etc/mosquitto/passwd homeassistant
```
Далее надо будет ввести два раза пароль по запросу. Эта команда создаст связку логина homeassistant и пароля который вы задали в файле /etc/mosquitto/passwd и теперь нам надо натравить брокер на этот файл и запретить анонимные подключения к нему. Сделаем это так, откроем файл конфига брокера:
```yaml
sudo nano /etc/mosquitto/conf.d/default.conf
```
И запишем туда следующие строки:
```yaml
allow_anonymous false password_file /etc/mosquitto/passwd
```
сохраняем файл, открываем 1883 порт
```yaml
sudo nano /etc/mosquitto/mosquitto.conf
```
записываем в него:
```yaml
pid_file /run/mosquitto/mosquitto.pid
persistence true
persistence_location /var/lib/mosquitto/
log_dest topic
log_type error
log_type warning
log_type notice
log_type information
connection_messages true
log_timestamp true
include_dir /etc/mosquitto/conf.d
listener 1883
```
и перезагружаем брокер командой:
```yaml
sudo systemctl restart mosquitto
```
Далее можем проверить, что все настроено правильно. Откроем параллельно два окна терминала и подключимся в обоих к нашей малине по ssh. Далее в одном из них напишем: 
```yaml
mosquitto_sub -h localhost -t test -u "homeassistant" -P "ваш_пароль"
```
а в другом:
```yaml
mosquitto_pub -h localhost -t "test" -m "Test message" -u "homeassistant" -P "ваш_пароль"
```
после этого в первом терминале мы увидим появившееся сообщение Test message. Если все так - вы все настроили верно! Можно приступать к настройке HA    
8. 
