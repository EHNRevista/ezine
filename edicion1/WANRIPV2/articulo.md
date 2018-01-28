# Conexión de redes WAN mediante RIPv2

En este artículo vamos a ver cómo podríamos realizar la configuración en una red WAN (Red de Área Extensa) utilizando el protocolo de enrutamiento dinámico RIPv2. Para la práctica usaremos el software de Cisco “Packet Tracer” para poder simular el comportamiento de la red. 

La herramienta Cisco Packet Tracer cuenta con una alta variedad de posibilidades para la experimentación del comportamiento de la red. Destacar también su facilidad de utilización, además en internet hay infinidad de tutoriales y manuales para su uso y creación de redes.
Comencemos con la práctica imaginándonos la situación de que necesitamos comunicar varias redes LAN (Redes de Área Local) las cuales están situadas en distintas ubicaciones (ej. En distintos edificios). Crearemos un escenario básico para realizar el ejercicio, para integrar routers, switchs y cualquier componente informático es tan sencillo como localizarlo y arrastrarlo al escenario:

Como podéis observar he separado en distintos círculos cada red local. Pongamos que el círculo amarillo representaría el edificio1 y el círculo verde el edificio2. Analicemos la imagen para aclarar diferentes términos:
  -	Las direcciones IP para las redes locales serán de clase C, mientras que para los routers que conectan una red con otra serán de clase      B.
  -	Los cuadros de texto .1 y .2 representan el número de la interfaz Gigabit Ethernet que se usa para esa conexión.
  -	Cada switch representaría una sala, al cual estarían conectados los equipos. En este caso solo he puesto uno para que la configuración     no sea tan tediosa.
  -	Los routers utilizados para las redes ethernet tienen de nombre en Packet Tracer “1941” mientras que los dos routers (Router -PT) que     serán utilizados para la conexión entre edificios son “Generic”.
  -	La conexión entre los Router –PT se hacen mediante las interfaces serial.
