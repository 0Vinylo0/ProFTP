# ProFTP

ProFTP es un servidor FTP de código abierto, ampliamente reconocido por su seguridad, flexibilidad y rendimiento. Este servidor permite la transferencia de archivos mediante el protocolo FTP y se utiliza tanto en entornos personales como empresariales.

## Índice

<img src="/image/pngegg.png" alt="GIF" width="200" height="175" align="right">

1. [`Introducción`](#introducción)
2. [`Instalación de ProFTP`](/doc/instalación-de-proftp.md)
3. [`Configuración Inicial`](/doc/configuración-inicial.md)
4. [`Configuraciones de Proftpd`](/doc/proftpd-conf.md)
5. [`Opciones Avanzadas de Configuración`](/doc/opciones-avanzadas-de-configuración.md)
6. [`Solución de Problemas`](/doc/solución-de-problemas.md)
7. [`Referencias`](#referencias)

---

## Introducción

El protocolo FTP (File Transfer Protocol) es un estándar de red utilizado para transferir archivos entre un cliente y un servidor a través de una red basada en TCP/IP. Es uno de los protocolos más antiguos utilizados en internet, diseñado inicialmente en los años 70. FTP opera en dos modos principales:

1. **Modo Activo:** El cliente se conecta al servidor y el servidor establece una conexión de retorno al cliente para la transferencia de datos.
2. **Modo Pasivo:** El cliente inicia tanto la conexión de control como la de datos, siendo más adecuado para superar restricciones de firewalls.

### Características del Protocolo FTP

- **Protocolo de Transporte:** Utiliza TCP (Transmission Control Protocol) como protocolo de transporte para garantizar la entrega confiable de datos.
- **Puertos Utilizados:**
  - **Puerto 21:** Utilizado para la conexión de control, donde se envían comandos y respuestas.
  - **Puerto 20:** Utilizado para la transferencia de datos en el modo activo.
- **Autenticación:** Permite el uso de credenciales (usuario y contraseña) y, en algunos casos, acceso anónimo.
- **Estructura de Datos:** Admite transferencias de archivos en modo binario y ASCII.

FTP, aunque funcional, tiene ciertas limitaciones de seguridad en su forma básica, ya que las credenciales y los datos se transmiten sin cifrado. Por ello, se recomienda usar extensiones como FTPS (FTP sobre SSL/TLS) o reemplazarlo con protocolos más modernos como SFTP (SSH File Transfer Protocol) cuando la seguridad sea una prioridad.

## Referencias

- [`Documentación Oficial de ProFTP`](http://www.proftpd.org/)
- [`RFC 959 - Protocolo FTP`](https://www.rfc-editor.org/rfc/rfc959)
- [`Introducción al protocolo FTP`](https://www.lifewire.com/file-transfer-protocol-overview-817944)
- [`Seguridad en FTP - Guía de FTPS y SFTP`](https://www.ssl.com/article/ftps-vs-sftp-understanding-the-differences/)


