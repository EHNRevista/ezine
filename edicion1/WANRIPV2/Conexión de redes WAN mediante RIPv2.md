# Conexión de redes WAN mediante RIPv2



En este artículo vamos a ver cómo podríamos realizar la configuración en una red WAN (Red de Área Extensa) utilizando el protocolo de enrutamiento dinámico RIPv2. Para la práctica usaremos el software de Cisco “Packet Tracer” para poder simular el comportamiento de la red.

La herramienta __Cisco Packet Tracer__ cuenta con una alta variedad de posibilidades para la experimentación del comportamiento de la red. Destacar también su facilidad de utilización, además en internet hay infinidad de tutoriales y manuales para su uso y creación de redes.



> Podréis descargar el software gratuitamente desde la página oficial de Cisco Packet Tracer: https://www.netacad.com/es/courses/packet-tracer-download/



Comencemos con la práctica imaginándonos la situación de que necesitamos comunicar varias redes LAN (Redes de Área Local) las cuales están situadas en distintas ubicaciones (ej. En distintos edificios). 

Crearemos un escenario básico para realizar el ejercicio, para integrar routers, switchs y cualquier componente informático es tan sencillo como localizarlo y arrastrarlo al escenario:




![alt text](http://i66.tinypic.com/1zv95r6.jpg "Escenario Ejercicio")



Cada círculo representa un edificio. El círculo amarillo representaría el __edificio1__ y el círculo verde el __edificio2__. Analicemos la imagen para aclarar diferentes términos:



#### Información acerca de los routers

- Los routers utilizados para las redes ethernet tienen de nombre en Packet Tracer __“1941”__ mientras que los dos routers (Router -PT) que serán utilizados para la conexión entre edificios son __“Generic”__. 


- La conexión entre los Router –PT se hacen mediante las interfaces __Serial__, la conexión entre estos y los demás routers de su edificio mediante interfaces __FastEthernet__, y las conexiones de los routers 1941 del mismo edificio mediante interfaces **GigabitEthernet**.

#### Información acerca de los switchs

- Los switch utilizados son __“2960”__. 
- Cada Switch representaría una sala, al cual estarían conectados los equipos. En este caso solo he puesto un equipo por sala para que la configuración no sea tan tediosa.

#### Aspectos generales

- Las direcciones IP para las redes locales serán de clase C, mientras que para los routers conectados mediante cable serial, es decir los routers que conectarán un edificio con otro serán de clase B.


- Los cuadros de texto __“.1”__ y __“.2”__ representan la dirección IP que se usará para esa conexión. Por ejemplo, el cable que conecta el Router 2 con el Switch 0 utilizará la dirección 192.168.1.2 y el cable que conecta este con el Switch 1 utilizará la dirección 192.168.2.1.
- Debéis de tener en cuenta que al crear vuestro escenario, el nombre asignado a los componentes no será el mismo que en el de la imagen. Para que esto no llege a provocar quiebros de cabeza es recomendable nombrarlos al igual que en la imagen. Lo mismo sucederá con el interfaz utilizado para cada conexión, sed cautelosos y estad seguros de que la interfaz a configurar es la correcta a la hora de asignar una dirección.



  

### Configuración de los equipos de la red / edificio 1

------

Introducir una dirección IP a un equipo es tan simple como: __Doble clic sobre el equipo a configurar > Desktop > IP Configuration__.



Ejemplo de configuración del equipo PC0:

![alt text](http://i66.tinypic.com/jqgf0w.jpg "PC0 Configuracion")



Le asignaremos una IP correspondiente a su red (192.168.1.0/24) y la dirección de su puerta de enlace, la cual será la 192.168.1.1 es decir, el Router –PT 10. En la siguiente tabla se muestra la configuración restante de los equipos del __edificio1__:

| Equipo | Dirección IP | Máscara de Red | Puerta de enlace | Red a la cual pertenece |
| :----: | :----------: | :------------: | :--------------: | :---------------------: |
|  PC1   | 192.168.2.3  | 255.255.255.0  |   192.168.2.1    |       192.168.2.0       |
|  PC2   | 192.168.3.3  | 255.255.255.0  |   192.168.3.1    |       192.168.3.0       |



### Configuración de los routers de la red / edificio 1

------

Asignar direcciones IP en routers es posible hacerlo mediante la interfaz gráfica del mismo o por **CLI** ( Interfaz de línea de comandos ). En esta práctica utilizaremos la interfaz gráfica pero también aprenderemos a como configurarlo mediante comandos. 

Empezamos con el __Router2__. Podemos observar como este está conectado al Switch 0 y al Switch 1, como explicamos anteriormente el cuadro de texto __.1__ simbolizará la dirección IP que se usará para esa conexión. Para saber sobre que conexión estamos tratando es tan sencillo como dejar el cursor sobre la misma, en caso de que no funcione siempre podemos quitar el cable y volverle a conectar al dispositivo para nosotros indicar el número interfaz a utilizar. Otra forma de saberlo es encendiendo el puerto (Port Status).



![alt text](http://i67.tinypic.com/2qxywyd.jpg)



Observamos como en la imagen el cable que conecta el __Router 2__ con el __Switch 0__ en mi caso utiliza la interfaz __Gig0/1__ en el router. Sabiendo que si ya se está utilizando la interfaz Gig0/1 en esa conexión sólo nos quedará la interfaz Gig0/0 para la conexión con el Switch 1. Procedemos con la configuración, para ello seguiremos los siguientes pasos: __Doble clic sobre el router a configurar > Config > GigabitEthernet0/*__:



![alt text](http://i67.tinypic.com/208tjsg.jpg "Router 2 Configuracion")



Como la conexión por la interfaz Gig0/1 conecta con el Switch0 ( la red 192.168.1.0 ) le asignamos la dirección 192.168.1.2. Debemos de estar seguros que el puerto este encendido, por lo que activaremos la palomilla de __“Port Status”__, pasando de estar la conexión del color rojo al color verde. Ahora solo nos quedaría configurar la interfaz GigabitEthernet0/0 del router para finalizar la configuración de mismo. La cual sería la siguiente: 192.168.2.1 con máscara 255.255.255.0.



#### // Configuración del Router 1:

| Interfaz | Dirección IP | Máscara de Red |
| :------: | :----------: | :------------: |
|  Gig0/0  | 192.168.2.2  | 255.255.255.0  |
|  Gig0/1  | 192.168.3.2  | 255.255.255.0  |

#### // Configuración del Router-PT 10 :

| Interfaz | Dirección IP | Máscara de Red |
| :------: | :----------: | :------------: |
|  Fa0/0   | 192.168.1.1  | 255.255.255.0  |
|  Fa1/0   | 192.168.3.1  | 255.255.255.0  |

Podemos observar como este router utiliza interfaces FastEthernet y no GigabitEthernet. A continuación te muestro como configurar este router mediante la pestaña __CLI__:

```
Router> enable
Router# configure terminal 		
Router(config)# interface FastEthernet 0/0  // Interfaz a configurar
Router(config-if)# ip address 192.168.1.1 255.255.255.0  // Dirección IP y Máscara de red
Route(config-if)# no shutdown  // Equivale a port status: ON
```



Viene bien familiarizarse con esta pestaña, normalmente los routers de CISCO traen un servidor web para conectarnos y realizar la configuración.

Para configurar la interfaz FastEthernet 0/1 seria exactamente igual, las interfaces serial serán configuradas posteriormente.



### Configuración de los equipos de la red / edificio 2

------

Comenzaremos a asignar direcciones a los equipos del edificio 2 como aprendimos anteriormente:


| Equipo | Dirección IP | Máscara de Red | Puerta de enlace | Red a la cual pertenece |
| :----: | :----------: | :------------: | :--------------: | :---------------------: |
|  PC3   | 192.168.4.3  | 255.255.255.0  |   192.168.4.1    |       192.168.4.0       |
|  PC4   | 192.168.5.3  | 255.255.255.0  |   192.168.5.1    |       192.168.5.0       |
|  PC5   | 192.168.6.3  | 255.255.255.0  |   192.168.6.1    |       192.168.6.0       |



### Configuración de los routers de la red / edificio 2

------



#### // Configuración del Router 3:

| Interfaz | Dirección IP | Máscara de Red |
| :------: | :----------: | :------------: |
|  Gig0/0  | 192.168.4.2  | 255.255.255.0  |
|  Gig0/1  | 192.168.5.2  | 255.255.255.0  |

#### // Configuración del Router 4:

| Interfaz | Dirección IP | Máscara de Red |
| :------: | :----------: | :------------: |
|  Gig0/0  | 192.168.5.1  | 255.255.255.0  |
|  Gig0/1  | 192.168.6.2  | 255.255.255.0  |

#### // Configuración del Router -PT 11:

| Interfaz | Dirección IP | Máscara de Red |
| :------: | :----------: | :------------: |
|  Fa0/0   | 192.168.6.1  | 255.255.255.0  |
|  Fa1/0   | 192.168.4.1  | 255.255.255.0  |



### Conexión entre edificio 1 y edificio 2

------

Ya solo nos queda configurar los routers que conectarán un edificio con otro, es aquí cuando usaremos el protocolo **RIPv2**. Empezaremos con las interfaces serial, pasaremos el cursor por encima de la conexión como hicimos anteriormente y nos fijaremos el número de interfaz que utilizan para la conexión, en mi caso los dos routers utilizan la **Se2/0** por lo que su configuración será la siguiente:


|    Router    | Interfaz | Dirección IP | Máscara de red | Red a la cual pertenece |
| :----------: | :------: | :----------: | :------------: | :---------------------: |
| Router-PT 10 |  Se2/0   |  172.16.0.1  | 255.255.255.0  |       172.16.0.0        |
| Router-PT 11 |  Se2/0   |  172.16.0.2  | 255.255.255.0  |       172.16.0.0        |

Una vez asignadas las IPs nos quedaría añadir las direcciones de red a sus tablas de encaminamiento para que estos sean capaces de re direccionar los paquetes. Utilizaremos la pestaña __CLI__.

#### // Terminal Router -PT 10

```
Router> enable
Router# configure terminal
Router(config)# router rip  // indicamos que utilizaremos RIP
Router(config-router)# version 2  // y usaremos la versión 2 del protocolo
Router(config-router)# network 192.168.1.0  // añadimos las direcciones de red
Router(config-router)# network 192.168.2.0
Router(config-router)# network 192.168.3.0
Router(config-router)# network 172.16.0.0
```



#### // Terminal Router -PT 11

```
Router>  enable
Router #  configure terminal
Router (config) #  router rip		
Router (config-router) #  version 2		
Router (config-router) #  network 192.168.4.0		
Router (config-router) #  network 192.168.5.0
Router (config-router) #  network 192.168.6.0
Router (config-router) #  network 172.16.0.0
```




### Problemas con la red

------

Comprobaremos que todos los equipos de edificio1 envían y reciben paquetes correctamente de los equipos de edificio2 y viceversa. Para ello utilizaremos el **modo simulación** de Packet Tracer situado en la esquina inferior derecha donde podremos observar el viaje de los paquetes en la red. Seleccionaremos la opción con el icono de un sobre y elegiremos de que equipo a que equipo queremos que se envíe información. Si el envío se ha realizado correctamente, el mensaje que mostrará el programa será __“Successful”__. En este caso el mensaje que aparecerá en pantalla sera **"failed"** ya que la conexión entre edificios no se llevará a cabo hasta solucionar los siguientes "errores" :



#### // Error 1

Al intentar comunicar el PC0 con el PC5, si visualizamos el envío desde el modo simulación observamos como el paquete al llegar al Router –PT 10 este no sabe por dónde enviar la información ya que no cuenta con la dirección destino en su tabla de encaminamiento. ¿Cómo solucionamos esto?
Es muy sencillo, configuraremos los Routers –PT añadiéndoles una ruta estática para cuando se encuentren en esta situación saber por dónde direccionar los paquetes.  

*Router –PT 10 :* 

```
Router(config)# ip route 0.0.0.0 0.0.0.0 172.16.0.2
```

Añadiendo la dirección 0.0.0.0 con máscara 0.0.0.0 a la tabla de enrutamiento, estaremos asignando a esta como puerta de enlace pretederminada del router. Es decir cuando en el router no exista una ruta disponible para llegar a la dirección destino lo enviará a la 172.16.0.2

*Router –PT 11 :*

```
Router(config)# ip route 0.0.0.0 0.0.0.0 172.16.0.1
```



Ej. Le llega un paquete al Router –PT 10 con dirección destino 192.168.6.3. Como éste no cuenta con dicha dirección en su tabla lo direcciona por la ruta por defecto (172.16.0.2). El paquete le llega a Router –PT 11 el cual sí contiene la dirección destino en su tabla de encaminamiento.



#### // Error 2

Envío de información a los equipos alojados en las redes 192.168.2.0 y 192.168.5.0. Si utilizamos el modo simulación podemos comprobar como cuando los paquetes le llegan a los Routers -PT estos no son capaces de direccionarlos, aunque les hayamos añadido estas dos direcciones de red anteriormente, pero al no estar conectados los switchs directamente con el mismo se produce el error.

*Router –PT 10 :* 

```
Router(config) # ip route 192.168.2.0 255.255.255.0 192.168.1.2
```


*Router –PT 11 :* 

```
Router(config) # ip route 192.168.5.0 255.255.255.0 192.168.6.2
```



Ej. Paquete es enviado a PC1. Cuando este le llege al Router-PT 10 con dirección destino 192.168.2.3 lo direccionará por la conexión 192.168.1.2, es decir llegará hasta el Router 2 el cual sí consta de dicha dirección destino en su tabla de encaminamiento y hará llegar el paquete hasta PC1.



#### // Error 3

Envío de información desde los equipos alojados en las redes 192.168.2.0 y 192.168.5.0 a cualquier red fuera de su edificio. Cuando el paquete a enviar le llega al Router 2 en el caso del edificio1, este al no tener la dirección destino en su tabla de encaminamiento no sabría direccionarlo:

*Router 2 / edificio 1  :* 

```
Router (config) # ip route 0.0.0.0 0.0.0.0 192.168.1.1
```

*Router 4 / edificio 2:*  

```
Router (config) # ip route 0.0.0.0 0.0.0.0 192.168.6.1
```



Ej: PC1 envía un paquete a PC3, el paquete le llega a Router 2 el cual tiene la “orden” de que cuando no posea la dirección destino en su tabla de encaminamiento lo envíe por la 192.168.1.1. Con lo cual el paquete le llegaría al Router –PT 10, que tampoco tendría ni idea de direccionarlo, pero también le asignamos una ruta estática anteriormente para este caso, entonces el paquete le llegaría al Router –PT 11 que ya contaría con la dirección destino en su tabla de encaminamiento y el paquete llegaría a su destino.



Ahora si que sí todos deberíamos de tener conectividad total entre todas las estaciones de trabajo. 