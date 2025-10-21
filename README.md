# Instalación y Configuración de PostgreSQL en Red

### Configuración de red - Servidor (`/etc/netplan/50-cloud-init.yaml`)

santino@scolmena-server:~$ sudo nano /etc/netplan/50-cloud-init.yaml

```yaml
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      addresses:
      - "172.16.50.2/24"
      dhcp4: no
```

---

### Comprobar interfaces de red (Servidor)

santino@scolmena-server:~$ ip a
<details>
<summary>Resultado del comando</summary>
  
```

1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:27:3c:b0 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 metric 100 brd 10.0.2.255 scope global dynamic enp0s3
       valid_lft 86033sec preferred_lft 86033sec
    inet6 fd17:625c:f037:2:a00:27ff:fe27:3cb0/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 86324sec preferred_lft 14324sec
    inet6 fe80::a00:27ff:fe27:3cb0/64 scope link 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:1b:9c:ba brd ff:ff:ff:ff:ff:ff
    inet 172.16.50.2/24 brd 172.16.50.255 scope global enp0s8
       valid_lft forever preferred_lft foreverResultado
    inet6 fe80::a00:27ff:fe1b:9cba/64 scope link 
       valid_lft forever preferred_lft forever
```
</details>


---

### Configuración de red - Cliente (`en mi caso es Conexión cableada 2.nmconnection`)

santino@scolmena-client:~$ sudo cat "/etc/NetworkManager/system-connections/Conexión cableada 2.nmconnection"
<details>
<summary>Contenido del archivo</summary>

```
[connection]
id=Conexión cableada 2
uuid=1c198e9b-7663-3249-8bb7-eeee2129a503
type=ethernet
interface-name=enp0s8
timestamp=1760991774

[ipv4]
address1=172.16.50.3/24
method=manual

[ipv6]
addr-gen-mode=stable-privacy
method=auto
```
</details>

---

### Comprobar interfaces de red (Cliente)

santino@scolmena-client:~$ ip a
<details>
<summary>Resultado del comando</summary>

  ```
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:29:b9:1d brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic noprefixroute enp0s3
       valid_lft 85906sec preferred_lft 85906sec
    inet6 fd17:625c:f037:2:7a83:98e0:a7b3:6bc6/64 scope global temporary dynamic 
       valid_lft 86390sec preferred_lft 14390sec
    inet6 fd17:625c:f037:2:277f:1c4c:a07c:c989/64 scope global dynamic mngtmpaddr noprefixroute 
       valid_lft 86390sec preferred_lft 14390sec
    inet6 fe80::1194:43a3:57e1:1162/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:24:dd:23 brd ff:ff:ff:ff:ff:ff
    inet 172.16.50.3/24 brd 172.16.50.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::1d07:ed13:b776:21dd/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
```
</details>

---

### Instalación de repositorios PostgreSQL

santino@scolmena-server:~$ sudo apt install -y postgresql-common
<details>
<summary>Resultado del comando</summary>
  
  ```
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Se instalarán los siguientes paquetes adicionales:
  libcommon-sense-perl libjson-perl libjson-xs-perl libtypes-serialiser-perl postgresql-client-common ssl-cert
Se instalarán los siguientes paquetes NUEVOS:
  libcommon-sense-perl libjson-perl libjson-xs-perl libtypes-serialiser-perl postgresql-client-common postgresql-common ssl-cert
0 actualizados, 7 nuevos se instalarán, 0 para eliminar y 1 no actualizados.
Se necesita descargar 413 kB de archivos.
Se utilizarán 1.394 kB de espacio de disco adicional después de esta operación.
Des:1 http://archive.ubuntu.com/ubuntu noble/main amd64 libjson-perl all 4.10000-1 [81,9 kB]
Des:2 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 postgresql-client-common all 257build1.1 [36,4 kB]
Des:3 http://archive.ubuntu.com/ubuntu noble/main amd64 ssl-cert all 1.1.2ubuntu1 [17,8 kB]
Des:4 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 postgresql-common all 257build1.1 [161 kB]
Des:5 http://archive.ubuntu.com/ubuntu noble/main amd64 libcommon-sense-perl amd64 3.75-3build3 [20,4 kB]
Des:6 http://archive.ubuntu.com/ubuntu noble/main amd64 libtypes-serialiser-perl all 1.01-1 [11,6 kB]
Des:7 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libjson-xs-perl amd64 4.040-0ubuntu0.24.04.1 [83,7 kB]
Descargados 413 kB en 0s (1.071 kB/s)   
Preconfigurando paquetes ...
Seleccionando el paquete libjson-perl previamente no seleccionado.
(Leyendo la base de datos ... 125341 ficheros o directorios instalados actualmente.)
Preparando para desempaquetar .../0-libjson-perl_4.10000-1_all.deb ...
Desempaquetando libjson-perl (4.10000-1) ...
Seleccionando el paquete postgresql-client-common previamente no seleccionado.
Preparando para desempaquetar .../1-postgresql-client-common_257build1.1_all.deb ...
Desempaquetando postgresql-client-common (257build1.1) ...
Seleccionando el paquete ssl-cert previamente no seleccionado.
Preparando para desempaquetar .../2-ssl-cert_1.1.2ubuntu1_all.deb ...
Desempaquetando ssl-cert (1.1.2ubuntu1) ...
Seleccionando el paquete postgresql-common previamente no seleccionado.
Preparando para desempaquetar .../3-postgresql-common_257build1.1_all.deb ...
Añadiendo `desviación de /usr/bin/pg_config a /usr/bin/pg_config.libpq-dev por postgresql-common'
Desempaquetando postgresql-common (257build1.1) ...
Seleccionando el paquete libcommon-sense-perl:amd64 previamente no seleccionado.
Preparando para desempaquetar .../4-libcommon-sense-perl_3.75-3build3_amd64.deb ...
Desempaquetando libcommon-sense-perl:amd64 (3.75-3build3) ...
Seleccionando el paquete libtypes-serialiser-perl previamente no seleccionado.
Preparando para desempaquetar .../5-libtypes-serialiser-perl_1.01-1_all.deb ...
Desempaquetando libtypes-serialiser-perl (1.01-1) ...
Seleccionando el paquete libjson-xs-perl previamente no seleccionado.
Preparando para desempaquetar .../6-libjson-xs-perl_4.040-0ubuntu0.24.04.1_amd64.deb ...
Desempaquetando libjson-xs-perl (4.040-0ubuntu0.24.04.1) ...
Configurando postgresql-client-common (257build1.1) ...
Configurando libcommon-sense-perl:amd64 (3.75-3build3) ...
Configurando ssl-cert (1.1.2ubuntu1) ...
Configurando libtypes-serialiser-perl (1.01-1) ...
Configurando libjson-perl (4.10000-1) ...
Configurando libjson-xs-perl (4.040-0ubuntu0.24.04.1) ...
Configurando postgresql-common (257build1.1) ...

Creating config file /etc/postgresql-common/createcluster.conf with new version
Building PostgreSQL dictionaries from installed myspell/hunspell packages...
Removing obsolete dictionary files:
Created symlink /etc/systemd/system/multi-user.target.wants/postgresql.service → /usr/lib/systemd/system/postgresql.service.
Procesando disparadores para man-db (2.12.0-4build2) ...
Scanning processes...                                                                                                                                                                                       
Scanning linux images...                                                                                                                                                                                    

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

```
</details>

---

### Añadir repositorio APT de PostgreSQL

santino@scolmena-server:~$ sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
<details>
<summary>Resultado del comando</summary>

  ```
This script will enable the PostgreSQL APT repository on apt.postgresql.org on
your system. The distribution codename used will be noble-pgdg.

Press Enter to continue, or Ctrl-C to abort.

Using keyring /usr/share/postgresql-common/pgdg/apt.postgresql.org.gpg
Writing /etc/apt/sources.list.d/pgdg.sources ...

Running apt-get update ...
Des:1 https://apt.postgresql.org/pub/repos/apt noble-pgdg InRelease [107 kB]
Obj:2 http://archive.ubuntu.com/ubuntu noble InRelease                                                              
Obj:3 http://security.ubuntu.com/ubuntu noble-security InRelease                
Obj:4 http://archive.ubuntu.com/ubuntu noble-updates InRelease
Obj:5 http://archive.ubuntu.com/ubuntu noble-backports InRelease
Des:6 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 Packages [350 kB]
Descargados 457 kB en 1s (806 kB/s)
Leyendo lista de paquetes... Hecho

You can now start installing packages from apt.postgresql.org.

Have a look at https://wiki.postgresql.org/wiki/Apt for more information;
most notably the FAQ at https://wiki.postgresql.org/wiki/Apt/FAQ

```
</details>

---

### Instalación de PostgreSQL 18

santino@scolmena-server:~$ sudo apt install postgresql-18
<details>
<summary>Resultado del comando</summary>

  ```
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Se instalarán los siguientes paquetes adicionales:
  libllvm19 libpq5 liburing2 postgresql-18-jit postgresql-client-18 postgresql-client-common postgresql-common
Paquetes sugeridos:
  libpq-oauth postgresql-doc-18
Se instalarán los siguientes paquetes NUEVOS:
  libllvm19 libpq5 liburing2 postgresql-18 postgresql-18-jit postgresql-client-18
Se actualizarán los siguientes paquetes:
  postgresql-client-common postgresql-common
2 actualizados, 6 nuevos se instalarán, 0 para eliminar y 1 no actualizados.
Se necesita descargar 48,6 MB de archivos.
Se utilizarán 201 MB de espacio de disco adicional después de esta operación.
¿Desea continuar? [S/n] s
Des:1 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 postgresql-common all 285.pgdg24.04+1 [112 kB]
Des:2 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 postgresql-client-common all 285.pgdg24.04+1 [47,9 kB]
Des:3 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 libpq5 amd64 18.0-1.pgdg24.04+3 [248 kB]
Des:4 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 postgresql-client-18 amd64 18.0-1.pgdg24.04+3 [2.091 kB]
Des:5 http://archive.ubuntu.com/ubuntu noble-updates/main amd64 libllvm19 amd64 1:19.1.1-1ubuntu1~24.04.2 [28,7 MB]
Des:6 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 postgresql-18 amd64 18.0-1.pgdg24.04+3 [7.516 kB]
Des:7 https://apt.postgresql.org/pub/repos/apt noble-pgdg/main amd64 postgresql-18-jit amd64 18.0-1.pgdg24.04+3 [9.861 kB]
Des:8 http://archive.ubuntu.com/ubuntu noble/main amd64 liburing2 amd64 2.5-1build1 [21,1 kB]
Descargados 48,6 MB en 3s (17,5 MB/s)
Preconfigurando paquetes ...
(Leyendo la base de datos ... 125573 ficheros o directorios instalados actualmente.)
Preparando para desempaquetar .../0-postgresql-common_285.pgdg24.04+1_all.deb ...
Dejando `desviación de /usr/bin/pg_config a /usr/bin/pg_config.libpq-dev por postgresql-common'
Desempaquetando postgresql-common (285.pgdg24.04+1) sobre (257build1.1) ...
Preparando para desempaquetar .../1-postgresql-client-common_285.pgdg24.04+1_all.deb ...
Desempaquetando postgresql-client-common (285.pgdg24.04+1) sobre (257build1.1) ...
Seleccionando el paquete libllvm19:amd64 previamente no seleccionado.
Preparando para desempaquetar .../2-libllvm19_1%3a19.1.1-1ubuntu1~24.04.2_amd64.deb ...
Desempaquetando libllvm19:amd64 (1:19.1.1-1ubuntu1~24.04.2) ...
Seleccionando el paquete libpq5:amd64 previamente no seleccionado.
Preparando para desempaquetar .../3-libpq5_18.0-1.pgdg24.04+3_amd64.deb ...
Desempaquetando libpq5:amd64 (18.0-1.pgdg24.04+3) ...
Seleccionando el paquete liburing2:amd64 previamente no seleccionado.
Preparando para desempaquetar .../4-liburing2_2.5-1build1_amd64.deb ...
Desempaquetando liburing2:amd64 (2.5-1build1) ...
Seleccionando el paquete postgresql-client-18 previamente no seleccionado.
Preparando para desempaquetar .../5-postgresql-client-18_18.0-1.pgdg24.04+3_amd64.deb ...
Desempaquetando postgresql-client-18 (18.0-1.pgdg24.04+3) ...
Seleccionando el paquete postgresql-18 previamente no seleccionado.
Preparando para desempaquetar .../6-postgresql-18_18.0-1.pgdg24.04+3_amd64.deb ...
Desempaquetando postgresql-18 (18.0-1.pgdg24.04+3) ...
Seleccionando el paquete postgresql-18-jit previamente no seleccionado.
Preparando para desempaquetar .../7-postgresql-18-jit_18.0-1.pgdg24.04+3_amd64.deb ...
Desempaquetando postgresql-18-jit (18.0-1.pgdg24.04+3) ...
Configurando postgresql-client-common (285.pgdg24.04+1) ...
Removing obsolete conffile /etc/postgresql-common/supported_versions ...
Configurando libllvm19:amd64 (1:19.1.1-1ubuntu1~24.04.2) ...
Configurando libpq5:amd64 (18.0-1.pgdg24.04+3) ...
Configurando postgresql-common (285.pgdg24.04+1) ...
Instalando una nueva versión del fichero de configuración /etc/postgresql-common/pg_upgradecluster.d/analyze ...
Replacing config file /etc/postgresql-common/createcluster.conf with new version
Configurando liburing2:amd64 (2.5-1build1) ...
Configurando postgresql-client-18 (18.0-1.pgdg24.04+3) ...
update-alternatives: utilizando /usr/share/postgresql/18/man/man1/psql.1.gz para proveer /usr/share/man/man1/psql.1.gz (psql.1.gz) en modo automático
Configurando postgresql-18 (18.0-1.pgdg24.04+3) ...
Creating new PostgreSQL cluster 18/main ...
/usr/lib/postgresql/18/bin/initdb -D /var/lib/postgresql/18/main --auth-local peer --auth-host scram-sha-256 --no-instructions
Los archivos de este cluster serán de propiedad del usuario «postgres».
Este usuario también debe ser quien ejecute el proceso servidor.

El cluster será inicializado con configuración regional «es_ES.UTF-8».
La codificación por omisión ha sido por lo tanto definida a «UTF8».
La configuración de búsqueda en texto ha sido definida a «spanish».

Las sumas de verificación en páginas de datos han sido activadas.

corrigiendo permisos en el directorio existente /var/lib/postgresql/18/main ... hecho
creando subdirectorios ... hecho
seleccionando implementación de memoria compartida dinámica ... posix
seleccionando el valor para «max_connections» ... 100
seleccionando el valor para «shared_buffers» ... 128MB
seleccionando el huso horario por omisión ... Etc/UTC
creando archivos de configuración ... hecho
ejecutando script de inicio (bootstrap) ... hecho
realizando inicialización post-bootstrap ... hecho
sincronizando los datos a disco ... hecho
Configurando postgresql-18-jit (18.0-1.pgdg24.04+3) ...
Procesando disparadores para libc-bin (2.39-0ubuntu8.6) ...
Procesando disparadores para man-db (2.12.0-4build2) ...
Scanning processes...                                                                                                                                                                                       
Scanning linux images...                                                                                                                                                                                    

Running kernel seems to be up-to-date.

No services need to be restarted.

No containers need to be restarted.

No user sessions are running outdated binaries.

No VM guests are running outdated hypervisor (qemu) binaries on this host.

```
</details>

---

### Verificar versión de PostgreSQL

santino@scolmena-server:~$ psql --version
<details>
<summary>Resultado del comando</summary>
  
  ```
psql (PostgreSQL) 18.0 (Ubuntu 18.0-1.pgdg24.04+3)
  ```
</details>

---

### Verificar estado del servicio

santino@scolmena-server:~$ sudo systemctl status postgresql
<details>
<summary>Resultado del comando</summary>
  
  ```
● postgresql.service - PostgreSQL RDBMS
     Loaded: loaded (/usr/lib/systemd/system/postgresql.service; enabled; preset: enabled)
     Active: active (exited) since Mon 2025-10-20 20:49:44 UTC; 5min ago
   Main PID: 6556 (code=exited, status=0/SUCCESS)
        CPU: 2ms

oct 20 20:49:44 scolmena-server systemd[1]: Starting postgresql.service - PostgreSQL RDBMS...
oct 20 20:49:44 scolmena-server systemd[1]: Finished postgresql.service - PostgreSQL RDBMS.
```
</details>

---

### Acceder al usuario `postgres`

#### santino@scolmena-server:~$ sudo -i -u postgres
#### postgres@scolmena-server:~$ psql
<details>
<summary>Resultado del comando</summary>
  
  ```
psql (18.0 (Ubuntu 18.0-1.pgdg24.04+3))
Digite «help» para obtener ayuda.
```
</details>

---

### Configurar acceso remoto
#### Archivo postgresql.conf
santino@scolmena-server:~$ sudo nano /etc/postgresql/18/main/postgresql.conf
##### Modificación del archivo
```yaml
listen_addresses = '*'
```

#### Archivo pg_hba.conf
santino@scolmena-server:~$ sudo nano /etc/postgresql/18/main/pg_hba.conf
##### Autorización de red
```yaml
host    all             all             172.16.50.0/24          md5
```

#### Reiniciar el Servicio Postgresql
santino@scolmena-server:~$ sudo systemctl restart postgresql

---

### Crear usuario administrador (Con contraseña `@lumne3$$`)

#### santino@scolmena-server:~$ sudo -i -u postgres
#### postgres@scolmena-server:~$ createuser --interactive --pwprompt
<details>
<summary>Resultado del comando</summary>
  
  ```
Ingrese el nombre del rol a agregar: scolmena
Ingrese la contraseña para el nuevo rol:
¿Será el nuevo rol un superusuario? (s/n) s
```
</details>

---

### Crear base de datos

santino@scolmena-server:~$ psql -U scolmena -h localhost
CREATE DATABASE scolmena OWNER scolmena;

<details>
<summary>Resultado</summary>
  
  ```
CREATE DATABASE
```
</details>

---

### Crear base de datos `mvm_asgbd`

CREATE DATABASE mvm_asgbd;
\c mvm_asgbd;

<details>
<summary>Resultado del comando</summary>
  
  ```
Conectado a la base de datos «mvm_asgbd» con el usuario «scolmena».
```
</details>

---

### Crear tabla `mascotes`

```yaml
CREATE TABLE mascotes (
id SERIAL PRIMARY KEY,
num_chip VARCHAR(20),
nom VARCHAR(50),
especie VARCHAR(30),
descripcio TEXT
);
```
<details>
<summary>Resultado</summary>
  
  ```
CREATE TABLE
```
</details>

---

### Insertar registros

```yaml
INSERT INTO mascotes (num_chip, nom, especie, descripcio)
VALUES
('Chip1', 'Boby', 'Perro', 'Galgo, color marrón'),
('Chip2', 'Michi', 'Gato', 'Gato negro, ojos verdes'),
('Chip3', 'Luna', 'Conejo', 'Orejas largas, pelaje blanco'),
('Chip4', 'Rex', 'Perro', 'Pastor alemán'),
('Chip5', 'Nemo', 'Pez', 'Pez payaso');
```
<details>
<summary>Resultado</summary>
INSERT 0 5
</details>

---

### Consultar registros

```yaml
SELECT * FROM mascotes;
```
<details>
<summary>Resultado</summary>
  
| id | num_chip | nom  | especie | descripcio                |
|----|----------|------|---------|---------------------------|
| 1  | Chip1    | Boby | Perro   | Galgo, color marrón       |
| 2  | Chip2    | Michi| Gato    | Gato negro, ojos verdes   |
| 3  | Chip3    | Luna | Conejo  | Orejas largas, pelaje blanco |
| 4  | Chip4    | Rex  | Perro   | Pastor alemán             |
| 5  | Chip5    | Nemo | Pez     | Pez payaso                |

(5 rows)

</details>

---

### Modificar y eliminar registros

```yaml
UPDATE mascotes SET nom='Michi 2.0' WHERE id=2;
DELETE FROM mascotes WHERE id=4;
SELECT * FROM mascotes;
```

<details>
<summary>Resultado</summary>
  
| id | num_chip |    nom    | especie |          descripcio          |
|----|----------|-----------|---------|-----------------------------|
|  1 | Chip1    | Boby      | Perro   | Galgo, color marrón          |
|  3 | Chip3    | Luna      | Conejo  | Orejas largas, pelaje blanco |
|  5 | Chip5    | Nemo      | Pez     | Pez payaso                  |
|  2 | Chip2    | Michi 2.0 | Gato    | Gato negro, ojos verdes      |

(4 rows)
</details>

---

### Instalar cliente PostgreSQL

santino@scolmena-client:~$ sudo apt install postgresql-client -y
<details>
<summary>Resultado</summary>

  ```
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Leyendo la información de estado... Hecho
Se instalarán los siguientes paquetes adicionales:
  libpq5 postgresql-client-14 postgresql-client-common
Paquetes sugeridos:
  postgresql-14 postgresql-doc-14
Se instalarán los siguientes paquetes NUEVOS:
  libpq5 postgresql-client postgresql-client-14 postgresql-client-common
0 actualizados, 4 nuevos se instalarán, 0 para eliminar y 1 no actualizados.
Se necesita descargar 1.435 kB de archivos.
Se utilizarán 4.881 kB de espacio de disco adicional después de esta operación.
Des:1 http://es.archive.ubuntu.com/ubuntu jammy-updates/main amd64 libpq5 amd64 14.19-0ubuntu0.22.04.1 [152 kB]
Des:2 http://es.archive.ubuntu.com/ubuntu jammy/main amd64 postgresql-client-common all 238 [29,6 kB]
Des:3 http://es.archive.ubuntu.com/ubuntu jammy-updates/main amd64 postgresql-client-14 amd64 14.19-0ubuntu0.22.04.1 [1.249 kB]
Des:4 http://es.archive.ubuntu.com/ubuntu jammy/main amd64 postgresql-client all 14+238 [3.292 B]
Descargados 1.435 kB en 1s (1.022 kB/s)       
Seleccionando el paquete libpq5:amd64 previamente no seleccionado.
(Leyendo la base de datos ... 210320 ficheros o directorios instalados actualmente.)
Preparando para desempaquetar .../libpq5_14.19-0ubuntu0.22.04.1_amd64.deb ...
Desempaquetando libpq5:amd64 (14.19-0ubuntu0.22.04.1) ...
Seleccionando el paquete postgresql-client-common previamente no seleccionado.
Preparando para desempaquetar .../postgresql-client-common_238_all.deb ...
Desempaquetando postgresql-client-common (238) ...
Seleccionando el paquete postgresql-client-14 previamente no seleccionado.
Preparando para desempaquetar .../postgresql-client-14_14.19-0ubuntu0.22.04.1_amd64.deb ...
Desempaquetando postgresql-client-14 (14.19-0ubuntu0.22.04.1) ...
Seleccionando el paquete postgresql-client previamente no seleccionado.
Preparando para desempaquetar .../postgresql-client_14+238_all.deb ...
Desempaquetando postgresql-client (14+238) ...
Configurando postgresql-client-common (238) ...
Configurando libpq5:amd64 (14.19-0ubuntu0.22.04.1) ...
Configurando postgresql-client-14 (14.19-0ubuntu0.22.04.1) ...
update-alternatives: utilizando /usr/share/postgresql/14/man/man1/psql.1.gz para proveer /usr/share/man/man1/psql.1.gz (psql.1.gz) en modo automático
Configurando postgresql-client (14+238) ...
Procesando disparadores para man-db (2.10.2-1) ...
Procesando disparadores para libc-bin (2.35-0ubuntu3.11) ...
```
</details>

---

### Comprobar conexión remota

santino@scolmena-client:~$ psql -h 172.16.50.2 -U scolmena -d mvm_asgbd
<details>
<summary>Resultado</summary>
  
  ```
Password for user scolmena: 
psql (14.19 (Ubuntu 14.19-0ubuntu0.22.04.1), server 18.0 (Ubuntu 18.0-1.pgdg24.04+3))
WARNING: psql major version 14, server major version 18.
         Some psql features might not work.
SSL connection (protocol: TLSv1.3, cipher: TLS_AES_256_GCM_SHA384, bits: 256, compression: off)
Type "help" for help.
```
</details>

---

### Visualización de valores (Cliente)

#### mvm_asgbd=# SELECT * FROM mascotes;
<details>
<summary>Resultado</summary>
  
| id | num_chip |    nom    | especie |          descripcio          |
|----|----------|-----------|---------|-----------------------------|
|  1 | Chip1    | Boby      | Perro   | Galgo, color marrón          |
|  3 | Chip3    | Luna      | Conejo  | Orejas largas, pelaje blanco |
|  5 | Chip5    | Nemo      | Pez     | Pez payaso                  |
|  2 | Chip2    | Michi 2.0 | Gato    | Gato negro, ojos verdes      |

(4 rows)

</details>

### Modificar y eliminar de valores (Cliente)

#### mvm_asgbd=# UPDATE mascotes SET nom='Michi Client' WHERE num_chip='Chip2';
UPDATE 1
#### mvm_asgbd=# DELETE FROM mascotes WHERE num_chip='Chip3';
DELETE 1

#### mvm_asgbd=# SELECT * FROM mascotes;
<details>
<summary>Resultado</summary>
  
| id | num_chip |     nom      | especie |       descripcio        |
|----|----------|--------------|---------|-------------------------|
|  1 | Chip1    | Boby         | Perro   | Galgo, color marrón     |
|  5 | Chip5    | Nemo         | Pez     | Pez payaso              |
|  2 | Chip2    | Michi Client | Gato    | Gato negro, ojos verdes |
  
(3 rows)
</details>



