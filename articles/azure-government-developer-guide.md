<properties 
    pageTitle="Příručka pro vývojáře Azure Government" 
    description="To poskytuje srovnání funkcí a pokyny pro na vývoj aplikací pro státní správu Azure" 
    services="" 
    cloud="gov"
    documentationCenter="" 
    authors="Joharve2" 
    manager="Chrisnie" 
    editor=""/>

<tags 
    ms.service="multiple" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="azure-government" 
    ms.date="10/29/2015" 
    ms.author="jharve"/>


#  <a name="microsoft-azure-government-developer-guide"></a>Příručka pro vývojáře pro státní správu Microsoft Azure 

<p> Microsoft Azure vláda fyzicky a sítě jsou izolovaná výskyt Microsoft Azure.  Tato příručka vývojáři bude poskytování údajů o rozdílech této vývojáře aplikace a Správci by potřeba komunikovat a práce s těmito samostatné oblasti Azure.

<!--Table of contents for topic, the words in brackets must match the heading wording exactly-->


## <a name="in-this-topic"></a>V tomto tématu


+ [Základní informace](#Overview)
+ [Pokyny pro vývojáře](#Guidance)
+ [Funkce aktuálně dostupné v Microsoft Azure pro státní správu](#Features)
+ [Koncový bod mapování](#Endpoint)
+ [Další kroky](#next)


## <a name="Overview"></a>Základní informace

Microsoft Azure pro státní správu je samostatnou instanci služby Microsoft Azure adresování zabezpečení a dodržování předpisů potřeb federal agencies USA, stav a státními orgány po místní a poskytovatelů řešení. Azure Government nabízí fyzické sítě izolace z pouze mimo Spojené government nasazení a poskytuje informacemi o zaměstnancích sledovaná USA. 

Microsoft poskytuje řadu nástrojů k vytvoření a nasazení aplikace cloudu globální služby Microsoft Azure ("globální služby") a Microsoft Azure veřejných služeb společnosti Microsoft.

Při vytváření a nasazení aplikace pro vývojáře služby Azure pro státní správu, namísto globální služby, měli byste vědět důležité rozdíly dvou službách.  Konkrétně kolem nastavení a konfiguraci programovacím prostředím, konfiguraci koncové body, psaní aplikací a nasazení jako služby Azure pro státní správu.

Informace v tomto dokumentu shrnuje tyto rozdíly a doplňující informace dostupné na webu(http://www.azure.com/gov "Azure Government") [Azure Government]a [Microsoft Azure Technická knihovna](http://msdn.microsoft.com/cloud-app-development-msdn "MSDN") na webu MSDN. Oficiální informace možná by vás k dispozici v mnoha jiné umístění, jako [Microsoft Azure Trust Center] (https://azure.microsoft.com/support/trust-center/ "Microsoft Azure Trust Center" /), [Centrum si přečtěte následující dokumentaci Azure](https://azure.microsoft.com/documentation/) a [Azure blogy] (https://azure.microsoft.com/blog/ "Azure blogy" /). 

Tento obsah je určený pro partnery a vývojáři, kteří instalují na Microsoft Azure pro státní správu.



## <a name="Guidance"></a>Pokyny pro vývojáře
Vzhledem k tomu, že většina technické obsahu, která je aktuálně dostupná předpokládá, že jsou aplikace vyvinutý pro službu globální a ne pro Microsoft Azure pro státní správu, je důležité, abyste k zajištění, aby vývojáři vědět důležité rozdíly pro aplikace vyvinuté hostitelem Azure Government.

- Nejdřív mívají služby rozdíly ve funkcích, znamená to, že některé funkce, které jsou v určité oblasti globální služby nemusí být k dispozici v Azure pro státní správu.

- Za druhé týkající se funkcí, které jsou k dispozici v Azure Government, jsou konfigurace rozdíly z globální služby.  Proto přečtěte si ukázkový kód, konfigurace a postup zajistit, aby se vytváření a spouštění prostředí Azure Government Cloud Services.


## <a name="Features"></a>Funkce aktuálně dostupné v Microsoft Azure pro státní správu
Azure aktuálně vláda k dispozici tyto služby ve nám GOV IOWA a nám GOV VIRGINIE oblastech:

- Virtuálních počítačích
- Cloud Services
- Úložiště
- Služby Active Directory
- Plánovač
- Virtuální sítě
- Databáze SQL
- Azure souborů
- Mediální služby
- Přenosy správce
- Služba Bus
- StorSimple
- Redis mezipaměti
- Azure zálohování
- Automatizace
- ExpressRoute
- atd.

Další služby jsou k dispozici a další služby se přidají trvale.  Nejnovější aktualizaci seznamu služeb najdete v článku [oblasti stránku](https://azure.microsoft.com/regions/#services) , která zvýrazní jednotlivé dostupné oblast a svých služeb.  

V současné době GOV Iowa cz a cz GOV Virginie jsou datacentrech podpůrné Azure Government.  Získáte oblasti stránku nad datacentrech aktuální a službách.

## <a name="Endpoint"></a>Koncový bod mapování

Umožňuje vám pomohou při mapování veřejné koncové body Microsoft Azure a databáze SQL Azure Government konkrétní koncové body v následující tabulce.


Typ služby|Azure veřejné|Azure Government
---|---|---
Správa portálu|Manage.windowsazure.com|Manage.windowsazure.us
Obecné|*. windows.net|*. usgovcloudapi.net
Základní|*. core.windows.net|*. core.usgovcloudapi.net
Výpočet|*. cloudapp.net|*. usgovcloudapp.net
Úložiště objektů BLOB|*. blob.core.windows.net|   *. blob.core.usgovcloudapi.net
Úložiště fronty|*. queue.core.windows.net|*. queue.core.usgovcloudapi.net
Úložiště tabulek|*. table.core.windows.net|*. table.core.usgovcloudapi.net
Správa služby|Management.Core.Windows.NET|Management.Core.usgovcloudapi.NET
Databáze SQL|*. database.windows.net|*. database.usgovcloudapi.net
Vyrovnávání zatížení ARM koncový bod|https://Management.Windows.NET|https://Management.usgovcloudapi.NET  

* ARM ověřovat pomocí Azure AD odkazovat [Ověřování žádosti správce prostředků Azure](https://msdn.microsoft.com/library/azure/dn790557.aspx)

## <a name="next"></a>Další kroky

Pokud vás zajímá dozvědět víc a o Azure Government prosím zjistit, jak využijte některé z těchto odkazů.

- **[Zaregistrovat ke zkušební verzi](https://azuregov.microsoft.com/trial/azuregovtrial)**

- **[Získání a přístup k vláda Azure](http://azure.com/gov)**

- **[Přehled Azure Government](/azure-government-overview)**

- **[Blog Azure Government](http://blogs.msdn.com/b/azuregov/)**

- **[Azure dodržování předpisů](https://azure.microsoft.com/support/trust-center/compliance/)**

<!--Anchors-->



<!-- Images. -->

[1]: ./media/azure-government-developer-guide/publisherguide.png


<!--Link references-->
[Link 1 to another azure.microsoft.com documentation topic]: virtual-machines-windows-hero-tutorial.md
[Link 2 to another azure.microsoft.com documentation topic]: web-sites-custom-domain-name.md
[Link 3 to another azure.microsoft.com documentation topic]: storage-whatis-account.md
