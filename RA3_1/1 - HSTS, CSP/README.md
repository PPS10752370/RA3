# Apache Security Hardening - Práctica 1 : HSTS, Certificados Autofirmados y CSP.

## Cambios realizados

### 1. Deshabilitación de AutoIndex
![Deshabilitación de AutoIndex](assets/1%20-%20Disable%20Autoindex.png)
Se ha deshabilitado el módulo `autoindex` para evitar la exposición accidental de archivos en directorios sin un `index.html`.

```bash
a2dismod autoindex
```

### 2. Configuración de Content Security Policy (CSP)
![Configuración CSP](assets/2%20-%20CSP.png)
Se ha añadido una política CSP en la configuración de Apache para restringir el origen de los recursos cargados en la página.

```bash
<IfModule mod_headers.c>
    Header set Content-Security-Policy "default-src 'self'; script-src 'self'"
</IfModule>
```

![Configuración VirtualHost](assets/3%20-%20000-default.conf.png)

![Prueba de configuración](assets/4%20-%20Test.png)
