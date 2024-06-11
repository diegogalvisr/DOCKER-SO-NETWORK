# DOCKER DESKTOP

#### Verificar solicitudes DHCP desde el servidor configurado, en este serverOslo.

```shell
tcpdump -i eth0 port 67 or port 68 -n
```

- **Port 67**: Es el puerto de destino utilizado por el servicio de protocolo DHCP (Dynamic Host Configuration Protocol) para recibir mensajes de solicitud de configuración de direcciones IP de los clientes DHCP.

- **Port 68**: Es el puerto de destino utilizado por los clientes DHCP para enviar mensajes de solicitud de configuración de direcciones IP al servidor DHCP.

### Verificar la tabla de arrendamiento

```shell
cat /var/lib/dhcp/dhcpd.leases
```

#### Realizar el cambio de IP por DINAMICA que nos otorgue el servicio DHCP.

Se debe ingresar al contenedor con el siguiente comando:

```shell
docker exec -it idContenedor bash
```

el idContenedor se toma despues de ingresar el comando y ver el contenedor:

```shell
docker ps
```

Una vez ingresamos al contenedor, debemos consultar la ip de la interfaz eth0, esto lo realizamos con el comando:

```shell
ip a
```

En este caso cuenta con la ip 192.168.1.2/24, por ende utilizamos el siguiente comando para eliminar esa configuracion de la interfaz.

```shell
ip addr del 192.168.1.2/24 dev eth0
```

Una vez realizado el paso anterior, debemos asignarle la nueva IP que nos otorgue el servidor DHCP mediante el comando:

```shell
dhclient -r eth0 && dhclient eth0
```

Validamos que se realice la asignacion de la IP que nos otorga el servidor DHCP mediante el comando:

```shell
ip a
```
