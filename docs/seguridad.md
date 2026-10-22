# Seguridad del Home Lab

## 1. Introducción

La seguridad del laboratorio se ha desarrollado de forma progresiva, aplicando medidas para proteger el servidor y controlar el acceso remoto.

El objetivo es reducir la exposición de los servicios, limitar el acceso administrativo a dispositivos autorizados y comprobar que las configuraciones aplicadas funcionan correctamente.

Este documento recoge la evolución de la configuración, desde el acceso remoto inicial mediante redirección de puertos hasta el uso de Tailscale, junto con las medidas de protección aplicadas en Ubuntu Server.

## 2. Configuración inicial: redirección de puertos

En la primera fase del laboratorio, se configuró el acceso remoto al servidor mediante una regla de redirección de puertos en el router doméstico.

El puerto externo 2267 se redirigía al puerto 22 del servidor, utilizado por SSH.

Esta configuración permitía iniciar conexiones SSH desde redes externas, pero requería mantener una regla de redirección en el router.

### Aprendizajes de esta fase

- Comprender la función de la redirección de puertos.
- Identificar la relación entre el puerto externo y el puerto interno del servidor.
- Entender cómo se puede acceder a un servicio de la red local desde el exterior.
- Reconocer la importancia de limitar la exposición de los servicios administrativos.

## 3. Evolución del acceso remoto: Tailscale

Posteriormente, se modificó la arquitectura para evitar exponer directamente el servicio SSH a Internet.

Se eliminó la redirección del puerto SSH en el router y se adoptó Tailscale para establecer conectividad privada entre los dispositivos autorizados y el servidor.

Esta solución permite administrar Ubuntu Server desde el portátil Windows y el dispositivo móvil, incluso cuando se encuentran fuera de la red doméstica.

### Ventajas de la nueva configuración

- Se evita mantener una redirección del puerto SSH en el router.
- El acceso remoto se realiza mediante una red privada virtual.
- Se mantiene la posibilidad de administrar el servidor desde diferentes redes.
- Se reduce la exposición directa del servicio SSH a Internet.

## 4. Endurecimiento del servicio SSH

SSH es el mecanismo principal de administración remota del servidor. Por ello, se aplicaron medidas para reforzar la autenticación y limitar las posibilidades de acceso no autorizado.

### 4.1. Autenticación mediante claves ED25519

Se generaron pares de claves SSH ED25519 en los dispositivos cliente.

La clave pública se incorporó al archivo `~/.ssh/authorized_keys` del usuario correspondiente en Ubuntu Server. La clave privada permanece en el dispositivo cliente y se utiliza para demostrar la identidad al iniciar una conexión.

Este método permite autenticarse mediante claves sin tener que introducir la contraseña de la cuenta del servidor en cada conexión.

### 4.2. Desactivación de la autenticación por contraseña

Se modificó la configuración de SSH para desactivar la autenticación mediante contraseña y permitir la autenticación mediante claves públicas.

La configuración efectiva se comprobó mediante el comando:

```bash
sudo sshd -T | grep -E 'pubkeyauthentication|passwordauthentication'
```

El resultado comprobado fue:

```text
pubkeyauthentication yes
passwordauthentication no
```

Esto confirma que la configuración efectiva permite la autenticación mediante claves públicas y desactiva la autenticación por contraseña.

## 5. Configuración del firewall UFW

UFW se utiliza para controlar el tráfico entrante en Ubuntu Server.

La política de entrada se configura para denegar las conexiones entrantes por defecto, permitiendo únicamente el tráfico contemplado en las reglas establecidas.

Las reglas SSH se ajustaron para permitir el acceso desde la red local y a través de la interfaz de Tailscale, eliminando la regla general que permitía conexiones al puerto 22 desde cualquier origen.

También se mantienen las reglas necesarias para Samba.

### Objetivos de la configuración

- Limitar los orígenes desde los que se puede acceder mediante SSH.
- Evitar permisos de entrada más amplios de lo necesario.
- Mantener la conectividad requerida por los servicios utilizados.
- Aplicar un control adicional sobre el tráfico entrante.

## 6. Protección mediante Fail2Ban

Fail2Ban forma parte de las herramientas de protección del laboratorio y está destinado a gestionar los intentos repetidos de autenticación fallidos.

Su función es ayudar a reducir los intentos reiterados de acceso no autorizado mediante el análisis de los registros y la aplicación de medidas de bloqueo cuando se cumplen las condiciones configuradas.

La configuración y las pruebas específicas de este servicio podrán ampliarse en esta sección con las evidencias correspondientes.

## 7. Copias de seguridad de configuraciones

Antes de modificar archivos de configuración importantes, se realizan copias de seguridad para conservar una versión anterior y facilitar la recuperación en caso de error.

Esta práctica permite reducir el riesgo de perder una configuración funcional durante las tareas de administración y mantenimiento.

Las copias previas a los cambios forman parte del procedimiento de trabajo del laboratorio.

## 8. Pruebas y verificaciones

Después de aplicar las medidas de seguridad, se realizaron pruebas de conexión y comprobaciones de configuración.

### 8.1. Acceso SSH desde la red local

Se comprobó el acceso al servidor desde el portátil Windows utilizando la dirección privada del servidor.

### 8.2. Acceso SSH mediante Tailscale

Se verificó la conexión desde el portátil Windows mediante la dirección de Tailscale del servidor.

También se comprobó el acceso desde el dispositivo móvil mediante Termius, tanto a través de la red local como utilizando datos móviles con Tailscale activado.

### 8.3. Verificación de la configuración SSH

Se comprobó la configuración efectiva del servicio SSH mediante `sshd -T`, verificando que la autenticación por clave pública estaba habilitada y la autenticación por contraseña desactivada.

Estas pruebas permitieron comprobar que el acceso administrativo seguía funcionando después de aplicar las modificaciones.

## 9. Conclusiones y mejoras futuras

La evolución del laboratorio ha permitido pasar de una configuración basada en la redirección de puertos a otra que utiliza Tailscale para el acceso remoto.

Además, se han aplicado medidas de endurecimiento de SSH y control del tráfico entrante mediante UFW.

Este proceso ha permitido practicar conceptos relacionados con la administración segura de Linux, la autenticación mediante claves, las VPN y la configuración de firewalls.

Como mejora futura, se plantea continuar documentando las configuraciones y pruebas de seguridad, así como ampliar las medidas de aislamiento de la red cuando se implemente la segmentación mediante VLAN.

---

*La documentación se actualizará a medida que se realicen nuevas configuraciones, pruebas y mejoras de seguridad en el laboratorio.*
