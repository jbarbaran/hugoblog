+++
date = '2024-09-09T16:00:00+01:00'
draft = false
title = 'DietPi, el sistema operativo para darle una nueva vida a una Raspberry Pi 1'
lang=  'es'
+++

![Raspberry pi 1 image](https://images.prismic.io/rpf-products/3dc09a41-c237-4d2c-a9b8-c92eb3dc98e8_B%2B+ANGLE+1+REFRESH.jpg?auto=compress%2Cformat&fit=max)

¿Tienes por ahí una Raspberry Pi 1 y no sabes que hacer con ella? Buscando un poco por Internet, me he topado con [DietPi](https://dietpi.com/), se trata de una distribución Debian muy optimizada en uso de memoria, espacio en disco y uso de CPU. Está orientada para ordenadores de una placa (o en inglés, Single Board Computer o SBC), como es la Raspberry Pi en todas sus versiones, entre otros (como Odroid, Pine64, Radxa, Allo, ASUS, NanoPi, OrangePi, ASUS Tinker), miniPCs y hasta maquinas virtuales o PCs.

En la web de DietPi muestran como de optimizada está esta distribución de Linux, en este [enlace](https://dietpi.com/stats.html#distrostats) puedes ver la comparativa con algunas placas y otras distribuciones. ¡Ojo! Esta comparativa no dice que DietPi sea mejor que las otras distribuciones, tan solo que es mucho mas ligera.

Otra ventaja muy importante que ofrece esta distribución es que ofrece una lista de software inmensa para instalar con DietPi, de forma que el usuario prácticamente no tiene que hacer nada para instalarlo. Aquí os dejo el [enlace](https://dietpi.com/dietpi-software.html) a todo lo que permite instalar.

Después de leer todas las bondades de DietPi no me quedaba otra que probarlo en mi Raspberry Pi 1, para ver que tal funciona.

### Instalación del DietPi para una Raspberry Pi 1

En la web del DietPi dan una descripción detallada de como se hace el proceso de instalación, haz clic [aquí](https://dietpi.com/docs/install/) para verlo. De todas formas, os cuento brevemente como ha sido mi experiencia y los pasos que he ido dando (muy sencillo):

1. Escoger y descargar la imagen adecuada en base al miniPC que vayas a usar (Raspberry Pi 1 en mi caso).

2. La imagen que te descargas no es directamente .iso, sino que viene comprimida, pero usando una aplicación con 7zip se puede descomprimir fácilmente.

3. Usando el programa [Balena Etcher](https://etcher.balena.io/) hay que grabar la imagen que te acabas de descargar.
Una vez grabada la imagen, sin extraerla de tu ordenador, accede a la tarjeta SD con el explorador de archivos (si usas Windows como yo), y busca el archivo ```dietpi.txt```. Este fichero te permite configurar ciertos parámetros de modo que DietPi en su primer arranque hará casi toda la configuración sin que tengas que hacer nada. 

Lo he abierto con Notepad++ y a continuación te cuento las partes que he editado:

```AUTO_SETUP_LOCALE=es_ES.UTF-8```, para la configuración regional.

```AUTO_SETUP_KEYBOARD_LAYOUT=es```, para configurar el teclado por si es necesario al conectarnos por SSH.

```AUTO_SETUP_TIMEZONE=Europe/Madrid```, para la zona horaria y que se actualice la hora correctamente.

```AUTO_SETUP_NET_WIFI_COUNTRY_CODE=ES```, esto es opcional, por si en algun momento conectas WiFi en la raspi.

```AUTO_SETUP_NET_USESTATIC=1```, para seleccionar una conexión estática en lugar de DHCP a tu router (en mi caso, aunque tiene el DHCP habilitado, funciona siempre que la IP que cojas esté libre).

```AUTO_SETUP_NET_STATIC_IP=192.168.0.100```, la IP que quieras asignar a tu raspi dentro de tu red local.

```AUTO_SETUP_NET_STATIC_MASK=255.255.255.0```, la mascara de red, es raro que tengas que cambiar este valor.
```AUTO_SETUP_NET_STATIC_GATEWAY=192.168.0.1```, la IP de tu router.

```AUTO_SETUP_NET_STATIC_DNS=9.9.9.9 149.112.112.112```, son los DNS Quad9, puedes dejar estos.

```AUTO_SETUP_NET_HOSTNAME=DietPi```, es el nombre que le quieres dar a tu raspi en tu red.

```AUTO_SETUP_HEADLESS=1```, en este caso deshabilita el HDMI para ahorrar recursos, porque siempre me voy a conectar por SSH.

```AUTO_SETUP_AUTOMATED=1```, selecciona la instalación automatizada de DietPi. Esto significa que el sistema se configurará y completará la instalación usando las configuraciones predefinidas en el archivo ```dietpi.txt```.

5. Una vez hecho esto, simplemente queda poner la microSD en la raspi, conectarle el cable de Ethernet (en mi caso era mas sencillo que buscar drivers para un adaptador WiFi USB, pero esto tambien podrías hacerlo), y simplemente conectarla a la alimentación.

En mi caso, confié en que con la configuración que hice todo funcionaría, así que no conecté pantalla ni teclado, simplemente esperé a que en algun momento apareciese en mi red en la IP que le había configurado. Cuando parecía que algo iba a fallar…. Et voilá! Cuando ya creía que algo había fallado (2 o 3 minutos, no tengo mucha paciencia) apareció en la red e hice una conexión SSH desde un terminal, y ya estaba conectado.

Una vez conectado, pude ver que fue haciendo, se actualizó con el típico ```apt-get update```, y me hizo algunas preguntas en el proceso de instalación:

- Si quería participar en unas estadísticas anónimas.

- Si quería cambiar el acceso de los usuarios root y dietpi. Esto hay que hacerlo si o si, por seguridad. Lo que te pide es una contraseña para cada tipo de usuario.

- Luego te pregunta si quieres dejar habilitada la salida serie que tiene la raspi, es útil si te puedes conectar a ella por un cable, en el caso de que algo no vaya bien. Si no tienes el conector serie necesario y no piensas que vayas a conectarte nunca, es mejor desactivarlo porque ahorra recursos.

Después de hacer todo esto, te muestra esta interfaz:

![DietPi configuration screenshot](/images/diet-pi-config.webp)

Lo interesante aquí es que puedes seleccionar el/los software/s que le quieres instalar. Si seleccionar «Browse software» aparece una lista inmensa de software a instalar.

Llegado a este punto ya tengo un DietPi ejecutándose en la raspi, pero.. ¿que puedo hacer con ella? ¿algún proyecto útil? Así que empecé a buscar…. últimamente ademas de buscar en Internet he descubierto el potencial de [Reddit](wwww.reddit.com), tiene una cantidad de conocimiento enorme y de ahí saqué algunas ideas de que hacer con mi Raspberry pi 1 con el DietPi instalado:

1. PiHole, que básicamente es un servidor de DNS para casa, con el objetivo de eliminar publicidad de las páginas webs que visitamos, bloquear rastreadores que quieran coger información de donde navegamos, y otros enlaces que tengan objetivos maliciosos.

2. Como primer ordenador para un niño pequeño, con algunos pequeños arreglos puede quedar muy chulo, y con una interfaz amigable que ponerle a DietPi, puede quedar bien.

3. Un servidor de impresión centralizado para impresoras que no tienen conexión local.

4. Hay otras muchas opciones interesantes, como un servidor de VPN, Home Assistant, emulador de juegos (que se ejecutan en Retropie o alguna otra plataforma similar), servidor de Minecraft, entre otras opciones, pero estos servicios requieren de mayor capacidad de cómputo, y tendrías que usar una raspi superior (3 o superior).

Por ahora, con esta que tenía por ahí perdida he decidido por montarle un PiHole para probar como funciona, y a ver si va tan bien como se dice en Internet.

Y esto sería todo! En otro post continuaré contando como es el proceso de instalación del PiHole, como lo he configurado y como funciona, para que podáis valorar si os interesa o no. 🚀