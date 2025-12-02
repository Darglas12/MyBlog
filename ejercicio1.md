# Guía para desplegar el tema Jekyll minima en GitHub Pages

Esta guía describe paso a paso cómo crear un sitio con Jekyll usando el tema **minima** y desplegarlo en **GitHub Pages**.

---

## 1. Requisitos previos

- Tener una cuenta en GitHub.
- Tener Ruby y Bundler instalados en tu sistema.
- (Opcional) Tener Jekyll instalado localmente para vista previa.

---

## 2. Crear un nuevo repositorio en GitHub

1. Accede a GitHub y crea un nuevo repositorio.
2. Asigna un nombre, por ejemplo: `mi-sitio-jekyll`.
3. No es necesario añadir archivos iniciales.

---

## 3. Crear la estructura de un proyecto Jekyll

En tu máquina local:

```bash
mkdir mi-sitio-jekyll
cd mi-sitio-jekyll
```

Inicializa un nuevo proyecto con Bundler:

```bash
bundle init
```

Edita el archivo `Gemfile` y añade:

```ruby
gem "jekyll"
gem "minima"
```

Instala dependencias:

```bash
bundle install
```

Crea la estructura base de Jekyll:

```bash
bundle exec jekyll new . --force
```

Esto generará carpetas como `_posts`, `_layouts`, `_includes`, y un archivo `index.md`.

---

## 4. Configurar el tema **minima**

Abre el archivo `_config.yml` y asegúrate de incluir:

```yaml
theme: minima
title: "Mi sitio con Minima"
author: "Tu nombre"
```

Si el archivo contiene `theme: minima` y `plugins:`, puedes dejarlos tal cual.

---

## 5. Probar el sitio localmente (opcional)

Ejecuta:

```bash
bundle exec jekyll serve
```

Abre en tu navegador:

```
http://localhost:4000
```

---

## 6. Subir el sitio a GitHub

1. Inicializa git:

```bash
git init
git add .
git commit -m "Primer commit"
```

2. Añade el origen remoto y sube el contenido:

```bash
git remote add origin https://github.com/tu-usuario/mi-sitio-jekyll.git
git push -u origin main
```

---

## 7. Habilitar GitHub Pages

1. En tu repositorio, entra en **Settings**.
2. Ve a **Pages**.
3. En *Build and deployment*, selecciona:
   - **Source**: *GitHub Actions* o *Deploy from a branch*.

### Opción A: Usar GitHub Actions (recomendado)

GitHub suele detectar Jekyll automáticamente. Si no lo hace:

1. Crea `.github/workflows/jekyll.yml` con:

```yaml
name: Jekyll site CI

on:
  push:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: ruby/setup-ruby@v1
        with:
          ruby-version: "3.1"
          bundler-cache: true
      - run: bundle exec jekyll build -d ./_site
      - uses: actions/upload-pages-artifact@v2
        with:
          path: "./_site"

  deploy:
    needs: build
    runs-on: ubuntu-latest
    permissions:
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v2
```

2. Guarda y sube los cambios.
3. GitHub generará tu sitio automáticamente.

### Opción B: Usar "Deploy from a branch"

1. Elige la rama `main`.
2. Selecciona la carpeta raíz `/`.
3. Guarda.

> Nota: Para esta opción tu repositorio **no** debe requerir plugins de Jekyll no permitidos por GitHub Pages.

---

## 8. Visitar tu sitio

Al finalizar la configuración, GitHub te mostrará la URL pública. Suele tener esta forma:

```
https://tu-usuario.github.io/mi-sitio-jekyll/
```

---

## 9. Actualizar el sitio

Cualquier cambio que envíes a la rama configurada se publicará automáticamente.

Ejemplo:

```bash
git add .
git commit -m "Actualización de contenido"
git push
```

---


