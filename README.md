# Gate opening control

<img src="https://github.com/teter08/Gates/blob/b727f3e660d7c0b8d49f36ed34c43dde3e6753d6/scheme1.jpg" width="500" />

1. Установка Debian
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
```yaml
wget https://github.com/home-assistant/os-agent/releases/download/1.2.2/os-agent_1.2.2_linux_x86_64.deb` (номер меняем на актуальный)    
sudo dpkg -i os-agent_1.2.2_linux_x86_64.deb
```
5. Установка Home Assisistant Supervised    
```yaml
wget https://github.com/home-assistant/supervised-installer/releases/latest/download/homeassistant-supervised.deb
sudo dpkg -i homeassistant-supervised.deb
```
6. Установка Portainer
```yaml
docker pull portainer/portainer-ce
docker volume create portainer_data
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
Веб интерфейс Portainer - IP adress:9000    
Веб интерфейс Home Assistant - IP adress:8123    

7. Устанавливаем mosquitto брокер
```yaml
sudo apt-get install mosquitto mosquitto-clients
```
8. Установка логина (homeassistant) и пароля для mosquitto
```yaml
sudo mosquitto_passwd -c /etc/mosquitto/passwd homeassistant
```
Вводим два раза пароль по запросу. Эта команда создаст связку логина homeassistant и пароля в файле /etc/mosquitto/passwd     
Указываем в конфиге брокера на этот файл пары логин/пароль и запрещаем анонимные подключения. Открываем
```yaml
sudo nano /etc/mosquitto/conf.d/default.conf
```
И записываем следующие строки
```yaml
allow_anonymous false password_file /etc/mosquitto/passwd
```
9. Открываем 1883 порт
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
10. Проверка. Откроем параллельно два окна терминала подключения по SSH к Debian. В одном: 
```yaml
mosquitto_sub -h localhost -t test -u "homeassistant" -P "ваш_пароль"
```
В другом:
```yaml
mosquitto_pub -h localhost -t "test" -m "Test message" -u "homeassistant" -P "ваш_пароль"
```
после этого в первом терминале мы увидим появившееся сообщение Test message

