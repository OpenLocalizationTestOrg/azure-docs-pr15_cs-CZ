<properties
    pageTitle="Zjistěte, jak pomocí zakódovat nebo dekódovat textových souborů pomocí aplikace Enterprise integrace Pack a logika | Služba Microsoft Azure aplikací | Microsoft Azure"
    description="Použití funkcí Enterprise integrace Pack a logika aplikace-li zakódovat nebo dekódovat textových souborů"
    services="app-service\logic"
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

# <a name="enterprise-integration-with-flat-files"></a>Podnikové integrace se službou textových souborů

## <a name="overview"></a>Základní informace

Je vhodné kódování XML obsahu před odesláním obchodnímu partnerovi ve scénáři business business (B2B). V aplikaci logiky funkci logiky aplikace služby Azure aplikace, které udělali použijete k plochému souboru kódování spojnici k tomuto. Použití logických operátorů aplikaci, kterou vytvoříte dostane jeho XML obsahu z různých zdrojů, včetně z aktivační události pro HTTP žádost, z jiné aplikace nebo dokonce i z jednoho z mnoha [spojnice](../connectors/apis-list.md). Další informace o použití logických operátorů aplikace najdete v [dokumentaci aplikace použití logických operátorů](./app-service-logic-what-are-logic-apps.md "Další informace o použití logických operátorů aplikace").  

## <a name="how-to-create-the-flat-file-encoding-connector"></a>Jak vytvořit plochému souboru kódování spojnice

Takto přidáte plochému souboru kódování spojnice tak, aby aplikace pro použití logických operátorů:

1. Vytvoření aplikace logiku a [Připojit se ke svému účtu integrace](./app-service-logic-enterprise-integration-accounts.md "informace o propojení klienta integrace logiku aplikaci"). Tento účet obsahuje schéma, které se používají k kódovat dat XML.  
2. Přidání aktivační **přijetí žádosti o - požadavek při HTTP** logiku aplikace.  
![Snímek obrazovky s aktivační události pro výběr](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
3. Přidání plochému souboru kódování akce, následujícím způsobem:

    na. Vyberte znaménko **plus** .

    b. Vyberte odkaz **Přidat akci** (se zobrazí po výběru na symbol plus).

    c. Do pole Hledat zadejte *ploché* filtrovat všechny akce té, kterou chcete použít.

    d. Vyberte možnost **Kódování plochému souboru** ze seznamu.   
![Snímek obrazovky s plochému souboru možnost](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
4. V dialogovém okně **Kódování plochému souboru** vyberte **obsahu** textové pole.  
![Snímek obrazovky obsahu textové pole](./media/app-service-logic-enterprise-integration-flatfile/flatfile-3.png)  
5. Vyberte značku textu jako obsah, který chcete-li zakódovat. Značky body naplní pole obsahu.     
![Snímek obrazovky s značku textu](./media/app-service-logic-enterprise-integration-flatfile/flatfile-4.png)  
6. Vyberte seznamu **Název schématu** a vyberte schéma, které chcete použít kódování vstupní obsahu.    
![Snímek obrazovky schématu název seznamu](./media/app-service-logic-enterprise-integration-flatfile/flatfile-5.png)  
7. Uložte svou práci.   
![Snímek obrazovky uložit ikonu](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)  

V tomto okamžiku dokončíte nastavte si svůj plochému souboru kódování spojnice. V aplikaci skutečném světě je vhodné ukládat zakódovaný data v řádku obchodní aplikací, například služby Salesforce. Nebo můžete odeslat, že zakódovaný dat obchodování partnera. Můžete snadno přidat akci, kterou chcete odeslat výstup kódování akce služby Salesforce nebo obchodního partnera, pomocí jedné z jiných spojnice k dispozici.

Nyní můžete otestovat na spojnici tím, že žádost HTTP koncový bod a včetně obsahu XML v textu žádosti o.  

## <a name="how-to-create-the-flat-file-decoding-connector"></a>Jak vytvořit plochému souboru dekódování spojnice

>[AZURE.NOTE] Tyto kroky, musíte mít soubor schématu už nahráli do integrace účtu.

1. Přidáte aktivační **přijetí žádosti o - požadavek při HTTP** logiky aplikace.  
![Snímek obrazovky s aktivační události pro výběr](./media/app-service-logic-enterprise-integration-flatfile/flatfile-1.png)    
2. Přidání plochému souboru dekódování akce, následujícím způsobem:

    na. Vyberte znaménko **plus** .

    b. Vyberte odkaz **Přidat akci** (se zobrazí po výběru na symbol plus).

    c. Do pole Hledat zadejte *ploché* filtrovat všechny akce té, kterou chcete použít.

    d. Vyberte možnost **Dekódování plochému souboru** ze seznamu.   
![Snímek obrazovky s ploché dekódování soubor možnost](./media/app-service-logic-enterprise-integration-flatfile/flatfile-2.png)   
- Vyberte ovládací prvek **obsahu** . To vytvoří seznam obsahu z předchozích krocích, které vám pomohou jako obsah dekódovat. Všimněte si, že má být použit jako obsah dekódovat neexistuje *textu* z příchozí žádost HTTP. Můžete také zadat obsah dekódovat přímo do ovládacího prvku **obsahu** .     
- Vyberte značku *textu* . Všimněte si, že značku textu je teď v ovládací prvek **obsahu** .
- Vyberte název schéma, které chcete použít dekódovat obsah. Následující obrázek ukazuje, že *OrderFile* je název vybrané schéma. Tento název schématu měli nahraná zohledňovala integrace dříve.

 ![Snímek obrazovky s ploché dekódování soubor dialogovým oknem](./media/app-service-logic-enterprise-integration-flatfile/flatfile-decode-1.png)    
- Uložte svou práci.  
![Snímek obrazovky uložit ikonu](./media/app-service-logic-enterprise-integration-flatfile/flatfile-6.png)    

V tomto okamžiku dokončíte nastavte si svůj plochému souboru dekódování spojnice. V aplikaci skutečném světě je vhodné ukládat dekódovanou data do řádku obchodní aplikací například služby Salesforce. Můžete snadno přidat akci, kterou chcete poslat služby Salesforce výstup dekódovací akce.

Nyní můžete otestovat na spojnici tím, že žádost HTTP koncový bod a včetně obsahu XML, který chcete dekódovat v textu žádosti o.  

## <a name="next-steps"></a>Další kroky
- [Další informace o Enterprise Integration Pack] (./app-service-logic-enterprise-integration-overview.md "Další informace o Enterprise Integration Pack").  
