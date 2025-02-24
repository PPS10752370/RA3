# Apache Security Hardening - Práctica 2: Web Application Firewall

Este documento detalla la configuración de **ModSecurity**, un Web Application Firewall (WAF) para Apache, con el objetivo de mitigar ataques web comunes, como **inyección de código** y **cross-site scripting (XSS)**.

## Cambios realizados

### 1. Habilitación de ModSecurity
![Configuración de ModSecurity](assets/1%20-%20modsecurity.conf.png)

Para activar ModSecurity en Apache, se modificó el archivo de configuración principal `/etc/modsecurity/modsecurity.conf` para asegurarse de que el motor de reglas está habilitado.

```bash
SecRuleEngine On
```

---

### 2. Configuración en VirtualHost
![Habilitación en VirtualHost](assets/2%20-%20Enable%20in%20000-default.conf.png)

Se añadió la configuración de ModSecurity en el archivo de configuración del sitio en Apache (`/etc/apache2/sites-available/000-default.conf`).

```apache
<IfModule security2_module>
    SecRuleEngine On
</IfModule>
```

Esto permite que ModSecurity monitoree y filtre las solicitudes en el servidor.

---

### 3. Prueba de Configuración: Bloqueo de Ataques XSS
**Captura de pantalla del intento de ataque:**
![Prueba de XSS](assets/3%20-%20Test.png)

**Captura de pantalla del bloqueo:**
![Respuesta 403 Forbidden](assets/4%20-%20Test.png)

Se realizó una prueba enviando una inyección de código JavaScript en un formulario web en `https://www.midominioseguro.com/post.php`.

Entrada maliciosa simulada:
```html
<script>alert(1)</script>
```

Al enviar esta solicitud, el servidor respondió con un **error 403 Forbidden**, lo que indica que ModSecurity bloqueó el intento de ataque.

---
