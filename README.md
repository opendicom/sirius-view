# sirius-view

dckv permite acceso granular y rapido a cualquier atributo de cualquier instancia de un estudio. El paradigma es muy diferente del de los PACS tradicionales, que copian unos atributos a índice y necesitan leer el archivo DICM de cada instancia para encontrar los otros atributos necesarios a la visualización.

DICOMweb fue diseñado por el grupo de trabajo WG-18 para coexistir con PACS tradicionales. Es el protocolo generalmente usado por los visualizadores DICOM HTML5. Es muy ineficiente porque se transfieren al visualizador :

- más datos de lo necesario,
- un mismo dato varias veces,
- los datos de pixeles tales cuales, sin posibilidad de progresión de la nitidez con layers de calidad.

## alternativa studydckv

[studydckv](https://github.com/jacquesfauquex/DCKV/wiki), formato kv de almacenamiento de datos dentro de un PACS de nueva generación fue diseñado para optimizar todos estos aspectos.

La delimitación qido / wado del diseño del WG-18 desaparece. Todos los pedidos REST de studydckv piden especificamente un conjunto de atributos (incluyendo o no los pixeles). Además el formato studydckv tiene por separado 4 layers de calidad para poder mostrar la imagen como miniatura, baja resolución, alta resolución y sin pérdida. De tal forma que se puede bajar el nivel bajo y luego los niveles adicionales, sin volver a bajar los niveles anteriores.

El mismo modelo de datos dckv (un key value clasificado con key y value siendo de tipo bytechain) se usa de ambos lados servidor / cliente. Del lado servidor, la fuente de dckv es una base de datos mdbx y del lado cliente un b-tree map en memoria.

El visualizador cliente pide todos los atributos que necesite via request REST, que del lado servidor un Connector recibe y transforma en pedidos a la base de datos mdbx. Se implementan macros, que corresponden muy exactamente con la información que necesita el visualizador.

Vale notar que del lado cliente todos los atributos recibidos de todas las instancias del estudio coexisten en un único b-tree map gracias al formato de key studydckv. B-tree map es necesario, porque además de las consultas por key exacta, se usan consultas con key prefix, y key regex, que devuelven grupos de valores 

[doc wiki](https://github.com/opendicom/sirius-view/wiki)
