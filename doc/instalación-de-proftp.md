# Instalación de ProFTP

ProFTP puede instalarse fácilmente en la mayoría de las distribuciones de Linux mediante los repositorios predeterminados o compilándolo desde el código fuente. A continuación, se describen ambos métodos.

---

## Instalación desde los Repositorios

### En sistemas basados en Debian/Ubuntu

1. Actualice los paquetes disponibles:
   ```bash
   sudo apt update
   ```
2. Instale ProFTP:
   ```bash
   sudo apt install proftpd
   ```
3. Verifique que el servicio esté activo:
   ```bash
   sudo systemctl status proftpd
   ```

### En sistemas basados en CentOS/Red Hat

1. Habilite el repositorio EPEL (si aún no está habilitado):
   ```bash
   sudo dnf install epel-release
   ```
2. Instale ProFTP:
   ```bash
   sudo dnf install proftpd
   ```
3. Inicie y habilite el servicio:
   ```bash
   sudo systemctl start proftpd
   sudo systemctl enable proftpd
   ```

---

## Instalación desde el Código Fuente

Si necesita una versión específica o características personalizadas, puede compilar ProFTP desde su código fuente.

1. Descargue el código fuente:
   ```bash
   wget ftp://ftp.proftpd.org/distrib/source/proftpd-X.X.X.tar.gz
   ```
2. Extraiga el archivo:
   ```bash
   tar -xvzf proftpd-X.X.X.tar.gz
   cd proftpd-X.X.X
   ```
3. Configure y compile el paquete:
   ```bash
   ./configure --prefix=/usr/local/proftpd
   make
   sudo make install
   ```
4. Verifique la instalación:
   ```bash
   /usr/local/proftpd/sbin/proftpd -v
   ```

