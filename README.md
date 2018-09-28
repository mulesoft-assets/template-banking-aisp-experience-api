# Template Banking AISP Experience API

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [APIs security considerations](#apissecurityconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Applying policies on CloudHub](#applyingpolicies)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)

# License Agreement <a name="licenseagreement"/>
Note that using this template is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this template. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>

This API exposes endpoints for external applications to retrieve account information (general information, balance, transactions).

# Considerations <a name="considerations"/>

To make this Anypoint Template run, there are certain preconditions that must be considered. **Failing to do so could lead to unexpected behavior of the template.**

## APIs security considerations <a name="apissecurityconsiderations"/>
This Experience API is meant to be deployed to CloudHub and managed using the API Platform Manager.

In order to retrieve the information, the third party application must be first registered with the bank's authorization server (AS) when it receives client_id and client_secret.
The user selects the bank which he/she wants to add access to the application and user is redirected to the bank AS OAuth 2 authorize endpoint to verify user's identity.
When user is successfully authenticated, the application receives authorization code which it can use to obtain the access token. With possession of access token, the application can make requests
on user's behalf to obtain account information until the token is valid. The token is in JWT form and it is signed and encrypted.

In order to use this API, you must apply and configure [Oauth 2.0 JWE access token enforcement policy](https://github.com/mulesoft/template-banking-authorization-policy)
Follow the instructions on the above link on how to configure the policy.



This API consumes [Accounts Process API](https://github.com/mulesoft/template-banking-accounts-process-api). The Accounts Process API is secured by Client ID Enforcement policy so when making calls to it, this API needs to authenticate itself (HTTP Basic Authentication). You must register this API via API Manager to get credentials (client_id and client_secret) as described [here](https://docs.mulesoft.com/api-manager/browsing-and-accessing-apis). 

### Exposing external endpoints with HTTPS
+ It is meant to be consumed by third party applications using HTTPS

# Run it! <a name="runit"/>
Simple steps to get AISP Experience API running.
See below.

## Running on premise <a name="runonopremise"/>
In this section we detail the way you should run your Anypoint Template on your computer.


### Where to Download Anypoint Studio and Mule ESB
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Anypoint Studio from this [Location](https://www.mulesoft.com/platform/studio)
+ You can download Mule ESB from this [Location](https://www.mulesoft.com/platform/soa/mule-esb-open-source-esb)

### Importing an Anypoint Template into Studio
Anypoint Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio Project from File System
+ Packaged mule application (.jar)ion

You can find a detailed description on how to do so in this [Documentation Page](http://www.mulesoft.org/documentation/display/current/Importing+and+Exporting+in+Studio).

### Running on Studio <a name="runonstudio"/>
Once you have imported you Anypoint Template into Anypoint Studio you need to follow these steps to run it:

+ Generate keystore and truststore (You can find a detailed description on how to do so in this [Documentation Page](https://docs.mulesoft.com/mule-runtime/4.1/tls-configuration)
+ Locate the properties file `mule-<env>.properties`, in src/main/app/resources
+ Complete all the properties required as per the examples in the section [Properties to be configured](#propertiestobeconfigured)
+ Once that is done, right click on you Anypoint Template project folder 
+ Hover you mouse over `"Run as"`
+ Click on  `"Mule Application"`

### Running on Mule ESB stand alone <a name="runonmuleesbstandalone"/>
Complete all properties in one of the property files, for example in [mule.prod.properties](../master/src/main/resources/mule.prod.properties) and run your app with the corresponding environment variable to use it. To follow the example, this will be `mule.env=prod`. 

## Running on CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](https://docs.mulesoft.com/runtime-manager/hello-world-on-cloudhub) (Or you can do it later as a next step), you need to go to `"Manage Application"` > `"Properties"` to set all environment variables detailed in **Properties to be configured**.
Follow other steps defined [here](#runonpremise) and once your app is all set and started, there is no need to do anything else.

### Deploying your Anypoint Template on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Anypoint Studio provides you with really easy way to deploy your Template directly to CloudHub, for the specific steps to do so please check this [link](https://docs.mulesoft.com/mule-user-guide/v/3.8/deploying#DeployingMuleApplications-DeploytoCloudHub)

### Applying policies on CloudHub <a name="applyingpolicies"/>
When a Mule application is deployed using the Mule 4.1.3+ Runtime, the API Manager allows you to dynamically apply different policies that can be used for securing the application, among many other things. More information can be found in [API Manager policies documentation](https://docs.mulesoft.com/api-manager/using-policies)

## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
In order to use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. 

Detailed list with examples:
### Application properties
+ https.port `8081`

#### HTTPS configuration
+ keystore.location `keystore.jks` 
+ keystore.password `password`
+ key.password `password`
+ key.alias `1`

#### Anypoint Platform API auto-discovery
+ anypoint.platform.client_id `cloudhub_client_id` 
+ anypoint.platform.client_secret `cloudhub_client_secret` 
+ api.id `123`

#### Process API calls configuration
+ api.banking.accounts.host `api.example.com`
+ api.banking.accounts.port `443` 
+ api.banking.accounts.basepath `/api`
+ api.banking.accounts.protocol `HTTPS`

#### Client ID Enforcement Policy for Anypoint Bank Process API
api.banking.accounts.client_id `client_id`
api.banking.accounts.client_secret `client_secret`
