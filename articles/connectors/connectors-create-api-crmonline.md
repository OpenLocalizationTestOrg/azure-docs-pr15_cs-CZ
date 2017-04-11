<properties
    pageTitle="Přidání konektoru Dynamics CRM Online ke svým aplikacím logiky | Microsoft Azure"
    description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Dynamics CRM Online poskytovatel připojení poskytuje rozhraní API pro práci s entity na Dynamics CRM Online."
    services="logic-apps"    
    documentationCenter=""     
    authors="MandiOhlinger"    
    manager="erikre"    
    editor="" 
    tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/15/2016"
ms.author="mandia"/>

# <a name="get-started-with-the-dynamics-crm-online-connector"></a>Začínáme s konektoru Dynamics CRM Online
Připojení k Dynamics CRM Online k vytvoření nového záznamu, aktualizujte položky a další. S CRM Online můžete:

- Vytvoření vaše obchodní toku podle data, která se zobrazí z CRM Online. 
- Použití akce odstranit záznam, potřebujete entity a další. Tyto akce se odpověď a zpřístupněte výstup pro jiné akce. Třeba při aktualizaci položky v CRM můžete poslat e-mailu s použitím Office 365.

V tomto tématu se dozvíte, jak pomocí konektoru Dynamics CRM Online v aplikaci použití logických operátorů a také aktivačními událostmi a akce.

>[AZURE.NOTE] Tuto verzi článku platí pro aplikace logiky všeobecně dostupná (GA).

Další informace o použití logických operátorů aplikace, najdete v článku [Co jsou logiky aplikace](../app-service-logic/app-service-logic-what-are-logic-apps.md) a [Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-dynamics-crm-online"></a>Připojení k Dynamics CRM Online

Pokud aplikace logiky získat přístup k jiné služby, nejprve vytvoříte *připojení* ke službě. Připojení umožňuje připojení mezi logiky aplikace a další služby. Například pro připojení k Dynamics, musíte nejdřív Dynamics CRM Online *připojení*. Pokud chcete vytvořit připojení, zadejte přihlašovací údaje, které obvykle používáte pro přístup ke službě, kterou chcete připojit k. Takže Dynamics, zadejte její přihlašovací údaje k účtu Dynamics CRM Online k vytvoření připojení.


### <a name="create-the-connection"></a>Vytvoření připojení

>[AZURE.INCLUDE [Steps to create a connection to Dynamics CRM Online Connection Provider](../../includes/connectors-create-api-crmonline.md)]

## <a name="use-a-trigger"></a>Použijte aktivační událost

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu v aplikaci logiky definované. Aktivace "hlasování" službu na interval, počet_plateb, který chcete. [Další informace o aktivačních událostí](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. V aplikaci logiky zadejte "dynamics" zobrazíte seznam aktivační události:  

    ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)

2. Vyberte **Dynamics CRM Online – vytvoření záznamu**. Pokud připojení už existuje, vyberte organizace a entity z rozevíracího seznamu.

    ![](./media/connectors-create-api-crmonline/select-organization.png)

    Pokud se zobrazí výzva k přihlášení, pak zadejte znaménko podrobnosti o vytvoření připojení. [Vytvoření připojení](connectors-create-api-crmonline.md#create-the-connection) v tomto tématu jsou uvedené kroky. 

    > [AZURE.NOTE] V tomto příkladu použití logických operátorů aplikace spustila vytvoření záznamu. Pokud chcete zobrazit výsledky tohoto aktivační událost, přidáte akci, která se pošle e-mailu. Například přidáte akci Office 365 *e-mailovou zprávu* , která e-mailové zprávy můžete po přidání nového záznamu. 

3. Klikněte na tlačítko **Upravit** a nastavte hodnoty **počet_plateb** a **Interval** . Například pokud chcete aktivační událost dotazování každých 15 minut, pak **frekvence** nastavte na **minuty**a nastavit **Interval** **15**. 

    ![](./media/connectors-create-api-crmonline/edit-properties.png)

4. **Uložte** provedené změny (levého horního rohu panelu). Použití logických operátorů aplikace se uloží a může být automaticky zapnutá.


## <a name="use-an-action"></a>Použití akce

Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akce](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

1. Vyberte znaménko plus. Uvidíte několik možností: **Přidat akci**, **Přidat podmínku**nebo nějakého s **dalšími** možnostmi.

    ![](./media/connectors-create-api-crmonline/add-action.png)

2. Zvolte **Přidat akci**.

3. Do textového pole zadejte "dynamics" zobrazte seznam všech dostupných akcí.

    ![](./media/connectors-create-api-crmonline/dynamics-actions.png)

4. V našem příkladu zvolte **Dynamics CRM Online – aktualizovat záznam**. Pokud připojení už existuje, klikněte na **Název organizace**, **Entita**a další vlastnosti:  

    ![](./media/connectors-create-api-crmonline/sample-action.png)

    Pokud se zobrazí výzva pro informace o připojení, zadejte podrobnosti o vytvoření připojení. [Vytvoření připojení](connectors-create-api-crmonline.md#create-the-connection) v tomto tématu popisuje tyto vlastnosti. 

    > [AZURE.NOTE] V tomto příkladu budeme aktualizovat existující záznam v CRM Online. Výstup jinou aktivační událost umožňuje aktualizovat záznam. Například přidáte aktivační událost SharePoint *Při změně existující položky* . Potom přidejte proces CRM Online *Aktualizovat záznam* , který používá pole služby SharePoint k aktualizaci stávajícího záznamu v CRM Online. 

5. **Uložte** provedené změny (levého horního rohu panelu). Použití logických operátorů aplikace se uloží a může být automaticky zapnutá.


## <a name="technical-details"></a>Podrobné technické informace

## <a name="triggers"></a>Aktivace

|Aktivační událost | Popis|
|--- | ---|
|[Vytvoření záznamu](connectors-create-api-crmonline.md#when-a-record-is-created)|Tok spustí, když se vytvoří objektu v CRM.|
|[Aktualizace záznamu](connectors-create-api-crmonline.md#when-a-record-is-updated)|Spustí tok při změně objektu v CRM.|
|[Při odstranění záznamu](connectors-create-api-crmonline.md#when-a-record-is-deleted)|Tok aktivuje, když objektu v CRM.|


## <a name="actions"></a>Akce

|Akce|Popis|
|--- | ---|
|[Seznam záznamů](connectors-create-api-crmonline.md#list-records)|Tuto operaci získá záznamy entity.|
|[Vytvořte nový záznam](connectors-create-api-crmonline.md#create-a-new-record)|Tuto operaci vytvoří nový záznam entity.|
|[Získaní záznamu](connectors-create-api-crmonline.md#get-record)|Tuto operaci získá zadaný záznam entity.|
|[Odstranění záznamu](connectors-create-api-crmonline.md#delete-a-record)|Tuto operaci odstraní záznam z kolekce entity.|
|[Aktualizujte záznam](connectors-create-api-crmonline.md#update-a-record)|Tuto operaci aktualizuje existující záznam entity.|

### <a name="trigger-and-action-details"></a>Podrobnosti o aktivační události a akce

V této části najdete v článku konkrétní informace o jednotlivých aktivační události a akce, včetně všechny požadované nebo volitelné vstupní vlastnosti a odpovídající výstup přidružené spojnice.

#### <a name="when-a-record-is-created"></a>Vytvoření záznamu
Tok spustí, když se vytvoří se objektu ve CRM. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet faktur od získat (výchozí = 256)|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ItemsList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="when-a-record-is-updated"></a>Aktualizace záznamu
Spustí tok při změně objektu v CRM. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet faktur od získat (výchozí = 256)|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ItemsList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="when-a-record-is-deleted"></a>Při odstranění záznamu
Tok aktivuje, když objektu v CRM. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet faktur od získat (výchozí = 256)|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ItemsList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="list-records"></a>Seznam záznamů
Tuto operaci získá záznamy entity. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|$skip|Přeskočit počet|Počet faktur od přeskočit (výchozí = 0)|
|$top|Získání maximální počet|Maximální počet faktur od získat (výchozí = 256)|
|$filter|Filtrování dotazu|Dotaz filtru ODATA omezení položky vrácené|
|$orderby|Řadit podle|Dotaz ODATA řadit podle určete pořadí položek|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
ItemsList

| Název vlastnosti | Datový typ |
|---|---|
|Hodnota|pole|


#### <a name="create-a-new-record"></a>Vytvořte nový záznam
Tuto operaci vytvoří nový záznam entity. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="get-record"></a>Získaní záznamu
Tuto operaci získá zadaný záznam entity. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|ID *|Identifikátor položky|Zadejte identifikátor záznamu|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


#### <a name="delete-a-record"></a>Odstranění záznamu
Tuto operaci odstraní záznam z kolekce entity. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|ID *|Identifikátor položky|Zadejte identifikátor záznamu|

Hvězdička (*) znamená, že vlastnost je povinný.


#### <a name="update-a-record"></a>Aktualizujte záznam
Tuto operaci aktualizuje existující záznam entity. 

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|datovou sadu *|Název organizace|Název organizace CRM, třeba Contoso|
|Tabulka *|Název entity|Název entity|
|ID *|Identifikátor záznamu|Zadejte identifikátor záznamu|

Hvězdička (*) znamená, že vlastnost je povinný.

##### <a name="output-details"></a>Podrobnosti výstupu
Žádná.


## <a name="http-responses"></a>Odpovědi na HTTP

Jeden nebo více z následujících kódů stav HTTP se můžete vrátit akcí a aktivačních událostí: 

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněným|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě.|
|Výchozí|Operace se nezdařila.|


## <a name="next-steps"></a>Další kroky

[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md). Prozkoumejte jiných spojnice k dispozici v aplikacích pro použití logických operátorů v našem [seznamu rozhraní API](apis-list.md).

