---
layout: default
title: Propuestas de proyecto
nav_order: 2
---

# Dos formas de darle al trabajador algo que no tiene
{: .fs-9 }

Dos conceptos de dispositivo corporal para entornos de trabajo, listos para discutirse y para formar equipo. Ambos parten del mismo criterio y llegan a soluciones muy distintas.
{: .fs-6 .fw-300 }

**Asesorías:** Ingeniería · Diseño
**Disciplinas:** Mecatrónica y Sistemas Ciberfísicos · Diseño de Modas · Diseño Industrial
**Estado:** Concepto en discusión

---

## Índice
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## El criterio que generó ambas propuestas

> **El dispositivo debe darle al usuario una capacidad que no tiene, no información sobre sí mismo que ya conoce.**

Antes de estos dos conceptos se exploraron varios de dosimetría —exposición a radiación UV en trabajo a la intemperie, ruido ocupacional, vibración mano-brazo—. Todos responden a riesgos reales y todos son construibles. Se descartaron por un defecto compartido: el dispositivo advierte al trabajador sobre su propia conducta, en situaciones donde con frecuencia no controla su horario ni su exposición. El resultado es una prenda de uso obligado que interrumpe y se percibe como monitoreo.

**La prueba que aplicamos:** si el dispositivo dejara de funcionar, ¿el usuario lo notaría y lo reclamaría? Si la respuesta es no, se diseñó para el patrón y no para él. Los dos conceptos siguientes pasan esa prueba.

---

## Concepto A — SEÑA

Enlace no auditivo para llamarse entre compañeros en un entorno ruidoso.
{: .fs-5 .fw-300 }

![Dos trabajadores con chaleco: uno presiona un botón y su chaleco se ilumina; el otro recibe una vibración y voltea.](/assets/img/chaleco.jpg)

Representación conceptual. El botón, el aviso háptico y la señal luminosa son elementos de la prenda; el fondo industrial es ilustrativo.
{: .fs-2 .text-grey-dk-000 }

### El problema

Llamar la atención de un compañero a distancia, en un taller ruidoso, hoy se resuelve gritando, aventando algo, caminando hasta la persona o esperando a que voltee por casualidad. La primera falla con ruido o con protección auditiva puesta. La segunda es peligrosa. La tercera interrumpe la tarea. La cuarta es azarosa. Para un trabajador sordo, la primera ni siquiera existe.

### Cómo funciona

1. **Llamas.** Presionas un botón en tu propio chaleco.
2. **El otro lo siente.** Su chaleco vibra. No necesita ver ni oír nada.
3. **Voltea a buscar.** El giro de cabeza es el gesto natural.
4. **Te identifica.** Tu chaleco está iluminado: sabe quién lo llamó, sin ambigüedad.

### Decisión de diseño central

Los dos canales están separados por función: **el háptico dice «alguien te llama», el visual dice «quién»**. Eso elimina la necesidad de vibración direccional —el problema técnico más caro de este tipo de sistemas, que normalmente exige radiolocalización de precisión—. La dirección la resuelve el cuerpo del usuario al voltear, no el dispositivo.

### Qué aporta cada disciplina

**Modas / Textil**

- Dónde colocar los actuadores: la sensibilidad a la vibración cambia mucho según la zona del cuerpo
- Acoplar el motor al cuerpo: sobre tela suelta la vibración se pierde
- Difusión de la luz visible en 360°, en interior y de día
- Botón accionable con guante, sin activarse solo
- Lavado, desmontaje de la electrónica, durabilidad

**Diseño Industrial**

- Encapsulado y conectores desmontables
- Interfaz física del control
- Ruteo interno y puntos de fatiga

**Mecatrónica / SCF**

- Radio punto a punto entre prendas, sin infraestructura instalada
- Direccionamiento y colisiones (dos llamados a la vez)
- Patrones de vibración distinguibles
- Energía para jornada completa

### Ficha rápida

| | |
|:---|:---|
| **ODS** | **8** Trabajo decente · **10** Reducción de desigualdades |
| **Usuario** | Cuadrillas de 2 a 6 personas en taller, mantenimiento, montaje u obra. Caso prioritario: cuadrillas mixtas con integrantes sordos o hipoacúsicos. |
| **Alcance del semestre** | Dos o tres nodos funcionales. Vocabulario mínimo de dos señales. Sin app, sin conectividad externa. |

### Encuadre y límites

**No se presenta como «un aparato para que las personas sordas no se accidenten».** Ese encuadre pone a la persona como el defecto a corregir. Se presenta al revés: el entorno industrial señaliza casi todo por audio, y esta prenda corrige el entorno. De paso sirve a cualquiera con tapones puestos.

**No es un sistema de seguridad ni de emergencia.** Declararlo así implicaría requisitos de confiabilidad y certificación imposibles de cumplir en un semestre. Es un canal de comunicación cotidiana, y esa modestia es deliberada.

**Hay que hablar con usuarios sordos antes de fijar el concepto.** Existe un historial largo de tecnología asistiva diseñada por equipos oyentes que la comunidad rechazó por partir de premisas equivocadas. Si nos dicen que su problema prioritario es otro, cambiamos de objetivo con evidencia y el proyecto se fortalece.

---

## Concepto B — TIENTO

Extensión sensorial de dos dedos para diagnóstico de mantenimiento.
{: .fs-5 .fw-300 }

![Mano con dos dedos instrumentados acercándose a un tablero eléctrico sin tocarlo: un dedo mide temperatura y el otro detecta tensión.](/assets/img/guantes.jpg)

Representación conceptual. El hueco entre los dedos y la superficie es intencional: la verificación ocurre antes del contacto.
{: .fs-2 .text-grey-dk-000 }

### El problema

Cuando un técnico se acerca a un tablero o a un equipo desconocido, no puede saber por inspección visual si una superficie está caliente o si un conductor está energizado. Las estrategias actuales son heurísticas y peligrosas: tocar rápido con el dorso de la mano, acercarla para sentir el calor, o asumir el estado del circuito. **El método humano de verificación es el contacto, y el contacto es justamente lo que se quiere evitar.**

### Cómo funciona

1. **Índice:** temperatura de la superficie, sin contacto, por infrarrojo.
2. **Pulgar:** indicio de conductor energizado, sin contacto, por campo eléctrico.
3. **Respuesta háptica** en la base de la mano, con patrones inconfundibles entre calor y tensión.

### Por qué corporal y no una herramienta de mano

Ya existen el termómetro infrarrojo y la pluma detectora de tensión. El diferencial no es la medición: **es el gesto**. La herramienta exige que el técnico *decida* verificar, saque el aparato y ocupe una mano. El dispositivo corporal verifica siempre, en el mismo movimiento de extender la mano, antes del contacto y sin interrumpir la tarea. Si no defendemos ese punto, la pregunta «¿y por qué no una pluma?» no tiene respuesta.

### Qué aporta cada disciplina

**Modas / Textil**

- El conflicto central: **destreza contra instrumentación**. Todo lo que se agrega al dedo resta tacto y agarre
- Patronaje de dos dedos: sujeción sin compresión y sin migración
- Conductores flexibles sin puntos de fatiga
- Separar el actuador de la yema para no contaminar el tacto natural
- Transpirabilidad, resistencia a grasa y abrasión

**Diseño Industrial**

- Encapsulado en punta de dedo
- Ventana óptica y orientación del sensor
- Módulo desmontable para lavado

**Mecatrónica / SCF**

- Acondicionamiento de señal de ambos sensores
- Umbrales y discriminación de patrones hápticos
- Autodiagnóstico y arquitectura a prueba de fallos

### Ficha rápida

| | |
|:---|:---|
| **ODS** | **8** Trabajo decente · **3** Salud y bienestar |
| **Usuario** | Técnico de mantenimiento en fase de *diagnóstico*. No es el electricista trabajando en tensión con guante dieléctrico: ahí un wearable interfiere con la protección aislante. |
| **Alcance del semestre** | Un prototipo, una mano, una talla. Pruebas en banco con fuentes controladas, nunca en instalaciones en servicio. |

### Principio no negociable

Los detectores de tensión sin contacto tienen fallas conocidas: no ven corriente directa, no atraviesan blindaje ni tubo metálico, dan falsos positivos por cables vecinos y dependen de que el usuario esté aterrizado. **Un falso negativo puede matar a alguien.** De ahí:

- Solo existen dos estados: **«detecto tensión»** y **«no detecto tensión»**. Nunca «seguro».
- La ausencia de señal debe distinguirse de la falla del aparato: autodiagnóstico y aviso inequívoco de batería agotada.
- No sustituye el bloqueo y etiquetado ni la verificación con instrumento adecuado. Es una capa previa de conciencia situacional.
- Los patrones de «caliente» y «energizado» deben ser imposibles de confundir entre sí.

Que el dispositivo declare honestamente su incertidumbre en vez de aparentar certeza es lo más valioso que tiene esta propuesta.

---

## En qué se diferencian

Ninguno es «el bueno». Tienen perfiles de dificultad distintos y conviene elegir con eso en la mano.

| | A — SEÑA | B — TIENTO |
|:---|:---|:---|
| Dificultad textil | Media-alta | **Alta** — destreza vs. instrumentación |
| Dificultad electrónica | Media — red multinodo | Media — acondicionamiento de señal |
| Riesgo si falla | Bajo | **Alto** — obliga a diseño a prueba de fallos |
| Novedad frente a lo existente | Media-alta | Media — compite con una herramienta establecida |
| Depende de acceso a usuarios | **Alta** — usuarios sordos, cuadrilla real | Media — técnicos accesibles |
| Se puede demostrar en vivo | **Sí** — bastan dos personas | Parcial — requiere banco de pruebas |

---

## Qué perfiles hacen falta

Cualquiera de los dos conceptos necesita las tres carreras. No hay una que «apoye» a otra: en ambos casos, si falta una disciplina, el proyecto deja de tener sentido.

**Diseño de Modas.** Patronaje de prenda técnica, integración de componentes en textil, selección de materiales, lavado y durabilidad. Es la disciplina que decide si el dispositivo se usa o se abandona.

**Diseño Industrial.** Encapsulados, interfaz física, ergonomía del control, módulos desmontables, y la representación visual del sistema.

**Mecatrónica / Sistemas Ciberfísicos.** Sensores y acondicionamiento, comunicación entre nodos, firmware, actuadores hápticos, energía y pruebas.

**Trabajo de campo (cualquier carrera).** Contacto con usuarios, entrevistas, consentimiento informado, documentación de las pruebas. Es la tarea más urgente de las primeras dos semanas y la que más determina si el proyecto tiene fundamento o solo una buena suposición.

### Lo que hay que resolver antes de comprometerse

- **Acceso a usuarios.** Para A: vínculo con asociaciones de personas sordas o intérpretes de LSM. Para B: técnicos de mantenimiento dispuestos a probar. Sin esto, ambos conceptos se quedan en suposición bien redactada.
- **Ética y permisos.** Si hay pruebas con usuarios externos, hace falta consentimiento informado y probablemente aval institucional. El trámite se empieza ahora, no en la semana diez.
- **Presupuesto y proveeduría.** Determina qué radio y qué actuadores son viables. Ningún componente está confirmado en precio ni disponibilidad.

### Preguntas abiertas para las asesorías

- ¿Hay forma de caracterizar umbral y discriminación de vibración por zona del cuerpo en el laboratorio, o se resuelve empíricamente con sujetos?
- Para B: ¿qué se considera evidencia suficiente de comportamiento a prueba de fallos en un prototipo académico? ¿Hay banco de pruebas con fuentes controladas?
- ¿Existe vínculo institucional con asociaciones de personas sordas, y cuál es el tiempo de trámite de un aval de ética?
- ¿Qué alcance esperan como entregable: prototipo funcional completo o demostración de subsistemas?

---

Documento de trabajo. Los datos técnicos de componentes y arquitectura son hipótesis, no especificaciones verificadas. Ningún componente ha sido confirmado en disponibilidad, precio ni desempeño. Las referencias a normatividad deben verificarse en fuente oficial antes de citarse. Las imágenes son representaciones conceptuales generadas para comunicar la idea, no diagramas técnicos.
{: .fs-2 .text-grey-dk-000 }