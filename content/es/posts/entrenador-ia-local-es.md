---
title: "Construí mi propio entrenador personal con IA local — y lo uso a diario"
date: 2026-05-16
draft: false
tags:
  - ia
  - n8n
  - ollama
  - fitness
  - homelab
  - telegram
  - self-hosting
categories:
  - Tecnología
summary: "Cómo monté un asistente de fitness privado con Ollama, n8n y Telegram, corriendo en mi propio servidor en casa, sin pagar suscripciones ni compartir mis datos con nadie."
---

Cuando te apuntas al gym te dan una rutina genérica, que va en una aplicación de móvil, y no tiene en cuenta tu progreso, lesiones, tu estado ese día, y además esta información no se tiene en cuenta para planear el siguiente día. ¿Y si un día no te apetece ir? Para evitarlo lo ideal es tener un agente de IA que tenga en cuenta toda esa información para planificar tu siguiente rutina y que te empuje y te anime a entrenar esos días que no te apetece. Todo esto, que sea gratuito y, por supuesto, privado.

La respuesta para mí fue que por supuesto. Así que decidí construirme el mío propio, ¡y aprender por el camino!

## La idea

Quería un asistente de entrenamiento que me conociera: mis objetivos, mi historial, lo que había hecho la semana anterior, si había dormido mal, si estaba en fase de volumen o de definición. Y que, en función de todo eso, me propusiera un plan para el día. Además que sea capaz de enviarme mensajes de Telegram motivadores recordándome que es hora de entrenar, y pidiéndome que le responda.

Nada que no pueda hacer un entrenador humano. Pero quería que lo hiciera un agente de IA que yo controlara completamente, sin enviar mis datos personales a ningún servidor externo, y sin pagar una suscripción mensual más.

Al agente resultante lo llamé **Carson** (sí, como el sirviente de Downton Abbey) — mi entrenador personal de IA local.

## Los ingredientes

Aquí pongo una lista lo más sencillo posible:

**Ollama** es una herramienta que permite ejecutar modelos de inteligencia artificial directamente en tu propio ordenador o servidor, sin conexión a internet. Funciona como el motor de la IA — el cerebro de Carson. Usé el modelo `llama3.2:3b`, una versión compacta que puede correr en hardware modesto.

**n8n** es una plataforma de automatización visual, parecida a Zapier o Make, pero que puedes instalar en tu propio servidor. Con ella diseñé todos los flujos de trabajo: cuándo se ejecuta el bot, qué información recoge, cómo se lo pasa a la IA, y qué devuelve al usuario.

**PostgreSQL** es una base de datos donde se guarda todo: el perfil del usuario, las sesiones de entrenamiento registradas, el historial de planes generados.

**Telegram** es la interfaz. No hice ninguna app — simplemente creé un bot de Telegram con el que hablo para registrar entrenamientos, pedir mi plan del día o recibir recordatorios.

**Proxmox** es el sistema que tengo en casa para virtualizar servidores. Todo esto corre en contenedores ligeros dentro de mi propio servidor doméstico, sin depender de ningún proveedor en la nube.

**Cloudflare Tunnel** — y aquí viene el detalle que nadie te cuenta.

Cuando instalas n8n en tu propio servidor, está completamente aislado del exterior. No tiene ninguna URL pública. Tu router no sabe que existe, tu IP de casa cambia cada cierto tiempo, y aunque quisieras abrir un puerto en el router para que Telegram llegara hasta él, estarías exponiendo tu red doméstica directamente a internet.

El problema es que Telegram funciona con webhooks: cuando alguien le escribe al bot, Telegram necesita llamar a una URL tuya para entregarle el mensaje. Si n8n no tiene URL pública, el bot no recibe nada.

Cloudflare Tunnel resuelve esto de forma elegante. En lugar de que el exterior entre a tu servidor, es tu servidor el que abre una conexión saliente hacia Cloudflare, que actúa como intermediario. El resultado es una URL pública estable con HTTPS incluido, sin abrir puertos, sin exponer tu IP, y de forma completamente gratuita para este caso de uso.

Es un paso que no aparece en la mayoría de tutoriales de n8n con Telegram, y que hace que mucha gente se atasque sin saber por qué su bot no responde.

## Cómo funciona en la práctica

Cuando me levanto por la mañana, Carson ya sabe lo que hice ayer. Si anoté algún deporte en Strava (correr, bici, tenis...), lo tiene en cuenta. Y además tiene en cuenta cuál fue el último entrenamiento y la progresión.

El flujo básico es este:

```
1. Recuperar perfil y objetivos actuales desde PostgreSQL
2. Consultar historial reciente de entrenamientos (últimos 7 días)
3. Obtener la última actividad de Strava (distancia, esfuerzo, tipo)
4. Construir un prompt con todo ese contexto
5. Enviarlo a Ollama → obtener el plan de entrenamiento
6. Entregar el plan al usuario por Telegram
```

![Imagen Workflow n8n](/images/n8nworkflow.webp)

También tiene un flujo de onboarding cuando alguien nuevo empieza a usarlo: le hace una serie de preguntas para entender sus objetivos, nivel, equipamiento disponible y preferencias. Todo eso queda guardado y la IA lo usa en cada interacción posterior.

Cuando llega la hora de entrenar me llegan una serie de mensajes motivadores desde el agente de IA para animarme a entrenar ese día.

## Lo que aprendí

**Automatizar flujos complejos es más accesible de lo que parece.** n8n tiene una curva de aprendizaje razonable y permite conectar piezas muy distintas sin escribir apenas código. La parte más interesante fue diseñar bien el contexto que se le pasa a la IA — si el prompt es vago, la respuesta también lo es.

**Los modelos pequeños tienen límites claros.** `llama3.2:3b` es impresionante para el hardware que consume, pero la verdad es que sus respuestas son genéricas y por regla general no respeta del todo el formato que le pides. Si quieres más calidad, necesitas un modelo mayor, y eso requiere más potencia de hardware.

**n8n es más potente de lo que parece a primera vista.** El editor visual de flujos permite iterar muy rápido. Lo difícil no fue montar la automatización — fue diseñar bien el modelo de datos y el flujo de conversación para que el contexto se acumulara correctamente con el tiempo.

**El coste de tener privacidad.** Controlar todo en local significa que tú eres el administrador del sistema. Si algo falla, lo tienes que arreglar tú. Es el precio de la soberanía sobre tus datos.

## ¿Merece la pena montarlo?

Si eres una persona técnica con curiosidad por la IA y el autoalojamiento, absolutamente sí. No solo por el resultado final, sino por todo lo que aprendes en el proceso: automatización, modelos de lenguaje, diseño de prompts, integración de APIs.

Si buscas un entrenador de IA sin complicaciones técnicas, probablemente una herramienta como ChatGPT con un buen prompt te dé resultados comparables con mucho menos esfuerzo.

Pero donde Carson no tiene rival es en privacidad y coste a largo plazo. Mis datos de entrenamiento no salen de casa. No hay suscripción que cancelar, no hay costes de API, y no hay unas condiciones de servicio que cambien de la noche a la mañana.

## El workflow completo

Si quieres montarlo tú mismo, aquí tienes el workflow listo para importar en n8n:

📥 [Descarga el workflow (JSON)](/files/AI_personal_trainer_public.json)

Una vez que lo hayas importado tendrás que reconectar tu Telegram, Ollama, PostgreSQL, y tus credenciales de Strava. El nodo **Onboarding** apunta al workflow de Onboarding que lo dejo sin subir para que puedas practicar.
