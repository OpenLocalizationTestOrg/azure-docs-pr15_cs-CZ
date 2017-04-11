Pomocí Správce prostředků Azure definovat parametry pro hodnoty, které chcete zadat při nasazení šablony. Šablona obsahuje oddílu s názvem parametrů obsahující všechny hodnoty parametrů.
Je třeba definovat parametr hodnoty, které se liší podle projektu, který nasazujete nebo podle prostředí, které nasazujete. Parametry pro hodnoty, které bude vždy, zůstane stejná není definovat. Každá hodnota parametru slouží v šabloně k definování zdroje, které jsou nasazení. 

Při definování parametrů pomocí pole **allowedValues** můžete určit hodnot, které uživatel může poskytnout během nasazení. Použijte **Výchozí hodnota** pole pro přiřazení hodnoty parametru, pokud je zadána žádná hodnota během nasazení.

Bude popisu každý parametr v šabloně.

### <a name="logicappname"></a>logicAppName

Název aplikace použití logických operátorů k vytvoření.

    "logicAppName": {
        "type": "string"
    }