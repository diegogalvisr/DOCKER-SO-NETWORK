

# INSTALACION DE SERVICE DNS

Actualizamos paquetes e instalamos el de BIND9

```shell
sudo apt update
sudo apt install bind9 bind9utils bind9-doc
```

Configurar la zona del dominio

```shell
sudo nano /etc/bind/named.conf.local
```

Agregar esto:

```shell
zone "gestsoft.com" {
    type master;
    file "/etc/bind/zones/db.gestsoft.com";
};
```

Crear el archivo de zona:

```shell
sudo mkdir -p /etc/bind/zones
sudo nano /etc/bind/zones/db.gestsoft.com
```

Agregar el siguiente contenido al archivo de zona:

```shell
$TTL    604800
@       IN      SOA     ns1.gestsoft.com. admin.gestsoft.com. (
                      2024061001         ; Serial
                      604800             ; Refresh
                      86400              ; Retry
                      2419200            ; Expire
                      604800 )           ; Negative Cache TTL
;
@       IN      NS      ns1.gestsoft.com.
@       IN      A       192.168.1.20
www     IN      A       192.168.1.20
ns1     IN      A       192.168.1.20

```

Reiniciamos el servicio

```shell
sudo systemctl restart bind9
```

Verificamos que este respondiendo correctamente:

```shell
dig @localhost www.gestsoft.com
```

# INSTALACION DE SERVICE WEB - NGINX

Actualizamos paquetes y instalamos el de NGINX

```shell
sudo apt update
sudo apt install nginx
```

Creamos un archivo de configuracion para el dominio gestsoft.com

```shell
sudo nano /etc/nginx/sites-available/gestsoft.com
```

Añadimos la siguiente configuracion y guardamos cambios:

```shell
server {
    listen 80;
    server_name gestsoft.com www.gestsoft.com;

    root /var/www/gestsoft.com;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Creamos un directorio raiz y un index de ejemplo:

```shell
sudo mkdir -p /var/www/gestsoft.com
sudo nano /var/www/gestsoft.com/index.html
```

Adimos un html basico:

<!DOCTYPE html>

```shell
<!DOCTYPE html>
<html>
<head>
    <title>Bienvenido a gestsoft.com</title>
</head>
<body>
    <h1>Hola, mundo!</h1>
    <p>Este es un ejemplo de HTML para gestsoft.com.</p>
</body>
</html>

```

Habilitamos la configuracion y reiniciamos:

```shell
sudo ln -s /etc/nginx/sites-available/gestsoft.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```



Finalmente agregar el DNS en la configuracion del servidor DHCP y reiniciamos la IP y probamos con el siguiente comando:

```shell
curl http://www.gestsoft.com
```

## BUGS AND FIXES

Creamos el script de arranque en caso que no exista /etc/init.d/bind9

```shell
#!/bin/bash

### BEGIN INIT INFO
# Provides:          bind9
# Required-Start:    $remote_fs $syslog
# Required-Stop:     $remote_fs $syslog
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Start BIND9
# Description:       Start the BIND9 DNS server
### END INIT INFO

BIND9_BIN="/usr/sbin/named"
BIND9_CONF="/etc/bind/named.conf"
BIND9_OPTS="-c $BIND9_CONF"

case "$1" in
  start)
    echo "Starting BIND9..."
    $BIND9_BIN $BIND9_OPTS
    ;;
  stop)
    echo "Stopping BIND9..."
    killall named
    ;;
  restart)
    echo "Restarting BIND9..."
    killall named
    sleep 2
    $BIND9_BIN $BIND9_OPTS
    ;;
  status)
    pidof named > /dev/null
    if [ $? -eq 0 ]; then
      echo "BIND9 is running"
    else
      echo "BIND9 is not running"
    fi
    ;;
  *)
    echo "Usage: $0 {start|stop|restart|status}"
    exit 1
    ;;
esac

exit 0
```
