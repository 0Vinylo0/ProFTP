# Configuración Avanzada de ProFTP

La configuración avanzada de ProFTP permite optimizar el servidor, mejorar la seguridad y personalizar las características según las necesidades específicas del entorno. En esta guía, se describen algunas configuraciones avanzadas comunes.

---

## Configuración de Modo Pasivo
El modo pasivo es útil para clientes detrás de firewalls o NAT. Debe definir un rango de puertos y configurarlo en el archivo `proftpd.conf`.

1. Edite el archivo de configuración:
   ```bash
   sudo nano /etc/proftpd/proftpd.conf
   ```

2. Añada o modifique las siguientes líneas:
   ```conf
   PassivePorts 49152 65534
   ```

3. Asegúrese de abrir este rango de puertos en el firewall:
   ```bash
   sudo ufw allow 49152:65534/tcp
   ```

---

## Creación de Usuarios Virtuales
Los usuarios virtuales permiten gestionar accesos sin crear cuentas en el sistema operativo.

1. Cree el archivo de contraseñas virtuales:
   ```bash
   ftpasswd --passwd --name=usuarioftp --uid=1001 --gid=1001 --home=/var/ftp/usuarioftp --shell=/bin/false
   ```

2. Configure ProFTP para usar el archivo de usuarios virtuales:
   ```conf
   AuthUserFile /etc/proftpd/ftpd.passwd
   AuthGroupFile /etc/proftpd/ftpd.group
   ```

3. Reinicie el servicio para aplicar los cambios:
   ```bash
   sudo systemctl restart proftpd
   ```

---

## Configuración de TLS/SSL
Para mejorar la seguridad, habilite TLS/SSL y cifre las conexiones.

1. Genere un certificado TLS:
   ```bash
   openssl req -new -x509 -days 365 -nodes -out /etc/ssl/certs/proftpd.crt -keyout /etc/ssl/private/proftpd.key
   ```

2. Edite el archivo de configuración:
   ```bash
   sudo nano /etc/proftpd/proftpd.conf
   ```

3. Añada la configuración para TLS:
   ```conf
   <IfModule mod_tls.c>
       TLSEngine on
       TLSLog /var/log/proftpd/tls.log
       TLSProtocol TLSv1.2
       TLSRSACertificateFile /etc/ssl/certs/proftpd.crt
       TLSRSACertificateKeyFile /etc/ssl/private/proftpd.key
       TLSRequired on
   </IfModule>
   ```

4. Reinicie el servicio:
   ```bash
   sudo systemctl restart proftpd
   ```

---

## Limitación de Velocidad de Transferencia
Puede limitar la velocidad de subida y bajada para optimizar el uso del ancho de banda.

1. Añada las siguientes líneas al archivo de configuración:
   ```conf
   <IfModule mod_ratios.c>
       Ratios on
       UploadRatio 1 10
       DownloadRatio 1 10
       TransferRate RETR 50
       TransferRate STOR 20
   </IfModule>
   ```

2. Reinicie el servicio:
   ```bash
   sudo systemctl restart proftpd
   ```

---

## Configuración de Logs Detallados
Los registros detallados ayudan a monitorear y diagnosticar problemas en el servidor.

1. Configure la ubicación de los logs:
   ```conf
   ExtendedLog /var/log/proftpd/access.log READ,WRITE
   ExtendedLog /var/log/proftpd/auth.log AUTH
   ExtendedLog /var/log/proftpd/tls.log ALL
   ```

2. Asegúrese de que los archivos de log tengan los permisos correctos:
   ```bash
   sudo chmod 640 /var/log/proftpd/*
   ```

3. Reinicie el servicio:
   ```bash
   sudo systemctl restart proftpd
   ```

---

Estas configuraciones avanzadas le permiten aprovechar al máximo ProFTP. Puede combinar varias de estas opciones para personalizar su entorno según las necesidades específicas.

