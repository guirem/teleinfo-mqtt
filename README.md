teleinfo-mqtt
  [![Build Status](https://travis-ci.org/fmartinou/teleinfo-mqtt.svg?branch=master)](https://travis-ci.org/fmartinou/teleinfo-mqtt)
  [![Maintainability](https://api.codeclimate.com/v1/badges/68abc62f1bd8a748273b/maintainability)](https://codeclimate.com/github/fmartinou/teleinfo-mqtt/maintainability)
  [![Test Coverage](https://api.codeclimate.com/v1/badges/68abc62f1bd8a748273b/test_coverage)](https://codeclimate.com/github/fmartinou/teleinfo-mqtt/test_coverage)
===========================================

Read teleinfo from serial port and publish with mqtt.

## Usage
Run the official Docker image (amd64 & arm64).

### Run
```
docker run -d --name teleinfo-mqtt          \
    --device=/dev/ttyUSB0:/dev/ttyUSB0      \ # add serial port device from host
    - e MQTT_URL=mqtt://my_mqtt_broker:1883 \ # set mqtt broker url
    fmartinou/teleinfo-mqtt:latest-amd64
```

### Configure
Configuration uses environment variables.

| Env var       | Description                    | Default value          |
|---------------|--------------------------------|------------------------|
|LOG_LEVEL      | Log level (INFO, DEBUG, ERROR) | INFO                   |
|SERIAL         | Serial Port location           | /dev/ttyUSB0           |
|MQTT_URL       | MQTT Broker connection URL     | mqtt://localhost:1883  |
|MQTT_TOPIC     | MQTT topic                     | /teleinfo              |
|MQTT_USER      | MQTT user     (optional)       |                        |
|MQTT_PASSWORD  | MQTT password (optional)       |                        |

### MQTT messages
MQTT JSON messages may vary upon your Electricity meter.  
Values are sanitized and converted to Numbers if possible (you can still access the original `raw` value if necessary).

Example
```json
{
    "ADCO": {
        "raw": "12345678901",
        "value": 12345678901
    },
    "OPTARIF": {
        "raw": "HC..",
        "value": "HC"
    },
    "ISOUSC": {
        "raw": "45",
        "value": 45
    },
    "HCHC": {
        "raw": "9058688",
        "value": 9058688
    },
    "HCHP": {
        "raw": "9752846",
        "value": 9752846
    },
    "PTEC": {
        "raw": "HP..",
        "value": "HP"
    },
    "IINST": {
        "raw": "22",
        "value": 22
    },
    "IMAX": {
        "raw": "90",
        "value": 90
    },
    "PAPP": {
        "raw": "4110",
        "value": 4110
    },
    "PAPP": {
        "raw": "A",
        "value": "A"
    }
}
```