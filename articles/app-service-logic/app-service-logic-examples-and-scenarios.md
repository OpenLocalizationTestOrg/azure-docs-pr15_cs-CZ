<properties
   pageTitle="Použití logických operátorů aplikace příklady a scénáře | Microsoft Azure"
   description="Příklady běžných logiky aplikace a zjistěte, jak implementovat obvyklé scénáře"
   services="logic-apps"
   documentationCenter=".net,nodejs,java"
   authors="jeffhollan"
   manager="erikre"
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="10/18/2016"
   ms.author="jehollan"/>

# <a name="logic-apps-examples-and-common-scenarios"></a>Příklady použití logických operátorů aplikace a obvyklé scénáře

Tento dokument podrobnosti obvyklé scénáře a příklady vám bychom si vysvětlit některé způsoby je můžete použít aplikace použití logických operátorů k automatizaci obchodních procesů. 

## <a name="custom-triggers-and-actions"></a>Vlastní aktivačních událostí a akce

Existuje několik způsobů, jak aktivovat aplikace použití logických operátorů z jiné aplikace. Tady je několik běžných příkladů:

- [Vytvoření vlastní spouštěcí nebo akce](app-service-logic-create-api-app.md)
- [Akce časově náročné](app-service-logic-create-api-app.md)
- [Aktivace žádost HTTP (publikovat)](app-service-logic-http-endpoint.md)
- [Aktivace Webhook a akce](app-service-logic-create-api-app.md)
- [Aktivace hlasování](app-service-logic-create-api-app.md)

### <a name="scenarios"></a>Scénáře

- [Žádost o synchronní odpověď](app-service-logic-http-endpoint.md)
- [Odpověď žádosti o služby SMS](https://channel9.msdn.com/Blogs/Windows-Azure/Azure-Logic-Apps-Walkthrough-Webhook-Functions-and-an-SMS-Bot)

## <a name="error-handling-and-logging"></a>Protokolování a zpracování chyb

- [Výjimky a zpracování chyb](app-service-logic-exception-handling.md)
- [Konfigurace upozornění Azure a diagnostických nástrojů](app-service-logic-monitor-your-logic-apps.md)

### <a name="scenarios"></a>Scénáře

- [Případu použití: Chyby a výjimek](app-service-logic-scenario-error-and-exception-handling.md)

## <a name="deploying-and-managing"></a>Nasazení a správu

- [Vytvoření automatického nasazení](app-service-logic-create-deploy-template.md)
- [Vytvoření a nasazení aplikace použití logických operátorů z aplikace Visual Studio](app-service-logic-deploy-from-vs.md)
- [Sledování aplikací logiky](app-service-logic-monitor-your-logic-apps.md)

## <a name="content-types-conversions-and-transformations"></a>Typy obsahu, převod a transformací

Použití logických operátorů aplikace [jazyka pro definici pracovního postupu](http://aka.ms/logicappsdocs) obsahuje mnoho funkcí lze převést a pracovat s různých typů obsahu.  Modul navíc udělá všechny že můžete uchovávat typy obsahu jako toky dat pomocí pracovního postupu.

- Například aplikace/json aplikace/xml a prostého [manipulaci s typy obsahu](app-service-logic-content-type.md)
- [Vytváření definice pracovního postupu](app-service-logic-author-definitions.md)
- [Reference jazyka definici pracovního postupu](http://aka.ms/logicappsdocs)

## <a name="batches-and-looping"></a>Listy a opakování

- [SplitOn](app-service-logic-loops-and-scopes.md)
- [ForEach](app-service-logic-loops-and-scopes.md)
- [Až](app-service-logic-loops-and-scopes.md)

## <a name="integrating-with-azure-functions"></a>Integrace s Azure funkcí

- [Azure integrace funkce](app-service-logic-azure-functions.md)

### <a name="scenarios"></a>Scénáře

- [Azure funkce jako služba Bus aktivační událost](app-service-logic-scenario-function-sb-trigger.md)

## <a name="http-rest-and-soap"></a>Nastavit informace HTTP, ZBÝVAJÍCÍ a SOAP

 - [Volání SOAP](https://blogs.msdn.microsoft.com/logicapps/2016/04/07/using-soap-services-with-logic-apps/)


Budeme se přidávejte příklady a scénáře pro tento dokument. Pomocí oddílu komentáře a dejte nám vědět, jaké příklady nebo scénáře byste chtěli vidět tady.