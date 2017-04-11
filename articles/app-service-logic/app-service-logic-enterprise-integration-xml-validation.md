<properties 
    pageTitle="Základní informace o ověření XML sady podnikové integrace | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Zjistěte, jak funguje ověřování v aplikacích Enterprise integrace Pack a logika" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/08/2016" 
    ms.author="deonhe"/>

# <a name="enterprise-integration-with-xml-validation"></a>Podnikové integrace se službou ověření XML

## <a name="overview"></a>Základní informace
Často B2B scénáře partnerům umožňuje dohodu musí ověřit, které zprávy, které exchange mezi sebou jsou platné před zahájením zpracování údajů. Integrace sady Enterprise můžete konektoru ověření XML k ověření dokumenty na základě předdefinovaného schéma.  

## <a name="how-to-validate-a-document-with-the-xml-validation-connector"></a>Jak ověřit dokumentu s konci konektoru ověření XML
1. Vytvoření aplikace logiky a [Připojit se ke svému účtu integrace](./app-service-logic-enterprise-integration-accounts.md "informace o propojení klienta integrace logiky aplikaci") , která obsahuje schéma, které se používají k ověření dat XML.
2. Přidání aktivační **přijetí žádosti o - požadavek při HTTP** do aplikace pro použití logických operátorů  
![](./media/app-service-logic-enterprise-integration-xml/xml-1.png)    
3. Přidat akci **Ověření XML** tak, že nejprve vyberou **Přidat akci**  
4. Pokud chcete filtrovat všechny akce té, kterou chcete použít vyhledávacího pole zadejte *xml* 
5. Vyberte **ověření XML**     
![](./media/app-service-logic-enterprise-integration-xml/xml-2.png)   
6. Výběr **obsahu** textové pole  
![](./media/app-service-logic-enterprise-integration-xml/xml-1-5.png)
7. Vyberte značku textu jako obsah, který bude nelze ověřit.   
![](./media/app-service-logic-enterprise-integration-xml/xml-3.png)  
8. Vyberte **Název schématu** seznamem a zvolte schéma, které chcete použít k ověření vstupní *obsah* výše     
![](./media/app-service-logic-enterprise-integration-xml/xml-4.png) 
9. Uložení prezentace  
![](./media/app-service-logic-enterprise-integration-xml/xml-5.png) 

V tomto okamžiku dokončíte nastavení ověření spojnice. V aplikaci skutečném světě je vhodné uložit ověřený data v aplikaci LOB – třeba služby SalesForce. Můžete snadno přidat akci, kterou chcete poslat služby Salesforce výstup ověření. 

Nyní můžete otestovat vzít ověření tím, že žádost HTTP koncový bod.  

## <a name="next-steps"></a>Další kroky

[Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack")   