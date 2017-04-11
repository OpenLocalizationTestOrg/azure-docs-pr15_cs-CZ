<properties
   pageTitle="Příručka k vytvoření šablony řešení pro Marketplace | Microsoft Azure"
   description="Podrobné informace o vytváření, certifikace a nasazení šablony s víc OM obrázek řešení pro nákup na Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

   <tags
      ms.service="marketplace"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="na"
      ms.date="07/27/2016"
      ms.author="hascipio; v-divte" />

# <a name="guide-to-create-a-solution-template-for-azure-marketplace"></a>Příručka k vytvoření šablony řešení pro Azure Marketplace
Po dokončení kroku 1, [Vytvoření účtu a registrace][link-acct-creation], jsme s asistencí můžete při vytváření šablona řešení kompatibilní s prohlížečem Azure na [technické požadavky pro vytvoření šablony řešení](marketplace-publishing-solution-template-creation-prerequisites.md). Teď můžeme vás provede jednotlivými kroky pro vytvoření šablony řešení pro více VMs na [Portál publikování] [ link-pubportal] pro Azure Marketplace.

## <a name="create-your-solution-template-offer-in-the-publishing-portal"></a>Vytvoření vaši nabídku šablony řešení v portál publikování
Přejděte na [https://publish.windowsazure.com](http://publish.windowsazure.com). Při přihlašování k [Portál publikování](https://publish.windowsazure.com/)poprvé používejte stejný účet, u kterého byla zaregistroval vaší společnosti prodávající profilu. Později můžete přidat jakékoli zaměstnanec vaší společnosti jako co správce na portálu pro publikování.

### <a name="1-select-solution-templates"></a>1. Vyberte "Řešení šablon"

  ![Kreslení][img-pubportal-menu-sol-templ]

### <a name="2-create-a-new-solution-template"></a>2. vytvoření nové šablony řešení

  ![Kreslení][img-pubportal-sol-templ-new]

### <a name="3-start-with-topologies"></a>3. začínat topologií
Šablona řešení je nadřazený "objekt" u všech jeho topologií. Více topologií můžete definovat v jedné šablony nabídky a řešení. Když nabídky se posune pracovní, se posune spolu s ostatními jeho topologií. Postupujte podle pokynů k definování vaší nabídky:     

- Vytvoření topologii: "Identifikátor topologie" je obvykle název topologii šablony řešení. Identifikátor topologie je použita v adrese URL, jak je ukázáno v následujícím příkladu:

  Azure Marketplace: http://azure.microsoft.com/marketplace/partners/ {PublisherNamespace} / {OfferIdentifier} {TopologyIdentifier}

  Azure portál: https://portal.azure.com/#gallery/ {PublisherNamespace}. {OfferIdentifier} {TopologyIdentifier}

- Přidáte novou verzi.

### <a name="4-get-your-topology-versions-certified"></a>4. získat topologie verze certifikované
Nahrajte soubor zip, který obsahuje všechny potřebné soubory zřízení této konkrétní verzi topologii. Tento soubor zip musí obsahovat takto:

- *mainTemplate.json* a *createUiDefinition.json* soubor kořenový adresář.
- Propojené šablony a všechny požadované skriptů.

  > [AZURE.TIP] Vaše vývojáři práci o vytváření řešení topologií šablony a jejich oprávnění, firmy a marketingového nebo právní oddělení vaší společnosti pracovat na marketingové a právní obsahu.

## <a name="next-steps"></a>Další kroky
Vytvoření šablony řešení a nahrát soubor zip prosím podle pokynů [Průvodce Marketplace marketingového obsahu](marketplace-publishing-push-to-staging.md) před předání nabídka pracovní. Pokud chcete zobrazit celou sadu marketplace publikování článků, navštivte [Začínáme: jak publikovat nabídka z Azure Marketplace](marketplace-publishing-getting-started.md).

Možná by vás zajímaly tyto související články:

- Obrázky OM: [O obrázky virtuálního počítače v Azure](https://msdn.microsoft.com/library/azure/dn790290.aspx)
- Rozšíření OM: [OM Agent a OM rozšíření přehled](https://msdn.microsoft.com/library/azure/dn832621.aspx) a [rozšíření OM Azure funkce](https://msdn.microsoft.com/library/azure/dn606311.aspx)
- [Vytváření šablon Azure ARM](../resource-group-authoring-templates.md) a [Příklady šablon jednoduché ARM](https://github.com/rjmax/ArmExamples) Azure správce zdrojů:
- Omezení úložiště účtu: [jak Monitor pro omezení úložiště účtu](http://blogs.msdn.com/b/mast/archive/2014/08/02/how-to-monitor-for-storage-account-throttling.aspx) a [Premium úložiště](../storage/storage-premium-storage.md#scalability-and-performance-targets-when-using-premium-storage)

[img-pubportal-menu-sol-templ]:media/marketplace-publishing-solution-template-creation/pubportal-menu-solution-templates.png
[img-pubportal-sol-templ-new]:media/marketplace-publishing-solution-template-creation/pubportal-solution-template-new.png
[link-acct-creation]:marketplace-publishing-accounts-creation-registration.md
[link-pubportal]:https://publish.windowsazure.com
