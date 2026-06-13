# Resúmenes — Criptografía y Seguridad (72.44) · Primera Parte (Criptografía)

Resúmenes de estudio en LaTeX, una unidad por archivo, generados a partir de
las clases teóricas (PDF), sus transcripts y los Primer Parcial.

## Estructura

- `main.tex` — documento maestro (preámbulo + índice). Incluye las 5 unidades.
- `unidades/01_introduccion.tex` — Clase 01 (Introducción)
- `unidades/02_cifrado.tex` — Clase 02 (Cifrado: secreto perfecto, OTP, modos, CPA/CCA)
- `unidades/03_macs_cifrado_autenticado.tex` — Clase 03 (MACs, hash, AEAD)
- `unidades/04_asimetrico_firma_digital.tex` — Clase 04 (RSA, Diffie-Hellman, firma, PKI)
- `unidades/05_protocolos.tex` — Clase 05 (KDC/PKI, SSL/TLS, **Secreto compartido de Shamir**)

Cada unidad es un *subfile*: se puede compilar sola o todo junto desde `main.tex`.

## Cajas de color (significado)

- **Importante (rojo)** — frase que el profesor remarcó como importante en clase.
- **En el parcial (azul)** — cómo se toma el tema y cómo resolverlo.
- **Cuidado (naranja)** — error típico que penaliza.
- **Definición (gris)** — definición formal.
- Las citas textuales del profe van en *itálica* con `\prof{}`.

## Cómo compilar

Con `tectonic` (instalado): `tectonic main.tex` desde esta carpeta → `main.pdf`.
Alternativa: `pdflatex main.tex` (2 veces, para el índice) o subir a Overleaf.

El preámbulo detecta el motor (`iftex`): usa `fontspec` bajo XeTeX/tectonic e
`inputenc/fontenc/lmodern` bajo pdfLaTeX, así compila bien en cualquier lado.

## Notas de alcance

- **Shamir (secreto compartido por interpolación de Lagrange)**: está en la Clase 05
  y quedó desarrollado a fondo (método + ejemplo (3,5) en Z₁₁ del PDF + ejemplo
  extra (3,4) en Z₇). Se toma sobre todo en el *segundo* parcial.
- **Shamir's no-key protocol** (distinto del anterior) se toma en el primer parcial
  (2017) y está en la unidad 5.
- **Esteganografía** y **encripción homomórfica** no aparecen en estas clases
  (son ítems sueltos de parcial sin teoría asociada en el material provisto).
