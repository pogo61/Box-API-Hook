# Akana API HOOK
![Image of Akana] 
(https://www.akana.com/img/formerlyLOGO8.png) 
[Akana.com](http://akana.com)

## Box-API-Hook
API Hook using SOA Software's Products to Merge both the Box Content and Box Upload API's into a single API, as well as integrate and hide the Box OAuth Auth steps. 

## BOX API 
### About the API
- The Box Content API gives you access to the content management features you see in our web app and lets you extend them for use in your own app.
- API Documentation: [Box Content API docs] (https://developers.box.com/docs/)

### Pre-Reqs
- Create a Box Developer account at [Box Developers] (https://app.box.com/developers/services)
    - Click on the "Create a Box Application" right hand menu item
    - enter the name of the application (any name you wish) into the "Application Name" field displayed, and ensure that the "Box content" radio button is selected
    - Click the "Create Application" button
    - If you are not already taken to the App you just created:
        - Click on the "My Applications" right menu item
        - Click the "Edit Application" button next to your newly created App
    - go to the "OAuth2 Parameters" section
    - Click the "Create a Developer Token" button
    - Copy the generated Token (This token has a 30 minute life)
- you must install the pso extensions custom polices:
    + unzip the com.soa.pso.openapi.extensions_7.2.2.zip (available in this repository) into the <Policy Manager Home>/sm70 directory. This will result in files placed in the sm70/lib/pso.opeapi.extensions_7.2.2 subdirectory
    + restart both PM and ND(s)
    + Using the SOA Admin Console, install the following features in each PM container:
        * SOA Professional Services OpenAPI Extensions
        * SOA Professional Services OpenAPI Extensions UI
    + Using the SOA Admin Console, install the following features in each ND container:
        * SOA Professional Services OpenAPI Extensions

### Getting Started Instructions
#### Download and Import
- Download BoxAPIHook.zip
- Download the migrations.properties file, and edit it to replace the <replace this with your key> text with the "Container Key" of the ND or ND cluster in your target PM.
    - the container key is found by going to the "Deatils Tab" of the ND cluster, or ND defined in the Policy Manager Console, then looking at the " Container Overview" tab on that page, and copying the "Container Key:" value. ![container key screenshot](https://github.com/pogo61/Google-Sheets-API-Integration/blob/master/Screen%20Shot%202015-03-18%20at%2011.24.45%20am.png "ND Container Key")
- Login to PolicyManager  example: http://localhost:9900
- Select the root "Registry" organisation and click on the "Import Package" from the Actions navigation window on the right side of the screen
  - click on button to browse for the BoxAPIHook.zip archive file 
  - make sure select the migrations.properties file 
  - click Okay to start the importation of the hook.
- this will create a Box API Hook Organisation with the requisite artefacts needed to run the API.

#### Verify Import
- Expand the services folder in the Box API Hook you imported and find Box_API_Hook VS

#### Configure Security
- go to your <SOA Home>/sm70/scripts directory and run the encriptData.sh or encriptData.bat file
- enter the token you copied from the Box Developer site
- copy the resultant encrypted token
- Go to PM and select the Box API Hook organisation
- select the Policies -> Operational Policies -> "PolicyVariables" policy 
    - click the "Start New Version" action in the right-hand Actions Portlet
    - Click the "OK" button on the warning dialog
    - Click the "Modify" link on the "XML Policy" tab
    - replace the value in the "auth.token" attribute of the "NameValue" element with the encrypted token you copied from the output of the encriptData script
    - Click the "Apply" button
    - Click the "Activate Policy" action on the Right-Hand Policy Workflow portlet
    - ensure the workflow state is "Active"
- select the Policies -> Operational Policies -> "AddAuthToken" policy
    - Click the "Activate Policy" action on the Right-Hand Policy Workflow portlet
    - ensure the workflow state is "Active"


#### Verify Connectivity
- Using curl http://"URL of the Listener of your ND"/box_api_hook/helloworld
- The correct response should be a JSON object listing the details of the user owning the credentials being used to make the call:
{
    "address": "",
    "avatar_url": "https://app.box.com/api/avatar/large/229214787",
    "created_at": "2014-12-15T01:12:03-08:00",
    "id": "229214787",
    "job_title": "",
    "language": "en",
    "login": "paul.pogonoski@soa.com",
    "max_upload_size": 2147483648,
    "modified_at": "2015-03-16T21:49:50-07:00",
    "name": "Paul Pogonoski",
    "phone": "+61416101363",
    "space_amount": 10737418240,
    "space_used": 790,
    "status": "active",
    "timezone": "Australia/Sydney",
    "type": "user"
}

### How Hello World Works
#### An Akana Integration Primer
The Google_Sheets_API_Hook API is a "Virtual Service". That is, its interface is not that of a real service implementation. It can be a proxy to a "real" implementation, or it can be an aggregate (a combination) of a number of "real" implementations. In Policy Manager a "real" implementation is called a "Physical Service".
Apart from offering a different interface to the Physical Service, a Virtual Service offers the ability to attach Policies for security, logging, QoS, and a number of other non-functional capabilities.
Virtual Services also have the ability to have Custom Process and Scripts run before the Physical Service is called. Here is where a lot of the magic of Integration occurs.

#### Hello World
To create the helloworld operation the following was added to a base RAML document to create the box API Hook HelloWorld.raml document:  
    /helloworld:  
      &nbsp;get:  
        &nbsp;&nbsp;description: "returns details about authorised user"
        &nbsp;&nbsp;&nbsp;responses:  
          &nbsp;&nbsp;&nbsp;&nbsp;200:  
            &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;body:  
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;application/atom+xml:  

Then a VS was created by using the RAML as the definition source.
The root contect of the VS was made exactly the same as the root context of the Box API Hook VS. the consequense of this is that this simple HelloWorld VS and the Hook VS become a single VS.

Then the /helloworld Operation in the VS was mapped to the GET /users/me operation in the Box API Hook PS.

Go to the Box_API_Hook_Helloworld VS -> Operations Tab -> GET /hellowworld operation -> Process tab you'll see this image:
![Helloworld process] 
(https://github.com/pogo61/Box-API-Hook/blob/master/Box%20API%20Hook.png)

Double click on the Process activity to see that it call's the Heloworld Process, which call's an invoke on the GET /users/me operation to make the Hello World operation call successful.


### Create Your Own Integration with the Google Sheets API
The Hello World operation is one simple way of integrating or extending your API's.
Take a look at the [Google Sheets API Integration](https://github.com/pogo61/Google-Sheets-API-Integration).
this will give you a deeper inderstanding of the richness of our gateway product in integrating to API's

### Modify and Build
In the event you need to change the API Hook.   Here are the instructions to do so. 

### License
Put a link to an open source license
