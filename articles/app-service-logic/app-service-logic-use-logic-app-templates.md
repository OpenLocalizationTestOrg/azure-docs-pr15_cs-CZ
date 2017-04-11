<properties
 pageTitle="Šablony aplikací logiky | Microsoft Azure"
 description="Naučte se používat předem vytvořené šablony aplikací použití logických operátorů k vám pomůžou začít"
 authors="kevinlam1"
 manager="dwrede"
 editor=""
 services="app-service\logic"
 documentationCenter=""/>

<tags
    ms.service="app-service-logic"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/24/2016"
    ms.author="klam"/>

# <a name="logic-app-templates"></a>Šablony aplikací logiky

## <a name="what-are-logic-app-templates"></a>Jaké jsou šablony aplikací logiky

Šablona aplikace logiku je předdefinovaných logiku aplikace, které můžete použít k rychlému vytvoření vlastního pracovního postupu. 

Tyto šablony jsou vhodná k Seznamte se s různých vzorky, které můžete vytvořené pomocí aplikace použití logických operátorů. Buď můžete použít tyto šablony jako-je nebo upravit tak, aby odpovídal nefunguje.

## <a name="overview-of-available-templates"></a>Základní informace o dostupných šablon

Existuje mnoho dostupných šablon aktuálně publikovaných v galerii platformu logiky aplikace. Některé příklad kategorie, jakož i typ spojnice použité v nich jsou vypsané dole.

### <a name="enterprise-cloud-templates"></a>Šablony organizace cloudu
Šablony, které integrace Dynamics CRM, Salesforce, pole, objektů Blob Azure a jiných spojnic pro vaše potřeby cloudu organizace. Několik příkladů z co lze provést tyto šablony patří uspořádání potenciální zákazníky a zálohování dat podnikové soubor.

### <a name="enterprise-integration-pack-templates"></a>Šablony organizace integrace pack
Konfigurace VETER (ověřit, extrahovat, transformace, obohacení, směrování) potrubí příjem X12 upravit dokumentu přes AS2 a transformace na XML, stejně jako se zobrazí zpráva X12 a AS2 manipulace.

### <a name="protocol-pattern-templates"></a>Protokol vzorek šablony
Tyto šablony sestávat z logických aplikace, které obsahují protokol vzorků například odpovědi na žádosti o přes HTTP, jakož i integrace různých FTP a SFTP. Použijte tyto existují, nebo jako základ pro vytvoření složitější vzorce protokol.  

### <a name="personal-productivity-templates"></a>Šablony produktivitu
Vzorky ke zlepšení produktivitu zahrnout šablonu, která nastavení denní připomenutí, důležité pracovní položky se mění na seznamy úkolů a automatizace dlouhé úkolů dolů kroku schválení jednoho uživatele.

### <a name="consumer-cloud-templates"></a>Šablony cloudu příjemce
Jednoduchý šablony, které integrace s sociálních médií služeb, jako je Twitter, časová rezerva a e-mailu, nakonec může posílit sociální média marketingové iniciativy. Tyto se týkají taky šablony například zakalená kopírování, které pomáhají zvýšení produktivity uložením čas strávený na obvykle opakující se úkoly. 

## <a name="how-to-create-a-logic-app-using-a-template"></a>Postup vytvoření logiky aplikace použít šablonu? 

Použití šablony aplikace použití logických operátorů, přejdete do návrháře logiky aplikace. Pokud zadáváte návrháře otevřením existující aplikace použití logických operátorů, aplikaci logiky automaticky načte v zobrazení návrhu. Ale pokud vytváříte novou aplikaci použití logických operátorů, se zobrazí dole na obrazovce.  
 ![](../../includes/media/app-service-logic-templates/template7.png)  

Pomocí této obrazovky buď můžete začít s prázdnou logiky aplikace nebo předdefinovaných šablon. Pokud jste vybrali jednu ze šablon, máte k dispozici další informace. V tomto příkladu jsme šablona *Při vytvoření nového souboru v Dropboxu, zkopírujte ho na OneDrive* .  
 ![](../../includes/media/app-service-logic-templates/template2.png)  

Pokud budete chtít použít šablonu, jednoduše klikněte na tlačítko *pomocí této šablony* . Budete požádáni se přihlásit k účty založené na které spojnic využívá šablonu. Nebo pokud jste dříve vytvořili připojení pomocí těchto spojnic, můžete vybrat pokračovat, jak je vidět níže.  
 ![](../../includes/media/app-service-logic-templates/template3.png)  

Po vytvoření připojení a vyberete *pokračovat*, aplikaci logiky otevře v zobrazení návrhu.  
 ![](../../includes/media/app-service-logic-templates/template4.png)  

V uvedeném příkladu stejně jako v případě mnoha šablonami některé z těchto polí vlastností povinné mohou vyplnit v rámci spojnic. ale některé můžou pořád vyžadovalo hodnotu před správně nasazení aplikace použití logických operátorů. Pokud se pokusíte nasazení bez zadání část chybějící pole, zobrazí se upozornění s chybovou zprávou.

Pokud se chcete vrátit do prohlížeče šablony, klikněte na tlačítko *šablony* v horním navigačním panelu. Přepnutím zpátky do prohlížeče šablony dojít ke ztrátě neuložené průběh. Před přepnutí zpět do šablony prohlížeče, zobrazí se zpráva s upozorněním vás upozorní na to.  
 ![](../../includes/media/app-service-logic-templates/template5.png)  

## <a name="how-to-deploy-a-logic-app-created-from-a-template"></a>Nasazení aplikace logiku vytvořený ze šablony

Po načtení šablony a všechny požadované změny udělal, kliknutím tlačítko v levém horním rohu. Uložení a publikování aplikace pro použití logických operátorů.  
 ![](../../includes/media/app-service-logic-templates/template6.png)  

Pokud chcete další informace o jak přidat další kroky do existující šablony aplikace logiky nebo úpravy obecně, přečtěte si něco víc při [vytváření aplikace logiky](app-service-logic-create-a-logic-app.md).