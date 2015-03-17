# Box-API-Hook
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

### Getting Started Instructions
#### Download and Import
- Download BoxAPIHook.zip
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

### Modify and Build
In the event you need to change the API Hook.   Here are the instructions to do so. 

### License
Put a link to an open source license
