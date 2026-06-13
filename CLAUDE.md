# Playbook: generar resúmenes de estudio en LaTeX para una materia

Este archivo documenta el procedimiento para producir resúmenes de estudio en
LaTeX a partir del material de una asignatura (clases teóricas, transcripts de
las grabaciones, parciales viejos y clase de consulta). Está escrito para
reutilizarse con **cualquier materia**: copiá este `CLAUDE.md` a la carpeta del
material de la nueva asignatura y seguí los pasos.

## Objetivo

Un PDF por "parte"/parcial, con un resumen por **unidad** (= una clase/tema),
pensado para **resolver parciales a futuro**. No es un apunte exhaustivo: prioriza
lo que se evalúa y lo que el profesor remarca.

## REGLAS DE ORO (no negociables)

1. **No agregar NADA que no esté en el material provisto** (PDFs de clase +
   transcripts). No completar con conocimiento externo ni "rellenar" temas.
2. **Marcar lo que el profesor remarca** como importante (extraído de los
   transcripts), con cita textual cuando sea revelador.
3. **Cruzar con los parciales y la clase de consulta**: el resumen debe reflejar
   qué se toma y cómo se espera resolverlo.
4. Si un tema **se toma en los parciales pero NO está en las clases provistas**,
   NO inventarlo: avisar al usuario y dejar que decida (mantener marcado / quitar
   / conseguir la clase fuente). Ver "Manejo de huecos".
5. **Verificar siempre** dónde vive cada tema antes de escribirlo (grep sobre el
   texto extraído de los PDFs). La herramienta Read de PDF SÍ ve diagramas/slides
   como imágenes; `pdftotext` NO (solo texto). Usar ambos para decidir.

## Layout de entrada esperado

Carpetas con el material (los nombres pueden variar; detectarlos al explorar):

```
<materia>/
├── teoricas/  (o clases/)   → PDFs de las clases teóricas, uno por unidad
├── transcripts/             → transcripts de las grabaciones (.vtt / .txt)
├── parciales/               → exámenes viejos (.pdf, .doc)
└── (opcional) un transcript de "clase de consulta"
```

Si el material está dividido en partes (ej. "Primer Parte" / "Segunda Parte"),
tratar cada parte por separado, con su propia carpeta `resumenes/`.

## Salida

```
<materia o parte>/resumenes/
├── main.tex                 → documento maestro (preámbulo + índice + \subfile de cada unidad)
├── unidades/
│   ├── 01_<slug>.tex        → un subfile por unidad
│   ├── 02_<slug>.tex
│   └── ...
├── README.md                → estructura, cómo compilar, notas de alcance
└── main.pdf                 → compilado
```

---

## Procedimiento paso a paso

### 1. Explorar y mapear
- Listar carpetas y archivos. Contar páginas de los PDFs:
  `mdls -name kMDItemNumberOfPages "<archivo>.pdf"`.
- **Mapear cada transcript a su clase.** Los nombres no siempre coinciden. Técnica:
  extraer texto y contar frecuencia de términos distintivos por transcript para
  inferir el tema (ej. "MAC/HMAC" alto ⇒ clase de MACs; "Shamir/TLS" alto ⇒
  protocolos). Ejemplo de conteo:
  ```bash
  for v in transcripts/*.VTT; do
    echo "=== $v ==="
    for kw in "tema1" "tema2" "tema3"; do
      echo -n "[$kw=$(grep -ci "$kw" "$v")] "
    done; echo
  done
  ```

### 2. Extraer texto para poder grepear
```bash
which pdftotext || brew install poppler          # provee pdftotext
for f in teoricas/*.pdf; do pdftotext "$f" "/tmp/$(basename "$f" .pdf).txt"; done
textutil -convert txt "parciales/algo.doc" -output /tmp/algo.txt   # para .doc
```

### 3. Analizar parciales + clase de consulta → "foco de examen"
Delegar a un subagente (Agent tool) la lectura de TODOS los parciales y la clase
de consulta, pidiendo un informe con: (a) foco de examen por unidad, (b)
ejercicios recurrentes y su método de resolución, (c) énfasis de la consulta con
citas textuales, (d) enunciados textuales de cada parcial. Este informe es el
insumo transversal para todas las unidades.

### 4. Verificar dónde vive cada tema evaluado (chequeo de fidelidad)
Antes de generar, grepear los términos clave de los parciales contra el texto de
las clases para saber qué está y qué no:
```bash
for term in "Tema crítico 1" "Tema crítico 2"; do
  echo "### $term"; grep -il "$term" /tmp/*.txt || echo "  (no aparece en ninguna clase)"
done
```
Lo que aparezca en los parciales pero no en ninguna clase → ver "Manejo de huecos".

### 5. Generar una unidad por subagente (en paralelo)
Lanzar los subagentes en una sola tanda (varias tool calls en un mismo mensaje).
A cada uno pasarle: el PDF de su clase (con los rangos de páginas a leer, máx. 20
por Read), el/los transcript(s) mapeados, el foco de examen de ESA unidad, y las
reglas de estilo. Plantilla de prompt al final de este archivo.

### 6. Compilar y pulir
```bash
which tectonic || brew install tectonic
cd <materia>/resumenes && tectonic main.tex          # genera main.pdf
```
- `tectonic` (XeTeX) descarga sus paquetes solo; es el camino más rápido.
- Revisar warnings `Missing character` / `could not represent`: con el preámbulo
  de abajo (que usa `fontspec` bajo XeTeX) los em-dash `—` y signos `¿¡` rinden
  bien. Si igual aparecen, reemplazar `—`→`---` con
  `perl -CSD -i -pe 's/\x{2014}/---/g' archivo.tex`.
- Limpiar archivos intermedios y PDFs sueltos de subfiles:
  `rm -f unidades/*.pdf unidades/*.aux main.aux main.log main.out main.toc`
  (ojo: en zsh un glob sin match aborta el `rm`; borrar por archivo si hace falta).
- **Verificación visual**: leer 2-3 páginas del PDF resultante (incluida una con
  fórmulas / cajas de color) para confirmar el render.

### 7. README y reporte
Escribir `resumenes/README.md` (estructura, cómo compilar, notas de alcance con
los temas que quedaron fuera y por qué).

---

## Manejo de huecos (temas tomados pero no dictados)

Si un tema se evalúa en parciales pero no está en las clases provistas:
**parar y preguntar al usuario** (AskUserQuestion) con opciones tipo:
(a) mantenerlo, marcado en una caja que aclare que no está en estas clases;
(b) quitarlo del todo (estricto con la regla de oro);
(c) que el usuario consiga la clase fuente para generarlo desde ahí.
No decidir por defecto: afecta la fidelidad del material.

---

## Convenciones de estilo LaTeX

Cada unidad es un **subfile** que referencia `../main.tex`. Empieza y termina así:
```latex
\documentclass[../main.tex]{subfiles}
\begin{document}
\section{<Nombre de la unidad>}
... contenido ...
\end{document}
```

Cajas y comandos (definidos en `main.tex`, usar consistentemente):
- `\begin{destacado}...\end{destacado}` — lo que el profesor REMARCÓ en clase.
- `\begin{examen}...\end{examen}` — cómo se toma el tema y cómo resolverlo.
- `\begin{ojo}...\end{ojo}` — error típico que penaliza.
- `\begin{definicion}{Nombre}...\end{definicion}` — definición formal.
- `\prof{frase textual}` — cita textual del profesor.

### Preámbulo `main.tex` (reutilizable, compatible pdfLaTeX + XeTeX/tectonic)
```latex
\documentclass[11pt,a4paper]{article}
\usepackage{iftex}
\ifPDFTeX
  \usepackage[utf8]{inputenc}
  \usepackage[T1]{fontenc}
  \usepackage{lmodern}
\else
  \usepackage{fontspec}
\fi
\usepackage[spanish,es-tabla,es-noshorthands]{babel}
\usepackage{amsmath,amssymb}
\usepackage{geometry}\geometry{margin=2.3cm}
\usepackage{enumitem}
\usepackage{xcolor}
\usepackage{tcolorbox}\tcbuselibrary{skins,breakable}
\usepackage{hyperref}
\hypersetup{colorlinks=true,linkcolor=blue!50!black,urlcolor=blue!50!black,linktoc=all}
\usepackage{subfiles}
\usepackage{array}\usepackage{booktabs}\usepackage{longtable}
\frenchspacing

\newtcolorbox{destacado}[1][]{breakable,enhanced,colback=red!5!white,colframe=red!70!black,
  fonttitle=\bfseries,title={\faExclam Importante --- remarcado en clase},coltitle=white,#1}
\newtcolorbox{examen}[1][]{breakable,enhanced,colback=blue!5!white,colframe=blue!60!black,
  fonttitle=\bfseries,title={\faGraduation En el parcial},coltitle=white,#1}
\newtcolorbox{ojo}[1][]{breakable,enhanced,colback=orange!10!white,colframe=orange!80!black,
  fonttitle=\bfseries,title={\faWarning Cuidado --- error típico},coltitle=black,#1}
\newtcolorbox{definicion}[1]{breakable,enhanced,colback=gray!8!white,colframe=gray!50!black,
  fonttitle=\bfseries,title={Definición: #1},coltitle=white}
\newcommand{\prof}[1]{\medskip\noindent\textit{``#1''}\\[-2pt]\hspace*{1em}{\footnotesize\textcolor{gray}{--- dicho en clase}}\medskip}
\newcommand{\faExclam}{\textbf{!}\ }
\newcommand{\faGraduation}{$\blacktriangleright$\ }
\newcommand{\faWarning}{$\triangle$\ }

\title{\textbf{Resúmenes --- <Materia>}\\[4pt]\large <Parte / Parcial>\\ <Institución>}
\author{Material de estudio basado en clases, transcripts y parciales}
\date{}
\begin{document}
\maketitle\tableofcontents\newpage
\subfile{unidades/01_<slug>}\newpage
\subfile{unidades/02_<slug>}
% ... una línea \subfile por unidad
\end{document}
```

---

## Plantilla de prompt para el subagente de cada unidad

> Generá un resumen de estudio en LaTeX para <MATERIA>. UNIDAD: "<NOMBRE>".
>
> **Fuentes (leé TODO con Read):** PDF `<ruta>` (N págs, leer en rangos de 20:
> 1-20, 21-40, ...); transcript(s) `<ruta(s)>`.
>
> **Regla de oro:** NO agregues nada que no esté en el PDF/transcript. No completes
> con conocimiento externo. Si algo del foco de examen no está en la fuente, no lo
> inventes (avisalo en el resumen de salida).
>
> **Salida:** escribí el archivo `<ruta>/unidades/0X_<slug>.tex` como subfile que
> empieza con `\documentclass[../main.tex]{subfiles}` … `\section{<NOMBRE>}` …
> `\end{document}`.
>
> **Usá las cajas** `destacado` (lo que el profe remarcó, con `\prof{}` para citas),
> `examen` (cómo se toma y cómo resolver), `ojo` (errores que penalizan),
> `definicion{...}`. Matemática con `$...$` y `align` cuando haga falta.
>
> **Foco de examen de esta unidad:** <pegar la sección correspondiente del informe
> del paso 3, con ejercicios recurrentes, métodos paso a paso y errores típicos>.
>
> **Estructura sugerida:** <lista de secciones ordenadas según el PDF>.
>
> Al terminar, verificá balanceo de entornos/llaves/`$` y devolvé un resumen de qué
> incluiste y de cualquier tema del foco que NO estaba en la fuente.

---

## Notas de herramientas (macOS)
- `pdftotext` (paquete `poppler`) y `tectonic` se instalan con `brew`.
- Lectura de PDFs grandes: máximo 20 páginas por llamada a Read; leer por rangos.
- `.doc` viejos → `textutil -convert txt`.
- Para tareas largas con mucha lectura, delegar a subagentes en paralelo (Agent
  tool, varias llamadas en un mismo mensaje) y reservar el contexto principal para
  coordinar y verificar.
