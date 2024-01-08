# SAP BTP

## Naming things
There is an unverified & untested character limit of 50 characters. If your MTA application for example exceeds 50 characters when appending the destination service and xsuaa authentication service then it might be a good idea to name it something else.

Trust me you don't want to end up debugging service based issues.

## Destination issues
If you see 403 while doing a POST or 404 using GET while requesting data, you might have one if not all of the following issues: 

### Bad source in xs-app
If the regex of the source is below a broader regex, it will fail to reach it.
```json
{
  "source": "^/ads/(.*)$",
  "target": "/$1",
  "authenticationType": "xsuaa",
  "csrfProtection": false,
  "destination": "ads"
}
```

### Invalid name
Don't use special characters in the destination name
### Wrong MTA destination specification
If the name of the instances are truncated within BTP you might need to grab destinations from instances instead of subaccount. (The default is instance but just in case...)
```mta
parameters:
	content:
		instance: // change from subaccount if truncated
			destinations:
```

## Managing roles for a project
### Frontend Launchpad
Visibility of applications.

Launchpad creates Roles entries that contain
Name: Idetifier
ID: SAP BTP Role ID
Apps and Plugins: All applications that become visible when the user account holds the SAP BTP Role.

### Frontend Controller
Visibility of elements within applications.
Roles of a user can only be checked through the backend (with the [following exception](https://answers.sap.com/questions/13950708/get-the-roles-of-the-logged-in-user-in-my-sapui5-a.html)).

*In previous applications I've seen a User function that returns a JSON with the roles of the user by checking [`req.user.is`](https://cap.cloud.sap/docs/node.js/authentication#user-id) and adding them to an array. Thereafter the client can send a request on this function and control the visibility.*
### Service Definitions
Visibility of services.

### BTP Roles
Definition of roles, role collections and user setup.
*(Italian: Uff. - Role Collection)*