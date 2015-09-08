[README en Español](README_ES.md)
This tool allows you to add sensor information received from a Context Broker into an ArcGIS Online or ArcGIS Server account.

# Fiware-ArcGIS

## How to
To install Fiware-ArcGIS you just need to execute the following commands:

``` 
$ git clone git@github.com:esri-es/fiware_arcgis.git
$ cd fiware_arcgis
$ npm install
$ node fiware_arcgis.js 
```

Con esta orden lanzamos el servidor y queda a la espera de recibir peticiones del Context Broker para procesar la información y añadirla a ArcGIS Online/Server.
With these commands we launched the server and waits until we receive any requests from the Context Broker to process the information and add it to ArcGIS Online / Server.

## Configuration file

Let's review the configuration file (*config.json*):
```javascript
{
    "debug": false,
    "port": 4003,
    "username": "<your user>",
    "password": "<your password>",
    "root_url": "www.arcgis.com",
    "services_url": "services1.arcgis.com",
    "account_id": "<your account id>",
    "sensors":{

        "traffic" : {
            "serviceName": "Traffic sensors",
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
            "serviceName": "Lux sensors",
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
            "serviceName": "Sound sensors",
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
            "serviceName": "Soundacc sensors",
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
            "serviceName": "Bus sensors",
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

Let's see the configurations variables:

* **debug**: *True/False* if we want to activate/desactivate the verbose mode.
* **port**: Listening port
* **username**: nominal user
* **password**: nominal user's password
* **root_url**: *www.arcgis.com* if we want to user ArcGIS Online; or the url where the *Portal for ArcGIS* is located in case you want to use *ArcGIS Server*. It's neccesary to call the [*generateToken*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#/Generate_Token/02r3000000m5000000/), [*isServiceNameAvailable*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#//02r300000076000000) and [*createService*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#//02r30000027r000000) methods
* **services_url**: url where the app is going to hostyour features services (en caso de ArcGIS Online es normalmente: *services1.arcgis.com*). It's neccesary to call the [*addFeatures*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#/Add_Features/02r30000010m000000/) method.
* **account_id**: Identifier after the *root_url*. Neccesary to call the [*isServiceNameAvailable*](http://resources.arcgis.com/en/help/arcgis-rest-api/index.html#//02r300000076000000) method.
* **sensors**: It's a JSON object that includes as many sensor descriptions as you need (normally you will add one for each Context Broker suscription), for each sensor we must specify:
  * **serviceName**: name we want to use to host the data
  * **route**: path where the app is going to be listening
  * **fields**: it's an array of arrays with two elements: the attribute name and the field type

The next image shows where you will find the **services_url** and **account_id** values in ArcGIS Online:
<img src="/docs/fiware_agol_params.png" style="width:100%">
