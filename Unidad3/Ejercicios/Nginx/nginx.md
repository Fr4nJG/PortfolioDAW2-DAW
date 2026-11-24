# Instalación y configuración de Nginx en Ubuntu





## Índice

1. [Introducción y objetivos](#1-introducción-y-objetivos)
2. [Investigación teórica](#2-investigación-teórica)
   1. [¿Qué es Nginx?](#21-qué-es-nginx)
   2. [Diferencias principales con Apache](#22-diferencias-principales-con-apache)
   3. [Instalación y ejecución en Ubuntu (resumen)](#23-instalación-y-ejecución-en-ubuntu-resumen)
3. [Implementación paso a paso](#3-implementación-paso-a-paso)
   1. [Instalación de Nginx](#31-instalación-de-nginx)
   2. [Configuración del servidor (bloque *server*, equivalente a VirtualHost)](#32-configuración-del-servidor-bloque-server-equivalente-a-virtualhost)
   3. [Validación y aplicación de cambios](#33-validación-y-aplicación-de-cambios)
4. [Verificación del acceso](#4-verificación-del-acceso)
5. [Conclusiones y dificultades encontradas](#5-conclusiones-y-dificultades-encontradas)


---

## 1. Introducción y objetivos

El objetivo de esta práctica es conocer el servidor web **Nginx**, compararlo con Apache e instalarlo y configurarlo en un sistema **Ubuntu**.



## 2. Investigación teórica

### 2.1. ¿Qué es Nginx?

Nginx es un software de código abierto que funciona como **servidor web**, pero que también se usa habitualmente como proxy inverso, balanceador de carga y caché HTTP. Está pensado para atender un gran número de conexiones simultáneas consumiendo pocos recursos.

Características principales:

- **Arquitectura asíncrona y orientada a eventos:** un número reducido de procesos *worker* gestiona miles de conexiones mediante un bucle de eventos no bloqueante, en lugar de dedicar un proceso o hilo a cada petición.
- **Muy eficiente sirviendo contenido estático** (HTML, CSS, JS, imágenes).
- **Proxy inverso y balanceo de carga:** reparte el tráfico entre varios servidores de aplicación.
- **Configuración centralizada** en ficheros de texto con una sintaxis de bloques (`http`, `server`, `location`).
- Existe una versión comercial, **Nginx Plus**, con funciones adicionales y soporte.

Estructura habitual de ficheros en Ubuntu:

| Ruta | Función |
|---|---|
| `/etc/nginx/nginx.conf` | Configuración global |
| `/etc/nginx/sites-available/` | Bloques de servidor disponibles (no necesariamente activos) |
| `/etc/nginx/sites-enabled/` | Enlaces simbólicos a los bloques activos |
| `/var/www/html/` | Raíz de documentos por defecto |
| `/var/log/nginx/access.log` y `error.log` | Registros de accesos y de errores |

### 2.2. Diferencias principales con Apache

| Aspecto | Nginx | Apache |
|---|---|---|
| **Modelo de procesamiento** | Orientado a eventos y asíncrono: pocos *workers* atienden muchas conexiones | Basado en MPM (*Multi-Processing Modules*): un proceso o hilo por conexión (el MPM *event* mejora esto) |
| **Contenido estático** | Muy rápido y con poco consumo de memoria | Mayor consumo de recursos bajo carga |
| **Contenido dinámico (PHP, etc.)** | No lo procesa por sí mismo: delega en un intérprete externo | Puede ejecutarlo mediante módulos integrados |
| **Configuración por directorio** | No admite `.htaccess`; todo se configura de forma centralizada y requiere recargar el servidor | Admite `.htaccess`, que permite cambios por directorio sin acceso a la configuración principal |
| **Módulos** | Soporta módulos dinámicos, pero muchos de terceros requieren compilar Nginx | Gran catálogo de módulos que se activan/desactivan fácilmente (`a2enmod`) |
| **Virtual hosts** | *Server blocks* (`server { ... }`) | `<VirtualHost>` |
| **Uso típico** | Alta concurrencia, proxy inverso, balanceo de carga, contenedores | Hosting compartido, aplicaciones que dependen de `.htaccess` |
| **Curva de aprendizaje** | Algo más exigente para principiantes (sin `.htaccess`, intérpretes externos) | Más sencillo y flexible para usuarios con poco acceso al servidor |


### 2.3. Instalación y ejecución en Ubuntu (resumen)

Nginx está incluido en los repositorios oficiales de Ubuntu, por lo que se instala con el gestor de paquetes `apt`. El esquema general que describen los tutoriales consultados es:

1. Actualizar el índice de paquetes (`sudo apt update`).
2. Instalar el paquete (`sudo apt install nginx`). Al terminar, el servicio se inicia automáticamente.
3. Si hay cortafuegos (UFW), permitir el tráfico HTTP (`sudo ufw allow 'Nginx HTTP'`).
4. Comprobar el estado del servicio (`systemctl status nginx`) y acceder desde el navegador para ver la página de bienvenida.
5. Crear un bloque de servidor: directorio de contenido en `/var/www/`, fichero en `sites-available`, enlace simbólico en `sites-enabled`.
6. Validar la sintaxis (`nginx -t`) y recargar (`systemctl reload nginx`).

Gestión del servicio: `start`, `stop`, `restart`, `reload`, `enable` (arranque automático) y `disable` mediante `systemctl`.

---

## 3. Implementación paso a paso

> **Entorno de la práctica:** Ubuntu _(indicar versión, p. ej. 22.04 / 24.04)_, con un usuario con privilegios `sudo`.
> **Dominio de prueba:** `nginx-practica.local` (resuelto localmente mediante `/etc/hosts`).

### 3.1. Instalación de Nginx

**Paso 1. Actualizar el índice de paquetes**

```bash
sudo apt update
```

**Paso 2. Instalar Nginx**

```bash
sudo apt install nginx
```

**Paso 3. Comprobar que el servicio está activo y la versión instalada**

```bash
systemctl status nginx
nginx -v
```

El estado debe mostrar `active (running)`.

![fotoMagica](fotos/foto1)
![fotoMagica](fotos/foto2)  
 Salida de `systemctl status nginx` y `nginx -v`.
**Paso 4. (Si UFW está activo) Permitir el tráfico HTTP**

```bash
sudo ufw allow 'Nginx HTTP'
sudo ufw status
```

**Paso 5. Comprobar la página de bienvenida por defecto**

```bash
curl -I http://localhost
```

o abrir `http://localhost` en el navegador. Debe aparecer *«Welcome to nginx!»*.

![fotoMagica](fotos/foto3) Página de bienvenida de Nginx en el navegador (o `curl -I` con `HTTP/1.1 200 OK`).

> **Nota:** si Apache estuviera instalado y escuchando en el puerto 80, habría un conflicto. En ese caso hay que detenerlo antes (`sudo systemctl stop apache2`) o cambiar uno de los dos de puerto.

### 3.2. Configuración del servidor (bloque *server*, equivalente a VirtualHost)

**Paso 1. Crear el directorio de contenido del sitio**

```bash
sudo mkdir -p /var/www/nginx-practica.local/html
```

**Paso 2. Crear una página de prueba**

```bash
sudo nano /var/www/nginx-practica.local/html/index.html
```

Contenido:

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Práctica Nginx</title>
</head>
<body>
  <h1>¡Funciona! Este sitio se sirve con Nginx</h1>
  <p>Bloque de servidor: nginx-practica.local</p>
</body>
</html>
```

**Paso 3. Ajustar propietario y permisos**

```bash
sudo chown -R $USER:$USER /var/www/nginx-practica.local/html
sudo chmod -R 755 /var/www/nginx-practica.local
```

**Paso 4. Crear el fichero del bloque de servidor**

```bash
sudo nano /etc/nginx/sites-available/nginx-practica.local
```

Contenido:

```nginx
server {
    listen 80;
    listen [::]:80;

    server_name nginx-practica.local www.nginx-practica.local;

    root /var/www/nginx-practica.local/html;
    index index.html;

    access_log /var/log/nginx/nginx-practica.access.log;
    error_log  /var/log/nginx/nginx-practica.error.log;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

Explicación de las directivas:

| Directiva | Significado | Equivalente en Apache |
|---|---|---|
| `listen` | Puerto (y dirección) en el que escucha | `<VirtualHost *:80>` |
| `server_name` | Nombre(s) de dominio atendidos por este bloque | `ServerName` / `ServerAlias` |
| `root` | Directorio raíz de documentos | `DocumentRoot` |
| `index` | Fichero que se sirve por defecto | `DirectoryIndex` |
| `access_log` / `error_log` | Registros específicos del sitio | `CustomLog` / `ErrorLog` |
| `location /` | Reglas para una ruta; `try_files` devuelve 404 si no existe el recurso | `<Directory>` / `<Location>` |

![fotoMagica](fotos/foto4) Fichero `/etc/nginx/sites-available/nginx-practica.local` abierto en el editor.

**Paso 5. Activar el bloque creando un enlace simbólico**

```bash
sudo ln -s /etc/nginx/sites-available/nginx-practica.local /etc/nginx/sites-enabled/
```

**Paso 6. (Opcional) Desactivar el sitio por defecto**

```bash
sudo rm /etc/nginx/sites-enabled/default
```

**Paso 7. Resolver el dominio de prueba en local**

```bash
sudo nano /etc/hosts
```

Añadir la línea:

```
127.0.0.1   nginx-practica.local www.nginx-practica.local
```

### 3.3. Validación y aplicación de cambios

**Paso 1. Comprobar la sintaxis de la configuración**

```bash
sudo nginx -t
```

Salida esperada:

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

![fotoMagica](fotos/foto5)  
 Resultado de `sudo nginx -t`.

**Paso 2. Aplicar los cambios sin cortar el servicio**

```bash
sudo systemctl reload nginx
```

`reload` aplica la nueva configuración sin interrumpir las conexiones activas; `restart` detiene y vuelve a arrancar el servicio.

**Paso 3. Comprobar que el servicio sigue activo y habilitar el arranque automático**

```bash
systemctl status nginx
sudo systemctl enable nginx
```

---

## 4. Verificación del acceso

**Prueba 1. Desde la terminal**

```bash
curl -I http://nginx-practica.local
curl http://nginx-practica.local
```

Resultado esperado: cabecera `HTTP/1.1 200 OK` con `Server: nginx/...` y el HTML de la página de prueba.

![fotoMagica](fotos/foto6)  
 Salida de `curl -I http://nginx-practica.local` mostrando `200 OK` y `Server: nginx`.

**Prueba 2. Desde el navegador**

Abrir `http://nginx-practica.local` y comprobar que aparece el mensaje «¡Funciona! Este sitio se sirve con Nginx».

![fotoMagica](fotos/foto7)  
 Página del sitio en el navegador con la URL visible.

**Prueba 3. Comprobar los registros**

```bash
sudo tail -n 5 /var/log/nginx/nginx-practica.access.log
```

Deben aparecer las peticiones realizadas en las pruebas anteriores.

![fotoMagica](fotos/foto8)  
 Últimas líneas del registro de accesos.

**Prueba 4. Comprobar el error 404 (control de `try_files`)**

```bash
curl -I http://nginx-practica.local/no-existe
```
![fotoMagica](fotos/fotoError)  

Resultado esperado: `HTTP/1.1 404 Not Found`.

---

## 5. Conclusiones y dificultades encontradas

### Conclusiones

- El **bloque `server`** cumple la misma función que un VirtualHost de Apache, y el esquema `sites-available` / `sites-enabled` con enlaces simbólicos permite activar y desactivar sitios sin borrar su configuración.
- Validar con `nginx -t` antes de recargar es una buena práctica: evita dejar el servidor caído por un error de sintaxis.
- La diferencia de diseño con Apache (arquitectura por eventos, configuración centralizada y sin `.htaccess`) hace de Nginx una opción muy adecuada para contenido estático, alta concurrencia y proxy inverso, a cambio de menos flexibilidad por directorio.

### Dificultades encontradas

> _Completar con tu experiencia real. Estas son las más habituales; conserva solo las que te hayan ocurrido y añade las tuyas._

- Errores de sintaxis en el bloque.
- Resolución del dominio de prueba: sin la entrada en `/etc/hosts`, el navegador no encontraba `nginx-practica.local`.
- Sitio por defecto activo: si no se desactiva o no se define bien `server_name`, puede responder el bloque `default` en lugar del nuevo.
- Adaptación mental desde Apache: no existe `.htaccess` y los cambios requieren recargar el servicio.

