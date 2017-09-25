---
title: Understand the Azure IoT SDKs | Microsoft Docs
description: Developer guide - information about and links to the various Azure IoT device and service SDKs that you can use to build device apps and back-end apps.
services: iot-hub
documentationcenter: ''
author: dominicbetts
manager: timlt
editor: ''

ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/04/2017
ms.author: dobett

---
# Azure IoT SDKs
## Azure IoT device SDK
The Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect to and are managed by Azure IoT Hub services.

The following Azure IoT device SDKs are available to download from GitHub:

* [Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.
* [Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]
* [Azure IoT device SDK for Java][lnk-java-device-sdk]
* [Azure IoT device SDK for Node.js][lnk-node-device-sdk]
* [Azure IoT device SDK for Python][lnk-python-device-sdk]

> [!NOTE]
> See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.
> 
> 

## OS platform and hardware compatibility
For more information about SDK compatibility with specific hardware devices, see the [Azure Certified for IoT device catalog][lnk-certified].

## Azure IoT service SDK
The Azure IoT service SDKs contain code to facilitate building applications that interact directly with IoT Hub to manage devices and security.

The following Azure IoT service SDKs are available to download from GitHub:

* [Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]
* [Azure IoT service SDK for Node.js][lnk-node-service-sdk]
* [Azure IoT service SDK for Java][lnk-java-service-sdk]
* [Azure IoT service SDK for Python][lnk-python-service-sdk]


> [!NOTE]
> See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.
> 
> 

## Azure IoT Gateway SDK
This Azure IoT Gateway SDK contains the infrastructure and modules to create IoT gateway solutions. You can extend the SDK to create gateways tailored to any end-to-end scenario.

You can download the [Azure IoT Gateway SDK][lnk-gateway-sdk] from GitHub.

## Online API reference documentation
The following list contains links to online API reference documentation for Azure IoT device, service, and gateway libraries:

* [Internet of Things (IoT) .NET][lnk-dotnet-ref]
* [IoT Hub REST][lnk-rest-ref]
* [Azure IoT device SDK for C][lnk-c-ref]
* [Azure IoT device SDK for Java][lnk-java-ref]
* [Azure IoT service SDK for Java][lnk-java-service-ref]
* [Azure IoT device SDK for Node.js][lnk-node-ref]
* [Azure IoT service SDK for Node.js][lnk-node-service-ref]
* [Azure IoT gateway SDK][lnk-gateway-ref]

## Next steps
Other reference topics in this IoT Hub developer guide include:

* [IoT Hub endpoints][lnk-devguide-endpoints]
* [IoT Hub query language for device twins and jobs][lnk-devguide-query]
* [Quotas and throttling][lnk-devguide-quotas]
* [IoT Hub MQTT support][lnk-devguide-mqtt]

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: http://azure.github.io/azure-iot-sdks/c/api_reference/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iot-device/1.0.15/index.html
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: http://azure.github.io/azure-iot-sdks/node/api_reference/azure-iothub/1.0.17/index.html
[lnk-gateway-ref]: http://azure.github.io/azure-iot-gateway-sdk/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
