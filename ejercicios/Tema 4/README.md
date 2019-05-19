# Ejercicios Tema 4

## T3.1: Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el enrutamiento del tráfico de un servidor para pasar el tráfico desde una subred a otra

Podemos usar el comando route para enrutar el tráfico si nuestro servidor corre tanto bajo linux o bajo Windows. Para ello añadimos a la tabla de rutas el enrutamiento que queremos que siga nuestro paquete con el siguiente comando:
`ROUTE [-f] [-p] [-4|-6] ADD [destino] [MASK máscara_red] [puerta_enlace]
      [METRIC métrica] [IF interfaz]`

## T3.2: Buscar con qué órdenes de terminal o herramientas gráficas podemos configurar bajo Windows y bajo Linux el filtrado y bloqueo de paquetes

- Linux: ufw, iptables, IPCop Firewall, Shorewall, pfSense, IPFire, VyOS
- Windows: Windows defender, Kaspersky, Norton, Bitdefender, Zone Alarm Pro Firewall