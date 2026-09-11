# Escaneo de puertos con NMAP

## Introducción
NMAP es una herramienta utilizada para descubrimiento de hosts y enumeración de servicios de red. 
El escaneo de puertos suele ser una de las primeras actividades realizadas sobre un objetivo, ya que permite identificar servicios expuestos, versiones de software y posibles vectores de ataques.
En este laboratorio, utilizaremos NMAP para analizar un objetivo vulnerable identificando puertos abiertos y versiones de software. 
Utilizaremos como máquina atacante un Kali Linux y como víctima a la conocida Metaesploitable2, una VM especialmente configurada para ser atacada y realizar diferentes pruebas de pentesting. 

## Descubrimiento de puertos
Como primera prueba, vamos a identificar qué servicios se encuentran expuestos en la VM objetivo. Para esto, se ejecuta un escaneo de los puertos TCP más comunes
```bash
nmap 192.168.0.200
```
![NMAP](NMAP/images/nmap.png)

En esta etapa todavía no conocemos qué aplicaciones se encuentran detrás de cada puerto. Solo sabemos que existen servicios disponibles para establecer conexiones. Cada puerto abierto representa un posible punto de entrada al sistema. 
En la captura anterior vemos como NMAP identifica múltiples puertos abiertos, indicando que existen servicios escuchando conexiones remotas. Esta información nos permite: 
- Identificar la superficie de ataque
- Determinar qué servicios pueden ser analizados posteriormente
- Desde el punto de vista defensivo, en caso de estar realizando un pentest, nos permite identificar servicios innecesarios o potencialmente inseguros

Sin embargo, el número de puerto por si solo no garantiza qué servicio está ejecutándose realmente. Por esto, será preciso realizar un escaneo con más detalle.

## Enumeración de servicios
Una vez identificados los puertos abiertos, vamos a determinar qué aplicaciones y versiones se encuentran ejecutándose detrás de cada servicio. 
La opción `-sV` permite identificar versiones de servicios, información útil para detectar vulnerabilidades conocidas y comprender mejor la superficie de ataque del sistema.

![NMAP](NMAP/images/nmapsv.png)

La información obtenida permite realizar actividades de investigación y análisis de vulnerabilidades. Por ejemplo, el resultado nos muestra que en el puerto 80 se identificó un servidor Apache 2.2.8 o en el puerto 22 está corriendo OpenSSH 4.7p1. 
Con estos datos, es posible consultar fuentes públicas para determinar si existen vulnerabilidades conocidas asociadas a esas versiones. Algunas fuentes conocidas son CVE, NVD, MITRE ATT&CK.
NMAP también ofrece la posibilidad de ir un poco más allá y obtener más información. La opción `-sC` ejecuta un conjunto de scripts diseñados para recopilar información útil sobre los servicios detectados. 
A modo de ejemplo, realizamos el análisis sobre los puertos 22 y 80 previamente identificados. 
```bash
nmap -sV -sC -p 80 192.168.0.200
nmap -sV -sC -p 22 192.168.0.200
```
![NMAP](NMAP/images/nmapsc.png)
![NMAP](NMAP/images/nmapsc2.png)

Con la opción `-p` limitamos el análisis a un puerto. Si bien en el ejemplo se hicieron dos análisis uno para cada puerto, podríamos haber obtenido el mismo resultado enumerando ambos puertos en el mismo comando: 

```bash
nmap -sV -sC -p 22,80 192.168.0.200
```
Para estos ejemplos, el análisis nos da mucha información: 

**PUERTO 80** 
- Servidor web Apache 2.2.8
- Sistema operativo Ubuntu
- Título de la página web: Metasploitable2 - Linux
La versión del servidor puede utilizarse para realizar búsqueda de vulnerabilidades conocidas. Esta información permite orientar posteriores pruebas de penetración sobre tecnologías específicas.

**PUERTO 22**
  - Servicio OpenSSH v4.7p1
  - Sistema operativo Debian/Ubuntu
  - Protocolo SSH 2.0
  - Claves públicas del servicor: DSA 1024 bits y RSA 2048 bitsf
La obtención de la versión del servicio facilita la búsqueda de CVEs asociados y la identificación de configuraciones inseguras (por ejemplo, versiones ya obsoletas).

## Identificación del sistema operativo

Una vez identificados los servicios expuestos, resulta útil también conocer el sistema operativo que ejecuta el objetivo. 
Con la opción `-O` NMAP puede estimar qué sistema operativo se encuentra en ejecución, incluso sin tener acceso al sistema. Esta información puedo utilizarse para identificar vulnerabilidades específicas del sistema operativo y verificar compatibilidad de exploits u otras herramientas de ataque.
```bash
nmap -O 192.168.0.200
```
![NMAP](NMAP/images/nmapo.png)

## Conclusión
Durante este laboratorio comprobamos la utilidad de NMAP en la fase de reconocimiento, realizando diferentes pruebas para identificar múltiples servicios expuestos, y obtener información detallada de esos servicios. Comprobamos la utilidad de NMAP durante la fase de reconocimiento. 
La información obtenida, puede ser utilizada posteriormente para realizar análisis de vulnerabilidades o pruebas de penetración más específicas.






