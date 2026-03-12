# Documentación Completa de Implementación de Servidor

## Ubuntu 22.04 LTS

Autor: Jorge\
Sistema Operativo: Ubuntu 22.04 LTS

------------------------------------------------------------------------

# 1. Introducción

Este documento describe la implementación completa de un servidor Linux
utilizando Ubuntu 22.04 LTS.\
El objetivo es configurar múltiples servicios de red que permitan
ofrecer funcionalidades de infraestructura dentro de una red local.

Los servicios implementados incluyen:

-   DHCP
-   DNS
-   SSH
-   FTP
-   TFTP
-   HTTP
-   HTTPS
-   NFS
-   LDAP
-   Correo electrónico (SMTP, POP3, IMAP, SASL)
-   Proxy

El servidor está diseñado para funcionar incluso cuando se mueve entre
diferentes redes, como una red doméstica o una red escolar.

------------------------------------------------------------------------

# 2. Preparación del sistema

## 2.1 Actualización del sistema

Antes de instalar cualquier servicio se debe actualizar el sistema.

sudo apt update sudo apt upgrade -y

------------------------------------------------------------------------

## 2.2 Instalación de herramientas básicas

sudo apt install net-tools curl wget git -y

Estas herramientas permiten administrar la red y probar servicios.

Para verificar la dirección IP:

ip a

o

hostname -I

------------------------------------------------------------------------

# 3. Configuración para funcionar en diferentes redes

El servidor debe obtener su dirección IP automáticamente usando DHCP.

Editar archivo de configuración de red:

sudo nano /etc/netplan/01-network-manager-all.yaml

Configuración recomendada:

network: version: 2 renderer: NetworkManager ethernets: enp0s3: dhcp4:
true

Aplicar cambios:

sudo netplan apply

Esto permite que el servidor funcione correctamente en redes diferentes.

------------------------------------------------------------------------

# 4. Configuración del nombre del servidor

sudo nano /etc/hostname

Ejemplo:

jorge-server

Editar archivo hosts:

sudo nano /etc/hosts

Agregar:

127.0.0.1 jorge-server

Reiniciar el sistema:

sudo reboot

------------------------------------------------------------------------

# 5. Servicio DHCP

Instalación:

sudo apt install isc-dhcp-server

Archivo de configuración:

/etc/dhcp/dhcpd.conf

Ejemplo:

subnet 192.168.10.0 netmask 255.255.255.0 {

range 192.168.10.50 192.168.10.100;

option routers 192.168.10.1;

option domain-name-servers 192.168.10.1;

}

Puerto DHCP:

67

------------------------------------------------------------------------

# 6. Servicio DNS

Instalación:

sudo apt install bind9

Archivo de configuración:

/etc/bind/named.conf.local

Ejemplo de zona:

zone "jorge.local" { type master; file "/etc/bind/db.jorge"; };

Puerto DNS:

53

------------------------------------------------------------------------

# 7. Servicio SSH

Instalación:

sudo apt install openssh-server

Puerto SSH:

22

Conexión remota:

ssh usuario@ip

------------------------------------------------------------------------

# 8. Servicio FTP

Instalación:

sudo apt install vsftpd

Archivo de configuración:

/etc/vsftpd.conf

Cambiar:

anonymous_enable=NO local_enable=YES write_enable=YES

Puerto FTP:

21

------------------------------------------------------------------------

# 9. Servicio TFTP

Instalar:

sudo apt install tftpd-hpa

Directorio:

/srv/tftp

Puerto:

69

------------------------------------------------------------------------

# 10. Servidor Web HTTP

Instalar Apache:

sudo apt install apache2

Puerto:

80

Directorio web:

/var/www/html

Archivo principal:

index.html

------------------------------------------------------------------------

# 11. Servidor HTTPS

Activar SSL en Apache:

sudo a2enmod ssl

Puerto HTTPS:

443

------------------------------------------------------------------------

# 12. Servicio NFS

Instalación:

sudo apt install nfs-kernel-server

Archivo de configuración:

/etc/exports

Ejemplo:

/home/jorge 192.168.1.0/24(rw,sync)

Puerto:

2049

------------------------------------------------------------------------

# 13. Servicio LDAP

Instalación:

sudo apt install slapd ldap-utils

Puerto LDAP:

389

------------------------------------------------------------------------

# 14. Servidor de correo

Instalar Postfix y Dovecot:

sudo apt install postfix dovecot-imapd dovecot-pop3d

Puertos:

SMTP 25 POP3 110 IMAP 143 SMTPS 465

------------------------------------------------------------------------

# 15. Servidor Proxy

Instalar Squid:

sudo apt install squid

Puerto Proxy:

3128

Archivo configuración:

/etc/squid/squid.conf

------------------------------------------------------------------------

# 16. Firewall

Instalar UFW:

sudo apt install ufw

Abrir puertos:

sudo ufw allow 22 sudo ufw allow 80 sudo ufw allow 21 sudo ufw allow 53
sudo ufw allow 443 sudo ufw allow 3128

Activar firewall:

sudo ufw enable

Ver reglas:

sudo ufw status

------------------------------------------------------------------------

# 17. Verificación de servicios

systemctl status apache2 systemctl status ssh systemctl status bind9
systemctl status vsftpd

Ver puertos abiertos:

sudo ss -tulnp

------------------------------------------------------------------------

# 18. Uso del servidor en diferentes redes

Cuando el servidor cambie de red se debe verificar su nueva IP.

hostname -I

Ejemplo:

Casa: 192.168.138.20

Escuela: 192.168.30.20

Acceso web:

http://IP

Acceso SSH:

ssh usuario@IP

------------------------------------------------------------------------

# 19. Conclusión

Este documento describe la implementación completa de un servidor Linux
con múltiples servicios de red.

La configuración permite ofrecer:

-   servicios web
-   transferencia de archivos
-   acceso remoto
-   resolución de nombres
-   correo electrónico
-   proxy de red

Además, el uso de DHCP permite que el servidor funcione correctamente
incluso cuando se conecta a diferentes redes.
