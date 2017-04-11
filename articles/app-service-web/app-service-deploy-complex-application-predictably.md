<properties
    pageTitle="Zřízení a nasazení microservices předvídatelností v Azure"
    description="Naučte se nasadit aplikaci skládající se ze microservices v aplikaci služby Azure jako jednu jednotku a předvídatelná způsobem pomocí šablony skupiny JSON zdroje a skriptů Powershellu."
    services="app-service"
    documentationCenter=""
    authors="cephalin"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/06/2016"
    ms.author="cephalin"/>


# <a name="provision-and-deploy-microservices-predictably-in-azure"></a>Zřízení a nasazení microservices předvídatelností v Azure #

Tento kurz ukazuje, jak zřídit a nasazení aplikace skládající se ze [microservices](https://en.wikipedia.org/wiki/Microservices) v [Aplikaci služby Azure](/services/app-service/) jako jednu jednotku a předvídatelná způsobem pomocí šablony skupiny JSON zdroje a skriptů Powershellu. 

Při vytváření a nasazení aplikace maximum měřítko, které se skládají z vysoce oddělené microservices, opakovatelnosti a předvídatelnost jsou důležité pro úspěšné. [Aplikace služby Azure](/services/app-service/) umožňuje vytvářet microservices obsahující webových aplikací web apps, mobilní aplikace, rozhraní API aplikace a logika aplikace. [Správce prostředků Azure](../azure-resource-manager/resource-group-overview.md) umožňuje spravovat všechny microservices jako celek, spolu s závislosti například databáze a zdroje nastavení ovládacích prvků. Teď můžete taky nasadíte tyto aplikace pomocí šablony JSON a jednoduché skriptů Powershellu. 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="what-you-will-do"></a>Co bude dělat ##

V tomto kurzu se nasadit aplikaci, která obsahuje:

-   Dvě webové aplikace (tedy dvou microservices)
-   Back-end databáze SQL
-   Nastavení aplikace, připojovací řetězec a ovládacího prvku zdroje
-   Přehledy aplikace, upozornění, nastavení neobsahovaly text

## <a name="tools-you-will-use"></a>Nástroje, které budete používat ##

V tomto kurzu použijete následující nástroje. Protože se nejedná podrobný popis v nabídce Nástroje, budu aby na začátku do konce scénář a právě poskytují stručný úvod ke každému, a kde lze najít další informace v něm. 

### <a name="azure-resource-manager-templates-json"></a>Azure správce prostředků šablony (JSON) ###
 
Pokaždé, když v aplikaci služby Azure vytvoříte do webových aplikací, například správce prostředků Azure pomocí šablony JSON vytvoří skupiny celý zdrojů pomocí prostředků komponent. Komplexní šablony z [Webu Azure Marketplace](/marketplace) jako aplikaci [Scalable WordPress](/marketplace/partners/wordpress/scalablewordpress/) může obsahovat databáze MySQL, úložiště účty, plán služeb aplikací v prohlížeči sebe sama pravidel výstrah, nastavení aplikace, nastavení automatické měřítko a další a všechny tyto šablony jsou k dispozici prostřednictvím Powershellu. Informace o tom, jak stáhnout a používat tyto šablony najdete v článku [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md).

Další informace o šablonách správce prostředků Azure najdete v článku [Vytváření šablon správce prostředků Azure](../resource-group-authoring-templates.md)

### <a name="azure-sdk-26-for-visual-studio"></a>Azure SDK 2.6 for Visual Studio ###

Nejnovější SDK obsahuje vylepšení v editoru JSON šablony podporu správce prostředků. Slouží k rychle vytvořit šablonu zdroje skupiny od začátku nebo otevřete existující šablony JSON (například šablonu staženou galerie) pro úpravy, naplnění souboru parametrů a i nasazení skupina zdroje přímo z Azure pole Skupina zdroje řešení.

Další informace najdete v tématu [Azure SDK 2.6 for Visual Studio](/blog/2015/04/29/announcing-the-azure-sdk-2-6-for-net/).

### <a name="azure-powershell-080-or-later"></a>Azure Powershellu 0.8.0 nebo novější ###

Od začátku roku verze 0.8.0 instalace prostředí PowerShell Azure obsahuje modul Azure správce prostředků kromě modul Azure. Tento nový modul umožňuje skriptu nasazení skupiny zdrojů.

Další informace najdete v tématu [přes Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md)

### <a name="azure-resource-explorer"></a>Průzkumník Azure zdroje ###

Tento [Nástroj pro zobrazení náhledu](https://resources.azure.com) umožňuje zkoumat definice JSON všech skupin zdrojů v předplatného a jednotlivých zdrojů. Pomocí nástroje můžete upravovat definice JSON zdroje, odstranit celou hierarchii zdrojů a vytváření nových zdrojů.  Snadno dostupné v tomto nástroji informace jsou velmi užitečné pro vytváření šablon, protože ho uvidíte, které vlastnosti je třeba nastavit pro určitý typ zdroje správné hodnoty, atd. Můžete dokonce vytvořit skupina zdroje na [Portálu Azure](https://portal.azure.com/)a potom zkontrolovat definici JSON v nástroji explorer vám templatize skupina zdroje.

### <a name="deploy-to-azure-button"></a>Nasazení Azure tlačítku ###

Pokud používáte GitHub ovládacího prvku zdroje, můžete umístit [nasazení Azure tlačítko](/blog/2014/11/13/deploy-to-azure-button-for-azure-websites-2/) do svého README. MD, což umožňuje zapnout klíč nasazení uživatelského rozhraní pro Azure. Když můžete to udělat pro všechny jednoduché v prohlížeči, můžete rozšířit umožníte nasazení celý skupina uložením souboru azuredeploy.json do kořenové úložiště. Tento soubor JSON, který obsahuje šablony skupina zdroje, použijí nasazení Azure tlačítko Vytvořit skupiny zdrojů. Příklad naleznete v tématu [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) vzorku, který budete používat v tomto kurzu.

## <a name="get-the-sample-resource-group-template"></a>Získání Ukázková šablona skupina zdroje ##

Nyní nastavíme doprava ji.

1.  Přejděte na ukázku [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) aplikaci služby.

2.   V readme.md klikněte na **Deploy Azure**.
 
3.  Jste dostali na web [nasazení azure](https://deploy.azure.com) a výzva k zadání parametrů nasazení. Všimněte si, že většina polí jsou vyplněné názvu úložiště a některých řetězcích náhodné za vás. Všechna pole můžete změnit, pokud chcete, ale jenom věci, které musíte zadat jsou SQL Server pro správu přihlašovací jméno a heslo a potom klikněte na tlačítko **Další**.
 
    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-1-deploybuttonui.png)

4.  Klepnutím na tlačítko **instalovat** k zahájení procesu nasazení. Jakmile probíhá dokončen, klikněte na http://todoapp*XXXX*. azurewebsites.net odkaz Procházet nasazení aplikace. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-2-deployprogress.png)

    V uživatelském rozhraní má být trochu pomalé při prvním procházení k němu protože aplikace jenom spouštění, ale sami přesvědčit, že je plně funkční aplikace.

5.  Zpátky na stránce domény v nasazení klikněte na odkaz **Spravovat** zobrazíte nové aplikace v portálu Azure.

6.  V rozevíracím seznamu **Základy** klikněte na odkaz skupina zdroje. Všimněte si také, že web appu připojeni k úložišti GitHub v části **Externí projekt**. 

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-3-portalresourcegroup.png)
 
7.  Ve skupině zásuvné zdroje Všimněte si, že již existují dva web apps a jeden SQL databáze ve skupině prostředků.

    ![](./media/app-service-deploy-complex-application-predictably/gettemplate-4-portalresourcegroupclicked.png)
 
Veškerý obsah, který jste právě viděli v krátkou chvíli aplikace plně nasazeném dvě microservice s všechny komponenty, závislosti, nastavení, databáze a nepřetržitý publikování nastavil automatické průběhu v Azure správce. To všechno platné dvě věci:

-   Nasazení Azure tlačítku
-   azuredeploy.JSON v kořenovém adresáři repo

Můžete nasadit stejná aplikace desítky, stovky nebo tisíce časy a přesné stejná konfigurace pokaždé, když. Opakovatelnost a předvídatelnost tento přístup umožňují nasazení aplikace maximum měřítko snadné a spolehlivosti.

## <a name="examine-or-edit-azuredeployjson"></a>Prozkoumat (nebo upravit) AZUREDEPLOY. JSON ##

Teď Pojďme se podívat na způsobu nastavení GitHub úložiště. Si budete moct pomocí editoru JSON v Azure .NET SDK, takže pokud jste ještě nenainstalovali [Azure .NET SDK 2.6](/downloads/)Uděláte to.

1.  Klonovat úložišti [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) nástrojem Oblíbené libovolná. V následující snímek můžu mi to v Průzkumníkovi týmu ve Visual Studiu 2013.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-1-vsclone.png)

2.  V kořenovém úložiště otevřete azuredeploy.json ve Visual Studiu. Pokud nevidíte podokno Obrys JSON, budete muset nainstalovat Azure .NET SDK.

    ![](./media/app-service-deploy-complex-application-predictably/examinejson-2-vsjsoneditor.png)

Nechci, který bude popisují všechny podrobnosti formátu JSON, ale [Více zdrojů](#resources) oddíl obsahuje odkazy pro výuku jazyk šablony skupina zdroje. Tady stačí budu znázorňují zajímavých funkcí, které vám pomůžou začít pracovat při vlastní šablony pro nasazení aplikace.

### <a name="parameters"></a>Parametry ###

Podívejte se na části parametry se, že většina tyto parametry je co na tlačítko **instalovat na Azure** vás vyzve, abyste při zadávání. Na webu za na tlačítko **instalovat na Azure** naplní zadání uživatelského rozhraní pomocí parametrů v azuredeploy.json definované. Tyto parametry se používají v definice zdroje, jako je například názvy zdrojů nemovitostí s hodnotou apod.

### <a name="resources"></a>Zdroje informací ###

Uzel zdrojů uvidíte, že je definován 4 nejvyšší úrovně zdrojů, včetně instanci systému SQL Server, plán služeb aplikací a dvě web apps. 

#### <a name="app-service-plan"></a>Plán služeb aplikací ####

Začněme s jednoduché kořenové úrovni zdroje v ve formátu JSON. V přehledu uvést JSON klikněte na plán služeb aplikací s názvem **[hostingPlanName]** zvýrazněte odpovídající kód JSON. 

![](./media/app-service-deploy-complex-application-predictably/examinejson-3-appserviceplan.png)

Všimněte si, že `type` element určuje řetězec pro plán aplikaci služby (je označená jako serverové farmy dlouhý, long dobou) a další prvky vlastnosti vyplnit a pomocí parametrů v souboru JSON definované a tento zdroj nemá vnořené zdroje.

>[AZURE.NOTE] Všimněte si také, hodnoty `apiVersion` říká Azure, jakou verzi systému rozhraní REST API používat definici JSON zdroje s a může mít vliv na způsob formátování zdroje uvnitř `{}`. 

#### <a name="sql-server"></a>SQL Server ####

Potom klikněte na serveru SQL Server zdroje s názvem **SQL Server** v přehledu uvést JSON.

![](./media/app-service-deploy-complex-application-predictably/examinejson-4-sqlserver.png)
 
Poznámka: Toto zvýrazněný JSON kód:

-   Použití parametrů zaručuje, že jsou vytvořené zdroje s názvem a nakonfigurovali způsobem, který vytváří konzistentní k sobě navzájem.
-   Dvěma vnořených zdroje má zdroj SQL Server, každá s jinou hodnotu pro `type`.
-   Vnořené zdroje uvnitř `“resources”: […]`, kde jsou definovány databázi a pravidla brány firewall, máte `dependsOn` prvek, který určuje číslo ID zdroje kořenové úrovni zdroje, SQL Server. Toto říká správce prostředků Azure "před vytvořením tento zdroj, který jiného zdroje, již musí existovat; a pokud daný zdroj je definováno v šabloně, vytvořte této nejdřív".

    >[AZURE.NOTE] Podrobné informace o používání `resourceId()` fungovat najdete v článku [Funkce šablony Azure správce prostředků](../resource-group-template-functions.md).

-   Vliv `dependsOn` element je správce prostředků Azure můžete vědět, které zdroje lze vytvořit paralelně a prostředků, které je třeba vytvořit postupně. 

#### <a name="web-app"></a>V prohlížeči ####

Teď Pojďme přejděte k skutečné webovými aplikacemi Web apps, které jsou složitější. Klikněte na web app [variables('apiSiteName')] v přehledu uvést JSON zvýrazněte jeho kód JSON. Zjistíte, že se zobrazují věci mnohem víc zajímavé. K tomuto účelu můžu budete komunikovat o funkcích po jednom:

##### <a name="root-resource"></a>Kořenové zdroje #####

Web appu, závisí na dvou různých zdrojů. To znamená, že správce prostředků Azure vytvoří web appu až po vytvoření plán služeb aplikací a instanci systému SQL Server.

![](./media/app-service-deploy-complex-application-predictably/examinejson-5-webapproot.png)

##### <a name="app-settings"></a>Nastavení aplikace #####

Nastavení aplikace je definována jako vnořené zdroje.

![](./media/app-service-deploy-complex-application-predictably/examinejson-6-webappsettings.png)

V `properties` prvek pro `config/appsettings`, máte dvě nastavení aplikace ve formátu `“<name>” : “<value>”`.

-   `PROJECT`je [KUDU nastavení](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) , která sděluje Azure nasazení, který projekt používat ve Visual Studiu řešení více projektů. Můžu se dozvíte dál zdrojového konfigurací, ale od ToDoApp kód je ve Visual Studiu řešení více projektů, potřebujeme toto nastavení.
-   `clientUrl`je jednoduše aplikace nastavení kód aplikace používá.

##### <a name="connection-strings"></a>Připojovací řetězec #####

Připojení řetězce, je definována jako vnořené zdroje.

![](./media/app-service-deploy-complex-application-predictably/examinejson-7-webappconnstr.png)

V `properties` prvek pro `config/connectionstrings`, každý připojovací řetězec se taky rozumí pár název: hodnota s konkrétní formát `“<name>” : {“value”: “…”, “type”: “…”}`. Pro `type` elementu možné hodnoty jsou `MySql`, `SQLServer`, `SQLAzure`, a `Custom`.

>[AZURE.TIP] Konečné seznam typů řetězec připojení, spusťte tento příkaz v prostředí PowerShell Azure: \[Enum]::GetNames("Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.DatabaseType")
    
##### <a name="source-control"></a>Ovládací prvek zdroje #####

Nastavení správy zdrojů je definována jako vnořené zdroje. Azure správce prostředků používá tyto materiály ke konfiguraci nepřetržitý publikování (viz výstrahou na `IsManualIntegration` později) a také a spusťte tak nasazení kódu aplikace automaticky během zpracování souboru JSON.

![](./media/app-service-deploy-complex-application-predictably/examinejson-8-webappsourcecontrol.png)

`RepoUrl`a `branch` by měl být poměrně intuitivní a odkazovat libovolná úložiště a název pobočky publikovat z. Znovu definovaní vstupní parametry. 

Poznámka: v `dependsOn` prvek, kromě zdroje webové aplikace, sebe sama `sourcecontrols/web` také závisí na `config/appsettings` a `config/connectionstrings`. Důvodem je, že po `sourcecontrols/web` je nakonfigurované, procesu Azure nasazení automaticky pokusí nasadit, vytvořit a spustit kód aplikace. Proto vložení tuto závislost vám pomůže aplikaci před spuštěním kód aplikace musí mít přístup k nastavení požadovaných aplikace a připojovací řetězec. 

>[AZURE.NOTE] Všimněte si také, že `IsManualIntegration` je nastavený na `true`. Tato vlastnost je nezbytné v tomto kurzu, protože nejste vlastníkem skutečně GitHub úložiště a tedy nelze udělit skutečně oprávnění Azure konfigurace (tedy nepřetržitý publikování z [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) nabízet úložiště automatické aktualizace Azure). Použijte výchozí hodnotu `false` pro zadané úložiště jenom v případě, že jste nakonfigurovali vlastníka GitHub pověření [Azure portálu](https://portal.azure.com/) před. Jinými slovy Pokud jste nastavili zdrojového GitHub nebo BitBucket aplikace [Portál Azure](https://portal.azure.com/) dříve, pomocí svých přihlašovacích údajů uživatele pak Azure bude pamatovat pověření a jejich použití pokaždé, když nasadíte nějakou aplikaci z GitHub nebo BitBucket v budoucnu. Ale pokud jste to ještě neudělali, nasazení šablony JSON se nepovede Pokud správce prostředků Azure pokusí konfigurovat nastavení správy zdrojů web appu, protože nemůžete přihlásit do GitHub nebo BitBucket s vlastníka úložiště pověření.

## <a name="compare-the-json-template-with-deployed-resource-group"></a>Porovnání JSON šablony se skupina nasazeném zdroje ##

Tady můžete procházet listy všechny webové aplikace na [Portál Azure](https://portal.azure.com/), ale existuje jiný nástroj, který není stejně jako užitečné, když informace. Přejděte na nástroj Náhled [Explorer Azure zdroje](https://resources.azure.com) , který vám formátu JSON všech skupin zdrojů v předplatných, ve skutečnosti platných v Azure back-end. Můžete taky najdete v článku jak odpovídá skupina zdroje JSON hierarchie v Azure pomocí hierarchie v souboru šablony, která slouží k vytvoření.

Třeba když mám přejděte na nástroj [Explorer zdroje Azure](https://resources.azure.com) , rozbalte uzel v Průzkumníku vidím skupina zdroje a kořenové úrovni prostředky, které byly shromážděny podle typů odpovídajících zdrojů.

![](./media/app-service-deploy-complex-application-predictably/ARM-1-treeview.png)

Pokud jste procházet hierarchii na web appu, by měl být vidět webové aplikace podrobnosti o konfiguraci podobně jako pod snímek:

![](./media/app-service-deploy-complex-application-predictably/ARM-2-jsonview.png)

Znovu vnořené zdroje by měla být hodně podobají profilům ve soubor šablony JSON hierarchie a byste měli vidět nastavení aplikace, připojovací řetězec, atd., správně projeví v podokně JSON. Absence nastavení mohou poukazovat problém se souborem JSON a vám můžou pomoct řešit soubor šablony JSON.

## <a name="deploy-the-resource-group-template-yourself"></a>Nasazení šablony skupina zdroje sami ##

Tlačítko **instalovat na Azure** je skvělý, ale umožňuje nasazení šablony skupina zdroje v azuredeploy.json jenom v případě, že jste již posune azuredeploy.json GitHub. Azure .NET SDK také poskytuje nástroje můžete nasazovat libovolný soubor šablony JSON přímo z místního počítače. K tomuto účelu postupujte následujícím způsobem:

1.  Ve Visual Studiu, klikněte na **soubor** > **Nový** > **projektu**.

2.  Klikněte na tlačítko **Visual Basic** > **cloudu** > **Azure pole Skupina zdroje**, klikněte na tlačítko **OK**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-1-vsproject.png)

3.  V dialogovém okně **Vybrat šablonu Azure**vyberte **Prázdné šablony** a klikněte na **OK**.

4.  Přetáhněte azuredeploy.json do složky **šablony** nového projektu.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-2-copyjson.png)

5.  V Průzkumníku Otevřete zkopírovaný azuredeploy.json.

6.  Jenom za účelem ukázka přidáte některé standardní aplikace přehled zdrojů náš JSON soubor kliknutím na **Přidat zdroje**. Pokud vás zajímá jenom nasazení JSON souboru, přejděte ke kroků nasazení.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-3-newresource.png)

7.  Vyberte **Přehledy aplikace pro Web Apps**, ujistěte se stávající služba aplikace plánování a web aplikace zaškrtnuté a klikněte na **Přidat**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-4-newappinsight.png)

    Teď budete moct zobrazit několik nových zdrojů, podle toho, zdroje a akce, jsou závislé na plán služeb aplikací nebo web app. Existující definici nejsou povolené tyto materiály a budete chtít změnit.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-5-appinsightresources.png)
 
8.  V přehledu uvést JSON klikněte na **appInsights automatické měřítko** zvýrazněte jeho kód JSON. Jedná se o měřítka nastavení pro aplikaci služby plán.

9.  Zvýrazněný JSON kód, vyhledejte `location` a `enabled` vlastnosti a jak je ukázáno v následujícím příkladu je nastavit.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-6-autoscalesettings.png)

10. V přehledu uvést JSON klikněte na **CPUHigh appInsights** zvýrazněte jeho kód JSON. Toto je upozornění.

11. Vyhledejte `location` a `isEnabled` vlastnosti a jak je ukázáno v následujícím příkladu je nastavit. Stejně postupujte i v u jiných tři oznámení (fialový žárovky).

    ![](./media/app-service-deploy-complex-application-predictably/deploy-7-alerts.png)

12. Teď jste připraveni nasazení. Klikněte pravým tlačítkem myši projektu a vyberte **instalovat** > **Nové nasazení**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-8-newdeployment.png)

13. Přihlaste se k účtu Azure, pokud jste tak již neučinili.

14. Vyberte existující skupinu zdrojů ve vašem předplatném nebo vytvořte nový účet, vyberte **azuredeploy.json**a potom klikněte na **Upravit parametry**.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-9-deployconfig.png)

    Teď budete moct upravit všechny parametry definice v souboru šablony v hodní tabulky. Parametry, které definují výchozí už mají výchozí hodnoty a parametrů, které definují seznamu Povolené hodnoty budou zobrazeny jako rozevírací seznamy.

    ![](./media/app-service-deploy-complex-application-predictably/deploy-10-parametereditor.png)

15. Vyplňte všechny prázdné parametry a použijte [adresu repo GitHub ToDoApp](https://github.com/azure-appservice-samples/ToDoApp.git) v **repoUrl**. Potom klikněte na **Uložit**.
 
    ![](./media/app-service-deploy-complex-application-predictably/deploy-11-parametereditorfilled.png)

    >[AZURE.NOTE] Neobsahovaly text je funkce, které v **Standardní** osy a novějších verzích a upozornění na úrovni plánu jsou funkce, které v **základní** osy nebo vyšší, budete muset nastavit parametr **sku** **Standardní** nebo **Premium** Chcete-li zobrazit všechny vaše nové přehledy aplikace zdroje částečné nahoru.
    
16. Klikněte na **nasazení**. Pokud jste vybrali možnost **Uložit hesla**, heslo uloženo v parametru soubor **ve formátu prostého textu**. Jinak budete požádáni o zadání hesla databáze během procesu nasazení.

Je to! Teď můžete stačí, když přejdete na [Portál Azure](https://portal.azure.com/) a nástroji [Azure zdroje Explorer](https://resources.azure.com) zobrazíte upozornění na nové a nastavení automatické měřítko přidají do vašeho JSON nasadit aplikaci.

Postup v této části hlavně provést následující:

1.  Připravit soubor šablony
2.  Vytvoření souboru parametr zvolit soubor šablony
3.  Nasazení soubor šablony s parametrem soubor

Poslední krok snadno vystavila společnost rutiny Powershellu. Visual Studio akce při nasazení aplikace zobrazíte otevřete Scripts\Deploy AzureResourceGroup.ps1. Je hodně tam kód, ale jenom budu zvýrazněte všechny relevantní kód je třeba nasadit soubor šablony s parametrem soubor.

![](./media/app-service-deploy-complex-application-predictably/deploy-12-powershellsnippet.png)

Poslední rutina `New-AzureResourceGroup`, je skutečně provede akci. Všechno nastavuje prokázat vám, pomocí nástrojů se relativně jednoduchých předvídatelností nasazení aplikace cloudu. Pokaždé, když spustíte rutinu na stejnou šablonu stejného souboru parametr, budete chtít získat stejný výsledek.

## <a name="summary"></a>Souhrn ##

V DevOps jsou klávesy se šipkami úspěšné nasazení aplikace typu maximum měřítko skládající se ze microservices opakovatelnost a předvídatelnost. V tomto kurzu nasadili aplikace dvě microservice Azure jako jeden zdroj skupiny pomocí šablony správce prostředků Azure. Zpravidla se vám Dal znalostí, které potřebujete k spuštění aplikace v Azure převádění šablony můžete vytvořit a nasazení předvídatelností. 

## <a name="next-steps"></a>Další kroky ##

Zjistěte, jak [použít aktivní metodiky nepřetržitě publikování aplikace microservices snadno](app-service-agile-software-development.md) a upřesnění zavedení techniky jako [flighting nasazení](app-service-web-test-in-production-controlled-test-flight.md) snadno.

<a name="resources"></a>
## <a name="more-resources"></a>Další materiály ##

-   [Azure jazyk šablony správce prostředků](../resource-group-authoring-templates.md)
-   [Vytváření Azure správce prostředků šablony](../resource-group-authoring-templates.md)
-   [Funkce šablony Azure správce prostředků](../resource-group-template-functions.md)
-   [Nasazení aplikace šablonou správce prostředků Azure](../resource-group-template-deploy.md)
-   [Správce Azure Powershellu s Azure zdroje](../powershell-azure-resource-manager.md)
-   [Poradce při potížích s nasazení pole Skupina zdroje v Azure](../resource-manager-troubleshoot-deployments-portal.md)




 
