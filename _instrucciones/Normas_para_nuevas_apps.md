# Normas para desarrollar apps con Claude

## Formato
- Un solo archivo HTML, sin instalación, que funcione sin servidor.
- Datos guardados en el navegador (localStorage) con exportar e importar en JSON, por expediente.
- Se prueba abriendo el archivo en Safari o Chrome.

## Estilo
- Interfaz clara, usable en obra desde móvil o portátil, con pocos pasos.
- Textos en español, sin emojis.
- Documentos para clientes (presupuestos, informes): sin marcas de agua, sin autoría añadida ni anotaciones externas.

## Datos y fuentes
- Catastro (OVC), GRAFCAN (ortofotos históricas), IDECanarias para afecciones.
- Normativa canaria vigente: Ley 4/2017, Decreto 182/2018, Ley 3/2023.
- Los precios y tarifas son valores de partida editables; los reales los fija el usuario.

## Entrega
- Al terminar una versión: guardar la nueva en la carpeta de la app y mover la anterior a `versiones_anteriores/`.
- Antes de abrir una versión nueva, exportar los expedientes de la anterior.
