# Gates
Gate opening control

sudo nano /etc/mosquitto/mosquitto.conf

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
