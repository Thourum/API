# Howdy API Documentation
> These page contains samples and specifications on how to integrate Howdy with HR-systems in order to maintain employee data.

# Endpoints

**Test:**  https://wlb-uat-ne1-api.azurewebsites.net/

**Production:**  https://api-ne1.worklifebarometer.com/

# Security
All calls to the API **must be authenticated** by presenting a valid JWT ([JSON Web Token](https://jwt.io/)).

To obtain the token do the following:
- Access **Howdy Portal** on the following url: *https://auth.worklifebarometer.com/*
  - In case you already have access to the Test Environment choose the desired Environment
- Navigate to *Company* > *Integration* and **Turn On External Integration**
  - Turn on **Batch limit** and set the max difference to 25 (Recommended)
- Navigate to *Access control* and look for a user with the name "&lt;YourCompanyName&gt; API User" (if you don't see it, please contact support at support@worklifebarometer.com)
  - The API user should have the role HRIntegration. Click the three dots and then "Generate new App Token"
  - This should open a dialog box with a newly generated token
  - Keep the token secure

The Token can be sent in one of two ways:
- **HTTP Header**: Add it as header `Authorization: Bearer <API_TOKEN_HERE>` to each request
- **Query String parameter**: Pass it as parameter `?access_token=<API_TOKEN_HERE>` to url for each request

# Integration

**External Integration**

Allows the integration of Howdy with HR-systems in order to maintain employee data.
Creates a User with the Role **HRIntegration** (see the user in *Access control* in Howdy Portal)
Following the steps above results in Authentication token used in **ALL** Api calls

**Test Environment**

Test Environment (also called *UAT*) is used to test new features and resets once a day, meening any and all changes made to the system are overwriten by the data used in production.

**Batch limit**

Percent of allowed change per one request
E.g. Batch limit of **25** and *100 Users in company* results in maximum of 25 addition or subtractions

**Automatically invite**

If **InvitationDate** is not set and this is enabled everytime a new user is added an invitation is send.

# API
## Get Employees
This call returns all employees that were created through the API. Employees created through the portal interface will not be included.

**URL** : `/v1.0/Company/{companyId}/Employee`

**Method** : `GET`

**Auth required** : YES

### Success Response

**Code:** `201 OK`

```json
[
  {
    "Email": "test@example.com",
    "Birthday": null,
    "Gender": 1,
    "ExternalId": "12345678",
    "ContactNumber": null,
    "HomeAddress1": null,
    "HomeAddress2": null,
    "HomeZipCode": "1000",
    "HomeCity": null,
    "HomeCountry": null,
    "LanguageId": 1045,
    "JobTitle": null,
    "HRContact": null,
    "HealthOffer": null,
    "NotifyByEmail": true,
    "NotifyBySMS": false,
    "NotifyByPush": true,
    "Dimensions": {
      "ImmediateManager": "John Doe",
      "Department": "2551",
      "Role": "1",
      "division": "Test"
    },
    "Position": null,
    "ResponseCenterId": null,
    "ResponseCenterName": null,
    "EmploymentStatus": 0,
    "Locked": false,
    "OptOut": false,
    "SourcedFromExternalSystem": true,
    "FirstInvitationDate": "2018-11-15T11:00:16.3627689",
    "EmployedOn": null,
    "Id": 0254799,
    "Firstname": "David",
    "Lastname": "Rasmussen",
    "Fullname": "David Rasmussen",
    "CompanyId": 1000,
    "EmployeeID": "12345678"
  },
  {
  ...
  }
]
```

### Error Response

**Code:** `401 Unauthorized`

```json
{
  "Message": "Authorization has been denied for this request."
}
```



## Put Employees
This call makes a complete set based change of all employees in the system.

**URL** : `/v1.0/Company/{companyId}/Employee`

**Method** : `PUT`

**Auth required** : YES

**Note**

> If employee `[A, B, C]`exists in Howdy and employee `[B, C, D]` are sent via this call then:
>- **A** will be removed
>- **B** and **C** will be updated with the values provided
>- **D** will be added

> **InvitationDate**
>- Only for new employees. 
>- If obmitted then then invitation will be sent immediately or at 8 o'clock if outside business hours

**Data constraints**

| Field                | Type      | Requirements                                      | Description                                                     |
| -------------------- | --------- | ------------------------------------------------- | --------------------------------------------------------------- |
| `EmployeeID`\*       | `String`  | `Unique`, *Max length: 50*                        | Your internal primary key                                       |
| `InvitationDate`     | `String`  | *Format:* `yyyy-MM-ddTHH:mm:ssZ`                  | Date and time when to sent out invitation. See Note\* |
| `Firstname`\*        | `String`  | *Max length:* **150**                             | Firstname of the employee                                       |
| `Lastname`\*         | `String`  | *Max length:* **150**                             | Lastname of the employee                                        |
| `Phonenumber`        | `String`  | `Unique`, *Must match regex:* `^\+[0-9]{6,20}$`   | Cell phone E.g. +4523232323                                     |
| `Email`\*            | `String`  | `Unique`, Valid email                             | E-mail address                                                  |
| `Gender`             | `Integer` | Values: `0` = Male, `1` = Female, `9` = Not known | Gender                                                          |
| `EmploymentStatus`\* | `Integer` | `0` = "Active",  `1` = "on leave"                 | Employment Status                                               |
| `JobTitle`           | `String`  | *Max length:* **50**                              | Users Role in the company. E.g. "Sales Manager" or "CEO"        |
| `Department`         | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Role`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `ImmediateManager`   | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim1`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim2`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim3`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim4`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim5`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim6`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim7`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim8`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim9`               | `String`  | *Max length:* **50**                              | Reporting specific data                                         |
| `Dim10`              | `String`  | *Max length:* **50**                              | Reporting specific data                                         |

**\*** Required

### Example
The request consists of an array of Employees based on the model described above.
```http
PUT /v1.0/company/{companyId}/employee HTTP/1.1
Host: <API_ENDPOINT>
Authorization: Bearer <API_TOKEN_HERE>
Content-Type: application/json
Cache-Control: no-cache
 
[
  {
    "EmployeeID": "eb8a4f10-1c24-409f-b680-92ae71253339",
    "Firstname": "Bilbo",
    "Lastname": "Baggins"
    "Email": "8462@dev.test",
    "EmploymentStatus": 0,
    "Phonenumber": "+00008462",
    "Birthday": null,
    "Gender": 9,
    ...
  },
  ...
]
```

### Success Response

**Code** : `200 OK`

**Content example**
```json
{
  "ApiOperationId": "string"
}
```

### Error Response

**Code** : `400 Bad Data`

**Content example**
```json
{
  "ApiOperationId": "string",
  "ValidationErrors": ["string"]
}
```

**Note**

The `ApiOperationId` can be used for further diagnostics so please log this if any errors are returned from the service

 


