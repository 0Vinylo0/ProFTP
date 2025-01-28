# Configuración Básica de ProFTP

Una vez que ProFTP está instalado, es necesario configurarlo para que funcione de acuerdo con los requisitos específicos del sistema. El archivo principal de configuración es `proftpd.conf`, que generalmente se encuentra en:

- **Debian/Ubuntu:** `/etc/proftpd/proftpd.conf`
- **CentOS/Red Hat:** `/etc/proftpd.conf`

A continuación, se describen los pasos para realizar una configuración básica.

---

## Configuración del Archivo principal

### Abrir el Archivo de Configuración
Utilice un editor de texto como `nano` o `vim` para editar el archivo:
```bash
sudo nano /etc/proftpd/proftpd.conf
```

### Establecer el Tipo de Servidor
ProFTP puede operar en dos modos: `standalone` (servidor independiente) o `inetd` (iniciado por el superdaemon).

Por defecto, el modo `standalone` es más común:
```conf
ServerType standalone
```

### Configurar el Puerto
El puerto predeterminado para FTP es el 21. Puede configurarlo si necesita un puerto diferente:
```conf
Port 21
```

### Definir el Directorio Raíz del Usuario
Asegúrese de que los usuarios no puedan navegar fuera de sus directorios principales:
```conf
DefaultRoot ~
```

### Registrar las Sesiones
Para mantener registros de las conexiones, configure la ubicación del archivo de log:
```conf
SystemLog /var/log/proftpd/proftpd.log
```

### Permitir o Denegar Acceso Anónimo
Si desea habilitar el acceso anónimo:
```conf
<Anonymous /var/ftp>
   User ftp
   Group nogroup
   <Limit WRITE>
       DenyAll
   </Limit>
</Anonymous>
```
Si no desea acceso anónimo, asegúrese de deshabilitarlo eliminando o comentando esta sección.

---

## Gestión del Servicio

### Reiniciar el Servicio
Después de realizar cambios en el archivo de configuración, es necesario reiniciar el servicio para aplicar los cambios:
```bash
sudo systemctl restart proftpd
```

### Habilitar el Servicio al Iniciar el Sistema
Para garantizar que el servidor ProFTP se inicie automáticamente después de un reinicio:
```bash
sudo systemctl enable proftpd
```

### Verificar el Estado del Servicio
Puede verificar si el servicio está funcionando correctamente:
```bash
sudo systemctl status proftpd
```

---

## Probar la Conexión FTP
Utilice cualquier cliente FTP para conectarse al servidor y verificar que funciona correctamente. Por ejemplo, en la terminal:
```bash
ftp localhost
```
Ingrese las credenciales para confirmar que el servidor está operativo.

