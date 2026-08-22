# Bitácora de Gym — Omar & Edson

Versión para GitHub Pages: un solo archivo `index.html` (front) + `data.json` (almacenamiento), en la misma carpeta del repo.

## Cómo funciona el guardado

No hay backend externo. El propio `index.html` lee y escribe `data.json` **dentro de este mismo repo** usando la API de contenidos de GitHub (`api.github.com/repos/.../contents/data.json`). Es decir, cada vez que marcas un ejercicio o escribes un peso, la app hace un commit automático a `data.json` con los datos actualizados.

## Pasos para dejarlo funcionando (configuración única, sin que cada quien haga nada)

1. **Sube ambos archivos** (`index.html` y `data.json`) a la raíz del repo.
2. **Edita las líneas de configuración** al inicio del `<script>` en `index.html`:
   ```js
   const GH_OWNER  = "omarhdezalvares";
   const GH_REPO   = "gym-tracker";
   const GH_BRANCH = "main";
   const GH_PATH   = "data.json";
   const HARDCODED_TOKEN = "";  // <- pega aquí tu GitHub Personal Access Token
   ```
3. **Crea un GitHub Personal Access Token** (fine-grained):
   - GitHub → Settings → Developer settings → Personal access tokens → Fine-grained tokens → Generate new token.
   - Repository access: **solo este repo** (`gym-tracker`).
   - Permissions → Contents: **Read and write**. Nada más.
   - Genera y copia el token.
4. **Pega el token** en `HARDCODED_TOKEN` (línea señalada arriba) y sube el archivo.
5. **Activa GitHub Pages**: Settings → Pages → Source: "Deploy from a branch" → rama `main`, carpeta `/root`.
6. Listo — cualquiera que abra el link ya puede usarlo con solo el login (`gymbro` / `gymBros$1`), sin configurar nada por su lado.

### ⚠️ Compromiso de seguridad de esta opción

Con el token embebido en `index.html`, **cualquiera que vea el código fuente de la página puede leer ese token y usarlo para escribir en tu repo** con los permisos que le diste. Se decidió así a propósito para evitar que cada persona configure algo en su navegador — mitigado por:

- Token **fine-grained**, limitado solo a este repo.
- Permiso **solo** de Contents (lectura/escritura), nada de administración ni otros repos.
- Si algo sale mal, puedes revocar el token en segundos desde GitHub → Settings → Developer settings, y generar uno nuevo.

Si más adelante prefieres quitar el token del código fuente, deja `HARDCODED_TOKEN = ""` y cada quien puede pegar el suyo con el botón 🔑 (sigue funcionando como respaldo, guardado solo en su navegador).


## ⚠️ Cosas a tener en cuenta

- **El token da acceso de escritura a tu repo.** Trátalo como una contraseña: no lo compartas fuera de este uso, y si lo pierdes o quieres revocarlo, bórralo desde GitHub → Settings → Developer settings.
- **Sin token, la app puede leer pero no guardar.** Verás un aviso claro (`⚠ No se pudo guardar...`) si falta configurarlo.
- **Concurrencia:** si Omar y Edson guardan casi al mismo tiempo, la app detecta el conflicto de versión (`sha` desactualizado) y reintenta automáticamente una vez, releyendo antes de reescribir.
- **El login es una cortina simple**, no seguridad real — cualquiera que vea el código fuente puede ver usuario/contraseña. El token de GitHub sí es lo que realmente protege la escritura de datos.
- Cada guardado genera un commit visible en el historial del repo — es normal, así es como persiste la información.

## Archivos

- `index.html` — toda la app (HTML + CSS + JS puro, sin frameworks ni build step).
- `data.json` — rutina + historial de sesiones. No lo edites a mano mientras la app esté en uso; ella lo mantiene actualizado.
- `manifest.json`, `icon-152.png`, `icon-167.png`, `icon-180.png`, `icon-192.png`, `icon-512.png` — para que se pueda instalar como app (ver abajo).

## Instalarla como app en iPhone (PWA)

1. Sube también estos archivos nuevos a la raíz del repo: `manifest.json` y los 5 `icon-*.png`.
2. Abre el link de GitHub Pages en **Safari** (tiene que ser Safari, no Chrome, para que funcione el "Agregar a inicio").
3. Toca el botón de compartir (el cuadrito con la flecha hacia arriba) → **"Agregar a pantalla de inicio"**.
4. Aparecerá con su propio ícono (una mancuerna dorada) y, al abrirla, corre a pantalla completa sin la barra de Safari — se siente como una app normal, sin pasar por la App Store ni pagar la cuenta de desarrollador de Apple.

Repite el mismo paso en el iPhone de Edson.

## Alternativa (ya no usada): Google Apps Script

Si prefirieras un backend separado en vez de que el propio repo sea la base de datos, existe la opción de Google Apps Script + Google Sheets (como en `guardias-mama`). No se usó aquí porque pediste que todo viviera en un solo archivo + un JSON en la misma carpeta.
