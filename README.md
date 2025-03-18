# **Rio-API-Middleware**

**Author**: Alex Leach Created: \[\[2025-03-13\]\] | #docs #handover #reference #readme

## **Introduction**

The **Rio API Middleware** project enables secure and efficient communication between Power Automate or other Low-code platforms with [Rio EPR](https://www.theaccessgroup.com/en-gb/products/rio/). It allows requests using both SOAP and REST standards for consuming, updating and creating Patient Data.

## **Updates**

Changes to the project documentation go here when needed. They will be major changes only. E.g. Versions, Breaking issues and deployment statuses.

| Date | Note | Branch | Tags |
| --- | --- | --- | --- |
| \[\[2025-03-13\]\] | Readme.md created. | Main | #docs |
| \[\[2025-03-17\]\] | Readme.md updated with quick start outline | Main | #doc |

## **Getting Started**

### **Things to know**

> \[!WARNING\] Redacted Values Please make sure to review actual values for where secure or sensitive information has been redacted. These are marked by `{REDACTED}`.

> \[!INFORMATION\] TAG is the acronym for **The Access Group**. The 3rd-party provider for the Rio EPR APIs.

> \[!INFORMATION\] Rio Web-service The Rio Web-service refers to the external services provided by TAG that handle and coordinate incoming requests for electronic patient record data. It is given an overview in the offical TAG Rio Api documentation.

### **Onboarding**

For new developers or developers picking up the project, it is recommended to read the \[\[Handover Reference\]\]. This document goes through the project, its' resources, codebase and other facets that comprise it.

**For a quick-start guide jump to** \[\[#Setup Development Environment\]\].

This will get you a local development and testing environment for working from the development instance and test branch from the project Azure DevOps Repository (_Repo_).

### **Setup Development Environment**

#### **Local**

##### **Outline**

**Needed Azure access permissions**.

As a minimum you will need the following Azure Role Based Access Permissions at the resource-group level.

| Resource | Type | Role |
| --- | --- | --- |
| rg-devtest-iautom | Resource Group | Reader |
| Key Vault |  | Key Vault Certificates Officer |
|  |  | Key Vault Secrets Officer |
| Storage Account |  | Storage Blob Data Owner |
| API Management |  | API Management Service Contributor |
| App Service | App Service | Contributor |
| Function App | Microsoft.Web/sites | Contributor |
| Redis Cache | _TBA_ | _TBA_ |

> \[!IMPORTANT\] Request permissions If these are not setup, please log a ticket via IT Support for review by _Server support/Systems_ Team. See below: \[\[#Contacts\]\]

##### **1.Robot/VM Setup**

Install if they're not already the following apps/packages by locating them in the **Company Portal**:

1.  Git
    
2.  Github Desktop
    
3.  Node.js
    
4.  Microsoft Azure Cli
    
5.  Microsoft Azure Functions Core Tools
    
6.  Postman
    
7.  Visual Studio Code
    

> \[!TIP\] VS Studio Code or _VS Code_ Recommended editor but if you wish to use a different IDE or text editor thats ok too. You may need to reach out to 365 Cloud & Modern Desktop Management Team for support and reference Microsoft documentation for getting setup with Azure Function Development.

*   Windows Terminal _Can be very handy Terminal emulator vs Windows CMD._
    
*   Cli Setup
    
*   From windows terminal type: `az --help` to verify the Azure cli has been installed successfully.
    
*   If the cli is accessible, run `az login`. This will open a browser window, allowing you to login using with your account credentials. These credentials should have been approved for access to the Azure Resource Group holding the Rio Middleware Api.
    
*   Follow the instructions in the terminal window from the Az Cli for selecting the correct subscription. As of 2025-03-14, the current subscription is **subscription-bhft-ea-devtest-iautom-team**
    

> \[!INFO\] For detailed steps and troubleshooting See: [Get Started with Azure Cli](https://learn.microsoft.com/en-us/cli/azure/get-started-with-azure-cli)

*   **Node.js**
    
*   Check that the node runtime is available from the terminal by running `node --version` in your terminal. If a version number is returned that it has been successfully installed.
    
*   Check the Node Pack
    

> \[!WARNING\] Supported Node Version Your node version must be greater than or equal to 20.x.x

##### **2\. Editor setup**

> \[!INFO\] For a detailed guide and explanation See: [Develop Azure Functions by using Visual Studio Code](https://learn.microsoft.com/en-us/azure/azure-functions/functions-develop-vs-code?tabs=node-v4,python-v2,isolated-process,quick-create&pivots=programming-language-javascript)

###### **Open Visual Studio Code**

*   **Install the Azure Functions Extension** Open the Extensions view (`Ctrl+Shift+X`), search for "Azure Functions," and install the extension by Microsoft.
    
*   **Install Azure Resources Extension**: Install the "Azure Account" extension to manage your Azure resources from VS Code.
    
*   **Install Azure Storage Extension** _Optionally_ install Azure Tools, Azure Developer CLI, Azure Cli Tools, Azure App Service
    

##### **3\. Install Azure Functions Core Tools**

*   The Azure Functions extension integrates with Azure Functions Core Tools, allowing you to run functions locally.
    
*   If Azure Functions Core Tools was not installed by the Company Portal:
    
*   Open the command palette (`ctrl+shift+P`) and type `Azure Functions: Install or Update Core Tools`.
    

##### **4\. Development Environment**

Now that you have an editor and the environment setup. The next step is to open the git repository from Azure Devops.

This can be done in two ways:

###### **The first is using the browser and VS Code GUI**

1.  Open the repository in the browser by signing into Azure [azrOp185\_RioApi\_Middleware\_Api Test](https://dev.azure.com/BerkshireHealthcareNHSFoundationTrust-IA/Intelligent%20Automation/_git/azrOp185_RioApi_Middleware_Api?path=%2F&version=GBtest&_a=contents).
    
2.  On the right hand side choose either **Clone** or alternatively open the 'more' menu by clicking the elipsis to the right. Then select download as zip. Place this in a folder where you keep development work and **extract all**.
    
3.  In VS Code choose File > Open Folder and choose the newly extracted folder from the repository zip file you just extracted.
    
4.  If a 'Do you trust the authors' popup appears. Check **Trust the authors of all files in the parent folder..** and press the **Yes, I trust the authors** button.
    

###### **The second method is open VS Code and open a terminal.**

*   In the terminal type `cd ~/{{Your location of choice for the repo}}`.
    
*   Then type `git clone https://BerkshireHealthcareNHSFoundationTrust-IA@dev.azure.com/BerkshireHealthcareNHSFoundationTrust-IA/Intelligent%20Automation/_git/azrOp185_RioApi_Middleware_Api`
    

> This will pull down the repository to the folder you navigated to. Once the pull has finished you can check your in the root project folder by typing `pwd`. Your current working director should look something like `/Rio-Api-middleware/Rio-Api-middleware/azrOp185_RioApi_Middleware_Api`.

*   Initialize the project node dependencies
    
*   In the Terminal panel of VS Code type `npm i`.
    

> Note this may take a minute so please let it finish.

##### **Once the dependencies (npm packages) have been installed**

Make sure to be logged in to the azure account for your user. You can do this by opening the terminal in VS Code `ctrl + backtick` and typing `az account show`.

###### **The output should a json project with the signed in account details**

```code-line
{
"environmentName": "AzureCloud",
"homeTenantId": "{REDACTED}",
"id": "{REDACTED}",
"isDefault": true,
"managedByTenants": [],
"name": "{REDACTED}",
"state": "Enabled",
"tenantDefaultDomain": "{REDACTED}",
"tenantDisplayName": "{REDACTED}",
"tenantId": "{REDACTED}",
"user": {
 "name": "{REDACTED}@berkshire.nhs.uk",
 "type": "user"
 }
}

```

> \[!TIP\] Output If the user object displays your current user account you are signed in.

If you have pulled down the latest branch for the Rio Api codebase and opened it in VS Code, you can now build and run the function-app locally. To do this, go to VS Code and either choose:

*   File > Run > **Start with Debugging** **Or**
    
*   Use short cut key `F5`. **Or**
    
*   Use the combination of `ctrl + shift + B` and then `Enter`. To start debugging.
    

**On successful build you should see a terminal output similar to this:**

```code-line
[2025-03-17T11:14:12.812Z] Debugger listening on ws://127.0.0.1:9229/b21757f5-e191-449a-b4af-2c4028b73b37
[2025-03-17T11:14:12.813Z] For help, see: https://nodejs.org/en/docs/inspector
[2025-03-17T11:14:12.904Z] Worker process started and initialized.
Functions:
Authn: [GET] http://localhost:7071/api/AuthMe
CaseLoadGet: [GET] http://localhost:7071/api/CaseLoadGet
DocumentAdd: [POST] http://localhost:7071/api/DocumentAdd
GetEnterpriseDataTest: [GET] http://localhost:7071/api/GetEnterpriseDataTest
PatientDemographicGet: [GET] http://localhost:7071/api/PatientDemographicGet
ProgressNoteAdd: [POST] http://localhost:7071/api/ProgressNoteAdd
RestReferrals: [POST] http://localhost:7071/api/route/referrals/{endpoint}
RestRouter: [GET] http://localhost:7071/api/route/{endpoint}
RioOutboundTest: [GET] http://localhost:7071/api/RioOutboundTest
TestCache: [GET] http://localhost:7071/api/TestCache
UnitTests: [GET] http://localhost:7071/api/UnitTests
VnetIntergrationTestUdp: [GET] http://localhost:7071/api/VnetIntergrationTestUdp
For detailed output, run func with --verbose flag.
[2025-03-17T11:14:13.370Z] Debugger attached.
**************************************...

```

**Success**! You have now built and run a local instance of the Rio-API-Middleware function-app.

> \[!WARNING\] Azure Storage Connection String Occasionally an error will show in the terminal pane/window stating that the _Azure Storage connection string is either empty or invalid_ . **You can ignore this error. Make sure to stop the running instance of the function and then re-run debugging to reconnect before continuing work.**

##### **Running a test in a Http-notebook**

To test the local function-app instance open the file: ./notebooks/**referrals.http**

*   At the top of the file is a Request Access-token section. In this section a HTTP request is defined. It retrieves a valid access-token from the Rio Api Web-service. It uses a mTLS certificate provided by The Access Group. See below for errors that state **401 Denied or 403 - Unauthorised**.
    
*   Press the play-icon to the right of the code-block directly under the Main section Title **Request Access-token**.
    
*   _The request will takes up to 12 seconds for the first time._
    

**A Sample Test resulting in a success - Http Status 200** (gif) !\[\[Screen Recording 2025-03-17 at 14.23.54.gif\]\]

**A Sample Test resulting in a fail - Http Status 403** (gif) !\[\[Screen Recording 2025-03-17 at 11.38.31.gif\]\]

###### **Invalid Certificate**

> \[!ERROR\] Failed request from a http book 403 If you receive a **403** This means that the certificate you are rpoviding to the function app by header `X-ARR-ClientCert` is invalid. A new one can be obtained in the APIM Portal when testing. See \[\[Handover Reference#APIM\]#Testing\]\].

###### **Unauthorized**

> \[!ERROR\] Failed request from a http book 401 If you received a **401** - This means that there has been an authentication failure between the function-app and the Rio Api Web Service.

##### **Local settings for http notebooks**

The variables that contain the **api-subscription-key** `subKey` and the **certificate** `cert` are located in the file **/notebooks/local-http-settings.http**.

> \[!WARNING\] This file is not version controlled and ignored by Git

You can change the values in that file to update the certificate base64 string and the subscription key issued for your account through APIM. See subscriptions under \[\[Handover Reference#APIM#Subscriptions\]\].

##### **Troubleshooting local VS Code development**

> \[!IMPORTANT\] Read more for troubleshooting help here: [Azure Functions](https://learn.microsoft.com/en-us/azure/azure-functions/) - Troubleshooting

###### **Most common problems**

Can occur to permissions but can also be from:

*   Accessing the function-app's storage account from a networked BHFT-machine, Role Based Access management and VM networking.
    
*   Storage Connection Strings or just incorrect connection-strings defined in the local.settings.json file (project root folder).
    

> \[!WARNING\] Do not add local.settings.json to version-control

*   Azure Cloud resources missing network configuration.
    
*   Having the incorrect NPM package versions set in the package.json file in the root project folder.
    
*   An incorrect version of Node.js installed or enabled in the local environment.
    
*   Out-of-date or incorrect install of Azure-Functions-Core tools.
    

###### **Request help for troubleshooting**

Any of the above causes should be resolved with the help of Server Support. See \[\[README#Contacts\]\]

##### **5\. Setup Postman**

The next step (this is optional) is to setup postman api development app.

###### **Step 1: Requesting an Invite to a Team**

1.  If your team is using Postman, request an invite from your team admiistrator.
    
2.  Once you receive the invite link, click on it to join the team worspace.
    
3.  Log in using your credentials or create an account if necessary.
    
4.  Access the team workspace and shared collections.
    

###### **Step 2: Opening a Collection Shared by the Team**

1.  In Postman, navigate to the **Workspaces** tab.
    
2.  Select the **Team Workspace** where shared collections are stored.
    
3.  In the left panel, find the **Collections** tab.
    
4.  Click on the shared collection to view its requests.
    
5.  Select a request and click **Send** to test it.
    
6.  Review the response in the lower panel.
    

###### **Step 3: Adding a Custom Certificate in Postman**

> \[!INFORMATION\] Why add a certificate? When working with **self-signed certificates** or **internal APIs** that require **mutual TLS authentication**. Postman allows you to add custom client certificates from the app's settings panel.

*   **To continue - you will need to have:**
    
*   A **certificate** for the test environment in the Rio Webservice. The file begins with `CN=Berks HSCN ...` and **must be in .pfx format**
    
*   Certificate **passphrase**
    
*   **Domain name** the certificate has been issued for.
    

##### **Once you have both the passphrase and certificate file in** `.pfx` **format.**

1.  Open **Postman** and go to **Settings**:
    
    *   Click on your profile image in the top-right corner.
        
    *   Select **Settings**.
        
    *   Navigate to the **Certificates** tab.
        
2.  Under the **Client Certificates** section, click **Add Certificate**.
    
3.  Enter the **domain name** for which the certificate should be used.
    
4.  Click **Select File** and upload the **.crt** (certificate) file.
    
5.  Click **Select File** and upload the **.key** (private key) file.
    
6.  (Optional) If your certificate requires a passphrase, enter it in the **Passphrase** field.
    
7.  Click **Add** to save the certificate.
    

> \[!INFORMATION\] Learn Postman For a more detailed walkthrough see: [Learn](https://learning.postman.com/docs/sending-requests/authorization/certificates/#add-a-client-certificate) [postman.com](//postman.com) [- Add a client certificate](https://learning.postman.com/docs/sending-requests/authorization/certificates/#add-a-client-certificate)

##### **Verifying the Certificate**

*   Make a request to the corresponding domain.
    
*   Check the response headers to confirm that the certificate was used.
    
*   If there's an issue, ensure that **SSL certificate verification** is enabled in **Settings → General**.
    

##### **Step 4: Configuring Additional Settings**

*   **Enable SSL Verification:** Make sure `SSL certificate verification` is enabled by selection **Settings → General**.
    
*   **Set Environment Variables:** Use environments to manage API keys and base URLs - _This should be preconfigured by the team._
    
*   **Enable Proxy Settings:** Make sure **system proxy** is enabled.
    

##### **Step 5: Using Collections and Workspaces**

*   **Collections** help organize API requests.
    
*   **Workspaces** allow collaboration with team members.
    
*   Check that your postman account has the collection or collections relating to the Rio-Api-Middleware service.
    

!\[\[Pasted image 20250317154310.png\]\]

> \[!WARNING\] POC\_RiO-Connector Collection This postman collection is legacy and should not be referenced or used.

##### **6\. Making a Request from Postman**

Now that Postman is set up to authenticate to the Rio API web service, we can run test requests through Postman. How you define and make a request depends on whether you are consuming the SOAP API endpoints or the REST API endpoints provided by the Rio Integration Hub (web service).

###### **SOAP Requests**

1.  **Find a SOAP operation** that satisfies your process requirement, either from the **Rio API Documentation** or by opening the **RiO API WSDL ApiService V40** collection.
    
    *   Look through the predefined list of requests. In this example, we'll focus on `/authenticate`.
        
2.  **Duplicate the Request**
    
    *   Right-click **Post Authenticate** and choose **Duplicate** or press `Ctrl + D`.
        
    *   Once the duplicate request appears below the original, move it to either a new collection or to the **Rio v4 REST** collection.
        
    *   Rename the duplicate to something more meaningful, e.g., _Authenticate - Testing Local_.
        
3.  **Configure Headers**
    
    *   Ensure the following custom headers and values are listed under the **Headers** tab:
        

```code-line
Content-Type: text/xml; charset=utf-8
SOAPAction: http://{REDACTED}.com/API/4.0/{ServiceContract}/Authenticate
Accept: */*

```

1.  **Define the Request Body**
    
2.  Select the **Body** tab.
    
3.  Choose _raw_ format for the editor preview.Fill in the required SOAP envelope parameters. For **Authentication**, the request body should look like this:
    

```code-line
<?xml version="1.0" encoding="utf-8"?>
<s:Envelope xmlns:s="http://schemas.xmls.org/s/envelope/">
<s:Header>
<Authentication xmlns="urn:cse-....:core" xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns:z="http://cse-..." z:Id="i1">
<PlatformType>{REDACTED}</PlatformType>
<SystemName>{REDACTED}</SystemName>
</Authentication>
</s:Header>
<s:Body>
<Authenticate xmlns="http://cse-...com/API/4.0">
<authenticationType>{REDACTED}</authenticationType>
<token/>
<userName/>
<password/>
</Authenticate>
</s:Body>
</s:Envelope>

```

> \[!NOTE\] When reviewing the WSDL - SOAP `<tag>` Keys You may see tag names that start with `SOAP:`, e.g., `<SOAP:Body></SOAP:Body>`. In the actual request, this is often shortened to `s:`. This formatting is required when calling a SOAP operation endpoint.

1.  **Send the Request**
    
    *   Confirm the SOAP Operation envelope.
        
    *   Once satisfied with the XML definition, press **Send**.
        
    *   When the Rio SOAP API responds, you will receive a response:
        
    *   If an error occurs, a valid HTTP status code will be returned.
        
    *   If there is an internal service error (Rio backend), a SOAP error code will be returned under a **200 OK** status.
        
    *   See **\[SOAP Response Codes\]** for more information.
        

###### **REST Requests**

1.  **Install and Open Postman**
    
    *   Download and install Postman from the official website: [https://www.postman.com/](https://www.postman.com/)
        
    *   Launch the application on your Windows machine.
        
2.  **Create a New Request**
    
    *   Click **"New"** in the top left corner.
        
    *   Select **"Request"** from the available options.
        
    *   Enter a request name and assign it to a collection (optional but recommended).
        
3.  **Configure the Request**
    
    *   Choose the **HTTP method** (`GET`, `POST`, `PUT`, `DELETE`, etc.) from the dropdown next to the request URL field.
        
    *   Enter the **request URL** (endpoint) in the input field.
        
4.  **Add Headers** (If Required)
    
    *   Navigate to the **Headers** tab.
        
    *   Add key-value pairs for required headers:
        
    
    ```code-line
    Content-Type: application/json
    Authorization: Bearer <token>
    
    ```
    
5.  **Add Query Parameters** (If Needed)
    
    *   Click on the **Params** tab.
        
    *   Add key-value pairs for any query parameters required by the API.
        
6.  **Provide a Request Body** (For `POST`, `PUT`, `PATCH` Requests)
    
    *   Go to the **Body** tab.
        
    *   Select **raw** and set the format to **JSON** (or other required format).
        
    *   Enter the JSON payload for the request:
        
7.  **Authentication** (If Required)
    
    *   Navigate to the **Authorization** tab.
        
    *   Select the required authentication type (`Bearer Token`, `Basic Auth`, `API Key`).
        
    *   Enter the necessary credentials or token.
        
8.  **Send the Request**
    
    *   Click the **Send** button.
        
    *   Review the response in the lower panel.
        
9.  **Save the Request**
    
    *   Click **Save** to store the request in a collection for future use.
        
10.  **Review Response and Debug**
    

*   Examine the **status code**, response body, and headers.
    
*   If needed, modify the request and resend.
    

###### **Useful Postman Keyboard Shortcuts**

| Action | Shortcut |
| --- | --- |
| Open a new request | `Ctrl + T` |
| Send request | `Ctrl + Enter` |
| Save request | `Ctrl + S` |
| Open Postman Console | `Ctrl + Alt + C` |
| Switch between tabs | `Ctrl + Tab` |
| Close tab | `Ctrl + W` |

###### **Notes**

*   Use **Postman Console** (`View > Console`) to debug requests.
    
*   Collections can help organize related API requests.
    
*   Environment variables can be used for managing different API environments (dev, test, production).
    

### **Portal (Cloud)**

*   \[ \] Brief guide to using the Azure portal for testing and debugging.
    

### **Local Development Tips**

> \[!TIP\] Optional but very useful VS Code extensions

> See other \[\[Optional VS Code extensions\]\] that are useful for working with the Rio Api Middleware codebase.

## **Documentation**

1.  📍 _Readme_
    
2.  \[\[Handover Reference\]\]
    
3.  \[\[Rio Middleware - Request flows\]\]
    
4.  \[\[Extending the custom-connector\]\]
    
5.  \[ \] Extending the function-app
    
6.  \[ \] Maintaining Azure API Management
    
7.  \[ \] Extending Azure API Operations
    

## **Contributing**

*   \[ \] Brief guide on how to get involved.
    

## **Contacts**

The following table lists current active Leads, Support Experts and Developers for reaching-out to in situations where _sign-off_, code-reviews, questions & support are required.

| Name | Department | Role | Responsibility | @ |
| --- | --- | --- | --- | --- |
| Ben Grant | Intelligent Automation | Intelligent Automation Manager | Lead | [Ben.Grant@berkshire.nhs.uk](mailto:Ben.Grant@berkshire.nhs.uk) |
| Stefan Vaughan | Intelligent Automation | Intelligent Automation Delivery Manager | Lead | [stefan.vaughan@berkshire.nhs.uk](mailto:stefan.vaughan@berkshire.nhs.uk) |
| Josh Davey | 3rd Party Consultant - _Digpacks_ | Automation Intergration Lead / Developer Consultant | Codebase, Custom Connectors (Power Automate), DevOps | [josh@digpacks.co.uk](mailto:josh@digpacks.co.uk) |
| Sam Hutchings | Server Support | Senior Systems Engineer | Infrastructure, IAC | [sam.hutchings@berkshire.nhs.uk](mailto:sam.hutchings@berkshire.nhs.uk) |
| Alex Leach | Intelligent Automation | API Developer | Codebase, Function-app, Azure Serverless Instances, DevOps | [Alex.Leach@berkshire.nhs.uk](mailto:Alex.Leach@berkshire.nhs.uk) |