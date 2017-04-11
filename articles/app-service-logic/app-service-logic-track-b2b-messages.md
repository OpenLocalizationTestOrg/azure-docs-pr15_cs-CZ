<properties 
   pageTitle="Sledování zpráv B2B v logických aplikace v aplikaci služby Azure | Microsoft Azure" 
   description="Toto téma popisuje sledování B2B zpracování" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>


# <a name="track-b2b-messages"></a>Sledování zpráv B2B

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

## <a name="b2b-tracking-information"></a>Informace o sledování B2B
Komunikace B2B zahrnuje zpracování mezi obchodními partnery zprávy. Vztahy se definují takhle dohod mezi dvěma obchodními partnery. Jakmile je vytvořeno komunikace, je třeba způsob, jak sledovat, pokud komunikace se děje podle očekávání. 

Jsme implementovali sledování zpráv v následujících situacích B2B: AS2, EDIFACT a X12.

## <a name="as2"></a>AS2
Po vytvoření instance aplikace rozhraní API AS2 vyhledejte danou instanci a vyberte **Sledování**. V tomto dokumentu můžete zobrazit a filtrovat všechny informace o sledování AS2:  

![][1]  

## <a name="edifact"></a>EDIFACT
Po vytvoření instance aplikace rozhraní API EDIFACT vyhledejte danou instanci a vyberte **Sledování**. V tomto dokumentu můžete zobrazit a filtrovat všechny EDIFACT informace o sledování. Kromě toho můžete zobrazit výměny úroveň, úrovně seskupení a transakce nastavení úrovně dat v jednom zobrazení. 

Pokud listy vytvořené v rámci EDIFACT dohod v aplikaci přidružené rozhraní API správy obchodování partnera, Batching části jsou uvedeny všechny tyto listy. Můžete vybrat dávky zobrazíte aktivní zprávu (pokud existuje) a také informace o kompletní:  

![][2]      

## <a name="x12"></a>X12
Po vytvoření instance X12 rozhraní API aplikace, vyhledejte danou instanci a vyberte **Sledování**. V tomto dokumentu můžete zobrazit a filtrovat všechny X12 sledování informací. Kromě toho můžete zobrazit výměny úroveň, úrovně seskupení a transakce nastavení úrovně dat v jednom zobrazení.

Pokud se vytvářejí listy jako součást X12 smlouvami v aplikaci přidružené obchodování partnera správy rozhraní API aplikace a potom v části Batching uvádí všechny tyto listy. Můžete vybrat dávky zobrazíte aktivní zprávu (pokud existuje) a také informace o dokončení listy.

<!--Image references-->
[1]: ./media/app-service-logic-track-b2b-messages/AS2Tracking.png
[2]: ./media/app-service-logic-track-b2b-messages/EDIFACTTracking.png
