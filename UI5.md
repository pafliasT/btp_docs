## Map
1. [Nice To Haves](#Nice%20To%20Haves)
	1. [Getting the URL of a resource](#Getting%20the%20URL%20of%20a%20resource)
	1. [Getting a Manifest entry](#Getting%20a%20Manifest%20entry)
 1. [Models](#Models)
 1. [Binding](#Binding)
 1. [Routing](#Routing)
 1. [Fragmentation (Code Cleanup)](#Fragmentation%20(Code%20Cleanup))
 1. [Controls](#Controls)
 1. [Smart Controls](#Smart%20Controls)
 1. [Comparisons](#Comparisons)
 1. [External Links](#External%20Links)
---
## Nice To Haves
### Getting the URL of a resource
```js
jQuery.sap.getModulePath("path/of/module/from/root"); // DEPRECATED
sap.ui.require.toUrl("path/of/module/from/root");
```
[deprecation note](https://ui5.sap.com/#/topic/a075ed88ef324261bca41813a6ac4a1c)
### Getting a Manifest entry
```js
Controller.getManifestEntry("/path/to/entry");
```
*Get the namespace using `/sap.app/id`, combine with [Getting the URL of a resource](#Getting%20the%20URL%20of%20a%20resource) to get the URL of the project.

---
## Models
There are three main models:
- **JSON**: Quick to work with and access.
- **OData v2**: Easier to manage requests from.
- **OData v4**: Asynchronous callback based setup.

Models can be created during runtime or they can be specified inside the `manifest.json`.
In the manifest you can see the sources of each service defined in `sap.app.dataSources` where each entry. Each `dataSource` can then be defined in the `sap.ui5.models` as a Model that we can see within the application.

### JSON
[`getJSON()`](https://ui5.sap.com/#/api/sap.ui.model.json.JSONModel%23methods/getJSON) Gets all the JSON data.
[`setJSON()`](https://ui5.sap.com/#/api/sap.ui.model.json.JSONModel%23methods/setJSON) Gets all the JSON data.
[`getProperty()`](https://ui5.sap.com/#/api/sap.ui.model.json.JSONModel%23methods/getProperty) Get a specific property of a JSON model. *Starts with a / if it's not relative.*
[`setProperty()`](https://ui5.sap.com/#/api/sap.ui.model.json.JSONModel%23methods/setProperty) Set a specific property of a JSON model.
### OData v2
Service CURD:
[`create(sPath, oData, mParameters)`](https://ui5.sap.com/#/api/sap.ui.model.odata.v2.ODataModel%23methods/create)
*No deep create support*
`update()`
`read()`
`remove()`
[`getProperty()`](https://ui5.sap.com/#/api/sap.ui.model.json.JSONModel%23methods/getProperty) Get a specific property of an OData model. *Starts with a / if it's not relative.*
[`setProperty()`](https://ui5.sap.com/#/api/sap.ui.model.json.JSONModel%23methods/setProperty) Set a specific property of an OData model. *If `changeBatchGroup` is not set to  `deferred` it updates automatically the data. Otherwise `submitChanges()` must be used*.
[`refresh()`](https://ui5.sap.com/#/api/sap.ui.model.odata.v2.ODataModel%23methods/refresh) Updates model's data with server.

### OData v4

---
## Binding
Binding doesn't just display model data to the frontend, it **connects** your model's data with your frontend's view. Any interactable control that modifies said data depending on the binding mode the model's data as well. 

### Element - Property
#### View Absolute Binding
```xml
<Control property="{model>/path/to/entity(id)/element}" />
```
View Relative `{model>element}`
or 
Controller Absolute 
```js
control.bindProperty(property, "model>/path/to/entity(id)/element");
```
Controller Relative 
```js
control.bindProperty(property, "model>/element");
```

### Entity - Control
Controller
``` js
control.bindElement("model>/path/to/entity(id)");
```
Then you can use element - property bindings with a relative path to the entity. The element - property binding propagates to children of the control.
*`bindObject` does the same thing.*

*TODO TALK ABOUT MULTIPART BINDING INSIDE THE XML*

---
## Routing
*TODO*

---
### Fragmentation (Code Cleanup)
*TODO*

---

## Formatters
*TODO TALK ABOUT FORMATTERS*

---
## Controls
### Text

#### Display Decimals in Text
```xml
<Text text="{
	path: 'path/to/float',
    type: 'sap.ui.model.type.Float',
    formatOptions: {
        minFractionDigits: 2, // The minimum fraction digits displayed
        maxFractionDigits: 2 // The maximum fraction digits displayed
    }
}" />
```

#### Display Date in Text
```xml
<Text text="{
	path: 'path/to/date', 
	type: 'sap.ui.model.type.Date', 
	formatOptions: { 
		pattern: 'FORMAT', // This is the pattern we want
		source: { pattern: 'FORMAT' } // This is the source pattern
	}
}" />
```

---
## Smart Controls
All smart controls enhance the data they're displaying using annotations. [Read more here](CAP.md#Annotations)
*They're currently obsolete and are being replaced by the [Flexible Programming Model](https://ui5.sap.com/test-resources/sap/fe/core/fpmExplorer/index.html#/overview/introduction)*

### Downsides of using smart controls
You can't use a formatter within any bound value (haven't checked everywhere yet).
You can't create a ValueList with distinct values from a table.
### Smart Field 
Displays a specific element enhanced with annotations.
```xml
<smartField:SmartField 
	entitySet="OData Entity" 
	value="{element}" 
	editable="boolean">
</smartField:SmartField>
```
*Example*

To assign a specific entry of an entity you must [bind it](#Entity%20-%20Control)

### Smart Table
Display a table enhanced with annotations. Preselect table rows with [this](CAP.md#LineItem%20annotation%20(Smart%20table%20column%20display)).
```xml
<smartTable:SmartTable
	tableType="ResponsiveTable" // Sets the table type
	entitySet="Orders"          // Selects the entity
	enableAutoBinding="true">   // Load table data on init
</smartTable:SmartTable>
```
*Example*

#### Control's parameters
##### `tableType`
| `tableType` | Function |
|---|---|
|Table | basic unresponsive table |
| ResponsiveTable | works good with smaller screens |
| TreeTable | creates a hierarchical tree |
| AnalyticalTable | don't know yet |

#### `enableAutoBinding`
**Enabled** - Loads the initial data.
**Disabled** - Manually load data with **`rebindTable`**.
#### `tableBindingPath`
Sets a specific path within an entity to get the data from, eg `\Orders(7)\items`. The `entitySet` needs to bind the entity that the data will be displayed.
#### `smartFilterId`
The id of the smart filter bar that will be filtering this table.

#### `beforeExport`
(*TODO EXPLAIN ON BEFORE EXPORT AND EXPAND ON THIS*)
```js
onBeforeExport: function (oEvent) {
                oEvent.getParameter("exportSettings").workbook.columns[11].type="Number"
            },
```

#### Issues

##### Change SmartTable's selection mode
The table extended by the SmartTable `SmartTable.getTable()` isn't the same as the one indicated in the API reference. 

*This could indicate a future deprecation of the old Smart Controls for the newer Flexible Programming Model Controls.*

Table's `Mode` is similar to SmartTable.getTable's `SelectionMode`.
```js
table.setMode(sap.m.ListMode.None);
// vs
smartTable.getTable().setSelectionMode('None');
```

##### Paginate more than the default threshold
```xml
<Table 
	id="idTable" 
	growing="true" 
	growingThreshold="1000000" 
	items="{listeModel>/}" 
	mode="MultiSelect" 
	updateFinished="onTableUpdateFinished" 
	selectionChange="onSelectionChange"
>
```

```js
const appId = this.getOwnerComponent().getManifestEntry("/sap.app/id");
const appPath = appId.replaceAll(".", "/");
const appModulePath = jQuery.sap.getModulePath(appPath);
let that = this;
let oTable = this.getView().byId("tableId");
$.ajax({
	url: appModulePath + "/v4/app-liste-pagamento2/DatiListe()",
	method: "GET",
	success: (oData) => {
		let data = JSON.parse(oData.value)
		let totalRowslength = data.length;
		oTable.setGrowingThreshold(totalRowslength + 1000);
		this.getView().setModel(new sap.ui.model.json.JSONModel(data), "listeModel");
	}, 
});
```

### Smart Filter Bar
Filters a smart table. You first need to assign it an id and then on the [Smart Table](#Smart%20Table) set the [smartFilterId](#`smartFilterId`).
```xml
<smartFilterbar:SmartFilterBar 
	id="smartfilter" 
	entitySet="Orders"> // Sets the entity 
	<smartFilterbar:controlConfiguration>
		<smartFilterbar:ControlConfiguration 
			key="oname" 
			visibleInAdvancedArea="true"
			preventInitialDataFetchInValueHelpDialog="false" />
	</smartFilterbar:controlConfiguration>
</smartFilterbar:SmartFilterBar>
```

#### Control's Parameters
##### `visibleInAdvancedArea`
Displays the filter in the bar.
##### `preventInitialDataFetchInValueHelpDialog`
Disables fetching of data for display in the value help.

## Comparisons
Difference between Freestyle, Smart Controls and FPM

---
## External Links
[How to make your app render faster](https://sapui5.hana.ondemand.com/sdk/docs/topics/408b40efed3c416681e1bd8cdd8910d4.html)