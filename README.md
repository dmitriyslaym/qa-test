# Requirement
Implement an automational test based on the scenario from https://github.com/dmitriyslaym/qa-test/blob/main/spec.feature with the help of Cypress tool (https://www.cypress.io).
The result should be provided in a git repository with two commands available:
- run in debug mode (when Cypress opens the browser)
- run in headless mode (suitable for running in CI)

The preparation and usage of reusable commands is highly appreciated since the usage of Admin REST API and Exchange WS Gateway is something that will be repeated in the majority of future tests.

Also it is highly recommended to make sure that the test is easy to debug (leave understandable logs or use any other technics that you're aware of to simply the process of debugging the test).

### Attention!
Admin part of the test should be implemented both using REST API and UI (you can implement one test where everything in Admin functionality has been done within REST API and the other one where everything is done from UI).

## Admin part
In order to create the resources for trading you should use admin functionality of the system.
There are two ways to use it:
- REST API (origin is https://admin-api-master.ops.rnd.exberry.io/api)
- UI Application (https://admin.master.dev.exberry.io)

Documentation for the REST API (note, that in the examples there is a different URL origin, you should use "https://admin-api-master.ops.rnd.exberry.io/api")
https://documenter.getpostman.com/view/6229811/TzCV3jcq#intro

### Login

Credentials to access the functionality for both ways are the following:
```
{
  "email": "qacandidate@gmail.com",
  "password": "p#xazQI!Y%z^L34a#"
}
```
#### UI application
Pass those credentials to the Universal Login form - and you will be logged in and redirected to the main page.

#### REST API
Use Get Token route (POST "/auth/token") and then include the received JWT token in the "Authorization" header of each request to a protected route (make sure that the value of this header always starts with "Bearer " and then the token).

### Create Calendar
#### UI application
Go to Calendars page, click on "Add new" button on the right top part of the screen. Fill the required and submit the form - newly created calendar should appear in the table.

#### REST API
Use Create Calendar protected route (POST "/v2/calendars").

### Create Instrument
#### UI application
Go to Instruments page, click on "Add new" button on the right top part of the screen. Fill the required inputs (in the Calendar input select the Calendar that you have created in the previous step) and submit the form - newly created instrument should appear in the table.

#### REST API
Use Create Instrument protected route (POST "/v2/instruments").

### Create MP
#### UI application
Go to Market Participants page, click on "Add new" button on the right top part of the screen. Fill the required inputs and submit the form - newly created MP should appear in the table.

#### REST API
Use Create MP protected route (POST "/mps").

### Create APIKey for MP
#### UI application
On Market Participants page, click on "Add APIKey" button of the MP that you have just created. Select all the permissions and generate APIKey. You will see APIKey and Secret - save this data since it will be hidden forever once you close the modal.

#### REST API
Use Create APiKey protected route (POST "/mps/:mpId/api-keys").

## Exchange GW (Trading) part
GW is available with the following DNS:
wss://exchange-gateway-master.exchange.rnd.exberry.io
You can use Sandbox application with the list of methods that you will need in this scenario - there you can send requests and see all the responses.
https://sandbox.exberry.io/?url=https://raw.githubusercontent.com/dmitriyslaym/qa-test/main/exchange-gw-sandbox-data.json

### Create session
#### Sandbox application
- In "Message Builder" section put the APIKey and Secret values of the APIKey that you have just generated.
- In "Message Builder" section in Timestamp input click on "Refresh" icon.
- In "Message Builder" section click on "Build" button.
- Notice, that the content of JSON block in the middle was updated. Click on "Send" button there. If everything is successfull, from now on you can use any endpoints from the list while having the current WS connection opened.

#### In nodeJS code
```
import sha256 from "crypto-js/hmac-sha256";

const apiKey = 'your-api-key';
const secret = 'your secret-from-api-key';
const signature = sha256(`"apiKey":"${apiKey}","timestamp":"${String(Date.now())}"`, secret).toString()
```

### Place order
#### Sandbox application
Select "placeOrder" method within TRADING API section on the left corner. In JSON block use the required values of the props in the "d", click "Send".

### Execution reports and Traders
#### Sandbox application
Select those methods within PRIVATE DATA API section on the left corner. No need to change anything in JSON block, just click "Send".

## General recommendations

It is recommended firstly to manually use Admin UI application and Sandbox application (for Trading functionality) in order to better understand the business logic and observe how the system behaves. After that the implementation of the scenario becomes a matter of technical skills.

Don't hesitate to ask questions/clarifications if you stuck. Good questions will not reduce your chances during the evaluation, but the lack of understanding and the lack of interest actually will.

Good luck!
