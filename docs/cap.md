# CAP - Cloud Application Programming Model 🚀

This document outlines essential notes on OData, utilizing smart controls, annotations within CAP, and nuances with functions & actions.

## OData Notes 📝

CAP defaults to exposing its data using the OData v4 protocol. However, it supports conversion to v2 with middleware proxy.

### Converting to v2 ↩️

- CAP leverages an Express server, enabling middleware proxy for protocol conversion.
- **Supported Version**: [`@cap-js-community/odata-v2-adapter`](https://www.npmjs.com/package/@cap-js-community/odata-v2-adapter#cap-js-communityodata-v2-adapter-cov2ap) is recommended for SAP support.
- **Deprecated Version**: [`@sap/cds-odata-v2-adapter-proxy`](https://www.npmjs.com/package/@sap/cds-odata-v2-adapter-proxy).

### Differences Between v4 and v2 🔄

*Details on the protocol differences are not covered here.*

## Using Smart Controls 💡

To leverage Smart Controls effectively:

### Prerequisites

- The OData model must be v2, facilitated by [odata v2 adapter](https://github.com/cap-js-community/odata-v2-adapter).
- Entities should be bound to the control programmatically.

## Annotations 📊

Annotations enhance OData services with metadata for UI, filtering, and more. For comprehensive details on annotations, visit [CAP's OData documentation](https://cap.cloud.sap/docs/advanced/odata#vocabularies).

### ValueList Annotation (Input Helper) 📋

```js
@Common.ValueList: {
    Label: 'Label',              // Description or label of the input helper
    CollectionPath: 'ODataPath', // Data selection from this OData Entity
    Parameters: [                // Columns to be displayed in the input helper
        {
            $Type: 'Common.ValueList____', // Interaction type with data
            ValueListProperty: 'Element',  // Element from OData's CollectionPath
            ?LocalDataProperty: 'Element'  // Local annotation's data filtered
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