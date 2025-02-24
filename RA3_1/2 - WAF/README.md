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

Se añadió la configuración de ModSecurity en el archivo de configuración del sitio en Apache (`/etc/apache2/sites-available/000-default.conf`).

```apache
<IfModule security2_module>
    SecRuleEngine On
</IfModule>
```

Esto permite que ModSecurity monitoree y filtre las solicitudes en el servidor.

**Captura de pantalla:**
![Habilitación en VirtualHost](assets/2%20-%20Enable%20in%20000-default.conf.png)

---

### 3. Prueba de Configuración: Bloqueo de Ataques XSS

Se realizó una prueba enviando una inyección de código JavaScript en un formulario web en `https://www.midominioseguro.com/post.php`.

Entrada maliciosa simulada:
```html
<script>alert(1)</script>
```

Al enviar esta solicitud, el servidor respondió con un **error 403 Forbidden**, lo que indica que ModSecurity bloqueó el intento de ataque.

**Captura de pantalla del intento de ataque:**
![Prueba de XSS](assets/3%20-%20Test.png)

**Captura de pantalla del bloqueo:**
![Respuesta 403 Forbidden](assets/4%20-%20Test.png)

---

## Instalación y Uso

1. Instalar ModSecurity en Apache:
   ```bash
   apt install libapache2-mod-security2 -y
   ```

2. Habilitar el módulo y reiniciar Apache:
   ```bash
   a2enmod security2
   systemctl restart apache2
   ```

3. Verificar que ModSecurity esté activo con:
   ```bash
   apachectl -M | grep security2
   ```

4. Realizar pruebas enviando peticiones maliciosas y asegurarse de que se bloquean.

## Conclusión

La implementación de ModSecurity en Apache permite mitigar ataques web como XSS e inyecciones de código. En esta práctica, se ha demostrado cómo configurar y verificar su funcionamiento en un entorno real.

## Autor
Configuración realizada por [Tu Nombre].

