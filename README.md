# Jim's Device MQTT Go
Using to drive demo where value sent to DataTopic outside of range of 0-100 triggers the SNMP Red Patlite.  Use CloudMQTT WebSocket UI (user:   jpwhite_tx@yahoo.com) to send
 {"cmd":"temp","method":"get","name":"MQTT test device","temp":temp value}
to the DataTopic


# Device MQTT Go
MQTT device service go version. The design is base on [ document](https://github.com/edgexfoundry/edgex-go/blob/master/docs/examples/Ch-ExamplesMQTTDeviceService.rst) .

## Requisite
* core-data
* core-metadata
* core-command

## Predefined configuration

### Incoming data listener and command response listener
Modify `configuration-driver.toml` file which under `./cmd/res` folder
```toml
[Incoming]
Protocol = "tcp"
Host = "m12.cloudmqtt.com"
Port = 17217
Username = "tobeprovided"
Password = "tobeprovided"
Qos = 0
KeepAlive = 3600
MqttClientId = "IncomingDataSubscriber"
Topic = "DataTopic"

[Response]
Protocol = "tcp"
Host = "m12.cloudmqtt.com"
Port = 17217
Username = "tobeprovided"
Password = "tobeprovided"
Qos = 0
KeepAlive = 3600
MqttClientId = "CommandResponseSubscriber"
Topic = "ResponseTopic"
```

### Device list
Define devices info for device-sdk to auto upload device profile and create device instance. Please modify `configuration.toml` file which under `./cmd/res` folder
```toml
[[DeviceList]]
  Name = "MQTT test device"
  Profile = "Test.Device.MQTT.Profile"
  Description = "MQTT device is created for test purpose"
  Labels = [ "MQTT", "test"]
  [DeviceList.Addressable]
    name = "Gateway address"
    Protocol = "TCP"
    Address = "m12.cloudmqtt.com"
    Port = 17217
    Publisher = "CommandPublisher"
    user = "tobeprovided"
    password = "tobeprovided"
    topic = "CommandTopic"

```

## Installation and Execution
```bash
make prepare
make build
make run
```

## build image
```bash
docker build -t edgexfoundry/docker-device-mqtt-go:0.1.0 .
```