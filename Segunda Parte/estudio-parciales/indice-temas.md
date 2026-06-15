# Índice de Temas — Segunda Parte (Criptografía y Seguridad, ITBA)

> **Qué es esto.** Índice maestro de la Segunda Parte: por cada unidad, el inventario
> completo de temas + un resumen híbrido (hechos clave, fórmulas y distinciones evaluables)
> + señales de examen + errores que penalizan + citas a la fuente. Sirve para estudiar y,
> sobre todo, como **insumo de la Fase 2**: mapear cada ejercicio de parcial a sus temas y
> resolverlo citando estas fuentes. La spec de la Fase 2 está al final de este documento.

## Cómo leer las fuentes

| Sigla | Archivo | Tema |
|-------|---------|------|
| **I6** | `informes-pina/clase06-politicas-control-acceso/clase06-politicas-control-acceso.{tex,pdf}` | Políticas y Control de Acceso |
| **I7** | `informes-pina/clase07-autenticacion/clase07-autenticacion.{tex,pdf}` | Autenticación |
| **I8** | `informes-pina/clase08-principios-diseno-seguridad/clase08-principios-diseno-seguridad.{tex,pdf}` | Principios de Diseño + Vulnerabilidades |
| **I9** | `informes-pina/clase09-flujo-informacion-malware/clase09-flujo-informacion-malware.{tex,pdf}` | Flujo de Información + Malware |
| **U4** | `resumenes/unidades/04_flujo_informacion_malware.tex` | Flujo de Información + Malware (resumen) |
| **U5** | `resumenes/unidades/05_seguridad_empresa.tex` | Seguridad en la Empresa |
| **U6** | `resumenes/unidades/06_pentesting.tex` | Pentesting |
| **U7** | `resumenes/unidades/07_proteccion_datos.tex` | Protección de Datos |

**Bishop** = *Computer Security: Art and Science* (Matt Bishop), el libro de la cátedra.

## Spine cronológico de la cátedra

```
Clase 6   →  Clase 7        →  Clase 8                    →  Clase 9 / Unidad 4   →  Unidad 5      →  Unidad 6     →  Unidad 7
Políticas    Autenticación     Principios de diseño +        Flujo de info +         Seguridad en     Pentesting      Protección
y Control                      Vulnerabilidades +            Malware                 la Empresa                       de Datos
de Acceso                      Threat Modeling
```

> ⚠️ **Fuentes y preferencia.** Las 7 unidades existen como **informe** (Clases 6–9, `I6`–`I9`)
> y como **resumen** (`resumenes/unidades/01`–`07`, con PDF standalone ya compilado). **Regla:
> preferir SIEMPRE el informe sobre el resumen.** Por eso las unidades 1–4 se citan desde I6–I9
> (para flujo de info, I9 es canónico; I8 lo introduce). **Las unidades 5–7 (Clases 10/10b/11)
> NO tienen informe** → su única fuente es el resumen (U5/U6/U7).
>
> 🚫 **Redundancia a evitar:** las cajas "En el parcial" de los resúmenes se generaron leyendo
> los parciales → para ESTA tarea son **circulares**. NO se usan como señal: la cobertura real
> de los parciales la deriva la Fase 2 desde los parciales mismos. Las únicas señales de
> prioridad válidas son el **énfasis del profesor en clase** (transcripts/informes).

---

# Unidad 1 (Clase 6) — Políticas de Seguridad y Control de Acceso

**Fuente:** I6 · **Bishop:** cap. 4–8.

### Temas tocados
1. Seguridad como **estados seguros/inseguros**; política de seguridad = definición de qué estados son seguros. Un sistema que pasa *eventualmente* a un estado inseguro deja de ser seguro (aunque vuelva).
2. **Tríada CIA** (Confidencialidad, Integridad, Disponibilidad).
3. **Sujeto / Objeto / Acceso**; identidad del sujeto (SID/UID).
4. **Políticas vs. Mecanismos** (prevenir / detectar / recuperar).
5. **Modelos de seguridad** (formales): qué son y por qué no cubren todo el sistema.
6. **Bell–LaPadula** (confidencialidad): read-down / write-up, dominancia, reticulado/compartimentos, *need-to-know*, principio de tranquilidad.
7. **Biba** (integridad): estricto, ring-policy, low-watermark.
8. **Clark–Wilson** (integridad comercial): CDI/UDI, TP/IVP, reglas CR/ER, *separation of duty*.
9. **MAC vs. DAC** (mandatorio/reglas vs. discrecional/identidad).
10. **DAC por extensión:** matriz de accesos, **ACL** (columnas) vs. **capability lists** (filas), conflictos de ACL, derechos por defecto, transferencia/delegación/revocación.
11. **DAC por comprensión:** **RBAC** (roles), **ABAC** (atributos), **RuBAC** (reglas).
12. **Resolución de múltiples reglas:** first-match, best-match, deny-over-allow (ejemplo firewall).
13. **Reglas por contexto** (anti-fraude); **Zero-Trust + OPA/Rego**.
14. **Ejemplos reales:** Unix (`rwx`, `umask`), SELinux, Windows, PostgreSQL (RBAC + RLS), Squid, AWS IAM, API gateway.
15. **OAuth 2.0** (autorización): flujo authorization-code, refresh token, scope.

### Lo más importante (hybrid)
- **Bell–LaPadula (confidencialidad, "read down / write up"):**
  - Seguridad simple (lectura): `L(s) ≥ L(o)` → leer hacia abajo.
  - Propiedad ⋆ (escritura): `L(o) ≥ L(s)` → escribir hacia arriba.
  - ⋆-fuerte: leer+escribir el mismo objeto ⇒ solo en la **misma** clasificación.
  - **Dominancia con compartimentos:** `(L,C) dom (L',C') ⟺ L' ≤ L ∧ C' ⊆ C`. Forma un **reticulado** (orden parcial): pares incomparables ⇒ separados (ni r ni w). Razón de fondo: BLP es **control de flujo** (la info no "baja").
  - Limitación: **principio de tranquilidad** (asume clasificaciones fijas).
- **Biba (integridad, inverso de BLP, "read up / write down"):** estricto → `il(s) ≥ il(o)` para escribir, `il(o) ≥ il(s)` para leer. Low-watermark: lectura libre pero `il(s) = min(il(s), il(o))` (dinámico). "Uno es tan confiable como las fuentes que usa."
- **Clark–Wilson:** CDI (datos restringidos) solo se modifican vía **TP** (transformation procedure); **IVP** verifica integridad; **separation of duty** (quien modifica ≠ quien verifica). Base de **user space / kernel space + syscalls**.
- **MAC vs DAC:** MAC = reglas universales del sistema (BLP). DAC = el dueño/admin define permisos (file systems, object storage). RBAC tiene **discrecionalidad fuerte** (alguien asigna roles).
- **ACL vs Capability:** ACL = **columna** de la matriz (por objeto: lista de (sujeto, permisos)). Capability list = **fila** (por sujeto). Matriz no escala por el *humano* que la llena, no por la computadora.
- **Resolución de conflictos** (¡ejercicio recurrente!): unión (permitir si alguna da) / Grant-All = intersección (denegar si alguna deniega) / first-match / best-match (más específica) / override vs augment / deny-over-allow.
- **Unix `-rwxrwxrwx`:** tipo + user/group/others; `umask` saca permisos por máscara.
- **OAuth2:** delega acceso sin compartir credenciales; access token (corto) + **refresh token** (un solo uso, permite recuperarse de robo); **scope** = permisos pedidos (mecanismo DAC).

### 🎯 Señales de examen
- **Ejercicios típicos de cuenta/regla:** resolver una **ACL con conflictos** según la política (unión / Grant-All / first-match / override / augment) — I6 trae dos resueltos. Resolver **reglas de firewall** con first-match vs best-match vs deny-over-allow — I6 trae el ejemplo completo de 9 reglas.
- Aplicar **dominancia** BLP a un reticulado (qué puede leer/escribir un sujeto en `(TS,{nuclear})`, etc.).
- Distinguir **BLP (read-down/write-up)** de **Biba (read-up/write-down)** — confundir las direcciones es error clásico.
- **MAC vs DAC** y por qué RBAC tiene parte discrecional.
- ACL = columnas / Capability = filas.

### ⚠️ Errores que penalizan
- Invertir las reglas de BLP/Biba.
- Decir que "la matriz no escala por la computadora" (el cuello de botella es el humano que la configura).
- Confundir ACL (por objeto) con capability list (por sujeto).

### 🔗 Cruces
- **Bell–LaPadula** se reusa en I8/I9/U4 como base del *control de flujo*. **Separation of duty** (Clark–Wilson) ↔ principio "Separación de tareas" de I8. **OAuth2** ↔ I7 (con OIDC pasa a autenticación). **Zero-Trust/contexto** ↔ U5 (ZTA NIST 800-207). **Contexto/velocity** ↔ I7 (factor contexto).

---

# Unidad 2 (Clase 7) — Autenticación

**Fuente:** I7 · **Bishop:** cap. 12–13.

### Temas tocados
1. Autenticación = asociar **identidad** a un **principal**; entidad / identidad / principal; autenticación ≠ autorización.
2. **Sistema de autenticación** como tupla `⟨A, C, F, L, S⟩`.
3. **Factores:** algo que conozco / tengo / soy + **contexto**.
4. **Claves:** tipos, almacenamiento (texto plano / cifrado / hash), Unix legacy (1 de 4096 hashes).
5. **Ataques:** offline vs online; mejoras (diccionarios, leaks, transformaciones, contexto); catálogo (ingeniería social, fuerza bruta, diccionario, rainbow tables, password stuffing).
6. **Prevenciones:** esconder A/C/F; exponential back-off, deshabilitar principales, jailing/honeypot, captchas.
7. **Fórmula de Anderson** `P = TG/N` (3 ejercicios resueltos).
8. Selección de claves; revisión proactiva; expiración.
9. **Salting** y **PBKDF2** (bcrypt/scrypt).
10. **Protocolos sin transmitir la clave:** challenge-response, EKE, passkeys.
11. **OTP:** HOTP (RFC 4226, contador) y TOTP (RFC 6238, tiempo).
12. **MFA**; step-up authentication.
13. **Autenticación remota/federada (SSO):** OAuth2 / OpenID Connect / SAML; IdP/SP; LDAP / Active Directory.

### Lo más importante (hybrid)
- **Tupla `⟨A,C,F,L,S⟩`:** A=info de autenticación (del usuario), C=info complementaria (lo que guarda el sistema, p.ej. hash), **F: A→C** (deriva, no invertible), **L: A×C→{0,1}** (corrobora = login), S=funciones de selección (alta/cambio). Si **cualquiera** falla, falla todo. El atacante busca `a` tal que `F(a)=c`.
- **F deriva, L corrobora** (distinción que se pregunta).
- **Factores:** conozco (secreto compartido) / tengo (posesión, token RSA rotativo) / soy (biométrico, no 100% — chip dedicado fuera del kernel, da OK/FAIL) / contexto (red, geo, **velocity**: BA→Ámsterdam en 2h ⇒ factor extra).
- **Offline vs online:** offline = tengo `F` y `c` (robé la base), pruebo `a` hasta `F(a)=c` (John the Ripper /etc/shadow, ophcrack SAM). Online = ataco `L` (login) con cuentas conocidas (root/admin/guest).
- **Anderson:** `P = T·G / N` (P=prob. de adivinar, T=tiempo, G=pruebas/seg, N=espacio). Despejar `T ≤ P·N/G` o `N = T·G/P` y luego `L ≥ log_base(N)`.
  - Ej: N=10¹⁰, G=10⁴, P=0.5 ⇒ T≈6 días. N=26⁸, G=10⁴ ⇒ ≈121 días. Longitud mín con 36 chars, G=10⁵, <1 año ⇒ **L ≥ 9**.
- **Salting:** `f(a) = x ‖ f'(a,x)` con `x` distinto por usuario ⇒ no se paraleliza el ataque, dos claves iguales no dan el mismo `c`. Algoritmos **costosos a propósito** (≥100ms).
- **PBKDF2:** no es hash, es derivación; itera una PRF `c` rondas y hace XOR. Más rondas = más costo para el atacante. `OTP ≠ One Time Pad`.
- **Challenge-response:** B manda `r`, A responde `f(k,r)` sin enviar `k`. **EKE** = lo mismo sobre canal cifrado `k_s` (atacante no sabe si una clave es correcta). **Passkeys** = challenge-response con clave pública/privada.
- **HOTP:** `Trunc(HMAC-SHA1(K,C))`, contador se incrementa, requiere look-ahead. **TOTP:** `HOTP(K, T)` con `T = ⌊(unixtime − T₀)/X⌋`, T₀=0, X=30s, sin contador, drift-window. Ambos ≥6 dígitos.
- **MFA:** factores de distinta naturaleza (comprometer distintos assets). El **celular concentra "tengo"+"soy"** → cómodo pero si lo clonan caen ambos.
- **SSO/federada:** **OAuth2 = autorización**, **OIDC = autenticación sobre OAuth2** ("login con Google"), **SAML** = intercambio IdP↔SP. LDAP almacena info de auth; AD usa LDAP.

### 🎯 Señales de examen
- **Ejercicios de Anderson** `P=TG/N`: calcular tiempo de ataque o longitud mínima de clave. Cuidado con unidades (segundos→días→años) y con despejar `L = log_base(N)`.
- Completar/explicar la tupla `⟨A,C,F,L,S⟩` y qué ataca offline (F) vs online (L).
- Distinguir OAuth2 (autoriza) / OIDC (autentica) / SAML.
- HOTP vs TOTP (contador vs tiempo); por qué salting frena la paralelización.

### ⚠️ Errores que penalizan
- Confundir **OTP** con **One Time Pad**.
- Decir que el biométrico es 100% seguro / que el SO accede al chip biométrico (solo recibe OK/FAIL).
- Confundir F (deriva) con L (corrobora).

### 🔗 Cruces
- **OAuth2/scope/refresh** ↔ I6 (allí se ve como autorización). **Contexto/velocity** ↔ I6 (reglas por contexto, anti-fraude) y U5 (ZTA). **Federación/SSO** ↔ U5 (ZTA relaja con SSO). **Tokens físicos/HSM/anti-tampering** ↔ I8 (FIPS) y U7 (HSM/KEK). **Autenticación inadecuada** ↔ I8 (catálogo de vulnerabilidades).

---

# Unidad 3 (Clase 8) — Principios de Diseño + Vulnerabilidades + Threat Modeling

**Fuente:** I8 · **Bishop:** cap. 12–13 (+ 16–17, 32 para la parte de flujo/entropía).

### Temas tocados
1. **8 principios de diseño** (Saltzer & Schroeder 1975).
2. Balance **tecnología / procesos / personas**; seguridad vs. funcionalidad vs. eficiencia.
3. **Flujo de información** (intro) — entropía, dados, Shamir, canales ocultos, aislamiento (se profundiza en I9/U4).
4. **Amenaza / Vulnerabilidad / Riesgo** + ecuación del riesgo.
5. **Taxonomías de amenazas** (4 ejes: origen / agente / intención / impacto).
6. **Catálogo de vulnerabilidades** (12 categorías).
7. **MITRE: CVE y CWE**; Top CWEs; buffer overflow / XSS / SQLi / use-after-free / command injection.
8. **Zero-day** y mercado de exploits (Zerodium); Log4j, Heartbleed.
9. **OWASP Top 10** (+ OWASP LLM Top 10, content poisoning).
10. **Threat Modeling** (Microsoft) y **STRIDE**; **DFD** (Data Flow Diagram).

### Lo más importante (hybrid)
- **8 principios:** (1) Mínimo privilegio; (2) Fail-safe defaults (whitelist>blacklist, default = sin acceso); (3) Economía de mecanismo (simplicidad; complejidad = enemiga); (4) Mediación completa (la "puerta del castillo", todo pasa por el mediador); (5) Diseño abierto (Kerckhoffs, no security-by-obscurity, "los bits son eternos"); (6) Separación de privilegios (oposición de intereses; nadie controla solo una tarea crítica); (7) Mecanismos exclusivos / defensa en profundidad (no compartir recursos; el bug del "flag reutilizado"); (8) Aceptación psicológica / mínimo asombro (el peor usuario es el más eficiente).
- **CIA / disponibilidad:** "el primer requisito de seguridad es que el sistema esté **disponible**" → un DoS **es** un ataque de seguridad.
- **Amenaza / Vulnerabilidad / Riesgo:**
  - **Amenaza** = la *intención* de dañar (no el hacker ni el script).
  - **Vulnerabilidad** = debilidad en un activo (un **bug disfrazado de feature** de seguridad).
  - **Exploit** = el programa que la explota.
  - **Riesgo = Amenaza × Vulnerabilidad × Valor del activo** (el prof agrega el *asset*; recursos finitos ⇒ priorizar).
- **4 ejes de amenazas:** origen (interna/externa), agente (humano/ambiental/tecnológico/—IA—), intención (maliciosa/no), impacto (destrucción/corrupción/revelación/robo-servicio/DoS/elevación/uso-ilegal).
- **Injection flaws ≈ 90% de las vulnerabilidades** (mezclar datos con código). Top CWEs: 787 out-of-bounds write, 79 XSS, 89 SQLi, 416 use-after-free, 78 command injection.
- **CVE** (vulnerabilidad concreta `CVE-AAAA-NNNN`) vs **CWE** (debilidad/clase). **Zero-day** = no reportada aún. Log4j (RCE), Heartbleed (buffer over-read en OpenSSL).
- **STRIDE:** Spoofing(autenticación) / Tampering(integridad) / Repudiation(no repudio) / Information disclosure(confidencialidad) / DoS(disponibilidad) / Elevation of privilege(autorización). Se cruza con **DFD** (procesos/entidades externas/flujos/almacenamientos). Por cada riesgo: eliminar / mitigar / compensar / aceptar.

### 🎯 Señales de examen
- **🔴 "Amenaza / Vulnerabilidad / Riesgo… lo tomamos SIEMPRE"** (palabras del prof). Saber definir y distinguir los tres + la ecuación con *asset*. Cae prácticamente seguro.
- **Injection = 90%** ("si se llevan una sola cosa de la materia").
- Mapear una amenaza a una categoría **STRIDE** y a un elemento del **DFD**.
- Asociar un caso (DoS, robo de cuenta, reutilización de flag) al **principio de diseño** que viola.
- "La complejidad es enemiga de la seguridad"; whitelist > blacklist; fail-closed.

### ⚠️ Errores que penalizan
- Decir que la amenaza "es el hacker/script" (es la *intención*).
- Olvidar que un **DoS es un problema de seguridad** (disponibilidad).
- Confundir CVE (instancia) con CWE (clase).

### 🔗 Cruces
- **Flujo de información** se profundiza en I9/U4. **Mínimo privilegio / fail-safe / defensa en profundidad** ↔ U5 (Defense-in-depth, ZTA, PAM). **Threat Modeling/STRIDE** ↔ U5 (DevSecOps "Plan & Develop") y U6 (hipótesis de falla). **Riesgo (eliminar/mitigar/compensar/aceptar)** ↔ U5 (gestión de riesgos, 5 opciones). **FIPS/anti-tampering** ↔ I7 (tokens) y U7 (HSM). **Vulnerabilidades concretas** ↔ U6 (pentesting) y I9/U4.

---

# Unidad 4 (Clase 9) — Flujo de Información y Malware

**Fuentes:** I9 + U4 · **Bishop:** cap. 16–17 (+ 32 entropía). Práctica: CWE Top 25.

### Temas tocados
1. **Control de acceso vs. flujo de información**; los controles de acceso son **mecanismos abiertos** (metáfora cebolla / queso suizo).
2. **Confinamiento** y **aislación total** (2 requisitos: no comunicar, no ser observado).
3. **Canales encubiertos (covert channels):** timing vs storage; ejemplos (sonido de impresora, DNS/ICMP tunneling, EM).
4. **Ataques de canal lateral (side-channel):** timing, power analysis, EM; Spectre/Meltdown/Rowhammer; hyperscalers.
5. **Entropía de Shannon** `H(X)` y condicional `H(X|Y)`.
6. **Flujo de información formal** (definición con entropía); flujo directo e indirecto.
7. **Control de flujo:** compile-time vs run-time.
8. **Aislamiento (run-time):** VMs (hipervisor, kernel propio), **sandboxing** (chroot, namespaces/cgroups), **contenedores** (kernel compartido).
9. **Malware:** virus/worm, troyano, rootkit, spyware, ransomware, botnet; propagación vs payload; ciclo de ataque (8 fases con loop).

### Lo más importante (hybrid)
- **Control de acceso NO controla el flujo:** un dato leído legítimamente puede copiarse a un objeto que otros leen. Las políticas **restringen el flujo**, no el acceso a objetos.
- **Entropía:** `H(X) = −Σ p(xᵢ) log₂ p(xᵢ)`. Mide **incertidumbre**. **Mín = 0** (un `p=1`: certeza). **Máx = log₂ n** (uniforme). Condicional `H(X|Y) = Σ p(yⱼ) H(X|Y=yⱼ)`.
- **Flujo de información (formal):** hay flujo de `x`→`y` si la incertidumbre sobre `x` **baja** al observar `y`:
  - Si `y` **ya existía** en `s`: `H(xₛ|yₜ) < H(xₛ|yₛ)`.
  - Si `y` **se crea** durante la ejecución: `H(xₛ|yₜ) < H(xₛ)`.
  - Sin flujo ⇒ `≥` (puede *aumentar* si una asignación "pisa" la variable).
- **Ejercicios resueltos:** `y = x+z` (H(x)=3 → H(x|y)=1.5 ⇒ hay flujo); `if(x==0)y=1 else y=0` (H=1→0, canal espacial); `while(x==0){}` (canal temporal por terminación).
- **Covert channel vs side-channel** (¡no confundir!): covert = el **medio** (vía no diseñada para transmitir, *evade controles*); side-channel = **explotar** observación física del hardware (consumo/tiempo/EM) para sacar info (típicamente la **clave**). "Dos caras de la misma moneda."
- **VM vs contenedor:** VM = **kernel propio** por instancia (aislamiento fuerte, pesado). Contenedor = **comparte el kernel** del host (liviano, comparte más recursos → quedan canales). Sandboxing = arbitra E/S (chroot, namespaces+cgroups).
- **Malware:** **virus/worm = se propaga**, **troyano = se esconde** en software benigno, **rootkit = se oculta** (contramedidas activas, kernel/BIOS), spyware = recolecta (covert channel), ransomware = cifra+extorsiona (cripto), botnet = zombis + **C&C** descentralizado. **Propagación/ocultamiento ≠ payload** (lo que hace adentro).
- **Ciclo de ataque (8):** Initial Recon → Initial Compromise → Establish Foothold → Escalate Privileges → Internal Recon → Move Laterally → Maintain Presence (4–7 = **loop**) → Complete Mission.

### 🎯 Señales de examen
- **Flujo con entropía:** calcular `H(x)` y `H(x|y)` y concluir si hay flujo. **Elegir la fórmula correcta:** condicional si `y` ya existía, absoluta si `y` se crea.
- **Reducir entropía = flujo de información** (caso típico: el sonido del disipador baja la entropía de la clave 256→200 ⇒ 56 bits filtrados; vía no diseñada = covert channel, explotarla = side-channel).
- **Covert channel vs side-channel** (no confundir).
- **Malware:** virus/worm propaga, troyano esconde, rootkit oculta; el "qué hace" es el payload.
- **VM vs contenedor** (kernel propio vs compartido).
- Control de acceso ≠ control de flujo (la matriz de acceso **no** evita el flujo).

### ⚠️ Errores que penalizan
- No reconocer que **reducir entropía = flujo de información**.
- Afirmar que el control de acceso/matriz **evita** el flujo (no lo evita).
- Usar la **fórmula de flujo equivocada** (condicional vs absoluta).
- Confundir covert channel con side-channel.

### 🔗 Cruces
- **Entropía** ↔ I8 (intro: dados, Shamir). **Bell–LaPadula** ↔ I6 (base del control de flujo). **VMs/sandbox/contenedores** ↔ U5 (host hardening) y I8 (least common mechanism). **Spectre/Rowhammer** ↔ I8 (shared tenancy). **Ciclo de ataque / movimiento lateral / pivoting** ↔ U6 (pentesting). **Ransomware/DLP** ↔ U7 (detección).

---

# Unidad 5 — Seguridad en la Empresa

**Fuente:** U5 · **Lectura:** **NIST 800-207 (cap. 2 y 3)**, *no* Bishop.

### Temas tocados
1. **Gobernanza** de seguridad (alinear con el negocio; cultura/concientización).
2. **Gestión de riesgos:** Identificar → Evaluar → Mitigar → Monitorear; **5 opciones** (evitar/reducir/transferir/compensar/aceptar).
3. **Políticas de seguridad:** pirámide Policy→Standards→Procedures→Guidelines→Baselines.
4. **Defense-in-depth** (5 capas: Perímetro/Red/Host/Aplicación/Datos); DMZ, IDS/IPS, honeypot, NAC, proxy, DLP, WAF.
5. **Zero Trust Architecture (ZTA)** NIST 800-207: PDP (PE+PA), PEP; CDM, Compliance, Threat Intel, Logs, SIEM, ID Mgmt, PKI, Data Access Policy.
6. **PAM** (Privileged Access Management).
7. **DevSecOps** (Shift Left); SAST/DAST/SCA; SBOM; Log4Shell.
8. **Protección de la nube:** IaC; CSPM/CWPP/CIEM/CNAPP.
9. **Evaluaciones de seguridad**; **PenTest** (tipos por visibilidad y por sistema); Red/Blue/Purple Team; BAS.

### Lo más importante (hybrid)
- **Riesgo — 5 opciones:** **evitar** (eliminar causa, caro), **reducir** (bajar prob/impacto, lo más común), **transferir** (a un proveedor), **compensar** (no se reduce pero se contrarresta), **aceptar** (documentado). **Reducir ≠ compensar.**
- **Pirámide de políticas:** arriba = más **ownership del management** + mandatorio; abajo = más **especificidad** + discrecional.
- **Defense-in-depth (cebolla, 5 capas):** Perímetro (firewall, honeypot, IDS/IPS) → Red (segmentación, NAC, proxy, DLP) → Host (hardening, H-IDS, AV) → Aplicación (WAF, buen diseño) → **Datos** (cifrado, IAM, DLP, PKI). Principios: diversificación, redundancia, segmentación, monitoreo. **IDS detecta/alerta, IPS previene/bloquea.** **WAF** = firewall de capa aplicación web. **DMZ** = zona expuesta entre FW externo e interno.
- **ZTA (NIST 800-207):** "Never Trust, Always Verify, Assume Breach". La **intranet ya no es de confianza**. **PDP** = Policy Engine + Policy Administrator; **PEP** separa Untrusted (Subject) de Trusted (Resource). Alimentado por CDM, Compliance, Threat Intel, Logs, SIEM, ID Mgmt, PKI, Data Access Policy. Se relaja con SSO/federación. Fácil de implementar mal.
- **DevSecOps / Shift Left:** seguridad en cada etapa (Plan&Develop: threat modeling, pre-commit; Commit: SAST; Build: DAST; Prod: pentest/WAF; Operate: monitoreo). **SAST** (estático, sin ejecutar) / **DAST** (dinámico, en ejecución) / **SCA** (dependencias, transitivas). **SBOM** (inventario de componentes) ← resurgió con **Log4Shell**.
- **Cloud:** **CSPM** (config, estático) / **CWPP** (workloads corriendo, comportamiento) / **CIEM** (permisos excesivos) / **CNAPP** (integra todo).
- **PenTest por visibilidad:** Black/Blind (sin info) / White (todo) / Gray (parcial) / Double-blind (el atacado tampoco sabe — mide respuesta). Por sistema: red externa/interna/wireless/web/ingeniería social.
- **Red/Blue/Purple Team**; **BAS** (Breach & Attack Simulation, recurrente, mide detección+reacción).

### 🎯 Señales de examen
- **Reducir vs compensar** (distinción explícita en el resumen).
- **Aceptar** un riesgo: ejemplo del **DRP** documentado pero no ejecutado (lo exige el Banco Central a las fintech).
- **IDS vs IPS** (detecta vs previene).
- Ubicar componentes en las **5 capas** de Defense-in-depth; rol del PDP/PEP en ZTA.
- SAST vs DAST vs SCA; qué etapa del pipeline.

### ⚠️ Errores que penalizan
- Confundir **reducir** (baja prob/impacto) con **compensar** (lo contrarresta, el riesgo sigue).
- Decir que IDS bloquea (ese es el IPS).
- Confundir el eje de la pirámide (ownership arriba / especificidad abajo).

### 🔗 Cruces
- **Riesgo (5 opciones)** ↔ I8 (eliminar/mitigar/compensar/aceptar). **Defense-in-depth** ↔ I8 (defensa en profundidad / least common mechanism) y U4 (host hardening). **ZTA/contexto** ↔ I6 (reglas por contexto, OPA) e I7 (SSO/federación). **PenTest / Red Team** ↔ U6 (es la unidad dedicada). **DevSecOps threat modeling** ↔ I8 (STRIDE). **DLP/SBOM** ↔ U7 (protección de datos). **PAM** ↔ I8 (mínimo privilegio).

---

# Unidad 6 — Pentesting

**Fuente:** U6 · **Bishop:** cap. 23 (1–2); OSSTMM. **⚠️ Entra COMPLETA en el parcial** (lo remarcó el prof).

### Temas tocados
1. **Verificación formal vs. pentesting** (precondiciones/poscondiciones; ausencia vs existencia).
2. Objetivos y **ethical hacking** (no reemplaza buen diseño).
3. Metodología informal (preparación).
4. **Metodología de Hipótesis de Falla** (los pasos en orden) — *núcleo del parcial*.
5. Caja negra / gris / blanca (vs niveles de conocimiento inicial).
6. Pasos en detalle (recolección, hipótesis, prueba, generalización, eliminación); **pivoting**.
7. Herramientas (nmap, vulnerability scanning, CVE/MITRE, OWASP Top Ten).
8. Ejemplos: **MTS** (Michigan Terminal System) e **ingeniería social** (ataque externo).
9. **El pentesting NO garantiza la seguridad** (trampa de parcial).

### Lo más importante (hybrid)
- **Verificación formal vs pentesting:**
  - Formal = prueba **matemáticamente** que se cumplen restricciones; **SÍ prueba ausencia** de vulnerabilidades, pero solo en un algoritmo/programa/ambiente acotado (incluyendo TODOS los factores) — impráctica para un sistema completo.
  - Pentesting = prueba la **existencia**; **NO prueba ausencia** (Dijkstra: el testing muestra presencia de bugs, no su ausencia). Económico, prioriza lo crítico.
- **🔴 Metodología de Hipótesis de Falla (MEMORIZAR EN ORDEN):**
  `Recolección de información → Planteo de hipótesis de falla → Prueba → Generalización → Eliminación (opcional)`.
  - **Pivoting** reinyecta info a la fase de recolección (proceso cíclico).
  - **Generalización** = la parte más importante (combinar vulnerabilidades para máximo impacto).
  - **Eliminación** = opcional (solo recomendaciones; quien testea no suele ser quien arregla).
- **Prueba:** priorizar por nivel de acceso; **explotar es el último recurso** (mejor: analizar doc/comportamiento). Debe ser **repetible y conclusiva**; **lo menos intrusiva posible** (puede causar DoS — ej. nmap que tiró la red, caso Chernóbil).
- **Caja negra (sin info) / gris (parcial) / blanca (todo)** ↔ niveles de conocimiento inicial (externo sin info / externo con info / con acceso).
- **🔴 El pentesting NO garantiza la seguridad** ("garantizar la seguridad es algo que no se puede hacer jamás"). No es sistemático (depende de la habilidad del tester para formular hipótesis); cada test vale para ese sistema en ese momento.
- Si se encuentra una falla **distinta** del objetivo, **se reporta igual** (ej. `/info.txt` con datos de tarjetas).

### 🎯 Señales de examen
- **🔴 Los pasos del pentesting EN ORDEN** con el nombre explícito **"Planteo de hipótesis de falla"** — el prof lo remarcó como "súper importante" y que "va a entrar en el parcial"; de memoria. **Equivocar el orden/pasos PENALIZA.** Eliminación = opcional.
- **🔴 GANCHO/TRAMPA:** si un enunciado afirma que algo **"garantiza la seguridad"** → incorrecto (el prof: "garantizar la seguridad jamás se puede").
- "Explotar es el último recurso"; la prueba debe ser **repetible y conclusiva**.
- Diferenciar verificación formal (prueba ausencia, acotada) de pentesting (prueba existencia).
- Identificar los pasos en los ejemplos (MTS, ingeniería social).

### ⚠️ Errores que penalizan
- Equivocar el **orden** o los **pasos** de la hipótesis de falla.
- Decir que el pentesting **garantiza/prueba ausencia** de vulnerabilidades.
- Olvidar que **explotar es el último recurso** y que la prueba debe ser **repetible**.

### 🔗 Cruces
- **Pivoting / movimiento lateral** ↔ U4 (ciclo de ataque del malware). **CVE/MITRE/OWASP** ↔ I8. **PenTest (tipos, Red Team)** ↔ U5 (evaluaciones de seguridad — se solapan; U5 da el encuadre organizacional, U6 la metodología). **Verificación formal** ↔ U4 (límites: no computable, ciclos, interactividad). **Ingeniería social** ↔ I7 (ataques) e I8 (vulnerabilidad psicológica).

---

# Unidad 7 — Protección de Datos

**Fuente:** U7 · Perspectiva del **CISO**.

### Temas tocados
1. Problema y **ciclo de protección** (Govern→Discover→Protect→Comply→Detect→Respond).
2. **Data Governance**; Data Life Cycle (PwC); **Data Roles** (Trustee/Steward/Custodian/User).
3. **Marco legal:** Ley argentina **25.326** (hábeas data); **GDPR** (derecho al olvido); HIPAA, PCI-DSS, SOX, FFIEC, FDA.
4. **Estados del dato:** at-rest / in-transit / in-use / en memoria + cifrado de cada uno.
5. Enforcement compile-time (análisis estático, GuardedString) vs execution-time (DLP).
6. **Encripción at-rest:** HSM, Keystore, **KEK-DEK** (envelope); at-rest en la nube (Own/HYOK/BYOK/Customer-Managed/Service-Managed).
7. Encripción in-transit (TLS/IPSEC/SSH/GPG); **encripción en memoria (TME/TME-MK)**; SoC/IoT.
8. Comply (retención: HIPAA 6 años, SOX 7 años); **Detect (DLP)**; **Respond** (primeras 24h).
9. **Encripción homomórfica**.

### Lo más importante (hybrid)
- **Dato personal** = identifica a una persona por sí solo. El control de acceso es parte de la solución pero **no alcanza** (el dato fluye por RR.HH.→App→Excel→Email→nube).
- **Data Roles:** Trustee (legal, cabinet-level) → Steward (aprueba accesos) → Custodian (otorga acceso) → User (accede). Baja jerarquía = dominio más chico.
- **🔴 Legal:** **Ley 25.326** (argentina) regula bases de datos públicas y privadas (Art. 9 = seguridad; **hábeas data** = ver/rectificar/borrar tus datos); **NO especifica tecnología** (por eso sigue vigente). El **Derecho al Olvido** lo regula el **GDPR europeo, NO la ley argentina**. **PCI-DSS** → acotar datos por criticidad + **tokenización**. **SOX** = 7 años. **HIPAA** = salud (anonimización), 6 años.
- **Estados + cifrado:** at-rest (HSM/Keystore/**KEK-DEK**) / in-transit (TLS, IPSEC, SSH, GPG) / in-use (TME/TME-MK, DLP) / memoria.
- **KEK-DEK (envelope):** DEK cifra los datos, KEK cifra la DEK. **Rotar la KEK no obliga a re-encriptar los datos**. Se guardan DEK-cifrada + datos-cifrados.
- **Nube (claves):** Own Encryption (independiente del proveedor) → HYOK → BYOK → Customer-Managed (no accede a privadas) → **Service-Managed** (proveedor rota; mín. control, máx. eficiencia).
- **TME** (una sola clave, desencripta en CPU antes del caché, AES-XTS) vs **TME-MK** (multi-clave, KeyID por VM vía VT-x).
- **DLP = DETECTIVO** (no preventivo ni correctivo): alerta al fallar una política; puede detectar cifrado no autorizado (ransomware).
- **Respond (24h):** registrar, alertar, asegurar, **frenar exfiltración**, documentar, forense, notificar. No ocultar; comunicar lo mínimo.
- **Encripción homomórfica:** operar sobre cifrado sin descifrar — `f(Enc_k(x)) = Enc_k(f(x))`. Permite cómputo en la nube preservando privacidad.

### 🎯 Señales de examen
- **🔴 Ley 25.326 (argentina, bases de datos) vs GDPR (europeo, derecho al olvido)** — la distinción más preguntable. El derecho al olvido **NO** es de la ley argentina.
- **KEK-DEK:** por qué rotar la KEK no obliga a re-encriptar.
- Asociar técnica de cifrado a cada **estado del dato**.
- DLP es **detectivo**.
- Propiedad de la **encripción homomórfica** (`f(Enc(x)) = Enc(f(x))`).
- PCI-DSS → tokenización + acotar por criticidad; retenciones HIPAA 6 / SOX 7.

### ⚠️ Errores que penalizan
- Atribuir el **derecho al olvido** a la ley argentina (es GDPR).
- Decir que DLP previene/corrige (es **detectivo**).
- Decir que la ley 25.326 especifica una tecnología (no lo hace, a propósito).

### 🔗 Cruces
- **HSM / anti-tampering / FIPS** ↔ I7 (tokens) e I8 (FIPS nivel 4). **Cifrado in-transit (TLS)** ↔ I8 (export cipher suites) y la Primera Parte. **DLP** ↔ U5 (capa Datos / Network). **Estados del dato / memoria (TME)** ↔ U4 (shared tenancy, Spectre). **Confidencialidad / control de flujo** ↔ I6 (BLP) y U4. **Data roles / governance** ↔ U5 (gobernanza, PAM).

---

# Mapa de cruces (temas que reaparecen en varias unidades)

| Tema | Aparece en | Tratamiento canónico |
|------|-----------|----------------------|
| **Bell–LaPadula / control de flujo** | I6, I8, I9/U4 | I6 (modelo) + U4 (flujo formal) |
| **Entropía / flujo de información** | I8 (intro), I9/U4 | I9/U4 |
| **Riesgo (eliminar/mitigar/compensar/aceptar)** | I8, U5 | U5 (5 opciones) |
| **Threat Modeling / STRIDE** | I8, U5 (DevSecOps) | I8 |
| **VMs / contenedores / sandboxing** | I8, I9/U4, U5 | U4 |
| **Side-channel (Spectre/Rowhammer)** | I8 (shared tenancy), U4 | U4 |
| **Mínimo privilegio / defensa en profundidad** | I8, U5 (PAM, Defense-in-depth) | I8 (principio) / U5 (implementación) |
| **PenTest / Red Team** | U5 (evaluaciones), U6 | U6 (metodología) |
| **Pivoting / movimiento lateral** | U4 (ciclo malware), U6 | ambas |
| **OAuth2 / SSO / federación** | I6 (autorización), I7 (OIDC/SAML) | I6 + I7 |
| **HSM / anti-tampering / FIPS / KEK** | I7, I8, U7 | U7 (KEK-DEK) / I8 (FIPS) |
| **Contexto / Zero-Trust / OPA** | I6, I7 (velocity), U5 (ZTA) | I6 (OPA) + U5 (ZTA) |
| **Ingeniería social / vuln. psicológica** | I7, I8, U6 | I8 (catálogo) + U6 (ejemplo) |
| **CVE / CWE / MITRE / OWASP** | I8, U6 | I8 |
| **DLP / SBOM / exfiltración** | U4 (ransomware), U5, U7 | U5 + U7 |

# Énfasis del profesor en clase (señales NO derivadas de los parciales)

> Únicas señales de prioridad válidas para esta tarea: vienen de lo que el profesor remarcó
> **en clase** (transcripts/informes), no de leer parciales. La cobertura real de los parciales
> la deriva la Fase 2 desde los parciales mismos — incluir aquí citas a parciales sería circular.

| Énfasis (palabras del profe en clase) | Clase | Fuente |
|----------------------------------------|-------|--------|
| **"Amenaza / Vulnerabilidad / Riesgo lo tomamos siempre"** | 8 | I8 |
| **"Injection = 90% de las vulnerabilidades"** ("una sola cosa de la materia") | 8 | I8 |
| **"El primer requisito de seguridad es la disponibilidad"** (un DoS es ataque) | 8 | I8 |
| **Pasos del pentesting de memoria** + "va a entrar en el parcial" | 10b | U6 |
| **"Garantizar la seguridad no se puede jamás"** | 10b | U6 |

---

# Fase 2 — Spec del documento de parciales resueltos

> **Objetivo:** por CADA ejercicio de cada parcial en `Segunda Parte/parciales/`, producir:
> qué temas toca, qué hay que saber para resolverlo (citando este índice y las fuentes I/U),
> notas que expliquen y conecten los temas, y la **resolución** entrelazada.

**Formato del entregable: LaTeX** (no markdown), aprovechando todo el toolset:
- Reusar el **preámbulo/cajas/macros** de `informes-pina/clase07-autenticacion/clase07-autenticacion.tex` como plantilla (paleta ITBA, `tcolorbox`, `tikz`, `amsmath`, `listings`, `hyperref`, `fancyhdr`).
- **Diagramas en TikZ** y **ecuaciones** donde ayuden (no imágenes embebidas).
- **`\href` a los PDFs — todos en la misma carpeta (PORTABILIDAD).** Antes de compilar, **copiar
  dentro de `estudio-parciales/pdfs/`** todos los archivos que el documento referencie, para que
  las rutas relativas sean **simples y portables** (la carpeta se puede mover/compartir y los
  links siguen funcionando — no se busca que sea regenerable, sino portable):
  - **Informes** (`clase06..09-*.pdf`): desde `../informes-pina/claseNN-.../`.
  - **Parciales** (`*.pdf`): desde `../parciales/`.
  - **Unidades de resumen:** ya compiladas standalone en `../resumenes/unidades/0X_*.pdf` (vía
    `subfiles`) → copiarlas.
  - Resultado: los links quedan `\href{file:./pdfs/clase06-....pdf}{...}` — sin `../`, así
    **siguen resolviendo al mover/compartir** la carpeta.
  - ⚠️ *Caveat:* los links a archivos locales dependen del visor (Acrobat/Skim los abren; otros
    no). Usar `\href{run:...}` o `\href{file:...}`. Tenerlos en la propia carpeta (no con `../`)
    maximiza la portabilidad del link.
- Por ejercicio, estructura: **(a) consigna VERBATIM** (copia textual del enunciado; reconstruir las ecuaciones que en los `.doc` salen como `EMBED Equation.3`) → **(b) temas que toca** ("Clase X — Tema") → **(c) qué necesitás saber** → **(d) notas que conectan** → **(e) resolución completa**. En (c)–(e) aplicar el **retrieval en dos pasos** (ver abajo).

**Convenciones:**
- **Nombrar cada unidad como "Clase X — Tema"** (no "Unidad N"): Clase 6 — Políticas y Control de Acceso · Clase 7 — Autenticación · Clase 8 — Principios de Diseño y Vulnerabilidades · Clase 9 — Flujo de Información y Malware · Clase 10 — Seguridad en la Empresa · Clase 10b — Pentesting · Clase 11 — Protección de Datos.
- **Citar SIEMPRE el informe sobre el resumen.** Clases 6–9 → informe (I6–I9). Clases 10/10b/11 → resumen (U5/U6/U7), única fuente disponible.
- **Links con sección si se puede:** los PDFs (informes y unidades de resumen) tienen destinos de `hyperref` → intentar `\href{file:./pdfs/<archivo>.pdf#nameddest=section.N}{...}` o, más portable, `#page=N`. ⚠️ El salto a sección/página depende del visor (Acrobat lo respeta); si no resuelve, degrada a abrir el archivo. Conviene mapear sección→página al armar.
- **PDFs de las unidades de resumen:** ya compilados standalone en `resumenes/unidades/0X_*.pdf` (vía `subfiles`) → copiar a `pdfs/`.
- **`.doc` (Parcial 2 – 2015):** `textutil -convert txt` extrae el texto de forma confiable, pero las ecuaciones embebidas salen como `EMBED Equation.3` → reconstruirlas del original para la consigna verbatim.

**Retrieval en dos pasos (OBLIGATORIO):** el índice es solo el **router**. Cuando un ejercicio
matchea un tema, el agente **abre la fuente específica** (informe `.tex`; resumen solo si no hay
informe) y **extrae el material concreto**: el **ejemplo**, la **explicación específica**, el
**ejercicio resuelto análogo**, la **fórmula/definición exacta**. La resolución y las citas se
fundamentan en ese material, **no** solo en el índice. Leer solo las secciones relevantes (usar
`Grep` para ubicarlas).

**Contrato del fragmento `.tex`** (cada subagente produce un fragmento que se ensambla; **sin
preámbulo**):
- `\section{<label del parcial>}`, y una `\subsection{Ejercicio N — ...}` por ejercicio.
- Consigna textual dentro de `\begin{consigna} ... \end{consigna}` (entorno definido en el maestro).
- Resto con `\paragraph{Temas.}` / `\paragraph{Qué necesitás saber.}` / `\paragraph{Notas y conexiones.}` / `\paragraph{Resolución.}`.
- Matemática con `equation`/`align*`; TikZ permitido (librerías cargadas en el maestro). **No**
  usar otros entornos custom, **no** incluir `\documentclass`/preámbulo, **no** poner `\href`
  (los links se agregan al ensamblar). Escapar caracteres especiales en la consigna.
- Citar la fuente como texto: `Clase X — Tema (\S<sección>)`.

**Parciales a procesar** (en `Segunda Parte/parciales/`):
`Recup.2025Q1.2.pdf`, `Parcial 2v3.doc`, `Parcial.2023Q1.1.pdf`, `Recup.2023Q1.2.pdf`,
`Parcial.2025Q2.2.pdf`, `Segundo Parcial 2013.pdf`, `Parcial.2025Q1.2.pdf`,
`Parcial.2018Q1.2b.pdf`, `Segundo Parcial 2016.pdf`, `Segundo Parcial 2017.pdf`.

**Orquestación sugerida:** subagentes en paralelo (uno por parcial) que leen el PDF
(con `Read` + `pages`), mapean cada ejercicio contra este índice y devuelven su sección
`.tex`; luego se ensamblan en un único documento maestro y se compila/verifica.
