# IonVision WebSocket API
The IonVision WebSocket API is a complimentary API to its the HTTP API. The WebSocket API is one-way
only. It sends real-time updates about the state of the device to users. The device does not accept
any input through the WebSocket API.

The HTTP API endpoints can be browsed at https://olfactomics.github.io/IonVision-API-docs/.

## Index
 - [scan.started](#scanstarted)
 - [scan.stopped](#scanstopped)
 - [scan.finished](#scanfinished)
 - [scan.resultsProcessed](#scanresultsprocessed)
 - [scan.progress](#scanprogress)
 - [scan.usernameChanged](#scanusernamechanged)
 - [scan.commentsChanged](#scancommentschanged)
 - [scope.started](#scopestarted)
 - [scope.stopped](#scopestopped)
 - [scope.data](#scopedata)
 - [scope.parametersChanged](#scopeparameterschanged)
 - [parameter.currentChanged](#parametercurrentchanged)
 - [parameter.setupCurrentStarted](#parametersetupcurrentstarted)
 - [parameter.setupCurrentFinished](#parametersetupcurrentfinished)
 - [parameter.preloadStarted](#parameterpreloadstarted)
 - [parameter.preloadFinished](#parameterpreloadfinished)
 - [parameter.edited](#parameteredited)
 - [parameter.listChanged](#parameterlistchanged)
 - [project.currentChanged](#projectcurrentchanged)
 - [project.setupCurrentStarted](#projectsetupcurrentstarted)
 - [project.setupCurrentFinished](#projectsetupcurrentfinished)
 - [project.edited](#projectedited)
 - [project.listChanged](#projectlistchanged)
 - [controllers.status](#controllersstatus)
 - [device.standbyButtonPressed](#devicestandbybuttonpressed)
 - [device.shutdown](#deviceshutdown)
 - [message.error](#messageerror)
 - [message.limitError](#messagelimiterror)
 - [message.timeChanged](#messagetimechanged)
 - [backup.started](#backupstarted)
 - [backup.progress](#backupprogress)
 - [backup.finished](#backupfinished)
 - [restore.started](#restorestarted)
 - [restore.progress](#restoreprogress)
 - [restore.finished](#restorefinished)
 - [reset.started](#resetstarted)
 - [reset.finished](#resetfinished)
 - [update.started](#updatestarted)
 - [update.progress](#updateprogress)
 - [update.finished](#updatefinished)

## IonVision WebSocket messages
This listing contains all WebSocket messages IonVision can send. All of the messages have the same
basic format, where they are JSON objects with `type`, `time` and `body` keys. `type` is always an
unique string used to identify the message. `time` is an Unix time timestamp in milliseconds of when
the message was sent from the server. The documentation below does not list the time key as it is
the same for each message type. `body` is an object that depends on the type. A type always has the
same keys in its body.

An example of a full message object from the WebSocket API:
```json
{
    "type": "parameter.setupCurrentStarted",
    "time": 1616057824108,
    "body": {}
}
```

Each list item contains the WebSocket message type as a title, the description of the message type, definition of body members and then an example message object.

### scan.started
A new scan has started.
```json
{
    "type": "scan.started",
    "body": {}
}
```

### scan.stopped
A scan has been stopped without finishing. No result data will be saved.
```json
{
    "type": "scan.stopped",
    "body": {}
}
```

### scan.finished
A scan has been finished successfully. The results of the scan are still being processed and are not
yet available.
```json
{
    "type": "scan.finished",
    "body": {}
}
```

### scan.resultsProcessed
The results of the previously finished scan have been processed to the device storage.
```json
{
    "type": "scan.resultsProcessed",
    "body": {}
}
```

### scan.progress
The progress of an ongoing scan.
 * `progress` {number} The progress of an ongoing scan as a percentage integer from 0 to 100.
 
```json
{
    "type": "scan.progress",
    "body": {
        "progress": 12
    }
}
```

### scan.usernameChanged
The local device username has changed.
 * `username` {string} The new username.

```json
{
    "type": "scan.usernameChanged",
    "body": {
        "username": "New username"
    }
}
```

### scan.commentsChanged
The comments object of an ongoing or next scan has changed. The new comments object is passed as the
body of this message.
 * `*` {*} The comments object in the body can contain any fields.

```json
{
    "type": "scan.commentsChanged",
    "body": {
        "These": "are the current comments."
    }
}
```

### scope.started
The device has moved to scope mode. Scope mode will now continue until stopped from the HTTP API.
```json
{
    "type": "scope.started",
    "body": {}
}
```

### scope.stopped
The scope mode has been stopped and device is idle again.
```json
{
    "type": "scope.stopped",
    "body": {}
}
```

### scope.data
A scope scan has finished. This message includes the scan results. A new scope scan will be started
automatically and scope mode will continue until manually stopped.
 * `usv` {number}
 * `ucv` {number[]}
 * `intensityTop` {number[]}
 * `intensityBottom` {number[]}
```json
{
    "type": "scope.data",
    "body": {
        "usv": 0,
        "ucv": [0],
        "intensityTop": [0],
        "intensityBottom": [0]
    }
}
```

### scope.parametersChanged
The scope parameters have changed. The new parameters are included in the message.
```json
{
    "type": "scope.parametersChanged",
    "body": {
        "ucvStart": 0,
        "ucvStop": 0,
        "usv": 0,
        "vb": 0,
        "pp": 0,
        "pw": 0,
        "sampleAverages": 0,
        "sampleFlowControl": 0,
        "circulatingGasFlowControl": 0
    }
}
```

### parameter.currentChanged
The current scan parameters preset has been changed.
 * `newParameter.id` {string} The id of the new parameter preset.
 * `newParameter.name` {string} The name of the new parameter preset.

```json
{
    "type": "parameter.currentChanged",
    "body": {
        "newParameter": {
            "id": "Parameter id",
            "name": "Parameter name"
        }
    }
}
```

### parameter.setupCurrentStarted
The current parameter preset is being set up on the RTM. A new current parameter preset or project
can't be set during this. The operation can take a few seconds.

```json
{
    "type": "parameter.setupCurrentStarted",
    "body": {}
}
```

### parameter.setupCurrentFinished
The current parameter preset has been set up to the RTM. The current parameter preset or project can
be changed again.

```json
{
    "type": "parameter.setupCurrentFinished",
    "body": {}
}
```

### parameter.preloadStarted
The current parameter preset is being preloaded. A new current parameter preset or project can't be
set during this. The operation takes a second.

```json
{
    "type": "parameter.preloadStarted",
    "body": {}
}
```

### parameter.preloadFinished
The current parameter preset has been preloaded. The current parameter preset or project can be
changed again.

```json
{
    "type": "parameter.preloadFinished",
    "body": {}
}
```

### parameter.edited
A parameter preset on the device has been edited. The edited version can be fetched through the HTTP
API.
 * `id` {string} The id of the parameter preset that was edited.
 * `name` {string} The name (new if changed) of the parameter preset that was edited.

```json
{
    "type": "parameter.edited",
    "body": {
        "id": "Parameter id",
        "name": "Parameter name"
    }
}
```

### parameter.listChanged
The list of parameters available on the device has changed. The new list must be fetched through the
HTTP API as it can be long.
```json
{
    "type": "parameter.listChanged",
    "body": {}
}
```

### project.currentChanged
The current project has been changed.
 * `newProject` {string} The name of the new current project.

```json
{
    "type": "project.currentChanged",
    "body": {
        "newProject": "Project name"
    }
}
```

### project.setupCurrentStarted
The current project is being set up on the RTM. A new current project or parameter preset can't be
set during this. The operation can take up to few minutes depending on the size of the project.

```json
{
    "type": "project.setupCurrentStarted",
    "body": {}
}
```

### project.setupCurrentFinished
The current project has been set up to the RTM. The current project or parameter preset can be
changed again.

```json
{
    "type": "project.setupCurrentFinished",
    "body": {}
}
```

### project.edited
A project on the device has been edited. The edited, new version can be fetched through the HTTP API.
 * `oldProject` {string} The name of the project that was edited.
 * `newProject` {string} The new name for the project that was edited. Can be the same as the old one if it wasn't changed.

```json
{
    "type": "project.edited",
    "body": {
        "oldProject": "Old project name",
        "newProject": "New project name"
    }
}
```

### project.listChanged
The list of projects available on the device has changed. The new list must be fetched through the
HTTP API as it can be long.
```json
{
    "type": "project.listChanged",
    "body": {}
}
```

### controllers.status
A message giving the current status of some hardware controllers in the system. This message is sent
automatically periodically.
 * `status.rtmReady` {boolean} The RTM is on.
 * `status.measurementRunning` {boolean} Measurement sequence is running.
 * `status.measurementReady` {boolean} Measurement sequence is completed.
 * `status.powerOn` {boolean} PSU 5V signal available.
 * `status.hvwmPowerOn` {boolean} HVWM has been powered and is on.
 * `status.demisecOn` {boolean} DEMISEC has been powered and is on.
 * `status.xappiPowerOn` {boolean} Ionisation module is powered.
 * `status.xappiOn` {boolean} Ionisation is on
 * `status.sampleHeaterOn` {boolean} Sample heater is used and powered.
 * `status.samplePumpOn` {boolean} Sample pump is running.
 * `status.sensorHeaterOn` {boolean} Sensor gas heater is used and powered.
 * `status.sensorPumpOn` {boolean} Sensor gas pump is running.
 * `sample.temperature` {number} The current temperature of sample gas.
 * `sample.heaterTemperature` {number} The current temperature of sample heater.
 * `sample.pressure` {number} The current pressure of sample gas.
 * `sample.flow` {number} The current gas flow of sample gas.
 * `sample.humidity` {number} The current humidity of sample gas.
 * `sensor.temperature` {number} The current gas temperature at sensor.
 * `sample.heaterTemperature` {number} The current temperature of sensor heater.
 * `sensor.pressure` {number} The current gas pressure at sensor.
 * `sensor.flow` {number} The current gas flow at sensor.
 * `sensor.humidity` {number} The current gas humidity at sensor.
 * `ambient.temperature` {number} The current ambient temperature.
 * `ambient.pressure` {number} The current ambient pressure.
 * `ambient.humidity` {number} The current ambient humidity.


```json
{
    "type": "controllers.status",
    "body": {
        "status": {
            "rtmReady": true,
            "measurementRunning": true,
            "measurementReady": true,
            "powerOn": true,
            "hvwmPowerOn": true,
            "demisecOn": true,
            "xappiPowerOn": true,
            "xappiOn": true,
            "sampleHeaterOn": true,
            "samplePumpOn": true,
            "sensorHeaterOn": true,
            "sensorPumpOn": true
        },
        "sample": {
            "temperature": 0,
            "heaterTemperature": 0,
            "pressure": 0,
            "flow": 0,
            "humidity": 0
        },
        "sensor": {
            "temperature": 0,
            "heaterTemperature": 0,
            "pressure": 0,
            "flow": 0,
            "humidity": 0
        },
        "ambient": {
            "temperature": 0,
            "pressure": 0,
            "humidity": 0
        }
    }
}
```

### device.standbyButtonPressed
The standby button at the front panel of the device has been pressed shortly. This is mainly used to
show a "Do you want to power off the device?" dialog in the user interface.
```json
{
    "type": "device.standbyButtonPressed",
    "body": {}
}
```

### device.shutdown
The device is powering off once this message is received. The device APIs will no be usable shortly
after this message.
```json
{
    "type": "device.shutdown",
    "body": {}
}
```

### message.error
A error or warning message from the back-end.
* `code` {string} An unique error code describing what the error is.

```json
{
    "type": "message.error",
    "body": {
        "code": "E010001"
    }
}
```

### message.limitError
An user set or safety limit has been crossed. The message always contains every possible limit error
and whether they are off (false) or on (true).
* `*` {boolean} The state of a single value error.

```json
{
    "type": "message.limitError",
    "body": {
        "ambientPressureUnderMin": false,
        "ambientPressureOverMax": false,
        "ambientHumidityUnderMin": false,
        "ambientHumidityOverMax": false,
        "ambientTemperatureUnderMin": false,
        "ambientTemperatureOverMax": false,
        "fetTemperatureUnderMin": false,
        "fetTemperatureOverMax": false,
        "sampleFlowUnderMin": false,
        "sampleFlowOverMax": false,
        "sampleTemperatureUnderMin": false,
        "sampleTemperatureOverMax": false,
        "samplePressureUnderMin": false,
        "samplePressureOverMax": false,
        "sampleHumidityUnderMin": false,
        "sampleHumidityOverMax": false,
        "circulatingFlowUnderMin": false,
        "circulatingFlowOverMax": false,
        "circulatingTemperatureUnderMin": false,
        "circulatingTemperatureOverMax": false,
        "circulatingPressureUnderMin": false,
        "circulatingPressureOverMax": false,
        "circulatingHumidityUnderMin": false,
        "circulatingHumidityOverMax": false,
        "sampleHeaterTemperatureUnderMin": false,
        "sampleHeaterTemperatureOverMax": false,
        "circulatingHeaterTemperatureUnderMin": false,
        "circulatingHeaterTemperatureOverMax": false,
        "ambientTemperatureOverSafety": false,
        "ambientTemperatureOverDanger": false,
        "fetTemperatureOverSafety": false,
        "fetTemperatureOverDanger": false,
        "circulatingHeaterTemperatureOverSafety": false,
        "circulatingHeaterTemperatureOverDanger": false,
        "sampleHeaterTemperatureOverSafety": false,
        "sampleHeaterTemperatureOverDanger": false,
        "sampleTemperatureOverSafety": false,
        "sampleTemperatureOverDanger": false,
        "circulatingTemperatureOverSafety": false,
        "circulatingTemperatureOverDanger": false
    }
}
```

### message.timeChanged
The system time of the device has changed. This also affects the WebSocket message timestamps. The
message timestamp and the time listed in the body are the same.
* `time` {number} The new current system time as an Unix timestamp in milliseconds.

```json
{
    "type": "message.timeChanged",
    "body": {
        "time": 123456
    }
}
```

### backup.started
A process to back up the device has started. Some device features like scanning are not available
during this.
```json
{
    "type": "backup.started",
    "body": {}
}
```

### backup.progress
A progress update for an ongoing backup process.
* `progress` {number} The progress of the backup process as a percentage number.

```json
{
    "type": "backup.progress",
    "body": {
        "progress": 20
    }
}
```

### backup.finished
A process to back up the device has finished successfully.
```json
{
    "type": "backup.finished",
    "body": {}
}
```


### restore.started
A process to restore the settings and storage of the device from a backup has started. Some device
features like scanning are not available during this.
```json
{
    "type": "restore.started",
    "body": {}
}
```

### restore.progress
A progress update for an ongoing system restore process.
* `progress` {number} The progress of the system restore process as a percentage number.

```json
{
    "type": "restore.progress",
    "body": {
        "progress": 20
    }
}
```

### restore.finished
A process to restore the device has finished successfully.
```json
{
    "type": "restore.finished",
    "body": {}
}
```

### reset.started
A process to reset the storage of the device has started. Some device features like scanning are not
available during this.
```json
{
    "type": "reset.started",
    "body": {}
}
```

### reset.finished
A process to reset the storage of the device has finished successfully.
```json
{
    "type": "reset.finished",
    "body": {}
}
```

### update.started
A process to update the system software of the device has started. Some device features like
scanning are not available during this.
```json
{
    "type": "update.started",
    "body": {}
}
```

### update.progress
A progress update for an ongoing system software update process.
* `progress` {number} The progress of the system software update process as a percentage number.

```json
{
    "type": "update.progress",
    "body": {
        "progress": 20
    }
}
```

### update.finished
A process to update the system software of the device has finished successfully.
```json
{
    "type": "update.finished",
    "body": {}
}
```

### cloud.sessionStart
A new Olfactomics cloud session has been established. All cloud functionality should be enabled after this message.
```json
{
    "type": "cloud.sessionStart",
    "body": {}
}
```

### cloud.sessionEnd
The active Olfactomics cloud session has ended. The cloud functionality of the device will be disabled after this until a new session is started.
```json
{
    "type": "cloud.sessionEnd",
    "body": {}
}
```

### cloud.connectionError
There has been an error in the Olfactomics cloud connection. Cloud functionality might not work until a cloud.connectionRestored is sent.
```json
{
    "type": "cloud.connectionError",
    "body": {}
}
```

### cloud.connectionRestored
Connection to Olfactomics cloud has been restored after an error. 
```json
{
    "type": "cloud.connectionRestored",
    "body": {}
}
```
