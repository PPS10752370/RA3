# Apache Security Hardening - Práctica 1 : HSTS, Certificados Autofirmados y CSP.

Este documento detalla los cambios realizados para mejorar la seguridad en un servidor Apache, incluyendo la desactivación del listado de directorios, la implementación de Content Security Policy (CSP), la configuración de VirtualHost con HTTPS y la verificación de las configuraciones aplicadas.

## Cambios realizados

### 1. Deshabilitación de AutoIndex
![Deshabilitación de AutoIndex](assets/1%20-%20Disable%20Autoindex.png)

Se ha deshabilitado el módulo `autoindex` para evitar la exposición accidental de archivos en directorios sin un `index.html`.

```bash
a2dismod autoindex
```

---

### 2. Configuración de Content Security Policy (CSP)
![Configuración CSP](assets/2%20-%20CSP.png)

Se ha añadido una política CSP en la configuración de Apache para restringir el origen de los recursos cargados en la página.

```bash
<IfModule mod_headers.c>
    Header set Content-Security-Policy "default-src 'self'; script-src 'self'"
</IfModule>
```

---

### 3. Configuración de VirtualHost
![Configuración VirtualHost](assets/3%20-%20000-default.conf.png)

Se han configurado los virtual hosts en Apache para redirigir el tráfico HTTP a HTTPS y definir la configuración SSL.

Archivo: `sites-available/000-default.conf`

```bash
<VirtualHost *:80>
    ServerName www.midominioseguro.com
    Redirect / https://www.midominioseguro.com/
</VirtualHost>

<VirtualHost _default_:443>
    ServerAdmin admin@midominioseguro.com
    ServerName www.midominioseguro.com
    DocumentRoot /var/www/html
    SSLEngine on
    SSLCertificateFile /etc/apache2/ssl/apache.crt
    SSLCertificateKeyFile /etc/apache2/ssl/apache.key
    Header always set Strict-Transport-Security "max-age=63072000; includeSubDomains"
</VirtualHost>
```

---

### 4. Verificación de Configuración
![Prueba de configuración](assets/4%20-%20Test.png)

Se ha utilizado `curl` para verificar los encabezados HTTP y confirmar que las políticas se están aplicando correctamente.

```bash
curl -I https://localhost --insecure
```

---

## Repositorio de Docker

Se ha configurado una imagen de Docker con todas estas configuraciones aplicadas. Puedes encontrarla en el siguiente enlace:

[Repositorio Docker - Apache Hardened:v2](https://hub.docker.com/layers/pps10752370/apache-hardened/v1/images/sha256-3160ffed4e77ecdc547e342f2d027efae57204579c5ddf404820706ed7eb98a3)

---
