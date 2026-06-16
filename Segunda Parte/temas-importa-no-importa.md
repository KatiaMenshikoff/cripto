# Qué importa / qué no — por clase (según aparición real en parciales)

> Categorización para el **2º parcial (jueves 18/06)**, basada en la **aparición real de cada tema en los 10 parciales resueltos**
> ⚠️ **Contexto del cuatrimestre:** **Shamir NO entra** (confirmado). **Entropía / flujo de
> información SÍ entra** (en su forma pura: H(x) vs H(x|y), OTP, side-channel/disipador).

---

## TL;DR — lo más accionable por clase

- **Clase 6** → lo más rentable es **modelar compartimentos** (BLP/Biba) + **conflictos de ACL** + Clark-Wilson. Firewall (rules) y OAuth2 sobrevendidos: no caen.
- **Clase 7** → todo se reduce a **Anderson + el distractor del salt**. HOTP/TOTP/EKE/SAML son relleno.
- **Clase 8** → **Amenaza/Vuln/Riesgo + ecuación**, **Injection=90%**, **DoS=disponibilidad**, **8 principios**. Las taxonomías (listas) y STRIDE/OWASP, solo reconocimiento.
- **Clase 9** → **entropía pura** (OTP, WAV, disipador) es lo jugoso; malware solo como ítems cortos (rootkit/troyano/polimórfico). Aislamiento/contenedores no cae.
- **Clase 10** → **DMZ** (qué va / qué no) es el ítem estrella; + Defense-in-depth, DevSecOps y RAID. Toda la parte "managerial" (governance, riesgo, PAM, cloud) no cae.
- **Clase 10b** → tres ganchos: **pasos en orden**, **"garantizar" = FALSO**, **"siempre se explota" = FALSO**. El resto es relleno.
- **Clase 11** → solo **ley 25.326 vs GDPR** y **homomórfica**; recién aparece en los parciales 2025.

**Patrón transversal:** los parciales **2025 (Q1, Q2 y recup)** son los más representativos del examen
actual — concentran Clark-Wilson, RBAC, DevSecOps, homomórfica, ley 25.326 y los ganchos de
pentesting. Para practicar, priorizá esos tres.

---

## Clase 6 — Políticas y Control de Acceso

**Hallazgos de la lectura crítica:**
- El **ejercicio estrella es modelar compartimentos** (BLP o Biba): aparece en 2013, 2015, 2016,
  2017, 2018 y 2025Q2. Es lo que más puntos vale → **practicar a mano**.
- El índice **sobrevendió "reglas de firewall"**: en los parciales **nunca** cae una tabla de
  firewall. Lo que sí cae siempre son **conflictos de ACL** (override/augment/first-rule/grant-all).
- **Muralla China (Chinese Wall) cayó en 2018 Ej5 y NO está en el informe** → agujero a cubrir.

### ✅ IMPORTA
- **Bell-LaPadula: reglas read-down / write-up + dominancia** — 2015, 2016, 2017, 2018, 2025Q2.
- **Compartimentos / reticulado / dominancia `(L,C) dom (L',C')` + need-to-know** — casi todos.
  Clave: categorías incomparables = separados (ni r ni w).
- **Biba (integridad) + 3 variantes (estricto / ring-policy / low-watermark)** — 2013 (clínica),
  2018 (retículo CEO/gerentes), recup-2025Q1 (V/F ring vs low-watermark).
- **Dualidad Biba ↔ BLP** (invertir las flechas del retículo) — 2018 Ej3b.
- **Conflictos de ACL: first-rule / grant-all (intersección) / override / augment (unión)** —
  2015, 2016, 2017 (MC de "Manuel" recurrente).
- **Clark-Wilson (CDI / TP / IVP / separation of duty), es INTEGRIDAD no confidencialidad** — 2025Q1.
- **RBAC: modelar roles + separación de tareas** — 2025Q1 Ej5 (ejercicio abierto).
- **Modelo de seguridad vs política de control de acceso vs reglas** (distinguir los 3) — 2025Q2 Ej1b.
- **ACL vs capability list** (columna por objeto vs fila por sujeto) — 2018 Ej2 (anónimos → capability).
- **MAC vs DAC** (RBAC tiene discrecionalidad fuerte) — marco de 2025Q2.
- **Muralla China / Chinese Wall = modelo de CONFLICTO DE INTERÉS (COI/CD)** — 2018 Ej5.
  *No está en el informe.* Agrupa objetos en clases de conflicto; quien accedió a una empresa no
  puede acceder a su competidora del mismo COI.
- **Autenticación ≠ autorización / orden de los módulos** (autentico → autorizo) — 2013 Ej5.

### ❌ NO IMPORTA
- **Reglas de firewall** (first/best-match/deny-over-allow, tabla de 9 reglas) — nunca cayó.
- **ABAC y RuBAC** — nombrados, nunca evaluados.
- **Reglas basadas en contexto / Zero-Trust / OPA-Rego** — es Clase 10 (ZTA), no Clase 6.
- **Ejemplos reales: Unix `rwx`/`umask`, SELinux, Windows, Squid, AWS IAM, API gateway** — relleno.
  (Excepción: **PostgreSQL Row-Level Security**, salió en 2025Q1 atado a Clark-Wilson.)
- **OAuth 2.0 en detalle** (authorization-code, refresh token, scope) — no lo piden desarrollar.
  (OIDC aparece como V/F suelto, y es más Clase 7.)
- **Usuarios privilegiados, transferencias y revocaciones** — no cae.
- **Tríada CIA, "políticas vs mecanismos", "estados seguros/inseguros", identidad del sujeto
  (SID/UID)** — teoría introductoria; a lo sumo un V/F (la "A" se evalúa desde Clase 8).
- **Matriz de accesos como tal** (filas×columnas) — sólo se usa para contrastar ACL vs capability.

---

## Clase 8 — Principios de Diseño y Vulnerabilidades

### ✅ IMPORTA
- **Amenaza / Vulnerabilidad / Riesgo + ecuación** (Riesgo = Amenaza × Vulnerabilidad × Valor del
  activo). El profe lo toma siempre.
- **Injection ≈ 90%** + clasificar un caso concreto como inyección ("mezclar datos con código").
  El reflejo que más puntúa (2013 router /etc/passwd, 2018 inyección, 2018 Von Neumann).
- **8 principios de diseño** (Saltzer & Schroeder) — asociar un caso al principio que viola
  (mínimo privilegio, fail-safe defaults, mediación completa, diseño abierto/Kerckhoffs,
  separación de privilegios, defensa en profundidad, economía de mecanismo, aceptación psicológica).
- **"Garantizar la seguridad" = FALSO** y **DoS = problema de disponibilidad** (CIA, primer
  requisito = disponibilidad; zip bomb en 2013 Ej4).
- **Buffer overflow** y qué lo mitiga / qué no (Rust, stack canary, validar input sí; garbage
  collector no) — 2025Q2 Ej2.
- **CVE vs CWE** (instancia vs clase) — distinción corta y preguntable.

### ❌ NO IMPORTA
- **Listas de taxonomías** (los 4 ejes de amenazas, las 12 categorías de vulnerabilidades) — nunca
  se enumeran. (Saber los 4 ejes sólo para clasificar un caso, no de memoria.)
- **STRIDE / Threat Modeling / DFD** — sólo como V/F ("STRIDE = 6 categorías de Microsoft + DFD" =
  V). No construir nada.
- **OWASP Top 10** (y OWASP LLM) — nunca piden aplicarlo.
- **Zero-day / mercado de exploits / Zerodium / Log4j / Heartbleed** — anécdota, a lo sumo un V/F.
- **Balance tecnología/procesos/personas** y **seguridad vs funcionalidad vs eficiencia** — relleno.
- **Top CWEs puntuales** (787, 79, 89, 416, 78 con número) — no hace falta el número; alcanza con
  saber que injection domina.

> El "flujo de información (intro)" listado en Clase 8 es **entropía**, que **sí entra** — pero se
> estudia desde Clase 9 (su tratamiento canónico).

---

## Clase 7 — Autenticación

**Hallazgos de la lectura crítica:**
- **La fórmula de Anderson es el rey de esta clase.** Cae como cálculo en recup-2023Q1 Ej1, 2025Q1 Ej3 y recup-2025Q1 Ej4, y como V-F calculable en 2015 Ej2 / 2017 Ej2. Es lo único que da puntos "duros".
- **El salt nunca se pregunta solo: siempre es la trampa pegada a Anderson.** Gancho recurrente: "sumar N bits de salt baja la probabilidad de adivinar una clave" → FALSO (el salt no agranda N, solo impide paralelizar). 2015, 2017, 2025Q1.
- **La tupla ⟨A,C,F,L,S⟩ cayó como multiple-choice** (2016 Ej4.3) y base de 2013 Ej5. La distinción F deriva / L corrobora es preguntable textual.
- **Casi todo lo "lindo" del temario (PBKDF2, HOTP/TOTP, EKE, challenge-response, SSO/SAML) NO aparece.** El informe lo desarrolla, los parciales no lo tocan (salvo OIDC en un V-F).

### ✅ IMPORTA
- **Fórmula de Anderson (P = TG/N)** — el tema más rentable. Tiempo (recup-2023Q1 Ej1), longitud mínima (recup-2025Q1 Ej4a), V-F calculable (2015 Ej2, 2017 Ej2, 2025Q1 Ej3a). Dominar los 3 despejes: T≤PN/G, N≥TG/P, L≥log_A(N).
- **Salting (qué hace y qué NO hace)** — siempre junto a Anderson como distractor (2015 Ej2, 2017 Ej2, 2025Q1 Ej3a, recup-2025Q1 Ej4b). No entra en N; solo impide paralelización/rainbow tables.
- **Tupla ⟨A,C,F,L,S⟩ / F deriva vs L corrobora** — MC directo (2016 Ej4.3) y base de 2013 Ej5.
- **Autenticación ≠ Autorización** — 2013 Ej5a (ordenar módulos), implícito en 2018 Ej2b.
- **Ataques offline vs online / impersonación** — 2013 Ej5c (offline ataca F, online ataca L).
- **Biométrico: no es 100% eficaz / robo de identidad** — V-F en 2013 Ej4.1 (VERDADERO).
- **MFA / multicanal e independencia de factores** — recup-2023Q1 Ej3b (V-F) y 2025Q1 Ej3b (SMS como canal de recuperación rompe la independencia).
- **OIDC / OAuth2 (autenticación vs autorización)** — V-F en 2018 Ej2b ("OIDC autoriza con password" → FALSO: OIDC autentica).
- **Almacenamiento: SHA-256 rápido vs PBKDF2/bcrypt + salt** — recup-2025Q1 Ej4b (por qué SHA-256 a secas no sirve). Piden el razonamiento, no el algoritmo.

### ❌ NO IMPORTA
- **HOTP / TOTP** (RFC 4226/6238, Truncate, contador/timestamp) — desarrollados con fórmulas en el informe, cero apariciones.
- **Challenge-Response y EKE** — diagramas en el informe; no caen.
- **Passkeys / clave pública en auth** — solo mención.
- **PBKDF2 en detalle** (iteraciones, XOR, PRF) — solo se menciona como "lo correcto"; nunca lo piden calcular.
- **Tipos de claves, claves pronunciables, selección/expiración/revisión proactiva** — buenas prácticas; no caen.
- **SSO / SAML / LDAP / Active Directory / IdP-SP** — salvo OIDC en un V-F, la federación no se evalúa.
- **Contexto/velocity, step-up authentication** — trasfondo conceptual.
- **Jailing/Honeypot, captchas, exponential back-off** — mencionados; no son ejercicio.

---

## Clase 9 — Flujo de Información y Malware

**Hallazgos de la lectura crítica:**
- El índice lista 14 ejercicios bajo Clase 9, pero **9 son entropía-vía-Shamir** (excluida). Esos NO cuentan, aunque la maquinaria de entropía que usan sí es de esta clase.
- Lo que **realmente cae como Clase 9 pura** es un núcleo chico pero estable: la **condición formal de flujo** `H(x|y)<H(x)` aplicada fuera de Shamir, y unas pocas definiciones de **malware** y **confinamiento**.
- La entropía-pura aparece 3 veces y distinta cada vez (esteganografía/WAV, OTP, disipador/side-channel). Es la **forma más jugosa** de Clase 9.
- El **malware** aparece siempre como ítem corto (definir, MC, V/F), nunca ejercicio entero.
- Toda la práctica del informe (**CWE Top 25, buffer overflow, XSS, SQLi, CSRF**) NO se evalúa como Clase 9: cuando aparece, el índice la imputa a Clase 8/10/10b.

### ✅ IMPORTA
- **Entropía de Shannon `H(X)` y condicional `H(X|Y)`** — herramienta transversal. Uniforme = lg n, certeza = 0. Base de todo.
- **Condición formal de flujo (`H(x|y)<H(x)` ⇒ hay flujo; `=` ⇒ no hay)** — el corazón evaluable. Pura en: recup-2025Q1 Ej3 (OTP), recup-2023Q1 Ej3.d (disipador 256→200 bits), 2018 Ej4 (WAV).
- **Entropía aplicada a esteganografía / canal de almacenamiento (WAV)** — 2018 Ej4, ejercicio completo (3 incisos): entropía máx de muestra (32 bits), alfabeto reducido (53 símbolos ≈ 5,73 bits), detectar anomalía por baja entropía.
- **One-Time-Pad por entropía (secreto perfecto, `H(m|c)=H(m)`)** — recup-2025Q1 Ej3, ejercicio entero (XOR con clave uniforme).
- **Side-channel / canal encubierto vía entropía (disipador acústico)** — recup-2023Q1 Ej3.d. Reducir entropía = flujo; nombrar covert channel (vía no diseñada) vs side-channel (emisión física).
- **Control de acceso vs flujo (la matriz NO controla el flujo)** — 2013 Ej4.2 (V/F: "matriz evita el flujo" = FALSO). Error penalizable.
- **Confinamiento** — 2016 Ej2 (definir y relacionar). No transmitir info directa/indirecta; ideal = aislación total.
- **Malware — Rootkit y Troyano (definición + relación)** — 2016 Ej2. Rootkit = ocultar/persistir; troyano = entrar disfrazado.
- **Virus polimórfico** — 2016 Ej4.2 (MC: "se modifica cada vez que se replica").

### ❌ NO IMPORTA
- **CWE Top 25 completo** (ranking, pillars, categorías) — nunca como Clase 9.
- **Buffer overflow, XSS, SQLi, CSRF, path traversal, SSRF** — aparecen, pero como Clase 8/10b.
- **Canales de temporización / DNS-ICMP tunneling / traffic shaping** — teoría de fondo.
- **Power analysis, timing attack, Rowhammer, Spectre/Meltdown** — color de la clase, cero apariciones.
- **Aislamiento run-time: virtualización, sandboxing, chroot, namespaces/cgroups, contenedores, VM vs container** — no cae.
- **Control de flujo compile-time (loop unrolling, computabilidad)** — teoría.
- **Spyware, Ransomware, Botnet/C&C, ciclo de ataque (8 fases)** — salvo rootkit/troyano/polimórfico, no sale. Ejemplos históricos = anécdota.
- **BLP/Biba/reticulado como "control de flujo"** — cuando el retículo aparece, se evalúa como Clase 6.

---

## Clase 10 — Seguridad en la Empresa

**Hallazgos de la lectura crítica:**
- El tema con peso real y recurrente es la **DMZ**: cae casi idéntico en 2015 Ej4(b), 2017 Ej4 y 2018Q1 Ej5(1) — siempre MC "qué va / qué no va en la DMZ". Lo más rentable de la unidad.
- **Defense-in-depth (5 capas)** sostiene la DMZ y da puntos propios en 2025Q1 Ej1 (red plana → colapsa Perímetro+Red) y como conexión en 2025Q2 Ej1.
- La unidad entró fuerte recién en **2025** vía **DevSecOps/DevOps** (componentes) y **tolerancia a fallas** (RAID vs HA) en 2025Q1 Ej2, y **WAF** en recup-2025Q1.
- **Toda la mitad "managerial" del resumen no aparece**: gobernanza, gestión de riesgos (5 opciones), pirámide de políticas, PAM, herramientas cloud, SBOM, honeypot, IDS/IPS, Red/Blue/Purple, BAS.

### ✅ IMPORTA
- **DMZ — qué servicios aloja (y cuáles no)** — el ítem estrella: 2015 Ej4(b), 2017 Ej4, 2018Q1 Ej5(1). Web/correo/DNS público/proxy sí; bases sensibles, claves y entornos de desarrollo no.
- **Defense-in-depth (5 capas: Perímetro→Red→Host→Aplicación→Datos) y segmentación** — 2025Q1 Ej1 (red plana rompe la cebolla), apoyo en 2025Q2 Ej1.
- **DevSecOps / DevOps — componentes del modelo** — 2025Q1 Ej2: DevOps = Desarrollo + Operaciones; DevSecOps suma Seguridad; QA NO es componente.
- **Tolerancia a fallas / disponibilidad** — 2025Q1 Ej2: RAID = almacenamiento; balanceo/clustering/HA = cómputo.
- **WAF (Web Application Firewall)** — recup-2025Q1 Ej(a): firewall de capa de aplicación (V/F verdadero).

### ❌ NO IMPORTA
- **ZTA en detalle** (componentes NIST 800-207: PDP/PEP, CDM, SIEM, ID Mgmt, PKI) — solo frase de apoyo ("never trust") en 2025Q1/Q2; nunca lo piden desarrollar. Sabé el eslogan y nada más.
- **Gobernanza** (governance, eslabón débil) — no aparece.
- **Gestión de riesgos** (ciclo; 5 opciones evitar/reducir/transferir/compensar/aceptar; DRP) — no cae (la caja "En el parcial" es circular).
- **Pirámide de políticas** (Policy/Standards/Procedures/Guidelines/Baselines) — sin aparición.
- **PAM** — no aparece.
- **Herramientas cloud** (IaC, CSPM/CWPP/CIEM/CNAPP) — no aparecen.
- **SAST/DAST/SCA, SBOM, Log4Shell** — contenido del pipeline; no caen como pregunta.
- **Honeypot, IDS/IPS, NAC, DLP, SIEM** — componentes de capa; teoría de fondo.
- **Red/Blue/Purple Team, BAS** — sin aparición (Red Team evaluado es Clase 10b).
- **Tipos de PenTest (black/white/gray/double-blind)** — pertenece a Clase 10b.

---

## Clase 10b — Pentesting

**Hallazgos de la lectura crítica:**
- De las unidades con **mayor densidad real**: cae en 2015, 2017, 2025Q2 (¡dos ejercicios!) y recup-2025Q1. Pero **lo que cae son dos o tres cosas muy concretas**, no todo el temario.
- **Gancho #1 — el orden de los pasos de la hipótesis de falla.** 2025Q2 Ej2 (MC pura): correcta = "Recolección → Planteo de hipótesis de falla → Prueba de la vulnerabilidad → Generalización → Eliminación". Las trampas sustituyen "hipótesis de falla" por "Prueba de la seguridad", "Prueba formal" o "Verificación formal" (que es lo *opuesto*).
- **Gancho #2 — "garantizar la seguridad" = FALSO.** 2025Q2 Ej5 (Tesla/Kharpaty) y recup-2025Q1 Ej(d) ("software de calidad es garantía" → FALSO). El verbo "garantizar" es el detonante.
- **Gancho #3 — "siempre se intenta explotar" = FALSO.** Idéntico en 2015 Ej2(a) y 2017 Ej2(a): explotar es el último recurso; test repetible, conclusivo y mínimamente intrusivo.
- Ejemplos largos (MTS, ingeniería social), herramientas (nmap, OWASP, CVE) y la metodología informal **nunca son ítem evaluable** — relleno.

### ✅ IMPORTA
- **Pasos de la hipótesis de falla EN ORDEN** (Recolección → Hipótesis de falla → Prueba → Generalización → Eliminación-opcional) — 2025Q2 Ej2. *El* tema más caído; el orden y los nombres exactos dan/quitan puntos.
- **"Planteo de hipótesis de falla" vs "prueba formal / verificación formal"** — 2025Q2 Ej2: las trampas sustituyen ese paso. Verificación formal ≠ pentesting (es lo contrario).
- **El pentesting NO garantiza la seguridad ("garantizar" = FALSO)** — 2025Q2 Ej5 y recup-2025Q1 Ej(d).
- **NO siempre se explota; explotar es el último recurso** — 2015 Ej2(a) y 2017 Ej2(a). Test repetible, conclusivo, mínimamente intrusivo (un exploit puede causar DoS).
- **Pentesting prueba EXISTENCIA, nunca AUSENCIA (Dijkstra)** — argumento central de 2025Q2 Ej5.
- **Verificación formal vs pentesting** (la formal sí prueba ausencia, pero solo en dominio acotado y tampoco da garantía total) — necesario para 2025Q2 Ej2 y Ej5.
- **No reemplaza buen diseño / no es sistemático / vale para ese sistema en ese momento** — los argumentos que pide listar 2025Q2 Ej5(a). Tener 3-4 razones a mano.

### ❌ NO IMPORTA
- **Metodología informal** (preparación, alcance, plan de ataque) — contexto previo, no evaluable.
- **Detalle interno de cada paso** y **caja negra/gris/blanca** — solo necesitás los *nombres* de los pasos; las cajas no aparecen en ningún parcial.
- **Pivoting** — mencionado, sin aparición.
- **Ethical hacking / marco ético / analogía con QA** — teoría de fondo.
- **Áreas modernas** (APIs/microservicios, Supply Chain, RASP, federadores, EOL) — color del profe.
- **Herramientas** (nmap, vulnerability scanning, CVE, MITRE, OWASP, ingeniería social) — no preguntadas en esta unidad.
- **Ejemplos largos** (MTS / IBM 360-370; ataque por ingeniería social) — ilustraciones; ningún parcial los toma.
- **OSSTMM, Bishop cap. 23** — bibliografía.

---

## Clase 11 — Protección de Datos

**Hallazgos de la lectura crítica:**
- De las clases **MENOS evaluadas**. En 10 parciales solo aparece "de verdad" en los dos más nuevos: **2025Q1** y **2025Q2**. Los parciales 2013–2018 NO la tocan.
- Cuando cae, cae concreto: un **ejercicio entero de homomórfica** (2025Q2 Ej3) y un **MC sobre la ley 25.326** (2025Q2 Ej2). El profe marca **25.326 (argentina, bases públicas/privadas) vs Derecho al Olvido = GDPR** como "la más preguntable".
- Los **Data Roles** (Trustee/Steward/Custodian/User) aparecen como herramienta de modelado en 2025Q1 Ej5, no como tema central, pero suman puntos si los citás.
- La mayoría del resumen (ciclo de protección, governance, KEK-DEK, TME, nube, DLP, respuesta a incidentes) NO ha caído nunca.

### ✅ IMPORTA
- **Ley 25.326** (objeto: bases de datos públicas y privadas; hábeas data; no especifica tecnología) — 2025Q2 Ej2. La opción correcta es "las bases de datos públicas y privadas".
- **GDPR y Derecho al Olvido** (es europeo, NO de la 25.326; cookies/consentimiento también GDPR) — 2025Q2 Ej2, los distractores a descartar. La distinción más preguntable.
- **HIPAA** (historias clínicas, EE.UU.) — 2025Q2 Ej2, distractor de la 25.326.
- **Encripción homomórfica** (`f(Enc_k(x))=Enc_k(f(x))`; operar sobre cifrado sin descifrar; cómputo en la nube preservando privacidad) — 2025Q2 Ej3, ejercicio completo de desarrollo.
- **Estado del dato "in-use"** (el más difícil de proteger; la homomórfica lo ataca) — 2025Q2 Ej3, el gancho conceptual.
- **Data Roles** (Trustee / Steward / Custodian / User; dominio más chico al bajar) — 2025Q1 Ej5, para modelar gobernanza. Suma citarlos.

### ❌ NO IMPORTA
- **Ciclo de protección** (Govern→Discover→Protect→Comply→Detect→Respond) — teoría de fondo.
- **Data Governance / Data Life Cycle (PwC)** — no aparece.
- **PCI-DSS y tokenización** — inventario teórico.
- **SOX / FFIEC / FDA y plazos de retención** (SOX 7 años, HIPAA 6) — trivia regulatoria.
- **Estados at-rest / in-transit / in-memory** — salvo in-use vía homomórfica, no se evalúan solos.
- **KEK-DEK / envelope encryption** — vistoso pero sin aparición.
- **HSM / Keystore / GuardedString** — no cae.
- **Cifrado at-rest en la nube** (Own/HYOK/BYOK/Customer/Service-Managed) — no preguntado.
- **In-transit (TLS, IPSEC, SSH, GPG)** — TLS aparece en 2025Q1 Ej1 pero como defensa en profundidad (Clase 8/10).
- **TME / TME-MK** (memoria, AES-XTS, VT-x) — no cae.
- **DLP / Respond a incidentes / primeras 24h / forenses** — todo teórico, sin aparición.
- **Data remanence / secure deletion** — marginal.
