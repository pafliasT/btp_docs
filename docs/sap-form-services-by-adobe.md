# SAP Form Services by Adobe Guide 📝

Learn how to set up and use the SAP Form Services by Adobe to create and manage PDF forms easily. This guide includes initial setup, designing PDFs, uploading templates, and utilizing the API for advanced integration.

## Useful Links 🔗

- **SAP Forms Service by Adobe Documentation:** [Help SAP - SAP Forms Service by Adobe](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/sap-forms-service-by-adobe)

## Basic Workflow 🔄

### Initial Setup ⚙️

For detailed setup instructions, [click here](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/getting-started).

Assign the necessary **entitlements to your global account** to access:

- **Form Service by Adobe**: `default(Application)` - A dashboard for managing forms.
- **Form Service by Adobe API**: `standard` - API access for programmatic control.

Then, in the Service Marketplace:

- For **Form Service by Adobe**: Create an application plan.
- For **Form Service by Adobe API**: Create a service plan.

**Create a service key** for the Adobe API service to authenticate your requests.

To connect to an application, create a Destination and configure it in the `xs-app.json` file for use. Detailed instructions can be found [here](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/integrate-rest-api-via-destination-service).

### Design a PDF 🎨

Utilize `Adobe LiveCycle` to create your PDF. Adjust the page size by selecting `Page1` in the hierarchy and modifying its Object details.

### Upload a Template 📤

1. Go to `Form Service by Adobe` and navigate to `/adsrestapi/ui.html`.
2. Click `Create Form`, specify a name, then upload your template.
3. Define the template name, xdp file, language, and in the optional data, the locale.

### Use the API 🚀

For integrating the API into your application for dynamic form handling, [read more here](https://help.sap.com/docs/forms-service-by-adobe/sap-forms-service-cf/integrate-rest-api-via-destination-service).

## Design Tips ✨

### Change Page Size & Orientation 🔧

To adjust, with the master page selected, go to: Object > Masterpage.

### Enable Table Overflow to Next Page 📊

For tables that extend beyond one page, with the page subform selected, navigate to: Object > Subform > Content: Flowed.

## Conclusion

By following these steps and utilizing the design tips, you can effectively create and manage PDF forms using SAP Form Services by Adobe, enhancing your document workflows and user interactions. Happy designing! 🎉
