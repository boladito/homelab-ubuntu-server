# Arquitectura del Home Lab

## 1. Descripción general

El laboratorio está organizado alrededor de un servidor Ubuntu Server 24.04 LTS, administrado desde dos dispositivos cliente: un portátil con Windows 11 y un Google Pixel 7a con GrapheneOS.

La arquitectura permite administrar el servidor tanto desde la red doméstica como desde redes externas mediante Tailscale. El acceso administrativo se realiza mediante SSH y está protegido por autenticación con claves y reglas de firewall.

## 2. Dispositivos y funciones

| Dispositivo | Función | Herramientas principales |
|---|---|---|
| Ubuntu Server | Servidor central del laboratorio y entorno de prácticas. | Netplan, SSH, UFW, Fail2Ban, MariaDB, Samba y Docker. |
| Portátil Windows 11 | Administración del servidor desde el equipo principal. | Cliente SSH y Tailscale. |
| Google Pixel 7a | Administración del servidor desde el dispositivo móvil. | Termius y Tailscale. |

## 3. Esquema lógico de la arquitectura

```text
          DISPOSITIVOS CLIENTE

    +-------------------+    +-------------------+
    | Portátil Windows  |    | Google Pixel 7a   |
    | Windows 11        |    | GrapheneOS        |
    +---------+---------+    +---------+---------+
              |                        |
     Conexión | SSH           Conexión | SSH
              |                        |
              +-----------+------------+
                          |
                 +--------v--------+
                 |                 |
                 |  Ubuntu Server  |
                 |       SSH       |
                 |    Servicios    |
                 | del laboratorio |
                 +-----------------+
```

El esquema representa los dispositivos que participan en la administración del laboratorio. Las conexiones pueden establecerse a través de la red local o mediante Tailscale, según la ubicación del dispositivo cliente.

## 4. Vías de acceso al servidor

### 4.1. Acceso desde la red doméstica

Cuando el portátil o el móvil están conectados a la red doméstica, pueden acceder al servidor mediante SSH utilizando la conectividad de la red local.

La configuración de la interfaz de red del servidor se gestiona mediante Netplan.

### 4.2. Acceso remoto mediante Tailscale

Cuando un dispositivo cliente se encuentra fuera de la red doméstica, Tailscale proporciona conectividad privada con el servidor.

Esta solución permite administrar el sistema mediante SSH sin mantener una redirección del puerto SSH en el router.

### 4.3. Esquema de conectividad

```text
ACCESO LOCAL

Portátil Windows / Google Pixel 7a
                 |
          Red doméstica
                 |
                SSH
                 |
           Ubuntu Server


ACCESO REMOTO

Portátil Windows / Google Pixel 7a
                 |
             Tailscale
            VPN privada
                 |
                SSH
                 |
           Ubuntu Server
```

Ambas vías permiten administrar el mismo servidor. El acceso mediante Tailscale evita depender de la exposición directa del servicio SSH a Internet.

## 5. Seguridad de las comunicaciones

La arquitectura incorpora diferentes medidas para proteger el acceso administrativo:

- **SSH:** permite administrar el servidor de forma remota mediante una conexión cifrada.
- **Claves ED25519:** se utilizan para autenticar los dispositivos cliente.
- **Autenticación por contraseña desactivada:** el acceso SSH se configura para utilizar claves públicas.
- **UFW:** restringe el tráfico entrante mediante reglas de firewall.
- **Fail2Ban:** ayuda a gestionar los intentos repetidos de autenticación fallidos.
- **Tailscale:** proporciona una vía privada para la administración remota.

Estas medidas se complementan para reducir la exposición del servidor y limitar el acceso a los dispositivos autorizados.

## 6. Evolución de la arquitectura

En la primera fase del proyecto, el acceso remoto se configuró mediante una redirección de puertos en el router.

Posteriormente, la arquitectura se modificó para eliminar esa redirección y utilizar Tailscale como mecanismo de acceso remoto.

Esta evolución permite mantener la administración remota sin publicar directamente el servicio SSH en Internet.

## 7. Mejoras futuras

Como siguiente fase se plantea diseñar e implementar una segmentación de la red mediante VLAN.

El objetivo será estudiar la separación de dispositivos en diferentes segmentos y aplicar reglas de comunicación entre ellos para mejorar el aislamiento de la red.

Esta funcionalidad todavía está pendiente de implementación.

---

*La arquitectura se actualizará a medida que se incorporen nuevos servicios, se realicen pruebas y se implementen mejoras en el laboratorio.*
