# Apache Security Hardening - Práctica 3: OWASP

Este documento detalla la instalación y configuración de las **Reglas Núcleo de OWASP (OWASP Core Rule Set - CRS)** en ModSecurity para fortalecer la seguridad en aplicaciones web y mitigar ataques como **inyección SQL (SQLi)**, **Cross-Site Scripting (XSS)** y **ejecución remota de código (RCE)**.

## Instalación de OWASP CRS

Para mejorar la seguridad del servidor web Apache, instalaremos y configuraremos el **OWASP Core Rule Set (CRS)**, un conjunto de reglas predefinidas que protegen contra ataques web comunes.

1. **Instalar ModSecurity**:
   ```bash
   sudo apt install libapache2-mod-security2 -y
   ```

2. **Clonar el repositorio oficial de OWASP CRS**:
   ```bash
   git clone https://github.com/SpiderLabs/owasp-modsecurity-crs.git
   ```

3. **Mover el archivo de configuración**:
   ```bash
   cd owasp-modsecurity-crs
   sudo mv crs-setup.conf.example /etc/modsecurity/crs-setup.conf
   ```

4. **Mover las reglas al directorio de ModSecurity**:
   ```bash
   sudo mv rules/ /etc/modsecurity/
   ```
   Si hay errores, crear manualmente el directorio y copiar las reglas:
   ```bash
   sudo mkdir /etc/modsecurity/rules
   cd rules
   sudo cp *.* /etc/modsecurity/rules
   ```

5. **Verificar la configuración en Apache**:
   ```bash
   sudo nano /etc/apache2/mods-enabled/security2.conf
   ```
   Asegurar que se incluyan las reglas:
   ```apache
   <IfModule security2_module>
       SecDataDir /var/cache/modsecurity
       SecRuleEngine On
       IncludeOptional /etc/modsecurity/*.conf
       Include /etc/modsecurity/rules/*.conf
   </IfModule>
   ```


   **Captura de pantalla:**

   ![Configuración de security2.conf](assets/1%20-%20security2.conf.png)

6. **Reiniciar Apache para aplicar los cambios**:
   ```bash
   sudo systemctl restart apache2
   ```

---

## Configuración en VirtualHost

Para garantizar que ModSecurity con OWASP CRS esté activo en los sitios configurados en Apache, agregamos lo siguiente en el archivo del VirtualHost (`/etc/apache2/sites-available/000-default.conf`):

```apache
<IfModule security2_module>
    SecRuleEngine On
    SecRule ARGS:testparam "@contains test" "id:1234,deny,status:403,msg:'Cazado por Ciberseguridad'"
</IfModule>
```

Esto permitirá bloquear peticiones que contengan el parámetro `testparam=test`, simulando una regla personalizada de detección de ataques.

**Captura de pantalla:**
![Habilitación en VirtualHost](assets/2%20-%20Enable%20in%20000-default.conf.png)

---

## Prueba de inyección de comandos (Command Injection)

Para verificar el funcionamiento de OWASP CRS, realizamos pruebas enviando peticiones maliciosas al servidor.

```bash
curl -k "https://www.midominioseguro.com/?exec=/bin/bash"
```

![Prueba de seguridad](assets/3%20-%20Test.png)



**Resultado esperado:** Bloqueo con mensaje **403 Forbidden**.
