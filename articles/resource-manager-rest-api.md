<properties
   pageTitle="Správce prostředků REST API | Microsoft Azure"
   description="Základní informace o příklady ověřování a použití Správce prostředků REST API"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="navalev"
   manager="timlt"
   editor=""/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/23/2016"
   ms.author="navale;tomfitz;"/>
   
# <a name="resource-manager-rest-apis"></a>Správce prostředků REST API

> [AZURE.SELECTOR]
- [Azure Powershellu](powershell-azure-resource-manager.md)
- [Azure rozhraní příkazového řádku](xplat-cli-azure-resource-manager.md)
- [Portál](./azure-portal/resource-group-portal.md) 
- [ROZHRANÍ REST API](resource-manager-rest-api.md)

Za každý volání na Azure správce prostředků, za Každá šablona nasazeném za každý účet nakonfigurovaný úložiště je jednoho nebo několika volání RESTful rozhraní API Správce prostředků Azure. Toto téma připadá na tyto rozhraní API a jak je můžete volat bez použití libovolné SDK vůbec. To je velmi užitečné, pokud chcete úplné řízení pro všechny žádosti o Azure nebo pokud SDK preferovaný jazyk není k dispozici, které nejsou podporovány operací, že kterou chcete provést.

Tento článek nebude absolvovat každé rozhraní API, které se zobrazí v Azure, ale bude raději použít některé jako příklad, jak Pojďte dále a připojit se k nim. Pokud jste seznámení se základy můžete pokračovat a číst [Azure správce prostředků REST API odkaz](https://msdn.microsoft.com/library/azure/dn790568.aspx) na podrobné informace o používání zbytek rozhraní API.

## <a name="authentication"></a>Ověřování
Ověřování pro ARM má na starosti tak, že Azure Active Directory (AD). Budete moct připojit k jakékoliv rozhraní API, musíte nejdřív ověření s Azure AD pro příjem token ověřování, který můžete přenést každou žádost. Jak jsme s popisem čistého volání přímo do rozhraní REST API, jsme bude taky se předpokládá, že nechcete ověřovat pomocí hesla normální uživatelské jméno, kde může pop-množství-obrazovky výzvu k zadání uživatelského jména a hesla a případně i jiné metody ověřování používané ve dvou scénářů ověřování faktorem. Proto vytvoříme takzvanou Azure AD aplikace a služby jistinu, který bude sloužit k přihlášení s. Ale nezapomeňte, že Azure AD podporují několika postupů ověřování a všem poznámkám lze použít k načítání token ověřování, potřebujeme pro následující požadavky rozhraní API.
[Vytvoření Azure AD aplikace a služby zásady](./resource-group-create-service-principal-portal.md) podle krok za krokem pokyny.

### <a name="generating-an-access-token"></a>Generování přístupový Token 
Ověření Azure AD vystavila společnost volání na Azure AD c:\ login.microsoftonline.com. Pokud chcete ověřit musí mít tyto informace:

* ID klienta Azure AD (jméno tohoto Azure AD používáte přihlásit, často stejný jako vaší společnosti, ale není potřeba)
* ID aplikace (pořízené během krok Azure AD aplikace vytvoření)
* Heslo (, který jste vybrali při vytváření Azure AD aplikace)

V pod žádost HTTP zkontrolujte, že "Azure AD klienta ID", "ID aplikace" a "Heslo" nahraďte správné hodnoty.

**Obecný žádost HTTP:**

```HTTP
POST /<Azure AD Tenant ID>/oauth2/token?api-version=1.0 HTTP/1.1 HTTP/1.1
Host: login.microsoftonline.com
Cache-Control: no-cache
Content-Type: application/x-www-form-urlencoded

grant_type=client_credentials&resource=https%3A%2F%2Fmanagement.core.windows.net%2F&client_id=<Application ID>&client_secret=<Password>
```

... bude (Pokud je ověření úspěšné) výsledkem podobné odpověď na to:

```json
{
  "token_type": "Bearer",
  "expires_in": "3600",
  "expires_on": "1448199959",
  "not_before": "1448196059",
  "resource": "https://management.core.windows.net/",
  "access_token": "eyJ0eXAiOiJKV1QiLCJhb...86U3JI_0InPUk_lZqWvKiEWsayA"
}
```
(Access_token ve výše uvedené odpověď byly zkráceny zvětšíte čitelnosti)

**Generování přístupový token pomocí flám:**

```console
curl -X POST -H "Content-Type: application/x-www-form-urlencoded" -d "grant_type=client_credentials&resource=https://management.core.windows.net&client_id=<application id>&client_secret=<password you selected for authentication>" https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0
```

**Generování přístupový token pomocí Powershellu:**

```powershell
Invoke-RestMethod -Uri https://login.microsoftonline.com/<Azure AD Tenant ID>/oauth2/token?api-version=1.0 -Method Post
 -Body @{"grant_type" = "client_credentials"; "resource" = "https://management.core.windows.net/"; "client_id" = "<application id>"; "client_secret" = "<password you selected for authentication>" }
```

Odpověď obsahuje aplikace Access tokenu, informace o tom, jak dlouho token je platný a informace o jaké zdroje můžete použít token pro.
Přístupový token, který jste dostali v hovoru HTTP musí být předán ve všech požadavku na rozhraní API ARM jako záhlaví s názvem "Se tak mohli ověřovat" s hodnotou "Nosný YOUR_ACCESS_TOKEN". Všimněte si mezery mezi "Nosný" a tokenu váš přístup.

Jak je vidět z výše uvedených výsledek HTTP tokenu platí pro určité období doba, po který má mezipaměti a opětovné použití stejné token. Přestože je možné k ověřování Azure AD pro každého volání rozhraní API, bude velmi neefektivní.

## <a name="calling-arm-rest-apis"></a>Volání ARM REST API

[Tady jsou popsány Azure správce prostředků REST API](https://msdn.microsoft.com/library/azure/dn790568.aspx) čímž je mimo rozsah pro účely tohoto návodu do dokumentu využití a každého každé. Tento si přečtěte následující dokumentaci se použije pouze několik rozhraní API vysvětlit základní použití rozhraní API a poté označovány jste oficiální dokumentaci.

### <a name="list-all-subscriptions"></a>Seznam všech předplatných

Jednou z nejjednodušší operace, které můžete dělat je seznam dostupných předplatných, která se zobrazí. V pod žádost najdete v článku jak tokenu přístup je předaná jako záhlaví.

(Nahraďte YOUR_ACCESS_TOKEN vaše skutečné přístup tokenu.)

```HTTP
GET /subscriptions?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

... jako výsledek, zobrazí se seznam předplatných, která tato služba jistinu bude moct přístup

(ID předplatného pod byly zkráceny pro snazší čitelnost)

```json
{
  "value": [
    {
      "id": "/subscriptions/3a8555...555995",
      "subscriptionId": "3a8555...555995",
      "displayName": "Azure Subscription",
      "state": "Enabled",
      "subscriptionPolicies": {
        "locationPlacementId": "Internal_2014-09-01",
        "quotaId": "Internal_2014-09-01"
      }
    }
  ]
}
```

### <a name="list-all-resource-groups-in-a-specific-subscription"></a>Seznam všech skupin zdrojů v konkrétní předplatného

Dostupné pro rozhraní API ARM všechny zdroje jsou vnořené do skupiny zdrojů. Budeme dotazu ARM pro existující skupiny zdrojů v naší předplatného pomocí pod HTTP GET požádat o. Všimněte si, jak ID předplatného předaná jako součást adresy URL tentokrát.

(YOUR_ACCESS_TOKEN a nahradit ID_ODBĚRU vaší skutečných tokenu přístupu a ID předplatného)

```HTTP
GET /subscriptions/SUBSCRIPTION_ID/resourcegroups?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json
```

Odpověď se zobrazí závisí, jestli máte všechny zdroje skupiny definované a pokud ano, kolik.

(ID předplatného pod byly zkráceny pro snazší čitelnost)

```json
{
    "value": [
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/myfirstresourcegroup",
            "name": "myfirstresourcegroup",
            "location": "eastus",
            "properties": {
                "provisioningState": "Succeeded"
            }
        },
        {
            "id": "/subscriptions/3a8555...555995/resourceGroups/mysecondresourcegroup",
            "name": "mysecondresourcegroup",
            "location": "northeurope",
            "tags": {
                "tagname1": "My first tag"
            },
            "properties": {
                "provisioningState": "Succeeded"
            }
        }
    ]
}
```

### <a name="create-a-resource-group"></a>Vytvoření skupiny prostředků

Pokud jsme jste pouze byla vyhledávání rozhraní API ARM informace, je čas jsme místo toho vytvořit některé zdroje informací a Začneme tím, že je nejjednodušší z nich, skupina zdroje. Následující žádost HTTP vytvoří nové skupiny prostředků v oblasti nebo umístění podle svého výběru a přidá jednu nebo více značek (ukázka pod skutečně pouze přidá jedna značka).

(Nahrazení YOUR_ACCESS_TOKEN, ID_ODBĚRU RESOURCE_GROUP_NAME s skutečné přístup tokenu, ID předplatného a název skupiny zdroje, chcete-li vytvořit)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  }
}
```

Pokud je úspěšná, dostanete podobné odpověď na to

```json
{
  "id": "/subscriptions/3a8555...555995/resourceGroups/RESOURCE_GROUP_NAME",
  "name": "RESOURCE_GROUP_NAME",
  "location": "northeurope",
  "tags": {
    "tagname1": "test-tag"
  },
  "properties": {
    "provisioningState": "Succeeded"
  }
}
```

Skupina zdroje úspěšně jste vytvořili v Azure. Blahopřejeme!

### <a name="deploy-resources-to-a-resource-group-using-an-arm-template"></a>Nasazení materiály ke skupině zdrojů pomocí šablony ARM

S ARM nástroje můžete nasazovat zdrojů pomocí ARM šablon. Šablonu ARM definuje několika zdrojů a jejich závislosti. Pro tento oddíl se jenom předpokladu, že máte zkušenosti s ARM šablony a jednoduše si ukážeme postup rozhraní API hovor zahájíte nasazení jednoho. Podrobné dokumentace ARM šablon najdete tady.

Nasazení šablony ARM mnohem nemá liší jak volat další rozhraní API. Jeden důležitým aspektem je, že nasazení šablony může trvat velmi dlouho, podle toho, co je uvnitř šablonu vyberete, právě vrátí volání rozhraní API a je to pro vás vývojář dotazu pro stav nasazení Pokud chcete zjistit, kdy se provádí nasazení.

V tomto příkladu použijeme veřejně vystavovaných ARM šablony k dispozici na [GitHub](https://github.com/Azure/azure-quickstart-templates). Šablony, které budeme používat nasazení Linux OM oblastí Západ USA. I když je tato šablona bude mít dostupné šablony v úložišti veřejné třeba GitHub, můžete taky vybrat předat úplné šablony jako součást žádost. Všimněte si, že nabízíme hodnoty parametrů v rámci požadavku, který se použijí uvnitř použité šablony.

(Nahraďte ID_ODBĚRU RESOURCE_GROUP_NAME, DEPLOYMENT_NAME, YOUR_ACCESS_TOKEN, GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME, ADMIN_USER_NAME, ADMIN_PASSWORD a DNS_NAME_FOR_PUBLIC_IP hodnoty vhodný pro vaši žádost)

```HTTP
PUT /subscriptions/SUBSCRIPTION_ID/resourcegroups/RESOURCE_GROUP_NAME/providers/microsoft.resources/deployments/DEPLOYMENT_NAME?api-version=2015-01-01 HTTP/1.1
Host: management.azure.com
Authorization: Bearer YOUR_ACCESS_TOKEN
Content-Type: application/json

{
  "properties": {
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json",
      "contentVersion": "1.0.0.0",
    },
    "mode": "Incremental",
    "parameters": {
        "newStorageAccountName": {
          "value": "GLOBALY_UNIQUE_STORAGE_ACCOUNT_NAME"
        },
        "adminUsername": {
          "value": "ADMIN_USER_NAME"
        },
        "adminPassword": {
          "value": "ADMIN_PASSWORD"
        },
        "dnsNameForPublicIP": {
          "value": "DNS_NAME_FOR_PUBLIC_IP"
        },
        "ubuntuOSVersion": {
          "value": "15.04"
        }
    }
  }
}
```

Aby se zlepšila čitelnost tento si přečtěte následující dokumentaci byly vynechány poměrně dlouho odpověď JSON tohoto požadavku na. Odpověď bude obsahovat informace o nasazení šablony, kterou jste právě vytvořili.

