---
layout: post
title:  "Active Directory Improvements"
date:   2013-08-13
author: "Sean McLellan"
tags: ["Active Directory Bundle"]
---

I've just checked in an update to the AD Bundle.

Groups can now be retrieved via:

```javascript
var ad = require("Active Directory");

var gp = ad.getADGroup("Administrators");

gp;
```

```json
{
    "rawSid": "01020000000000052000000020020000",
    "sId": "S-1-5-32-544",
    "name": "Administrators",
    "displayName": null,
    "members": [
        "CN=WDeployAdmin,CN=Users,DC=myDomain,DC=local",
        "CN=SP_WorkerProcess,CN=Users,DC=myDomain,DC=local",
        "CN=Domain Admins,CN=Users,DC=myDomain,DC=local",
        "CN=Enterprise Admins,CN=Users,DC=myDomain,DC=local",
        "CN=Administrator,CN=Users,DC=myDomain,DC=local"
    ]
}
```

there are two new functions on the group object, expandUsers and expandGroups.. they do what they say..

```javascript
var ad = require("Active Directory");

var gp = ad.getADGroup("Administrators");

gp.expandUsers();
```


result: 

```json
[
    {
        "rawSid": "010500000000000515000000f79696237988f2d101e1ffba13060000",
        "sId": "S-1-5-21-597071607-3522332793-3137331457-1555",
        "name": "WDeployAdmin",
        "firstName": null,
        "initials": null,
        "lastName": null,
        "displayName": "WDeployAdmin",
        "distinguishedName": "CN=WDeployAdmin,CN=Users,DC=myDomain,DC=local",
        "description": null,
        "office": null,
        "email": null,
        "homePage": null,
        "street": null,
        "poBox": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "userLogonName": null,
        "preWin2kLogonName": "WDeployAdmin",
        "isAccountDisabled": 512,
        "logonCount": 0,
        "passwordLastSet": "2011-11-17T18:50:25.234Z",
        "lastLogon": "1601-01-01T00:00:00.000Z",
        "lastLogoff": "1601-01-01T00:00:00.000Z",
        "badPasswordTime": "1601-01-01T00:00:00.000Z",
        "badPasswordCount": 0,
        "lastSuccessfulInteractiveLogonTime": null,
        "lastFailedInteractiveLogonTime": null,
        "failedInteractiveLogonCount": 0,
        "failedInteractiveLogonCountAtLastSuccessfulLogon": 0,
        "homePhone": null,
        "phoneNumber": null,
        "mobileNumber": null,
        "faxNumber": null,
        "pager": null,
        "ipPhone": null,
        "title": null,
        "department": null,
        "company": null,
        "managerLdap": null,
        "managerName": null
    },
    {
        "rawSid": "010500000000000515000000f79696237988f2d101e1ffba54040000",
        "sId": "S-1-5-21-597071607-3522332793-3137331457-1108",
        "name": "SP_WorkerProcess",
        "firstName": "SP_WorkerProcess",
        "initials": null,
        "lastName": null,
        "displayName": "SP_WorkerProcess",
        "distinguishedName": "CN=SP_WorkerProcess,CN=Users,DC=myDomain,DC=local",
        "description": null,
        "office": null,
        "email": null,
        "homePage": null,
        "street": null,
        "poBox": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "userLogonName": "SP_WorkerProcess@myDomain.local",
        "preWin2kLogonName": "SP_WorkerProcess",
        "isAccountDisabled": 66048,
        "logonCount": 20678,
        "passwordLastSet": "2013-12-08T23:19:51.519Z",
        "lastLogon": "2014-06-06T21:35:44.315Z",
        "lastLogoff": "1601-01-01T00:00:00.000Z",
        "badPasswordTime": "2011-09-26T16:19:54.278Z",
        "badPasswordCount": 0,
        "lastSuccessfulInteractiveLogonTime": null,
        "lastFailedInteractiveLogonTime": null,
        "failedInteractiveLogonCount": 0,
        "failedInteractiveLogonCountAtLastSuccessfulLogon": 0,
        "homePhone": null,
        "phoneNumber": null,
        "mobileNumber": null,
        "faxNumber": null,
        "pager": null,
        "ipPhone": null,
        "title": null,
        "department": null,
        "company": null,
        "managerLdap": null,
        "managerName": null
    },
    {
        "rawSid": "010500000000000515000000f79696237988f2d101e1ffbaf4010000",
        "sId": "S-1-5-21-597071607-3522332793-3137331457-500",
        "name": "Administrator",
        "firstName": "Sean",
        "initials": null,
        "lastName": "McLellan",
        "displayName": "Sean McLellan",
        "distinguishedName": "CN=Administrator,CN=Users,DC=myDomain,DC=local",
        "description": "Built-in account for administering the computer/domain",
        "office": null,
        "email": "testemail@testemail.com",
        "homePage": null,
        "street": null,
        "poBox": null,
        "city": null,
        "state": null,
        "zip": null,
        "country": null,
        "userLogonName": null,
        "preWin2kLogonName": "Administrator",
        "isAccountDisabled": 66048,
        "logonCount": 1033,
        "passwordLastSet": "2011-08-02T22:15:30.531Z",
        "lastLogon": "2014-06-06T15:08:02.791Z",
        "lastLogoff": "1601-01-01T00:00:00.000Z",
        "badPasswordTime": "2014-06-02T20:13:39.950Z",
        "badPasswordCount": 0,
        "lastSuccessfulInteractiveLogonTime": null,
        "lastFailedInteractiveLogonTime": null,
        "failedInteractiveLogonCount": 0,
        "failedInteractiveLogonCountAtLastSuccessfulLogon": 0,
        "homePhone": "112345",
        "phoneNumber": "12345",
        "mobileNumber": "12345",
        "faxNumber": "12345",
        "pager": "12345",
        "ipPhone": "12345",
        "title": "Blah",
        "department": "blue",
        "company": null,
        "managerLdap": null,
        "managerName": null
    }
```