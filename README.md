# Mouse AR Dental Clinic (Unity WebGL)

Experiencia de Realidad Aumentada basada en cámara (WebAR) generada con Unity WebGL y OpenCV (`opencv.js`).

Este proyecto es un sitio estático: puede hospedarse en cualquier CDN (Netlify, GitHub Pages, Vercel static) y solo requiere HTTPS para acceso a la cámara.

## Estructura

- `index.html`: página principal con integración de Unity y cámara.
- `Build/`: artefactos de Unity WebGL (`.data`, `.framework.js`, `.wasm`).
- `TemplateData/`: recursos visuales de la plantilla de Unity.
- `arcamera.js`: control de cámara y canvas.
- `itracker.js`: tracking de imagen y bindings con OpenCV.
- `opencv.js`: librería OpenCV para navegador.
- `netlify.toml`: configuración de cabeceras y caché para producción.

## Requisitos de producción

- HTTPS (obligatorio para `getUserMedia` en móviles/desktop).
- Cabeceras de aislamiento (COOP/COEP) si el build usa `SharedArrayBuffer`/WebAssembly con hilos. Ya están definidas en `netlify.toml`.
- Tipo MIME correcto para `.wasm` (forzado por `netlify.toml`).

## Correr localmente

Opción 1 (Node):

```bash
npx serve . -p 5173
```

Opción 2 (Python 3):

```bash
python -m http.server 5173
```

Luego abre: http://localhost:5173

Nota: Algunos navegadores no habilitan cámara en `http://localhost` sin HTTPS. Netlify en producción usa HTTPS.

## Deploy en Netlify (desde GitHub)

1. Sube el repo a GitHub (ver sección siguiente).
2. En Netlify, "Add new site" → "Import an existing project" → elige tu repo.
3. Build command: (vacío) — es un sitio estático.
4. Publish directory: `.` (raíz del proyecto).
5. Despliega. Netlify aplicará `netlify.toml` para cabeceras y caché.

## Subir a GitHub (línea de comandos)

Asegúrate de estar en el directorio del proyecto.

```bash
git init
git add .
git commit -m "Initial commit: Mouse AR Dental Clinic (Unity WebGL)"
# Crea repo en GitHub manualmente o con GitHub CLI (gh)
# Opción A: Manual
# 1) Crea un repo vacío en GitHub llamado, por ejemplo, "mouse-ar-dental-clinic"
# 2) Luego:
git branch -M main
git remote add origin https://github.com/<TU_USUARIO>/mouse-ar-dental-clinic.git
git push -u origin main

# Opción B: Con GitHub CLI (si tienes "gh" instalado)
# gh repo create mouse-ar-dental-clinic --public --source . --remote origin --push
```

## Deploy directo con Netlify CLI (alternativo)

```bash
npm i -g netlify-cli
netlify init        # Selecciona tu cuenta, crea o vincula un sitio
netlify deploy --prod --dir .
```

## Notas

- El proyecto incluye `opencv.js`. `itracker.js` la carga y reporta error si no está disponible.
- Si modificas `Build/` (cambio de build Unity), recuerda mantener `netlify.toml` para `.wasm` y caché.
- Para móviles, es necesario aceptar permisos de cámara. iOS requiere Safari y HTTPS.
