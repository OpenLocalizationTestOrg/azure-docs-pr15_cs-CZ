Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem parametrů obsahující všechny hodnoty parametrů.
Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Parametry pro hodnoty, které bude vždy, zůstane stejná není definovat. Každá hodnota parametru slouží v šabloně k definování zdroje, které používají. 

Při definování parametrů pomocí pole **allowedValues** můžete určit hodnot, které uživatel může poskytnout během nasazení. Použijte **Výchozí hodnota** pole pro přiřazení hodnoty parametru, pokud je zadána žádná hodnota během nasazení.

Bude popisu každý parametr v šabloně.

### <a name="sitename"></a>název webu

Název, který chcete vytvořit web appu.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Název plán služeb aplikací pro účely hostingu web appu.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU

Ceny vrstva hostingu plán.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Šablona definuje hodnoty, které jsou povoleny pro tento parametr a přiřadí výchozí hodnotu (S1), pokud je zadána žádná hodnota.

### <a name="workersize"></a>workerSize

Velikost instance hostingu plán (malá, střední nebo velký).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Šablona definuje hodnoty, které jsou povoleny pro tento parametr (0, 1 nebo 2) a přiřadí výchozí hodnoty (0), pokud je zadána žádná hodnota. Hodnoty odpovídají malá, středních a velkých.
