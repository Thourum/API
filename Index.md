---
layout: default
title: Home
nav_order: 1
permalink: /
---
# Howdy API Documentation
{: .fs-9 }

These page contains samples and specifications on how to integrate Howdy with HR-systems in order to maintain employee data.
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 } [View it on GitHub](https://github.com/WorklifeBarometer/API){: .btn .fs-5 .mb-4 .mb-md-0 }

---

# Getting started

- [C# Sample code](./Samples/ActiveDirectoryExample/ActiveDirectoryExample)

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