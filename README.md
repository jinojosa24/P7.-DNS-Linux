# P7.-DNS-Linux
Se ha configurado un servidor DNS utilizando BIND9 y un cliente basado en Alpine Linux, ambos ejecutándose en contenedores Docker. La idea es disponer de un entorno donde el servidor DNS pueda gestionar consultas locales y reenviar aquellas que no pueda resolver a otros servidores externos mediante forwarders.

Se parte de un archivo docker-compose.yml que define dos servicios: el servidor DNS (bind9) y un cliente (alpine). El servidor tiene asignada una dirección IP fija dentro de una red interna (172.20.5.1), mientras que el cliente también tiene una IP fija (172.20.5.2). Esto se gestiona mediante una red interna propia llamada bind9_subnet, lo que asegura que los contenedores se comuniquen exclusivamente entre sí, sin interferencias externas.

El archivo de configuración docker-compose.yml incluye lo siguiente:

    Se utiliza la imagen oficial de internetsystemsconsortium/bind9:9.20 para el servidor.
    Se exponen los puertos 53 (TCP/UDP) mapeados al puerto 54 en el host, para evitar conflictos con otros posibles servicios DNS en la máquina.
    Se utilizan volúmenes para separar la configuración (./conf) y los archivos de zona (./zonas). De esta forma, es posible modificar los archivos locales sin necesidad de reconstruir el contenedor.

En cuanto a la configuración del servidor BIND9, en el archivo named.conf.options se han definido los forwarders para que, en caso de que el servidor no pueda resolver una consulta local, lo redirija a otro servidor.


Se ha creado una zona propia (db.example.com) que incluye los registros necesarios:

    Un registro SOA para definir la autoridad del dominio.
    Un registro NS para especificar el servidor de nombres.
    Un registro A que apunta la IP del servidor.