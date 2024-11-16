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

# 🗂️ Estructura del Proyecto

```
PRACTICA3/
├── Vagrantfile            # Configuración de Vagrant para la VM con Nginx
├── README.md              # Documentación del proyecto
├── nginx_pedro            # Archivo de configuración para Nginx
└── vsftpd.conf            # Archivo de configuración para FTPS

```

---

# 🗃️ Archivos importantes

```
/etc/nginx/sites-available/pedro: Archivo de configuración del sitio en Nginx.
/var/www/pedro/html/: Directorio raíz del sitio web.
/etc/hosts: Archivo donde se asigna el nombre de dominio a la IP de la máquina virtual.
```

---

# 📝 Comprobaciones y Pruebas

## ✔️ Verificación del sitio web

Abre un navegador web en tu máquina anfitriona e ingresa http://pedro para acceder al sitio web.
Si la configuración es correcta, deberías ver el contenido del sitio clonado en la máquina virtual.

## ✅ Comprobación de logs

Para verificar los logs de acceso y errores, revisa los siguientes archivos:

Log de accesos: /var/log/nginx/access.log
Log de errores: /var/log/nginx/error.log

## ❌ Solución de Problemas

El sitio no carga:

Verifica que el archivo de configuración esté en sites-enabled.
Asegúrate de que Nginx esté en ejecución:

```
sudo systemctl status nginx.
```

Errores en los permisos:

Asegúrate de que los permisos en /var/www/pedro/html sean correctos (propietario www-data y permisos 755).
Problemas de red en Windows:

---

# 👤 Autor

Pedro Rodríguez Gallegos
Módulo: Despliegue de Aplicaciones Web
