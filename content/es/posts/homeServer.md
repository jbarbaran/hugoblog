---
title: "Home server: Un mini PC, Proxmox y demasiadas ganas de cacharrear"
date: 2025-09-15
draft: false
tags:
  - homelab
  - self-hosting
  - proxmox
  - mini-pc
  - home-server
categories:
  - Tecnología
summary: "Cómo monté mi home server con un Beelink S12 Pro, por qué elegí Proxmox VE y qué servicios estoy ejecutando."
---

# Montando un home server con un mini PC

Tener un servidor en casa abre un montón de posibilidades. Llevo tiempo dándole vueltas a montar uno, y cuando finalmente me puse, me di cuenta de que la mitad del proceso era precisamente decidir qué comprar y qué instalar. ¿Y para qué sirve un home server? Hay ventajas evidentes como abaratar costes de SaaS o ganar en privacidad, pero hay una que me parece más importante que todas: *aprender*.

En este post cuento cómo fue el proceso, qué opciones valoré y por qué terminé montando mi servidor sobre un **Beelink S12 Pro** con **Proxmox VE**.

## ¿Por qué un mini PC?

Lo primero es decidir el formato. Las opciones más habituales son:

- **Un PC o portátil viejo del trastero.** Funciona, pero suele ser ruidoso, consume bastante energía y ocupa mucho espacio.
- **Una Raspberry Pi.** Pequeña y de consumo ridículo, pero limitada en RAM y potencia para correr varios servicios a la vez.
- **Un NAS.** Buena opción si el almacenamiento es el protagonista, pero el precio se dispara enseguida.
- **Un mini PC.** Pequeño, silencioso, bajo consumo, precio razonable. Ganador claro.

## Los mini PCs que valoré

En 2025 hay tres familias que tienen sentido para un homelab doméstico:

**Intel N95 / N100** es probablemente la opción más popular del momento para este tipo de uso. Eficiencia energética muy buena, soporte completo para virtualización y precios muy competitivos. El N100 tiene mejor eficiencia que el N95; el N95 gana algo en rendimiento bruto pero a costa de consumir un poco más. Hay multitud de fabricantes con modelos basados en estos chips, y la diferencia entre ellos suele ser mínima: lo que cambia es la conectividad, la RAM de serie y el soporte postventa.

**Mac Mini** merece una mención porque es de lo más conocido del mercado. Tienen un rendimiento muy bueno y un consumo sorprendentemente bajo pero para mi, el problema es el precio, aunque es claramente superior a un mini PC chino, no necesito algo así para empezar.

**Mini PCs fanless** (sin ventilador). Existen modelos completamente pasivos, y por tanto muy silenciosos. El inconveniente es que la refrigeración pasiva hace que el rendimiento no sea tan alto y además encarece bastante el precio.

## La decisión final: Beelink S12 Pro

Me decanté por el **Beelink S12 Pro** con un **Intel N100**. Las razones fueron bastante simples:

- Buena relación calidad-precio
- Consumo energético muy reducido
- 16 GB de RAM
- SSD NVMe incluido
- Tamaño compacto

![Imagen Beelink S12 Pro](/beelink.webp)

El lado malo es que no tiene GPU dedicada, así que el uso de modelos de IA está limitado a modelos pequeños que funcionan razonablemente con CPU. No es el mini PC más potente del mercado, pero sí uno de los más equilibrados para un homelab doméstico. Mi objetivo no es montar una infraestructura empresarial, sino ejecutar varios servicios de forma estable y eficiente.

## El sistema operativo

Con el hardware decidido, el siguiente dilema era qué sistema operativo instalar. Básicamente hay tres caminos:

**Linux + Docker directamente** es la opción más popular y la más sencilla de entender. Instalas Debian o Ubuntu Server, levantas los servicios con Docker Compose y listo. Debe funcionar muy bien, pero todo convive en el mismo host: si algo se tuerce a nivel de sistema operativo, todo cae junto.

**TrueNAS o Unraid** son interesantes cuando vas a usar NAS casero y lo que priorizas es tu nube personal de datos. No es mi caso.

**Proxmox VE** no lo conocía para nada, y sin embargo es lo que elegí. Es una plataforma de virtualización basada en Debian que permite gestionar máquinas virtuales y contenedores LXC ([LinuX Containers](https://en.wikipedia.org/wiki/LXC)) desde una interfaz web bastante cómoda. La principal ventaja es el aislamiento: cada servicio vive en su propio entorno. Si rompo un contenedor, el resto sigue funcionando. Si quiero experimentar con algo raro, lo hago en una VM desechable y la elimino sin rastro.

No es la opción más sencilla de primeras, pero la curva de aprendizaje se paga sola.

## Instalando Proxmox VE

El proceso de instalación es más sencillo de lo que parece. Proxmox distribuye su sistema como una imagen ISO, así que el primer paso es grabarlo en un pendrive.

Desde Windows, la herramienta más cómoda para esto es **Balena Etcher**: descargas la ISO desde la web oficial de Proxmox, abres Etcher, seleccionas la imagen y el pendrive, y en pocos minutos tienes el instalador listo. Sin complicaciones.

Con el pendrive preparado, toca arrancar el Beelink desde él. Para eso hay que acceder al menú de arranque del sistema: en el caso del S12 Pro se hace pulsando la tecla correcta justo al encender el equipo (en mi caso **F7**, aunque puede variar según el modelo). Desde ahí seleccionas el pendrive como dispositivo de arranque y el instalador de Proxmox arranca solo.

El instalador en sí es un asistente gráfico bastante guiado. Básicamente te pide:

- El disco donde instalar el sistema
- La configuración de red (IP, puerta de enlace, DNS)
- El nombre del nodo
- Una contraseña para el usuario root

En diez minutos el sistema está instalado. Retiras el pendrive, el equipo reinicia y ya puedes acceder a la interfaz web de Proxmox desde cualquier navegador en tu red local, en la dirección `https://IP-del-servidor:8006`.

![Fin de la instalacion de Proxmox VE](/proxmox-finished.webp)

A partir de ahí, todo se gestiona desde el navegador.

## Conceptos clave de Proxmox VE

Antes de empezar a desplegar servicios, hay dos conceptos que merece la pena entender bien.

### LXC: contenedores ligeros

Los contenedores LXC comparten el kernel del sistema anfitrión. Esto implica menor consumo de memoria, menor uso de CPU y arranque prácticamente instantáneo. Son ideales para servicios ligeros como servidores DNS, proxies o cualquier aplicación que no requiera un entorno muy específico.

### VM: máquinas virtuales completas

Las máquinas virtuales funcionan de forma completamente independiente, con su propio kernel y sistema operativo. A cambio, consumen más recursos y tardan más en arrancar. Son la opción cuando necesitas máxima compatibilidad o aislamiento, o cuando la propia aplicación lo recomienda.

### ¿Qué uso yo?

Mi enfoque es sencillo:

> Si algo puede ejecutarse cómodamente en un LXC, lo ejecuto en un LXC.

Solo utilizo máquinas virtuales cuando realmente aportan alguna ventaja clara.

## Cómo tengo organizado el servidor

Mi estructura actual es bastante sencilla:

```text
proxmox-node
│
├── CT 100 - alpine-adguard
├── CT 101 - tailscale
├── CT 102 - n8n
├── CT 103 - ollama
├── CT 104 - postgresql
│
└── VM 200 - home-assistant
```

Servicios ligeros en LXC, servicios más complejos en VM. Esto mantiene el consumo de recursos bajo control y simplifica la administración.

## Servicios que estoy ejecutando

### AdGuard Home

Actúa como servidor DNS para toda mi red doméstica. Bloquea publicidad a nivel de red, filtra dominios y da visibilidad sobre el tráfico DNS de todos los dispositivos conectados. Desplegado en un contenedor LXC, apenas consume recursos.

### Home Assistant

Siempre me ha llamado la atención, es un sistema de gestión de domótica. Este software donde brilla es con las automatizaciones, y también recoge datos de sensores y se integra prácticamente con cualquier dispositivo que tengas en casa. En este caso he usado una máquina virtual porque es lo recomendado por Home Assistant.

### Tailscale

Se trata de una VPN que me permite acceder a mi red doméstica desde cualquier lugar sin abrir puertos. La configuración es muy sencilla y las conexiones son seguras.

### n8n

[n8n](https://n8n.io) es una herramienta de automatización de flujos de trabajo, similar en concepto a Zapier o Make, pero que puedes alojar tú mismo. Permite conectar aplicaciones y servicios entre sí mediante una interfaz visual de nodos, y construir automatizaciones bastante complejas sin necesidad de escribir código. Es uno de los servicios que más me está dando juego últimamente.

### PostgreSQL

La mayoría de los servicios que monto acaban necesitando algún tipo de base de datos, y prefiero tener una instancia centralizada de PostgreSQL en lugar de levantar una base de datos por contenedor. Es más limpio, más fácil de mantener y los backups se simplifican bastante.

### Ollama

Con todo el hype que hay ahora con la IA, quiero experimentar con IA en local. He instalado Ollama con algunos modelos pequeños para hacer pruebas. Una cosa importante: un Intel N100 no está pensado para este uso. Sin embargo, es perfectamente válido para aprender, hacer pruebas y ejecutar modelos ligeros, todo ello sin depender de servicios externos.

## Lo que podría añadir más adelante

Como ocurre con cualquier homelab, la lista de ideas nunca termina. Algunas posibilidades que tengo en el radar:

- Jellyfin
- Nextcloud
- Grafana + Prometheus
- Immich
- Nginx Proxy Manager

De momento intento mantener las cosas simples y añadir nuevos servicios únicamente cuando realmente los necesito. Y siendo realistas mi Beelink no puede con más, pero quizás a futuro dé el salto a un dispositivo más potente o directamente a un NAS... ¡quien sabe!

## Conclusión

Montar un home server con un mini PC ha resultado ser mucho más sencillo de lo que esperaba. El Beelink S12 Pro ofrece potencia suficiente para una gran cantidad de servicios domésticos, y Proxmox VE aporta la flexibilidad necesaria para experimentar sin miedo a romper nada.

La combinación de ambos me ha permitido construir una plataforma estable, eficiente y, sobre todo, divertida de mantener. Y como suele ocurrir en el mundo del self-hosting, una vez empiezas siempre aparece un nuevo servicio que probar.