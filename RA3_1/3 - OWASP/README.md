# Apache Security Hardening - Práctica 3: OWASP

Este documento detalla la configuración y uso de las **Reglas Núcleo de OWASP (OWASP Core Rule Set - CRS)** en ModSecurity para proteger aplicaciones web contra vulnerabilidades comunes como **inyección SQL (SQLi)**, **Cross-Site Scripting (XSS)** y **ejecución remota de código (RCE)**.

## Cambios realizados

### 1. Instalación y Configuración de OWASP CRS

Las **OWASP Core Rule Set (CRS)** son un conjunto de reglas de ModSecurity diseñadas para detectar y prevenir ataques web. Para instalarlas en Apache:

1. Descargar y extraer las reglas de OWASP CRS:
   ```bash
   cd /etc/modsecurity/
   git clone https://github.com/coreruleset/coreruleset.git
   mv coreruleset /etc/modsecurity/crs
   ```

2. Copiar el archivo de configuración predeterminado:
   ```bash
   cp /etc/modsecurity/crs/crs-setup.conf.example /etc/modsecurity/crs/crs-setup.conf
   ```

3. Incluir las reglas en la configuración de ModSecurity (`/etc/apache2/mods-enabled/security2.conf`):
   ```apache
   IncludeOptional /etc/modsecurity/crs/crs-setup.conf
   IncludeOptional /etc/modsecurity/crs/rules/*.conf
   ```

4. Reiniciar Apache para aplicar los cambios:
   ```bash
   systemctl restart apache2
   ```

---

### 2. Configuración en VirtualHost

Para asegurarnos de que ModSecurity con OWASP CRS está activo en los sitios configurados en Apache, agregamos lo siguiente en la configuración del VirtualHost (`/etc/apache2/sites-available/000-default.conf`):

```apache
<IfModule security2_module>
    SecRuleEngine On
    SecDefaultAction "phase:1,log,auditlog,pass"
</IfModule>
```

Esto habilita ModSecurity con CRS en modo **detección y auditoría**, registrando eventos en los logs sin bloquear el tráfico inmediatamente.

---

### 3. Prueba de Configuración: Simulación de Ataques SQLi y XSS

Para verificar el funcionamiento de las reglas de OWASP CRS, realizamos pruebas enviando peticiones maliciosas al servidor.

#### Prueba de inyección SQL (SQLi):

Enviamos una consulta SQL maliciosa a la URL `https://www.midominioseguro.com/login.php`:

```bash
curl -X POST -d "username=admin' OR '1'='1" https://www.midominioseguro.com/login.php
```

**Resultado esperado:** Bloqueo de la solicitud con un código **403 Forbidden**.

#### Prueba de ataque XSS:

```bash
curl -X POST -d "<script>alert('XSS')</script>" https://www.midominioseguro.com/post.php
```

**Resultado esperado:** Bloqueo del intento de inyección de script.

---

### 4. Análisis de Logs

Para verificar los intentos de ataque detectados por OWASP CRS, podemos revisar los registros de ModSecurity:

```bash
cat /var/log/apache2/modsec_audit.log | grep "Warning"
```

Esto mostrará las solicitudes bloqueadas y los detalles de cada ataque detectado.

---

## Conclusión

La integración de las **OWASP Core Rule Set** en ModSecurity añade una capa avanzada de protección contra ataques web, permitiendo detectar y bloquear amenazas como SQLi y XSS. Esta práctica demuestra cómo instalar, configurar y probar estas reglas en un entorno Apache.

## Autor
Configuración realizada por [Tu Nombre].

