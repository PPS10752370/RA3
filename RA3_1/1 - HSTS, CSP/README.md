# Apache Security Hardening - Práctica 1 : HSTS, Certificados Autofirmados y CSP.

## Cambios realizados

### 1. Deshabilitación de AutoIndex
Se ha deshabilitado el módulo `autoindex` para evitar la exposición accidental de archivos en directorios sin un `index.html`.

```bash
a2dismod autoindex



![Deshabilitación de AutoIndex](assets/1%20-%20Disable%20Autoindex.png)

![Configuración CSP](assets/2%20-%20CSP.png)

![Configuración VirtualHost](assets/3%20-%20000-default.conf.png)

![Prueba de configuración](assets/4%20-%20Test.png)
