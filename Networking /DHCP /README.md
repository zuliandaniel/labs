## DHCP
DHCP es un protocolo de asignación dinámica de direcciones IP.
En este laboratorio, vamos a analizar cómo un host se comunica con un servidor DHCP para obtener información de direccionamiento. 

# DHCP DISCOVER
El cliente se conecta a la red. Envía un broadcast con IP destino 255.255.255.255 puerto destino 67. Como origen, utiliza la dirección 0.0.0.0:68. 
Esta solicitud llegará a todos los dispositivos dentro de la misma subred, pero solo será atendida si existe un servidor DHCP escuchando el puerto 67. 
