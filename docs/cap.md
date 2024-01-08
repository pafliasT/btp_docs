# CAP

## OData Notes
CAP exposes by default it's data using the v4 protocol, but there are ways to convert back to v2.
*We won't look into the benefits of v2 & v4 here.*
### Converting to v2
As cap is creating an express server, we can provide a middleware proxy to do the conversion.
[This is the supported version](https://www.npmjs.com/package/@cap-js-community/odata-v2-adapter#cap-js-communityodata-v2-adapter-cov2ap) and [the deprecated version](https://www.npmjs.com/package/@sap/cds-odata-v2-adapter-proxy).

The supported version is required for sap support to help a project.
### Differences between v4 and v2

## Using Smart Controls
Prerequisites:
- OData model must be V2 (see [odata v2 adapter](https://github.com/cap-js-community/odata-v2-adapter)).
- Entities must be bound to the control programmatically.
## Annotations
To find information about any annotation look [here](https://cap.cloud.sap/docs/advanced/odata#vocabularies)
### ValueList annotation (Input Helper):
```js
@Common.ValueList: {
	Label: 'Label',              // <-- No clue yet
	CollectionPath: 'ODataPath', // <-- Selection Data from this OData Entity
	Parameters: [                // <-- Columns will be displayed in Helper
		{
			$Type: 'Common.ValueList____', // <-- Interraction with data
			ValueListProperty: 'Element',  // <-- Element from OData's CollectionPath
			?LocalDataProperty: Element    // <-- Local anotation's data filtered
		}
	]
}
```
If you have only one parameter it can't be of $Type: `Common.ValueListParameterDisplayOnly`

#### $Type Values
Every value is defined with the following schema `Common.ValueListParameter<Value>` where value is:
- `Out`: Local -> OData (Displays)
- `In`: Local -> OData, auto-filters with `eq` comparison (Doesn't display)
- `InOut`: Filters with `startswith` comparison (Displays)
- `Constant`: 
- `FilteredOnly`:
- `DisplayOnly`: Display only, requires at least one filter column (Displays)

### LineItem annotation (Smart table column display):
Manage which columns will be displayed by default using the following annotation
```js
annotate Entity with @(UI.LineItem: [
    { Value: Element },
]);
```

## Functions & Actions

`INSERT INTO` queries don't insert the data after the Promise resolves. The function needs to exit out completely without any issues for the data to begin it's insertion.