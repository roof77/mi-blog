---
title: Modificar la hoja de estilos
date: 2026-07-28T11:00:00
tags:
- hugo
- blog
- blowfish
- css
showHero: true
heroStyle: big
id: 17c2ce22-ee40-4b56-827b-d6aead175cec
pinned: false
created: 2026-07-28T11:39:31.325685878+00:00
modified: 2026-07-29T06:31:02.307766+00:00
---
# Modificar la hoja de estilos

## Como funcionan los esquemas de color

Cada esquema es un archivo independiente que se encuentra en `assets/css/schemes/<nombre-esquema>.css`. Blowfish contruye la paleta con tres colores base:

- primary: El color principal del sitio. Enlaces, botones, etc
- secondary: Color de apoyo para detalles puntuales.
- neutral: Grises/tonos base del sitio: fondo de la página, de las tarjetas, etc.

Cada uno de esos tres colores tiene 10 tonalidades (50 a 900, como lo hace Tailwind). A diferencia de como lo hace un CSS normal, Tailwind CSS 3.0 no usa codigos hexadecimales ni RGB, cada color se define como tres números separados por espacio. Esos tres números corresponden con los tres números del código hexadecimal. Por ejemplo:

```css
--color-primary-500: 139 92 246; /* equivale al hex #8B5CF6 ya que 139 es 8B en hexadecimal, 92 es 5C y 246 es F6 */
```

Para este blog se ha creado un esquema llamado `mi-esquema.css` y para usarlo se ha modificado el fichero `config/_default/params.toml` cambiando la linea:

```toml
colorScheme = "mi-esquema"
```

Este es el css usado:

```css
/* =========================================================
   Esquema de color "mi-esquema" para Blowfish

   MODO CLARO:  Fondo #FAF9F6 | Cabeceras #B7410E | Texto #282727
   MODO OSCURO: Fondo #264653 | Cabeceras #F46B61 | Texto #F8F8FF
   ========================================================= */

/* -----------------------------------------------------------
   1. Paleta base requerida por Blowfish/Tailwind.
   Los valores van en formato "R G B" (sin comas, sin rgb()),
   ya que así los usa Tailwind CSS 3.0 para poder aplicar
   opacidad sobre las variables.
   ----------------------------------------------------------- */
:root {
  /* neutral: fondos, bordes y base de la interfaz.
     Escala generada a partir de #FAF9F6 (H=45°, S=29%);
     el tono 50 ya coincide casi exactamente con el original. */
  --color-neutral-50: 250 248 245;
  --color-neutral-100: 244 242 235;
  --color-neutral-200: 231 227 212;
  --color-neutral-300: 211 203 176;
  --color-neutral-400: 191 179 140;
  --color-neutral-500: 164 146 91;
  --color-neutral-600: 141 125 78;
  --color-neutral-700: 115 102 64;
  --color-neutral-800: 88 79 49;
  --color-neutral-900: 66 58 36;

  /* primary: color de acento principal (cabeceras, enlaces).
     Escala generada a partir de #B7410E (H=18°, S=86%);
     el tono 700 (166 59 13) es el más cercano al original. */
  --color-primary-50: 254 245 241;
  --color-primary-100: 253 234 227;
  --color-primary-200: 250 210 193;
  --color-primary-300: 246 173 141;
  --color-primary-400: 242 135 89;
  --color-primary-500: 237 84 18;
  --color-primary-600: 204 72 16;
  --color-primary-700: 166 59 13;
  --color-primary-800: 128 45 10;
  --color-primary-900: 95 33 7;

  /* secondary: texto y detalles de apoyo.
     Escala generada a partir de #282727 (H=0°, S=1%, casi gris);
     el tono 900 (52 50 50) es el más cercano al original. */
  --color-secondary-50: 247 247 247;
  --color-secondary-100: 240 239 239;
  --color-secondary-200: 222 221 221;
  --color-secondary-300: 195 193 193;
  --color-secondary-400: 167 165 165;
  --color-secondary-500: 129 126 126;
  --color-secondary-600: 111 108 108;
  --color-secondary-700: 90 88 88;
  --color-secondary-800: 70 68 68;
  --color-secondary-900: 52 50 50;
}

/* -----------------------------------------------------------
   Modo oscuro: paleta propia, distinta de la del modo claro.
   ----------------------------------------------------------- */
.dark {
  /* neutral: fondo oscuro.
     Escala generada a partir de #264653 (H=197°, S=37%);
     el tono 800 (43 80 94) es el más cercano al original. */
  --color-neutral-50: 244 249 250;
  --color-neutral-100: 234 242 245;
  --color-neutral-200: 210 227 234;
  --color-neutral-300: 171 204 217;
  --color-neutral-400: 133 180 199;
  --color-neutral-500: 80 148 175;
  --color-neutral-600: 69 127 150;
  --color-neutral-700: 56 104 122;
  --color-neutral-800: 43 80 94;
  --color-neutral-900: 32 59 70;

  /* primary: cabeceras.
     Escala generada a partir de #F46B61 (H=4°, S=87%);
     el tono 400 (243 99 88) es el más cercano al original. */
  --color-primary-50: 254 242 241;
  --color-primary-100: 253 228 226;
  --color-primary-200: 251 197 193;
  --color-primary-300: 247 148 141;
  --color-primary-400: 243 99 88;
  --color-primary-500: 238 32 17;
  --color-primary-600: 205 27 14;
  --color-primary-700: 167 22 12;
  --color-primary-800: 129 17 9;
  --color-primary-900: 95 13 7;

  /* secondary: texto.
     Escala generada a partir de #F8F8FF (H=240°, S=100%);
     el tono 50 (240 240 255) es el más cercano al original. */
  --color-secondary-50: 240 240 255;
  --color-secondary-100: 224 224 255;
  --color-secondary-200: 189 189 255;
  --color-secondary-300: 133 133 255;
  --color-secondary-400: 77 77 255;
  --color-secondary-500: 0 0 255;
  --color-secondary-600: 0 0 219;
  --color-secondary-700: 0 0 179;
  --color-secondary-800: 0 0 138;
  --color-secondary-900: 0 0 102;
}

/* -----------------------------------------------------------
   2. Colores exactos que pediste.
   La paleta de arriba ya deja tonos muy cercanos, pero estas
   reglas fuerzan los valores hex exactos, sin depender de a
   qué tono concreto de la escala tire cada elemento.
   ----------------------------------------------------------- */

/* Modo claro */
html:not(.dark) body {
  background-color: #FAF9F6;
}

html:not(.dark) h1,
html:not(.dark) h2,
html:not(.dark) h3,
html:not(.dark) h4,
html:not(.dark) h5,
html:not(.dark) h6 {
  color: #B7410E;
}

html:not(.dark) p,
html:not(.dark) li,
html:not(.dark) span,
html:not(.dark) article {
  color: #282727;
}

/* Modo oscuro */
html.dark body {
  background-color: #264653;
}

html.dark h1,
html.dark h2,
html.dark h3,
html.dark h4,
html.dark h5,
html.dark h6 {
  color: #F46B61;
}

html.dark p,
html.dark li,
html.dark span,
html.dark article {
  color: #F8F8FF;
}
```

En realidad me da que la parte 2 es la que hace que se usen los colores elegidos, pero la otra es como se debe hacer en Tailwind.

¿Que sentido tiene esa manera? Lo intento explicar a ver si así tambien me entero yo.

Igual que los colores se pueden representar en el formato RBG, también existe el formato [HLS](https://en.wikipedia.org/wiki/HSL_and_HSV) donde H es el matíz, S es la saturacion y L la luminosidad. Lo que hace este método es mantener la H y la S e ir modificando la L. La L se puede calcular como porcentaje, que indica el porcentaje de luminosidad de un color. En cada uno de los valores de

```css
 --color-neutral-50: 250 248 245;
 --color-neutral-100: 244 242 235;
 --color-neutral-200: 231 227 212;
 --color-neutral-300: 211 203 176;
 --color-neutral-400: 191 179 140;
 --color-neutral-500: 164 146 91;
 --color-neutral-600: 141 125 78;
 --color-neutral-700: 115 102 64;
 --color-neutral-800: 88 79 49;
 --color-neutral-900: 66 58 36;
```

hay un color con un porcentaje determinado de luminosidad según esta tabla.

| **Tono** | Luminosidad |
| --- | --- |
| 50 | 97% |
| 100 | 94% |
| 200 | 87% |
| 300 | 76% |
| 400 | 65% |
| 500 | 50% |
| 600 | 43% |
| 700 | 35% |
| 800 | 27% |
| 900 | 20% |

¿Que quiere decir por ejemplo cuando en el css pone que un color “es el más cercano al original”?

```css
  /* neutral: fondo oscuro.
     Escala generada a partir de #264653 (H=197°, S=37%);
     el tono 800 (43 80 94) es el más cercano al original. */
  --color-neutral-50: 244 249 250;
  --color-neutral-100: 234 242 245;
  --color-neutral-200: 210 227 234;
  --color-neutral-300: 171 204 217;
  --color-neutral-400: 133 180 199;
  --color-neutral-500: 80 148 175;
  --color-neutral-600: 69 127 150;
  --color-neutral-700: 56 104 122;
  --color-neutral-800: 43 80 94;
  --color-neutral-900: 32 59 70;
```

Vamos a ver como se calcula la luminosidad de un color.

## Calcular la luminosidad de un color

### Paso 1. Pasar el número de hexadecimal a RGB.

Cojo como ejemplo el #264653 que he puesto antes. Se convierte cada número (separandolo 2 a 2) en decimal.

```text
#264653 → R=38, G=70, B=83
```

### Paso 2. Lo convertimos a un rango de 0 a 1 y encontramos el máximo y mínimo.

Para eso dividimos entre 255 (el máximo valor de un color en RGB)

```text
R = 38/255=0,1490196078 (es el valor mínimo)
G = 70/255=0,2745098039
B = 83/255=0,3254901961 (es el valor máximo)
```

### Paso 3. Aplicamos la fórmula de la luminosidad

```text
L = (Máx + Min) / 2
L = (0,3254901961 + 0,1490196078) / 2
L = 0,4745098039 / 2 
L = 0,237255
```

La luminosidad del color #264653 entonces, convertida en porcentaje es 23.7255 ≈ 23.73 Si nos fijamos en la tabla de antes, el valor de 800 es un color con una luminosidad del 27% y el del 700 tiene un 35%, por lo que el valor del 800 es el más cercano al que queremos.

### Paso 4. Obtener el color con el porcentaje de luminosidad que deseamos

Para #264653 teníamos que:

```text
R = 38/255=0,14902
G = 70/255=0,27451 
B = 83/255=0,32549
Max = 0,32549
Min = 0,14902
Δ = Máx - Min = 0,17647
L = (Max + Min) / 2 = 0,237255 → 23,73%
```

La saturación la calculamos como:

```text
S = Δ / (1 - |2L - 1|)
S = 0,17647 / (1 - |2*0,237255 - 1|)
S = 0,371899 -> 37.19%
```

Para calcular el matiz tenemos que ver el valor mayor de RGB y tendremos:

```text
Si Mayor = R:  H = 60 * (((G' - B') / Δ) mod 6)
Si Mayor = G:  H = 60 * (((B' - R') / Δ) + 2)
Si Mayor = B:  H = 60 * (((R' - G') / Δ) + 4)
```

Como en nuestro ejemplo el mayor valor es el B, tenemos que:

```text
H = 60 × (((R' - G') / Δ) + 4)
H = 60 × (((0,14902 - 0,27451) / 0,17647) + 4)
H = 60 × ((-0,12549 / 0,17647) + 4)
H = 60 × (-0,71114 + 4)
H = 60 × 3,28886
H = 197,33°
```

Con lo que el color es H=197°, S=37,19%, L=23,73%

### Paso 5. Cambiar la L y mantener el resto.

Para el 800, el valor más cercano a la luminosidad que queremos, tenemos que L debe ser 27%, por lo que el color que necesitamos para el 800 sería H=197°, S=37,19%, L=27%

### Paso 6. Convertir el HSL a RGB.

Para eso usamos las siguientes fórmulas.

```text
C = (1 - |2L - 1|) × S
X = C × (1 - |(H/60 mod 2) - 1|)
m = L - C/2
```

Con lo que tenemos

```text
C = (1 - |0,54 - 1|) × 0,3719
C = (1 - 0,46) × 0,3719
C = 0,54 × 0,3719
C = 0,2008

X = 0,2008 × (1 - |(197,33/60 mod 2) - 1|)
X = 0,2008 × (1 - |1,2888 - 1|)
X = 0,2008 × (1 - 0,2888)
X = 0,2008 × 0,7112
X = 0,1428

m = 0,27 - 0,2008/2
m = 0,27 - 0,1004
m = 0,1696
```

### Paso 7. Asignar el (R,G,B) según el sector de 60°

La rueda de color se divide en 6 sectores de 60 grados. Eso es así pq el matiz H va de 0° a 360° y hay 6 colores “puros” (rojo, amarillo, verde, cian, azul, magenta) y cada 60° cambia la combinacion de que canal es dominante (C), cúal es el intermedio (X) y cual el que no está (0).

| Sectores | Rango de H | (R', G', B') |
| --- | --- | --- |
| Sector 0 | 0° ≤ H < 60° | (C, X, 0) |
| Sector 1 | 60° ≤ H < 120° | (X, C, 0) |
| Sector 2 | 120° ≤ H < 180° | (0, C, X) |
| Sector 3 | 180° ≤ H < 240° | (0, X, C) |
| Sector 4 | 240° ≤ H < 300° | (X, 0, C) |
| Sector 5 | 300° ≤ H < 360° | (C, 0, X) |

Para saber en que color cae se divide la H entre 60 y nos quedamos con el entero. En nuestro ejemplo `H = 197,33°`, por lo que hacemos `197,33/60=3,28 → 3`. Es decir, está en el sector 3, que corresponde con (0, X, C).

Calculamos entonces el valor RGB como 

```text
R' = 0 + m = 0,1696
G' = X + m = 0,1428 + 0,1696 = 0,3124
B' = C + m = 0,2008 + 0,1696 = 0,3704
```

### Paso 7. Multiplicar por 255 y redonderar.

```text
R = 0,1696 × 255 ≈ 43
G = 0,3124 × 255 ≈ 80
B = 0,3704 × 255 ≈ 94
```

Y como resultado tenemos que

```text
RGB: 43 80 94
Hex: #2B505E
```

Y el css sería

```text
--color-neutral-800: 43 80 94; /* L=27%, mismo H y S que #264653 */
```

Y con esto termino la turra
