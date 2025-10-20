<details>
<summary>Comando: sudo apt install postgresql-18 -y</summary>

```bash
[sudo] contraseña para santino: 
Leyendo lista de paquetes... Hecho
Creando árbol de dependencias... Hecho
Se instalarán los siguientes paquetes:
  postgresql-18 postgresql-client-18 postgresql-contrib-18 ...
0 actualizados, 3 nuevos se instalarán, 0 para eliminar y 1 no actualizados.
Se necesitan descargar 12,3 MB de archivos.
Después de esta operación, se usarán 48,0 MB de espacio adicional.
</details> <details> <summary>Editar Netplan: sudo nano /etc/netplan/50-cloud-init.yaml</summary>
yaml
Copiar código
network:
  version: 2
  ethernets:
    enp0s3:
      dhcp4: true
    enp0s8:
      addresses:
      - "172.16.50.2/24"
      dhcp4: no
</details> <details> <summary>Ver interfaces: ip a (Servidor)</summary>
bash
Copiar código
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 10.0.2.15/24 scope global dynamic enp0s3
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 172.16.50.2/24 scope global enp0s8
</details> <details> <summary>Cliente - Ver configuración NM</summary>
ini
Copiar código
[connection]
id=Conexión cableada 2
uuid=1c198e9b-7663-3249-8bb7-eeee2129a503
type=ethernet
autoconnect-priority=-999
interface-name=enp0s8
timestamp=1760991774

[ethernet]

[ipv4]
address1=172.16.50.3/24
method=manual

[ipv6]
addr-gen-mode=stable-privacy
method=auto

[proxy]
</details> <details> <summary>Cliente - Ver interfaces: ip a</summary>
bash
Copiar código
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 ...
    inet 127.0.0.1/8 scope host lo
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 10.0.2.15/24 scope global dynamic noprefixroute enp0s3
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 ...
    inet 172.16.50.3/24 scope global noprefixroute enp0s8
</details> ```
