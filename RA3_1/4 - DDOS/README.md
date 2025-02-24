# Apache Security Hardening - Práctica 4: Evitar ataques DDoS

En esta práctica se configura **mod_evasive**, un módulo de Apache diseñado para detectar y mitigar ataques de Denegación de Servicio Distribuido (**DDoS**) mediante la restricción de accesos excesivos desde una misma IP en un corto período de tiempo.

## Instalación y Configuración de mod_evasive

1. **Instalar mod_evasive**:
   
   ```bash
   sudo apt install libapache2-mod-evasive -y
   ```

3. **Configurar el módulo**:
   Editar el archivo de configuración de `mod_evasive` en `/etc/apache2/mods-available/evasive.conf`:
   
   ```apache
   <IfModule mod_evasive20.c>
       DOSHashTableSize 3097
       DOSPageCount 3
       DOSSiteCount 10
       DOSPageInterval 1
       DOSSiteInterval 1
       DOSBlockingPeriod 300
       DOSLogDir "/var/log/mod_evasive"
       DOSEmailNotify admin@midominio.com
       DOSWhitelist 127.0.0.1
   </IfModule>
   ```

4. **Crear el directorio de logs y asignar permisos adecuados**:
   
   ```bash
   sudo mkdir -p /var/log/mod_evasive
   sudo chmod 777 /var/log/mod_evasive
   ```

5. **Habilitar el módulo y reiniciar Apache**:
   
   ```bash
   sudo a2enmod evasive
   sudo systemctl restart apache2
   ```

**Captura de pantalla de la configuración:**

![Configuración de mod_evasive](assets/1%20-%20evasive.png)

---

## Prueba de Ataque DDoS Simulado

Para verificar la efectividad de `mod_evasive`, se utilizó **ApacheBench (ab)** para simular múltiples peticiones concurrentes:

```bash
ab -n 1000 -c 50 https://www.midominioseguro.com/
```

**Resultados esperados:**
- La mayoría de las solicitudes fallarán con un código **403 Forbidden**.
- Se observará un número elevado de peticiones bloqueadas en los registros.

**Captura de pantalla del ataque simulado:**

![Prueba de resistencia](assets/2%20-%20Test.png)

---

## Repositorio de Docker

Se ha configurado una imagen de Docker con todas estas configuraciones aplicadas. Puedes encontrarla en el siguiente enlace:

[Repositorio Docker - Apache Hardened:v4](https://hub.docker.com/layers/pps10752370/apache-hardened/v4/images/sha256-7f7f1d46216e5f7d2110b8110dee3f787f29cb845e75d1be8867e0960a2890ef)
