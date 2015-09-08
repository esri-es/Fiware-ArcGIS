Esta herramienta ha sido realizada para poder insertar la información de los sensores recibida de un Context Broker en una cuenta de ArcGIS Online o ArcGIS Server.

A continuación se explica como instalar el conector.

# Fiware-ArcGIS

## Instalación y ejecución

Para instalar Fiware-ArcGIS tan solo hay que ejecutar las siguientes órdenes:

``` 
$ git clone git@github.com:esri-es/fiware_arcgis.git
$ cd fiware_arcgis
$ npm install
$ node fiware_arcgis.js 
```

Con esta orden lanzamos el servidor y queda a la espera de recibir peticiones del Context Broker para procesar la información y añadirla a ArcGIS Online/Server.

## Fichero de configuración

Vamos el fichero de configuraciones del servidor (*config.json*):
```javascript
{
    "debug": false,
    "port": 4003,
    "username": "<tu usuario>",
    "password": "<tu password>",
    "root_url": "www.arcgis.com",
    "services_url": "services1.arcgis.com",
    "account_id": "<tu id de cuenta>",
    "sensors":{

        "traffic" : {
            "serviceName": "Sensores tipo TRAFFIC de Santander",
            "route": "/santander_traffic",
            "fields": [
                ["TimeInstant", "esriFieldTypeDate"],
                ["id", "esriFieldTypeString"],
                ["Latitud", "esriFieldTypeDouble"],
                ["Longitud", "esriFieldTypeDouble"],
                ["average_speed", "esriFieldTypeDouble"],
                ["median_speed", "esriFieldTypeDouble"],
                ["occupancy", "esriFieldTypeDouble"],
                ["trafficIntensity", "esriFieldTypeDouble"]
            ]
        },

        "lux" : {
            "serviceName": "Sensores tipo LUX de Santander",
            "route": "/santander_lux",
            "fields": [
                ["TimeInstant", "esriFieldTypeDate"],
                ["id", "esriFieldTypeString"],
                ["Latitud", "esriFieldTypeDouble"],
                ["Longitud", "esriFieldTypeDouble"],
                ["temperature", "esriFieldTypeDouble"],
                ["luminousFlux", "esriFieldTypeDouble"],
                ["batteryCharge", "esriFieldTypeDouble"],
                ["acceleration", "esriFieldTypeDouble"]
            ]
        },

        "sound" : {
            "serviceName": "Sensores tipo SOUND Santander",
            "route": "/santander_sound",
            "fields": [
                ["TimeInstant", "esriFieldTypeDate"],
                ["id", "esriFieldTypeString"],
                ["Latitud", "esriFieldTypeDouble"],
                ["Longitud", "esriFieldTypeDouble"],
                ["sound", "esriFieldTypeDouble"],
                ["batteryCharge", "esriFieldTypeDouble"]
            ]
        },

        "soundacc" : {
            "serviceName": "Sensores tipo SOUNDACC Santander",
            "route": "/santander_soundacc",
            "fields": [
                ["TimeInstant", "esriFieldTypeDate"],
                ["id", "esriFieldTypeString"],
                ["Latitud", "esriFieldTypeDouble"],
                ["Longitud", "esriFieldTypeDouble"],
                ["sound", "esriFieldTypeDouble"],
                ["batteryCharge", "esriFieldTypeDouble"],
                ["acceleration", "esriFieldTypeDouble"]
            ]
        },

        "bus" : {
            "serviceName": "Sensores tipo BUS Santander",
            "route": "/santander_bus",
            "fields": [
                ["TimeInstant", "esriFieldTypeDate"],
                ["TimeInstantModified", "esriFieldTypeDate"],
                ["id", "esriFieldTypeString"],
                ["index", "esriFieldTypeString"],
                ["line", "esriFieldTypeString"],
                ["position", "esriFieldTypeString"],
                ["service", "esriFieldTypeDouble"],
                ["speed", "esriFieldTypeDouble"],
                ["status", "esriFieldTypeDouble"],
                ["vehicle", "esriFieldTypeDouble"]
            ]
        }
    }
}
```

Veamos a continuación las variables contenidas en el fichero:

* **debug**: Si está establecida a *False* indicará que no queremos que imprima por consola las trazas del programa.
* **port**: indica el puerto en el que queremos que corra el servidor
* **username**: usuario nominal
* **password**: password del usuario nominal
* **root_url**: *www.arcgis.com* en caso de usar ArcGIS Online; o la url donde se encuentre tu *Portal* en caso de *ArcGIS Server*. Es necesario para las llamadas a [*generateToken*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#/Generate_Token/02r3000000m5000000/), [*isServiceNameAvailable*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#//02r300000076000000) y [*createService*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#//02r30000027r000000)
* **services_url**: url del servicio donde se alojan tus features services (en caso de ArcGIS Online es normalmente: *services1.arcgis.com*). Es necesario para la llamada a [*addFeatures*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#/Add_Features/02r30000010m000000/)
* **account_id**: Identificador a continuación de la *root_url*. Necesario para la llamada a [*isServiceNameAvailable*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#//02r300000076000000)
* **sensors**: es un objecto que incluye tantos objetos como sensores vayamos a recibir (dependerá de las suscripciones realizadas al context broker), por cada sensor deberemos indicar:
  * **serviceName**: nombre que queremos que tenga el servicio en el que alojaremos los datos
  * **route**: ruta en la que queremos que el servicio quede a la escucha de la información del sensor
  * **fields**: un array de arrays con el nombre de los atributos y el tipo del sensor que deseamos almacenar en la capa

En la siguiente imagen se muestran dónde se pueden encontrar los valores de **services_url** y **account_id** en ArcGIS Online:
<img src="/docs/fiware_agol_params.png" style="width:100%">
