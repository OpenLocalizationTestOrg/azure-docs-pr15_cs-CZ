<properties
    pageTitle="Přidat akci HTTP v aplikacích pro použití logických operátorů | Microsoft Azure"
    description="Základní informace o akce HTTP se vlastnosti"
    services=""
    documentationCenter=""
    authors="jeffhollan"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/15/2016"
   ms.author="jehollan"/>

# <a name="get-started-with-the-http-action"></a>Začínáme s akce HTTP

Akce HTTP můžete rozšířit pracovní postupy pro vaši organizaci a všechny koncový bod komunikovat HTTP.

Můžeš:

- Vytvořte logiky aplikace pracovních postupů, které aktivovat (aktivační události) při web, který je spravován havaruje.
- Všechny koncový bod komunikujte HTTP rozšíříte pracovních postupů do jiných služeb.

Abyste mohli začít používat HTTP akce v aplikaci logiku, v tématu [Vytvoření logiku aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="use-the-http-trigger"></a>Použijte aktivační událost protokolu HTTP

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu, který je definován v aplikaci použití logických operátorů. [Další informace o aktivačních událostí](connectors-overview.md).

Tady je příklad řadu nastavíte aktivační událost protokolu HTTP v Návrháři logiky aplikace.

1. V aplikaci logiky přidáte aktivační událost protokolu HTTP.
2. Vyplňte parametry HTTP koncového bodu, který chcete hlasování.
3. Změna intervalu opakování na frekvence by měl hlasování.
4. Použití logických operátorů aplikaci teď aktivuje s obsahem, který bude vrácena při každé kontrole.

![Nastavit informace HTTP aktivační událost](./media/connectors-native-http/using-trigger.png)

### <a name="how-the-http-trigger-works"></a>Jak funguje aktivační událost protokolu HTTP

Aktivační událost protokolu HTTP chce zavolat na koncový bod HTTP opakované intervalu. Ve výchozím nastavení kód odpovědi HTTP menší než 300 výsledků v aplikaci logiky spustit. V zobrazení kód, který se vyhodnotí po volání HTTP a zjistit, pokud mají spustit aplikaci logiky můžete přidat podmínku. Tady je příklad aktivační události pro HTTP, který se aktivuje, když Vrácený stavový kód je větší než nebo rovno `400`.

```javascript
"Http":
{
    "conditions": [
        {
            "expression": "@greaterOrEquals(triggerOutputs()['statusCode'], 400)"
        }
    ],
    "inputs": {
        "method": "GET",
        "uri": "https://blogs.msdn.microsoft.com/logicapps/",
        "headers": {
            "accept-language": "en"
        }
    },
    "recurrence": {
        "frequency": "Second",
        "interval": 15
    },
    "type": "Http"
}
```

Úplné informace o parametry HTTP aktivační událost jsou dostupné na [webu MSDN](https://msdn.microsoft.com/library/azure/mt643939.aspx#HTTP-trigger).

## <a name="use-the-http-action"></a>Pomocí akce HTTP

Akce je operaci, která provádí pracovního postupu, která je definovaná v aplikaci logiku. [Další informace o akce](connectors-overview.md).

1. Klikněte na tlačítko **Nový krok** .
2. Zvolte **Přidat akci**.
3. Do pole Hledat akce zadejte **http** seznam HTTP akce.

    ![Vyberte akci HTTP](./media/connectors-native-http/using-action-1.png)

4. Přidáte do všech parametrů, které jsou potřeba pro volání HTTP.

    ![Provedení akce HTTP](./media/connectors-native-http/using-action-2.png)

5. Klikněte na levém horním rohu panelu nástrojů na Uložit. Použití logických operátorů aplikace bude jak uložit a publikovat (aktivovat).

## <a name="http-trigger"></a>Nastavit informace HTTP aktivační událost

Tady jsou podrobnosti o aktivační událost, která podporuje tato spojnice. Konektor HTTP obsahuje jednu aktivační událost.

|Aktivační událost|Popis|
|---|---|
|NASTAVIT INFORMACE HTTP|Provede volání HTTP a vrátí obsah odpověď.|

## <a name="http-action"></a>Nastavit informace HTTP akce

Tady jsou podrobnosti o akci, která podporuje tato spojnice. Konektor HTTP obsahuje jeden možných akcí.

|Akce|Popis|
|---|---|
|NASTAVIT INFORMACE HTTP|Provede volání HTTP a vrátí obsah odpověď.|

## <a name="http-details"></a>Podrobnosti o HTTP

Následující tabulka popisuje povinných a nepovinných vstupních polí pro různé akce a výstup podrobností o odpovídající, které jsou spojené s použitím akce.


#### <a name="http-request"></a>Žádost HTTP
Následují vstupních polí pro akce, která zajišťuje odchozí žádost HTTP.
A * znamená, že požadované pole.

|Zobrazované jméno|Název vlastnosti|Popis|
|---|---|---|
|Metoda *|Metoda|Příkaz HTTP používat|
|IDENTIFIKÁTOR URI *|identifikátor URI|Identifikátor URI protokolu HTTP požadavku na|
|Záhlaví|záhlaví|Objekt JSON záhlaví HTTP zahrnout|
|Textu|textu|Požadavek HTTP|
|Ověřování|ověřování|Podrobnosti v části [ověřování](#authentication)|
<br>

#### <a name="output-details"></a>Podrobnosti výstupu

Následují výstup podrobnosti pro odpověď HTTP.

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Záhlaví|objekt|Záhlaví odpovědí|
|Textu|objekt|Objekt odpovědi|
|Stavový kód|Funkce INT|Nastavit informace HTTP stavový kód|

## <a name="authentication"></a>Ověřování

Funkce aplikace použití logických operátorů aplikace služby Azure umožňuje použít různé typy ověření HTTP koncové body. Použijte toto ověření se spojnicemi **HTTP** **[HTTP + Swagger](./connectors-native-http-swagger.md)**a **[HTTP Webhook](./connectors-native-webhook.md)** . Jsou následující typy ověřování, která dokáže nahradit:

* [Základní ověřování](#basic-authentication)
* [Ověření certifikátů klienta](#client-certificate-authentication)
* [Azure Active Directory (Azure AD) OAuth ověřování](#azure-active-directory-oauth-authentication)

#### <a name="basic-authentication"></a>Základní ověřování

Následující objekt ověřování není potřeba pro základní ověřování.
A * znamená, že požadované pole.

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Typ *|Typ|Typ ověřování (musí být `Basic` pro základní ověřování)|
|Uživatelské jméno *|uživatelské jméno|Uživatelské jméno pro ověření|
|Heslo *|heslo|Heslo k ověření|

>[AZURE.TIP] Pokud chcete použít heslo, které nelze načtená z kontingenčního seznamu definici použití `securestring` parametr a `@parameters()` [funkce definici pracovního postupu](http://aka.ms/logicappdocs).

Tak do pole ověření vytvoříte objektu takto:

```javascript
{
    "type": "Basic",
    "username": "user",
    "password": "test"
}
```

#### <a name="client-certificate-authentication"></a>Ověření certifikátů klienta

Následující objekt ověřování není potřeba k ověření certifikátů klienta. A * znamená, že požadované pole.

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Typ *|Typ|Typ ověřování (musí být `ClientCertificate` pro klientské certifikáty SSL)|
|PFX *|PFX|Kódováním Base 64 obsah souboru osobních informací Exchange (PFX)|
|Heslo *|heslo|Heslo pro přístup k souboru PFX|

>[AZURE.TIP] Můžete použít `securestring` parametr a `@parameters()` [funkce definici pracovního postupu](http://aka.ms/logicappdocs) použít parametr, který nebude možné číst při definici po uložení logiky aplikace.

Příklad:

```javascript
{
    "type": "ClientCertificate",
    "pfx": "aGVsbG8g...d29ybGQ=",
    "password": "@parameters('myPassword')"
}
```

#### <a name="azure-ad-oauth-authentication"></a>Azure AD OAuth ověřování

Následující objekt ověřování není potřeba k ověření Azure AD OAuth. A * znamená, že požadované pole.

|Název vlastnosti|Datový typ|Popis|
|---|---|---|
|Typ *|Typ|Typ ověřování (musí být `ActiveDirectoryOAuth` pro Azure AD OAuth)|
|Klienta *|klienta|Identifikátor klienta pro klienta Azure AD|
|Cílové skupiny *|cílové skupiny|Nastavit`https://management.core.windows.net/`|
|Klient ID *|clientId|Identifikátor klienta aplikace Azure AD|
|Tajná *|tajná|Tajná klienta, který požaduje tokenu|

>[AZURE.TIP] Můžete použít `securestring` parametr a `@parameters()` [funkce definici pracovního postupu](http://aka.ms/logicappdocs) použít parametr, který nebude možné číst při definici po uložení.

Příklad:

```javascript
{
    "type": "ActiveDirectoryOAuth",
    "tenant": "72f988bf-86f1-41af-91ab-2d7cd011db47",
    "audience": "https://management.core.windows.net/",
    "clientId": "34750e0b-72d1-4e4f-bbbe-664f6d04d411",
    "secret": "hcqgkYc9ebgNLA5c+GDg7xl9ZJMD88TmTJiJBgZ8dFo="
}
```

## <a name="next-steps"></a>Další kroky

Teď vyzkoušejte si platformu a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumání dalších spojnice k dispozici v aplikacích pro použití logických operátorů hledáním v našem [seznamu rozhraní API](apis-list.md).
