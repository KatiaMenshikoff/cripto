# Ganchos y trampas — V/F y multiple-choice recurrentes

> Repaso final: cada ítem es un V/F o MC que **ya cayó** en los parciales, con la **respuesta
> correcta** y el porqué en una línea. Citado el parcial donde apareció.
> Convención: ✅ = la afirmación es Verdadera · ❌ = es Falsa (con la corrección).

---

## 🔴 Familia "garantizar la seguridad" (gancho del verbo)
Si un enunciado dice que algo **garantiza** la seguridad → es **FALSO**. "Garantizar la seguridad jamás se puede" (palabras del profe). Penaliza si se escapa.

- ❌ "Un esquema basado en pentesting **garantiza** la seguridad" → FALSO. El pentesting prueba *existencia* de fallas, nunca *ausencia* (Dijkstra). — **2025Q2 Ej5** (Tesla/Kharpaty).
- ❌ "Construir **software de calidad** es **garantía** de seguridad" → FALSO. Corrección: "garantía" → "ayuda". Favorece, no garantiza. — **recup-2025Q1 Ej2d**.

## 🔴 Pentesting
- **MC — Pasos para un pentesting exitoso:** correcta = **"Recolección de información → Planteo de Hipótesis de falla → Prueba de la vulnerabilidad → Generalización → Eliminación"**. Trampas: sustituir "hipótesis de falla" por *"Prueba de la seguridad"*, *"Prueba formal"* o *"Verificación formal"* (que es lo **opuesto** al pentesting). — **2025Q2 Ej2**.
- ❌ "En pentesting **siempre** se intenta **explotar** las vulnerabilidades" → FALSO. Explotar es el **último recurso** (puede causar DoS); mejor analizar doc/comportamiento; el test debe ser repetible, conclusivo y mínimamente intrusivo. — **2015 Ej2a, 2017 Ej2a**.

## 🔴 Autenticación — Anderson y salt
- ❌ "Con 5 símbolos **+ 5 bits de SALT** la prob. de adivinar en un año es < 0,5" → FALSO. (1) El cálculo no da (recalcular con N=62⁵). (2) **El salt no agranda N** de un ataque a una clave concreta: solo impide paralelizar/rainbow tables. — **2015 Ej2b, 2025Q1 Ej3a**.
- ❌ "Guardar las claves con **SHA-256** está bien" → FALSO/mala práctica. SHA-256 es hash **rápido y sin salt**; usar PBKDF2/bcrypt/scrypt (costoso) + salt por usuario. — **recup-2025Q1 Ej4b**.
- ✅ "En un sistema **biométrico** puede darse robo de identidad" → VERDADERO. No es 100%: falso positivo + lector manipulable. — **2013 Ej4.1**.
- ❌ "**OpenID Connect** permite que mediante un password se **autorice** a un usuario a loguearse" → FALSO. OIDC **autentica** (no autoriza) y **delega** el login en el IdP (no maneja el password). Quien autoriza es OAuth2. — **2018 Ej2b**.
- **MC — Función de complementación (F):** correcta = "dada info de autenticación, **obtiene** info complementaria" (F: A→C, deriva). Distractor: "retorna V/F" = esa es **L** (corrobora). — **2016 Ej4.3**.

## 🔴 Control de acceso (Clase 6)
- ❌ "Una **matriz de acceso** bien diseñada **evita el flujo** de información no deseado" → FALSO. El control de acceso limita operaciones sobre objetos, **no el flujo** del dato una vez leído. — **2013 Ej4.2**.
- ❌ "Para usuarios **anónimos**, en vez de capability es más práctico una **ACL**" → FALSO (está al revés). La ACL exige identificar al sujeto; para anónimos conviene **capability** (token que en sí habilita). — **2018 Ej2a**.
- **MC — ACL(o)={(Manuel,r),(Directivos,w),(*,w)}, Manuel es Directivo. ¿Con qué política lee Y escribe?** correcta = **augment** (parte del default y suma la específica → {r,w}). *first-rule* → solo r; *grant-all* (intersección) → ∅; *override* → solo r. — **2015 Ej4a, 2016 Ej4.1**.
- **MC — Muralla China (Chinese Wall) es adecuado para:** correcta = **evitar conflicto de interés** (no es integridad ni confidencialidad de CDs en sí). — **2018 Ej5.3**.
- **MC/V-F — variantes de Biba:** ❌ "En **Low-watermark** la escritura se permite a niveles inferiores y la lectura es **irrestricta**" → FALSO. Corrección: "Low-watermark" → **Ring-Policy**. En Low-watermark al leer el sujeto se reclasifica `il(s)=min(il(s),il(o))`. — **recup-2025Q1 Ej2c**.
- ❌ "Los modelos de seguridad mantienen confidencialidad, integridad y/o **autenticidad**" → FALSO. Corrección: "autenticidad" → **disponibilidad** (tríada C-I-A). — **recup-2025Q1 Ej2b**.

## 🔴 Vulnerabilidades (Clase 8)
- ✅ "STRIDE forma parte del **Threat Modeling de Microsoft**" → VERDADERO (6 categorías + DFD). — **2015 Ej2c, 2017 Ej2c**.
- ✅ "Una **zip bomb** (archivo chico que explota al descomprimir) puede violar la seguridad" → VERDADERO. Es un **DoS** → disponibilidad (primer requisito de seguridad). — **2013 Ej4.3**.
- ❌ "**XSS** es el aprovechamiento de la confianza que un **sitio** tiene en la **sesión** con el browser" → FALSO. Eso es **CSRF**. XSS = inyectar JS que el sitio renderiza y ejecuta en el browser de la víctima. — **2018 Ej2d**.
- ✅ "Una defensa recomendada para **SQLi** es **escapar** los datos (comillas)" → VERDADERO (con matiz: la canónica son **prepared statements**/parametrizar). — **2018 Ej2c**.
- **MC — iOS rompe Von Neumann (datos y código en el mismo espacio). ¿Qué resuelve?** correcta = **inyección de código** que venía en los datos (separar datos/código). No "resuelve" buffer overflow ni CSRF. — **2018 Ej5.2**.
- **MC — Estrategia que NO tiene sentido contra buffer overflow:** correcta = **garbage collector** (gestiona memoria dinámica, no evita escribir fuera del buffer). Rust / stack canary / validar input sí. — **2025Q2 Ej2**.
- **MC — Virus polimórfico:** correcta = "se **modifica cada vez que se replica**" (muta el código para evadir firmas). — **2016 Ej4.2**.

## 🔴 Empresa (Clase 10)
- **MC — En la DMZ se localizan / nunca debería contener:** **van** servidores Web, Email, DNS público, Web-proxy. **NO van**: bases de datos sensibles, claves, **subred de desarrollo** (activos internos, detrás del firewall interno). — **2015 Ej4b, 2017 Ej4, 2018 Ej5.1**.
- **MC — ¿Cuál NO es componente de DevOps?** correcta = **QA / Control de Calidad** (DevOps = Desarrollo + Operaciones de IT; DevSecOps suma Seguridad). — **2025Q1 Ej2**.
- **MC — Tolerancia a fallas en almacenamiento:** correcta = **RAID** (redundancia de discos). Balanceo/clustering/HA = disponibilidad de cómputo, no de almacenamiento. — **2025Q1 Ej2**.
- **MC — Estrategia que NO tiene sentido contra CSRF:** correcta = **banear la IP** (el request lo dispara el navegador de la víctima legítima). Token anti-CSRF, evitar GET para acciones, etc., sí. — **2025Q1 Ej2**.
- ✅ "Un **WAF** analiza el intercambio de los protocolos a **nivel aplicación**" → VERDADERO (firewall de capa de aplicación web). — **recup-2025Q1 Ej2a**.

## 🔴 Protección de Datos (Clase 11)
- **MC — La ley argentina 25.326 regula:** correcta = **las bases de datos públicas y privadas** (hábeas data; no especifica tecnología). Distractores: *Derecho al olvido* (= GDPR europeo, NO la 25.326), cookies (GDPR), historias clínicas (HIPAA). — **2025Q2 Ej2**.

---

### Reflejos rápidos (cuando dudes)
- ¿"Garantiza la seguridad"? → **FALSO**.
- ¿Qué vulnerabilidad es este caso raro? → apostá a **inyección** (~90%).
- ¿La matriz/control de acceso "evita el flujo"? → **NO** (control de acceso ≠ control de flujo).
- ¿"Biométrico/100% seguro"? → **NO** (falso positivo, lector manipulable).
- ¿OIDC autoriza? → **NO**, autentica (OAuth2 autoriza).
- ¿IDS bloquea? → **NO**, detecta/alerta (el IPS bloquea).
- ¿DLP previene? → **NO**, es **detectivo**.
- ¿El salt agranda el espacio N? → **NO**, solo impide paralelizar.
- ¿Derecho al olvido = ley argentina? → **NO**, es GDPR.
- ¿"Prueba formal" en los pasos del pentesting? → **NO**, es "hipótesis de falla" (la verificación formal es lo opuesto).
