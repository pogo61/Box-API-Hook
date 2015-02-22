# Box-API-Hook
API Hook using SOA Software's Products to take a string input and create a file from the input, then store the file in BOX
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
- Expand the services folder in the Google Sheets API Hook you imported and find Box_API_Hook VS

#### Configure Security
- select the Box_API_Hook VS
- select the save process icon
- Select the "PostFileToBox" process in the Processes branch
- Select the "Process" tab
- Double click on the "Initialise Bearer Token" script activity
- replace the value in the "processContext.setVariable("Bearer","BAJKzHbVsE7o66zIJ0iGXa3gU4sdRIRw");" line with the token you copied from the Box Developer site
- Click the Finish button
- Click the save process icon


#### Verify Connectivity
- Using curl -X PUT  -H "Content-Type: application/json" -d '"hello world"' http://"URL of the Listener of your ND"/box/file
- The correct response should be:
{"status":"Success"}
- Log in to your Box Account [Box] (https://app.box.com)
- ensure that there is a "API_HOOK" folder.
- open the "API_HOOK" folder and ensure there is a "input.txt" file
- Open that file, wait for the preview to generate the contents, and ensure that your input text posted in the curl request has been inserted into the file. In this case - "Hello World"

### Modify and Build
In the event you need to change the API Hook.   Here are the instructions to do so. 

### License
Put a link to an open source license
