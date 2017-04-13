<properties
    pageTitle="Azure zásad správce prostředků | Microsoft Azure"
    description="Popisuje, jak používat Azure správce prostředků zásad abychom zabránili posílání porušení na různých místech jako předplatné, skupiny zdrojů nebo jednotlivé zdroje."
    services="azure-resource-manager"
    documentationCenter="na"
    authors="ravbhatnagar"
    manager="timlt"
    editor="tysonn"/>

<tags
    ms.service="azure-resource-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="gauravbh;tomfitz"/>

# <a name="use-policy-to-manage-resources-and-control-access"></a>Přidávání a používání zdrojů a řízení přístupu pomocí zásad

Azure správce prostředků umožňuje řídit přístup pomocí vlastní zásady. Zásady můžete zabránit uživatelům ve vaší organizaci v porušení smlouvy, které jsou potřebné pro přidávání a používání zdrojů organizace. 

Vytvoření definice zásad, které popisují akce nebo zdroje, které jsou specificky odepřen. Přiřadit tyto zásady definice na požadovaný rozsah, třeba předplatné, skupina zdroje nebo konkrétního zdroje. 

V tomto článku je vysvětleno jsme základní struktura zásad definice jazyk, který můžete použít k vytvoření zásad. Potom jsme popisují, jak můžete použít tyto zásady na různých místech.

## <a name="how-is-it-different-from-rbac"></a>Čím se liší od RBAC?

Existuje několik důležité rozdíly mezi zásady a řízení přístupu na základě rolí, ale první věc, kterou chcete porozumět tomu, je společně pracovat zásady a RBAC. Použití zásad, musí být ověřeny prostřednictvím RBAC. Na rozdíl od RBAC, je povolit výchozí zásady a explicitní odepřít systému. 

RBAC se zaměřuje na akce, které můžete dělat **uživateli** na různých místech. Například určitému uživateli vkládá role přispěvatele skupina zdroje na požadovaný rozsah, aby uživatel můžete provádět úpravy k této skupině zdrojů. 

Zásady se zaměřuje na **zdroje** akce na různé obory. Například prostřednictvím zásad, můžete určit typy zdrojů, které můžete zřízení nebo omezit umístění, ve kterých můžete zřízení zdroje.

## <a name="common-scenarios"></a>Obvyklé scénáře

Jeden scénář běžné je vyžadovat oddělení značky za účelem zpětné. Organizaci možná budete chtít povolit operace pouze v případě potřeby nákladové středisko je přidružená; v opačném zamítnutí žádosti. Díky tomu je účtovat poplatky odpovídající nákladové středisko pro provedených.

Jiné běžné scénáře je, že organizaci možná budete chtít určit umístění, kde jsou vytvářeny zdroje. Nebo kterým můžou chtít řízení přístupu k prostředkům tím, že povolí jenom určité typy zdrojů zřídit.

Podobně organizace můžete ovládat katalog služby a jejímu vynucení požadované konvence pro zdroje.

Zásady podobnému sledu můžete snadno dosáhnout.

## <a name="policy-definition-structure"></a>Struktura definice zásad

Definice zásad vytvoří se na základě JSON. Je tvořen jeden nebo více podmínek/logické operátory, které definují akce a efekt informující o tom, co se stane, pokud jsou splněny podmínky. Schéma je publikován na [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json). 

V podstatě zásadu obsahuje následující elementy:

**Podmínka/logických operátorů:** sadu podmínek, které můžete pracovat pomocí sady logické operátory.

**Efekt:** situace, kdy splněny – odepřít nebo auditování. Efekt auditování posílá služby protokol událostí upozornění. Správce můžete například vytvořit zásad, které způsobují události auditování pokud každý uživatel vytvoří velké OM. Správce může protokolech později.

    {
      "if" : {
          <condition> | <logical operator>
      },
      "then" : {
          "effect" : "deny | audit | append"
      }
    }
    
## <a name="policy-evaluation"></a>Hodnocení zásad

Zásady jsou vyhodnoceny při vytváření zdroje. V případě nasazení šablony zásady vyhodnoceny při vytváření jednotlivé zdroje v šabloně. 

> [AZURE.NOTE] V současné době zásad vyhodnotí typů zdrojů, které nepodporují značky, typ a umístění, třeba typ zdroje Microsoft.Resources/deployments. Toto odborné pomoci přidá později. Chcete-li předejít zpětné kompatibilitě, měli byste explicitně určit typ při vytváření zásad. Například zásady značky, které nejsou určete typy se použije pro všechny typy. V takovém případě nasazení šablony se nemusí podařit, když není vnořené prostředek, který nepodporuje značky a přidal typ zdroje nasazení zásad hodnocení. 

## <a name="logical-operators"></a>Logické operátory

Jsou podporované logické operátory spolu s syntaxe:

| Název operátor     | Syntaxe         |
| :------------- | :------------- |
| Not            | "není": {&lt;podmínku nebo operátor &gt;}             |
| A           | "allOf": [{&lt;podmínku nebo operátor &gt;}, {&lt;podmínku nebo operátor &gt;}] |
| Nebo                         | "anyOf": [{&lt;podmínku nebo operátor &gt;}, {&lt;podmínku nebo operátor &gt;}] |

Správce prostředků umožňuje zadat komplexní logiky pomocí vnořené operátory pravidla. Můžete třeba odepřít vytvoření zdrojů v na určité místo pro typ daný zdroj. Níže je znázorněn příklad vnořené operátory.

## <a name="conditions"></a>Podmínky

Podmínka vyhodnocena jako, jestli **pole** nebo **zdroj** splňují určitá kritéria. Jsou podporované podmínka názvy a syntaxe:

| Název podmínky | Syntaxe                |
| :------------- | :------------- |
| Rovná se             | "rovná se": "&lt;hodnotu&gt;"               |
| Jako                  | ", například": "&lt;hodnotu&gt;"                   |
| Obsahuje          | "obsahuje": "&lt;hodnotu&gt;"|
| V                        | "v": ["&lt;hodnota1&gt;","&lt;hodnota2&gt;"]|
| ContainsKey    | "containsKey": "&lt;Název_klíče&gt;" |
| Existuje     | "existuje": "&lt;logická hodnota&gt;" |

### <a name="fields"></a>Pole

Podmínky jsou vytvořené pomocí pole a zdroje. Pole představuje vlastnosti v žádosti o datové zdroje, který slouží k tomu, stav zdroje. Zdroj představuje vlastnosti žádost. 

Jsou podporovány následující pole a zdrojů:

Pole: **název**, **Typ**, **Typ**, **umístění**, **značky** **značky.** *, and * *vlastnost alias**. 

### <a name="property-aliases"></a>Vlastnost aliasy 
Vlastnost alias je název, který lze použít v definici zásad pro přístup k zdroje typ specifické vlastnosti, například nastavení a skladové jednotky. Funguje mezi všemi verzemi rozhraní API Pokud vlastnost existuje. Aliasy lze získat pomocí rozhraní REST API vidíte na obrázku (prostředí Powershell podpory, se přidají v budoucích):

    GET /subscriptions/{id}/providers?$expand=resourceTypes/aliases&api-version=2015-11-01
    
Definice alias jsou uvedeny níže. Můžete vidět, alias definuje cesty v různých verzích rozhraní API, i když dojde ke změně název vlastnosti. 

    "aliases": [
        {
          "name": "Microsoft.Storage/storageAccounts/sku.name",
          "paths": [
            {
              "path": "properties.accountType",
              "apiVersions": [
                "2015-06-15",
                "2015-05-01-preview"
              ]
            },
            {
              "path": "sku.name",
              "apiVersions": [
                "2016-01-01"
              ]
            }
          ]
        }
    ]

V současné době podporované aliasy jsou:

| Název aliasu | Popis |
| ---------- | ----------- |
| {resourceType}/sku.name | Jsou podporované zdroje typy: Microsoft.Compute/virtualMachines,<br />Microsoft.Storage/storageAccounts,<br />Microsoft.Web/serverFarms,<br /> Microsoft.Scheduler/jobcollections,<br />Microsoft.DocumentDB/databaseAccounts,<br />Microsoft.Cache/Redis,<br />Microsoft.CDN/profiles |
| {resourceType}/sku.family | Typ podporované zdroje je Microsoft.Cache/Redis |
| {resourceType}/sku.capacity | Typ podporované zdroje je Microsoft.Cache/Redis |
| Microsoft.Compute/virtualMachines/imagePublisher |  |
| Microsoft.Compute/virtualMachines/imageOffer  |  |
| Microsoft.Compute/virtualMachines/imageSku  |  |
| Microsoft.Compute/virtualMachines/imageVersion  |  |
| Microsoft.Cache/Redis/enableNonSslPort |  |
| Microsoft.Cache/Redis/shardCount |  |
| Microsoft.SQL/servers/version |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveId |  |
| Microsoft.SQL/servers/databases/requestedServiceObjectiveName |  |
| Microsoft.SQL/servers/databases/edition |  |
| Microsoft.SQL/servers/databases/elasticPoolName |  |
| Microsoft.SQL/servers/elasticPools/dtu |  |
| Microsoft.SQL/servers/elasticPools/edition |  |

V současné době zásad funguje jenom na žádosti o umístění. 

## <a name="effect"></a>Efekt
Zásady podporuje tři typy efekt – **Odepřít**, **auditování**ani **Připojit**. 

- Odepřít generuje události v protokolu auditování a přestane žádosti
- Auditování generuje události v protokolu auditování, ale nepovede žádosti
- Připojení přidá definovaný sadu polí k žádosti o 

Pro **připojení**je nutné zadat tyto údaje:

    ....
    "effect": "append",
    "details": [
      {
        "field": "field name",
        "value": "value of the field"
      }
    ]

Hodnota může být řetězec nebo objekt formátu JSON. 

## <a name="policy-definition-examples"></a>Příklady definice zásad

Teď Pojďme se podívat na tom, jak jsme definování zásad pro dosažení předchozích případů.

### <a name="chargeback-require-departmental-tags"></a>Zpětné: Vyžadují oddělení značky

Následující zásady zakazuje požadavky, které nemají značku obsahující klíč "costCenter".

    {
      "if": {
        "not" : {
          "field" : "tags",
          "containsKey" : "costCenter"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

Následující zásady Přidá značku costCenter s hodnotou předdefinované při prezentování bez značky. 

    {
      "if": {
        "field": "tags",
        "exists": "false"
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags",
            "value": {"costCenter":"myDepartment" }
          }
        ]
      }
    }
    
Následující zásady Přidá značku costCenter s hodnotou předdefinované při značku costCenter není k dispozici, ale existují jiné značky. 

    {
      "if": {
        "allOf": [
          {
            "field": "tags",
            "exists": "true"
          },
          {
            "field": "tags.costCenter",
            "exists": "false"
          }
        ]
    
      },
      "then": {
        "effect": "append",
        "details": [
          {
            "field": "tags.costCenter",
            "value": "myDepartment"
          }
        ]
      }
    }


### <a name="geo-compliance-ensure-resource-locations"></a>Dodržování předpisů GEO: Zajistit umístění zdroje

Následující příklad ukazuje zásadu zakazuje žádosti o, kde je umístěný není severní Europe nebo západní Europe.

    {
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="service-curation-select-the-service-catalog"></a>Služba Curation: Vyberte katalog služby

Následující příklad ukazuje zásadu, která umožňuje akce jenom na služeb typu Microsoft.Resources/\*, Microsoft.Compute/\*, Microsoft.Storage/\*, Microsoft.Network/\* jsou povolené. Cokoli jiného byl odepřen.

    {
      "if" : {
        "not" : {
          "anyOf" : [
            {
              "field" : "type",
              "like" : "Microsoft.Resources/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Compute/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Storage/*"
            },
            {
              "field" : "type",
              "like" : "Microsoft.Network/*"
            }
          ]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }

### <a name="use-approved-skus"></a>Použití schváleno skladové jednotky

Následující příklad ukazuje použití vlastnost alias omezení skladové jednotky. V příkladu pouze Standard_LRS a Standard_GRS schváleno u účtů úložiště.

    {
      "if": {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Storage/storageAccounts"
          },
          {
            "not": {
              "allof": [
                {
                  "field": "Microsoft.Storage/storageAccounts/sku.name",
                  "in": ["Standard_LRS", "Standard_GRS"]
                }
              ]
            }
          }
        ]
      },
      "then": {
        "effect": "deny"
      }
    }
    

### <a name="naming-convention"></a>Pojmenování

Následující příklad ukazuje použití zástupných znaků, které podporuje podmínka "je". Podmínka uvádí, že pokud název odpovídá zmíněná vzorek (namePrefix\*nameSuffix) odepřít žádost.

    {
      "if" : {
        "not" : {
          "field" : "name",
          "like" : "namePrefix*nameSuffix"
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }
    
### <a name="tag-requirement-just-for-storage-resources"></a>Značka požadavek jenom pro zdroje úložiště

Následující příklad ukazuje, jak vnořit logické operátory vyžadovat značku aplikace pro pouze úložiště zdroje.

    {
        "if": {
            "allOf": [
              {
                "not": {
                  "field": "tags",
                  "containsKey": "application"
                }
              },
              {
                "field": "type",
                "equals": "Microsoft.Storage/storageAccounts"
              }
            ]
        },
        "then": {
            "effect": "audit"
        }
    }

## <a name="policy-assignment"></a>Přiřazení zásad

Použití zásad na různých místech jako předplatné, skupiny zdrojů a jednotlivé zdroje. Zásady se dědí tak, že všechny podřízené zdroje. Ano Pokud je zásada použita ke skupině zdroje, bude k dispozici všechny zdroje v dané skupině zdroje.

## <a name="creating-a-policy"></a>Vytvoření zásad

Tato část obsahuje podrobnosti o tom, jak zásadu lze vytvářet pomocí rozhraní REST API.

### <a name="create-policy-definition-with-rest-api"></a>Vytvoření zásad definice pomocí rozhraní REST API

Vytvoření zásad s [Rozhraní REST API pro definice zásad](https://msdn.microsoft.com/library/azure/mt588471.aspx). Rozhraní REST API umožňuje vytváření a odstraňování definice zásad a získání informací o stávající definice.

Pokud chcete vytvořit zásady, spusťte:

    PUT https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.authorization/policydefinitions/{policyDefinitionName}?api-version={api-version}

Žádost o subjektu podobně jako v následujícím příkladu:

    {
      "properties":{
        "policyType":"Custom",
        "description":"Test Policy",
        "policyRule":{
          "if" : {
            "not" : {
              "field" : "tags",
              "containsKey" : "costCenter"
            }
          },
          "then" : {
            "effect" : "deny"
          }
        }
      },
      "name":"testdefinition"
    }


Verze rozhraní api dosáhnete *2016 04 01*. Příklady a další informace najdete v tématu [Rozhraní REST API pro definice zásad](https://msdn.microsoft.com/library/azure/mt588471.aspx).

### <a name="create-policy-definition-using-powershell"></a>Vytvoření zásad definice pomocí prostředí PowerShell

Můžete vytvořit zásady definice pomocí rutinu New-AzureRmPolicyDefinition. Následující příklad vytvoří zásad pro povolení zdrojů jen v Severní Evropa a západní Evropa.

    $policy = New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain regions" -Policy '{  
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'          

Výstup spuštění je uložen v $policy objektu a mohou sloužit později během přiřazení zásad. Pro parametr zásad cesta k souboru .json obsahující zásady lze také zadat místo určení vložené zásad.

    New-AzureRmPolicyDefinition -Name regionPolicyDefinition -Description "Policy to allow resource creation only in certain    regions" -Policy "path-to-policy-json-on-disk"

### <a name="create-policy-definition-using-azure-cli"></a>Vytvoření zásad definice pomocí rozhraní příkazového řádku Azure

Můžete vytvořit zásady definici azure rozhraní příkazového řádku pomocí příkazu zásad definice jak je ukázáno v následujícím příkladu. Následující příklad vytvoří zásad pro povolení zdrojů jen v Severní Evropa a západní Evropa.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy-string '{   
      "if" : {
        "not" : {
          "field" : "location",
          "in" : ["northeurope" , "westeurope"]
        }
      },
      "then" : {
        "effect" : "deny"
      }
    }'    
    

Je možné zadat cestu k souboru .json obsahující zásady místo určení vložené zásad, jak je ukázáno v následujícím příkladu.

    azure policy definition create --name regionPolicyDefinition --description "Policy to allow resource creation only in certain regions" --policy "path-to-policy-json-on-disk"


## <a name="applying-a-policy"></a>Použití zásad

### <a name="policy-assignment-with-rest-api"></a>Přiřazení zásad pomocí rozhraní REST API

Můžete použít definici zásad na požadovaný rozsah prostřednictvím [Rozhraní REST API pro přiřazení zásad](https://msdn.microsoft.com/library/azure/mt588466.aspx). Rozhraní REST API umožňuje vytváření a odstraňování přiřazení zásad a získání informací o stávající přiřazení.

Pokud chcete vytvořit přiřazení zásad, spusťte:

    PUT https://management.azure.com /subscriptions/{subscription-id}/providers/Microsoft.authorization/policyassignments/{policyAssignmentName}?api-version={api-version}

{Zásad přiřazení} je název přiřazení zásad. Verze rozhraní api dosáhnete *2016 04 01*. 

Žádost o subjektu podobně jako v následujícím příkladu:

    {
      "properties":{
        "displayName":"VM_Policy_Assignment",
        "policyDefinitionId":"/subscriptions/########/providers/Microsoft.Authorization/policyDefinitions/testdefinition",
        "scope":"/subscriptions/########-####-####-####-############"
      },
      "name":"VMPolicyAssignment"
    }

Příklady a další informace najdete v tématu [Rozhraní REST API pro přiřazení zásad](https://msdn.microsoft.com/library/azure/mt588466.aspx).

### <a name="policy-assignment-using-powershell"></a>Přiřazení zásad pomocí prostředí PowerShell

Použijete zásady vytvořené v nad pomocí prostředí PowerShell na požadovaný rozsah rutinu New-AzureRmPolicyAssignment:

    New-AzureRmPolicyAssignment -Name regionPolicyAssignment -PolicyDefinition $policy -Scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Tady je $policy objekt zásad vrácená důsledku spuštění rutinu New-AzureRmPolicyDefinition uvedené výše. Tady je název Skupina zdroje, které zadáte.

Pokud chcete odebrat výše přiřazení zásady, můžete to udělat takto:

    Remove-AzureRmPolicyAssignment -Name regionPolicyAssignment -Scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Můžete získat, změna nebo odebrání zásad definice prostřednictvím Get-AzureRmPolicyDefinition, sadu AzureRmPolicyDefinition a odebrat AzureRmPolicyDefinition rutin v tomto pořadí.

Podobně můžete získat, změna nebo odebrání přiřazení zásad prostřednictvím Get-AzureRmPolicyAssignment, sadu AzureRmPolicyAssignment a odebrat AzureRmPolicyAssignment rutin v tomto pořadí.

### <a name="policy-assignment-using-azure-cli"></a>Přiřazení zásad pomocí rozhraní příkazového řádku Azure

Použijete zásady vytvořené nad prostřednictvím rozhraní příkazového řádku Azure na požadovaný rozsah pomocí příkazu přiřazení zásad:

    azure policy assignment create --name regionPolicyAssignment --policy-definition-id /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/<policy-name> --scope    /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>
        
Tady je název Skupina zdroje, které zadáte. Pokud je argument hodnota parametru zásad definice id neznámý, je možné získat prostřednictvím rozhraní příkazového řádku Azure, jak je ukázáno v následujícím příkladu: 

    azure policy definition show <policy-name>

Pokud chcete odebrat výše přiřazení zásady, můžete to udělat takto:

    azure policy assignment delete --name regionPolicyAssignment --scope /subscriptions/########-####-####-####-############/resourceGroups/<resource-group-name>

Můžete získat, změna nebo odebrání zásad definice prostřednictvím zásad definice zobrazíte nastavit a odstraňte příkazy v tomto pořadí.

Podobně získáte, změna nebo odebrání zásad přiřazení prostřednictvím přiřazení zásad zobrazit a odstranit příkazy v tomto pořadí.

##<a name="policy-audit-events"></a>Události zásad auditování

Po použití svoje zásady začnete najdete v článku zásad událostí. Můžete buď přejít na portál, pomocí prostředí PowerShell nebo rozhraní příkazového řádku Azure tato data. 

### <a name="policy-audit-events-using-powershell"></a>Události zásad auditování pomocí prostředí PowerShell

Pokud chcete zobrazit všechny události, které souvisí odepřít efekt, můžete pomocí následujícího příkazu Powershellu:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/deny/action"} 

Pokud chcete zobrazit všechny události týkající se mají auditovat efekt, můžete tento příkaz:

    Get-AzureRmLog | where {$_.OperationName -eq "Microsoft.Authorization/policies/audit/action"} 

### <a name="policy-audit-events-using-azure-cli"></a>Události zásad auditování pomocí rozhraní příkazového řádku Azure

Můžete zobrazit všechny události z zdroj skupině související s odepřít efekt, můžete použít příkaz rozhraní příkazového řádku:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/deny/action\")"

Pokud chcete zobrazit všechny události týkající se mají auditovat efekt, můžete provádět následující příkaz rozhraní příkazového řádku:

    azure group log show ExampleGroup --json | jq ".[] | select(.operationName.value == \"Microsoft.Authorization/policies/audit/action\")"
