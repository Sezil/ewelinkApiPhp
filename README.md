# ewelinkApiPhp

API connector for Sonoff/ewelink devices using simple webapi based on OAuth2 with Websockets features.

PHP 7.4+, **no other dependiencies required**.

## Current features

- log-in and authorization to ewelink APP via web or websockets
- get devices list with all or chosen parameters
- saving devices and other outputs from API to .json
- search for any value of parameter for each device (f.e switch status, productName, MAC etc.)
- set any parameter/state of device
- check if device has MultiChannel support
- set parameter for Multichannel devices
- check if device is Online
- debug all requests and responses to debug.log
- use Websocket connection to get and update parameters

## Public key and secret

Generate here: [dev.ewelink](https://dev.ewelink.cc/)

And put your keys and redirect Url in **Constants.php**

## Example

This is a single case example to turn on device, look at **index.php** in your root directory to get all possible methods.

```php
<?php

/*
Example Turn device ON
*/

require_once __DIR__ . '/autoloader.php';

//initialize core classes
$httpClient = new HttpClient();
$token = new Token($httpClient);

    if ($token->checkAndRefreshToken()) {
        // Initialize Devices class which will also initialize Home class and fetch family data
        $devices = new Devices($httpClient);

        // Device ID to be turned on
        $deviceId = '100xxxxxx'; // Replace with your actual device ID

        // Turn on the device
        $param = ['switch' => 'on'];
        $setStatusResult = $devices->setDeviceStatus($deviceId, $param);

    } else {

        //if we have no token, or we are not authorized paste link to authorization
        $loginUrl = $httpClient->getLoginUrl();
        echo '<a href="' . htmlspecialchars($loginUrl) . '">Authorize ewelinkApiPhp</a>';
    }

```

## Structure

``` rust
ewelinkApiPhp/
│
├── src/
│   ├── Constants.php
│   ├── Devices.php
│   ├── Home.php
│   ├── HttpClient.php
│   ├── Token.php
│   └── Utils.php
│   └── WebSocketsClient.php
│
├── autoloader.php
└── index.php
```

All classes are located in src directory.

Index.php works as a gateway to API and also as a debug for all availablemethods.

.json files outputs will be saved in project_root (ensure writeability)

## More inside info about current development

- main branch when commited used to be operative.
- enable DEBUG = 1; in Constants to log every get and postRequest with output and parameters to **debug.log**
- with next stable release methods and invoking functions structure can be changed (keep this in mind)
- branch **websockets** will probably be not maintened anymore
- there could be some incosistency in the code still, like f.e handling error messages or orphan methods
- index.php is a quasi-test file which helps me to check new methods and it is also a great example
