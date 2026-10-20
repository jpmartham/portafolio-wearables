---
layout: default
title: Diseño Digital — Técnica y electrónica
nav_order: 5
---

# Diseño Digital — Diseño en técnica y electrónica
{: .fs-9 }

Sistema eléctrico del chaleco térmico: potencia, lógica y actuadores
{: .fs-6 .fw-300 }

**Equipo:** JPM · JPB | Diseño de Moda · Ingeniería Mecatrónica
**Alcance:** sistema final propuesto. Lo validado en banco y lo pendiente se marcan por separado (sección 9).

---

## Índice
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## 1. Arquitectura general

El chaleco usa un **pod de control central** en la espalda media y **módulos térmicos pasivos** conectados por un arnés desmontable.

- **Pod:** concentra toda la electrónica activa (microcontrolador, drivers, MOSFETs y convertidor de voltaje).
- **Módulos:** solo contienen la celda Peltier, el ventilador, el termostato de seguridad y los sensores de temperatura.

Razones de la decisión:

- **Lavabilidad.** Al desconectar los cuatro conectores del pod (J1–J4) y el XT30 de la batería, el textil queda sin electrónica y se puede lavar.
- **Peso y cableado.** Un microcontrolador por módulo triplicaría el firmware y la sincronización sin ganar función. Con el pod en la espalda, la batería en la espalda baja y los módulos en la nuca, los tramos de cable son cortos y no cruzan el torso, salvo el ramal al pecho.
- **Pruebas independientes.** Cada módulo se prueba conectándolo solo al pod, sin rediseñar nada.

---

## 2. Esquema eléctrico

![Diagrama del sistema eléctrico en tres columnas: potencia (batería 2S, BMS, fusible, interruptor, XT30), pod de control (bus VBAT, MP1584, bus de 5 V, ESP32-C3 con asignación de pines, dos L298N en paralelo, tres MOSFET y conectores J1–J4) y actuadores (módulos Peltier A y B en la nuca, película calefactora en el pecho, interfaz en el tirante).]({{ '/assets/img/04-diseno-tecnico/sistema-electrico.svg' | relative_url }})

Figura 1. Esquema de bloques del sistema. GND no se dibuja: todo retorna al punto único de tierra del pod. [Abrir en tamaño completo]({{ '/assets/img/04-diseno-tecnico/sistema-electrico.svg' | relative_url }}).
{: .fs-2 .text-grey-dk-000 }

---

## 3. Potencia

| Etapa | Componente | Función |
|---|---|---|
| Fuente | 2× 18650 en serie (2S), espalda baja | 7.4 V nominales; rango de 6.0 a 8.4 V |
| Gestión | BMS 2S 10 A con balanceo | Corta por sobrecarga, descarga profunda y cortocircuito, y balancea las celdas |
| Protección | Fusible 5 A | Protege el arnés si falla el BMS |
| Mando | Interruptor 5 A | Corte general |
| Desconexión | XT30 | Permite retirar la batería para cargar y lavar |
| Distribución | Bus VBAT dentro del pod | Alimenta los L298N, el MP1584, la película del pecho y el divisor de medición |
| Regulación | MP1584 ajustado a 5.00 V | Bus de 5 V: lógica de los L298N, ventiladores, ESP32 (vía 1N5819) y LED |

**Reglas de potencia**

- **Calibre de cable:**
  - 20 AWG de silicón en las líneas de las Peltier.
  - 24 AWG en ventiladores y sensores.
- **Jumper 5V-EN del L298N:** se retira. Su regulador interno no se usa; los 5 V llegan desde el MP1584.
- **Diodo 1N5819 al ESP32:** va entre el bus de 5 V y el pin 5V del ESP32. Evita alimentar la laptop por el USB cuando ambos están conectados.
- **Tierra en estrella:** un solo punto de unión. La potencia y los sensores no comparten conductor de retorno.

---

## 4. Lógica y control

Controlador: **ESP32-C3 SuperMini**. Se eligió porque el asesor tiene experiencia con él y porque genera su propia red WiFi para la interfaz.

### Asignación de pines

| GPIO | Señal | Va a | Nota |
|---|---|---|---|
| 0 | VBAT ÷ 4.17 | Divisor 100k / 31.5k | ADC; 8.4 V → ~2.0 V |
| 5 | EN_A (PWM) | L298N A · ENA y ENB unidos | Potencia del módulo A |
| 6 | IN_A1 | L298N A · IN1 e IN4 | Dirección (frío/calor) |
| 7 | IN_A2 | L298N A · IN2 e IN3 | Dirección (frío/calor) |
| 20 | FAN_A | Gate de Q1 | Enciende o apaga el ventilador A |
| 4 | EN_B (PWM) | L298N B · ENA y ENB unidos | Potencia del módulo B |
| 3 | IN_B1 | L298N B · IN1 e IN4 | Dirección |
| 1 | IN_B2 | L298N B · IN2 e IN3 | Dirección |
| 21 | FAN_B | Gate de Q2 | Ventilador B |
| 2 | HEAT (PWM) | Gate de Q3 | Película del pecho, solo calor |
| 10 | 1-Wire | 5× DS18B20 en un bus | Pull-up de 4.7–5.1 kΩ a 3V3 |
| 9 | BTN | Botón, interfaz B | Usa el pull-up del botón BOOT |
| 8 | LED | WS2812, interfaz B | Pin de arranque; sin carga a GND |

Se usan los 13 GPIO disponibles. No queda pin libre para un motor de vibración: si se quiere, hace falta un expansor I2C, y eso queda fuera de esta versión.

**Pines de arranque (2, 8 y 9).** El ESP32-C3 lee estos pines al encender. La asignación de arriba los usa en funciones compatibles. Sin embargo, el pull-down de 10 kΩ en el gate de Q3 (GPIO2) es una **hipótesis**: hay que verificar en banco que el flasheo por USB sigue funcionando. Si falla, se sube ese pull-down a 100 kΩ.

### Drivers

- **Un L298N por módulo Peltier.** Los dos puentes de cada placa trabajan en paralelo, según la conexión que indica la hoja de datos de ST para corrientes mayores (canal 1 con 4, canal 2 con 3):
  - Las entradas se unen IN1=IN4, IN2=IN3 y ENA=ENB.
  - Las salidas se unen OUT1‖OUT4 y OUT2‖OUT3.
- **Efecto del paralelo:** la corriente se reparte entre los dos puentes y cada uno trabaja con menos estrés térmico.
- **Q1–Q3:** MOSFET de canal N con nivel lógico (Vgs(th) < 2.5 V), en configuración low-side.
  - Llevan una resistencia de 10 kΩ de gate a GND para que arranquen apagados.
  - Los ventiladores llevan un diodo flyback 1N5819.

### Interfaz de usuario

Se mantienen dos objetivos. El que resulte realista se elige conforme avance el proyecto.

| Objetivo | Cómo funciona | Estado |
|---|---|---|
| **A · WiFi** | El ESP32 levanta su propia red; en 192.168.4.1 se elige modo (frío/calor/STOP), setpoint y potencia máxima | Funciona en banco |
| **B · Físico** | Botón y LED WS2812 en el tirante frontal izquierdo, conectados al pod por J4 | Propuesto, no validado |

---

## 5. Actuadores y sensores

| Conector | Destino | Tipo | Conductores |
|---|---|---|---|
| J1 | Módulo A · nuca izquierda | XT30 + JST-XH 5p | TEC+ · TEC− / FAN+ · FAN− · 3V3 · DQ · GND |
| J2 | Módulo B · nuca derecha | XT30 + JST-XH 5p | TEC+ · TEC− / FAN+ · FAN− · 3V3 · DQ · GND |
| J3 | Zona pecho · solo calor | JST-XH 5p | HEAT+ (VBAT) · HEAT− (Q3) · 3V3 · DQ · GND |
| J4 | Interfaz B · tirante izquierdo | JST-XH 4p | 5V · DIN · BTN · GND |

Contenido de cada módulo térmico (A y B):

- Celda Peltier de 40 × 40 × 3.6 mm sobre placa de aluminio de 60 × 60 × 1.5 mm. La placa es la que toca la piel.
- Disipador de 39.7 × 40 × 10.8 mm y ventilador de 5 V de 40 mm.
- Termostato **KSD9700 60 °C normalmente cerrado**, pegado al disipador y en serie con la Peltier.
- Dos **DS18B20**: uno en la placa (lado piel) y otro en el disipador.

Zona del pecho:

- Película de carbono de ~5 W con KSD9700 de 45 °C en serie y un DS18B20.
- **Pendiente de compra.** Por eso aparece en gris en los diagramas.

---

## 6. Protección térmica

Hay dos capas de protección independientes:

- **Firmware:** el ESP32 recorta o corta la potencia según los cinco sensores.
- **Hardware:** el termostato abre el circuito de la Peltier aunque el microcontrolador se cuelgue.

| Condición | Respuesta | Capa |
|---|---|---|
| Disipador > 60 °C o falla del ventilador | Corte de la Peltier | Firmware + KSD9700 60 °C |
| Placa > 41 °C en modo calor | Limitar o cortar la potencia | Firmware |
| Placa < 18 °C en modo frío | Limitar la potencia | Firmware (umbral hipotético) |
| Película del pecho > 45 °C | Corte | KSD9700 45 °C (pendiente) |
| Batería baja | Apagado ordenado | Firmware (GPIO0) + BMS |

El límite de 41 °C proviene de IEC 80601-2-35 para calentamiento en contacto con la piel; falta verificar el valor exacto en la norma. Para el límite de frío no se localizó una norma específica.

---

## 7. Ubicación en el chaleco

![Vistas frontal y posterior del chaleco. Atrás: módulos A y B en la nuca, pod en la espalda media y batería en la espalda baja, unidos por el arnés. El arnés sale por el costado izquierdo hacia el frente, donde alimenta la película del pecho y la interfaz del tirante.]({{ '/assets/img/04-diseno-tecnico/ubicacion-chaleco.svg' | relative_url }})

Figura 2. Ubicación de componentes y ruta del arnés.
{: .fs-2 .text-grey-dk-000 }

El arnés corre por dentro, entre el forro y la tela exterior. Sale del pod hacia tres destinos:

- **Hacia arriba:** los módulos de la nuca.
- **Hacia abajo:** la batería.
- **Por el costado izquierdo:** el frente, donde están la película del pecho y la interfaz B.

Cada extremo termina en un conector, para que nada quede cosido de forma permanente al textil.

### Ilustración conceptual (ejemplo)

![Render conceptual del chaleco negro visto desde atrás: dos módulos Peltier en la parte alta de la espalda, un pod de control en la espalda media, la batería en la espalda baja y un arnés naranja que cruza al costado izquierdo hacia una película calefactora y un botón en el frente.]({{ '/assets/img/04-diseno-tecnico/ilustracion-conceptual.jpg' | relative_url }})

Figura 3. Ilustración conceptual generada con IA (Nano Banana Pro), solo como referencia visual. Las proporciones, la forma de la prenda y la posición de los módulos no son exactas; la distribución técnica es la de la Figura 2.
{: .fs-2 .text-grey-dk-000 }

### Patrón y muestra textil

![Tres piezas de patrón en papel kraft: mitad de delantero, costado y espalda.]({{ '/assets/img/04-diseno-tecnico/patrones.jpg' | relative_url }})

Figura 4. Patrón: mitad de delantero (izquierda), costado (centro) y espalda (derecha).
{: .fs-2 .text-grey-dk-000 }

![Chaleco negro sin mangas, con cierre frontal al centro, cortado a partir del patrón.]({{ '/assets/img/04-diseno-tecnico/chaleco-prototipo.jpg' | relative_url }})

Figura 5. Muestra del chaleco que sirve de base para integrar la electrónica.
{: .fs-2 .text-grey-dk-000 }

---

## 8. Decisiones, riesgos y pendientes

**L298N en lugar de un driver MOSFET.** Se eligió por disponibilidad local, costo y robustez. El costo es eficiencia:

- El L298N es bipolar. Según la hoja de datos de ST, cada puente pierde entre ~1.8 V y 3.2 V a 1 A.
- Con una batería de 7.4 V, una parte considerable de la energía se convierte en calor dentro del pod en lugar de llegar a la Peltier.
- Poner los puentes en paralelo reduce esa caída, pero no la elimina.
- Cada placa pesa unos 30 g con su disipador (hipótesis, sin pesar).
- **Si las pruebas de autonomía no alcanzan los 60 min, el driver es el primer componente a reemplazar.**

**Autonomía sin validar.**

- 2× 18650 en serie dan entre ~18 y 22 Wh nominales (hipótesis: celdas de 2500–3000 mAh, sin medir).
- Para durar 60 min, el sistema completo debe promediar menos de ~18–22 W, incluyendo las pérdidas de los L298N.
- Este dato sale de la prueba de autonomía con batería.

**Pendientes de compra:** película de carbono y KSD9700 de 45 °C para el pecho, cable de silicón de 20 y 24 AWG, cinta Kapton, conectores XT30 y JST-XH, y MOSFETs de nivel lógico.

**Interfaz B:** requiere cable hasta el tirante frontal y usa los últimos pines libres.

---

## 9. Estado de validación

| Elemento | Estado |
|---|---|
| Peltier + L298N + ESP32-C3 con fuente de laboratorio | Probado; el consumo se aceptó como suficiente |
| Módulo A (housing, DS18B20, KSD9700 60 °C, MP1584, BMS 2S) | En armado y pruebas de banco |
| Módulo B | Pendiente; se arma después de validar A |
| Película del pecho, interfaz B, PCB del pod | Diseño propuesto, sin construir |
| Integración en el chaleco | Pendiente |
