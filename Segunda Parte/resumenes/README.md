# Resúmenes — Criptografía y Seguridad (72.44) · Segundo Parcial

Resúmenes de estudio en LaTeX, una unidad por archivo, generados a partir de:
las clases teóricas (PDF), sus transcripts, la clase de consulta y los parciales.

## Estructura

- `main.tex` — documento maestro (preámbulo + índice). Incluye las 7 unidades.
- `unidades/01_politicas_control_acceso.tex` — Clase 06
- `unidades/02_autenticacion.tex` — Clase 07
- `unidades/03_principios_diseno_vulnerabilidades.tex` — Clase 08
- `unidades/04_flujo_informacion_malware.tex` — Clase 09
- `unidades/05_seguridad_empresa.tex` — Clase 10
- `unidades/06_pentesting.tex` — Clase 10b
- `unidades/07_proteccion_datos.tex` — Clase 11

Cada unidad es un *subfile*: se puede compilar sola o todo junto desde `main.tex`.

## Cajas de color (significado)

- **Importante (rojo)** — frase que el profesor remarcó como importante en clase.
- **En el parcial (azul)** — cómo se toma el tema y cómo resolverlo.
- **Cuidado (naranja)** — error típico que penaliza.
- **Definición (gris)** — definición formal.
- Las citas textuales del profe van en *itálica* con `\prof{}`.

## Cómo compilar

No hay compilador LaTeX instalado en esta máquina. Opciones:

1. Instalar MacTeX: `brew install --cask mactex-no-gui` (o el MacTeX completo).
2. Luego, desde esta carpeta:
   - Documento completo: `pdflatex main.tex` (correr 2 veces para el índice).
   - Una sola unidad: `pdflatex unidades/01_politicas_control_acceso.tex`
3. Alternativa sin instalar nada: subir la carpeta a Overleaf y compilar `main.tex`.

Paquetes usados: `babel(spanish)`, `amsmath`, `tcolorbox`, `subfiles`, `hyperref`,
`booktabs`, `enumitem` — todos estándar en TeX Live / MacTeX / Overleaf.

## Nota de alcance (importante)

Por pedido explícito, los resúmenes contienen **solo** material presente en las
clases 06–11 y sus transcripts. Se excluyeron a propósito temas que **se toman en
los parciales pero NO se dictan en estas clases** (pertenecen a la primera mitad de
la materia, criptografía, clases 1–5, no provistas):

- **Secreto compartido de Shamir** (interpolación de Lagrange) — aparece en casi
  todos los segundos parciales.
- **Esteganografía** y **esquema XOR (n,n)**.
- **One-time-pad / secreto perfecto** con cálculo de entropía.
- **RAID / clustering / alta disponibilidad** y **defensa Man-in-the-Middle**
  (CA/DNS/SSL-TLS/token).

Si conseguís los PDFs/transcripts de esas clases, se pueden generar esas secciones
desde la fuente real.
