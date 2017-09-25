---
title: Simulate a device with the Azure IoT Gateway SDK (Windows) | Microsoft Docs
description: How to use the Azure IoT Gateway SDK on Windows to create a simulated device that sends telemetry through a gateway to an IoT hub.
services: iot-hub
documentationcenter: ''
author: chipalost
manager: timlt
editor: ''

ms.assetid: 6a2aeda0-696a-4732-90e1-595d2e2fadc6
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2016
ms.author: andbuc

---
# Use the Azure IoT Gateway SDK to send device-to-cloud messages with a simulated device (Windows)
[!INCLUDE [iot-hub-gateway-sdk-simulated-selector](../../includes/iot-hub-gateway-sdk-simulated-selector.md)]

## Build and run the sample
Before you get started, you must:

* [Set up your development environment][lnk-setupdevbox] for working with the SDK on Windows.
* [Create an IoT hub][lnk-create-hub] in your Azure subscription, you will need the name of your hub to complete this walkthrough. If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.
* Add two devices to your IoT hub and make a note of their ids and device keys. You can use the [device explorer or iothub-explorer][lnk-explorer-tools] tool to add your devices to the IoT hub you created in the previous step and retrieve their keys.

To build the sample:

1. Open a **Developer Command Prompt for VS2015** command prompt.
2. Navigate to the root folder in your local copy of the **azure-iot-gateway-sdk** repository.
3. Run the **tools\\build.cmd** script. This script creates a Visual Studio solution file and builds 
the solution. You can find the Visual Studio solution in the **build** folder in your local copy of 
the **azure-iot-gateway-sdk** repository. Additional parameters can be given to the script to build 
and run unit and end to end tests. These paramaters are **--run-unittests** and **--run-e2e-tests**
respectively.

To run the sample:

In a text editor, open the file **samples\\simulated_device_cloud_upload\\src\\simulated_device_cloud_upload_win.json** in your local copy of the **azure-iot-gateway-sdk** repository. This file configures the modules in the sample gateway:

* The **IoTHub** module connects to your IoT hub. You must configure it to send data to your IoT hub. Specifically, set the **IoTHubName** value to the name of your IoT hub and set the **IoTHubSuffix** value to **azure-devices.net**. Set the **Transport** value to one of: "HTTP", "AMQP", or "MQTT". Note that currently, only "HTTP" will share one TCP connection for all device messages. If you set the value to "AMQP", or "MQTT", the gateway will maintain a separate TCP connection to IoT Hub for each device.
* The **mapping** module maps the MAC addresses of your simulated devices to your IoT Hub device ids. Make sure that **deviceId** values match the ids of the two devices you added to your IoT hub, and that the **deviceKey** values contain the keys of your two devices.
* The **BLE1** and **BLE2** modules are the simulated devices. Note how their MAC addresses match those in the **mapping** module.
* The **Logger** module logs your gateway activity to a file.
* The **module path** values shown below assume that you cloned the IoT Gateway SDK repository to the root of your **C:** drive. If you downloaded it to another location, you need to adjust the **module path** values accordingly.
* The **links** array at the bottom of the JSON file connects the **BLE1** and **BLE2** modules to the **mapping** module, and the **mapping** module to the **IoTHub** module. It also ensures that all messages are logged by the **Logger** module.

```
{
    "modules" :
    [
      {
        "name": "IotHub",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\iothub\\Debug\\iothub.dll"
          }
          },
          "args": {
            "IoTHubName": "<<insert here IoTHubName>>",
            "IoTHubSuffix": "<<insert here IoTHubSuffix>>",
            "Transport": "HTTP"
          }
        },
      {
        "name": "mapping",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\identitymap\\Debug\\identity_map.dll"
          }
          },
          "args": [
            {
              "macAddress": "01:01:01:01:01:01",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            },
            {
              "macAddress": "02:02:02:02:02:02",
              "deviceId": "<<insert here deviceId>>",
              "deviceKey": "<<insert here deviceKey>>"
            }
          ]
        },
      {
        "name": "BLE1",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "01:01:01:01:01:01"
          }
        },
      {
        "name": "BLE2",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\simulated_device\\Debug\\simulated_device.dll"
          }
          },
          "args": {
            "macAddress": "02:02:02:02:02:02"
          }
        },
      {
        "name": "Logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "..\\..\\..\\modules\\logger\\Debug\\logger.dll"
          }
        },
        "args": {
          "filename": "deviceCloudUploadGatewaylog.log"
        }
      }
    ],
    "links" : [
        { "source" : "*", "sink" : "Logger" },
        { "source" : "BLE1", "sink" : "mapping" },
        { "source" : "BLE2", "sink" : "mapping" },
        { "source" : "mapping", "sink" : "IotHub" }
    ]
}
```

Save any changes you made to the configuration file.

To run the sample:

1. At a command prompt, navigate to the root folder of your local copy of the **azure-iot-gateway-sdk** repository.
2. Run the following command:
   
    ```
    build\samples\simulated_device_cloud_upload\Debug\simulated_device_cloud_upload_sample.exe samples\simulated_device_cloud_upload\src\simulated_device_cloud_upload_win.json
    ```
3. You can use the [device explorer or iothub-explorer][lnk-explorer-tools] tool to monitor the messages that IoT hub receives from the gateway.

## Next steps
If you want to gain a more advanced understanding of the IoT Gateway SDK and experiment with some code examples, visit the following developer tutorials and resources:

* [Send device-to-cloud messages from a physical device with the IoT Gateway SDK][lnk-physical-device]
* [Azure IoT Gateway SDK][lnk-gateway-sdk]

To further explore the capabilities of IoT Hub, see:

* [IoT Hub developer guide][lnk-devguide]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
[lnk-free-trial]: https://azure.microsoft.com/pricing/free-trial/
[lnk-explorer-tools]: https://github.com/Azure/azure-iot-sdks/blob/master/doc/manage_iot_hub.md
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/

[lnk-physical-device]: iot-hub-gateway-sdk-physical-device.md

[lnk-devguide]: ./iot-hub-devguide.md
[lnk-create-hub]: iot-hub-create-through-portal.md 
