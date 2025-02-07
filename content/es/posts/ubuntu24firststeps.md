+++
date = '2024-10-26T16:00:00+01:00'
draft = false
title = 'Primeros pasos en Ubuntu 24.04 recién instalado'
+++

Acabo de instalar Ubuntu 24.04 LTS en un MacBook Air 2015 para darle una segunda vida. ¡Sorpresa! Todas las funcionalidades del portátil funcionan a la perfección: trackpad, WiFi, teclas de funciones especiales, e incluso el modo de reposo al cerrar el equipo. Además, la velocidad del sistema es notablemente superior comparada con macOS.

Ahora que tengo Ubuntu recién instalado, ¿qué pasos debo seguir? Tras investigar en Internet, he recopilado algunas de las primeras tareas recomendadas:

Actualizar el sistema
Mantener el sistema actualizado es muy importante para obtener los últimos parches de seguridad y un sistema optimizado.

**1. Primero, actualiza el índice de paquetes local ejecutando el siguiente comando desde el terminal:**

```sudo apt update```

Después, actualiza los paquetes a sus últimas versiones con este comando:

```sudo apt upgrade```

Finalmente, abre el Centro de Aplicaciones para verificar si alguna aplicación tiene una actualización pendiente.


**2. Familiarizarte con el entorno GNOME**
Aunque he usado Linux y, en concreto, Ubuntu anteriormente, suelo utilizar Windows a diario. GNOME es muy intuitivo, pero es buena idea dedicar un rato a explorar sus opciones, menús, aplicaciones y en general familiarizarse con el sistema.

**3. Personaliza el escritorio**
En el menú de Configuración encontrarás herramientas para personalizar el sistema. Especialmente útil es la sección de Apariencia, donde puedes activar el modo oscuro, cambiar el fondo de pantalla y ajustar la posición de la barra de tareas.


**4. Instalar el software que te prefieras**
La forma más sencilla de instalar software es usando el Centro de Aplicaciones, donde encontrarás muchas aplicaciones populares.


En mi caso, ya he instalado algunas aplicaciones básicas que uso a menudo y añadiré más según las necesite:

- **Brave**, este navegador tiene muy buena pinta, y quiero probarlo. Por defecto Ubuntu trae Firefox.
- **Visual Studio Code**, es un entorno de desarrollo software muy versatil.
- **Element**, es un cliente de Matrix.

**5. Seguridad ante todo: habilitar y configurar el firewall**
Ubuntu incluye el firewall UFW (Uncomplicated Firewall), muy sencillo de configurar y eficaz. Para configurarlo, usa estos comandos en el terminal.

Para conocer el estado actual del firewall:

```sudo ufw status```

Por defecto, estará inactivo, así que actívalo con este comando:

```sudo ufw enable```

Luego, configura una política mínima, denegando por defecto las conexiones entrantes y permitiendo las salientes:

```sudo ufw default deny incoming```

```sudo ufw default allow outgoing```

A continuación, permite excepciones para navegar por Internet (habilitando los puertos HTTP y HTTPS) y para acceder al equipo por SSH:

```sudo ufw allow http```

```sudo ufw allow https```

```sudo ufw allow ssh```

**6. Otros pasos más avanzados**
En muchas guías se menciona la posibilidad de habilitar **Flatpak**, una alternativa a Snap, el gestor de paquetes por defecto de Ubuntu. Cada uno tiene sus ventajas e inconvenientes, así que es recomendable informarse bien antes de tomar una decisión. [En este enlace puedes encontrar más detalles sobres ambos sistemas de paquetes](https://itsfoss.com/flatpak-vs-snap/).

**Conclusión**
Mi última experiencia con Ubuntu fue hace unos años, y ahora que lo he retomado puedo decir que la evolución ha sido impresionante. Ha mejorado en compatibilidad con hardware más antiguo y hoy en día ofrece una experiencia mucho más pulida y eficiente. En mi caso, instalar Ubuntu en mi MacBook Air ha sido todo un acierto: el equipo ha pasado de ser poco usable a tener una velocidad, facilidad de uso y compatibilidad completas.

Con los pasos que hemos visto, ya tienes una buena base para empezar a experimentar con Ubuntu.
