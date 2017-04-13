<properties
   pageTitle="Azure projekty Visual Studio skupina zdroje | Microsoft Azure"
   description="Vytvoření projektu skupiny Azure zdroje a nasazení zdrojů do Azure pomocí aplikace Visual Studio."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="azure-resource-manager"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/20/2016"
   ms.author="tomfitz" />

# <a name="creating-and-deploying-azure-resource-groups-through-visual-studio"></a>Vytvoření a nasazení Azure zdroje skupiny pomocí aplikace Visual Studio

Visual Studiu a v [Azure SDK](https://azure.microsoft.com/downloads/)můžete vytvořit projekt, který nasadí infrastrukturu a kód na Azure. Můžete například definovat webu hostitele webu a databáze aplikace a nasazení infrastruktury a kód. Nebo můžete definovat virtuálního počítače, virtuální sítě a úložiště účtu a nasazení infrastruktury spolu s skript, který se spustí na virtuálního počítače. **Pole Skupina zdroje Azure** nasazení projektu umožňuje nasadit všechny potřebné zdroje v operaci jediné, opakující. Další informace o nasazení a správu prostředků najdete v tématu [Přehled Správce zdrojů Azure](azure-resource-manager/resource-group-overview.md).

Pole Skupina zdroje Azure projekty obsahují Azure správce prostředků JSON šablon, které definují prostředky, které můžete nasadit do Azure. Další informace o prvcích šabloně správce prostředků najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md). Visual Studio umožňuje upravovat tyto šablony a poskytuje nástroje, které usnadňují práci se šablonami.

V tomto tématu nasadíte web app a databáze SQL. Kroky jsou ale skoro stejně libovolný typ zdroje. Jako snadno nasazením virtuální počítač a související materiály. Visual Studio poskytuje mnoho různých starter šablon pro nasazení obvyklé scénáře.

Tento článek obsahuje Visual Studio 2015 aktualizace 2 a Microsoft Azure SDK pro .NET 2.9. Pokud používáte Visual Studio 2013 s Azure SDK 2.9, prostředí je převážně stejné. Používání verzí SDK Azure z 2.6 nebo později; prostředí a možností uživatelského rozhraní, se liší od uživatelského rozhraní uvedené v tomto článku. Důrazně doporučujeme nainstalovat nejnovější verzi [Azure SDK](https://azure.microsoft.com/downloads/) před spuštěním kroky. 

## <a name="create-azure-resource-group-project"></a>Vytvoření pole Skupina zdroje Azure projektu

V tomto postupu vytvořit projekt Azure pole Skupina zdroje se šablonou **v prohlížeči + SQL** .

1. Ve Visual Studiu, zvolte **soubor**, **Nový projekt**, zvolte **C#** nebo **Visual Basic**. Zvolte **cloudu**a klikněte na **Pole Skupina zdroje Azure** projektu.

    ![Shluk nasazení projektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-project.png)

1. Vyberte šablonu, kterou chcete nasadit správci zdrojů Azure. Všimněte si tam, mnoha různých možností založené na typ projektu, který chcete nasadit. Toto téma zvolte šablonu **v prohlížeči + SQL** .

    ![Vyberte si šablonu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-project.png)

    Šablony, které jste vybrali je právě výchozí bod; můžete přidat a odebrat materiály ke splnění nefunguje.

    >[AZURE.NOTE] Visual Studio načte seznam dostupných šablon online. V seznamu se lišit.

    Visual Studio vytvoří projektu nasazení skupina zdroje pro web app a databáze SQL.

1. Pokud chcete zobrazit, co jste vytvořili, rozbalte uzlů v projektu nasazení.

    ![Zobrazit uzlů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-items.png)

    Protože zvolili jsme v prohlížeči + šablony SQL v tomto příkladu, zobrazí se následující soubory: 

  	|Název souboru|Popis|
  	|---|---|
  	|Nasazení AzureResourceGroup.ps1|Skript Powershellu, který spustí příkazy Powershellu pro nasazení správci zdrojů Azure.<br />**Poznámka:** Visual Studio používá tento skript Powershellu pro nasazení šablony. Všechny změny provedené u tohoto skriptu vliv na nasazení ve Visual Studiu, proto buďte opatrní.|
  	|WebSiteSQLDatabase.json|Správce prostředků šablonu, která definuje infrastruktury chcete nasadit Azure a parametrů, které může poskytnout během nasazení. Definuje také závislosti mezi prostředky, správce prostředků nasadí zdrojů ve správném pořadí.|
  	|WebSiteSQLDatabase.parameters.json|Parametry soubor, který obsahuje hodnoty potřebné šablonou. Předat hodnoty parametrů můžete přizpůsobit jednotlivé nasazení.|

    Všechny zdroje skupiny nasazení projekty obsahují tyto základní soubory. Jiné projekty může obsahovat další soubory k podpoře dalších funkcí.

## <a name="customize-the-resource-manager-template"></a>Přizpůsobení šablony správce prostředků

Nasazení projektu můžete upravit změnou JSON šablony, které popisují prostředky, které chcete nasadit. JSON zkratka JavaScript Object Notation a je sériové formátu, který se dá snadno pracovat s. Soubory JSON pomocí schéma, které odkazují na začátku každého souboru. Pokud chcete porozumět tomu, že schéma, můžete stáhnout a analyzovat. Schéma definuje, jakou jsou platné, typy a formáty polí, možnými hodnotami výrazu výčtu hodnoty a tak dál. Další informace o prvcích šabloně správce prostředků najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).

Pokud chcete pracovat na vaší šabloně, otevřete **WebSiteSQLDatabase.json**.

Editoru Visual Studio obsahuje nástroje, které vám pomohou s úpravy šablony správce prostředků. Okno **Osnovy JSON** snadno abyste viděli prvky podle šablony.

![zobrazení osnovy JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-json-outline.png)

Prvky v přehledu uvést přenese vás do části šablony a zvýraznění provedených odpovídající JSON.

![přejděte JSON](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/navigate-json.png)

Zdroje můžete přidat tak, že vyberete na tlačítko **Přidat zdroje** v horní části okna JSON osnovy nebo pravým tlačítkem myši na **zdroje** a výběrem **Přidání nového prostředku**.

![Přidání zdrojů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource.png)

Pro účely tohoto návodu vyberte **Účet úložiště** a pojmenujte ho. Zadejte název, který je nesmí přesáhnout 11 znaků a obsahuje pouze čísla a malými písmeny.

![Přidání úložiště](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-storage.png)

Všimněte si, že nejen byl zdroj přidat, ale taky parametru typ účtu úložiště a proměnná název účtu úložiště.

![zobrazení osnovy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-new-items.png)

Parametr **storageType** je předdefinovaných s povolené typy a výchozí typ. Můžete nechat tyto hodnoty nebo upravovat pro nefunguje. Pokud nechcete, aby všichni nasazení účtu úložiště **Premium_LRS** pomocí této šablony, odeberte ho z povolené typy. 

    "storageType": {
      "type": "string",
      "defaultValue": "Standard_LRS",
      "allowedValues": [
        "Standard_LRS",
        "Standard_ZRS",
        "Standard_GRS",
        "Standard_RAGRS"
      ]
    }

Visual Studio také poskytuje technologie intellisense vám pomůže pochopit, jaké vlastnosti jsou k dispozici při úpravách šablony. Například upravit vlastnosti pro váš plán služeb aplikací, přejděte na **HostingPlan** zdroj a přidejte hodnotu **Vlastnosti**. Všimněte si, že technologie intellisense jsou uvedeny dostupné hodnoty a obsahuje popis daná hodnota.

![Zobrazit technologie intellisense](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-intellisense.png)

**NumberOfWorkers** můžete nastavit na hodnotu 1.

    "properties": {
      "name": "[parameters('hostingPlanName')]",
      "numberOfWorkers": 1
    }

## <a name="deploy-the-resource-group-project-to-azure"></a>Nasazení projektu pole Skupina zdroje Azure

Teď jste připraveni nasazení projektu. Při nasazení projekt Azure pole Skupina zdroje, můžete ho nasadit do skupiny Azure zdroje. Skupina zdroje je logické seskupení zdrojů, která sdílejí společná životního cyklu.

1. V místní nabídce na uzel projektu nasazení, zvolte **instalovat** > **Nové nasazení**.

    ![Nasazení, položka nabídky nová nasazení](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/deploy.png)

    Zobrazí se dialogové okno **nasadit do pole Skupina zdroje** .

    ![Nasazení do dialogového pole Skupina zdroje](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployment.png)

1. Do pole rozevíracího seznamu **Skupina zdroje** zvolte existující skupiny zdrojů nebo vytvořte nový účet. Pokud chcete vytvořit skupinu zdrojů, otevřete rozevírací seznam pole **Skupina zdroje** a zvolte **Vytvořit nový**.

    ![Nasazení do dialogového pole Skupina zdroje](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-new-group.png)

    Zobrazí se dialogové okno **Vytvořit pole Skupina zdroje** . Zadejte svoji skupinu název a umístění a klikněte na tlačítko **vytvořit** .

    ![Pole Skupina zdroje dialogové okno vytvořit](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/create-resource-group.png)
   
1. Parametry pro nasazení upravte tak, že vyberete tlačítko **Upravit parametry** .

    ![Obrázek tlačítka parametry](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/edit-parameters.png)

1. Zadejte hodnoty parametrů prázdné a klikněte na tlačítko **Uložit** . Prázdný parametry jsou **hostingPlanName**, **administratorLogin**, **administratorLoginPassword**a **název databáze**.

    **hostingPlanName** Určuje název [plán služeb aplikací](./app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) vytvořit. 
    
    **administratorLogin** Určuje uživatelské jméno pro správce serveru SQL Server. Nepoužívejte běžné Správce názvů jako **správce systému** nebo **Správce**. 
    
    **AdministratorLoginPassword** určuje hesla pro správce serveru SQL Server. Možnost **uložení hesel jako prostý text v souboru parametrů** není zabezpečené; proto není tato možnost. Protože heslo neuloží jako prostý text, je třeba zadejte heslo znovu během nasazení. 
    
    **název databáze** Určuje název databáze, kterou chcete vytvořit. 

    ![Úprava dialogového okna parametry](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/provide-parameters.png)
    
1. Klikněte na tlačítko **instalovat** a nasazení projektu Azure. Prostředí PowerShell konzoly otevře mimo instanci aplikace Visual Studio. V konzole prostředí PowerShell po zobrazení výzvy zadejte heslo správce serveru SQL Server. **Prostředí PowerShell konzoly je možné skrýt před ostatními položkami nebo minimalizované na hlavním panelu.** Vyhledejte tento konzoly a vyberte ho k zadání hesla.

    >[AZURE.NOTE] Visual Studio vás může požádat o instalaci rutiny prostředí PowerShell Azure. Potřebujete rutiny prostředí PowerShell Azure k úspěšnému zavedení skupiny zdrojů. Pokud se zobrazí výzva, nainstalujte je.
    
1. Nasazení může trvat několik minut. Ve windows **výstup** se zobrazí stav nasazení. Po dokončení nasazení poslední zprávu označuje úspěšné zavedení se něco podobného:

        ... 
        18:00:58 - Successfully deployed template 'c:\users\user\documents\visual studio 2015\projects\azureresourcegroup1\azureresourcegroup1\templates\websitesqldatabase.json' to resource group 'DemoSiteGroup'.


1. V prohlížeči otevřete [Azure portál](https://portal.azure.com/) a přihlaste se ke svému účtu. Aby se zobrazila skupina zdroje, vyberte **skupiny zdrojů** a skupina zdroje, které jste nasadili.

    ![Vyberte skupinu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-group.png)

1. Zobrazí všechny nasazeném zdroje. Všimněte si, že název účtu úložiště přesně co jste zadali při přidání daný zdroj. Účet úložiště musí být jedinečný. Šablona automaticky přidá řetězec znaků pro název, který jste zadali zadejte jedinečný název. 

    ![zobrazení zdrojů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-resources.png)

1. Pokud provedete změny a chcete přeinstalujte projektu, vyberte existující skupiny zdrojů z místní nabídky Azure zdroje skupinovém projektu. V místní nabídce vyberte **nasazení**a klikněte na zdroje skupinu, kterou jste nainstalovali.

    ![Pole Skupina zdroje Azure nasazení](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/redeploy.png)

## <a name="deploy-code-with-your-infrastructure"></a>Nasazení kódu s infrastrukturu

V tomto okamžiku infrastruktury nasadili aplikace, ale není žádný skutečné kód používaný k projektu. Toto téma ukazuje, jak nasadit web app a tabulky databáze SQL během nasazení. Pokud nasazujete Virtual Machine místo do webových aplikací, který chcete spustit kód v počítači jako součást nasazení. Proces nasazení kódu pro web app nebo k nastavení virtuálního počítače se skoro stejně.

1. Přidáte projekt na řešení aplikace Visual Studio. Klikněte pravým tlačítkem myši na řešení a vyberte **Přidat** > **Nový projekt**.

    ![Přidání projektu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-project.png)

1. Přidání **webové aplikace**. 

    ![Přidejte web appu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-app.png)
    
1. Vyberte **MVC** a zrušte zaškrtnutí políčka hostitele **v cloudu** , protože projekt skupiny zdrojů provádí daného úkolu.

    ![Vyberte MVC](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/select-mvc.png)
    
1. Jakmile aplikace Visual Studio vytvoří webovou aplikaci, uvidíte oba projekty v řešení.

    ![zobrazení projektů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-projects.png)

1. Teď budete muset zkontrolujte, zda že projekt skupina zdroje známa nový projekt. Přejděte zpět do projektu skupina zdroje (AzureResourceGroup1). Klikněte pravým tlačítkem na **odkazy** a zvolte **Přidat odkaz**.

    ![Přidání odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-new-reference.png)

1. Vyberte projekt webové aplikace, kterou jste vytvořili.

    ![Přidání odkazu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-reference.png)
    
    Přidáním odkaz na odkaz webové aplikace project na skupinovém projektu zdroje a automaticky nastavit tři klíčové vlastnosti. Zobrazí tyto vlastnosti v okně **Vlastnosti** pro odkaz.

      ![Přečtěte si článek Principy](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/see-reference.png)
    
    Vlastnosti jsou:

    - **Další vlastnosti** obsahuje balíček pro nasazení webové pracovní umístění, které se posune k základnímu úložišti Azure. Poznámka: složku (ExampleApp) a soubor (package.zip). Při nasazení aplikace, poskytne tyto hodnoty jako parametry. 
    - **Zahrnout cesta k souboru** obsahuje cestu, kde je vytvářeny balíček. **Zahrnout cílů** obsahuje příkaz, který se spustí nasazení. 
    - Výchozí hodnoty **Sestavit; Balíček** umožňuje nasazení sestavovat a vytvořit balíček pro nasazení web (package.zip).  
    
    Při nasazení získává potřebných informací z vlastnosti, které chcete vytvořit balíček nepotřebujete profilu publikování.
      
1. Přidání zdroje do šablony.

    ![Přidání zdrojů](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-resource-2.png)

1. Tentokrát vyberte **Nasazení webu pro Web Apps**. 

    ![Přidání webových nasazení](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/add-web-deploy.png)
    
1. Přeinstalujte projektu skupiny zdrojů do skupiny zdrojů. Tentokrát existují některé nové parametry. Není potřeba zadat hodnoty pro **_artifactsLocation** nebo **_artifactsLocationSasToken** , protože Visual Studio automaticky vygeneruje jsou tyto hodnoty. Ale musíte nastavit složky a název souboru cestu, která obsahuje balíček pro nasazení (vidíte na následujícím obrázku jako **ExampleAppPackageFolder** a **ExampleAppPackageFileName** ). Obsahují hodnoty, které jste viděli dříve ve vlastnostech odkaz (**ExampleApp** a **package.zip**).

    ![Přidání webových nasazení](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/set-new-parameters.png)
    
    **Artefakt úložiště účet**vyberte položku nasazených pomocí této skupiny zdrojů.
    
1. Po dokončení nasazení, vyberte webovou aplikaci na portálu. Vyberte adresu URL umožňuje přecházet na web.

    ![procházení webu](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/browse-site.png)

1. Všimněte si, že jste úspěšně nasadili aplikaci ASP.NET výchozí.

    ![Zobrazit nasazení aplikace](./media/vs-azure-tools-resource-groups-deployment-projects-create-deploy/show-deployed-app.png)

## <a name="next-steps"></a>Další kroky

- Další informace o správě zdrojů prostřednictvím portálu, najdete v článku [Azure portálu pro správu Azure prostředků](./azure-portal/resource-group-portal.md).
- Další informace o šablonách najdete v tématu [vytváření správce prostředků Azure šablony](resource-group-authoring-templates.md).
