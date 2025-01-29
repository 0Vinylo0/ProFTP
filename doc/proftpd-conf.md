# Guía de Configuración de ProFTPD

## 📌 Ubicación del Archivo de Configuración
El archivo de configuración de ProFTPD se encuentra en:

```bash
/etc/proftpd/proftpd.conf
```

## 📌 Parámetros Globales

### **1. Configurar el Nombre del Servidor**
```apache
ServerName "Servidor FTP"
```

### **2. Puerto de Escucha**
```apache
Port 21
```

### **3. Modo Activo vs Pasivo**
```apache
PassivePorts 49152 65534
```

### **4. Usuarios y Autenticación**
```apache
AuthOrder mod_auth_unix.c
RequireValidShell off  # Permitir usuarios sin shell válido
```

### **5. Tiempo de Inactividad**
```apache
TimeoutIdle 600  # Tiempo de desconexión por inactividad
TimeoutLogin 120  # Tiempo de espera en la autenticación
TimeoutNoTransfer 300  # Tiempo de espera sin transferencias
```

## 📌 Restricciones de Usuarios

### **1. Restringir Usuarios a su Directorio Home**
```apache
DefaultRoot ~
```

### **2. Permitir Solo Ciertos Usuarios**
```apache
AllowUser usuario1 usuario2
DenyUser root
```

### **3. Restringir Acceso a un Directorio**
```apache
<Directory /home/usuarioftp>
  <Limit READ WRITE>
    DenyAll
  </Limit>
</Directory>
```

## 📌 Configuración de Seguridad

### **1. Deshabilitar Acceso Anónimo**
```apache
<Anonymous ~ftp>
  User ftp
  Group nogroup
  UserAlias anonymous ftp
  RequireValidShell off
  <Limit WRITE>
    DenyAll
  </Limit>
</Anonymous>
```

### **2. Bloquear Usuarios Root y del Sistema**
```apache
DenyUser root bin daemon
```

## 📌 Configuración Avanzada

### **1. Usuarios Virtuales (sin cuenta en el sistema)**
```apache
AuthUserFile /etc/proftpd/ftpd.passwd
AuthOrder mod_auth_file.c
```

### **2. Configuración de un Buzón FTP (Solo Subir Archivos)**
```apache
<Directory /var/ftp/buzon>
  <Limit READ>
    DenyUser usuarioftp
  </Limit>
  <Limit STOR>
    AllowUser usuarioftp
  </Limit>
  <Limit RETR>
    DenyUser usuarioftp
  </Limit>
</Directory>
```
#### 📌 **Explicación de la Configuración**
Este bloque configura un **buzón FTP**, permitiendo que el usuario `usuarioftp` **solo pueda subir archivos** sin ver ni descargar los archivos existentes.

- **`<Limit READ>`** → Bloquea la lectura del directorio, por lo que `usuarioftp` **no podrá listar (`ls`) los archivos**.
- **`<Limit STOR>`** → Permite a `usuarioftp` **subir archivos** (`put archivo.txt`).
- **`<Limit RETR>`** → Evita que `usuarioftp` **descargue archivos** (`get archivo.txt`).

🔹 **Resultado:** `usuarioftp` puede enviar archivos, pero no ver ni descargar lo que ya está en el buzón.

## 📌 Reiniciar ProFTPD Después de Cambios

```bash
sudo systemctl restart proftpd
```

## 📌 Verificar el Estado del Servicio
```bash
sudo systemctl status proftpd
```

## 📌 Probar Conexión FTP
```bash
ftp <IP_DEL_SERVIDOR>
```
