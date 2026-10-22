# Servicios del Home Lab

## 1. Introducción

El servidor Ubuntu Server 24.04 LTS proporciona un entorno de prácticas para la administración de sistemas, el acceso remoto, la gestión de bases de datos, el intercambio de archivos y el despliegue de contenedores.

En este documento se describen los cuatro servicios y tecnologías principales implementados en el laboratorio: SSH, MariaDB, Samba y Docker.

## 2. SSH (Secure Shell)

### Función

SSH permite administrar el servidor de forma remota mediante una conexión cifrada.

### Configuración

Se instaló OpenSSH Server para habilitar el acceso remoto al sistema. La administración se realiza desde el portátil Windows y el dispositivo móvil Google Pixel 7a mediante Termius.

La configuración actual permite conexiones desde la red local y a través de la interfaz de Tailscale.

### Aplicación en el laboratorio

SSH es el principal mecanismo de administración remota y permite realizar tareas de configuración y mantenimiento sin necesidad de conectar una pantalla al servidor.

## 3. MariaDB

### Función

MariaDB es un sistema gestor de bases de datos relacionales que permite almacenar, consultar y administrar información mediante SQL.

### Configuración

Se creó un usuario administrativo denominado `lab` con privilegios para gestionar las bases de datos, evitando utilizar directamente la cuenta `root` para las tareas habituales.

Las bases de datos se almacenan en el SSD del servidor.

### Aplicación en el laboratorio

Este servicio proporciona un entorno para practicar SQL y trabajar con bases de datos relacionales dentro del laboratorio.

## 4. Samba

### Función

Samba permite compartir archivos a través de la red mediante los protocolos SMB/CIFS, facilitando el intercambio de información entre diferentes sistemas operativos.

### Configuración

Se configuró una carpeta compartida denominada `PRUEBAS_ASIR` para realizar pruebas de intercambio de archivos en red.

Antes de modificar la configuración de Samba, se realizó una copia de seguridad del archivo `smb.conf` para conservar la configuración anterior y facilitar su recuperación si fuera necesario.

### Aplicación en el laboratorio

El servicio permite practicar la configuración de recursos compartidos y el acceso a archivos desde distintos dispositivos de la red.

## 5. Docker

### Función

Docker permite desplegar aplicaciones y servicios mediante contenedores aislados, facilitando la creación y gestión de entornos de prueba.

### Configuración

Se instaló el motor Docker y se configuraron permisos de usuario para ejecutar contenedores sin utilizar directamente `root` en las operaciones habituales.

### Aplicación en el laboratorio

Docker proporciona una base para desplegar servicios adicionales, experimentar con entornos aislados y preparar futuras prácticas relacionadas con retos CTF (*Capture The Flag*).

## 6. Resumen de los servicios

| Servicio o tecnología | Función principal | Aplicación en el laboratorio |
|---|---|---|
| SSH | Administración remota cifrada. | Configuración y mantenimiento de Ubuntu Server. |
| MariaDB | Gestión de bases de datos relacionales. | Prácticas de SQL. |
| Samba | Intercambio de archivos mediante SMB/CIFS. | Carpeta compartida `PRUEBAS_ASIR`. |
| Docker | Despliegue de contenedores. | Pruebas con servicios aislados y preparación de entornos CTF. |

## 7. Ampliaciones futuras

El laboratorio está en desarrollo. La documentación de los servicios se ampliará a medida que se incorporen nuevas configuraciones, se realicen pruebas adicionales y se desplieguen nuevos entornos de prácticas.

---

*Los servicios descritos forman parte del laboratorio personal de aprendizaje de administración de sistemas y redes dentro del ciclo formativo ASIR.*
