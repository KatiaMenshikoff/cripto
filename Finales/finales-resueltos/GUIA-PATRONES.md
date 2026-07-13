# Guía de patrones — Finales de Criptografía y Seguridad (72.44)

Basado en los 7 finales resueltos en `fragments/` (21/07/2021, 17/07/2023, 06/07/2023,
10/07/2023, 2025-C2(1), Diciembre 2025, Random reciente) + los 3 ejercicios adicionales.
Todo lo que sigue está construido exclusivamente sobre esos 7 finales y el material de la
materia — es un mapa de **patrones repetidos**, no contenido nuevo.

## 1. Estructura fija del final: casi siempre 3 ejercicios con roles fijos

En **6 de los 7 finales** el patrón es idéntico:

| # | Rol del ejercicio | Tema típico | Clase(s) involucradas |
|---|---|---|---|
| **1** | Sistema con **cifrado simétrico** que falla en integridad/replay/modo degradado | IoT, sensores, pulseras, dispositivos de acceso | Primera Parte Clase 2 (Cifrado: modos ECB/CBC/CTR/stream) + Clase 3 (MAC/AEAD) + Clase 5 (nonce/replay) |
| **2** | **Protocolo criptográfico** (autenticación, reparto de secreto, firma, SSO) con 2-3 preguntas puntuales | challenge-response, XOR "sombras", SSO/IdP, hash vs HMAC | Primera Parte Clase 2/3/4/5 + Segunda Parte Clase 2 (Autenticación) |
| **3** | **Pentesting**: dar **4 hipótesis de falla** + cómo probar cada una | app web/mobile con varias funcionalidades (pagos, IoT, exchanges, calendario, noticias) | Segunda Parte Clase 6 (Pentesting) + a veces Clase 3 (vulnerabilidades) |

El único final que rompe el molde es **17/07/2023**, que reemplaza el ejercicio 2 por una
variante del mismo esquema "sombras" del final 2025-C2 pero con enfoque distinto — el
patrón temático (protocolo con preguntas de robustez) se mantiene igual.

**Implicancia para estudiar:** memorizar bien estos 3 "moldes" (no los enunciados exactos)
cubre la gran mayoría del examen. Practicar variantes del ejercicio 1 y usar la checklist
de hipótesis de falla del ejercicio 3 "de memoria" rinde más que estudiar cada final como
un caso aislado.

## 2. Ejercicio 1 — el "molde IoT/sensor con cifrado roto"

Reaparece casi textual en 3 finales (pulseras Helíos / pulseras 2025-C2 / sensor-maestro
10/07/2023) con la misma trampa:

- Se describe un sistema que envía `(ID, contador/ctr, C)` con `C = cifrado_simétrico(dato)`.
- El enunciado **nunca dice "y esto tiene un MAC"** — el punto es notar que el cifrado
  *solo* (CTR/CBC/ECB) **da confidencialidad pero no integridad**, y que sin MAC un
  atacante puede:
  - reproducir/reenviar (**replay**) un mensaje válido capturado antes,
  - o explotar la **maleabilidad** (en CTR/stream: XORear el keystream para voltear bits
    específicos sin conocer la clave; en ECB: bloques repetidos revelan patrones).
- Casi siempre hay un **"modo degradado"** (sin conexión al servidor) que baja la guardia
  de validación — ahí la pregunta es cómo mitigar sin agregar dispositivos nuevos (la
  respuesta típica: OTP/HOTP precargadas, listas de hashes de un solo uso).

**Puntos clave a leer con cuidado en la consigna:**
- ¿El campo que viaja cifrado incluye un **contador/nonce**? Si sí, preguntar si el
  verificador chequea que sea **estrictamente creciente** (sin eso, no hay anti-replay
  real aunque el nonce exista).
- ¿Qué pasa si **falla la conexión** al servidor central? Ese es siempre el gancho para la
  pregunta de "modo degradado".
- Fijarse en el **orden de los campos dentro del cifrado** (p. ej. `wearing‖ctr` vs
  `ctr‖wearing`): no cambia la vulnerabilidad de fondo, pero el enunciado espera que la
  resolución sea precisa con el orden exacto.

## 3. Ejercicio 2 — el "molde de protocolo con preguntas de robustez"

Patrones que reaparecen:
- **Challenge-response con hash:** `C→S: id`, `S→C: r aleatorio`, `C→S: H(Kc‖r)`. Casi
  siempre piden (a) por qué se usa `r` y no `H(Kc)` solo (evita replay), (b) si cambiar
  `r` por un contador incremental es menos seguro (sí: predecible, aunque conceptualmente
  sirve como nonce único — el problema real es previsibilidad, no repetición), y (c) cómo
  lograr autenticación **mutua sin nuevas claves** (romper la simetría con literales de
  rol, p. ej. `H(Kc‖r‖"servidor")`).
- **Esquema tipo "sombras" con XOR + hash:** reparto de un secreto en 2 partes (análogo a
  Shamir con umbral 2) donde cada parte se acompaña de un hash de la otra. Preguntan qué
  pasa si un atacante ve **solo un canal** (nada: OTP con partes independientes no filtra
  nada) y qué hace falta si ve **los dos canales** (pasar de hash simple a HMAC, porque el
  hash sin clave lo puede recalcular cualquiera que controle el mensaje).
- **SSO/IdP con firma digital:** por qué el servicio no necesita la contraseña (firma
  verificable con clave pública, no hace falta compartir secreto), cómo evitar reuso del
  token en otro servicio (agregar `nonce`/expiración, o firmar el campo "servicio" junto
  con la identidad del que lo solicitó).

**Puntos clave a leer con cuidado:**
- Cuando el enunciado dice **"sin introducir nuevas claves"**, la respuesta esperada usa
  literales/roles dentro del hash existente, no criptografía asimétrica nueva.
- Distinguir siempre **integridad vs. confidencialidad vs. no repudio** — la mayoría de las
  preguntas de "¿qué se pierde si...?" giran en torno a esta tríada (p. ej. MAC/HMAC da
  integridad+autenticidad pero **no** no-repudio; eso solo lo da firma digital).

## 4. Ejercicio 3 — el "molde de pentesting: 4 hipótesis de falla"

Aparece en **todos** los finales, siempre con la misma consigna implícita: dar **4
hipótesis de falla concretas** (no genéricas) sobre un sistema descripto, y para cada una
explicar **la acción concreta** que la probaría o refutaría, siguiendo la metodología de
hipótesis de falla (Recolección de información → Hipótesis → Prueba → Generalización →
Eliminación).

**Puntos clave a leer con cuidado en la consigna** (esto es lo que más puntaje pierde si se
ignora):
- La aclaración **"no usar hipótesis genéricas"** es literal en varios enunciados: no
  alcanza con decir "puede haber inyección SQL" — hay que **anclar la hipótesis a un
  campo/flujo/URL concreto** de la descripción (ej. "es probable que el endpoint
  `/user_id/public/calendar` permita enumerar `user_id` y acceder a calendarios ajenos",
  no "puede haber problemas de autorización").
- **Leer todo el enunciado buscando "ganchos" ya insertados por el enunciado**: casi
  siempre cada funcionalidad descripta (SMS/2FA, JWT en localStorage, panel de soporte,
  proveedor de pago externo, links de acceso temporal por mail, QR de vinculación,
  complejidad algorítmica alta) es un gancho para una hipótesis distinta. El enunciado
  suele describir 4–6 funcionalidades porque cada una es una hipótesis "escondida".
- Cada hipótesis necesita: (1) **dónde** está el problema (componente/flujo concreto), (2)
  **por qué es plausible** dado el diseño descripto, (3) **la prueba concreta** (acción
  ejecutable, no "revisar el código").
- Vocabulario a usar: el que está en las clases (hipótesis de falla, mediación completa,
  mínimo privilegio, relaciones de confianza, elabosarán las pruebas según metodología).
  **Evitar nombres de categorías estilo OWASP (BOLA, IDOR, SSRF con esos nombres
  puntuales)** si no aparecen en el material — se puede describir el problema en esos
  términos conceptuales sin el nombre, y así queda dentro de la regla de oro.

## 5. Temas que se repiten cruzando ejercicios (para relacionar clases)

- **Cifrado ≠ integridad** (Clase 2 y 3): es el eje de casi todos los ejercicios 1. Todo
  esquema que solo cifra (ECB/CBC/CTR/stream sin MAC) es vulnerable a manipulación o
  replay — el material lo remarca explícitamente con el ejemplo de RR.HH.
- **MAC da integridad + autenticidad, no no-repudio** (Clase 3): aparece cada vez que se
  compara MAC/HMAC contra firma digital.
- **Nonce/nonce creciente/contador vs. aleatorio** (Clase 5 y Segunda Parte Clase 2): el
  eje de todas las preguntas de replay y challenge-response.
- **Metodología de hipótesis de falla** (Segunda Parte Clase 6): un solo esqueleto de 5
  pasos que hay que poder aplicar a *cualquier* sistema descripto, sin conocer el sistema
  de antemano.
- **Principios de diseño (mediación completa, mínimo privilegio, defensa en profundidad)**
  (Segunda Parte Clase 3): sirven como "por qué" detrás de cada hipótesis de falla.

## 6. Huecos de material detectados al resolver (no inventar si aparecen en el parcial real)

Estos términos aparecieron en enunciados de finales pero **no están, tal cual, en los
resúmenes de la materia** — si aparecen en el examen real, resolver con los conceptos
afines que sí están (indicado entre paréntesis) y no dar por sentado vocabulario externo:
- **k-anonimato** (usar: resistencia a preimagen, salting, efecto avalancha).
- **SSRF** (usar: XXE / falta de mediación completa sobre recursos internos).
- **ChaCha20** por nombre (usar: cifrado de flujo genérico, `E_k(m) = G(k)⊕m`, PRG).
- **Árbol de Merkle** (usar: Merkle-Damgård es lo que sí está — son conceptos distintos).
- **Términos OWASP puntuales** (BOLA, IDOR, "Broken Access Control" como nombre propio):
  usar el concepto de autorización rota / control de acceso, no el nombre de la categoría.

## 7. Checklist rápida para el día del examen

1. Identificar el "molde" de cada ejercicio (¿es IoT+cifrado? ¿protocolo? ¿pentesting?).
2. Ejercicio 1: preguntarse primero "¿esto tiene MAC o no?" y "¿qué pasa en modo
   degradado/sin conexión?".
3. Ejercicio 2: identificar si la pregunta es sobre integridad, confidencialidad o
   no-repudio antes de responder — cada una tiene una herramienta distinta.
4. Ejercicio 3: releer la descripción y anotar, funcionalidad por funcionalidad, un
   candidato a hipótesis de falla concreta (URL/campo/flujo específico) antes de escribir
   nada — normalmente hay más ganchos en el enunciado que las 4 hipótesis pedidas, así que
   se puede elegir las 4 más sólidas.
5. Citar explícitamente la clase/tema al fundamentar (igual que en `fragments/`), y si algo
   excede el material, decirlo en vez de inventar.
