## 4 SERVIDORES CON SERVICIOS DIFERENTES Y SO COMO UBUNTU Y DEBIAN EN DOCKER.

#### Ingresar serverParis desde windows

```shell
ssh -i C:\Users\juang\.ssh\id_rsa -p 25 root@127.168.1.5
```

#### Ingresar serverParis desde cualquier contenedor

```shell
ssh -i /mnt/public-keys/id_rsa -p 22 root@192.168.1.5
```

#### Ingresar serverOslo desde windows

```shell
ssh -i C:\Users\juang\.ssh\id_rsa -p 24 root@127.168.1.10
```

#### Ingresar serverOslo desde cualquier contenedor

```shell
ssh -i /mnt/public-keys/id_rsa -p 22 root@192.168.1.10
```

#### Ingresar serverMadrid desde windows

```shell
ssh -i C:\Users\juang\.ssh\id_rsa -p 23 root@127.168.1.20
```

#### Ingresar serverMadrid desde cualquier contenedor

```shell
ssh -i /mnt/public-keys/id_rsa -p 22 root@192.168.1.20
```

#### Ingresar serverManchester desde windows

```shell
ssh -i C:\Users\juang\.ssh\id_rsa -p 22 root@127.168.1.30
```

#### Ingresar serverManchester desde cualquier contenedor

```shell
ssh -i /mnt/public-keys/id_rsa -p 22 root@192.168.1.30
```

#### Copiar archivos desde windows hacia una ruta de un contenedor

```shell
chmod 600 /mnt/public-keys/id_rsa
```

```shell
docker cp C:\Users\juang\.ssh\id_rsa 65baee3d7131:/mnt/public-keys/id_rsa
```

#### Para crear la red de docker use esto:
```shell
docker network create --driver=bridge --subnet=192.168.1.0/24 --gateway=192.168.1.1
```