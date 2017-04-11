<properties
    pageTitle="Správa řízení přístupu na základě rolí pomocí rozhraní REST API"
    description="Správa řízení přístupu na základě rolí pomocí rozhraní REST API"
    services="active-directory"
    documentationCenter="na"
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="multiple"
    ms.tgt_pltfrm="rest-api"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="managing-role-based-access-control-with-the-rest-api"></a>Správa řízení přístupu na základě rolí pomocí rozhraní REST API

> [AZURE.SELECTOR]
- [Prostředí PowerShell](role-based-access-control-manage-access-powershell.md)
- [Azure rozhraní příkazového řádku](role-based-access-control-manage-access-azure-cli.md)
- [ROZHRANÍ REST API](role-based-access-control-manage-access-rest.md)

Na základě rolí přístup ovládacího prvku (RBAC) Azure portálu a rozhraní API Azure správce zdrojů pomáhá spravovat přístup k svoje předplatné a zdroje jemně odstupňovaná úrovni. Pomocí této funkce můžete udělit přístup uživatelů, skupin a objektů služby Active Directory některé role jim přiřadíte na určitý obor.

## <a name="list-all-role-assignments"></a>Seznam všech přiřazování rolí

Obsahuje seznam všech přiřazování rolí v zadaném oboru a subscopes.

Přiřazení role seznam musíte mít přístup k `Microsoft.Authorization/roleAssignments/read` operace na rozsah. Předdefinované role mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **GET** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments?api-version={api-version}&$filter={filter}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, u kterého chcete zobrazit seznam přiřazování rolí. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{verze rozhraní api}* 2015 07 01.

3. *{Filtr}* nahradíte podmínek, které chcete použít k filtrování seznamu přiřazení role:

  - Přiřazení rolí seznamu jenom určité oboru, bez čísel přiřazování rolí v subscopes:`atScope()`    
  - Přiřazení rolí seznamu pro určitého uživatele, skupinu nebo aplikace:`principalId%20eq%20'{objectId of user, group, or service principal}'`  
  - Seznam přiřazování rolí pro určitého uživatele, včetně těch dědí od skupiny |`assignedTo('{objectId of user}')`

### <a name="response"></a>Odpověď

Stavový kód: 200

```
{
  "value": [
    {
      "properties": {
        "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
        "principalId": "2f9d4375-cbf1-48e8-83c9-2a0be4cb33fb",
        "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
        "createdOn": "2015-10-08T07:28:24.3905077Z",
        "updatedOn": "2015-10-08T07:28:24.3905077Z",
        "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
        "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/baa6e199-ad19-4667-b768-623fde31aedd",
      "type": "Microsoft.Authorization/roleAssignments",
      "name": "baa6e199-ad19-4667-b768-623fde31aedd"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role-assignment"></a>Získat informace o přiřazování rolí

Získání informací o přiřazení jedné rolí nastavil identifikátor přiřazení role.

Chcete-li získat informace o přiřazování rolí, musíte mít přístup k `Microsoft.Authorization/roleAssignments/read` operace. Předdefinované role mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **GET** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, u kterého chcete zobrazit seznam přiřazování rolí. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role přiřazení}* identifikátor GUID přiřazování rolí.

3. Nahraďte *{verze rozhraní api}* 2015 07 01.

### <a name="response"></a>Odpověď

Stavový kód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "principalId": "672f1afa-526a-4ef6-819c-975c7cd79022",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "createdOn": "2015-10-05T08:36:26.4014813Z",
    "updatedOn": "2015-10-05T08:36:26.4014813Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleAssignments/196965ae-6088-4121-a92a-f1e33fdcc73e",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "196965ae-6088-4121-a92a-f1e33fdcc73e"
}

```

## <a name="create-a-role-assignment"></a>Vytvoření přiřazování rolí

Vytvoření přiřazování rolí na zadaný obor pro zadané jistinu udělení zadané role.

Pokud chcete vytvořit přiřazování rolí, musíte mít přístup k `Microsoft.Authorization/roleAssignments/write` operace. Předdefinované rolí jenom *vlastník* a *Správce přístup uživatelů* mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **umístění** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, pro niž chcete vytvořit přiřazování rolí. Při vytváření přiřazování rolí v nadřazený oboru zdědí všechny podřízené obory stejné přiřazování rolí. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1   
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role přiřazení}* nové identifikátor GUID, což bude identifikátor GUID nové přiřazování rolí.

3. Nahraďte *{verze rozhraní api}* 2015 07 01.

Žádost o textu poskytují hodnoty v tomto formátu:

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b"
  }
}

```

| Název elementu     | Povinné | Typ   | Popis |
|------------------|----------|--------|-------------|
| roleDefinitionId | Ano      | Řetězec | Identifikátor role. Formát identifikátor je:`{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id-guid}` |
| principalId      | Ano      | Řetězec | Objektu Azure AD hlavního uživatele (uživateli, skupině nebo služby jistinu) přiřazenou roli. |

### <a name="response"></a>Odpověď

Stavový kód: 201

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-16T00:27:19.6447515Z",
    "updatedOn": "2015-12-16T00:27:19.6447515Z",
    "createdBy": null,
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/2e9e86c8-0e91-4958-b21f-20f51f27bab2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "2e9e86c8-0e91-4958-b21f-20f51f27bab2"
}

```

## <a name="delete-a-role-assignment"></a>Odstranění přiřazování rolí

Odstranění přiřazování rolí v zadaném oboru.

Pokud chcete odstranit přiřazování rolí, musíte mít přístup k `Microsoft.Authorization/roleAssignments/delete` operace. Předdefinované rolí jenom *vlastník* a *Správce přístup uživatelů* mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Způsob **Odstranění** pomocí následující URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleAssignments/{role-assignment-id}?api-version={api-version}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, pro niž chcete vytvořit přiřazování rolí. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role přiřazení}* id přiřazení role GUID.

3. Nahraďte 2015. 07 01 *{verze rozhraní api}* .

### <a name="response"></a>Odpověď

Stavový kód: 200

```
{
  "properties": {
    "roleDefinitionId": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
    "principalId": "5ac84765-1c8c-4994-94b2-629461bd191b",
    "scope": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND",
    "createdOn": "2015-12-17T23:21:40.8921564Z",
    "updatedOn": "2015-12-17T23:21:40.8921564Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network/providers/Microsoft.Network/virtualNetworks/EASTUS-VNET-01/subnets/Devices-Engineering-ProjectRND/providers/Microsoft.Authorization/roleAssignments/5eec22ee-ea5c-431e-8f41-82c560706fd2",
  "type": "Microsoft.Authorization/roleAssignments",
  "name": "5eec22ee-ea5c-431e-8f41-82c560706fd2"
}

```

## <a name="list-all-roles"></a>Seznam všech rolí

Uvádí všechny role, které jsou k dispozici pro přiřazení v zadaném oboru.

Seznam role musíte mít přístup k `Microsoft.Authorization/roleDefinitions/read` operace v oboru. Předdefinované role mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **GET** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions?api-version={api-version}&$filter={filter}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, u kterého chcete zobrazit seznam role. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - /Subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1 zdroje  

2. Nahraďte *{verze rozhraní api}* 2015 07 01.

3. *{Filtr}* nahradíte podmínek, které chcete použít k filtrování seznamu role:

  - Role seznam umožňující přiřazení v zadaném oboru a všechny její podřízené obory:`atScopeAndBelow()`
  - Vyhledejte role pomocí přesné zobrazované jméno: `roleName%20eq%20'{role-display-name}'`. Pomocí formuláře překódován jako adresa URL přesné zobrazované jméno roli. Například`$filter=roleName%20eq%20'Virtual%20Machine%20Contributor'` |

### <a name="response"></a>Odpověď

Stavový kód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="get-information-about-a-role"></a>Získání informací o Role

Získání informací o jedné role nastavil identifikátor definice role. Informace o jedné role pomocí zobrazované jméno, získáte [seznam všech rolí](role-based-access-control-manage-access-rest.md#list-all-roles).

Pokud chcete získat informace o role, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/read` operace. Předdefinované role mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **GET** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, u kterého chcete zobrazit seznam přiřazování rolí. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role definice}* identifikátor GUID definice role.

3. Nahraďte *{verze rozhraní api}* 2015 07 01.

### <a name="response"></a>Odpověď

Stavový kód: 200

```
{
  "value": [
    {
      "properties": {
        "roleName": "Virtual Machine Contributor",
        "type": "BuiltInRole",
        "description": "Lets you manage virtual machines, but not access to them, and not the virtual network or storage account they\u2019re connected to.",
        "assignableScopes": [
          "/"
        ],
        "permissions": [
          {
            "actions": [
              "Microsoft.Authorization/*/read",
              "Microsoft.Compute/availabilitySets/*",
              "Microsoft.Compute/locations/*",
              "Microsoft.Compute/virtualMachines/*",
              "Microsoft.Compute/virtualMachineScaleSets/*",
              "Microsoft.Insights/alertRules/*",
              "Microsoft.Network/applicationGateways/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/backendAddressPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatPools/join/action",
              "Microsoft.Network/loadBalancers/inboundNatRules/join/action",
              "Microsoft.Network/loadBalancers/read",
              "Microsoft.Network/locations/*",
              "Microsoft.Network/networkInterfaces/*",
              "Microsoft.Network/networkSecurityGroups/join/action",
              "Microsoft.Network/networkSecurityGroups/read",
              "Microsoft.Network/publicIPAddresses/join/action",
              "Microsoft.Network/publicIPAddresses/read",
              "Microsoft.Network/virtualNetworks/read",
              "Microsoft.Network/virtualNetworks/subnets/join/action",
              "Microsoft.Resources/deployments/*",
              "Microsoft.Resources/subscriptions/resourceGroups/read",
              "Microsoft.Storage/storageAccounts/listKeys/action",
              "Microsoft.Storage/storageAccounts/read",
              "Microsoft.Support/*"
            ],
            "notActions": []
          }
        ],
        "createdOn": "2015-06-02T00:18:27.3542698Z",
        "updatedOn": "2015-12-08T03:16:55.6170255Z",
        "createdBy": null,
        "updatedBy": null
      },
      "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/9980e02c-c2be-4d73-94e8-173b1dc7cf3c",
      "type": "Microsoft.Authorization/roleDefinitions",
      "name": "9980e02c-c2be-4d73-94e8-173b1dc7cf3c"
    }
  ],
  "nextLink": null
}

```

## <a name="create-a-custom-role"></a>Vytvořit vlastní roli
Vytvořte vlastní roli.

Pokud chcete vytvořit vlastní role, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/write` operace ve všech `AssignableScopes`. Předdefinované rolí jenom *vlastník* a *Správce přístup uživatelů* mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **umístění** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci identifikátor URI proveďte následující náhradu přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* první *AssignableScope* vlastní roli. Následující příklady ukazují, jak můžete určit obor pro různé úrovně.

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role definice}* nové identifikátor GUID, což bude identifikátor GUID nové vlastní role.

3. Nahraďte *{verze rozhraní api}* 2015 07 01.

Textu žádosti o poskytují hodnoty v tomto formátu:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Název elementu | Povinné | Typ | Popis |
|--------------|----------|------|-------------|
| Jméno         | Ano | Řetězec   | Identifikátor GUID vlastní role.    |
| properties.roleName               | Ano | Řetězec   | Zobrazovaný název vlastní roli. Maximální velikost 128 znaků.                        |
| Properties.Description            | Ne  | Řetězec   | Popis vlastního role. Maximální velikost 1 024 znaky.                                               |
| Properties.Type                   | Ano | Řetězec   | Nastavte na "CustomRole."                                         |
| Properties.Permissions.Actions    | Ano | Řetězec] | Pole určující operace poskytované roli vlastní akce řetězců.             |
| properties.permissions.notActions | Ne  | Řetězec] | Pole určující operace vyloučit z operace poskytované roli vlastní akce řetězců. |
| properties.assignableScopes       | Ano | Řetězec] | Pole obory, ve kterých lze použít vlastní roli.   |

### <a name="response"></a>Odpověď

Stavový kód: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="update-a-custom-role"></a>Aktualizace vlastní roli

Upravte vlastní roli.

Chcete-li upravit vlastní roli, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/write` operace ve všech `AssignableScopes`. Předdefinované rolí jenom *vlastník* a *Správce přístup uživatelů* mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Použijte metodu **umístění** s URI následující:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* první *AssignableScope* vlastní roli. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role definice}* identifikátor GUID vlastní role.

3. Nahraďte *{verze rozhraní api}* 2015 07 01.

Žádost o textu poskytují hodnoty v tomto formátu:

```
{
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "properties": {
    "roleName": "Virtual Machine Operator",
    "description": "Lets you monitor virtual machines and restart them.",
    "type": "CustomRole",
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ]
  }
}

```

| Název elementu | Povinné | Typ | Popis |
|--------------|----------|------|-------------|
| Jméno         | Ano      | Řetězec | Identifikátor GUID vlastní role. |
| properties.roleName | Ano | Řetězec | Zobrazovaný název aktualizované vlastní roli. |
| Properties.Description | Ne | Řetězec | Popis aktualizované vlastní roli. |
| Properties.Type | Ano | Řetězec | Nastavte na "CustomRole." |
| Properties.Permissions.Actions | Ano | Řetězec] | Pole Akce řetězců určující operace, ke kterým uděluje aktualizované vlastní roli přístup. |
| properties.permissions.notActions | Ne | Řetězec] | Pole určující operace vyloučení operacích, které aktualizované vlastní role uděluje řetězců akce. |
| properties.assignableScopes | Ano | Řetězec] | Pole obory, ve kterých mohou sloužit aktualizované vlastní roli. |

### <a name="response"></a>Odpověď

Stavový kód: 201

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-18T00:10:51.4662695Z",
    "updatedOn": "2015-12-18T00:10:51.4662695Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "7c8c8ccd-9838-4e42-b38c-60f0bbe9a9d7"
}

```

## <a name="delete-a-custom-role"></a>Odstranění vlastní Role

Odstraňte vlastní roli.

Pokud chcete odstranit vlastní roli, musíte mít přístup k `Microsoft.Authorization/roleDefinitions/delete` operace ve všech `AssignableScopes`. Předdefinované rolí jenom *vlastník* a *Správce přístup uživatelů* mají přístup k této operaci. Další informace o přiřazování rolí a správa přístup pro Azure zdroje najdete v tématu [Řízení přístupu Azure Role-Based](role-based-access-control-configure.md).

### <a name="request"></a>Žádost o

Způsob **Odstranění** pomocí následující URI:

    https://management.azure.com/{scope}/providers/Microsoft.Authorization/roleDefinitions/{role-definition-id}?api-version={api-version}

V rámci URI zkontrolujte následující náhrady přizpůsobení žádosti o:

1. Nahraďte *{rozsah}* obor, pro niž chcete odstranit definici role. Následující příklady ukazují, jak můžete určit obor pro různé úrovně:

  - Předplatné: /subscriptions/ {id předplatného}  
  - Pole Skupina zdroje: /subscriptions/ {id předplatného} / resourceGroups/myresourcegroup1  
  - Zdroje: /subscriptions/{subscription-id}/resourceGroups/myresourcegroup1/providers/Microsoft.Web/sites/mysite1  

2. Nahraďte *{id role definice}* identifikátor GUID role definice vlastní roli.

3. Nahraďte 2015. 07 01 *{verze rozhraní api}* .

### <a name="response"></a>Odpověď

Stavový kód: 200

```
{
  "properties": {
    "roleName": "Virtual Machine Operator",
    "type": "CustomRole",
    "description": "Lets you monitor virtual machines and restart them.",
    "assignableScopes": [
      "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"
    ],
    "permissions": [
      {
        "actions": [
          "Microsoft.Authorization/*/read",
          "Microsoft.Compute/*/read",
          "Microsoft.Insights/alertRules/*",
          "Microsoft.Network/*/read",
          "Microsoft.Resources/subscriptions/resourceGroups/read",
          "Microsoft.Storage/*/read",
          "Microsoft.Support/*",
          "Microsoft.Compute/virtualMachines/start/action",
          "Microsoft.Compute/virtualMachines/restart/action"
        ],
        "notActions": []
      }
    ],
    "createdOn": "2015-12-16T00:07:02.9236555Z",
    "updatedOn": "2015-12-16T00:07:02.9236555Z",
    "createdBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e",
    "updatedBy": "877f0ab8-9c5f-420b-bf88-a1c6c7e2643e"
  },
  "id": "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/providers/Microsoft.Authorization/roleDefinitions/0bd62a70-e1b8-4e0b-a7c2-75cab365c95b",
  "type": "Microsoft.Authorization/roleDefinitions",
  "name": "0bd62a70-e1b8-4e0b-a7c2-75cab365c95b"
}

```


[AZURE.INCLUDE [role-based-access-control-toc.md](../../includes/role-based-access-control-toc.md)]
