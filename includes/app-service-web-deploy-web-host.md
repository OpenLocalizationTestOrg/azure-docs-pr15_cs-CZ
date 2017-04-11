### <a name="app-service-plan"></a>Plán služeb aplikací

Vytvoří plán služeb pro hostování web appu. Zadejte název plánu přes **hostingPlanName** parametr. Umístění plánu je stejný pro skupiny zdrojů. Ceny osy a pracovní velikost jsou uvedeny v parametrech **sku** a **workerSize**

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

