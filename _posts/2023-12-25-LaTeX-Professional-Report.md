---
title: "Reporte profesional en LaTeX"
date: 2023-12-25
categories: [Professional Report]
tags: [report, latex, tex]
image:
    path: /assets/thumbnail/latex.png
---


# Reporte profesional en LaTeX

## Introducción
LaTeX **es un sistema de composición tipográfica de alta calidad**. Incluye características especialmente diseñadas para la producción de documentación científica y ténica. LaTeX es el estándar para publicación de documentos científicos pero puede usarse para cualquier tipo de publicación.

## Instalación

Ejecutar los siguientes comandos como root.

```bash
apt update
apt install latexmk zathura
apt install texlive-full

# Latexmk y zathura se utilizan para visualizar 
# los documentos LaTeX, sin embargo, no se utiliza 
# zathura por default, para que lo haga, se ejecutarán 
# los siguientes comandos.
xdg-mime query default application/pdf
xdg-mime default zathura.desktop application/pdf

# Crear directorio
cd /home/bryan/.config/
mkdir latexmk
cd latexmk
nvim latexmkrc
# Contenido de latexmkrc:
	$pdf_previewer = "zathura";

# Crear un enlace simbólico en el directorio de root
cd /root/.config/
mkdir latexmk
cd latexmk
ln -s -f /home/bryan/.config/latexmkrc latexmkrc
```

# Creación del documento

### Puntos a tomar en cuenta

El documento debe tener el formato “.tex”. ejemplo: reporte.tex

Para ver el documento en el navegador, ingresar: file:///path/to/file.pdf

Los comentarios se ponen con un “%”. ejemplo: % esto es un comentario.

Si se representan scripts largos, estos se pueden acortar quitando comentarios o eliminando comparativas como verificaciones de versión o códigos de estado que sirven para verificar que el script funcionará, es decir, si se sabe que el script funcionará, todo eso se puede quitar.

Terminología:
“pt”: Puntos tipográficos.
“em”: Espacio de fuente.

### Ejemplo de estructura de un reporte

```latex
Portada
índice

Antecedentes
Objetivos
	Alcance
	Impedimentos y limitaciones
	Resumen general
Reconocimiento
	Enumeración de servicios expuestos
	Enumeración de servidores web
	Enumeración de subdominios
	Enumeración de paneles de autenticación
Identificación y explotación de vulnerabilidades
Escalada de privilegios
Contramedidas y buenas prácticas
Concluciones
```

### Instrucciones

- Overleaf templates
    
    ```latex
    {% raw %}
    % CAJAS:
    \usepackage{tikz,lipsum,lmodern} % Paquetes
    \usepackage[most]{tcolorbox}     % requeridos
    
    % Caja1
    \begin{tcolorbox}[colback=red!5!white,colframe=red!75!black]
      Texto de ejemplo
    \end{tcolorbox}
    
    % Caja2
    \begin{tcolorbox}[enhanced,attach boxed title to top center={yshift=-3mm,yshifttext=-1mm},
      colback=blue!5!white,colframe=blue!75!black,colbacktitle=nombreColor!80!black,
      title=My title,fonttitle=\bfseries,
      boxed title style={size=small,colframe=red!50!black} ]
    	Esto es un texto de ejemplo
    \end{tcolorbox}
    
    % TEXTO CON FORMATO CÓDIGO:
    \usepackage{listings}
    \definecolor{codegreen}{rgb}{0,0.6,0}
    \definecolor{codegray}{rgb}{0.5,0.5,0.5}
    \definecolor{codepurple}{rgb}{0.58,0,0.82}
    \definecolor{backcolour}{rgb}{0.95,0.95,0.92}
    \lstdefinestyle{mystyle}{
        backgroundcolor=\color{backcolour},   
        commentstyle=\color{codegreen},
        keywordstyle=\color{magenta},
        numberstyle=\tiny\color{codegray},
        stringstyle=\color{codepurple},
        basicstyle=\ttfamily\footnotesize,
        breakatwhitespace=false,         
        breaklines=true,                 
        captionpos=b,                    
        keepspaces=true,                 
        numbers=left,                    
        numbersep=5pt,                  
        showspaces=false,                
        showstringspaces=false,
        showtabs=false,                  
        tabsize=2
    }
    \lstset{style=mystyle}
    % (Opcional) Cambiar el nombre por defecto "Listing"
    \renewcommand{lstlistingname}{nombreCustom}
    
    % Para insertar el texto como código
    \begin{lstlisting}[language=Python, caption=Esto es un caption]
    Aquí va el código...
    \end{lstlisting}
    {% endraw %}
    ```
    

Primero se debe Indicar el formato de plantilla.

```latex
{% raw %}
% Esto crear un folio de dimensiones normales
\documentclass[a4paper]{article}
{% endraw %}
```

Añadir soporte a caracteres latinos

```latex
{% raw %}
\usepackage[utf8]{inputenc}
\usepackage[spanish]{babel}
{% endraw %}
```

Indicar el inicio y final del documento.

```latex
{% raw %}
\begin{document} % Se indica en el tope del archivo
	Contenido del documento...
\end{document} % Se indica al final del archivo
{% endraw %}
```

Compilar el documento.

```latex
% Creará un documento PDF.
latexmk -pdf reporte.tex

% Creará un documento PDF y abrirá una nueva ventana 
% con el documento, esto funciona para hacer cambios 
% en tiempo real e incluso muestra los errores en 
% tiempo real.
latexmk -pdf reporte.tex -pvc
	% Simbología
	? % Indica que ocurrió un error
	presionar ENTER % Itera en cada error
	X y presionar ENTER % Se salta todas las 
	% iteraciones de errores y va hasta el final
```

Definir un título.

```latex
{% raw %}
\begin{titlepage}
	Esto es un título
\end{titlepage}
{% endraw %}
```

Centrar elementos.

```latex
{% raw %}
\centering
Esto se verá centrado

\begin{center}
	Esto se verá centrado
\end{center}

% Quitar el efecto de centering:
\usepackage{ragged2e}

\justifying
{% endraw %}
```

Insertar imágenes.

```latex
{% raw %}
% Paquete requerido:
\usepackage{graphicx}

% Dentro de "[]" se inidican las domensiones de la imagen (ancho, 
% altura, etc) y dentro de "{}" se indica la ruta de la imagen 
% partiendo del directorio donde se encuentra el documento.
\includegraphics[]{}
	% Opciones dentro de [] (separados por coma):
	% Cambiar proporción: width=0.8\paperwidth
	width=\textwidth % La imagen toma la proporción del texto.
	width=\paperwidth % La imagen toma la proporción de la página 
	% (si la imagen se inserta incorrectamente, utilizar makebox).
	height=1cm % Ajustar la altura
	keepaspectratio % Cambia la altura de forma proporcional
	

% Insertar imagen utilizando makebox:
\makebox[]{}
	% Ejemplo
	\makebox[\textwidth]{\includegraphics[width=\textwidth]{images/test.jpg}}
{% endraw %}
```

Aplicar saltos de línea y espaciados.

```latex
{% raw %}
% Aplicar salto de línea:
\par

% En textos se usa:
\\

% Aplicar un espaciado vertical:
\vspace{1cm}

% Aplicar un epaciado vertical equitativo:
\vfill

% Aplicar un espaciado vertical entre cada línea 
% de los textos:
\usepackage{setspace}
\setstretch{1.9}

% Aplicar un espaciado vertical entre párrafos:
\setlength{\parindent}{0pt}
% Establecer el espacio entre párrafos en 1em más 0.5em de 
% espacio extra permitidio con una reducción máxima de 0.2em.
\setlength{\parskip}{0.8em plus 0.5em minus 0.2em}
\setlength{\parfillskip}{\parindent plus 1fill}

% Aplicar un espaciado horizontal
Texto de \quad ejemplo
Texto de \qquad ejemplo % Espaciado doble
{% endraw %}
```

Customización de texto.

```latex
{% raw %}
% Hacer la letra más grande:
{\scshape\LARGE Texto de ejemplo}
{\Huge Texto de ejemplo}

% Hacer la letra en negrita:
\textbf{Texto de ejemplo}

% Fuentes
	\texttt{Texto de ejemplo}
{% endraw %}
```

Declaración de variables.

```latex
{% raw %}
\newcommand{\nombreDeVariable}{valorDeVariable}
	% Ejemplo: \newcommand{\logoPortada}{images\test.jpg}

% Para hacer alusión a una variable, se usa su nombre:
\nombreDeVariable
{% endraw %}
```

Definir colores.

```latex
{% raw %}
% Paquete requerido:
\usepackage{xcolor}
\usepackage[table,xcdraw]{xcolor} % Añade soporte para colores 
% en tablas

% Dentro de los "{}" se establece primero un nombre personalizado 
% para el color y en el tercero se indica el color en hexadeximal.
\definecolor{nombreParaColor}{HTML}{146c8a}

% Para hacer alusión al color, se usa su nombre:
{\textcolor{nombreParaColor}{Texto de ejemplo}}
{% endraw %}
```

Cambiar los márgenes del documento.

```latex
{% raw %}
\usepackage[margin=2cm, top=2, includefoot]{geometry}
{% endraw %}
```

Crear una nueva página.

```latex
\clearpage
Esto es una prueba
```

Crear un índice.

El índice se completa automáticamente.

```latex
{% raw %}
% (Opcional) Hacer que el índice cree hipervínculos:
\usepackage{hyperref}
\usepackage[hidelinks]{hyperref} % Ocultar el subrayado

% (Opcional) Cambiar el nombre por defecto "índice":
\addto\captionsspanish{\renewcommand{\contentsname}{Nombre personalizado}}

\clearpage
\tableofcontents
\clearpage
{% endraw %}
```

Crear un estilo de página.

```latex
{% raw %}
% Esto creará una barra en la cabecera de la página
% Paquete requerido:
\usepackage{facyhdr}

\setlength{\headheight}{30.2pt} % Crear un espaciado entre el 
% tope de la página y el estilo
\pagestyle{fancy}
\fancyhf{}
\lhead\includegraphics[width=4cm]{\nombreDeLogo} % Incluir logo al 
% lado izquierdo

\renewcommand{\headrulewidth}{3pt} % Aumenta el grosor de la barra

% Cambiar el color de la barra:
\renewcommand{\headrule}{\hbox to\headwidth\color{nombreDeColor}\leaders\hrule height \headrulewidth\hfill}}
{% endraw %}
```

Crear secciones.

```latex
{% raw %}
% (Opcional) Eliminar sangrías del texto:
\usepackage{parskip}

\section{Nueva sección}
Esto es un texto de ejemplo

% Subsección
\subsection{Nueva subsección}
Esto es un texto de ejemplo
{% endraw %}
```

Crear links o hipervínculos.

```latex
{% raw %}
\href{https://test.com}{textbf{\color{nombreColor}Texto de ejemplo}}
{% endraw %}
```

Crear viñetas.

```latex
{% raw %}
\begin{itemize}
	\item Esto es un texto de ejemplo
\end
{% endraw %}
```

Crear figuras.

```latex
{% raw %}
% (Opcional) Cambiar el nombre por defecto "Figura"
\usepackage[figurename=nombrePersonalizado]{caption}

\begin{figure}[h] % "h" indica que se quiere incluir la imagen 
% en ese mismo lugar
	\includegraphics[width=\textwidth]{images/test.jpg}
	\caption{Esto es un caption}
\end{figure}

% Figura con bordes:
\begin{figure}[h] % "h" indica que se quiere incluir la imagen 
% en ese mismo lugar
	\setlength{\fboxrule}{1pt}
	\fbox{\includegraphics[width=\textwidth]{images/test.jpg}}
	\caption{Esto es un caption}
\end{figure}

% Crear un label:
\label{fig:customLabelName} % Va dentro de la figura
	% Referenciar número de figura:
	\ref{fig:customLabelName}
	% Referenciar el número de página en donde se encuentra 
	% la figura:
	\pageref{fig:customLabelName}
{% endraw %}
```

Numerar las páginas.

```latex
{% raw %}
\cfoot{\thepage}
{% endraw %}
```

Crear diagrama.

```latex
{% raw %}
% Paquetes requeridos:
\usepackage{tikz,lipsum,lmodern}

\begin{tikzpicture}[node distance=2cm every node/.style={rectangle, draw, fill=white}]
	\node (center) {nombreNodo};

	% Colocar nodo abajo a la izq del nodo central con distancia de 3cm:
	\node (nodo1) [below left of=center, node distance=3cm] {nombreNodo2};
	% Colocar nodo abajo a la derecha del nodo central con distancia de 3cm:
	\node (nodo2) [below right of=center, node distance=3cm] {nombreNodo3};
	% Dibujar líneas de un nodo a otro:
	\node (center) -- (nodo1); % Línea del centro al nodo1
	\node (center) -- (nodo2); % Línea del centro al nodo2
\end{tikzpicture}
{% endraw %}
```

Crear una tabla.

```latex
{% raw %}
\begin{tabular}{ c | c } % Crea dos columnas centradas
	{columna1} & {columna2} \\
	\hline % Línea delimitadora
	elemento1 & elemento2 \\
	elemento3 & elemento4
\end{tabular}

% Simboología:
c % centered
{% endraw %}
```

Caja para definiciones.

```latex
{% raw %}
% Incorporación de colores y fuentes
\newtcolorbox{definicion}{
	breakable,
	enhanced,
	colback=white,
	colframe=nombreColor!75!black,
	arc=0mm,
	boxrule=1pt,
	leftrule=12mm,
	fonttitle=\bfseries,
	coltitle=blue!75!black,
	title=Definición,
	attach title to upper=\par,
}

% Para insertar la caja de definición
\begin{definicion}
Texto de ejemplo
\end{definicion}
{% endraw %}
```

### Más info:

- Links
    
    overleaf(cajas) - (”[https://www.overleaf.com/latex/examples/drawing-coloured-boxes-using-tcolorbox/pvknncpjyfbp](https://www.overleaf.com/latex/examples/drawing-coloured-boxes-using-tcolorbox/pvknncpjyfbp)”)
    
    overleaf(código) - (”[https://www.overleaf.com/learn/latex/Code_listing](https://www.overleaf.com/learn/latex/Code_listing)”)
