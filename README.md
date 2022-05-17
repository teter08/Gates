# Gates

Gate opening control

Schema

![Схема организации связи](https://github.com/teter08/Gates/blob/b727f3e660d7c0b8d49f36ed34c43dde3e6753d6/scheme1.jpg)

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
:arrow_right: Веб интерфейс Portainer - IP adress:9000    
:arrow_right: Веб интерфейс Home Assistant - IP adress:8123    
7. 
8. 
9. **sudo nano /etc/mosquitto/mosquitto.conf**
