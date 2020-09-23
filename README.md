# SDIS-mgeo-20-21

[https://gss-mtig.github.io/sdis-mgeo-20-21/](https://gss-mtig.github.io/sdis-mgeo-20-21/)

## Temario

* Introducción a las IDE's 
    * Antecedentes
    * Qué es una IDE?
    * Componentes de una IDE
    * IDEC
    * IDEE
    * INSPIRE
    * Otras IDES

* Estándares OCG
    * Qué es OGC?
    * Estándares OGC
    * WMS
    * WMTS
    * WFS
    * GML
    * SLD
    * SOS
    * CSW
    * Otros estándares OGC

* Estándares ISO
    * Qué es ISO?
    * Familia ISO19x
    * ISO 19115
    * ISO 19119
    * ISO 19139
    * Ejemplos de implementación de estándares

* Metadatos
    * Qué es un catálogo de metadatos
    * Creación de metadatos desde ArcGIS
    * Exportación desde ArcGIS e importación en GeoNetwork

* Servidores de mapes
    * Qué es un geoservicio?
    * Qué es un servidor de mapas?
    * Servidores de mapas estándar
    * Cómo preparar la geoinformación
        * BBDD
        * RAW data
        * Pirámides cache
    * Práctica implementar un servidor de mapas

* Servicios WMTS
    * Creación de teselas desde Geoserver
    * Creación de teselas desde ArcGIS Online

* Clientes de mapa
    * Clientes web
    * Clientes desktop
    * Práctica conectar servicios OGC con QGIS
    * Práctica conectar servicios OGC con Leaflet
    * ArcGIS Online


## Entorno

Se puede crear un entorno para generar la documentación instalando [Anaconda](https://www.anaconda.com/)

Una vez instalado el Anaconda crear un *enviroment* donde instalar el mkdocs

Para crear el *enviroment* abrir la consola de Anaconda y escribir
```bash
conda create --name <NOMBRE_DEL_ENVIROMENT>
```

Para activar el nuevo *enviroment* escribir
```bash
conda activate <NOMBRE_DEL_ENVIROMENT>
```

### Herramienta de documentación

Se usa [mkdocs](http://mkdocs.org) con el tema [mkdocs-material](https://squidfunk.github.io/mkdocs-material/).

Desinstalar versiones anteriores de mkdocs:

```bash
    sudo pip uninstall mkdocs
```

E instalar con el comando:

```bash
pip install mkdocs-material
```

### Comandos mkdocs

* `mkdocs serve`: Arranca un servidor web con auto-recarga.
* `mkdocs build`: Compila la documentación en html.
* `mkdocs gh-deploy`: Publica la documentación en gh-pages.

### Layout

    mkdocs.yml    # El fichero de configuración.
    docs/
        index.md  # La portada.
        ...       # Otras páginas en markdown, imágenes, etc.

### Markdown

* Chuleta rápida sobre links, imágenes y tablas en markdown: http://www.mkdocs.org/user-guide/writing-your-docs/#linking-documents
* [Especificación Markdown](http://spec.commonmark.org/0.28/) completa.
* Visual Studio Code ofrece una vista de Preview que va mostrando el resultado del markdown en tiempo real sin tener que salir del editor.
