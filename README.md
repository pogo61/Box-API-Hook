# Box-API-Hook
API Hook using SOA Software's Products to integrate lead generation into BOX
## BOX API 
### About the API
- The Box Content API gives you access to the content management features you see in our web app and lets you extend them for use in your own app.
- API Documentation: [Box Content API docs] (https://developers.box.com/docs/)

### Pre-Reqs
- Create an Account with [SmartyStreet] (https://smartystreets.com)
- Login, click on the "API Keys" left menu item to Obtain the Auth ID and Auth Token (they are under the "Secret Keys" section)
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
  - click on button to browse for the GoogleSheetsHookAPI.zip archive file 
  - make sure select the migrations.properties file 
  - click Okay to start the importation of the hook.
- this will create a Box API Hook Organisation with the requisite artefacts needed to run the API.
- Create a leads.txt file in the "/sm70/instances/nd "directory

#### Verify Import
- Expand the services folder in the Google Sheets API Hook you imported and find TrialLeads_API_vs1 VS

#### Configure Security
- select the TrialLeads_API_vs1 VS
- select the "Operations" tab, then the "POST /leads" operation, then the "Process" tab for the operation.
- Double click on the "ConfigureSmartyStreet" script activity 
- Put in your Auth ID value into the "newQueryString" after the "auth-id=" and Auth Token after the "auth-token="
- save the script activity
- select the save process icon
- Select the "PostLeadToBox" process in the Processes branch
- Select the "Process" tab
- Double click on the "Initialise Bearer Token" script activity
- replace the value in the "processContext.setVariable("Bearer","BAJKzHbVsE7o66zIJ0iGXa3gU4sdRIRw");" line with the token you copied from the Box Developer site
- Click the Finish button
- Click the save process icon


#### Verify Connectivity
- Using curl -H "Content-Type: application/json" -d '{"FirstName":"Paul","LastName":"Pogo40","Company":"SOA","Street":"2962 Rosemary LN NE","City":"Rochester","State":"MN","PostalCode":"55906","Email":"paul40@soa.com"}' http://Win7-64-vm:9905/leads_vs1/leads (Make sure you use a unique value for both the Email and LastName values)
- The correct response should be:
{"FirstName":"Paul","LastName":"Pogo40","Company":"SOA","Street":"2962 Rosemary LN NE","City":"Rochester","State":"MN","PostalCode":"55906","Email":"paul40@soa.com","SalesForceId":"00Q60000017Ar19EAC","SignUpStatus":{"salesForceStatus":"Success","MailGunStatus":"Success","Box":"Success"}}
- Log in to your Box Account [Box] (https://app.box.com)
- ensure that there is a "Lead" folder.
- open the "Lead" folder and ensure there is a "leads.txt" file
- Open that file, wait for the preview to generate the contents, and ensure that your lead posted in the curl request has been appended to the bottom of the file.

### Modify and Build
In the event you need to change the API Hook.   Here are the instructions to do so. 

### License
Put a link to an open source license
