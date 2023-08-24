---
title:  Create Devops Test Run
#author: Jan Helberg
date:   2023-08-24 20:00:00 +0200
categories: [Software, Devops, Test run]
tags: [Software, Devops, TestRun, Postman, Association, Pipelines]
---
## Why I needed to Create a Devops Test run programatically
We had Postman tests, these tests were run in a pipeline, We needed to report on the test cases that was run by assosiating the test runs to test cases\
**Note: Unfortunately, Postman does not provide a direct way to assign the value of a test assertion (pm.test) to a variable, The closest we can get is to set the value to passed if the test has run

## Create a PAT token
A Personal Access Token (PAT) is a security token used to authenticate and authorize access to resources on online platforms or systems\
We need a PAT token to talk to Devops. After creating a PAT token set the Authorization to Basic Auth, And add the token to the password field, no need for a username\
<a href="https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Windows#create-a-pat" target="_blank">Create a PAT</a>

## Test Points? 
What is a test point?\
Test cases by themselves are not executable. When you add a test case to a test suite then test point(s) are generated. A test point is a unique combination of test case, test suite, configuration, and tester.\
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/points?view=azure-devops-rest-7.0" target="_blank">Test Points</a>

## Get Test Points
Firstly we need to get the Test Points, Later we use them when creating the test run
```powershell
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Authorization", "Basic dummytoken41d8cd98f00b")
$response = Invoke-RestMethod 'https://dev.azure.com/{organization}/{project}/_apis/test/Plans/{PlanID}/Suites/{SuiteID}/points/?api-version=7.0' -Method 'GET' -Headers $headers
$response | ConvertTo-Json
```
Storing the ID's in a environment variable 'PointIDs', This code needs to be placed in Tests tab
```bash
const jsonData = pm.response.json();
// Extract the IDs from the "value" array
const ids = jsonData.value.map(item => item.id);
// Convert the array of IDs to a comma-separated string
const idsString = ids.join(',');
// Store the comma-separated string of IDs in a Postman variable
pm.environment.set('PointIDs', idsString);
```

## Create a Test Run
We need to give a unique test run name, I prefer to add the Date and time the test run ran, In the pre-request script tab add this:
```bash
var moment = require('moment');
pm.environment.set('currentdatetime', moment().format(("YYYY-MM-DD-HH-mm:ss")));
```
{% raw %}
```powershell
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")
$headers.Add("Authorization", "Basic dummytoken41d8cd98f00b")
$body = @"
{
  "name": "Postman Test Run - {{currentdatetime}}",
  "Automated": true,
  "plan": {
    "id": "{{PlanID}}"
  },
  "pointIds": [
    {{PointIDs}}
  ]
}
"@

$response = Invoke-RestMethod 'https://dev.azure.com/{organization}/{project}/_apis/test/runs?api-version=7.0' -Method 'POST' -Headers $headers -Body $body
$response | ConvertTo-Json
```
{% endraw %}
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/runs/create?view=azure-devops-rest-7.0" target="_blank">Create a Test Run</a>

## Get assertion result
Append to the environment variable if test has run

```powershell
let testResult = false;

// Define assertions using pm.test
pm.test("Response status is 200 OK", function () {
    pm.response.to.have.status(200);
    assertionResult = true;
});

// Get the current value of the testResults
let TestResults = pm.environment.get('testResults');
// Add a new value separated by a comma
let newTestResults = TestResults + ',' + assertionResult;
// Update the environment variable with the new value
pm.environment.set('testResults', newTestResults);
```

## Update Test Run
Update Test Run

Get the TestID's, In the pre-request script tab add this:
```powershell
// Get the comma-separated values from the environment variable
const PointIdsArray = pm.environment.get('PointIDs');
console.log(PointIdsArray)
// Split the values into an array using the comma as a delimiter
const valuesArray = PointIdsArray.split(',');
console.log(valuesArray[0])
```
Add the values in the request
```powershell
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")
$headers.Add("Authorization", "Basic dummytoken41d8cd98f00b")
$body = @"
[
    {
        `"id`": 100000,
        `"state`": `"Completed`",
        `"outcome`": valuesArray[0]
    }
]
"@

$response = Invoke-RestMethod 'https://dev.azure.com/{organization}/{project}/_apis/test/Runs/{{TestRunID}}/results?api-version=7.0' -Method 'PATCH' -Headers $headers -Body $body
$response | ConvertTo-Json
```
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/results/update?view=azure-devops-rest-7.0" target="_blank">Update Test Run</a>

## Close Test Run
After we are complete with the test run we need to set it to completed

```powershell
$headers = New-Object "System.Collections.Generic.Dictionary[[String],[String]]"
$headers.Add("Content-Type", "application/json")
$headers.Add("Authorization", "Basic dummytoken41d8cd98f00b")
$body = @"
{
    `"state`" : `"Completed`"
}
"@
Invoke-RestMethod 'https://dev.azure.com/{organization}/{project}/_apis/test/runs/{{TestRunID}}?api-version=7.0' -Method 'PATCH' -Headers $headers -Body $body
```
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/runs/update?view=azure-devops-rest-7.0" target="_blank">Close Test Run</a>

## Reference
<a href="https://learn.microsoft.com/en-us/azure/devops/organizations/accounts/use-personal-access-tokens-to-authenticate?view=azure-devops&tabs=Windows#create-a-pat\" target="_blank">Create a PAT</a>\
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/points?view=azure-devops-rest-7.0\" target="_blank">Test Points</a>\
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/runs/create?view=azure-devops-rest-7.0\" target="_blank">Create a Test Run</a>\
<a href="https://learn.microsoft.com/en-us/rest/api/azure/devops/test/results/update?view=azure-devops-rest-7.0\" target="_blank">Update Test Run</a>\
<a href="https://141gaurav.medium.com/update-postman-test-scripts-results-in-azure-devops-d3e1388845c5" target="_blank">Reference Article</a>
