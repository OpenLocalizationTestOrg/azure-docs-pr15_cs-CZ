<properties
pageTitle="SMTP | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Připojení k SMTP odeslání e-mailu."
services="logic-apps"   
documentationCenter=".net,nodejs,java"  
authors="msftman"   
manager="erikre"    
editor=""
tags="connectors" />

<tags
ms.service="app-service-logic"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="07/15/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-smtp-connector"></a>Začínáme s konektoru SMTP

Připojení k SMTP odeslání e-mailu.

Pokud chcete použít [libovolnou spojnici](./apis-list.md), musíte nejdřív při vytváření aplikace logiky. Jak začít vytvořením [aplikace logiky nyní](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-smtp"></a>Připojení k SMTP

Pokud aplikace logiky získat přístup k jiné služby, musíte nejdřív vytvořit *připojení* ke službě. [Připojení](./connectors-overview.md) umožňuje připojení mezi logiky aplikace a další služby. Například budete moct připojit k SMTP, musíte nejdřív SMTP *připojení*. Pokud chcete vytvořit připojení, musíte pověření běžně používané pro přístup ke službě, kterou chcete připojit k. Ano v příkladu SMTP, které jsou potřeba přihlašovací údaje pro název připojení, adresa SMTP serveru a přihlašovací údaje uživatele k vytvoření připojení k SMTP. [Další informace o připojení]()  

### <a name="create-a-connection-to-smtp"></a>Vytvoření připojení k SMTP

>[AZURE.INCLUDE [Steps to create a connection to SMTP](../../includes/connectors-create-api-smtp.md)]

## <a name="use-an-smtp-trigger"></a>Použijte aktivační události pro protokol SMTP

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu definované v aplikaci použití logických operátorů. [Další informace o aktivačních událostí](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

V tomto příkladu protože SMTP, které neobsahují aktivační vlastní, použijeme aktivační událost **služby Salesforce – vytvoření objektu** . Aktivační události aktivuje po vytvoření nového objektu v služby Salesforce. Pro zpět k našemu příkladu jsme budete ji nastavit tak, aby se pokaždé, když je v služby Salesforce vytvořeno nové zájemce, *odeslání e-mailu* akce nastane pomocí konektoru SMTP s oznámení o nové zájemce vzniku.

1. Zadejte *služby salesforce* do vyhledávacího pole v Návrháři logiky aplikace a pak vyberte aktivační událost **služby Salesforce – vytvoření objektu** .  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-1.png)  

2. Ovládací prvek **Po vytvoření objekt** zobrazený.
 ![](../../includes/media/connectors-create-api-salesforce/trigger-2.png)  

3. Vyberte **Typ objektu** a vyberte *vést* ze seznamu objektů. V tomto kroku jsou označující, zda vytváříte aktivační události, které vás upozorní aplikace logiku pokaždé, když se vytvoří nové zájemce v služby Salesforce.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger3.png)  

4. Aktivační událost byl vytvořen.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger-4.png)  

## <a name="use-an-smtp-action"></a>Použití akce SMTP

Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akce](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

Teď aktivační událost byly přidány, SMTP akci, že dojde při vytváření nové zájemce služby Salesforce přidáte takto:

1. Vyberte **+ nový krok** přidat akci, která se mají provedeny, pokud se vytvoří nové zájemce.  
 ![](../../includes/media/connectors-create-api-salesforce/trigger4.png)  

2. Vyberte **Přidat akci**. Toto se otevře, ke které chcete vyhledávací pole, kde můžete vyhledávat všech akcí můžete udělat.  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-2.png)  

3. Zadejte *smtp* vyhledat akce související s SMTP.  

4. Vyberte **SMTP - odeslání e-mailu** jako akce provedeny, pokud se vytvoří nové zájemce. Ovládací prvek bloku akce otevře. Budete muset vytvořit smtp připojení v Návrháři bloku Pokud jste tak již neučinili.  
 ![](../../includes/media/connectors-create-api-smtp/smtp-2.png)    

5. Zadávání informacím požadovanou e-mailu v bloku **SMTP - odeslat E-mail** .  
 ![](../../includes/media/connectors-create-api-smtp/using-smtp-action-4.PNG)  

6. Uložte pro aktivaci pracovní postup.  

## <a name="technical-details"></a>Podrobné technické informace

Tady jsou podrobnosti o aktivačních událostí, akce a odpovědi, které podporuje připojení:

## <a name="smtp-triggers"></a>SMTP aktivačních událostí

SMTP má žádné aktivačních událostí. 

## <a name="smtp-actions"></a>SMTP akce

SMTP obsahuje následující akce:


|Akce|Popis|
|--- | ---|
|[Odeslání e-mailu](connectors-create-api-smtp.md#send-email)|Tuto operaci odešle e-mailu do jednoho nebo více příjemců.|

### <a name="action-details"></a>Podrobnosti o akci

Tady jsou podrobnosti o akce tato spojnice spolu s jeho odpovědi:


### <a name="send-email"></a>Odeslání e-mailu
Tuto operaci odešle e-mailu do jednoho nebo více příjemců. 


|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|K|K|Zadejte e-mailové adresy oddělené středníkem jakorecipient1@domain.com;recipient2@domain.com|
|KOPIE|kopie|Zadejte e-mailové adresy oddělené středníkem jakorecipient1@domain.com;recipient2@domain.com|
|Předmět|Předmět|Předmět e-mailu|
|Textu|Textu|Text e-mailu|
|Z|Z|E-mailovou adresu odesílatele jakosender@domain.com|
|IsHtml|Je ve formátu Html|Odeslání e-mailu ve formátu HTML (true nebo false)|
|Skrytá|Skrytá|Zadejte e-mailové adresy oddělené středníkem jakorecipient1@domain.com;recipient2@domain.com|
|Význam|Význam|Význam e-mailu (vysoké, normální nebo nízká)|
|ContentData|Data obsahu přílohy|Obsah dat (kódováním Base 64 datových proudů a jako-je určený pro řetězec)|
|Typ obsahu|Typ obsahu přílohy|Typ obsahu|
|ContentTransferEncoding|Kódování obsahu přenosu přílohy|Kódování přenosu obsahu (ve formátu Base 64 nebo žádný)|
|Název souboru|Název souboru přílohy|Název souboru|
|Formatting Type|ID obsahu přílohy|Id obsahu|

* Označuje, že není potřeba vlastnost


## <a name="http-responses"></a>Odpovědi na HTTP

Jeden nebo více z následujících kódů stav HTTP se můžete vrátit akcí a aktivační události nad: 

|Jméno|Popis|
|---|---|
|200|Ok|
|202|Přijaté|
|400|Chybný požadavek|
|401|Neoprávněnému|
|403|Zakázáno|
|404|Nenalezeno|
|500|Vnitřní chyba serveru. Došlo k neznámé chybě.|
|Výchozí|Operace se nezdařila.|

## <a name="next-steps"></a>Další kroky
[Vytvoření aplikace logiku](../app-service-logic/app-service-logic-create-a-logic-app.md)