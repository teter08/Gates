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
4. Установка OS-Agent    
:white_check_mark: [Последний релиз](https://github.com/home-assistant/os-agent/releases/latest)    
Загружаем - `wget https://github.com/home-assistant/os-agent/releases/download/1.2.2/os-agent_1.2.2_linux_aarch64.deb` (номер меняем на актуальный)    
Установка - `sudo dpkg -i os-agent_1.2.2_linux_aarch64.deb`    
5. 
6. 
7. 
8. 
9. **sudo nano /etc/mosquitto/mosquitto.conf**
