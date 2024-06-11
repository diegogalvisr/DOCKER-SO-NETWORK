# INSTALACION SERVIDOR SFTP

Actualizamos paquetes e instalamos el SFTP:

```shell
sudo apt update
sudo apt install vsftpd
```

Configuramos el VSFTP:

```shell
sudo nano /etc/vsftpd.conf
```

Se debe verificar que esta lineas existan tal cual, en caso que este comentadas descomentarlas y dejarlas tal cual:

```shell
# Descomenta o añade estas líneas
listen=YES
listen_ipv6=NO
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022
chroot_local_user=YES
allow_writeable_chroot=YES
rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem
rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key
ssl_enable=YES
allow_anon_ssl=NO
force_local_data_ssl=YES
force_local_logins_ssl=YES
ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO
require_ssl_reuse=NO
ssl_ciphers=HIGH

```

Reiniciamos el servicio:

```shell
service vsftp restart
```

Creamos el usuario para el login en especifico del SFTP:

```shell
adduser yksogeid
```

Cambiamos el directorio de inicio:

```shell
usermod -d /mnt/yksogeid yksogeid
```

Creamos el directorio y le cambiamos el propietario:

```shell
mkdir -p /mnt/yksogeid
chown yksogeid:yksogeid /mnt/yksogeid

```

Y listo, ahora ingresamos por Filezilla o WinSCP como :

WINDOWS:

```shell
SERVIDOR: 127.168.1.30
PORT: 22
USUARIO: yksogeid
CLAVE: yksogeid
```

DESDE UN CONTENEDOR:

```shell
sftp -i /mnt/public-keys/id_rsa yksogeid@192.168.1.30
```
