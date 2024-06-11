# SERVICE DHCP IN DOCKER DESKTOP

Inicialmente actualizamos e instalamos el paquete de DHCP de la siguiente manera:

```shell
apt update
apt install isc-dhcp-server
```

En la siguiente ruta /etc/dhcp/dhcpd.conf encontramos el archivo de configuracion del DHCP el cual modificaremos de la siguiente manera.

Instalamos un editor de texto, si ya cuenta con uno abran ese archivo, si no instale el nano de la siguiente manera:

```shell
apt install nano
```

Luego abrimos el archivo de la siguiente manera:

```shell
nano /etc/dhcp/dhcpd.conf
```

Una vez abierto, ingresamos las siguientes configuraciones:

```shell
option domain-name "tu-dominio.local";

option domain-name-servers ns1.tu-dominio.local, ns2.tu-dominio.local;

default-lease-time 600;
max-lease-time 7200;

authoritative;

subnet 192.168.1.0 netmask 255.255.255.0 {
    range 192.168.1.40 192.168.1.50;
    option routers 192.168.1.1;
    option subnet-mask 255.255.255.0;
    option broadcast-address 192.168.1.255;
    option domain-name-servers 8.8.8.8, 8.8.4.4;
}8.8.8.8, 8.8.4.4;
}
```

Iniciamos y habilitamos el servidor de DHCP:

```shell
service isc-dhcp-server start
service isc-dhcp-server enable
```

Verificamos que el servicio este corriendo con el siguiente comando:

```shell
service isc-dhcp-server status
```
