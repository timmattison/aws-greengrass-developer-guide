# What Is AWS IoT Greengrass?<a name="what-is-gg"></a>

AWS IoT Greengrass is software that extends cloud capabilities to local devices\. This enables devices to collect and analyze data closer to the source of information, react autonomously to local events, and communicate securely with each other on local networks\. AWS IoT Greengrass developers can use AWS Lambda functions and prebuilt [connectors](connectors.md) to create serverless applications that are deployed to devices for local execution\.

The following diagram shows the basic architecture of AWS IoT Greengrass\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/greengrass.png)

AWS IoT Greengrass makes it possible for customers to build IoT devices and application logic\. Specifically, AWS IoT Greengrass provides cloud\-based management of application logic that runs on devices\. Locally deployed Lambda functions and connectors are triggered by local events, messages from the cloud, or other sources\.

In AWS IoT Greengrass, devices securely communicate on a local network and exchange messages with each other without having to connect to the cloud\. AWS IoT Greengrass provides a local pub/sub message manager that can intelligently buffer messages if connectivity is lost so that inbound and outbound messages to the cloud are preserved\.

AWS IoT Greengrass protects user data:
+ Through the secure authentication and authorization of devices\.
+ Through secure connectivity in the local network\.
+ Between local devices and the cloud\.

Device security credentials function in a group until they are revoked, even if connectivity to the cloud is disrupted, so that the devices can continue to securely communicate locally\.

AWS IoT Greengrass provides secure, over\-the\-air software updates of Lambda functions\.

AWS IoT Greengrass consists of:
+ Software distributions
  + AWS IoT Greengrass core software
  + AWS IoT Greengrass core SDK
+ Cloud service
  + AWS IoT Greengrass API
+ Features
  + Lambda runtime
  + Shadows implementation
  + Message manager
  + Group management
  + Discovery service
  + Over\-the\-air update agent
  + Local resource access
  + Machine learning inference
  + Local secrets manager

## AWS IoT Greengrass Core Software<a name="gg-core"></a>

The AWS IoT Greengrass core software provides the following functionality:
+ Deployment and local execution of connectors and Lambda functions\.
+ Secure, encrypted storage of local secrets and controlled access by connectors and Lambda functions\.
+ MQTT messaging over the local network between devices, connectors, and Lambda functions using managed subscriptions\.
+ MQTT messaging between AWS IoT and devices, connectors, and Lambda functions using managed subscriptions\.
+ Secure connections between devices and the cloud using device authentication and authorization\.
+ Local shadow synchronization of devices\. Shadows can be configured to sync with the cloud\.
+ Controlled access to local device and volume resources\.
+ Deployment of cloud\-trained machine learning models for running local inference\.
+ Automatic IP detection that enables devices to discover the Greengrass core device\.
+ Central deployment of new or updated group configuration\. After the configuration data is downloaded, the core device is automatically restarted\.
+ Secure, over\-the\-air software updates of user\-defined Lambda functions\.

AWS IoT Greengrass core instances are configured through AWS IoT Greengrass APIs that create and update AWS IoT Greengrass group definitions stored in the cloud\.

### AWS IoT Greengrass Core Versions<a name="ggc-versions"></a>

The following tabs describe what's new and changed in AWS IoT Greengrass core software versions\.

------
#### [ GGC v1\.7\.0 ]

Current version\.

New features:
+ Greengrass connectors provide built\-in integration with local infrastructure, device protocols, AWS, and other cloud services\. For more information, see [Integrate with Services and Protocols Using Greengrass Connectors](connectors.md)\.
+ AWS IoT Greengrass extends AWS Secrets Manager to core devices, which makes your passwords, tokens, and other secrets available to connectors and Lambda functions\. Secrets are encrypted in transit and at rest\. For more information, see [Deploy Secrets to the AWS IoT Greengrass Core](secrets.md)\.
+ Support for a hardware root of trust security option\. For more information, see [Hardware Security Integration](hardware-security.md)\.
+ Isolation and permission settings that allow Lambda functions to run without Greengrass containers and to use the permissions of a specified user and group\. For more information, see [Controlling Execution of Greengrass Lambda Functions by Using Group\-Specific Configuration](lambda-group-config.md)\.
+ You can run AWS IoT Greengrass in a Docker container by configuring your Greengrass group to run with no containerization\. For more information, see [Running AWS IoT Greengrass in a Docker Container](run-gg-in-docker-container.md)\.
+ MQTT messaging on port 443 with Application Layer Protocol Negotiation \(ALPN\) or connection through a network proxy\. For more information, see [Connect on Port 443 or through a Network Proxy](gg-core.md#alpn-network-proxy)\.
+ The Amazon SageMaker Neo deep learning runtime, which supports machine learning models that have been optimized by the Amazon SageMaker Neo deep learning compiler\. For information about the Neo deep learning runtime, see [Runtimes and Precompiled Framework Libraries for ML Inference](ml-inference.md#precompiled-ml-libraries)\.
+ Support for Raspbian Stretch \(2018\-06\-27\) on Raspberry Pi core devices\.

Bug fixes and improvements:
+ General performance improvements and bug fixes\.

In addition, the following features are available with this release:
+ The AWS IoT Device Tester for AWS IoT Greengrass, which you can use to verify that your CPU architecture, kernel configuration, and drivers work with AWS IoT Greengrass\. For more information, see [ AWS IoT Device Tester for AWS IoT Greengrass User Guide](device-tester-for-greengrass-ug.md)\.
+ The AWS IoT Greengrass Core Software, AWS IoT Greengrass Core SDK, and AWS IoT Greengrass Machine Learning SDK packages are available for dowload through Amazon CloudFront\. For more information, see [AWS IoT Greengrass Downloads](#gg-downloads)\.

------
#### [ GGC v1\.6\.0 ]

New features:
+ Lambda executables that run binary code on the Greengrass core\. Use the new AWS IoT Greengrass Core SDK for C to write Lambda executables in C and C\+\+\. For more information, see [Lambda Executables](lambda-functions.md#lambda-executables)\.
+ Optional local storage message cache that can persist across restarts\. You can configure the storage settings for MQTT messages that are queued for processing\. For more information, see [MQTT Message Queue](gg-core.md#mqtt-message-queue)\.
+ Configurable maximum reconnect retry interval for when the core device is disconnected\. For more information, see the `mqttMaxConnectionRetryInterval` property in [AWS IoT Greengrass Core Configuration File](gg-core.md#config-json)\.
+ Local resource access to the host /proc directory\. For more information, see [Access Local Resources with Lambda Functions](access-local-resources.md)\.
+ Configurable write directory\. The AWS IoT Greengrass core software can be deployed to read\-only and read\-write locations\. For more information, see [Configure a Write Directory for AWS IoT Greengrass](gg-core.md#write-directory)\.

Bug fixes and improvements:
+ Performance improvement for publishing messages in the Greengrass core and between devices and the core\.
+ Reduced the compute resources required to process logs generated by user\-defined Lambda functions\.

------
#### [ GGC v1\.5\.0 ]

New features:
+ AWS IoT Greengrass Machine Learning \(ML\) Inference is generally available\. You can perform ML inference locally on AWS IoT Greengrass devices using models that are built and trained in the cloud\. For more information, see [Perform Machine Learning Inference](ml-inference.md)\.
+ Greengrass Lambda functions now support binary data as input payload, in addition to JSON\. To use this feature, you must upgrade to AWS IoT Greengrass Core SDK version 1\.1\.0, which you can download from the **Software** page in the AWS IoT console\.

Bug fixes and improvements:
+ Reduced the overall memory footprint\.
+ Performance improvements for sending messages to the cloud\.
+ Performance and stability improvements for the download agent, Device Certificate Manager, and OTA update agent\.
+ Minor bug fixes\.

------
#### [ GGC v1\.3\.0 ]

New features:
+ Over\-the\-air \(OTA\) update agent capable of handling cloud\-deployed, Greengrass update jobs\. The agent is found under the new `/greengrass/ota` directory\. For more information, see [OTA Updates of AWS IoT Greengrass Core Software](core-ota-update.md)\.
+ Local resource access feature allows Greengrass Lambda functions to access local resources, such as peripheral devices and volumes\. For more information, see [Access Local Resources with Lambda Functions](access-local-resources.md)\.

------
#### [ GGC v1\.1\.0 ]

New features:
+ Deployed AWS IoT Greengrass groups can be reset by deleting Lambda functions, subscriptions, and configurations\. For more information, see [Reset Deployments](reset-deployments-scenario.md)\.
+ Support for Node\.js 6\.10 and Java 8 Lambda runtimes, in addition to Python 2\.7\.

To migrate from the previous version of the AWS IoT Greengrass core:
+ Copy certificates from the `/greengrass/configuration/certs` folder to `/greengrass/certs`\.
+ Copy `/greengrass/configuration/config.json` to `/greengrass/config/config.json`\.
+ Run `/greengrass/ggc/core/greengrassd` instead of `/greengrass/greengrassd`\.
+ Deploy the group to the new core\.

------
#### [ GGC v1\.0\.0 ]

Initial version\.

------

## AWS IoT Greengrass Groups<a name="gg-group"></a>

An AWS IoT Greengrass group definition is a collection of settings for AWS IoT Greengrass core devices and the devices that communicate with them\. The following diagram shows the objects that make up an AWS IoT Greengrass group\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/gg-group.png)

In the preceding diagram:

A: AWS IoT Greengrass group definition  
A collection of information about the AWS IoT Greengrass group\.

B: AWS IoT Greengrass group settings  
These include:   
+ AWS IoT Greengrass group role\.
+ Log configuration\.
+ Certification authority and local connection configuration\.
+ AWS IoT Greengrass core connectivity information\.

C: AWS IoT Greengrass core  
The AWS IoT thing that represents the AWS IoT Greengrass core\.

D: Lambda function definition  
A list of Lambda functions that run locally on the core, with associated configuration data\. For more information, see [Run Lambda Functions on the AWS IoT Greengrass Core](lambda-functions.md)\.

E: Subscription definition  
A list of subscriptions that enable communication using MQTT messages\. A subscription defines:  
+ A message source\. This can be an AWS IoT thing \(device\), Lambda function, AWS IoT, or the local AWS IoT Greengrass shadow service\.
+ A subject, which is an MQTT topic or topic filter that's used to filter message data\.
+ A message target, which identifies the destination for messages published by the message source\. This can be an AWS IoT thing \(device\), Lambda function, AWS IoT, or the local AWS IoT Greengrass shadow service\.
For more information, see [Greengrass Messaging Workflow](gg-sec.md#gg-msg-workflow)\.

F: Connector definition  
A list of connectors that run locally on the core, with associated configuration data\. For more information, see [Integrate with Services and Protocols Using Greengrass Connectors](connectors.md)\.

G: Device definition  
A list containing an AWS IoT Greengrass core and AWS IoT things that are members of the AWS IoT Greengrass group, with associated configuration data\. This data specifies which devices are AWS IoT Greengrass cores and which devices should sync shadow data with AWS IoT\.

H: Resource definition  
A list of local resources, machine learning resources, and secret resources on the AWS IoT Greengrass core, with associated configuration data\. For more information, see [Access Local Resources with Lambda Functions](access-local-resources.md), [Perform Machine Learning Inference](ml-inference.md), and [Deploy Secrets to the AWS IoT Greengrass Core](secrets.md)\.

When deployed, the AWS IoT Greengrass group definition, Lambda functions, connectors, resources, and subscription table are copied to an AWS IoT Greengrass core device\.

## Devices in AWS IoT Greengrass<a name="devices"></a>

There are two types of devices:
+ AWS IoT Greengrass cores\.
+ AWS IoT devices connected to an AWS IoT Greengrass core\.

An AWS IoT Greengrass core is an AWS IoT device that runs specialized AWS IoT Greengrass software that communicates directly with the AWS IoT and AWS IoT Greengrass cloud services\. It is an AWS IoT device with its own certificate used for authenticating with AWS IoT\. It has a device shadow and exists in the AWS IoT device registry\. AWS IoT Greengrass cores run a local Lambda runtime, a deployment agent, and an IP address tracker that sends IP address information to the AWS IoT Greengrass cloud service to allow AWS IoT devices to automatically discover their group and core connection information\.

Any AWS IoT device can connect to an AWS IoT Greengrass core\. These devices run software written with the AWS IoT Device SDK\. 

The following table shows how these device types are related\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/images/devices.png)

The AWS IoT Greengrass core device stores certificates in two locations:
+ Core device certificate in /greengrass/certs \- The core device certificate is named `hash.cert.pem` \(for example, `86c84488a5.cert.pem`\)\. This certificate is used to authenticate the core when connecting to the AWS IoT and AWS IoT Greengrass services\.
+ MQTT core server certificate in /greengrass/ggc/var/state/server \- The MQTT core server certificate is named `server.crt`\. This certificate is used for mutual authentication between the local MQTT service \(that's on the Greengrass core\) and Greengrass devices before messages are exchanged\.

## SDKs<a name="gg-sdks"></a>

The following AWS\-provided SDKs are used to work with AWS IoT Greengrass:

------
#### [ GGC 1\.7\.0 ]

AWS SDK  
Use the AWS SDK to build applications that interact with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK in deployed Lambda functions to make direct calls to any AWS service\. For more information, see [AWS SDK](lambda-functions.md#lambda-sdks-aws)\.

AWS IoT Device SDK  
The AWS IoT Device SDK helps devices connect to AWS IoT or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the AWS IoT Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDK platforms to connect to an AWS IoT Greengrass core, only the C\+\+ and Python SDKs provide AWS IoT Greengrass\-specific functionality, such as access to the AWS IoT Greengrass Discovery Service and AWS IoT Greengrass core root CA downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot/sdk/)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the Greengrass core, publish messages to AWS IoT, interact with the local shadow service, invoke other deployed Lambda functions, and access secret resources\. This SDK is used by Lambda functions that run on an AWS IoT Greengrass core\. For more information, see [AWS IoT Greengrass Core SDK](lambda-functions.md#lambda-sdks-core)\.

AWS IoT Greengrass Machine Learning SDK  
The AWS IoT Greengrass Machine Learning SDK enables Lambda functions to consume machine learning models that are deployed to the Greengrass core as machine learning resources\. This SDK is used by Lambda functions that run on an AWS IoT Greengrass core and interact with a local inference service\. For more information, see [AWS IoT Greengrass Machine Learning SDK](lambda-functions.md#lambda-sdks-ml)\.

------
#### [ GGC 1\.6\.0 ]

AWS SDKs  
Using the AWS SDKs, you can build applications that work with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK in deployed Lambda functions to make direct calls to any AWS service\. For more information, see [SDKs for Greengrass Lambda Functions](lambda-functions.md#lambda-sdks)\.

AWS IoT Device SDKs  
The AWS IoT Device SDKs helps devices connect to AWS IoT or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the AWS IoT Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDKs to connect to an AWS IoT Greengrass core, only the C\+\+ and Python Device SDKs provide AWS IoT Greengrass\-specific functionality, such as access to the AWS IoT Greengrass Discovery Service and AWS IoT Greengrass core root CA downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot/sdk/)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the AWS IoT Greengrass core on which they run in order to publish messages, interact with the local Device Shadow service, or invoke other deployed Lambda functions\. This SDK is used exclusively for writing Lambda functions running in the Lambda runtime on an AWS IoT Greengrass core\. For more information, see [SDKs for Greengrass Lambda Functions](lambda-functions.md#lambda-sdks)\.

------
#### [ GGC v1\.5\.0 ]

AWS SDKs  
Using the AWS SDKs, you can build applications that work with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK inside deployed Lambda functions to make direct calls to any AWS service\.

AWS IoT Device SDKs  
The AWS IoT Device SDKs helps devices connect to AWS IoT or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the AWS IoT Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDKs to connect to an AWS IoT Greengrass core, only the C\+\+ and Python Device SDKs provide AWS IoT Greengrass\-specific functionality, such as access to the AWS IoT Greengrass Discovery Service and AWS IoT Greengrass core root CA downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot/sdk/)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the AWS IoT Greengrass core on which they run in order to publish messages, interact with the local Device Shadow service, or invoke other deployed Lambda functions\. This SDK is used exclusively for writing Lambda functions running in the Lambda runtime on an AWS IoT Greengrass core\. Lambda functions running on an AWS IoT Greengrass core can interact with AWS cloud services directly using the AWS SDK\. The AWS IoT Greengrass Core SDK and the AWS SDK are contained in different packages, so you can use both packages simultaneously\. You can download the AWS IoT Greengrass Core SDK from the **Software** page of the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.  
The AWS IoT Greengrass Core SDK follows the AWS SDK programming model\. It allows you to easily port Lambda functions developed for the cloud to Lambda functions that run on an AWS IoT Greengrass core\. For example, using the AWS SDK, the following Lambda function publishes a message to the topic `"/some/topic"` in the cloud:   

```
import boto3
        
client = boto3.client('iot-data')
response = client.publish(
	topic = "/some/topic",
	qos = 0,
	payload = "Some payload".encode()
)
```
To port this Lambda function for execution on an AWS IoT Greengrass core, replace the `import boto3` statement with the `import greengrasssdk`, as shown in the following snippet:  
The AWS IoT Greengrass Core SDK only supports sending MQTT messages with QoS = 0\.

```
import greengrasssdk
                        
client = greengrasssdk.client('iot-data')
response = client.publish(
    topic='/some/topic',
    qos=0,
    payload='some payload'.encode()
)
```
This allows you to test your Lambda functions in the cloud and migrate them to AWS IoT Greengrass with minimal effort\.

**Note**  
The AWS SDK is natively part of the environment of a Lambda function that runs in the AWS cloud\. If you want to use `boto3` in a Lambda function that's deployed to an AWS IoT Greengrass core, make sure to include the AWS SDK in your package\. In addition, if you use both the AWS IoT Greengrass Core SDK and the AWS SDK in the same package, make sure that your Lambda functions use the correct namespaces\. For more information about how to create your deployment package, see:  
[Creating a Deployment Package \(Python\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html)
[ Creating a Deployment Package \(Node\.js\)](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-create-deployment-pkg.html)
[ Creating a Deployment Package \(Java\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-java-how-to-create-deployment-package.html)

------
#### [ GGC v1\.3\.0 ]

AWS SDKs  
Using the AWS SDKs, you can build applications that work with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK inside deployed Lambda functions to make direct calls to any AWS service\.

AWS IoT Device SDKs  
The AWS IoT Device SDKs helps devices connect to AWS IoT or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the AWS IoT Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDKs to connect to an AWS IoT Greengrass core, only the C\+\+ and Python Device SDKs provide AWS IoT Greengrass\-specific functionality, such as access to the AWS IoT Greengrass Discovery Service and AWS IoT Greengrass core root CA downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot/sdk/)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the AWS IoT Greengrass core on which they run in order to publish messages, interact with the local Device Shadow service, or invoke other deployed Lambda functions\. This SDK is used exclusively for writing Lambda functions running in the Lambda runtime on an AWS IoT Greengrass core\. Lambda functions running on an AWS IoT Greengrass core can interact with AWS cloud services directly using the AWS SDK\. The AWS IoT Greengrass Core SDK and the AWS SDK are contained in different packages, so you can use both packages simultaneously\. You can download the AWS IoT Greengrass Core SDK from the **Software** page of the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.  
The AWS IoT Greengrass Core SDK follows the AWS SDK programming model\. It allows you to easily port Lambda functions developed for the cloud to Lambda functions that run on an AWS IoT Greengrass core\. For example, using the AWS SDK, the following Lambda function publishes a message to the topic `"/some/topic"` in the cloud:   

```
import boto3
        
client = boto3.client('iot-data')
response = client.publish(
	topic = "/some/topic",
	qos = 0,
	payload = "Some payload".encode()
)
```
To port this Lambda function for execution on an AWS IoT Greengrass core, replace the `import boto3` statement with the `import greengrasssdk`, as shown in the following snippet:  
The AWS IoT Greengrass Core SDK only supports sending MQTT messages with QoS = 0\.

```
import greengrasssdk
                        
client = greengrasssdk.client('iot-data')
response = client.publish(
    topic='/some/topic',
    qos=0,
    payload='some payload'.encode()
)
```
This allows you to test your Lambda functions in the cloud and migrate them to AWS IoT Greengrass with minimal effort\.

**Note**  
The AWS SDK is natively part of the environment of a Lambda function that runs in the AWS cloud\. If you want to use `boto3` in a Lambda function that's deployed to an AWS IoT Greengrass core, make sure to include the AWS SDK in your package\. In addition, if you use both the AWS IoT Greengrass Core SDK and the AWS SDK in the same package, make sure that your Lambda functions use the correct namespaces\. For more information about how to create your deployment package, see:  
[Creating a Deployment Package \(Python\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html)
[ Creating a Deployment Package \(Node\.js\)](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-create-deployment-pkg.html)
[ Creating a Deployment Package \(Java\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-java-how-to-create-deployment-package.html)

------
#### [ GGC v1\.1\.0 ]

AWS SDKs  
Using the AWS SDKs, you can build applications that work with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK inside deployed Lambda functions to make direct calls to any AWS service\.

AWS IoT Device SDKs  
The AWS IoT Device SDKs helps devices connect to AWS IoT or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the AWS IoT Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDKs to connect to an AWS IoT Greengrass core, only the C\+\+ and Python Device SDKs provide AWS IoT Greengrass\-specific functionality, such as access to the AWS IoT Greengrass Discovery Service and AWS IoT Greengrass core root CA downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot/sdk/)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the AWS IoT Greengrass core on which they run in order to publish messages, interact with the local Device Shadow service, or invoke other deployed Lambda functions\. This SDK is used exclusively for writing Lambda functions running in the Lambda runtime on an AWS IoT Greengrass core\. Lambda functions running on an AWS IoT Greengrass core can interact with AWS cloud services directly using the AWS SDK\. The AWS IoT Greengrass Core SDK and the AWS SDK are contained in different packages, so you can use both packages simultaneously\. You can download the AWS IoT Greengrass Core SDK from the **Software** page of the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.  
The AWS IoT Greengrass Core SDK follows the AWS SDK programming model\. It allows you to easily port Lambda functions developed for the cloud to Lambda functions that run on an AWS IoT Greengrass core\. For example, using the AWS SDK, the following Lambda function publishes a message to the topic `"/some/topic"` in the cloud:   

```
import boto3
        
client = boto3.client('iot-data')
response = client.publish(
	topic = "/some/topic",
	qos = 0,
	payload = "Some payload".encode()
)
```
To port this Lambda function for execution on an AWS IoT Greengrass core, replace the `import boto3` statement with the `import greengrasssdk`, as shown in the following snippet:  
The AWS IoT Greengrass Core SDK only supports sending MQTT messages with QoS = 0\.

```
import greengrasssdk
                        
client = greengrasssdk.client('iot-data')
response = client.publish(
    topic='/some/topic',
    qos=0,
    payload='some payload'.encode()
)
```
This allows you to test your Lambda functions in the cloud and migrate them to AWS IoT Greengrass with minimal effort\.

**Note**  
The AWS SDK is natively part of the environment of a Lambda function that runs in the AWS cloud\. If you want to use `boto3` in a Lambda function that's deployed to an AWS IoT Greengrass core, make sure to include the AWS SDK in your package\. In addition, if you use both the AWS IoT Greengrass Core SDK and the AWS SDK in the same package, make sure that your Lambda functions use the correct namespaces\. For more information about how to create your deployment package, see:  
[Creating a Deployment Package \(Python\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html)
[ Creating a Deployment Package \(Node\.js\)](https://docs.aws.amazon.com/lambda/latest/dg/nodejs-create-deployment-pkg.html)
[ Creating a Deployment Package \(Java\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-java-how-to-create-deployment-package.html)

------
#### [ GGC v1\.0\.0 ]

AWS SDKs  
Using the AWS SDKs, you can build applications that work with any AWS service, including Amazon S3, Amazon DynamoDB, AWS IoT, AWS IoT Greengrass, and more\. In the context of AWS IoT Greengrass, you can use the AWS SDK inside deployed Lambda functions to make direct calls to any AWS service\.

AWS IoT Device SDKs  
The AWS IoT Device SDKs helps devices connect to AWS IoT or AWS IoT Greengrass services\. Devices must know which AWS IoT Greengrass group they belong to and the IP address of the AWS IoT Greengrass core that they should connect to\.   
Although you can use any of the AWS IoT Device SDKs to connect to an AWS IoT Greengrass core, only the C\+\+ and Python Device SDKs provide AWS IoT Greengrass\-specific functionality, such as access to the AWS IoT Greengrass Discovery Service and AWS IoT Greengrass core root CA downloads\. For more information, see [AWS IoT Device SDK](https://aws.amazon.com/iot/sdk/)\.

AWS IoT Greengrass Core SDK  
The AWS IoT Greengrass Core SDK enables Lambda functions to interact with the AWS IoT Greengrass core on which they run in order to publish messages, interact with the local Device Shadow service, or invoke other deployed Lambda functions\. This SDK is used exclusively for writing Lambda functions running in the Lambda runtime on an AWS IoT Greengrass core\. Lambda functions running on an AWS IoT Greengrass core can interact with AWS cloud services directly using the AWS SDK\. The AWS IoT Greengrass Core SDK and the AWS SDK are contained in different packages, so you can use both packages simultaneously\. You can download the AWS IoT Greengrass Core SDK from the **Software** page of the [AWS IoT console](https://console.aws.amazon.com/iot/home)\.  
The AWS IoT Greengrass Core SDK follows the AWS SDK programming model\. It allows you to easily port Lambda functions developed for the cloud to Lambda functions that run on an AWS IoT Greengrass core\. For example, using the AWS SDK, the following Lambda function publishes a message to the topic `"/some/topic"` in the cloud:   

```
import boto3
        
client = boto3.client('iot-data')
response = client.publish(
	topic = "/some/topic",
	qos = 0,
	payload = "Some payload".encode()
)
```
To port this Lambda function for execution on an AWS IoT Greengrass core, replace the `import boto3` statement with the `import greengrasssdk`, as shown in the following snippet:  
The AWS IoT Greengrass Core SDK only supports sending MQTT messages with QoS = 0\.

```
import greengrasssdk
                        
client = greengrasssdk.client('iot-data')
response = client.publish(
    topic='/some/topic',
    qos=0,
    payload='some payload'.encode()
)
```
This allows you to test your Lambda functions in the cloud and migrate them to AWS IoT Greengrass with minimal effort\.

**Note**  
The AWS SDK is natively part of the environment of a Lambda function that runs in the AWS cloud\. If you want to use `boto3` in a Lambda function that's deployed to an AWS IoT Greengrass core, make sure to include the AWS SDK in your package\. In addition, if you use both the AWS IoT Greengrass Core SDK and the AWS SDK in the same package, make sure that your Lambda functions use the correct namespaces\. For more information about how to create your deployment package, see:  
[Creating a Deployment Package \(Python\)](https://docs.aws.amazon.com/lambda/latest/dg/lambda-python-how-to-create-deployment-package.html)

------

## Supported Platforms and Requirements<a name="gg-platforms"></a>

The AWS IoT Greengrass core software is supported on the platforms listed here, and requires a few dependencies\.

------
#### [ GGC v1\.7\.0 ]
+ Supported platforms:
  + Architecture: ARMv7l; OS: Linux; Distribution: [Raspbian Stretch, 2018\-06\-29](https://downloads.raspberrypi.org/raspbian/images/raspbian-2018-06-29/)\. While several versions may work with AWS IoT Greengrass, we recommend this as it is the officially supported distribution\.
  + Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\), Ubuntu 14\.04 – 16\.04
  + Architecture: ARMv8 \(AArch64\); OS: Linux; Distribution: Arch Linux

  You can download the core software from the **Software** page in the AWS IoT console or from the [AWS IoT Greengrass Core Software](#gg-core-download-tab) downloads\.
+ The following items are required:
  + Minimum 128 MB RAM allocated to the AWS IoT Greengrass core device\.
  + Linux kernel version 4\.4 or greater: while several versions may work with AWS IoT Greengrass, for optimal security and performance, we recommend version 4\.4 or greater\.
  + [Glibc library](https://www.gnu.org/software/libc/) version 2\.14 or greater\.
  + The `/var/run` directory must be present on the device\.
  + AWS IoT Greengrass requires hardlink and softlink protection to be enabled on the device\. Without this, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + The `ggc_user` and `ggc_group` user and group must be present on the device\.
  + The following Linux kernel configurations must be enabled on the device: 
    + Namespace: CONFIG\_IPC\_NS, CONFIG\_UTS\_NS, CONFIG\_USER\_NS, CONFIG\_PID\_NS
    + CGroups: CONFIG\_CGROUP\_DEVICE, CONFIG\_CGROUPS, CONFIG\_MEMCG
    + Others: CONFIG\_POSIX\_MQUEUE, CONFIG\_OVERLAY\_FS, CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER, CONFIG\_SECCOMP\_FILTER, CONFIG\_KEYS, CONFIG\_SECCOMP
  + `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` must be enabled\.
  + The Linux kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\.
  + The *memory* `cgroup` must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
  + The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items may be optional:
  + The *devices* `cgroup` must be enabled and mounted if Lambda functions with [Local Resource Access \(LRA\)](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
  + [Python](https://www.python.org/) version 2\.7 is required if Python Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [NodeJS](https://www.nodejs.org/) version 6\.10 or greater is required if Node\.JS Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [ Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or greater is required if Java Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + The following commands are required for [Greengrass OTA Agent](core-ota-update.md#ota-agent): `wget`, `realpath`, `tar`, `readlink`, `basename`, `dirname`, `pidof`, `df`, `grep`, and `umount`\.

------
#### [ GGC v1\.6\.0 ]
+ Supported platforms:
  + Architecture: ARMv7l; OS: Linux; Distribution: [Raspbian Jessie, 2017\-03\-02](https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-03-03/)
**Note**  
 You can choose to use **Raspbian Stretch** instead of Jessie\. Using Stretch will require additional memory set\-up\. For more information, see [this step](module1.md#stretch-step)\. 
  + Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\)
  + Architecture: x86\_64; OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04
  + Architecture: ARMv8 \(AArch64\); OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04 \(Annapurna Alpine V2\)
+ The following items are required:
  + Minimum 128 MB RAM allocated to the AWS IoT Greengrass core device\.
  + Linux kernel version 4\.4 or greater: while several versions may work with AWS IoT Greengrass, for optimal security and performance, we recommend version 4\.4 or greater\.
  + [Glibc library](https://www.gnu.org/software/libc/) version 2\.14 or greater\.
  + The `/var/run` directory must be present on the device\.
  + AWS IoT Greengrass requires hardlink and softlink protection to be enabled on the device\. Without this, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + The `ggc_user` and `ggc_group` user and group must be present on the device\.
  + The following Linux kernel configurations must be enabled on the device: 
    + Namespace: CONFIG\_IPC\_NS, CONFIG\_UTS\_NS, CONFIG\_USER\_NS, CONFIG\_PID\_NS
    + CGroups: CONFIG\_CGROUP\_DEVICE, CONFIG\_CGROUPS, CONFIG\_MEMCG
    + Others: CONFIG\_POSIX\_MQUEUE, CONFIG\_OVERLAY\_FS, CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER, CONFIG\_SECCOMP\_FILTER, CONFIG\_KEYS, CONFIG\_SECCOMP
  + `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` must be enabled\.
  + The Linux kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\.
  + The *memory* `cgroup` must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
  + The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items may be optional:
  + The *devices* `cgroup` must be enabled and mounted if Lambda functions with [Local Resource Access \(LRA\)](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
  + [Python](https://www.python.org/) version 2\.7 is required if Python Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [NodeJS](https://www.nodejs.org/) version 6\.10 or greater is required if Node\.JS Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [ Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or greater is required if Java Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [OpenSSL](https://www.openssl.org/) 1\.01 or greater is required for [Greengrass OTA Agent](core-ota-update.md#ota-agent) as well as the following commands: `wget`, `realpath`, `tar`, `readlink`, `basename`, `dirname`, `pidof`, `df`, `grep`, and `umount`\.

------
#### [ GGC v1\.5\.0 ]
+ Supported platforms:
  + Architecture: ARMv7l; OS: Linux; Distribution: [Raspbian Jessie, 2017\-03\-02](https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-03-03/)
**Note**  
 You can choose to use **Raspbian Stretch** instead of Jessie\. Using Stretch will require additional memory set\-up\. For more information, see [this step](module1.md#stretch-step)\. 
  + Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\)
  + Architecture: x86\_64; OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04
  + Architecture: ARMv8 \(AArch64\); OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04 \(Annapurna Alpine V2\)
+ The following items are required:
  + Minimum 128 MB RAM allocated to the AWS IoT Greengrass core device\.
  + Linux kernel version 4\.4 or greater: while several versions may work with AWS IoT Greengrass, for optimal security and performance, we recommend version 4\.4 or greater\.
  + [Glibc library](https://www.gnu.org/software/libc/) version 2\.14 or greater\.
  + The `/var/run` directory must be present on the device\.
  + AWS IoT Greengrass requires hardlink and softlink protection to be enabled on the device\. Without this, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + The `ggc_user` and `ggc_group` user and group must be present on the device\.
  + The following Linux kernel configurations must be enabled on the device: 
    + Namespace: CONFIG\_IPC\_NS, CONFIG\_UTS\_NS, CONFIG\_USER\_NS, CONFIG\_PID\_NS
    + CGroups: CONFIG\_CGROUP\_DEVICE, CONFIG\_CGROUPS, CONFIG\_MEMCG
    + Others: CONFIG\_POSIX\_MQUEUE, CONFIG\_OVERLAY\_FS, CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER, CONFIG\_SECCOMP\_FILTER, CONFIG\_KEYS, CONFIG\_SECCOMP
  + `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` must be enabled\.
  + The Linux kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\.
  + The *memory* `cgroup` must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
  + The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items may be optional:
  + The *devices* `cgroup` must be enabled and mounted if Lambda functions with [Local Resource Access \(LRA\)](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
  + [Python](https://www.python.org/) version 2\.7 is required if Python Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [NodeJS](https://www.nodejs.org/) version 6\.10 or greater is required if Node\.JS Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [ Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or greater is required if Java Lambda functions are used\. If so, ensure that it's added to your `PATH` environment variable\.
  + [OpenSSL](https://www.openssl.org/) 1\.01 or greater is required for [Greengrass OTA Agent](core-ota-update.md#ota-agent) as well as the following commands: `wget`, `realpath`, `tar`, `readlink`, `basename`, `dirname`, `pidof`, `df`, `grep`, and `umount`\.

------
#### [ GGC v1\.3\.0 ]
+ Supported platforms:
  + Architecture: ARMv7l; OS: Linux; Distribution: [Raspbian Jessie, 2017\-03\-02](https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-03-03/)
  + Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\)
  + Architecture: x86\_64; OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04
  + Architecture: ARMv8 \(AArch64\); OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04 \(Annapurna Alpine V2\)
+ The following items are required:
  + Minimum 128 MB RAM allocated to the AWS IoT Greengrass core device\.
  + Linux kernel version 4\.4 or later\. Although several versions might work with AWS IoT Greengrass, for optimal security and performance, we recommend version 4\.4 or later\.
  + [Glibc library](https://www.gnu.org/software/libc/) version 2\.14 or later\.
  + The `/var/run` directory must be present on the device\.
  + AWS IoT Greengrass requires hardlink and softlink protection to be enabled on the device\. Without this, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + The `ggc_user` and `ggc_group` user and group must be present on the device\.
  + The following Linux kernel configurations must be enabled on the device: 
    + Namespace: CONFIG\_IPC\_NS, CONFIG\_UTS\_NS, CONFIG\_USER\_NS, CONFIG\_PID\_NS
    + CGroups: CONFIG\_CGROUP\_DEVICE, CONFIG\_CGROUPS, CONFIG\_MEMCG
    + Others: CONFIG\_POSIX\_MQUEUE, CONFIG\_OVERLAY\_FS, CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER, CONFIG\_SECCOMP\_FILTER, CONFIG\_KEYS, CONFIG\_SECCOMP
  +  The [https://www.sqlite.org/](https://www.sqlite.org/) package is required for AWS IoT device shadows\. Make sure it’s added to your `PATH` environment variable\.
  + `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` must be enabled\.
  + The Linux kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\.
  + The *memory* `cgroup` must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
  + The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items might be optional:
  + The *devices* `cgroup` must be enabled and mounted if Lambda functions with [local resource access \(LRA\)](access-local-resources.md) are used to open files on the AWS IoT Greengrass core device\.
  + [Python](https://www.python.org/) version 2\.7 is required if Python Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.
  + [Node\.js](https://www.nodejs.org/) version 6\.10 or later is required if Node\.js Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.
  + [ Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or later is required if Java Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.
  + [OpenSSL](https://www.openssl.org/) 1\.01 or later is required for [Greengrass OTA Agent](core-ota-update.md#ota-agent) as are the following commands: `wget`, `realpath`, `tar`, `readlink`, `basename`, `dirname`, `pidof`, `df`, `grep`, and `umount`\.

------
#### [ GGC v1\.1\.0 ]
+ Supported platforms:
  + Architecture: ARMv7l; OS: Linux; Distribution: [Raspbian Jessie, 2017\-03\-02](https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-03-03/)
  + Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\)
  + Architecture: x86\_64; OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04
  + Architecture: ARMv8 \(AArch64\); OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04 \(Annapurna Alpine V2\)
+ The following items are required:
  + Minimum 128 MB RAM allocated to the AWS IoT Greengrass core device\.
  + Linux kernel version 4\.4 or later\. Although several versions might work with AWS IoT Greengrass, for optimal security and performance, we recommend version 4\.4 or later\.
  + [Glibc library](https://www.gnu.org/software/libc/) version 2\.14 or later\.
  + The `/var/run` directory must be present on the device\.
  + AWS IoT Greengrass requires hardlink and softlink protection to be enabled on the device\. Without this, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + The `ggc_user` and `ggc_group` user and group must be present on the device\.
  + The following Linux kernel configurations must be enabled on the device: 
    + Namespace: CONFIG\_IPC\_NS, CONFIG\_UTS\_NS, CONFIG\_USER\_NS, CONFIG\_PID\_NS
    + CGroups: CONFIG\_CGROUP\_DEVICE, CONFIG\_CGROUPS, CONFIG\_MEMCG
    + Others: CONFIG\_POSIX\_MQUEUE, CONFIG\_OVERLAY\_FS, CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER, CONFIG\_SECCOMP\_FILTER, CONFIG\_KEYS, CONFIG\_SECCOMP
  +  The [https://www.sqlite.org/](https://www.sqlite.org/) package is required for AWS IoT device shadows\. Make sure it’s added to your `PATH` environment variable\.
  + `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` must be enabled\.
  + The Linux kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\.
  + The *memory* `cgroup` must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
  + The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items might be optional:
  + [Python](https://www.python.org/) version 2\.7 is required if Python Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.
  + [Node\.js](https://www.nodejs.org/) version 6\.10 or later is required if Node\.js Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.
  + [ Java](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) version 8 or later is required if Java Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.

------
#### [ GGC v1\.0\.0 ]
+ Supported platforms:
  + Architecture: ARMv7l; OS: Linux; Distribution: [Raspbian Jessie, 2017\-03\-02](https://downloads.raspberrypi.org/raspbian/images/raspbian-2017-03-03/)
  + Architecture: x86\_64; OS: Linux; Distribution: Amazon Linux \(amzn\-ami\-hvm\-2016\.09\.1\.20170119\-x86\_64\-ebs\)
  + Architecture: x86\_64; OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04
  + Architecture: ARMv8 \(AArch64\); OS: Linux; Distribution: Ubuntu 14\.04 – 16\.04 \(Annapurna Alpine V2\)
+ The following items are required:
  + Minimum 128 MB RAM allocated to the AWS IoT Greengrass core device\.
  + Linux kernel version 4\.4 or later\. Although several versions might work with AWS IoT Greengrass, for optimal security and performance, we recommend version 4\.4 or later\.
  + [Glibc library](https://www.gnu.org/software/libc/) version 2\.14 or later\.
  + The `/var/run` directory must be present on the device\.
  + AWS IoT Greengrass requires hardlink and softlink protection to be enabled on the device\. Without this, AWS IoT Greengrass can only be run in insecure mode, using the `-i` flag\.
  + The `ggc_user` and `ggc_group` user and group must be present on the device\.
  + The following Linux kernel configurations must be enabled on the device: 
    + Namespace: CONFIG\_IPC\_NS, CONFIG\_UTS\_NS, CONFIG\_USER\_NS, CONFIG\_PID\_NS
    + CGroups: CONFIG\_CGROUP\_DEVICE, CONFIG\_CGROUPS, CONFIG\_MEMCG
    + Others: CONFIG\_POSIX\_MQUEUE, CONFIG\_OVERLAY\_FS, CONFIG\_HAVE\_ARCH\_SECCOMP\_FILTER, CONFIG\_SECCOMP\_FILTER, CONFIG\_KEYS, CONFIG\_SECCOMP
  +  The [https://www.sqlite.org/](https://www.sqlite.org/) package is required for AWS IoT device shadows\. Make sure it’s added to your `PATH` environment variable\.
  + `/dev/stdin`, `/dev/stdout`, and `/dev/stderr` must be enabled\.
  + The Linux kernel must support [cgroups](https://en.wikipedia.org/wiki/Cgroups)\.
  + The *memory* `cgroup` must be enabled and mounted to allow AWS IoT Greengrass to set the memory limit for Lambda functions\.
  + The root certificate for Amazon S3 and AWS IoT must be present in the system trust store\.
+ The following items might be optional:
  + [Python](https://www.python.org/) version 2\.7 is required if Python Lambda functions are used\. If so, make sure that it's added to your `PATH` environment variable\.

------

## AWS IoT Greengrass Downloads<a name="gg-downloads"></a>

 You can use the following information to locate and download software for use with AWS IoT Greengrass\. 

### AWS IoT Greengrass Core Software<a name="gg-core-download-tab"></a>

 AWS IoT Greengrass Software extends AWS functionality onto a AWS IoT Greengrass core device, enabling local Devices to act locally on the data they generate\. 

------
#### [  v1\.7\.0  ]
+ [ Architecture: AArch64, OS: Linux](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.7.0/greengrass-linux-aarch64-1.7.0.tar.gz)\.
+ [ Architecture: ARMv7l, OS: Linux](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.7.0/greengrass-linux-armv7l-1.7.0.tar.gz)\.
+ [ Architecture: x86\_64; OS: Linux](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.7.0/greengrass-linux-x86-64-1.7.0.tar.gz)\.

------
#### [  v1\.6\.0  ]
+ [ Architecture: AArch64; OS: Linux](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.6.0/greengrass-linux-aarch64-1.6.0.tar.gz)\.
+ [ Architecture: ARMv7l; OS: Linux](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.6.0/greengrass-linux-armv7l-1.6.0.tar.gz)\.
+ [ Architecture: x86\_64; OS: Linux](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.6.0/greengrass-linux-x86-64-1.6.0.tar.gz)\.
+ [ Architecture: x86\_64; OS: Ubuntu](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.6.0/greengrass-ubuntu-x86-64-1.6.0.tar.gz)\.

------

### AWS IoT Greengrass Core SDK Software<a name="gg-core-sdk-download"></a>

 The [AWS IoT Greengrass Core SDK](lambda-functions.md#lambda-sdks-core) enables Lambda functions to interact with the AWS IoT Greengrass core on which they run\. 

------
#### [  v1\.3\.0  ]
+  [ Java 8](https://d1onfpft10uf5o.cloudfront.net/greengrass-sdk/downloads/java/8/aws-greengrass-core-sdk-java-1.3.0.tar.gz)\. 
+  [ Node JS 6\.10](https://d1onfpft10uf5o.cloudfront.net/greengrass-sdk/downloads/nodejs/6.10/aws-greengrass-core-sdk-js-1.3.0.tar.gz)\. 
+  [ Python 2\.7](https://d1onfpft10uf5o.cloudfront.net/greengrass-sdk/downloads/python/2.7/greengrass-core-python-sdk-1.3.0.tar.gz)\. 

------
#### [  v1\.2\.0  ]
+  [ Java 8](https://d1onfpft10uf5o.cloudfront.net/greengrass-sdk/downloads/java/8/aws-greengrass-core-sdk-java-1.2.0.tar.gz)\. 
+  [ Node JS 6\.10](https://d1onfpft10uf5o.cloudfront.net/greengrass-sdk/downloads/nodejs/6.10/aws-greengrass-core-sdk-js-1.2.0.tar.gz)\. 
+  [ Python 2\.7](https://d1onfpft10uf5o.cloudfront.net/greengrass-sdk/downloads/python/2.7/greengrass-core-python-sdk-1.2.0.tar.gz)\. 

------

### AWS IoT Greengrass Docker Software<a name="gg-docker-download"></a>

 The AWS IoT Greengrass Docker Software download enables you to run AWS IoT Greengrass in a Docker container\. Instructions are provided in the Docker file\. 

------
#### [  v1\.7\.0  ]
+  [ Docker v1\.7\.0](https://d1onfpft10uf5o.cloudfront.net/greengrass-core/downloads/1.7.0/aws-greengrass-docker-1.7.0.tar.gz)\. 

------

### AWS IoT Greengrass ML SDK Software<a name="gg-ml-sdk-download"></a>

The [AWS IoT Greengrass Machine Learning SDK](lambda-functions.md#lambda-sdks-ml) enables the Lambda functions you author to consume the machine learning model available on the device\.

------
#### [  v1\.0\.0  ]
+  [ Python 2\.7](https://d1onfpft10uf5o.cloudfront.net/greengrass-ml-sdk/downloads/python/2.7/greengrass-machine-learning-python-sdk-1.0.0.tar.gz)\. 

------

## We Want to Hear from You<a name="contact-us"></a>

We welcome your feedback\. To contact us, visit the [AWS IoT Greengrass Forum](https://forums.aws.amazon.com/forum.jspa?forumID=254)\.