---
title: "Crear un blog personal con Hugo y Blowfish"
date: 2026-07-28T10:00:00
tags: ["hugo", "blog" , "blowfish"]
showHero: true
heroStyle: "big" # "big" | "background" | "thumbAndBackground" | "basic"
---

# Crear un blog personal con Hugo y Blowfish (a mi manera)

## 1. Requisitos previos.

Comprobar que tenemos hugo extended

```bash
# Comprobar que tienes Hugo extended (obligatorio para Blowfish, usa Tailwind/pipes)
hugo version
# Debe decir "extended"
```

# 2. Crear el repositorio en github.

Creamos el repositorio en github con el nombre que vayamos a usar. En este caso será ```mi-blog```.

Descargamos el repositorio a nuestro disco duro con el comando

```
git clone https://github.com/TU-NOMBRE/mi-blog.git
```

Entramos dentro de la carpeta mi-blog y ejecutamos:

```
hugo new site . --force
```

Para forzar la creación del sitio de Hugo.

Lo normal sería crear el sitio de Hugo, hacer un ```git init``` y subirlo luego con un commit y un push, pero no se tanto de git

## 3. Instalar Blowfish como submódulo git

```
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
git submodule update --init --recursive
```

## 4. Preparar la estructura de configuración

Blowfish no usa un único `hugo.toml`, sino una carpeta `config/_default/` con varios archivos separados. Primero, borra el `hugo.toml` que generó Hugo por defecto:

```bash
rm hugo.toml
mkdir -p config/_default
```

Copia los archivos de configuración de ejemplo que trae el propio tema (ruta válida para instalación por submódulo, distinta de la de Hugo Modules):

```bash
cp themes/blowfish/config/_default/*.toml config/_default/
```

Esto te deja esta estructura:

```
config/_default/
├── hugo.toml
├── languages.en.toml
├── markup.toml
├── menus.en.toml
└── params.toml
```

> **Importante**: no se copia `module.toml` porque ese archivo es exclusivo de la instalación vía Hugo Modules. Como usas submódulo, no lo necesitas.

## 5. Activar el tema en `config/_default/hugo.toml`

Al instalar por submódulo (no por Hugo Modules), hay que indicar explícitamente qué tema usar. Abre `config/_default/hugo.toml` y ajústalo así:

```toml
baseURL = "https://TU_USUARIO.github.io/mi-blog/" # URL final donde vivirá el sitio publicado
title = "Mi Blog" # Título del sitio
theme = "blowfish" # Obligatorio en instalación por submódulo; le dice a Hugo qué carpeta de themes/ usar
defaultContentLanguage = "es" # Debe coincidir exactamente con el código de idioma del archivo languages.es.toml (paso 6)
```

Si `defaultContentLanguage` no coincide con ningún archivo `languages.<código>.toml` presente en `config/_default/`, Hugo falla con el error `defaultContentLanguage "es" not found in languages configuration`.

## 6. Configurar el idioma en `config/_default/languages.es.toml`

Renombra el archivo de idioma (viene en inglés por defecto) y ajusta su contenido:

```bash
mv config/_default/languages.en.toml config/_default/languages.es.toml
```

Las claves van **directamente en el nivel superior del archivo**, sin ninguna tabla `[es]` envolviéndolas — el propio nombre del archivo ya le dice a Hugo a qué idioma pertenece:

```toml
disabled = false # Si es true, este idioma queda desactivado
locale = "es" # Código de idioma (reemplaza a la antigua clave "languageCode", deprecada en Hugo 0.158+)
label = "español" # Nombre mostrado en el selector de idiomas (reemplaza a "languageName")
weight = 1 # Prioridad del idioma si hubiera varios
title = "Mi Blog" # Título real de tu blog (sustituye el placeholder "Blowfish" de la plantilla de ejemplo)

[params]
  displayName = "ES" # Abreviatura mostrada en el selector de idiomas
  isoCode = "es"
  rtl = false # false para español (se escribe de izquierda a derecha)
  dateFormat = "2006-01-02" # Formato de fecha ISO-8601 (AAAA-MM-DD), usando la fecha de referencia de Go

[params.author]
  name = "Tu nombre" # Descomenta y rellena si usas el layout "profile" en portada
  email = "tu@email.com"
  image = "img/avatar.jpg"
  headline = "Una frase breve sobre ti"
  bio = "Un poco más de contexto sobre ti o tu blog"
  links = [
    { github = "https://github.com/TU_USUARIO" },
  ]
```

> Nota sobre `dateFormat`: Go/Hugo usan una fecha de referencia fija (`2006-01-02 15:04:05`) en vez de tokens como `YYYY-MM-DD`. Para el formato ISO-8601 de solo fecha, el valor exacto es `"2006-01-02"`. Si además necesitas hora y zona horaria, usa `"2006-01-02T15:04:05Z07:00"`.

**¿Necesitas `content/es/`?** Solo si vas a tener varios idiomas simultáneos en el sitio. Con un único idioma configurado (como en esta guía), el contenido va directamente en `content/`, sin subcarpeta de idioma.

## 7. Configurar `config/_default/params.toml`

Edita los valores clave para colores, modo claro/oscuro, tags y búsqueda. Estas opciones están en el nivel superior del archivo (no anidadas bajo `[colorScheme]` o `[theme]` como en versiones antiguas del tema):

```toml
colorScheme = "blowfish" # Esquema de color predefinido: "blowfish", "avocado", "fire", "ocean", "forest", "princess", "neon", "noir"
defaultAppearance = "light" # Tema por defecto: "light" o "dark" (no existe "auto" como valor; el cambio automático se controla con autoSwitchAppearance)
autoSwitchAppearance = true # Si es true, cambia automáticamente entre claro/oscuro según la preferencia del sistema del visitante
enableSearch = true # Activa la búsqueda con Fuse.js (ya viene activada por defecto)

[homepage]
  layout = "profile" # Layout de portada: "page", "profile", "hero", "card", "background", "custom"
  showRecent = true # Muestra los artículos recientes en la portada

[article]
  showTags = true # Solo se muestran si showTaxonomies (abajo) también es true
  showTaxonomies = true # Activa la visualización de categorías/etiquetas en cada artículo
  showTableOfContents = true # Muestra la tabla de contenidos generada automáticamente
```

> **Nota**: en la versión actual del tema no existe un bloque `[list]` con `showTags` para las páginas de listado — las etiquetas de cada artículo se controlan únicamente desde `[article]`, con `showTaxonomies = true` como interruptor general y `showTags` / `showCategories` para elegir cuál de las dos taxonomías mostrar.

Si quieres revisar el resto de opciones disponibles (fecha, autor, vistas, "me gusta", modo zen de lectura, botones para compartir, etc.), deja el resto del archivo tal como lo copiaste del tema y ajusta solo lo que necesites — `params.toml` viene ya documentado línea a línea con comentarios.

## 8. Menú de navegación en `config/_default/menus.es.toml`

Renombra también el archivo de menús para que coincida con el idioma:

```bash
mv config/_default/menus.en.toml config/_default/menus.es.toml
```

```toml
[[main]]
  name = "Buscar" # Texto mostrado para el enlace
  pageRef = "search" # Referencia a la página de búsqueda
  weight = 10 # Orden de aparición (menor peso = más a la izquierda)

[[main]]
  name = "Etiquetas"
  pageRef = "tags"
  weight = 20
```

## 9. Cabecera general del sitio (imagen en portada)

A diferencia de PaperMod, Blowfish permite añadir una imagen de cabecera a la portada sin tocar ningún partial del tema — es una opción de configuración nativa.

### 9.1 Elegir el layout

En `config/_default/params.toml`, bajo `[homepage]`:

```toml
[homepage]
  layout = "background" # o "hero", según el efecto que prefieras
  homepageImage = "img/cabecera.jpg" # ruta a tu imagen
  showRecent = true
  showRecentItems = 5
  layoutBackgroundBlur = false # solo aplica si layout = "background"; añade desenfoque a la imagen
```

- **`"hero"`**: la imagen queda contenida en la parte superior, junto con tu información de autor y el contenido markdown debajo.
- **`"background"`**: versión más suave, con la imagen ocupando todo el fondo de la portada.

### 9.2 Dónde colocar el archivo de imagen

**Opción A — Procesada por Hugo (recomendada):**

```bash
mkdir -p assets/img
cp /ruta/a/tu/imagen.jpg assets/img/cabecera.jpg
```

Referencia sin barra inicial en `params.toml`:
```toml
homepageImage = "img/cabecera.jpg"
```

Con esto, Hugo optimiza y redimensiona automáticamente la imagen (controlado por `backgroundImageWidth`, que aparece comentado en `params.toml` como `# backgroundImageWidth = 1200`).

**Opción B — Archivo estático sin procesar:**

```bash
mkdir -p static/img
cp /ruta/a/tu/imagen.jpg static/img/cabecera.jpg
```

Referencia **con** barra inicial:
```toml
homepageImage = "/img/cabecera.jpg"
```

### 9.3 Tamaño recomendado de la imagen

| Layout | Ancho | Alto | Proporción |
|---|---|---|---|
| `"background"` | 1920-2560 px | 1080-1440 px | Panorámica ancha (16:9), cubre toda la ventana del navegador |
| `"hero"` | 1600-1920 px | 600-900 px | Panorámica, tipo 21:9 o 16:6, menos alta que "background" |

Recomendaciones generales:
- **Peso del archivo**: idealmente bajo 500 KB, aunque al cargar solo en portada no es tan crítico como una imagen presente en todas las páginas.
- **Punto de interés centrado**: la imagen se recorta tipo `cover` para llenar el contenedor, así que evita elementos importantes pegados a bordes o esquinas.
- Si usas la Opción A (procesada), puedes subir el original más grande sin miedo al peso final, ya que Hugo la comprime. Para más nitidez en pantallas grandes, sube el original en la parte alta del rango y ajusta también:
  ```toml
  backgroundImageWidth = 1920
  ```

## 10. Cabecera por artículo

En el front matter de cada post:

```yaml
---
title: "Mi primer artículo"
date: 2026-07-28
tags: ["hugo", "blog"]
showHero: true
heroStyle: "big" # imagen normal al principio del artículo, no como fondo detrás del título
heroCaption: "Pie de foto opcional" # solo se muestra con heroStyle: big
---
```

Las opciones de `heroStyle` son:
- **`"basic"`**: imagen pequeña/discreta.
- **`"big"`**: la imagen se muestra como un bloque propio al principio del artículo, con posibilidad de leyenda (`heroCaption`). Es la opción recomendada si no quieres que la imagen ocupe todo el fondo.
- **`"background"`**: la imagen actúa como fondo de toda la cabecera del artículo, con efecto de difuminado al hacer scroll (activable/desactivable con `layoutBackgroundBlur`).
- **`"thumbAndBackground"`**: combina una miniatura con fondo difuminado.

Y coloca la imagen como `featured.jpg` en la misma carpeta del post (page bundle).

## 11. Estructura de contenido recomendada (page bundles)

```
content/
  posts/
    mi-primer-articulo/
      index.md
      featured.jpg
```

## 12. Personalizar colores propios

Si los esquemas predefinidos (`blowfish`, `avocado`, `fire`, etc.) no te convencen, crea uno propio en `assets/css/schemes/mi-esquema.css`:

```css
:root,
:root[data-scheme="mi-esquema"] {
  --color-primary: #1a1a1a;
  --color-neutral: 220 13% 18%;
  --color-accent-500: 210 90% 55%;
}
```

Y en `config/_default/params.toml`: `colorScheme = "mi-esquema"`.

## 13. Publicar en GitHub Pages

La ventaja de instalar Blowfish por submódulo (en vez de como Hugo Module) es que el workflow de despliegue es más simple: **no necesita Go**, solo Hugo.

Crea `.github/workflows/hugo.yaml`:

```yaml
name: Deploy Hugo site to Pages

on:
  push:
    branches: ["main"]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

defaults:
  run:
    shell: bash

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.164.0
    steps:
      - name: Install Hugo CLI
        run: |
          curl -sL "https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb" -o /tmp/hugo.deb
          sudo dpkg -i /tmp/hugo.deb
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Build with Hugo
        run: |
          hugo \
            --gc --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

En GitHub: **Settings → Pages → Source: GitHub Actions**.

## 14. Subir todo

```bash
git add .
git commit -m "Sitio inicial con tema Blowfish (submódulo)"
git branch -M main
git push -u origin main
```

En unos minutos la Action se ejecuta y el blog queda publicado en `https://TU_USUARIO.github.io/mi-blog/`.

Si el `push` falla con `repository not found`, comprueba que el repositorio existe en GitHub y que la URL del remoto usa tu usuario real:

```bash
git remote -v
git remote set-url origin https://github.com/TU_USUARIO_REAL/mi-blog.git
```

## 15. Actualizar el tema en el futuro

Como está instalado como submódulo, actualizar Blowfish es tan sencillo como:

```bash
cd themes/blowfish
git pull origin main
cd ../..
git add themes/blowfish
git commit -m "Actualizar tema Blowfish"
```

Si la nueva versión añade parámetros nuevos en `params.toml` u otros archivos de configuración, conviene revisar el registro de cambios del tema y copiar manualmente los que te falten a tu `config/_default/`.
