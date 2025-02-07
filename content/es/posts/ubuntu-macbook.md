+++
date = '2024-10-25T16:00:00+01:00'
draft = false
title = 'Instalar Ubuntu en Macbook Air de 2015'
lang=  'es'
+++

**Tengo un MacBook Air de 2015** con un procesador Intel i5-5240U de 4 núcleos, 4 GB de RAM y un disco duro de 128 GB. Aunque siempre lo he actualizado a las últimas versiones de macOS (algo cada vez más complicado, por falta de espacio en disco), cometí el error de instalarle la versión de macOS Monterey. Esto hizo que el MacBook comenzara a ir terriblemente lento, hasta el punto de que apenas lo usaba… un pisapapeles, vaya.

La experiencia de instalación y uso de Ubuntu ha mejorado mucho desde la última vez que lo probé hace unos seis o siete años. El proceso es súper sencillo y eficaz. ¡Gracias a Ubuntu, mi portátil ha recuperado agilidad! Ahora os cuento el proceso de instalación paso a paso:

### Requisitos previos
- **El portátil/ordenador viejo** y algo de ganas.

- **Un pendrive de al menos 8 GB** (en mi caso usé uno de 128 GB, lo cual es más que suficiente).

### Paso a paso de la instalación de Ubuntu

**1. Descargar Ubuntu:** Primero, descarga la imagen de Ubuntu desde [su página oficial](https://ubuntu.com/download/desktop). Puedes hacerlo desde el mismo portátil (si es que sigue siendo usable) o desde otro ordenador, como hice yo.
**¿Qué versión elegir?** Te recomiendo la última versión LTS (Long-Term Support), que garantiza 5 años de actualizaciones de mantenimiento y seguridad. Yo instalé Ubuntu 24.04 LTS.

**2. Crear un USB de arranque:** La imagen descargada tiene formato .iso, que es una imagen de disco. Para crear el USB de arranque en Windows, utilicé el programa Balena Etcher, que es sencillo y efectivo. Este software básicamente sigue tres pasos:

![Balena Etcher](/images/balenaetcher.webp)

**- Seleccionar la imagen:** Haz clic en «Flash from file» y elige el archivo .iso que descargaste.

**- Seleccionar la unidad de destino:** Asegúrate de elegir el pendrive y no el disco duro de tu ordenador.

**- Iniciar el proceso:** Verifica que todo está correcto y haz clic en «Flash». La escritura y verificación pueden tardar unos minutos.

![Balena Etcher flashing](/images/balenaetcherflashing-c.webp)

### Instalación de Ubuntu en el MacBook

**1. Arrancar desde el pendrive:** Conecta el pendrive al MacBook (debe estar apagado), y enciéndelo mientras mantienes pulsada la tecla Option/Alt (⌥). Aparecerán los discos de arranque reconocidos, y debes seleccionar el icono del pendrive.

![Balena Etcher flasing](/images/startmacsystem-c.webp)

**2. Probar antes de instalar:** Una vez iniciado Ubuntu, te ofrece dos opciones: “Try” o “Install Ubuntu”. Te recomiendo elegir “Try” para verificar si reconoce todos los drivers importantes. En mi caso, ¡reconoció todo! Desde el teclado retroiluminado hasta el trackpad, el WiFi y las teclas especiales.

**3. Instalar Ubuntu:** Cuando estés listo, haz clic en el icono de instalación que aparece en el escritorio de Ubuntu. El proceso está totalmente guiado y es muy intuitivo. Sólo necesitas elegir el idioma, la configuración del teclado y la zona horaria. Tras unos minutos, Ubuntu estaba instalado y listo para usarse.

### Resultados y conclusiones

¡Y voilà! Mi MacBook Air ha vuelto a la vida. Con Ubuntu, su rendimiento es mucho más fluido, los tiempos de arranque son rápidos, y la experiencia de usuario es excelente. Aunque no pude comparar directamente el rendimiento porque no hice pruebas antes de formatear macOS, os aseguro que el cambio es increíble: ¡el portátil vuela!

Así que, si tienes un ordenador antiguo y crees que su única opción es reciclarlo o tirarlo, considera instalar Linux. Puedes darle una segunda vida con Ubuntu, Mint o cualquier otra distribución. Mi recomendación es Ubuntu, porque como os he contado, el proceso ha sido sencillo y el resultado, genial.

¡Hasta la próxima!