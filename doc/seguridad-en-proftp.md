# Solución de Problemas en ProFTP

A pesar de una configuración cuidadosa, pueden surgir problemas al ejecutar ProFTP. Esta guía proporciona soluciones a los problemas más comunes y herramientas para diagnosticar errores.

---

## Verificar el Estado del Servicio

El primer paso para solucionar problemas es verificar si el servicio está activo.

1. Compruebe el estado del servicio:
   ```bash
   sudo systemctl status proftpd
   ```

2. Si el servicio no está activo, intente iniciarlo:
   ```bash
   sudo systemctl start proftpd
   ```

3. Verifique el archivo de logs para más información:
   - **Ubicación del archivo de logs principal:** `/var/log/proftpd/proftpd.log`
   ```bash
   sudo tail -f /var/log/proftpd/proftpd.log
   ```

---

## Problema: "Connection Refused"

### Posibles Causas
1. El servicio ProFTP no está en ejecución.
2. El firewall está bloqueando el puerto 21 o los puertos pasivos.

### Soluciones
1. Asegúrese de que el servicio está en ejecución:
   ```bash
   sudo systemctl restart proftpd
   ```

2. Verifique el estado del firewall:
   ```bash
   sudo ufw status
   ```
   Asegúrese de permitir el puerto 21 y los puertos pasivos:
   ```bash
   sudo ufw allow 21/tcp
   sudo ufw allow 49152:65534/tcp
   ```

---

## Problema: "530 Login Incorrect"

### Posibles Causas
1. Credenciales incorrectas.
2. El usuario no tiene permisos adecuados en el directorio FTP.

### Soluciones
1. Verifique el archivo de usuarios virtuales o del sistema para confirmar las credenciales:
   - **Usuarios del sistema:** `/etc/passwd`
   - **Usuarios virtuales:** `/etc/proftpd/ftpd.passwd`

2. Asegúrese de que el usuario tiene los permisos correctos:
   ```bash
   sudo chmod -R 750 /var/ftp
   sudo chown -R ftp:ftp /var/ftp
   ```

---

## Problema: "TLS Not Available"

### Posibles Causas
1. TLS no está configurado correctamente.
2. El certificado TLS no tiene permisos adecuados.

### Soluciones
1. Verifique la configuración de TLS en el archivo de configuración:
   - **Ubicación del archivo de configuración:** `/etc/proftpd/proftpd.conf`
   ```conf
   <IfModule mod_tls.c>
       TLSEngine on
       TLSProtocol TLSv1.2
       TLSRSACertificateFile /etc/ssl/certs/proftpd.crt
       TLSRSACertificateKeyFile /etc/ssl/private/proftpd.key
   </IfModule>
   ```

2. Asegúrese de que los permisos del certificado son correctos:
   - **Certificado:** `/etc/ssl/certs/proftpd.crt`
   - **Clave privada:** `/etc/ssl/private/proftpd.key`
   ```bash
   sudo chmod 600 /etc/ssl/private/proftpd.key
   sudo chmod 644 /etc/ssl/certs/proftpd.crt
   ```

3. Reinicie el servicio:
   ```bash
   sudo systemctl restart proftpd
   ```

---

## Problema: Transferencias Lentas

### Posibles Causas
1. Configuración de velocidad limitada.
2. Congestión de red.

### Soluciones
1. Verifique si existen limitaciones en la configuración de ProFTP:
   - **Ubicación del archivo de configuración:** `/etc/proftpd/proftpd.conf`
   ```conf
   <IfModule mod_ratios.c>
       TransferRate RETR 0
       TransferRate STOR 0
   </IfModule>
   ```
   Establezca los valores en `0` para eliminar las restricciones.

2. Verifique la calidad de la conexión de red con herramientas como `ping` o `traceroute`.

---

## Problema: Usuarios Bloqueados Después de Varios Intentos Fallidos

### Posibles Causas
1. El módulo de límites de inicio de sesión está activado.

### Soluciones
1. Revise el archivo de configuración:
   - **Ubicación del archivo de configuración:** `/etc/proftpd/proftpd.conf`
   ```conf
   <IfModule mod_ban.c>
       BanEngine on
       BanTable /var/run/proftpd/ban.tab
   </IfModule>
   ```

2. Para desbloquear un usuario:
   - **Ubicación de la tabla de baneos:** `/var/run/proftpd/ban.tab`
   ```bash
   sudo rm /var/run/proftpd/ban.tab
   ```

3. Reinicie el servicio:
   ```bash
   sudo systemctl restart proftpd
   ```

---

## Herramientas Útiles para Diagnóstico

1. **Modo Debug de ProFTP:**
   Ejecute ProFTP en modo debug para analizar errores detalladamente:
   ```bash
   sudo proftpd -nd
   ```

2. **Comando `ftp`:**
   Pruebe la conexión al servidor desde la misma máquina:
   ```bash
   ftp localhost
   ```

3. **Logs de ProFTP:**
   Revise los logs de autenticación y acceso para identificar problemas:
   - **Ubicación de los logs:** `/var/log/proftpd/`
   ```bash
   sudo tail -f /var/log/proftpd/*.log
   ```

---

Siguiendo estas soluciones y utilizando las herramientas de diagnóstico mencionadas, puede identificar y resolver la mayoría de los problemas comunes en ProFTP.
