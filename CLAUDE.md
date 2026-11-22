# Instrucciones para Claude Code: Apps_Aparejador

## Contexto
- Usuario: aparejador en Tenerife (Canarias). Responder siempre en **español**. Datos personales y rutas locales en `CLAUDE.local.md` (solo en el Mac, no se publica).
- Esta carpeta contiene las apps propias que desarrolla con Claude para su trabajo.
- En el Mac la carpeta está en iCloud Drive (`APAREJADOR/Apps_Aparejador`); la ruta tiene espacios: entrecomillarla siempre.
- Lee también `LEEME.md` (índice) y `_instrucciones/Normas_para_nuevas_apps.md` (normas comunes).

## Estructura
- `NN_Nombre_App/` una carpeta por app, numerada por orden de creación.
  - `Nombre_App.html` versión vigente, un solo archivo autónomo.
  - `LEEME.md` qué hace, cómo se usa y qué falta.
  - `versiones_anteriores/` copias antiguas con sufijo `_vN_motivo`.
- Antes de cambiar una app, copiar la versión vigente a `versiones_anteriores/` (siguiente número). Después editar la vigente.
- Al crear una app nueva: carpeta `NN_Nombre/`, su HTML, su `LEEME.md`, y añadirla a la tabla de `LEEME.md` de la raíz.

## Estilo de trabajo del usuario
- Explicaciones claras, estructuradas y cortas; **negrita** en lo clave; empezar por la solución más sencilla; pasos cortos y por fases.
- Sin emojis. En documentos profesionales (presupuestos, informes): sin marcas de agua, sin autoría añadida y sin anotaciones externas.
- Si algo no está claro, **preguntar** antes de hacerlo.
- Interfaz usable en obra: pegar datos "de golpe" en un solo campo, nada de campos separados cuando el dato se copia entero (ejemplo: coordenadas de Google Maps).

## App 01: Legalizacion_Viviendas
Evalúa en el lugar si una vivienda en una parcela puede legalizarse en Canarias.
- Archivo: `01_Legalizacion_Viviendas/Legalizacion_Viviendas.html`. La versión se muestra en pantalla junto al título ("v3"); subir ese número en cada versión.
- Un solo HTML con **Leaflet 1.9.4 incrustado** (CSS y JS dentro del propio archivo). Editar el HTML directamente y no tocar el bloque de Leaflet.
- Datos en `localStorage` (clave `legalizacion_expedientes_v1`), con exportar e importar JSON por expediente. No cambiar la estructura de datos sin migración, para no perder expedientes.
- Pestañas: 1 Expediente, 2 Visor, 3 Edificaciones, 4 Informe, 5 Presupuesto.
- Localizar: por dirección, por polígono y parcela, por referencia catastral, y por **coordenadas** (campo único que acepta `28°24'18.9"N 16°31'15.5"W`, `28.4052, -16.5216` o un enlace de Google Maps).

### Servicios externos (todos con CORS abierto)
- Catastro JSON: `https://ovc.catastro.meh.es/OVCServWeb/OVCWcfCallejero/COVCCallejero.svc/json/` (ObtenerMunicipios, ObtenerCallejero, Consulta_DNPLOC, Consulta_DNPPP, Consulta_DNPRC). Provincias: "S.C. TENERIFE" y "LAS PALMAS". Los nombres de calle vienen truncados a 25 caracteres.
- Coordenadas a referencia: `https://ovc.catastro.meh.es/ovcservweb/OVCSWLocalizacionRC/OVCCoordenadas.asmx/Consulta_RCCOOR`. **La ruta distingue mayúsculas y minúsculas**; en otra forma devuelve 400.
- Parcela (WFS INSPIRE): `https://ovc.catastro.meh.es/INSPIRE/wfsCP.aspx?service=wfs&version=2&request=getfeature&STOREDQUERY_ID=GetParcel&srsname=EPSG::4326&refcat=` (las coordenadas llegan como latitud y longitud).
- GRAFCAN ortofotos: actual `https://idecan1.grafcan.es/ServicioWMS/OrtoExpress` capa `ortoexpress`; históricas `.../Historico/Ortofotos/OrtoExpress_<año>`; años 2009, 2012, 2015 (capas `ortoexpress_01,ortoexpress_02`), 2017, 2019, 2022.
- Las peticiones de red usan `fetch(url)` **sin opciones**, con respaldo `XMLHttpRequest` y tiempo límite propio con `Promise.race`. No usar `AbortController`: el panel de vista previa de Claude no lo admite.

### Lógica legal
- Ley 4/2017 (suelo y espacios naturales protegidos de Canarias), art. 361: plazo de **4 años** desde la completa terminación; no prescribe en suelo rústico de protección ambiental, dominio público ni espacios naturales protegidos (art. 361.5). La prescripción no legaliza: la situación (fuera de ordenación o asimilado) depende del planeamiento.
- Vías que calcula la app: A (antigüedad), B (legalización con licencia), C (suelo o afección protegida), E (con título). Otras normas: Decreto 182/2018, Ley 3/2023, RDL 7/2015 art. 28.

### Presupuesto
- Las tarifas por defecto son **valores de partida inventados**; las reales las fija el usuario en "Tarifas y criterios". IGIC 7 %, horquilla del 15 %.

### Pendiente
- Detección automática de afecciones y clasificación del suelo desde IDECanarias.
- Importar construcciones del Catastro como edificaciones.
- Botones de Google Maps y Street View en la parcela localizada.

## Cómo probar
- El panel de vista previa de Claude **bloquea las conexiones externas**; no sirve para probar el Catastro. Probar en **Chrome** (o Playwright con Chrome) abriendo el HTML y usando "Probar conexión con el Catastro". El usuario comprueba en Safari.
- Parcela de prueba: las coordenadas `28°24'18.9"N 16°31'15.5"W` dan la parcela `1229113CS5412N`; la dirección Prolongación Ramón y Cajal 2, Santa Cruz de Tenerife, da `7593402CS7479S`.
- Tras cada cambio: comprobar que no hay errores en la consola y que un expediente guardado se sigue cargando.
- iCloud puede tardar en sincronizar: confirmar al usuario la versión mostrada en pantalla.

## Publicación y trabajo desde varios dispositivos
- Repositorio público: `https://github.com/juancarlosbimdivpass-pixel/Apps_Aparejador` (rama `main`). Web: `https://juancarlosbimdivpass-pixel.github.io/Apps_Aparejador/` (portada `index.html` con enlace a cada app). La web se actualiza 1-2 minutos después de cada `push` a `main`.
- **GitHub es la copia de referencia.** El usuario trabaja desde el iPhone (sesiones de Claude Code en la nube sobre este repositorio), desde el MacBook y quizá desde el iMac.
- **Al empezar** cualquier sesión: `git pull` para traer lo último. **Al terminar** cada cambio: commit y `git push` a `main` (el usuario es el único autor; no hace falta rama ni pull request). Si la sesión en la nube no puede subir a `main`, abrir un pull request y decir al usuario que lo fusione desde la app de GitHub.
- Sin datos personales en nada que se suba: `.gitignore` excluye `CLAUDE.local.md` y `.DS_Store`. No escribir el nombre del usuario en archivos públicos.
- `versiones_anteriores/` también está en el repositorio; la regla de copiar la versión vigente antes de cambiarla se mantiene.
- Al crear una app nueva, añadir su enlace en `index.html`.
- En una sesión en la nube no hay navegador para probar el Catastro: revisar el código con cuidado, comprobar que el JavaScript no tiene errores de sintaxis (por ejemplo extrayendo el `<script>` y pasándolo por `node --check`) y pedir al usuario que pruebe en Safari del iPhone tras el push.
