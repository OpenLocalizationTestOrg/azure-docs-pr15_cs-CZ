<properties
pageTitle="Naučte se používat konektoru Bus služby Azure v aplikacích logiku | Microsoft Azure"
description="Vytváření aplikací pro použití logických operátorů službou Azure aplikace. Připojení k Bus služby Azure odesílat a přijímat zprávy. Můžete provádět akce, jako je odeslat do fronty, pošlete téma, dostanete od fronty a přijímat z předplatného."
services="logic-apps"
documentationCenter=".net,nodejs,java"  
authors="msftman"
manager="erikre"
editor=""
tags="connectors" />

<tags
ms.service="logic-apps"
ms.devlang="multiple"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="integration"
ms.date="08/02/2016"
ms.author="deonhe"/>

# <a name="get-started-with-the-azure-service-bus-connector"></a>Začínáme s konektoru Bus služby Azure

Připojení k Bus služby Azure odesílat a přijímat zprávy. Můžete provádět akce, jako je odeslat do fronty, pošlete téma, dostanete od fronty a přijímat z předplatného.

Pokud chcete použít [libovolnou spojnici](./apis-list.md), musíte nejdřív při vytváření aplikace logiky. Jak začít vytvořením [aplikace logiky nyní](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="connect-to-service-bus"></a>Připojení k Bus služby

Pokud aplikace logiky získat přístup k jiné služby, musíte nejdřív vytvořit připojení ke službě. [Připojení](./connectors-overview.md) umožňuje připojení mezi logiky aplikace a další služby.  

>[AZURE.INCLUDE [Steps to create a connection to Azure Service Bus](../../includes/connectors-create-api-servicebus.md)]

## <a name="use-a-service-bus-trigger"></a>Použijte aktivační událost Bus služby

Aktivační událost je u události, mohou sloužit k spuštění pracovního postupu definované v aplikaci použití logických operátorů. [Další informace o aktivačních událostí](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).  

>[AZURE.INCLUDE [Steps to create a Service Bus trigger](../../includes/connectors-create-api-servicebus-trigger.md)]  

## <a name="use-a-service-bus-action"></a>Použití služby Bus akce

Akce je operace prováděné definované v aplikaci logiky pracovního postupu. [Další informace o akce](../app-service-logic/app-service-logic-what-are-logic-apps.md#logic-app-concepts).

[AZURE.INCLUDE [Steps to create a Service Bus action](../../includes/connectors-create-api-servicebus-action.md)]  

## <a name="technical-details"></a>Podrobné technické informace

Tady jsou podrobnosti o aktivačních událostí, akce a odpovědi, které podporuje připojení.

### <a name="service-bus-triggers"></a>Aktivace služby Bus

Služba Bus obsahuje následující aktivačních událostí:  

|Aktivační událost | Popis|
|--- | ---|
|[Při doručení zprávy ve frontě](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-queue)|Tuto operaci spustí tok po přijetí zprávy ve frontě.|
|[Při příjmu zprávy v tématu předplatného](connectors-create-api-servicebus.md#when-a-message-is-received-in-a-topic-subscription)|Tuto operaci spustí tok po přijetí zprávy v tématu předplatné.|


### <a name="service-bus-actions"></a>Služba Bus akce

Služba Bus obsahuje následující akce:


|Akce|Popis|
|--- | ---|
|[Odeslání zprávy](connectors-create-api-servicebus.md#send-message)|Tuto operaci odešle zprávu do fronty nebo tématu.|
### <a name="action-and-trigger-details"></a>Podrobnosti o akce a aktivační událost

Tady jsou podrobnosti pro akce a aktivační události pro tento spojnice, spolu s jejich odpovědi.



#### <a name="send-message"></a>Odeslání zprávy

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|ContentData *|Obsah|Obsah zprávy.|
|Typ obsahu|Typ obsahu|Typ obsahu obsahu zprávy.|
|Vlastnosti|Vlastnosti|Klíč dvojice pro jednotlivá pole brokered vlastnost.|
|Název_entity *|Název fronty nebo tématu|Název fronty nebo tématu.|

Tyto rozšířené parametrů jsou k dispozici:

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|MessageId|Id zprávy|Uživatelem definované hodnotu, která služby Bus slouží k identifikaci duplicitních zpráv, pokud je povoleno.|
|K|K|Adresa odešlete.|
|ReplyTo|Odpovědět|Adresa fronty chcete odpovědět.|
|ReplyToSessionId|Odpověď na Id relace|Identifikátor URI relace, pokud chcete odpovědět.|
|Popisek|Popisek|Specifické pro aplikaci popisek.|
|ScheduledEnqueueTimeUtc|ScheduledEnqueueTimeUtc|Funkce data a času UTC, když se zpráva se přidají do fronty.|
|ID relace|Id relace|Identifikátor relace.|
|CorrelationId|Id korelace|Identifikátor korelace.|
|TimeToLive|Time To Live|Doba trvání, v značky, platí zprávy. Spustí se ze kdy zpráva odeslaná do služby Bus doby trvání.|



* Označuje, že vlastnost není potřeba.


#### <a name="when-a-message-is-received-in-a-queue"></a>Při doručení zprávy ve frontě

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|název_fronty *|Název fronty|Název fronty.|


* Označuje, že vlastnost není potřeba.


##### <a name="output-details"></a>Podrobnosti výstupu

ServiceBusMessage: Tento objekt obsahuje obsahu a vlastností zprávy Bus služby.


| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ContentData|řetězec|Obsah zprávy.|
|Typ obsahu|řetězec|Typ obsahu obsahu zprávy.|
|Vlastnosti|objekt|Klíč dvojice pro jednotlivá pole brokered vlastnost.|
|MessageId|řetězec|Uživatelem definované hodnotu, která služby Bus slouží k identifikaci duplicitní zprávy, pokud je povoleno.|
|K|řetězec|Odešlete na adresu.|
|ReplyTo|řetězec|Adresa fronty chcete odpovědět.|
|ReplyToSessionId|řetězec|Identifikátor URI relace, pokud chcete odpovědět.|
|Popisek|řetězec|Specifické pro aplikaci popisek.|
|ScheduledEnqueueTimeUtc|řetězec|Funkce data a času UTC, když se zpráva se přidají do fronty.|
|ID relace|řetězec|Identifikátor relace.|
|CorrelationId|řetězec|Identifikátor korelace.|
|TimeToLive|řetězec|Doba trvání, v značky, platí zprávy. Spustí se ze kdy zpráva odeslaná do služby Bus doby trvání.|




#### <a name="when-a-message-is-received-in-a-topic-subscription"></a>Při příjmu zprávy v tématu předplatného

|Název vlastnosti| Zobrazované jméno|Popis|
| ---|---|---|
|topicName *|Název tématu|Název tématu.|
|subscriptionName *|Název tématu předplatného|Název tématu předplatného.|


* Označuje, že vlastnost není potřeba.


##### <a name="output-details"></a>Podrobnosti výstupu

ServiceBusMessage: Tento objekt obsahuje obsahu a vlastností zprávy Bus služby.


| Název vlastnosti | Datový typ | Popis |
|---|---|---|
|ContentData|řetězec|Obsah zprávy.|
|Typ obsahu|řetězec|Typ obsahu obsahu zprávy.|
|Vlastnosti|objekt|Klíč dvojice pro jednotlivá pole brokered vlastnost.|
|MessageId|řetězec|Uživatelem definované hodnotu, která služby Bus slouží k identifikaci duplicitních zpráv, pokud je povoleno.|
|K|řetězec|Odešlete na adresu.|
|ReplyTo|řetězec|Adresa fronty chcete odpovědět.|
|ReplyToSessionId|řetězec|Identifikátor URI relace, pokud chcete odpovědět.|
|Popisek|řetězec|Specifické pro aplikaci popisek.|
|ScheduledEnqueueTimeUtc|řetězec|Funkce data a času UTC, když se zpráva se přidají do fronty.|
|ID relace|řetězec|Identifikátor relace.|
|CorrelationId|řetězec|Identifikátor korelace.|
|TimeToLive|řetězec|Doba trvání, v značky, platí zprávy. Spustí se ze kdy zpráva odeslaná do služby Bus doby trvání.|



### <a name="http-responses"></a>Odpovědi na HTTP

Jednu nebo více z následujících kódů stav HTTP se můžete vrátit na předchozí akce a aktivace:

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
[Vytvoření logiky aplikace](../app-service-logic/app-service-logic-create-a-logic-app.md).
