# SAP Form Services by Adobe

## Links
[Help SAP - SAP Forms Service by Adobe](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/sap-forms-service-by-adobe)

## Basic Workflow
### Initial Setup
If you need more details regarding the process [click here](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/getting-started).

To have access to the service which is split into the Form Service and the Form Service API you need to assign the corresponding **entitlements to the global account**.
- **Form Service by Adobe**: default(Application) - Dashboard for managing forms
- **Form Service by Adobe API**: standard - API access

Next navigate to Service Marketplace and:
- **Form Service by Adobe**: Create an application plan
- **Form Service by Adobe API**: Create a service plan

Create a service key for the Adobe API service

Now to be able to connect to an application you need to create a Destination and allow it to be used inside of it through the `xs-app.json` file [like so](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/integrate-rest-api-via-destination-service).
### Design a PDF
Create a PDF using `Adobe LiveCycle` 
Setup page size by clicking on `Page1` in the Hierarchy and navigating to it's Object details.
### Upload a Template
Navigate to `Form Service by Adobe` and change the path to `/adsrestapi/ui.html`.
Press `Create Form` and specify a name
Then upload a template and specify the name, xdp file, language and inside the optional data the locale.
### Use the API
[read more here](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/integrate-rest-api-via-destination-service)

## Design
### Change the page size & orientation
With masterpage selected: Object > Masterpage
### Make tables overflow to next page
With page subform selected:  Object > Subform > Content: Flowed
