# Práctica: Configuración de correo electrónico y servidor Poste.io

## Introducción

En esta práctica se ha trabajado con distintos sistemas de correo electrónico,
abarcando tanto la configuración de cuentas y clientes gráficos como el despliegue
y administración de un servidor de correo completo.

El objetivo principal es comprender el funcionamiento del correo electrónico,
sus protocolos (SMTP, IMAP, POP3), el uso de contenedores Docker y la aplicación
de cifrado SSL/TLS en los servicios de correo.



## 1. Redirección de correo y configuración de cliente gráfico

### 1.1 Redirección de correo institucional a Gmail

Se ha configurado una redirección automática de los correos recibidos en la cuenta
institucional `@ieszaidinvergeles.org` hacia una cuenta personal de Gmail.

Además, se ha creado un filtro en Gmail que permite identificar y etiquetar
automáticamente los correos procedentes del instituto.

Para comprobar el correcto funcionamiento de la redirección, se ha enviado un
correo de prueba a la cuenta institucional, verificando su correcta recepción
en Gmail y la aplicación de la etiqueta correspondiente.

**Evidencias:**
- [Configuración de la redirección](docs/evidences/1-redireccion%20y%20cliente/01-redireccion-gmail/configuracion.png)
- [Correo de prueba recibido](docs/evidences/1-redireccion%20y%20cliente/01-redireccion-gmail/correo-prueba.png)
- [Etiqueta aplicada en Gmail](docs/evidences/1-redireccion%20y%20cliente/01-redireccion-gmail/etiqueta.png)



### 1.2 Configuración de cliente de correo de escritorio

Se ha creado una segunda cuenta de correo electrónico utilizando el proveedor
Outlook (`@outlook.com`).

Posteriormente, se ha instalado y configurado un cliente de correo de escritorio,
definiendo correctamente los siguientes protocolos:

- Correo entrante: IMAP / POP3
- Correo saliente: SMTP

Durante la configuración se han especificado los servidores, puertos,
autenticación y cifrado correspondientes.

Se han realizado pruebas de envío y recepción de correo para comprobar el correcto
funcionamiento del cliente.

**Evidencias:**
- [Configuración del servidor entrante](docs/evidences/1-redireccion%20y%20cliente/02-cliente-correo/servidor-entrante.png)
- [Configuración del servidor saliente](docs/evidences/1-redireccion%20y%20cliente/02-cliente-correo/servidor-saliente.png)
- [Prueba de envío y recepción](docs/evidences/1-redireccion%20y%20cliente/02-cliente-correo/envio-recepcion-correo.png)

## 2. Servidor de correo todo-en-uno con Poste.io

### 2.1 Despliegue del servidor Poste.io

Se ha desplegado un servidor de correo completo utilizando Poste.io dentro de un
contenedor Docker, integrando los servicios de SMTP, IMAP, POP3, antispam,
antivirus, panel de administración web y cliente web de correo.

El despliegue y la configuración del contenedor se han realizado mediante comandos
Docker, quedando este proceso implícito en la documentación. Las evidencias
gráficas se centran exclusivamente en verificar el correcto funcionamiento del
sistema una vez desplegado.

**Evidencias:**
- [Contenedor Docker en ejecución](docs/evidences/2-poste.io/01-poste.io-despliegue/contenedor-activo.png)
- [Panel de administración de Poste.io](docs/evidences/2-poste.io/01-poste.io-despliegue/dashboard.png)



### 2.2 Ejecución en máquina virtual (justificación técnica)

El contenedor de Poste.io presenta problemas de compatibilidad en sistemas Mac con
arquitectura ARM (Apple Silicon).

Para solventar esta limitación, el contenedor se ha ejecutado dentro de una máquina
virtual Linux con arquitectura x86_64, donde Poste.io funciona de forma estable y
correcta.



### 2.3 Creación de usuario y cuota de almacenamiento

Desde el panel de administración de Poste.io se ha creado el siguiente usuario:

- Usuario: `mario@sri.test`
- Cuota de almacenamiento: 100 MB

**Evidencias:**
- [Usuario creado con cuota de 100 MB](docs/evidences/2-poste.io/02-usuarios/cuota%20mario@sri.test.png)



### 2.4 Generación y verificación del cifrado SSL/TLS

Para proteger las comunicaciones de los servicios de correo, se ha generado un
certificado SSL/TLS autofirmado mediante la herramienta openssl utilizando el
siguiente comando:

```bash
sudo openssl req -x509 -nodes -days 365 \
  -newkey rsa:2048 \
  -keyout /var/lib/docker/volumes/poste_data/_data/ssl/mail.key \
  -out /var/lib/docker/volumes/poste_data/_data/ssl/mail.crt \
  -subj "/C=ES/ST=Madrid/L=Madrid/O=SRI/OU=Mail/CN=mail.sri.test"
  ```
Posteriormente, se ha verificado el cifrado TLS del servicio IMAP mediante:

```bash
sudo openssl s_client -connect 127.0.0.1:993
```

**Evidencias:**
- [Comprobación certificado ssl](docs/evidences/2-poste.io/03-ssl:tls/certificado.png)

### 2.5 Acceso mediante cliente web (Webmail)

Se ha accedido correctamente a la cuenta de correo creada utilizando el cliente
web integrado en Poste.io, comprobando el envío y la recepción de correos.

**Evidencias:**
- [Acceso webmail](docs/evidences/2-poste.io/04-webmail/mario@sri.test.png)
- [Comprobación de envío de correos](docs/evidences/2-poste.io/04-webmail/envio-recepcion-correo.png)

## 3. Evidencias

Todas las evidencias gráficas de la práctica se encuentran organizadas en la
carpeta docs/evidences/, estructuradas por apartados según los ejercicios propuestos.