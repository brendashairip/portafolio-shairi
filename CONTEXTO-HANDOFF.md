# Contexto del proyecto — Portafolio de Shairi Pineda

> **Documento de traspaso (handoff)** para que otro agente continúe el trabajo con contexto completo.
> Última actualización: 2026-09-09.

---

## 1. Quién es la usuaria y cómo comunicarse

- **Shairi Pineda** — Diseñadora **UX/UI & Product Designer** (México). Email: `17670115@itiguala.edu.mx`.
- Es **diseñadora, no desarrolladora**. Quiere aprender a construir y **publicar** aplicaciones.
- **Comunícate SIEMPRE en español**, de forma amable, explicando los pasos con calma.
- **No asumas comodidad con la terminal.** Cuando un paso requiera terminal, da el comando exacto + explicación en lenguaje sencillo de qué hace y por qué.
- Ella trabaja por iteraciones: manda capturas de pantalla (como instrucciones de cambio) e imágenes nuevas (para reemplazar placeholders), de a una o pocas a la vez.

---

## 2. Qué es el proyecto

Portafolio personal de Shairi, **originalmente creado en Claude Design** (formato `.dc.html`, projectId `1780fbe7-e860-4f2b-8f6c-a12d4f50e74d`). Se **convirtió a un sitio estático** (HTML/CSS/JS puro, sin dependencias ni runtime), con 5 vistas y un hash-router:

- `#inicio` — Inicio
- `#case` — Case study (proyecto de Remesas "Kobo")
- `#sobre` — Sobre mí
- `#contacto` — Contacto
- `#sistema` — Sistema (design system)

El case study principal es un proyecto de **remesas EE.UU. → México** (app ficticia "Kobo").

---

## 3. Carpeta y forma de trabajo

- **Carpeta de trabajo (repo git real):** `/Users/brendashairipinedaflores/Desktop/portafolio-shairi/`
  - `index.html` — el sitio completo (todo el HTML/CSS/JS inline).
  - `img/` — todas las imágenes (~36+ archivos).
  - `README.md`, `.gitignore` (ignora `.DS_Store`), y este `CONTEXTO-HANDOFF.md`.
- **Se edita DIRECTAMENTE aquí** `index.html` + `img/`. (Había una copia vieja en un scratchpad, ya superada — ignórala.)

### Cómo se editan las imágenes en el HTML
Las imágenes NO son `<img>`, son **fondos CSS** en divs, con comillas codificadas:
```html
<div style="aspect-ratio: 9/16; background: url(&quot;img/archivo.png&quot;) center / contain no-repeat; width:100%;max-width:200px"></div>
```
- Ojo: las comillas van como `&quot;` (por eso un grep de `url(img/` falla; busca `url(&quot;img/`).
- Para **mockups con marco de teléfono** (fondo transparente) usar `center / contain` (muestra el teléfono completo, sin recortar). Para fotos que deben llenar la caja, `center / cover`.

### Cómo obtener las imágenes que Shairi pega en el chat (a calidad completa)
Las imágenes pegadas/adjuntas se extraen del **transcript JSONL de la sesión**, NO del sistema de archivos (los adjuntos se pierden o iCloud bloquea el acceso). Patrón:
1. Transcript: `/Users/brendashairipinedaflores/.claude/projects/-Users-brendashairipinedaflores-Desktop-portafolio-shairi/9a638f92-578a-4888-aa61-66750e7fde36.jsonl`
2. Recorrer el JSONL buscando bloques `{"type":"image","source":{"type":"base64",...}}`.
3. Tomar la **última línea** que contiene imágenes (el mensaje más reciente), decodificar base64, guardar, e identificar por dimensiones (la 1ª suele ser la captura de referencia ancha; las demás son los mockups verticales ~790x1584 o 997x2000).
4. Convertir webp→png con `sips -s format png in.webp --out out.png` si hace falta.
5. Copiar a `img/` con nombres claros (ej. `cs-mockup-1.png`, `sol-01-home.png`).

### Verificación (el panel del navegador da capturas en blanco)
Los screenshots del Browser pane salen en blanco de forma persistente. **Verificar de forma determinista**: servir con `python3 -m http.server 8765` en la carpeta y comprobar con `curl`/`fetch` que las imágenes dan **200** y sus dimensiones naturales, y revisar el HTML con grep/python. Si se abre en el navegador, **recargar sin caché** (`?v=N` o Cmd+Shift+R) porque cachea la versión vieja.

---

## 4. Publicación / Deploy (YA ESTÁ FUNCIONANDO ⚡)

- **GitHub:** repo público `https://github.com/brendashairip/portafolio-shairi` (rama `main`). Remote `origin` ya configurado.
- **Vercel:** conectado al repo, con **deploy automático en cada push**. URL en vivo: **https://portafolio-shairi.vercel.app**
- **Autenticación GitHub:** Shairi usa un **Personal Access Token (classic, scope `repo`)**. ⚠️ **NUNCA manejes su token ni credenciales** — ella lo pega SOLO en su propia terminal; el agente jamás lo ve ni hace push por ella.

### Flujo de cada cambio (el circuito ya está cerrado)
1. El agente edita `index.html` / `img/` en la carpeta.
2. El agente deja el cambio listo con `git add -A && git commit` (mensaje en español).
3. **Shairi corre `git push`** (usuario `brendashairip` + token como password).
4. Vercel republica solo en ~30 s.

> Atribución de commits (obligatoria): terminar el mensaje con
> `Co-Authored-By: Claude Opus 4.8 <noreply@anthropic.com>`

### Nota de entorno (macOS TCC)
Su terminal es **Warp**. Al inicio dio "Operation not permitted" leyendo `~/Desktop` (permiso TCC de macOS). Solución: darle a Warp **Acceso total al disco** (Ajustes → Privacidad y seguridad → Acceso total al disco → activar Warp → reiniciar Warp), o mover la carpeta al home. Si `cd`/`ls`/`git` fallan por permisos, es esto.

---

## 5. Qué se ha hecho (historial de cambios)

Commits en `main` (del más reciente al primero):
- `be85099` — Iguala el tamaño de los 4 mockups de la fila "solución" (todos `max-width:200px`, misma estructura).
- `a365c93` — Elimina el mockup "02 · Método" y renumera la fila a 01–04 sin saltos.
- `92c165e` — Reemplaza los 4 mockups de la fila "La solución en 30 segundos" (01·Home, 02·Destinatario, 03·Pago, 04·Tracking) en alta resolución.
- `baf30e5` — Reemplaza los 4 mockups de la **primera fila** del case study por versiones con marco de teléfono en alta resolución (`cs-mockup-1..4.png`).
- `494102c` — Primer commit: portafolio Shairi Pineda.

Otros cambios ya aplicados antes del repo (histórico):
- Conversión completa `.dc` → sitio estático fiel.
- **Se quitaron los bordes** (`border:1px solid`) de TODAS las cajas de imagen del proyecto (se conservó `border-radius`).
- Se colocaron los 3 mockups del **Inicio**: destacado=`mockup-page-mttzffej-o9qz.png`, SANAVI=`mockup-page-3-mttzqgi3-tyzw.png`, Logística=`mockup-page-6-mtu0fk4q-rd2y.png`.
- Se eliminaron los textos "Case study en preparación".
- Hero del Inicio simplificado a **un solo botón principal** "Ver todos los proyectos" (se quitó "Ver proyecto destacado").

---

## 6. Qué está pendiente (TODO)

1. **6 imágenes placeholder** en la parte de **research/sistema** del case study, aún como marcadores "en preparación" (archivos de **88 bytes, 24×24 px** = vacíos). Cuando Shairi las mande, extraer del JSONL y colocar con estos nombres exactos:
   - `blue-modern-ui-window-with-question-and--mttyf9hf-taho.png`
   - `captura-de-pantalla-2026-09-09-a-la-s-2--mttu255m-dxl2.png`
   - `captura-de-pantalla-2026-09-09-a-la-s-3--mttx2j4x-w5vg.png`
   - `captura-de-pantalla-2026-09-09-a-la-s-3--mttx5wdf-5ahx.png`
   - `captura-de-pantalla-2026-09-09-a-la-s-3--mttxehpl-depj.png`
   - `captura-de-pantalla-2026-09-09-a-la-s-4--mttxh8yl-usgy.png`
2. **Calidad de imágenes**: las capturas de celular viejas están a **390 px de ancho (1x)** → se ven borrosas en pantallas retina. La solución real es que Shairi **re-exporte a 2x/3x** desde Figma/Claude Design (mismo nombre de archivo). No hay que "inventar" nitidez con upscalers.
3. **Botón "Descargar CV"**: por ahora enruta a Contacto. Conectar un PDF real cuando ella lo tenga.
4. Posibles mejoras opcionales que ella podría querer: URL más bonita en Vercel (Settings → Domains), dominio propio.

---

## 7. Reglas de seguridad vigentes

- **Nunca** manejar el token/credenciales de GitHub de Shairi ni hacer push por ella ni crear cuentas a su nombre. Ella hace el `git push` en su terminal.
- El agente edita + commitea; **Shairi publica** con `git push`.

---

## 8. Estado actual exacto

- Case study, **primera fila**: 4 mockups con marco de teléfono (`cs-mockup-1..4.png`), encuadre `contain`, sobre fondo morado `#231544`.
- Case study, **fila "La solución en 30 segundos"**: 4 mockups (`sol-01-home.png`, `sol-03-destinatario.png`, `sol-04-pago.png`, `sol-05-tracking.png`), todos `max-width:200px`, numerados 01·Home / 02·Destinatario y resumen / 03·Pago y verificación / 04·Tracking. (Nota: los nombres de archivo conservan 01/03/04/05 pero los **pies de foto** dicen 01/02/03/04.)
- Todo commiteado en `main`. **Falta que Shairi haga `git push`** del último lote para verlo en Vercel.
