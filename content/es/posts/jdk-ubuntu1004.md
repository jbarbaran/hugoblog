+++
date = '2013-03-16T16:00:00+01:00'
draft = false
title = 'Instalar Java JDK en Ubuntu 10.04 Lucyd Lynx'
lang=  'es'
+++

Hace poco que he formateado mi Ubuntu y al volver a instalar todo, no he podido encontrar fácilmente la maquina virtual de Java. Intenté instalarlo con el nombre jdk, pero me decía que el paquete hacia referencia a otro y que probablemente estaba obsoleto. ¿Que hice después, irme a la página de Oracle desde donde descargué el último JDK, pero a la hora de asociarlo con mi Ubuntu, tampoco funcionaba. Total, que después de un buen rato buscando en Internet, estoy viendo que ha cambiado de nombre y no es JDK, sino que es,  sorpresa!!, OpenJDK.

Así que os dejo aquí los pasos que tuve que hacer para instalarlo:

(Tened en cuenta que en el momento de escribir este post la versión de Java era la 6, asi que tenedlo en cuenta y poned el número de la versión que querais instalar)

```sudo apt-get install openjdk-6-jdk```

```apt-cache search jdk```

Con estas dos sencillas opciones se instala. Espero que os sirva.