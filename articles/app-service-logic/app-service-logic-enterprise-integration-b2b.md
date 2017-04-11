<properties 
    pageTitle="Vytváření B2B řešení se podnikové integrace Packem | Služba Microsoft Azure aplikací | Microsoft Azure" 
    description="Další informace o příjem dat pomocí funkce B2B Enterprise integrace Pack" 
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

# <a name="learn-about-receiving-data-using-the-b2b-features-of-the-enterprise-integration-pack"></a>Další informace o příjem dat pomocí funkce B2B Enterprise integrace Pack#

## <a name="overview"></a>Základní informace ##

Tento dokument je součástí logiky aplikace Enterprise integrace Pack. Podívejte se na Přehled zobrazíte další informace o [Možnosti sady podnikové integrace](./app-service-logic-enterprise-integration-overview.md).

## <a name="prerequisites"></a>Zjistit předpoklady pro ##

Použití AS2 a X12 akce byste potřebovali integrace účtu organizace

[Jak vytvořit účet podnikové integrace](./app-service-logic-enterprise-integration-accounts.md)

## <a name="how-to-use-the-logic-apps-b2b-connectors"></a>Jak používat logiky aplikace B2B spojnic ##

Jakmile jste vytvořili účet integrace a přidá partnery a smlouvami k němu jste připraveni k vytvoření aplikace použití logických operátorů, který používá pracovního postupu (B2B) business pro firmy.

V tomto popisný návod uvidíte používání AS2 a X12 akce při vytváření aplikace použití logických operátorů business pro firmy, který načítá data z obchodního partnera.

1. Vytvoření nové aplikace logiky a [Připojit se ke svému účtu integrace](./app-service-logic-enterprise-integration-accounts.md).  
2. Přidání aktivační **přijetí žádosti o - požadavek při HTTP** do aplikace pro použití logických operátorů  
![](./media/app-service-logic-enterprise-integration-b2b/flatfile-1.png)  
3. Přidat akci **Dekódovat AS2** tak, že nejprve vyberou **Přidat akci**  
![](./media/app-service-logic-enterprise-integration-b2b/transform-2.png)  
4. Zadejte slovo **as2** ve vyhledávacím poli za účelem filtrování všechny akce té, kterou chcete použít  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-5.png)  
6. Vyberte akci **AS2 - dekódovat AS2 zprávy**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-6.png)  
7. Jak je vidět, přidání **textu** , který bude trvat předávat na vstupu. V tomto příkladu vyberte text žádost HTTP, který spustil aplikaci použití logických operátorů. Můžete také zadat výraz zadat záhlaví v poli**záhlaví** :

    @triggerOutputs()['headers']

8. Přidání **záhlaví** , které jsou potřeba pro AS2. Tyto budou v záhlavích žádost HTTP. V tomto příkladu vyberte záhlaví žádost HTTP, který spustil aplikaci použití logických operátorů.
9. Teď přidejte akce zprávy dekódovat X12 tak, že znovu vyberete **Přidat akci**  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-9.png)   
10. Zadejte slovo **x12** ve vyhledávacím poli za účelem filtrování všechny akce té, kterou chcete použít  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-10.png)  
11. Vyberte **X12-dekódovat X12 zprávy** akci, kterou chcete přidat do aplikace logiky  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-as2message.png)  
12. Teď budete muset zadat vstupů pro tuto akci, která bude výstupní výše AS2 akce. Obsah skutečný zpráva je v objektu JSON a je kódováním Base 64. Proto muset zadat výraz jako vstup tak zadat Tento výraz do pole vstupní **X12 plochá DEKÓDOVAT zprávu do souboru**  

    @base64ToString(body('Decode_AS2_message')?['AS2Message']?['Content'])  

13. Tento krok bude dekódovat X12 dat dostali od obchodního partnera a bude výstupní počet položek v objektu JSON. Pokud chcete, aby partnera vědět přijetí data můžete poslat zpět odpověď obsahující AS2 zprávy dispozice oznámení (MDN) v akci odpověď HTTP  
14. Přidat akci **odpověď** tak, že vyberete **Přidat akci**   
![](./media/app-service-logic-enterprise-integration-b2b/b2b-14.png)  
15. Zadejte slovo **odpověď** do vyhledávacího pole za účelem filtrování všechny akce té, kterou chcete použít  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-15.png)  
16. Vyberte akci **odpověď** , kterou chcete přidat  
![](./media/app-service-logic-enterprise-integration-b2b/b2b-16.png)  
17. Nastavil pole **text** odpovědi pomocí následujícího výrazu pro přístup k MDN z výstupu akce **dekódovat X12 zprávy**  

    @base64ToString(body('Decode_AS2_message')?['OutgoingMdn']?['Content'])  

![](./media/app-service-logic-enterprise-integration-b2b/b2b-17.png)  
18. Uložení prezentace  
![](./media/app-service-logic-enterprise-integration-b2b/transform-5.png)  

V tomto okamžiku dokončíte nastavení B2B logiky aplikace. V aplikaci reálné je vhodné ukládat dekódovanou X12 dat v LOB aplikace nebo datový úložiště. Můžete snadno přidat další akce nebo napište vlastní rozhraní API pro připojení k aplikace LOB a pomocí těchto rozhraní API aplikace logiky.

## <a name="features-and-use-cases"></a>Funkce a případy použití ##

- AS2 X12 dekódovat a kódovat akcí umožňují data z přijímání a odesílání dat na obchodování partnery pomocí standardních protokolů používání aplikací logiky  
- Můžete AS2 a X12 pomocí hesla nebo bez vzájemně pro výměnu dat s obchodními partnery podle potřeby
- Akce B2B usnadňují vytváření partnery a smlouvami účtu integrace a používání v aplikaci použití logických operátorů  
- Prodloužením logiky aplikace pomocí příkazu jiné akce můžete odesílat a přijímat dat a od ostatních aplikací a služeb, jako je služby SalesForce  

## <a name="learn-more"></a>Víc se uč ##

[Další informace o Enterprise Integration Pack](./app-service-logic-enterprise-integration-overview.md)  