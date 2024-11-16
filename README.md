# 📘 Configuración de Servidor Web Nginx

Este proyecto configura un servidor web Nginx en una máquina virtual Debian utilizando Vagrant. El servidor está configurado para servir contenido estático mediante un archivo de configuración personalizado en el directorio `sites-available` de Nginx.

## 📑 Descripción General

Este proyecto proporciona un entorno web configurado en una máquina virtual usando Vagrant para la gestión y Nginx como servidor. Se configuran directorios, permisos y archivos necesarios para servir una página estática a través del dominio `pedro` en la IP `192.168.57.103`.

Este README describe paso a paso la configuración de Nginx, desde la instalación hasta la verificación del correcto funcionamiento del sitio.

---

## 📌 Objetivos del Proyecto

1. Instalar y configurar Nginx en una máquina virtual Debian.
2. Configurar un sitio web en el directorio `/var/www/pedro/html`.
3. Crear un archivo de configuración de sitio en `sites-available` y enlazarlo a `sites-enabled`.
4. Asignar el dominio `pedro` a la IP `192.168.57.103` en el archivo `/etc/hosts` para acceso local.
5. Verificar el funcionamiento del sitio accediendo al dominio desde el navegador.

---

# 📝 Requisitos

- Vagrant instalado en la máquina anfitriona.
- VirtualBox u otro proveedor compatible con Vagrant.
- Conexión a internet para descargar dependencias.

# 🗃️ Estructura del Proyecto

```
PRACTICA3/
├── Vagrantfile            # Configuración de Vagrant para la VM con Nginx
├── README.md              # Documentación del proyecto
├── pedro            # Archivo de configuración para Nginx
└── vsftpd.conf            # Archivo de configuración para FTPS

```

---

# 🗂️ Archivos importantes

```
/etc/nginx/sites-available/pedro: Archivo de configuración del sitio en Nginx.
/var/www/pedro/html/: Directorio raíz del sitio web.
/etc/hosts: Archivo donde se asigna el nombre de dominio a la IP de la máquina virtual.
```

Este es el archivo de configuración de Nginx:

```
server {
    listen 80;
    server_name pedro;
    root /var/www/pedro/html;
    index index.html index.htm index.nginx-debian.html;
}
```

Este es el archivo d configuración para hosts:

```
127.0.0.1	localhost
127.0.0.2	bookworm
::1		localhost ip6-localhost ip6-loopback
ff02::1		ip6-allnodes
ff02::2		ip6-allrouters
192.168.57.103 	pedro
```

---

# 📝 Comprobaciones y Pruebas

## ✔️ Verificación del sitio web

Abre un navegador web en tu máquina anfitriona e ingresa http://pedro para acceder al sitio web.
Si la configuración es correcta, deberías ver el contenido del sitio clonado en la máquina virtual.

## ✅ Comprobación de logs

Para verificar los logs de acceso y errores, revisa los siguientes archivos en la máquina virtual:

Log de accesos: `/var/log/nginx/access.log`
Log de errores: `/var/log/nginx/error.log`

## ❌ Solución de Problemas

A continuación, se describen los problemas comunes que podrías encontrar y cómo solucionarlos.

El sitio no carga:

1. Verifica que el archivo de configuración esté en sites-enabled
   El archivo de configuración de tu sitio debe estar enlazado simbólicamente desde sites-available a sites-enabled. Para comprobarlo, lista los archivos en sites-enabled:

```
ls -l /etc/nginx/sites-enabled/
```

Si el archivo pedro no aparece en la lista, crea el enlace simbólico:

```
sudo systemctl reload nginx
```

Después de realizar esto, recarga la configuración de Nginx:

```
sudo systemctl reload nginx
```

2. Verifica que Nginx esté en ejecución
   Comprueba si Nginx está corriendo correctamente:

```
sudo systemctl status nginx
```

Si el servicio no está activo, intenta reiniciarlo:

```
sudo systemctl restart nginx
```

Si encuentras un error, verifica que no haya problemas en el archivo de configuración usando:

```
sudo nginx -t
```

Esto mostrará detalles de errores de sintaxis en los archivos de configuración.

Errores en los permisos

1. Permisos del directorio /var/www/pedro/html
   Asegúrate de que el propietario y los permisos del directorio del sitio sean correctos. Esto puede resolverse con los siguientes comandos:

```
sudo chown -R www-data:www-data /var/www/pedro/html
sudo chmod -R 755 /var/www/pedro/html
```

Propietario: www-data, que es el usuario que Nginx utiliza para acceder a los archivos.
Permisos: 755 asegura que el propietario tiene permisos de lectura, escritura y ejecución, mientras que otros usuarios solo tienen lectura y ejecución.

2. Verifica la existencia del archivo index.html
   Nginx busca un archivo index.html (o uno equivalente) en el directorio /var/www/pedro/html. Si el archivo no existe, crea uno básico para pruebas:

```
echo "<h1>Hello, world!</h1>" > /var/www/pedro/html/index.html
```

Problemas de red en Windows
Si tienes problemas para acceder a la IP 192.168.57.103 o al dominio pedro desde tu navegador en Windows, verifica lo siguiente:

1. Modifica el archivo hosts en tu máquina anfitriona
   Asegúrate de que el dominio pedro esté asignado a la IP 192.168.57.103 en tu archivo hosts. En Windows, este archivo se encuentra en:

   ```
   C:\Windows\System32\drivers\etc\hosts
   ```

   Añade la siguiente línea al final del archivo:

   ```
   192.168.57.103 	pedro
   ```

   Guarda el archivo (necesitarás permisos de administrador) y prueba nuevamente accediendo a http://pedro en tu navegador.

2. Verifica la conexión de red entre la máquina anfitriona y la máquina virtual
   Desde tu máquina anfitriona, intenta hacer ping a la IP de la máquina virtual para comprobar que está accesible:

```
ping 192.168.57.103
```

Si no responde, revisa la configuración de red de la máquina virtual en Vagrant. Asegúrate de que estás usando una red privada con la IP 192.168.57.103 en el Vagrantfile:

```
pedro.vm.network "private_network", ip: "192.168.57.103"
```

3.

Después de realizar cambios, reinicia la máquina virtual:

```
vagrant reload
```

Problemas adicionales
Si todavía tienes problemas después de intentar estas soluciones, revisa los registros de errores para más detalles:

Registro de Nginx:
Errores:

```
/var/log/nginx/error.log
```

Accesos:

```
 /var/log/nginx/access.log
```

Con estos pasos, deberías poder solucionar la mayoría de los problemas relacionados con la configuración del servidor web.

---

# 👤 Autor

Pedro Rodríguez Gallegos
Módulo: Despliegue de Aplicaciones Web
