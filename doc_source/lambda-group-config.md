# Controlling Execution of Greengrass Lambda Functions by Using Group\-Specific Configuration<a name="lambda-group-config"></a>

AWS IoT Greengrass provides cloud\-based management of Greengrass Lambda functions\. You can configure details of how the Lambda function behaves when it runs in a particular group\. Although a function's code and dependencies are managed using AWS Lambda, AWS IoT Greengrass supports the following group\-specific configuration settings:

**Run as**  
The access identity used to run each Lambda function\. By default, Lambda functions run as ggc\_user and ggc\_group\. You can change the setting and choose the user ID and group ID that has the permissions required to run the Lambda function\. You can override both UID and GID or just one if you leave the other field blank\. This setting gives you more granular control over access to device resources\. We recommend that you configure your Greengrass hardware with appropriate resource limits, file permissions, and disk quotas for the users and groups whose permissions are used to run Lambda functions\.  
We recommend that you avoid running as root unless absolutely necessary\. When you run a Lambda function as root, you increase the risk of unintended changes, such as accidentally deleting a critical file\. In addition, running as root increases the risks to your data and device from malicious individuals\. If you do need to run as root, you must update the AWS IoT Greengrass configuration to enable it\. For more information, see [Running a Lambda Function as Root](#lambda-running-as-root)\.  
We disallow running as root when you run in a Greengrass container to make it more difficult for malicious attacks to get root access on your device\.

**UID \(number\)**  
The user ID for the user that has the permissions required to run the Lambda function\. This setting is only available if you choose **Run as another user ID/group ID**\. You can look up the user ID you want to use to run the Lambda function by using the getent passwd command on your AWS IoT Greengrass device\.

**GID \(number\)**  
The group ID for the group that has the permissions required to run the Lambda function\. This setting is only available if you choose **Run as another user ID/group ID**\. You can look up the group ID you want to use to run the Lambda function by using the getent group command on your AWS IoT Greengrass device\.

**Containerization**  
Choose whether the Lambda function runs with the default containerization for the group, or specify the containerization that should always be used for this Lambda function\. To run without enabling your device kernel namespace and cgroup, all your Lambda functions must run without containerization\. You can accomplish this easily by setting the default containerization for the group\. See [Setting Default Containerization for Lambda Functions in a Group](#lambda-containerization-groupsettings)\.  
We recommend that you run Lambda functions in a Greengrass container unless your use case requires them to run without containerization\. When running in a Greengrass container, you can use attached resources, and gain the benefits of isolation and increased security\. Before you change the containerization, see [Considerations When Choosing Lambda Function Containerization](#lambda-containerization-considerations)\.

**Memory limit**  
The memory allocation for the function\. The default is 16 MB\.  
This setting is not available when you run a Lambda function without containerization\. Lambda functions run without containerization have no memory limit\. The memory limit setting is discarded when you change the Lambda function to run without containerization\.

**Timeout**  
The amount of time before the function or request is terminated\. The default is 3 seconds\.

**Lifecycle**  
A Lambda function lifecycle can be *on\-demand* or *long\-lived*\. The default is on\-demand\.  
An on\-demand Lambda function starts in a new or reused container when invoked\. Requests to the function might be processed by any available container\. A long\-lived—or *pinned*—Lambda function starts automatically after AWS IoT Greengrass starts and keeps running in its own container \(or sandbox\)\. All requests to the function are processed by the same container\. For more information, see [Lifecycle Configuration for Greengrass Lambda Functions](lambda-functions.md#lambda-lifecycle)\.

**Read access to /sys directory**  
Whether the function can access the host's /sys folder\. Use this when the function must read device information from /sys\. The default is false\.  
This setting is not available when you run a Lambda function without containerization\. This value of this setting is discarded when you change the Lambda function to run without containerization\.

**Input payload data type**  
The expected encoding type of the input payload for the function, either JSON or binary\. [Lambda executables](lambda-functions.md#lambda-executables) support the binary encoding type only, not JSON\.  
Accepting binary input data can be useful for functions that interact with device data, because the restricted hardware capabilities of devices often make it difficult or impossible for them to construct a JSON data type\. Support for the binary encoding type is available starting in AWS IoT Greengrass Core Software v1\.5\.0 and AWS IoT Greengrass Core SDK v1\.1\.0\.

**Environment variables**  
Key\-value pairs that can dynamically pass settings to function code and libraries\. Local environment variables work the same way as [AWS Lambda function environment variables](https://docs.aws.amazon.com/lambda/latest/dg/env_variables.html), but are available in the core environment\.

**Resource access policies**  
A list of up to 10 [local resources](access-local-resources.md), [secret resources](secrets.md), and [machine learning resources](ml-inference.md) that the Lambda function is allowed to access, and the corresponding `read-only` or `read-write` permission\. In the console, these *affiliated* resources are listed on the function's **Resources** page\.  
Resource access policies apply only when Lambda functions are run in a Greengrass container\. When you run Lambda functions without containerization, you can directly access local resources\. Secret resources can be accessed when running without containerization\.

## Running a Lambda Function as Root<a name="lambda-running-as-root"></a>

Before you can run one or more Lambda functions as root, you must first update the AWS IoT Greengrass configuration to enable support\. Support for running Lambda functions as root is off by default\. The deployment fails if you try to deploy a Lambda function and run it as root \(UID and GID of 0\) and you haven't updated the AWS IoT Greengrass configuration\. An error like the following appears in the runtime log \(*greengrass\_root*/ggc/var/log/system/runtime\.log\):

```
                lambda(s)
                [list of function arns] are configured to run as root while Greengrass is not configured to run lambdas with root permissions
```

**Important**  
We recommend that you avoid running as root unless absolutely necessary\. When you run a Lambda function as root, you increase the risk of unintended changes, such as accidentally deleting a critical file\. In addition, running as root increases the risks to your data and device from malicious individuals\.

**To allow Lambda functions to run as root**

1. On your AWS IoT Greengrass device, navigate to the *greengrass\-root*/config folder\.
**Note**  
By default, *greengrass\-root* is the /greengrass directory\.

1. Update the config\.json file to add `"allowFunctionsToRunAsRoot" : "yes"` to the `runtime` field\. For example:

   ```
                       {
                           "coreThing" : {
                               ...
                           },
                           "runtime" : {
                               ...
                               "allowFunctionsToRunAsRoot" : "yes"
                           }
                       }
   ```

1. Use the following commands to restart AWS IoT Greengrass:

   ```
                       
                       cd /greengrass/ggc/core
                       sudo ./greengrassd restart
   ```

   Now you can set the user ID and group ID \(UID/GID\) of Lambda functions to 0 to run that Lambda function as root\.

You can change the value of `"allowFunctionsToRunAsRoot"` to `"no"` and restart AWS IoT Greengrass if you want to disallow Lambda functions to run as root\.

## Considerations When Choosing Lambda Function Containerization<a name="lambda-containerization-considerations"></a>

By default, Lambda functions run inside an AWS IoT Greengrass container\. That container provides isolation between your functions and the host, providing additional security for both the host and the functions in the container\.

We recommend that you run Lambda functions in a Greengrass container unless your use case requires them to run without containerization\. By running your Lambda functions in a Greengrass container, you have more control over restricting access to resources\.

Here are some example use cases for running without containerization:
+ You want to run AWS IoT Greengrass on a device where you cannot make changes to the underlying kernel\. This might be due to company security policies or because the OS version does not allow it\.
+ You want to run your Lambda function in another container environment with its own OverlayFS, but encounter OverlayFS conflicts when you run in a Greengrass container\.
+ You need access to local resources with paths that can't be determined at deployment time or whose paths can change after deployment, such as pluggable devices\.
+ You have a legacy application that was written as a process and you have encountered issues when running it as a containerized Lambda function\.


**Containerization Differences**  

| Containerization | Notes | 
| --- | --- | 
| AWS IoT Greengrass container | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/lambda-group-config.html) | 
| No container | [\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/greengrass/latest/developerguide/lambda-group-config.html) | 

Changing the containerization for a Lambda function can cause problems when you deploy it\. If you had assigned local resources to your Lambda function that are no longer available with your new containerization settings, deployment fails\. 
+ When you change a Lambda function from running in a Greengrass container to running without containerization, memory limits for the function are discarded\. You must access the file system directly instead of using attached local resources\. You must remove any attached resources before you deploy\.
+ When you change a Lambda function from running without containerization to running in a container, your Lambda function loses direct access to the file system\. You must define a memory limit for each function or accept the default 16 MB\. You can configure those settings for each Lambda function before you deploy\.

**To change containerization settings for a Lambda function**

1. In the AWS IoT console, choose **AWS IoT Greengrass**, and then choose **Groups**\.

1. Choose the group that contains the Lambda function whose settings you want to change\.

1. Choose **Lambdas**\.

1. On the Lambda function that you want to change, choose the ellipsis \(**…**\) and then choose **Edit configuration**\.

1. Change the containerization settings\. If you configure the Lambda function to run in a Greengrass container, you must also set the **Memory limit** and **Read access to /sys directory** properties\.

1. Choose **Update** to save the changes to your Lambda function\.

The changes take effect when the group is deployed\.

You can also use the [CreateFunctionDefinition](https://docs.aws.amazon.com/greengrass/latest/apireference/createfunctiondefinition-post.html) and [CreateFunctionDefinitionVersion](https://docs.aws.amazon.com/greengrass/latest/apireference/createfunctiondefinitionversion-post.html)in the *AWS IoT Greengrass API Reference*\. If you are changing the containerization setting, be sure to update the other parameters too\. For example, if you are changing from running a Lambda function in a Greengrass container to running without containerization, be sure to clear the `MemorySize` parameter\.

### Determine the Isolation Modes Supported by Your Greengrass Device<a name="dependency-checker-tests-isolation"></a>

You can use the AWS IoT Greengrass dependency checker to determine which isolation modes \(Greengrass container/no container\) are supported by your Greengrass device\.

**To run the AWS IoT Greengrass dependency checker**

1. Download the AWS IoT Greengrass dependency checker from the [GitHub repository](https://github.com/aws-samples/aws-greengrass-samples)\.

1. Run the AWS IoT Greengrass dependency checker as follows:

   ```
   git clone https://github.com/aws-samples/aws-greengrass-samples.git
   cd aws-greengrass-samples
   cd greengrass-dependency-checker-GGCv1.7.0
   sudo modprobe configs
   sudo ./check_ggc_dependencies | more
   ```

1. Where `more` appears, press the Spacebar key to display another page of text\.

For information about the modprobe command, you can run man modprobe in the terminal\. 

## Setting Default Containerization for Lambda Functions in a Group<a name="lambda-containerization-groupsettings"></a>

You can modify the group settings to specify the default containerization for Lambda functions in the group\. You can override this setting for one or more Lambda functions in the group if you want the Lambda functions to run with containerization different from the group default\. Before you change containerization settings, see [Considerations When Choosing Lambda Function Containerization](#lambda-containerization-considerations)\.

**Important**  
If you want to change the default containerization for the group, but have one or more functions that use a different containerization, change the settings for the Lambda functions before you change the group setting\. If you change the group containerization setting first, the values for the **Memory limit** and **Read access to /sys directory** settings are discarded\.

**To modify containerization settings for your AWS IoT Greengrass group**

1. In the AWS IoT console, choose **AWS IoT Greengrass** and then choose **Groups**\.

1. Choose the group whose settings you want to change\.

1. Choose **Settings**\.

1. In the **Lambda runtime environment** section, change the containerization setting\.

The changes take effect when the group is deployed\.