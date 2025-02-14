#############################################

# Copyright 2020 CC-BY-SA author : MUG Lyon

#############################################

## SmartFactory - IoT Edge Modules

IoT edge modules should be run on your IoT edge device. And this device should be connected to your OPC-UA server.
Like [Production_simulator](https://github.com/muglyon/SmartFactory/blob/master/Production_simulator "Production_simulator")

![Smart-F-Educational-platform.png](https://github.com/muglyon/SmartFactory/blob/master/Resources/iotedgeopensource.png?raw=true)


### Modules list
 1. OPCPublisher : Microsoft module. It role is to listen several OPC UA servers and to send every modification with the iot edge message system. The list of servers and nodes to listen should be referenced in the file /iiotedge/pn.json. [See more...](https://docs.microsoft.com/en-us/azure/iot-accelerators/howto-opc-publisher-configure "See more...")

 2. DataParser : It role is to catch all messages from OPCPublisher and format them to create a valid JSON object. It use the node DisplayName to create the JSON structure [See more...](https://github.com/muglyon/SmartFactory/tree/master/IotEdgeModules/modules/DataParser "See more...")

 3. MongoLocal : Official MongoDB docker image. Used to locally store the messages. [See more...](https://hub.docker.com/_/mongo "See more...")

 4. MongoModule : Store the JSON objects sent by the DataParser in the local mongoDB. [See more...](https://github.com/muglyon/SmartFactory/tree/master/IotEdgeModules/modules/MongoModule "See more...")

 5. DataProcessing : It role is to process the JSON message. Our version use a basic rule framework to create several alert tagged messages according to the values of the message. [See more...](https://github.com/muglyon/SmartFactory/tree/master/IotEdgeModules/modules/DataProcessing "See more...")

 6. SmartFactoryDataUpdater : It role is to send modifications to your OPC UA server. It can monitor opc-ua nodes and modify opc-ua nodes. [See more...](https://github.com/muglyon/SmartFactory/tree/master/IotEdgeModules/modules/SmartFactoryDataUpdater "See more...")

 7. OPCAdmin : This module uses Direct Methods to connect to an OPC UA server and get the nodes id and childs. Usefull to programmatically configure the OPCPublisher [See more...](https://github.com/muglyon/SmartFactory/tree/master/IotEdgeModules/modules/OPCAdmin "See more...")

### Getting Started

#### Prerequisites
We supposed you are familiar with IoT Edge modules. 
__If it's not your case we recommend you to follow [this tutorial](https://docs.microsoft.com/en-us/azure/iot-edge/tutorial-machine-learning-edge-06-custom-modules "this tutorial").__

You should have [Visual Studio Code](https://code.visualstudio.com/ "Visual Studio Code") installed.

With theses extensions :
 - [Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge "Azure IoT Edge")
 - [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit "Azure IoT Toolkit")

You should have [Docker](https://www.docker.com/ "Docker") installed on your comuputer. It'll useful for build out IoT Edge modules images.

You should have a [Azure container registry](https://azure.microsoft.com/en-us/services/container-registry/ "Azure Container Registry") or any docker images server.
_If you use a Azure Container Registry you should connect your docker client to it with this [tutorial](https://docs.microsoft.com/en-us/azure/container-registry/container-registry-authentication "tutorial")_

You should have a IoT Hub with a IoT device connected. [How ?](https://docs.microsoft.com/en-us/azure/iot-hub/iot-hub-create-through-portal "How ?")

You should have a computer with the IoT Edge runtime installed and connected to your previous IoT Hub. This machine could be run with [Linux](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-linux "Linux") or [Windows](https://docs.microsoft.com/en-us/azure/iot-edge/how-to-install-iot-edge-windows "Windows")

For build and push you should create and fill the .env file at the same level than this readme.
```
CONTAINER_REGISTRY_ADDRESS=<Your docker container registry adress>
CONTAINER_REGISTRY_PASSWORD=<Your docker container registry password>
CONTAINER_REGISTRY_USERNAME=<Your docker container registry username>
OPCPUBLISHER_VERSION=2.3
```

#### Step one : Build images

- Go in every modules's folder and right click in the Visual Studio Code editor on their `module.json` then, select `Build and Push IoT Edge Module Image`.

#### Step two : Build the solution                    

- Right click on the deployment.template.json file in Visual Studio Code editor and select `Build and Push IoT Edge Solution`.
- Then you should see a new json file in your config folder.

#### Step three : Deploy your IoT Edge solution 

- Uncollapse the Azure IoT Hub panel in files tab of Visual Studio Code.
- Click on Set IoT Hub Connection String
- Paste your IoT Hub connection string
- Right click on the name of your IoT Edge device then, choose `Create deployment for Single Device`.
- Choose your `config/deployment.amd64.json` by default.

That it ! You have the SmartFactory stack installed on your IoT Edge Device. You should verify if the installation correctly suceed.


