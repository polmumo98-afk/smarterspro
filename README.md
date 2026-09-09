# Web de descarga de APK

Página estática (sin backend) para descargar la app desde el navegador de
una Smart TV, sin contraseña por ahora. Todo el proceso de abajo se hace
**desde el navegador**, sin usar terminal en tu ordenador.

## Estructura

```
index.html   → la página (todo el diseño y lógica está aquí, un solo archivo)
vercel.json  → cabeceras para servir descargas correctamente
```

La APK **no** va dentro del repositorio normal (pesa 77 MB y GitHub limita
a 25 MB los archivos que se suben arrastrándolos por la web). En su lugar
se sube como "asset" de un **Release** de GitHub, que sí admite archivos
grandes (hasta 2 GB) subidos directamente desde el navegador.

## Paso 1 — Sube los archivos web al repositorio (por el navegador)

1. Ve a tu repositorio: `github.com/polmumo98-afk/smarterspro`
2. En la página principal verás el enlace **"uploading an existing file"**
   (o el botón "Add file" → "Upload files" si el repo ya no está vacío).
3. Arrastra `index.html` y `vercel.json` a la zona de arrastre.
4. Baja y pulsa **"Commit changes"**.

## Paso 2 — Sube la APK como Release (por el navegador, sin límite de 25 MB)

1. En tu repositorio, en la columna derecha busca **"Releases"** → clic en
   **"Create a new release"** (o entra directamente a
   `github.com/polmumo98-afk/smarterspro/releases/new`).
2. En "Choose a tag" escribe `v5.0` y dale a "Create new tag".
3. Título, por ejemplo: `IPTV Smarters Pro v5.0`.
4. Abajo, en "Attach binaries by dropping them here", arrastra tu archivo
   `iptv-smarters-pro-5-0.apk`.
5. Pulsa **"Publish release"**.
6. Una vez publicado, verás el archivo listado en la propia página del
   release. Haz clic derecho sobre su nombre → **"Copiar dirección del
   enlace"**. Debería ser algo así:
   `https://github.com/polmumo98-afk/smarterspro/releases/download/v5.0/iptv-smarters-pro-5-0.apk`

## Paso 3 — Pon esa URL en index.html (editando directamente en GitHub)

1. En el repositorio, abre `index.html`.
2. Pulsa el icono de lápiz (✏️) arriba a la derecha del archivo para
   editarlo online.
3. Busca la línea que empieza por `file:` dentro del objeto de Android y
   sustituye la URL de ejemplo por la URL real que copiaste en el paso 2
   (tiene que ser exactamente la misma, incluido el nombre del archivo).
4. Baja y dale a **"Commit changes"**.

> Ya viene puesta una URL de ejemplo con el tag `v5.0` — si usas ese mismo
> tag y nombras el archivo `app-android.apk` al subirlo al Release, no
> tendrás que cambiar nada. Si GitHub te obliga a mantener el nombre
> original (`iptv-smarters-pro-5-0.apk`), sí tendrás que actualizar la URL.

## Paso 4 — Despliega en Vercel (también por el navegador)

1. Entra en https://vercel.com/new
2. Conecta tu cuenta de GitHub si no lo has hecho, y selecciona el
   repositorio `smarterspro` (al ser privado, Vercel te pedirá darle
   permiso).
3. Framework Preset: **"Other"** (es HTML estático, no necesita build).
4. Pulsa **Deploy**.

Vercel te dará una URL tipo `https://smarterspro.vercel.app`. Esa es la que
pones en el QR para que la TV la abra.

## Actualizaciones futuras

- Para cambiar la web (`index.html`), edítala directamente en GitHub con el
  lápiz (✏️) y haz commit — Vercel vuelve a desplegar solo.
- Para subir una nueva versión de la APK, crea un nuevo Release (por
  ejemplo `v5.1`) con el archivo nuevo, y actualiza la URL en `index.html`
  como en el Paso 3.
- Para añadir la app de iOS (o más apps), añade una entrada nueva al array
  `APPS` en `index.html` con el mismo formato que la de Android (subiendo
  ese archivo también como asset de un Release, igual que la APK).

## Sobre iOS

A diferencia de Android, iOS **no permite instalar apps sueltas** solo con
descargar un archivo desde el navegador. Para que un `.ipa` se instale así
necesitas una de estas opciones:

- Una cuenta de **Apple Developer Enterprise** (firma la app y permite
  distribución fuera de la App Store).
- Distribuirla por **TestFlight** (requiere invitar usuarios y pasar por
  Apple, no es instalación directa desde una web).
- Un servicio de distribución ad-hoc (Diawi, Firebase App Distribution, etc.)
  que gestione el `manifest.plist` y el enlace `itms-services://` necesario.

Si consigues alguna de estas opciones, dímelo y adapto la web para que el
botón de iOS abra el enlace `itms-services://...` correcto en vez de una
descarga directa.

## Sobre la contraseña

Ahora mismo la página es de acceso libre (sin contraseña), tal y como
pediste. Si más adelante quieres añadir una, hay dos formas típicas:

- **Simple (cliente):** una pantalla de "introduce la contraseña" en
  JavaScript antes de mostrar los botones. Fácil de montar, pero no es
  seguridad real (cualquiera con conocimientos técnicos puede saltársela).
- **Real (servidor):** protección de despliegue de Vercel (Password
  Protection, disponible en planes de pago) o una función serverless que
  valide la contraseña antes de servir el archivo.

Dime cuál prefieres cuando llegue el momento y te lo añado.
