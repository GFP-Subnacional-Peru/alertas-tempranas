# Alertas tempranas de deforestación

Lectura técnica de las alertas de pérdida de bosque que GeoBosques publica dentro de las Áreas de Conservación Regional. El polígono lo produce el Ministerio del Ambiente. Este visor sirve para interpretarlo, contrastarlo con imagen satelital y, cuando corresponde, constatarlo en campo.

En línea: https://www.gfpsubnacional.pe/sistema-de-alertas-tempranas-de-deforestacion/

## Método

La unidad de trabajo es la alerta ya publicada: un polígono con código, fecha de imagen y territorio (región, ámbito, ACR y periodo). El sistema no genera ese polígono ni lo sustituye.

La interpretación usa Sentinel-2, nivel L2A, del catálogo Earth Search. Para cada alerta se buscan dos escenas alrededor de la fecha de detección: una anterior y una posterior, con un tope de nubosidad. Ese par orienta la lectura. El reporte oficial guarda las escenas que el especialista elige.

La clasificación la decide la persona. Puede haber una sugerencia automática de clase, causa y confianza; esa sugerencia no se graba sola y no reemplaza el criterio del especialista. El polígono de trabajo sigue siendo el de GeoBosques. Si se edita el contorno, queda registrado como edición de ese polígono.

La constatación de campo adjunta una fotografía. La coordenada se guarda en UTM. Si el archivo trae la ubicación del celular, el visor la lee y reemplaza el centro de la alerta.

La capa de Global Forest Watch es una referencia visual del año en curso, de enero a la fecha, recortada al ACR y a su zona de influencia. No entra a la bandeja de clasificación ni al PDF.

El informe reúne la ubicación, el mapa, el par de imágenes, la clase asignada y la afectación. La fotografía de campo va en el reporte de constatación.

Las claves de ingreso al visor no están en este repositorio.
