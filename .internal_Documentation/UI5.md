# how to make app render faster

    -component-preload.js
    https://sapui5.hana.ondemand.com/sdk/docs/topics/408b40efed3c416681e1bd8cdd8910d4.html



# String to date compatability issues solved 
    DataMovimento value = '2023-05-05'
    was getting error The given date instance isn't valid. -  
    source: { pattern: 'yyyy-MM-dd' }
 <Text text="{path: 'ReportDettaglioCu>DataMovimento', type: 'sap.ui.model.type.Date', formatOptions: { pattern: 'MMM',source: { pattern: 'yyyy-MM-dd' }}}" wrapping="false" /> 


# float decimals Digits
<Text text="{path: 'ReportRiepilogoCu>PercentualeAliquota',
                type: 'sap.ui.model.type.Float',
                formatOptions: {
                    minFractionDigits: 2,
                    maxFractionDigits: 2
                }}" wrapping="false" />


### load more than 100rows in to a table => oTable.setGrowingThreshold() / oTable.setThreshold()
<Table id="idTable" growing="true" growingThreshold="1000000" items="{listeModel>/}" mode="MultiSelect" updateFinished="onTableUpdateFinished" selectionChange="onSelectionChange">


let appId = this.getOwnerComponent().getManifestEntry("/sap.app/id"),
                appPath = appId.replaceAll(".", "/"),
                appModulePath = jQuery.sap.getModulePath(appPath);
                let that = this;
                let oTable = this.getView().byId("tableId");
                $.ajax({
                    url: appModulePath + "/v4/app-liste-pagamento2/DatiListe()",
                    method: "GET",
                    success: (oData) => {
                        //that.model.setProperty("/Movimenti",new sap.ui.model.json.JSONModel(JSON.parse(oData.value)));
                        //aMovimentiDisplay.push(JSON.parse(oData.value));
                        let data = JSON.parse(oData.value)
                        let totalRowslength = data.length;
                            oTable.setGrowingThreshold(totalRowslength + 1000);
                            // oTable.attachEvent("rowSelectionChange", that.onRowSelectionChange, that);
                        this.getView().setModel(new sap.ui.model.json.JSONModel(data), "listeModel");
                    }, error: (oError) => {

                    }
                });


#### change date formt XMl based on the source format 
<Text text="{path: 'listeModel>DataContabile', type: 'sap.ui.model.type.Date', formatOptions: { pattern: 'dd/MM/yyyy',source: { pattern: 'yyyy-MM-dd' }}}" wrapping="false" />


### multiinput arguments Formatter 

<Text text="{parts: [{path: 'CausaleMovimento/DareAvere_name'}, {path: 'ImportoLordo'}, {path: 'Valuta'}], formatter: '.formatImportoLordo', type: 'sap.ui.model.type.Currency', formatOptions: { showMeasure: false } }" />

             formatImportoLordo: function (dareAvereName, importoLordo) {
                //  if (dareAvereName === 'D') {
                //      return '-' + importoLordo;
                //  }
                //  return typeFloat.formatValue(importoLordo);
                 var floatType = new Float({
                     maxFractionDigits: 2,
                     decimalSeparator: ",",
                     groupingSeparator: "."
                 });

                 if (dareAvereName === "D") {
                     importoLordo = -importoLordo;
                 }

                 return floatType.formatValue(importoLordo, "string");
             },

#### edit data format excel for SmartTable
onBeforeExport: function (oEvent) {
                var oSmartTable = oEvent.getSource();
                var oTable = oSmartTable.getTable();

                var oBinding = oTable.getBinding("items");
                var aItems = oBinding.getCurrentContexts().map(function (oContext) {
                    var oItemData = oContext.getObject();
                    oItemData.Importo = Number(oItemData.Importo);
                    return oItemData;
                });

                // Update the exported data with the modified array
                oEvent.getParameter("exportSettings").data = aItems;
            },