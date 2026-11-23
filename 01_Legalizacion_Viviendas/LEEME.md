# Legalizacion_Viviendas

Evalúa en el lugar si una vivienda en una parcela puede legalizarse en Canarias.

## Pestañas
1. **Expediente**: isla, municipio y localización de la referencia catastral por dirección, polígono y parcela, referencia catastral o coordenadas y enlace de Google Maps.
2. **Visor**: parcela sobre mapa y comparación de ortofotos históricas de GRAFCAN (2009 a 2022) para la prescripción de 4 años.
3. **Edificaciones**: existentes y a legalizar, con vía de legalización resultante.
4. **Informe**: normativa aplicable y documentación a presentar.
5. **Presupuesto**: honorarios, IGIC 7 %, tasas y horquilla, imprimible para el cliente.

## Versión actual: v5 (revisión de errores)
- Los datos de una parcela anterior ya no se mezclan al buscar otra; al cambiar de expediente se limpian los resultados y el visor.
- Edificaciones: se guarda mientras se escribe y no se cierra el teclado del móvil al pasar de campo.
- Presupuesto: el panel de tarifas no se cierra al cambiar un valor; botón para volver al valor calculado; el visado sigue los cambios del proyecto; el mínimo de tarifa se respeta.
- Mensajes de error más claros (sin conexión, Catastro lento o en mantenimiento); no se lanzan dos búsquedas a la vez.
- Coordenadas pegadas al revés o sin el signo menos se corrigen solas; ya no se busca dos veces al pegar.
- Expedientes antiguos o importados incompletos se completan al cargar; aviso si no se puede guardar; copia aparte si los datos están dañados.
- Exportar funciona mejor en Safari; al importar en el iPhone ya se pueden elegir los archivos .json.

## Pendiente
- Detección automática de afecciones y clasificación del suelo (IDECanarias).
- Importar construcciones del Catastro como edificaciones.
- Revisar y ajustar las tarifas del presupuesto.
