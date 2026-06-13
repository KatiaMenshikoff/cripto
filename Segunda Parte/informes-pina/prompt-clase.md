# Prompt para generar apuntes integrales (teoría + práctica) en LaTeX

Copiá el bloque de abajo en un nuevo Claude, **reemplazando los valores entre `<...>`**
por los de la clase que quieras procesar. Está pensado para correr desde el directorio
`Segunda Parte/`.

---

## PROMPT (copiar desde acá)

Vamos a crear un apunte en LaTeX que incluya la **totalidad** de la información de una clase
de Criptografía y Seguridad, integrando **parte teórica y práctica**.

**Materiales de esta clase** (`<NOMBRE_TEMA>`):
- Clase teórica (transcripts): `<transcripts-teoricas/claseNNa.VTT>` + `<transcripts-teoricas/claseNNb.VTT>`
- Presentación teórica: `<teoricas/Clase NN - Tema.pdf>`
- Clase práctica (transcript): `<transcripts-practicas/claseN-tema.md>`
- Presentación práctica: `<practicas/Clase N.pdf>`

**Procedimiento (seguilo en orden):**

1. **Cargá todo a contexto antes de escribir nada.** Leé en su totalidad:
   - Los dos transcripts teóricos (`.VTT`) — vienen partidos, leé todos los offsets.
   - El transcript práctico (`.md`) — suele ser una sola línea larga, leelo entero.
   - **Todas** las diapositivas de ambos `.pdf` con la herramienta Read y el parámetro
     `pages` (máx. 20 páginas por llamada). Los PDF se renderizan como imágenes: mirá los
     diagramas, fórmulas y figuras, no solo el texto.

2. **Verificá que transcripts y presentaciones matcheen** antes de empezar. Confirmá que la
   secuencia de temas del transcript teórico sigue las slides teóricas, e ídem práctica.
   Reportá brevemente el resultado del cross-check (qué cubre cada uno, qué agrega la
   práctica). Si NO matchean, frená y avisá.

3. **Escribí el informe en LaTeX** en una carpeta nueva:
   `informes-pina/claseNN-<tema>/claseNN-<tema>.tex`.
   - Usá la extensión completa de LaTeX y todas las herramientas disponibles:
     `tikz` (con `arrows.meta`, `shapes.symbols`, `positioning`, `calc`, ...), `tcolorbox`,
     `amsmath/mathtools`, `listings`, `booktabs`, `hyperref`, `fancyhdr`, `titlesec`.
   - **Recreá los diagramas de las diapositivas en TikZ** (no embebas imágenes): diagramas
     de flujo, secuencias de mensajes (protocolos), arquitecturas, etc. Generá diagramas
     propios donde ayuden a entender.
   - Transcribí **todas** las fórmulas con notación matemática correcta y **resolvé los
     ejercicios** que aparezcan en las slides.
   - Integrá teoría y práctica: usá cajas diferenciadas (p. ej. una caja verde para los
     aportes/aclaraciones de la clase práctica, cajas de definición, de atención/ataque, y
     cajas que reproduzcan recuadros de las slides).
   - Incluí las **anécdotas, aclaraciones y respuestas a preguntas** relevantes del profe
     que enriquezcan el contenido (no solo el texto pelado de las slides).
   - Portada, índice (`tableofcontents`), encabezados, y referencia a la lectura recomendada.

4. **Estilo visual:** respetá la identidad de la cátedra. Paleta cian/celeste ITBA
   (`#6EC6D9` / `#2E8B9E` / `#AEDCEA`), verde para la práctica (`#4F8A3D`), rojo para
   ataques/atención. Mirá `informes-pina/clase07-autenticacion/clase07-autenticacion.tex`
   como **plantilla de referencia** — reusá su preámbulo, cajas y macros.

5. **Compilá y verificá:**
   - `cd "informes-pina/claseNN-<tema>" && pdflatex -interaction=nonstopmode -halt-on-error claseNN-<tema>.tex` (dos veces, para el índice).
   - Si usás `babel` español con TikZ, acordate de `es-noshorthands` (si no, `>=Stealth` rompe).
   - Cargá las librerías TikZ que uses (p. ej. `shapes.symbols` para `cloud`).
   - Leé el PDF resultante (páginas clave: portada, un diagrama, una fórmula) para confirmar
     que renderiza bien. No declares el trabajo terminado sin haber compilado a PDF sin
     errores y revisado el output.
   - Limpiá los auxiliares (`.aux .out .toc .log`) al final, dejá solo `.tex` y `.pdf`.

**Notas:**
- El idioma del apunte es español.
- No inventes contenido que no esté en los materiales; si algo queda ambiguo en el
  transcript, contrastalo con la slide correspondiente.

## FIN DEL PROMPT

---

### Estado de las clases

| Clase | Tema | Estado |
|-------|------|--------|
| 7 | Autenticación | ✅ hecho — `clase07-autenticacion/` |
| 8 | Principios de Diseño de Seguridad y Flujo de Información | ✅ hecho — `clase08-principios-diseno-seguridad/` |
| 9 | Flujo de Información y Malware / Seguridad en aplicaciones (CWE Top 25) | ✅ hecho — `clase09-flujo-informacion-malware/` |

(ir completando a medida que se generen)
