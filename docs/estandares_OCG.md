# Estándares OCG
    
## Qué es OGC?

El OGC (Open Geospatial Consortium fué creada en 1994 y está formada por más de 500 empresas, agencias gubernamentales y universidades. Es una organización internacional sin ánimo de lucro comprometida con la elaboración de estándares abiertos e interoperables de calidad para la comunidad geoespacial global. 

Estos estándares se elaboran a través de un proceso de consenso y están disponibles de forma gratuita para que cualquier persona pueda utilizarlos para mejorar el intercambio de los datos geoespaciales del mundo.

Estas especificaciones tienen como objetivo, fomentar la interoperabilidad geoespacial. El éxito de estas especificaciones radica en el hecho de que son impulsadas y creadas por las mismas empresas, que después las implementan en sus productos comerciales. 

### Interoperabilidad

Se puede definir la interoperabilidad como la habilidad de dos o más sistemas para intercambiar información y poderla utilizar, sin ningún esfuerzo por parte del usuario. Las modalidades de interoperatividad dependen de los agentes que interaccionan. Dicho de otra manera, los datos producidos en un entorno operativo tienen que poder ser leídos e interpretados por otro entorno sin que el usuario tenga que hacer una conversión de formatos.

La interoperabilidad permite:

* Acceder a más y mejor información (topográficos, ortoimágenes, callejeros, catastro, medio ambiente...), sin tener que disponer de un software específico o directamente desde el navegador. Esto permite que el usuario pueda acceder a cartografía de varias fuentes, con arquitecturas diferentes, de una forma fácil y transparente.

* Facilita el acceso a geoservicios.

## Estándares OGC

Un estándar es un modelo, un patrón que tiene como finalidad la fijación de unas normas comunes para garantizar la homogeneidad en el proceso de producción. Pero para que tenga éxito tiene que tener un uso mayoritario. 

![estandares](img/standards.png)
*Estándares*

Para conseguir que, tanto la documentación de los datos como la creación de servicios sea comprensible y accesible para todo el mundo, es decir, para hacer que todos “hablemos el mismo idioma”, se han diseñado estándares de metadatos y de servicios que pretenden poner en común los puntos de vista de los diferentes productores. Por lo tanto, la estandarización consiste en hacer que, tanto la documentación de datos como la creación de geoservicios, así como su acceso, se hagan conforme a unas norma previamente establecidas. 

Los estándares de la OGC son documentos de carácter técnico dónde se describen las interfaces de comunicación entre servidores y la forma de implementarlos.

En estas especificaciones no se menciona ni la arquitectura, plataforma o lenguajes de programación a utilizar. 

Los documentos, antes de ser consideradas como una OpenGIS Implementation Specification, son elaborados y probados por diferentes grupos de trabajo dentro de OGC y finalmente sometidas a votación.

Los servicios de cartografía online o web de OGC (OWS) son estándares de OGC creados para su uso en aplicaciones de la World Wide Web (WWW). Estos permiten formular petiones al servidor para: enviar preguntas, obtener datos vectoriales o ráster, hacer geoprocesos, etc.   

Todos los estándares creados por OGC se pueden consultar en [https://www.ogc.org/docs/is](https://www.ogc.org/docs/is)

![Evolución de servicios OWS](img/ows.png)
*Evolución de servicios OWS*

### WMS

La intención de WMS (Web Map Service) es la de permitir la superposición visual de información geográfica compleja y distribuida en diferentes servidores. Un cliente puede hacer peticiones a otros servidores también basados en esta especificación para descubrir información geográfica deseada. Una vez encontrada el cliente puede recurrir a ella de forma simultánea y puede visualizar diferentes datos geográficos de diferentes servidores en un mismo entorno.

Cada petición está compuesta por unos parámetros concretos definidos por la especificación WMS y que es entendida por todos los servidores de mapas que cumplen con la especificación.

Por lo tanto, cuando se dice que un Servidor de Mapas es estándar y cumple con WMS, significa que es capaz de dar respuesta a estas peticiones.

El estándar WMS proporciona una interfaz HTTP simple para solicitar imágenes de mapas georegistrados desde una o más bases de datos geoespaciales distribuidas. 

Se puede ver la especificación en [https://www.ogc.org/standards/wms](https://www.ogc.org/standards/wms)

#### Tipos de peticiones WMS

#### GetCapabilities

Nos permite descubrir cuales son las capacidades del servidor. Como respuesta va a obtener un archivo en formato xml dónde podremos saber cuales son las características del servicio, las versiones de WMS soportadas por el servidor, las operaciones que soporta, cual es su sistema de referencia, sus coordenadas, que formato de imagen soporta y metadatos de las capas de información que contiene.

##### Parámetros del GetCapabilities

| Parámetro | Obligatoriedad | Descripción                                                            |
|-----------|----------------|------------------------------------------------------------------------|
| VERSION   | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.1, 1.3.0)          |
| SERVICE   | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WMS**)                  |
| REQUEST   | Obligatorio    | Nombre de la operación (**GetCapabilities**)                               |
| LANGUAGE  | Opcional       | Se obtiene el fichero de salida en el idioma solicidado                |
| FORMAT    | Opcional       | Formato de salida del metadato del servicio.   (Por defecto text/xml)  |

Ejemplos: 

http://www.ign.es/wms-inspire/ign-base?VERSION=1.3.0&REQUEST=GetCapabilities&SERVICE=WMS

http://geoserveis.icc.cat/icc_bt5m/wms/service?REQUEST=GetCapabilities&SERVICE=WMS

##### Aspectos prácticos

###### Tamaño máximo de la imágen

Si se pide una imagen mayor que el tamaño máximo permitodo retornará un error

``` xml
<Service>
    <Name>icc_bt5m</Name>
    <Title>
    ICC - Base topogràfica de Catalunya 1:5 000 (BT-5M) - Capes WMS 96dpi (píxel 0,26458333 mm)
    </Title>
    ...
    <MaxWidth>2048</MaxWidth>
    <MaxHeight>2048</MaxHeight>
</Service>
```

###### OnlineResource

En algunos software de escritorio utiliza esta url para hacer las peticiones de las operaciones GetMap.

``` xml
<GetMap>
    <Format>image/jp2;subtype="gmljp2"</Format>
    <Format>image/gif</Format>
    <Format>image/png</Format>
    <Format>image/bmp</Format>
    <Format>image/jpeg</Format>
    <Format>image/tiff</Format>
    <DCPType>
        <HTTP>
            <Get>
                <OnlineResource xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="http://shagrat.icc.cat/lizardtech/iserv/ows" xlink:type="simple"/>
            </Get>
        </HTTP>
    </DCPType>
</GetMap>
```

###### Layer

El atributo queryable indica si la capa es consultable (1 =  consultable, 0 = no consultable)

Name es indentificador de la capa. Es el valor que se debe usar en el parámetro LAYERS de las peticiones GetMap

Title es nombre descriptivo de la capa

LegendURL hace referencia a una url de una imagen externa que contiene la leyenda de la capa

Min y Max(ScaleDenominator) factor de escala. Limita la visualización de la capa a estas escalas. Si se pide una capa fuera de esa escala retorna en blanco.

``` xml
<Layer queryable="1">
    <Name>02_ALTI_PA</Name>
    <Title>[BT5M] (02) (x) ALTIMETRIA: talussos, marges (àrees)</Title>
    <Abstract>02_ALTI_PA</Abstract>
    <CRS>EPSG:25831</CRS>
    <CRS>EPSG:4326</CRS>
    <BoundingBox CRS="EPSG:25831" minx="254904.96" miny="4484796.89" maxx="530907.30" maxy="4749795.10"/>
    ...
    <Style>
    ...
    <LegendURL width="328" height="64">
        <Format>image/png</Format>
        <OnlineResource xmlns:xlink="http://www.w3.org/1999/xlink" xlink:href="http://geoserveis.icc.cat/icc_bt5m/wms/service?request=GetLegendGraphic%26version=1.3.0%26format=image/png%26layer=02_ALTI_PA" xlink:type="simple"/>
    </LegendURL>
    </Style>
    <MinScaleDenominator>472.470238</MinScaleDenominator>
    <MaxScaleDenominator>7087.053571</MaxScaleDenominator>
</Layer>
```

###### GetFeatureInfo

Formatos de salida de la consulta

``` xml
<GetFeatureInfo>
    <Format>application/vnd.esri.wms_raw_xml</Format>
    <Format>application/vnd.esri.wms_featureinfo_xml</Format>
    <Format>application/vnd.ogc.wms_xml</Format>
    <Format>text/xml</Format>
    <Format>text/html</Format>
    <Format>text/plain</Format>
    <DCPType>
        <HTTP>
            <Get>
                <OnlineResource xmlns:xlink="http://www.w3.org/1999/xlink" xlink:type="simple" xlink:href="http://geoserveis.icc.cat/icc_bt5m/wms/service?"/>
            </Get>
        </HTTP>
    </DCPType>
</GetFeatureInfo>
```

#### GetMap

Petición GetMap devolverá un mapa en formato imagen, ya sea un PNG, JPEG, GIF, etc.

##### Parámetros del GetMap

| Parámetro   | Obligatoriedad | Descripción                                                                                                    |
|-------------|----------------|----------------------------------------------------------------------------------------------------------------|
| VERSION     | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.1, 1.3.0)                                                  |
| SERVICE     | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WMS**)                                                      |
| REQUEST     | Obligatorio    | Nombre de la operación (**GetMap**)                                                                            |
| LAYERS      | Obligatorio    | Lista de nombres de las capas separadas por coma                                                               |
| FORMAT      | Obligatorio    | Formato de salida de la imagen.  (image/png, image/jpeg, ...)                                                  |
| STYLES      | Obligatorio    | Lista de estilos separados por coma. Si no hay estilo se puede dejar en blanco                                 |
| SRS o CRS   | Obligatorio    | Código ESPG del sistema de referencia                                                                          |
| BBOX        | Obligatorio    | Caja de coordenadas del mapa (minx,miny,maxx,maxy)                                                             |
| WIDTH       | Obligatorio    | Número píxeles del ancho de la imágen                                                                          |
| HEIGHT      | Obligatorio    | Número píxeles del alto de la imágen                                                                           |
| TRANSPARENT | Opcional       | Indica si el fondo del mapa debe ser transparente (true ,false)                                                |
| BGCOLOR     | Opcional       | Color de fondo para la imagen del mapa. El valor está en la formato RRGGBB hexadecimal                         |
| SLD         | Opcional       | Una URL que hace referencia a un archivo XML StyledLayerDescriptor que controla el estilo de las capas de mapa |
| EXCEPTIONS  | Opcional       | Formato excepciones                                                                                            |

Ejemplo: http://www.ign.es/wms-inspire/ign-base?SERVICE=WMS&REQUEST=GetMap&VERSION=1.3.0&LAYERS=IGNBaseTodo&STYLES=&FORMAT=image/png&BGCOLOR=0xFFFFFF&TRANSPARENT=TRUE&SRS=EPSG:4258&BBOX=26.4764705882353,-19,44.5235294117647,5&WIDTH=1020&HEIGHT=767

#### GetFeatureInfo

Petición GetFeatureInfo sirve para mostrar los atributos de los objetos del mapa, vuelve la información en formato de tabla o XML. Si una capa está marcada como “consultable” (queryable), se puede solicitar datos sobre una coordenada de la imagen del mapa.

##### Parámetros del GetFeatureInfo

| Parámetro     | Obligatoriedad | Descripción                                                                    |
|---------------|----------------|--------------------------------------------------------------------------------|
| VERSION       | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.1, 1.3.0)                  |
| SERVICE       | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WMS**)                      |
| REQUEST       | Obligatorio    | Nombre de la operación (**GetFeatureInfo**)                                    |
| LAYERS        | Obligatorio    | Lista de nombres de las capas separadas por coma                               |
| FORMAT        | Obligatorio    | Formato de salida de la imagen.  (image/png, image/jpeg, ...)                  |
| STYLES        | Obligatorio    | Lista de estilos separados por coma. Si no hay estilo se puede dejar en blanco |
| SRS o CRS     | Obligatorio    | Código ESPG del sistema de referencia                                          |
| BBOX          | Obligatorio    | Caja de coordenadas del mapa (minx,miny,maxx,maxy)                             |
| WIDTH         | Obligatorio    | Número píxeles del ancho de la imágen                                          |
| HEIGHT        | Obligatorio    | Número píxeles del alto de la imágen                                           |
| QUERY_LAYERS  | Obligatorio    | Lista de nombres de las capas que se quieren consultar separadas por coma      |
| X o I            | Obligatorio    | Valor del píxel a consultar                                                    |
| Y o J           | Obligatorio    | Valor del píxel a consultar                                                    |
| INFO_FORMAT   | Opcional       | Formato de la respuesta (por defecto text/xml)                                 |
| FEATURE_COUNT | Opcional       | Número máximo de elementos a devolver                                          |
| EXCEPTIONS    | Opcional       | Formato excepciones                                                            |

Ejemplo:

http://geoserveis.icc.cat/icgc_bm5m/wms/service?REQUEST=GetFeatureInfo&SERVICE=WMS&VERSION=1.1.1&LAYERS=10_MUNICIPI_PC&QUERY_LAYERS=10_MUNICIPI_PC&INFO_FORMAT=text/html&STYLES=&SRS=EPSG:25831&BBOX=257904,4484796,680304,4907196&WIDTH=768&HEIGHT=768&X=295&Y=580

#### GetLegendGraphic

Petición que devuelve una imagen de la imagen de la leyenda del mapa de una capa, proporcionando una guía visual de los elementos del mapa.

| Parámetro | Obligatoriedad | Descripción                                                    |
|-----------|----------------|----------------------------------------------------------------|
| VERSION   | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.1, 1.3.0)  |
| SERVICE   | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WMS**)      |
| REQUEST   | Obligatorio    | Nombre de la operación (**GetLegendGraphic**)                  |
| LAYER     | Obligatorio    | Nombre de la capa                                              |
| FORMAT    | Obligatorio    | Formato de salida de la imagen.  (image/png, image/jpeg, ...)  |
| WIDTH     | Opcional       | Número píxeles del ancho de la imágen                          |
| HEIGHT    | Opcional       | Número píxeles del alto de la imágen                           |

Ejemplo:

http://wms.guifi.net/cgi-bin/mapserv?map=/home/guifi/maps.guifi.net/guifimaps/GMap.map&version=1.3.0&service=WMS&request=GetLegendGraphic&sld_version=1.1.0&layer=Nodes&format=image/png&STYLE=default

#### Aspector prácticos WMS

##### Principales diferencias entre las versiones 1.1.1 y 1.3.0 
* En la operación GetMap, el parámetro SRS se llama CRS en 1.3.0
* En la operación GetFeatureInfo, los parámetros X e Y se llaman I y J en 1.3.0.
* En 1.1.1, los sistemas de coordenadas geográficas especificados con el espacio de nombres EPSG se definen para tener un orden de ejes de longitud / latitud. En 1.3.0 el orden es la latitud / longitud.

Por ejemplo, considere la solicitud WMS 1.1 utilizando el SRS WGS84 (EPSG: 4326):
    
    server/wms?VERSION=1.1.1&REQUEST=GetMap&SRS=epsg:4326&BBOX=-180,-90,180,90&...
La solicitud equivalente WMS 1.3.0 es:
    
    server/wms?VERSION=1.3.0&REQUEST=GetMap&CRS=epsg:4326&BBOX=-90,-180,90,180&...

##### Problemas comunes
* Tamaño de la imagen (pantallas grandes y/o de mucha resolución)
* Capas no visibles por el control de escala
* Capas no consultables
* Formato de salida del GetFeatureInfo
* No están pensados para peticiones teseladas (velocidad) 
* No tienen caché. Las imágenes se generan al vuelo 
* Lista restringida de SRS soportados
* En software de escritorio el onlineResource (QGis tiene la opción de ignorar el onlineResource )
* Modificar el estilo (SLD poco soportado)
* SLD difícil de entender y hacer  

## WMTS

Un WMTS es un servicio que permite almacenar los datos recientemente leídos, por tanto agilizar la carga de los mismos en caso de que estos vuelvan a ser solicitados (caché). Este servicio usa un modelo de teselas (Tiling Model) parametrizado de tal manera que un cliente puede hacer peticiones de un conjunto discreto de valores y recibir rápidamente del servidor fragmentos de imágenes prerenderizadas (Tiles), que generalmente ya no requieren de ninguna manipulación posterior para ser mostrados en pantalla.

Cada una de las capas (layers) de un servidor WMTS sigue una o diversas estructuras piramidales de escalas (Tile Matrix sets o conjunto de Matrices de Teselas), en la que cada escala o nivel de la pirámide (Tile Matrix o Matriz de Teselas), es una rásterización y fragmentación regular de los datos geográficos a una escala o tamaño de píxel concreto. Por ello, una capa puede estar disponible en varios sistemas de coordenadas, y tener diferente ámbito en función de éstos.

El WMTS de OGC proporciona un enfoque complementario al WMS; a diferencia del WMS que fue concebido para poder compartir por renderizado mapas personalizados y se adoptó como una solución ideal para mostrar datos dinámicos, el WMTS renuncia a la personalización de estos mapas para obtener una mayor escalabilidad, sirviendo datos prerenderizados donde la envolvente y las escalas han sido restringidas a un conjunto discreto de teselas que siguen una geometría de malla regular.

Se puede ver la especificación en [https://www.ogc.org/standards/wmts](https://www.ogc.org/standards/wmts)

Una especificación anterior para esto es el Tile Map Service (TMS). Es más simple que WMTS. Fue desarrollado por miembros de OSGeo y no está respaldada por un organismo oficial de estándares.

También existe la especificación ZXY o "slippy map" que es igual que TMS pero la Y empieza por arriba a la izquierda. Esta especificación tampoco está respaldada por un organismo oficial de estándares.

![alt text](img/tile_pyramid_1.png "Pirámides")
![alt text](img/zoom.png "Zoom")

Para cargar la imágenes se ulitza una llamada HTTP rest dónde se especifica;
 
> https://.../.../z/x/y.format

> Z= Nivel de zoom

> X=coordenada X

> Y=coordenada Y

> Formato 

>   Raster: Imágen png o JPEG

![alt text](img/request.png "XyZ")

http://a.tile.openstreetmap.org/3/2/4.png

### Protocolos

* TMS (Tile Map Service): X Y coordenadas empiezan de debajo  a la izquierda (típico eje cartesiano de coordenadas)

* WMTS (Web Map Tile Service): OGC estandard , corrdenadas empiezan de arriba  a la izquierda.

* ZXY o "slippy map": Igual que TMS pero la Y empieza por arriba a la izquierda

![alt text](img/tms.png "XyZ")
![alt text](img/xyz.png "XyZ")

## WFS

El WFS (Web Feature Service) es una especificación que sirve para lanzar consultas sobre objetos geográficos.

Los WFS implementan también la especificación OGC FILTER encoding que permite dotar a WFS de un gran potencial ya que le permite realizar tanto consultas alfanuméricas y espaciales.

WFS básico permite hacer consultas y recuperación de elementos geográficos. Por el contrario WFS-T (Web Feature Service Transactional) permite además la creación, eliminación y actualización de estos elementos geográficos del mapa. 

Para realizar estas operaciones se utiliza el lenguaje GML (Geography Markup Language) que deriva del XML, que es el estándar a través del que se transmiten las órdenes WFS. el GML También es el formato de retorno de las consultas.

| Operadores espaciales | Operadores lógicos |
|-----------------------|--------------------|
| BBOX                  | LessThan           |
| Intersects            | LessThanEqualTo    |
| Within                | GreaterThanEqualTo |
| Beyond                | NotEqualTo         |
| Equals                | Like               |
| Disjoint              | GreaterThan        |
| Touches               | EqualTo            |
| Crosses               | Between            |
| Contains              |                    |
| Overlaps              |                    |

Se puede ver la especificación en [https://www.ogc.org/standards/wfs](https://www.ogc.org/standards/wfs)


#### Tipos de peticiones WFS

#### GetCapabilities

Nos permite descubrir cuales son las capacidades del servidor. Como respuesta va a obtener un archivo en formato xml dónde podremos saber cuales son las características del servicio, las versiones de WFS soportadas por el servidor, las operaciones que soporta, cual es su sistema de referencia, sus coordenadas y metadatos de las capas de información que contiene.

##### Parámetros del GetCapabilities

| Parámetro | Obligatoriedad | Descripción                                                            |
|-----------|----------------|------------------------------------------------------------------------|
| VERSION   | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.3, 2.0, 2.0.2)          |
| SERVICE   | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WFS**)                  |
| REQUEST   | Obligatorio    | Nombre de la operación (**GetCapabilities**)                               |

Ejemplo: http://www.juntadeandalucia.es/institutodeestadisticaycartografia/geoserver-ieca/grid/wfs?REQUEST=GetCapabilities&SERVICE=WFS&VERSION=2.0.0

#### DescribeFeatureType

Devuelve la descripción de los tipos de objetos geográficos (XML schema de los feature types) que el servicio puede ofrecer. El servidor devuelve como respuesta un archivo XML. En la descripción del tipo de objeto geográfico se indica cómo hay que codificar los objetos geográficos para enviarlos como datos de entrada en operaciones de inserción, actualización o sustitución, y cómo se codifican cuando son datos de salida (en las respuestas de las operaciones GetPropertyValue, GetFeature o GetFeatureWithLock). Es una operación obligatoria.

##### Parámetros del DescribeFeatureType

| Parámetro    | Obligatoriedad | Descripción                                                                                                                                                           |
|--------------|----------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VERSION      | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.1, 1.3.0)                                                                                                         |
| SERVICE      | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WFS**)                                                                                                             |
| REQUEST      | Obligatorio    | Nombre de la operación (**DescribeFeatureType**)                                                                                                                         |
| TYPENAME     | Opcional       | Lista los nombres de los tipos de objeto geográfico que se van a describir, separados por comas. Si no se indica ninguno, devuelve la descripción de todos los tipos. |
| OUTPUTFORMAT | Opcional       | Formato de salida para describir los tipos de objetos. Por defecto GML3.2 (text/xml;subt ype=gml/3.2)                                                                                             |

Ejemplo: http://www.ign.es/wfs/redes-geodesicas?REQUEST=DescribeFeatureType&SERVICE=WFS&VERSION=1.1.0&TYPENAME=RED_ROI

#### GetFeature

Esta operación devuelve una selección de objetos geográficos en formato GML. Además, debe ser posible realizar un filtro en función de sus propiedades para obtener los objetos geográficos que desea y de realizar tanto consultas espaciales como no espaciales. Es una operación obligatoria.

Para definir el tipo de objeto geográfico a consultar, qué propiedades obtener y las restricciones a aplicar se utilizan el elemento Query

Para ver más operaciones y ejemplos https://www.idee.es/resources/documentos/RD_wfs_v2_0.pdf

##### Parámetros del GetFeature

| Parámetro       | Obligatoriedad | Descripción                                                                                                                                                                                                                                                                                                             |
|-----------------|----------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VERSION         | Obligatorio    | Versión de la especificación OGC (1.0.0, 1.1.0, 1.1.1, 1.3.0)                                                                                                                                                                                                                                                           |
| SERVICE         | Obligatorio    | Tipo de servicio al que va dirigida la petición (**WFS**)                                                                                                                                                                                                                                                               |
| REQUEST         | Obligatorio    | Nombre de la operación (**GetFeature**)                                                                                                                                                                                                                                                                           |
| TYPENAME        | Obligatorio    | Lista los nombres de los tipos de objeto geográfico que se van a describir, separados por comas. (Excepto cuando el parámetro RESOURCE_ID es especificado)                                                                                                                                                              |
| RESOURCEID      | Opcional       | Lista los identificadores únicos de los objetos geográficos que se quieren obtener. Mutuamente excluyente con FILTER y BBOX.                                                                                                                                                                                            |
| FILTER          | Opcional       | Describe un conjunto de características sobre las que operar. Se debe establecer un filtro por cada tipo de objeto geográfico listado en el parámetro TYPENAME                                                                                                                                                          |
| BBOX            | Opcional       | Solicitud mediante una bounding box (rectángulo envolvente). Mutuamente excluyente con RESOURCEID y FILTER.                                                                                                                                                                                                             |
| SORTBY          | Opcional       | Indica los nombres de las propiedades cuyos valores se van a utilizar para ordenar el resultado de la consulta. Se puede indicar si el orden es ascendente o descendente, valor ASC o DESC (Valor por defecto: orden descendente DESC). Ejemplo: SORTBY=Apellido ASC,Nota DESC                                          |
| FILTER_LANGUAGE | Opcional       | Indica el lenguaje que se emplea para codificar la expresión (valor de FILTER). Valor por defecto urn:ogc:def:queryLanguage:OGC-FES:Filter.                                                                                                                                                                             |
| SRSNAME         | Opcional       | Sistema de referencia que debe aplicarse en la geometría de los objetos geográficos resultantes de la petición. Si no se indica, el servicio devuelve las geometrías en el sistema que posea por defecto. El servidor debe ser capaz de transformar las geometrías en los distintos sistemas de referencia que soporta. |

Ejemplos: 

*Solicitud para obtener todos los vértices geodésicos entre los paralelos 38 y 39 entre las latitudes 0 y 2 (parámetro BBOX) de la Red de Orden Inferior (parámetro typeName) del servicio de redes geodésicas del Instituto Geográfico Nacional. Los resultados los pedimos en proyección UTM huso 30 (parámetro srsNAME) y en el formato XML (parámetro outputFormat)*

http://www.ign.es/wfs/redes-geodesicas?SERVICE=WFS&REQUEST=GetFeature&TYPENAME=RED_ROI&srsNAME=urn:ogc:def:crs:EPSG::25830&BBOX=38,0,39,2&outputFormat=text/xml;%20subtype=gml/3.1.1

*Solicitud del objeto geográfico denominado “Teide” del Nomenclátor Geográfico Básico de España usando el parámetro FILTER*

http://www.ign.es/wfs-inspire/ngbe?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&COUNT=10&TYPENAME=gn:NamedPlace&FILTER=%3cFilter%20xmlns:gn=%22http://inspire.ec.europa.eu/schemas/gn/4.0%22%3e%3cPropertyIsEqualTo%3e%3cValueReference%3egn:name/gn:GeographicalName/gn:spelling/gn:SpellingOfName/gn:text%3c/ValueReference%3e%3cLiteral%3eTeide%3c/Literal%3e%3c/PropertyIsEqualTo%3e%3c/Filter%3e

## GML

El GML (Geography Markup Language) es una codificación basada en XML, pensada para la descarga, transporte , almacenaje y intercambio, de la información geográfica sobre Internet, pero no de presentación final.

GML contempla la descripción de entidades geométricas y topológicas así como sus relaciones y atributos alfanuméricos mediante esquemas XML (XSD).

Esto permite a cada usuario o institución crear sus propios esquemas para describir de forma compleja objetos geográficos para después poderla compartir o vincular con otros esquemas

Se puede ver la especificación en [https://www.ogc.org/standards/gml](https://www.ogc.org/standards/gml)

``` xml
<?xml version="1.0" encoding="utf-8"?>
<!--Parcela Catastral de la D.G. del Catastro.-->
<!--La precisión es la que corresponde nominalmente a la escala de captura de la cartografía-->
<FeatureCollection xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:gml="http://www.opengis.net/gml/3.2" xmlns:xlink="http://www.w3.org/1999/xlink" xmlns:cp="http://inspire.ec.europa.eu/schemas/cp/4.0" xmlns:gmd="http://www.isotc211.org/2005/gmd" xsi:schemaLocation="http://www.opengis.net/wfs/2.0 http://schemas.opengis.net/wfs/2.0/wfs.xsd http://inspire.ec.europa.eu/schemas/cp/4.0 http://inspire.ec.europa.eu/schemas/cp/4.0/CadastralParcels.xsd" xmlns="http://www.opengis.net/wfs/2.0" timeStamp="2020-09-23T10:21:02" numberMatched="1" numberReturned="1">
<member>
<cp:CadastralParcel gml:id="ES.SDGC.CP.5049611DF2954G">
<cp:areaValue uom="m2">1978612</cp:areaValue>
<cp:beginLifespanVersion>2011-05-31T00:00:00</cp:beginLifespanVersion>
<cp:endLifespanVersion xsi:nil="true" nilReason="http://inspire.ec.europa.eu/codelist/VoidReasonValue/Unpopulated"></cp:endLifespanVersion>
<cp:geometry>
<gml:MultiSurface gml:id="MultiSurface_ES.SDGC.CP.5049611DF2954G" srsName="http://www.opengis.net/def/crs/EPSG/0/25831">
  <gml:surfaceMember>
  <gml:Surface gml:id="Surface_ES.SDGC.CP.5049611DF2954G.1" srsName="http://www.opengis.net/def/crs/EPSG/0/25831">
  <gml:patches>
  <gml:PolygonPatch>
  <gml:exterior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="578">423887.57 4594331.7 423879.49 4594342.66 423876.58 4594346.6 423873.49 4594350.78 423872.41 4594352.25 423870.09 4594355.38 423869.68 4594355.94 423867.24 4594359.32 423864.4 4594363.27 423860.33 4594368.9 423865.29 4594376.37 423876.45 4594393.45 423886.62 4594408.85 423905.97 4594438.33 423907.38 4594440.74 423909.5 4594444.1 423911.68 4594447.42 423913.93 4594450.7 423916.25 4594453.94 423932.58 4594478.69 423948.65 4594503.23 423951.61 4594507.73 423976.85 4594545.72 423982.82 4594554.82 423993.71 4594571.16 423998.25 4594577.9 424036.06 4594616.5 424078.88 4594625.36 424075.73 4594669.91 424085.01 4594691.54 424094 4594712.51 424104.21 4594736.09 424105.05 4594737.36 424104.82 4594737.52 424110.27 4594750.11 424134.12 4594785.62 424138.65 4594787.74 424140.95 4594788.81 424144.78 4594790.6 424145.25 4594797.71 424146.29 4594799.26 424150.53 4594805.79 424154.69 4594812.37 424160.14 4594821.16 424167.82 4594833.59 424175.95 4594846.95 424182.96 4594858.59 424184.8 4594861.62 424188.66 4594868.06 424192.44 4594874.53 424196.16 4594881.05 424199.8 4594887.61 424203.38 4594894.21 424206.89 4594900.85 424212.48 4594911.31 424219.21 4594924.05 424225.88 4594936.83 424228.35 4594941.74 424232.66 4594950.39 424237.5 4594960.35 424242.12 4594969.95 424243.72 4594973.38 424246.56 4594979.65 424248.84 4594984.82 424251.3 4594990.58 424253.66 4594996.38 424256.3 4595003.06 424258.21 4595008.06 424260.48 4595014.28 424264.21 4595024.69 424266.18 4595030.32 424280.88 4595050.89 424281.25 4595051.41 424274.5 4595053.71 424280.52 4595072.07 424283.32 4595076.98 424284.62 4595087.72 424287.03 4595096.14 424288.48 4595101.48 424290.95 4595110.89 424292.7 4595117.8 424297.57 4595137.68 424299.72 4595146.87 424301.73 4595155.79 424304.68 4595169.4 424305.74 4595174.81 424306.67 4595179.93 424307 4595181.92 424307.93 4595182.69 424307.83 4595185.48 424307.79 4595186.45 424309.11 4595193.75 424310.32 4595200.89 424311.45 4595208.05 424312.5 4595215.21 424313.71 4595224.24 424314.92 4595233.61 424316.05 4595242.99 424316.17 4595244.13 424327.72 4595282.51 424322.66 4595295.73 424320.91 4595305.68 424321.21 4595311.98 424321.65 4595327.76 424322.03 4595343.77 424322.2 4595358 424322.09 4595374.14 424322.02 4595378.04 424323.43 4595383.08 424328.16 4595405.5 424328.91 4595413.1 424324.24 4595418.96 424323.21 4595429.76 424325.59 4595479.2 424326.47 4595478.7 424329.96 4595476.41 424332.82 4595474.32 424336.08 4595472.46 424348.54 4595473.35 424349.53 4595473.24 424350.3 4595473.7 424350.73 4595474.38 424351.21 4595475.94 424351.15 4595476.92 424350.99 4595478.16 424350.53 4595479.47 424349.99 4595480.65 424349.53 4595481.84 424348.7 4595482.82 424347.94 4595485.78 424347.12 4595488.79 424346.2 4595493.1 424345.4 4595497.52 424343.86 4595501.58 424342.45 4595504.95 424341.15 4595508.81 424340.04 4595512.04 424338.96 4595515.64 424337.76 4595519.93 424336.65 4595525.65 424335.43 4595529.95 424334.35 4595534.66 424333.2 4595539.53 424331.35 4595544.52 424330.17 4595549.26 424329.2 4595554.21 424328.51 4595557.02 424326.86 4595559.74 424324.06 4595563.45 424321.77 4595566.24 424320 4595569.12 424318.33 4595572.4 424317.69 4595575.56 424317.62 4595577.85 424317.79 4595580.28 424317.18 4595583.93 424317.55 4595585.89 424317.8 4595587.2 424318.75 4595588.69 424320.04 4595589.92 424320.92 4595590.5 424322.07 4595591.97 424323.47 4595593.13 424324.89 4595594.54 424324.4 4595595.06 424315.91 4595594.8 424316.28 4595597.59 424354.31 4595599.77 424385.47 4595601.77 424405.98 4595602.83 424428.51 4595604.36 424455.19 4595606.11 424478.53 4595607.66 424533.62 4595611.48 424559.82 4595613.33 424609.2 4595616.7 424642.23 4595618.94 424686.42 4595621.89 424726.78 4595624.75 424733.47 4595625.3 424740.76 4595625.82 424748.06 4595626.27 424755.36 4595626.64 424762.66 4595626.92 424765.67 4595627.02 424771.52 4595627.12 424777.37 4595627.14 424783.23 4595627.08 424789.08 4595626.94 424794.93 4595626.72 424800.78 4595626.43 424806.62 4595626.05 424809.84 4595625.81 424815.31 4595625.33 424820.77 4595624.77 424826.23 4595624.13 424831.68 4595623.41 424837.11 4595622.61 424842.54 4595621.74 424849.38 4595620.52 424853.97 4595619.77 424858.56 4595618.95 424867.92 4595617.1 424876.74 4595614.89 424881.24 4595613.68 424883.28 4595613.11 424889.77 4595611.23 424896.24 4595609.28 424902.68 4595607.25 424905.98 4595606.05 424910.09 4595604.72 424916.75 4595602.42 424924.54 4595599.58 424932.57 4595596.45 424938.94 4595593.89 424946.92 4595590.48 424955.73 4595586.54 424964.52 4595582.28 424973.82 4595577.46 424984.17 4595571.94 424992.29 4595567.24 424998.97 4595563.16 425006.27 4595558.56 425012.1 4595554.68 425018.65 4595550.23 425025.19 4595545.71 425058.87 4595519.69 425059.74 4595519.09 425080.25 4595504.06 425080.72 4595503.73 425092.73 4595494.92 425104.59 4595486.24 425264.54 4595344.88 425326.37 4595290.22 425405.98 4595219.9 425420.57 4595207.2 425446.98 4595184.41 425472.87 4595162.17 425499.68 4595139.03 425500.15 4595138.64 425526.08 4595116.29 425551.07 4595094.32 425554.21 4595091.52 425576 4595072.22 425590.98 4595059.09 425606.57 4595045.54 425612.83 4595040.17 425623.24 4595031.26 425631.83 4595023.8 425656.07 4595002.93 425660.88 4594998.77 425671.18 4594989.83 425689.45 4594974.17 425725.2 4594943.65 425743.15 4594928.5 425761.16 4594913.44 425770.83 4594905.47 425783.26 4594895.08 425787.7 4594891.43 425795.95 4594884.67 425803.39 4594878.37 425813.94 4594869.33 425821.61 4594862.72 425831.67 4594853.93 425837.35 4594848.94 425849.42 4594837.82 425861.44 4594826.66 425876.7 4594812.38 425882.31 4594807.04 425887.86 4594801.64 425893.83 4594795.71 425905.98 4594784.29 425925.47 4594766.23 425949.17 4594743.85 425973.97 4594720.57 426007.79 4594688.95 426033.66 4594664.52 426068.66 4594631.72 426075.92 4594624.96 426082.92 4594618.38 426096.18 4594605.97 426103.5 4594599.14 426120.58 4594582.9 426126.84 4594577.15 426124.24 4594556.33 426123.12 4594547.27 426122.92 4594545.7 426122.52 4594543.93 426121.51 4594539.48 426120.87 4594536.51 426120.67 4594535.56 426120.32 4594534.08 426119.76 4594531.66 426118.77 4594527.78 426117.7 4594523.92 426116.55 4594520.08 426115.33 4594516.27 426114.03 4594512.48 426112.83 4594509.16 426111.21 4594505.07 426109.51 4594501.01 426107.74 4594496.99 426105.9 4594493 426103.99 4594489.04 426102.01 4594485.12 426099.95 4594481.22 426099.44 4594480.28 426096.9 4594476.04 426094.29 4594471.86 426091.62 4594467.72 426088.88 4594463.62 426086.07 4594459.56 426083.2 4594455.54 426080.26 4594451.58 426077.26 4594447.68 426074.4 4594444.06 426066.49 4594434.5 426062.26 4594429.46 426058.53 4594425 426057.07 4594423.28 426050.5 4594415.57 426048.61 4594413.36 426045.97 4594410.28 426042.72 4594406.47 426036.78 4594399.64 426030.78 4594392.88 426028.91 4594390.8 426026.55 4594388.18 426025.19 4594386.68 426018.5 4594379.24 426011.76 4594371.86 426004.97 4594364.54 426004.32 4594363.85 426001.13 4594360.47 425998.11 4594357.27 425997.33 4594356.45 425991.92 4594350.66 425991.67 4594350.39 425990.67 4594349.35 425988 4594346.55 425986.45 4594344.93 425982.92 4594341.3 425980.93 4594339.26 425975.61 4594333.9 425975.44 4594333.72 425975.35 4594333.64 425971.47 4594329.81 425969.71 4594328.08 425968.89 4594327.28 425961.04 4594319.56 425958.02 4594316.59 425950.69 4594309.44 425947.1 4594305.95 425941.58 4594300.63 425940.44 4594299.52 425936.46 4594295.71 425927.14 4594288.17 425923.86 4594285.53 425915.62 4594278.92 425915.57 4594278.88 425914.5 4594278.03 425912.25 4594276.25 425905.97 4594271.28 425899.47 4594266.35 425887.9 4594257.53 425880.64 4594252.26 425877.18 4594249.75 425869.52 4594243.88 425861.1 4594237.44 425855.08 4594232.92 425851.41 4594230.18 425844.07 4594224.83 425841.77 4594223.16 425831.53 4594216.03 425822.22 4594209.24 425812.25 4594202.33 425802.5 4594195.8 425801.32 4594194.99 425790.89 4594187.86 425779.76 4594180.65 425774.04 4594176.98 425766.47 4594172.13 425753.27 4594163.54 425744.21 4594157.89 425733.48 4594151.31 425720.93 4594143.62 425707.6 4594135.95 425704.76 4594134.28 425694.6 4594128.3 425686.47 4594123.74 425681.68 4594121.06 425673.77 4594116.7 425669.64 4594114.43 425659.8 4594109.32 425653.7 4594106.06 425646.3 4594102.1 425634 4594095.89 425632.78 4594095.28 425623.29 4594090.57 425616.71 4594087.49 425616.48 4594087.38 425610.57 4594084.62 425602.32 4594080.84 425595.36 4594077.66 425593.45 4594076.78 425585.69 4594073.21 425585.68 4594094.89 425581.46 4594094.07 425570.58 4594095.39 425555.97 4594100.78 425539.69 4594102.19 425533.42 4594103.46 425523.85 4594105.4 425507.6 4594108.62 425490.45 4594111.96 425473.31 4594115.39 425455.86 4594118.53 425436.8 4594121.3 425418.23 4594123.77 425401.29 4594126.9 425388.61 4594128.35 425378.15 4594130.45 425371.79 4594130.58 425366.94 4594130.68 425358.13 4594130.25 425347.27 4594127.96 425335.62 4594125.59 425327.84 4594121.94 425319.74 4594117 425313.14 4594111.94 425307.52 4594106.14 425298.39 4594099.53 425288.55 4594093.02 425280.84 4594087.48 425272.75 4594083.03 425264.07 4594079.41 425252.54 4594073.33 425243.29 4594070.72 425235.62 4594067.47 425227.06 4594064.54 425216.59 4594061.55 425204.88 4594056.17 425197.27 4594051.03 425189.1 4594047.19 425179.1 4594042.79 425169.75 4594040.37 425160.72 4594039.35 425153.61 4594038.98 425142.93 4594040.3 425131.55 4594041.82 425118.4 4594044.68 425104.67 4594048.94 425095.12 4594051.23 425085.96 4594054.31 425081.56 4594059.49 425080.99 4594050.6 425080.17 4594039.33 425079.66 4594036.48 425078.38 4594029.37 425075.23 4594022.03 425071.69 4594009.71 425068.16 4593998.67 425068.5 4593990.27 425067.24 4593977.01 425067.98 4593963.19 425062.71 4593970.09 425056.44 4593972.22 425048.98 4593974.46 425041.42 4593976.51 425031.21 4593976.9 425021.62 4593977.2 425013.81 4593977.05 425006.98 4593975.98 425002.17 4593975.48 424993.24 4593973.55 424986.89 4593971.18 424980.97 4593970.7 424976.59 4593971.48 424974.98 4593976.11 424975.02 4593988.51 424973.1 4594003.14 424972.69 4594017.75 424972.72 4594029.34 424973.16 4594038.19 424973.38 4594042.62 424973.7 4594059.01 424973.83 4594075.81 424974.84 4594091.98 424974.82 4594105.87 424973.7 4594115.2 424972.53 4594119.69 424971.84 4594122.32 424969.33 4594126.97 424964.88 4594129.76 424959.35 4594132.97 424949.94 4594138.65 424941.6 4594141.71 424932.39 4594146.29 424922.3 4594147.59 424909.87 4594151.03 424901.1 4594153.3 424895.65 4594156.01 424889.03 4594160.33 424884.22 4594165.03 424884.91 4594159.21 424884.8 4594153.33 424886.75 4594145.89 424890.96 4594137.24 424849.56 4594148.8 424806.93 4594162.72 424764.95 4594178.5 424723.7 4594196.1 424689.25 4594212.01 424655.39 4594229.18 424622.17 4594247.54 424600.99 4594260.08 424580.12 4594273.11 424559.55 4594286.63 424526.26 4594310.03 424493.84 4594334.61 424462.33 4594360.34 424442.77 4594376.71 424423.35 4594393.25 424404.08 4594409.96 424329.55 4594479.57 424326.05 4594483.2 424322.41 4594486.67 424318.61 4594489.98 424314.67 4594493.12 424310.6 4594496.08 424306.41 4594498.87 424302.09 4594501.47 424297.67 4594503.88 424293.15 4594506.09 424287.34 4594508.63 424281.61 4594494.73 424265.84 4594501.36 424250.17 4594508.22 424234.61 4594515.32 424225.23 4594519.65 424215.9 4594524.09 424206.62 4594528.66 424212.89 4594541.18 424213.33 4594542.07 424184.81 4594557.05 424156.91 4594573.14 424129.67 4594590.32 423960.4 4594367.67 423963.7 4594366.83 423961.46 4594363.71 423960.34 4594362.14 423958.5 4594359.58 423957.23 4594357.81 423955.07 4594354.8 423953.57 4594352.71 423952.41 4594351.08 423951.62 4594349.97 423949.72 4594347.33 423947.95 4594344.85 423946.46 4594342.78 423944.35 4594339.84 423941.96 4594336.51 423939.9 4594333.63 423937.45 4594330.21 423936.35 4594328.68 423936.74 4594328.4 423936.32 4594328.14 423935.62 4594327.62 423934.96 4594327.03 423934.37 4594326.39 423933.93 4594325.84 423920.3 4594328.91 423913.82 4594329.92 423909.26 4594330.45 423905.97 4594330.72 423900.23 4594330.95 423890.44 4594331.67 423887.57 4594331.7</gml:posList>
      </gml:LinearRing>
  </gml:exterior>
  <gml:interior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="73">425164.92 4594241.97 425176.47 4594233.04 425206.28 4594209.61 425210.27 4594206.39 425247.32 4594176.49 425254.64 4594171.69 425262.15 4594167.2 425268.7 4594163.65 425269.85 4594163.02 425277.82 4594159.33 425283.08 4594157.23 425285.98 4594156.08 425294.3 4594153.27 425302.76 4594150.92 425325.43 4594146.87 425345.25 4594143.8 425354.86 4594144.35 425364.42 4594145.52 425373.88 4594147.31 425404.81 4594155.47 425407.44 4594158.31 425412.66 4594163.97 425413.72 4594165.39 425418.78 4594170.92 425420.11 4594172.37 425429.1 4594164.38 425431.05 4594163.76 425432 4594163.85 425448.4 4594169 425512.26 4594189.58 425514.25 4594190.22 425522.73 4594192.81 425530.84 4594195.91 425538.81 4594199.39 425546.97 4594202.76 425575.79 4594216.63 425580.9 4594219.28 425586.12 4594222.4 425591.44 4594225.8 425593.45 4594227.09 425595.44 4594228.83 425596.91 4594231.03 425597.76 4594233.54 425597.92 4594236.19 425597.85 4594238.98 425597.24 4594241.71 425596.12 4594244.27 425594.25 4594247.97 425594.07 4594248.2 425592.79 4594249.83 425591.09 4594251.48 425587.96 4594254.1 425518.23 4594315.26 425517.96 4594314.97 425513.43 4594310.16 425511.88 4594311.31 425507.28 4594314.74 425505.67 4594315.93 425510.62 4594321.48 425510.82 4594321.71 425473.11 4594354.38 425461.28 4594364.63 425445.9 4594348.78 425443.73 4594350.77 425422.24 4594370.4 425421.72 4594369.82 425355.43 4594296.39 425352.17 4594292.78 425272.84 4594364.58 425271.56 4594363.17 425251.64 4594341.15 425163.12 4594243.36 425164.92 4594241.97</gml:posList>
      </gml:LinearRing>
  </gml:interior>
  <gml:interior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="41">425085.7 4595246.71 425094.8 4595257.19 425120.04 4595286.19 425123.63 4595283.05 425123.72 4595282.97 425124.2 4595283.52 425136.06 4595297.12 425136.08 4595297.14 425129.26 4595303.1 425128.56 4595303.7 425132.45 4595308.18 425134.1 4595310.09 425133.15 4595310.86 425119.56 4595321.77 425096.25 4595342.29 425067.15 4595367.89 425064.54 4595370.18 425059.33 4595374.74 425055.65 4595377.98 425049.86 4595383.3 425046.86 4595386.37 425042.18 4595392.13 425037.81 4595398.51 425034.69 4595404.03 425032.44 4595408.98 425031.68 4595410.95 425029.23 4595408.07 425003.04 4595377.27 425002.68 4595364.56 425011.69 4595357 425002.58 4595346.38 425021.82 4595300.95 425009.6 4595285.81 424995.92 4595267.45 425001.5 4595252.92 425026.85 4595239.43 425022.51 4595229.52 425032.52 4595213.68 425041.87 4595206.63 425072.28 4595231.29 425085.7 4595246.71</gml:posList>
      </gml:LinearRing>
  </gml:interior>
  <gml:interior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="47">425067.16 4595383.1 425064.7 4595380.3 425067.27 4595378.08 425070.23 4595375.4 425071.97 4595374.02 425076.03 4595370.45 425085.55 4595362.09 425097.32 4595351.67 425107.89 4595342.35 425116.26 4595334.99 425121.18 4595330.61 425128.12 4595324.49 425131.4 4595321.64 425136.68 4595317.3 425142.27 4595313.58 425146.95 4595311.08 425148.66 4595310.24 425149.46 4595309.85 425155.13 4595307.71 425161.16 4595305.99 425167.67 4595304.75 425175.08 4595304.08 425180.68 4595304.09 425186.2 4595304.48 425192.87 4595305.46 425199.84 4595307.02 425205.26 4595308.64 425209.65 4595310.29 425209.95 4595310.43 425215.74 4595313.13 425221.33 4595316.48 425225.98 4595319.76 425230.16 4595323.39 425230.51 4595323.69 425231.27 4595324.46 425231.58 4595324.77 425110.83 4595432.94 425084.24 4595402.59 425083.67 4595401.94 425079.25 4595396.9 425078.31 4595395.83 425077.68 4595395.11 425075.62 4595392.76 425071.98 4595388.6 425071.01 4595387.5 425067.51 4595383.51 425067.16 4595383.1</gml:posList>
      </gml:LinearRing>
  </gml:interior>
  <gml:interior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="7">425536.49 4594331.99 425604.7 4594272.14 425649.55 4594323.25 425581.34 4594383.1 425558.8 4594357.42 425554.29 4594352.3 425536.49 4594331.99</gml:posList>
      </gml:LinearRing>
  </gml:interior>
  <gml:interior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="25">425527.13 4594340.22 425612.61 4594438.61 425578.76 4594484.34 425569.02 4594466.89 425516.34 4594372.51 425513.75 4594373.84 425512.18 4594371.6 425510.79 4594369.27 425510.25 4594368.26 425509.79 4594367.01 425509.6 4594366.22 425509.52 4594365.71 425509.45 4594364.38 425509.49 4594362.47 425509.72 4594360.57 425509.8 4594360.19 425510.16 4594358.71 425510.81 4594356.81 425511.52 4594355.22 425511.64 4594354.99 425512.65 4594353.25 425513.25 4594352.55 425513.78 4594351.92 425524.73 4594342.32 425527.13 4594340.22</gml:posList>
      </gml:LinearRing>
  </gml:interior>
  <gml:interior>
      <gml:LinearRing>
          <gml:posList srsDimension="2" count="8">425908.55 4594760.46 425824.5 4594834.58 425804.49 4594811.89 425804.73 4594808.38 425887.23 4594736.14 425888.74 4594737.57 425904.41 4594755.64 425908.55 4594760.46</gml:posList>
      </gml:LinearRing>
  </gml:interior>
  </gml:PolygonPatch>
  </gml:patches>
  </gml:Surface>
  </gml:surfaceMember>
</gml:MultiSurface>
</cp:geometry>
<cp:inspireId>
<Identifier xmlns="http://inspire.ec.europa.eu/schemas/base/3.3">
  <localId>5049611DF2954G</localId>
  <namespace>ES.SDGC.CP</namespace>
</Identifier>
</cp:inspireId>
<cp:label>11</cp:label>
<cp:nationalCadastralReference>5049611DF2954G</cp:nationalCadastralReference>
<cp:referencePoint>
<gml:Point gml:id="ReferencePoint_ES.SDGC.CP.5049611DF2954G" srsName="http://www.opengis.net/def/crs/EPSG/0/25831">
  <gml:pos>425003.4 4594786.65</gml:pos>
</gml:Point>
</cp:referencePoint>
</cp:CadastralParcel>
</member>
</FeatureCollection>
```
## SLD

EL SLD (Style Layer Descriptor) es una codificación xml para permitir al usuario ampliar las especificaciones (WMS) y definir símbolos de objetos.

El usuario puede aplicar estilos a los objetos de forma diferentes de cómo han sido configurados en el servidor.

Los servidores WMS que soportan SLD permiten añadir los parámetros SLD dónde como valor se describe la url dónde se encuentra el documento xml o SLDBODY, dónde se pasan los valores SLD de forma directa (método poco recomendado) a las peticiones WMS GetMap.

Ejemplos: 

*Petición sin SLD*
http://servicios.idee.es/wms-inspire/hidrografia?VERSION=1.1.1&SERVICE=WMS&REQUEST=GetMap&SRS=EPSG:25830&FORMAT=image/png&BBOX=419685.23094987,4082028.7934849,582245.81538657,4201830.8601227&WIDTH=1247&HEIGHT=919&LAYERS=HY.Network

*Petición con SLD*
http://servicios.idee.es/wms-inspire/hidrografia?VERSION=1.1.1&SERVICE=WMS&REQUEST=GetMap&SRS=EPSG:25830&FORMAT=image/png&BBOX=419685.23094987,4082028.7934849,582245.81538657,4201830.8601227&WIDTH=1247&HEIGHT=919&SLD=http://betaserver.icgc.cat/dades/sld.xml

http://servicios.idee.es/wms-inspire/hidrografia?VERSION=1.1.1&SERVICE=WMS&REQUEST=GetMap&SRS=EPSG:25830&FORMAT=image/png&BBOX=419685.23094987,4082028.7934849,582245.81538657,4201830.8601227&WIDTH=1247&HEIGHT=919&SLD_BODY=%3CStyledLayerDescriptor+version%3D%221.0.0%22+xmlns%3D%22http%3A%2F%2Fwww.opengis.net%2Fsld%22+xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22+xmlns%3Axsi%3D%22http%3A%2F%2Fwww.w3.org%2F2001%2FXMLSchemainstance%22+xsi%3AschemaLocation%3D%22http%3A%2F%2Fschemas.opengis.net%2Fsld%2F1.0.0%2FStyledLayerDescriptor.xsd%22+xmlns%3Ase%3D%22http%3A%2F%2Fwww.opengis.net%2Fse%22+xmlns%3Abcn%3D%22http%3A%2F%2Fwww.ign.es%2Fbcn25%22%3E%3CNamedLayer%3E%3CName%3EHY.Network%3C%2FName%3E%3CUserStyle%3E%3CFeatureTypeStyle%3E%3Cse%3AFeatureTypeName%3EHY.Network%3C%2Fse%3AFeatureTypeName%3E%3CRule%3E%3CLineSymbolizer%3E%3CStroke%3E%3CCssParameter+name%3D%22stroke%22%3E%23ffaaff%3C%2FCssParameter%3E%3CCssParameter+name%3D%22strokewidth%22%3E5.0%3C%2FCssParameter%3E%3C%2FStroke%3E%3C%2FLineSymbolizer%3E%3C%2FRule%3E%3C%2FFeatureTypeStyle%3E%3C%2FUserStyle%3E%3C%2FNamedLayer%3E%3C%2FStyledLayerDescriptor%3E

Se puede ver la especificación en [https://www.ogc.org/standards/sld](https://www.ogc.org/standards/sld)

## SOS

El estandar SOS (Sensor Observation Service) es un servicio de datos. Define un interface estandarizado y operaciones para el acceso a observaciones desde sensores y sistemas de sensores que es consistente con todos los sistemas, incluyendo remoto, in-situ, fijos y sensores móviles. SOS proporciona resultados de consultas en el formato estandar de observación y medida (en inglés Observation and Mesurements, O&M) para modelizar observaciones de sensores y la especificación SensorML para modelizar sensores y sistemas sensor.

Una observación es un evento cuyo resultado es una estimación del valor de alguna propiedad de la característica de interés, obtenida usando un procedimiento específico. 

Las observaciones se definen por

* eventTime – Cuando se tomó la medida
* featureOfInterest – La entidad que se mide
* observedProperty - La característica que se midió
* procedure - cómo se midió

Operaciones SOS requeridas incluyen: 
* GetObservation - acceso a datos de observación y medida del sensor a través de una consulta espacio-temporal que se puede filtrar por un fenómeno 
* GetCapabilities - Metadatos del servicio SOS 
* DescribeSensor - información sobre los sensores, sus procesos y plataformas en SensorML

Operaciones opcionales incluyen: 
* GetResult 
* GetFeatureOfInterest 
* GetFeatureOfInterestTime 
* DescribeFeatureofInterest 
* DescribeObservationType
* DescribeResultModel
* Register Sensor
* InsertObservation

Algunos conceptos de interés para trabajar con sensores [^1]

### Offering

Los datos servidos por un servicio SOS se agrupan en diferentes offerings. Por ejemplo, un servicio SOS “meteo” podría tener los siguientes offerings: imágenes de satélite, datos de radar, medidas de estaciones meteorológicas, mapas de predicción, etc. Cada offering expone datos de un sensor o una red de sensores, descritos como un procedure.

Se pueden considerarlos distintos Offerings como secciones co cajones que clasifican los diferentes datos según su origen o naturaleza.

### Procedure

Una procedure describe un sensor, una colección de sensores, or un proceso que produce un conjunto de observaciones. Proporciona metadatos sobre las entradas y salidas del sensor, datos de calibración y procesado, información de contacto, y la disponibilidad de datos (extensiones espacial y temporal), etc.

Normalmente viene descrito en el formato SensorML.

Se puede considerar una Procedure como una ficha de metadatos acerca del (los) sensor(es) o proceso(s) a cargo de generar los datos que ofrece el servicio.

Un Offering está relacionado con una sola Procedure, mientras que una Procedure puede ser usada en diferentes Offerings. Por ejemplo, una Procedure podría ser una “Red de Estaciones Meteorológicas”, y ésta misma red de estaciones ser usada en diferentes Offerings, por ejemplo para diferentes períodos de tiempo. El Offerinf sería “Medidas de la Red de Estaciones Meteorológicas para el año 2015”.

### Feature of Interest

Cada observacion en un servicio SOS está ligada a una Feature Of Interest (FoI), que habitualmente determina el lugar donde el fenómeno observado tuvo lugar. Por ejemplo, para imágenes satélite, la FoI podría ser su footprint (polígono que determina el área fotografiada sobre la superficie de la tierra), o para una medición de temperatura, la FoI podría ser la ubicación del termómetro (punto).

Las FoI pueden considerarse como el conjunto de lugares a los que están referidos los datos.

### Observed Property

La propiedad que se mide, tal que: Temperatura, Dirección del viento, Nubosidad, Número de vehículos... puede ser un valor numérico (una cantidad y una unidad de medida), lógico (toma los valores verdadero o falso), categórico (un valor de entre una lista: soleado, nublado, lluvioso), o descriptivo (un texto).

### Observation

Finalmente, una Observation es el valor que toma una Observed Property en un momento (Phenomenon Time) y un lugar (Feature Of Interest) dados. Por ejemplo: “La temperatura en Barcelona el 22/09/2015 a las 11:52 es de 23 grados centígrados”.

Se puede ver la especificación en [https://www.ogc.org/standards/sos](https://www.ogc.org/standards/sos)

## CSW

La especificación (CSW Catalog Service for the Web) establece cómo deben estructurarse e implementarse los servicios de catalogación y de búsqueda de metadatos geospaciales, estableciendo el subconjunto mínimo de metadatos que deben ser interrogables.

#### Tipos de peticiones CSW

Las operaciones que se definen en este estándar OGC son 7, siendo 4 obligatorias y 3 opcionales.

| Operación       | Obligatoriedad | Descripción                                                                                                                                                                                                                                          |
|-----------------|----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| GetCapabilities | Obligatorio    | Solicitud de las características del CSW. Devuelve el documento Capabilities, en XML                                                                                                                                                                 |
| DescribeRecord  | Opcional       | Permite realizar búsquedas de elementos del modelo de datos implementado en el servicio de catálogo. Con esta operación se pueden obtener las descripciones de todos los elementos del modelo datos o sólo de algunos                                |
| GetDomain       | Opcional       | Permite a los usuarios consultar los valores permitidos de un parámetro o propiedad determinados. Se utiliza para obtener información acerca del rango de valores que puede poseer un elemento del registro de metadatos o un parámetro de consulta. |
| GetRecords      | Obligatorio    | Envía una consulta(query) al catálogo y devuelve todos los registros de metadatos de los recursos catalogados que satisfacen los requisitos de la consulta                                                                                           |
| GetRecordById   | Obligatorio    | Obtiene los registros de metadatos de los recursos catalogados mediante los identificadores de dichos registros de metadatos                                                                                                                         |
| Harvest         | Opcional       | Esta operación indica la URI que apunta al recurso de metadatos a insertar o actualizar en el catálogo. El servicio de catálogo se encarga de analizarlo y de crear o modificar registros de metadatos en el catálogo                                |
| Transaction     | Opcional       | Define una interfaz para crear, actualizar y borrar registros del catálogo de metadatos                                                                                                                                                              |

Se puede ver la especificación en [https://www.ogc.org/standards/cat](https://www.ogc.org/standards/cat)

## Otros estándares OGC

### Web Map Context (WMC)

Especifica formato estandarizado para almacenar un contexto. Un contexto recoge la información necesaria para reproducir las condiciones de una determinada sesión de uso de un cliente, de tal forma que ese cliente pueda restablecerlas posteriormente. El contexto se almacena en un archivo XML. [^2]

En el contexto se almacena información sobre las capas que forman el mapa representado por el cliente y los servidores de los que estas se obtienen, la región cubierta por el mapa, así como información adicional para anotar este mapa.

Permite:
* Crear vistas predefinidas, mapas temáticos
* Guardar Y/o cargar on-line estas vistas

Se puede ver la especificación en [https://www.ogc.org/standards/wmc](https://www.ogc.org/standards/wmc)

### Keyhole Markup Language (KML)

Es un lenguaje XML centrado en la descripción y visualización de la geoinformación en actuales y futuras aplicaciones webs de gestión de mapas (2d y 3d). 

Este lenguaje fue presentado por Google al OGC con el objetivo de incoroporarlo como un estándar. Actualmente OGC y Google trabajan en colaboración para asegurar este proceso.

Se puede ver la especificación en [https://www.ogc.org/standards/kml](https://www.ogc.org/standards/kml)

### Web Coverage Service(WCS)

Amplía la interfaz Web Map Server para permitir el acceso a "coberturas" geoespaciales que representen valores o propiedades de localizaciones geográficas; más que los mapas generados por WMS (imágenes). 

La diferencia principal con el WMS es que el servicio WCS proporciona los datos junto con su descripción detallada, define peticiones con una sintaxis rica para obtener esos datos y devuelve la información con su semántica original, lo cual permite que puedan ser interpretados, extrapolados, etc., y no sólo representados de forma estática.

Básicamente sirve para descargar archivos raster a escala 1 a 1 y preparados para poder trabajarlos en un sig raster.

Princpales inerfaces:
* GetCapabilities
* DescribeCoverage
* GetCoverage

### Web Processing Service (WPS)

Servicio de publicación de procesos geoespaciales en la Web. Se entiende por procesos cualquier algoritmo, cálculo o modelo, que opere sobre datos espacialmente referenciados tanto en formato raster como vectorial, de este modo un WPS puede ofrecer cualquier tipo de funcionalidad GIS a través de una red.

Se puede ver la especificación en [https://www.ogc.org/standards/wps](https://www.ogc.org/standards/wps)

!!! question "Ejercicio 3,5 pts"
    En un documento de texto poner:

    * La url de una IDE (0,5 pt)
    * Una captura de una búsqueda en el catálogo de la IDE de servicios WMS (0,5 pt)
    * Seleccionar un servicio WMS del catálogo de la IDE
        * Escribir la url del getCapabilities (1 pt)
        * Escribir la url de una petición getMap (1 pt)
        * Captura de la imagen retornada de la petición getMap (0,5 pt)

## Recursos 

https://www.ogc.org/standards/

https://www.idee.es/web/guest/rincon-del-desarrollador

## Referencias

[^1]: https://sensor-widgets.readthedocs.io/es/latest/sos.html#
[^2]: https://github.com/volaya/libro-sig/releases/download/v3.0/Sistemas.de.Informacion.Geografica.pdf
