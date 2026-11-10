# Instalación Ubuntu:
![imagen1](fotosdani/1oracle.png)   
![imagen2](fotosdani/2oracle.png)   
fran y 1234  
![imagen3](fotosdani/3oracle.png)   
8MB   
![imagen4](fotosdani/4oracle.png)   
# Instalación docker
## Imágenes:
![imagen5](fotosdani/ubuntu1.png)    
![imagen6](fotosdani/ubuntu2.png)    
![imagen7](fotosdani/ubuntu3.png)    
![imagen8](fotosdani/ubuntu4.png)     
![imagen9](fotosdani/ubuntu5.png)    
![imagen10](fotosdani/ubuntu6.png)   
![imagen11](fotosdani/ubuntu7.png)   
![imagen12](fotosdani/ubuntu8.png)    
![imagen13](fotosdani/ubuntu9.png)    
![imagen14](fotosdani/ubuntu10.png)    
![imagen15](fotosdani/ubuntu11.png)   
![imagen16](fotosdani/ubuntu12.png)   
![imagen17](fotosdani/ubuntu13.png)    
![imagen18](fotosdani/ubuntu14.png)   
![imagen19](fotosdani/ubuntu15.png)   
![imagen20](fotosdani/ubuntu16.png)    
![imagen21](fotosdani/ubuntu17.png)   

## Comandos usados:


# Requerimientos mínimos para implantar una aplicación web:
* ## Requisitos de hardware y software 
    * **Procesador(CPU)** Se requiere un mínimo de 2 núcleos virtuales (vCPU) con arquitectura de 64 bits. 
    * **Memoria RAM** Se necesitan al menos 2 GB a 4 GB de memoria RAM. 
    * **Almacenamiento** Al menos 20 GB a 40 GB. 
    * **Sistema operativo** Se recomienda una distribución Linux de soporte extendido (LTS) como Ubuntu Server, Debian o AlmaLinux. 
    * **Entorno de ejecución y base de datos** Es necesario instalar el intérprete o compilador específico del lenguaje de programación (como Node.js, Python, PHP o Java) junto con un sistema gestor de bases de datos (como PostgreSQL, MySQL o MongoDB).

* ## Infraestructura de red
    * **Dirección IP pública y estática** El servidor debe contar con una dirección IP fija accesible desde internet.
    * **Nombre de dominio y DNS** Se requiere registrar un nombre de dominio y configurar sus registros DNS (como los registros tipo A o CNAME). 
    * **Ancho de banda y latencia** Se necesita una conexión de red estable con un ancho de banda simétrico mínimo de 100 Mbps.
    * **Apertura de puertos** La red debe permitir el tráfico entrante hacia los puertos estándar de navegación web. Los puertos principales son el 80 para tráfico HTTP y el 443 para tráfico seguro HTTPS.

* ## Configuración del servidor web y de aplicaciones
    * **Servidor web y proxy inverso** Se debe configurar un software como Nginx o Apache para recibir las peticiones de los clientes.
    * **Servidor de aplicaciones** Es necesario configurar un gestor de procesos o servidor interno (como Gunicorn, PM2 o Tomcat) que ejecute el código fuente. 
    * **Variables de entorno** Los datos sensibles y parámetros de configuración (como claves de API, credenciales de base de datos y modo de producción) deben almacenarse en variables de entorno externas al código fuente. 
    * **Gestión de registros (Logs)** Se debe habilitar la escritura de archivos de registro tanto para los accesos como para los errores del servidor. 

* ## Seguridad y mantenimiento
    * **Certificado SSL/TLS** Es obligatorio instalar un certificado digital (como los emitidos por Let's Encrypt) para habilitar el protocolo HTTPS. 
    * **Cortafuegos (Firewall)** Se debe activar un firewall a nivel de sistema operativo (como UFW o iptables) o de red. 
    * **Acceso administrativo seguro** El acceso remoto al servidor mediante SSH debe configurarse utilizando claves criptográficas en lugar de contraseñas simples.
    * **Copias de seguridad (Backups)** Se requiere programar respaldos automáticos y periódicos tanto de la base de datos como de los archivos subidos por los usuarios. 
    * **Actualizaciones y monitoreo** Es necesario aplicar de forma regular los parches de seguridad del sistema operativo y de las dependencias del software. 

