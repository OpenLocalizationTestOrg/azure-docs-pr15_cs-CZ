<properties
    pageTitle="Web App klonováním pomocí prostředí PowerShell"
    description="Zjistěte, jak vytvořit kopii Web Apps na nový Web Apps pomocí Powershellu."
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="01/13/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-powershell"></a>Aplikace služby Azure aplikace klonováním pomocí prostředí PowerShell#

Verze Microsoft Azure PowerShell verze 1.1.0 nová možnost přidaná do nové AzureRMWebApp, který chcete umožnit uživatel klonovat existující Web Appu do nově vytvořený aplikace v jiné oblasti nebo ve stejné oblasti. To vám umožní zákazníkům nasadit celá řada aplikací mezi různými oblastmi snadno a rychle.

Kopírování aplikace je v současné době podporuje jen premium osy aplikace služby plány. Nové funkce využívá stejné omezení jako zálohování webových aplikací najdete v tématu [obecnějším údajům web app v aplikaci služby Azure](web-sites-backup.md).

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Další informace o použití Správce prostředků Azure na základě Azure PowerShell rutiny pro správu Web Apps najdete v [Správce prostředků Azure na základě příkazy Powershellu pro Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="cloning-an-existing-app"></a>Kopírování existující aplikace ##

Scénář: Stávající web aplikaci pro v oblasti Jih ústřední nám uživatele chtějí klonovat obsahu na nový web app v oblasti severní ústřední US. Můžete to provést pomocí Správce prostředků Azure verzi rutiny prostředí PowerShell k vytvoření nové webové aplikace s parametrem – SourceWebApp.

Znalost název skupiny zdroje, který obsahuje zdrojový web appu, můžete používáme následujícího příkazu Powershellu získat informace o zdroji web appu (v tomto případě s názvem zdrojový web appu):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Pokud chcete vytvořit nový plán služeb aplikací, můžete použít příkaz Nový AzureRmAppServicePlan jako v následujícím příkladu

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Pomocí příkazu Nový AzureRmWebApp můžeme vytvořit novou webovou aplikaci v oblasti severní ústřední US a nevážou na existující osy premium plán služeb aplikací, kromě doporučujeme použít stejné skupina zdroje jako zdrojový web app nebo definovat nové skupiny prostředků, následující ukazuje:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Vytvořit kopii existující web appem, včetně všechny přidružené nasazení paticím, si uživatel bude muset použít parametr IncludeSourceWebAppSlots, následujícího příkazu Powershellu ukazuje použití tohoto parametru pomocí příkazu Nový AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots $true

Vytvořit kopii existující web aplikace ve stejné oblasti, uživatel bude potřebovat k vytvoření nové skupiny prostředků a nová aplikace služba plánování ve stejné oblasti a potom pomocí následujícího příkazu Powershellu klonovat web appu

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Kopírování existující aplikace prostředí aplikace služby ##

Scénář: Stávající web aplikaci pro v oblasti Jih centrální nám uživatel chtějí klonovat obsahu na nový web appu do existující aplikace služby prostředí řízení.

Znalost název skupiny zdroje, který obsahuje zdrojový web appu, můžete používáme následujícího příkazu Powershellu získat informace o zdroji web appu (v tomto případě s názvem zdrojový web appu):

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Znalost pomocného mechanismu řízení název a název skupiny zdroje, které patří pomocného mechanismu řízení, uživatel můžete použít příkaz Nový AzureRmWebApp v existující pomocného mechanismu řízení vytvořit novou webovou aplikaci, následující ukazuje:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

Parametr umístění je potřebný kvůli starší důvod, ale v případě vytváření aplikace pomocného mechanismu řízení, bude ignorována. 

## <a name="cloning-an-existing-app-slot"></a>Kopírování existující úsek aplikace ##

Scénář: Uživatel chtěli klonovat existující úsek App Web na položku nové webové aplikace nebo nové pozici v prohlížeči. Nový Web Appu můžou být ve stejné oblasti jako původní úsek v prohlížeči nebo v jiné oblasti.

Znalost zdroje název skupiny, který obsahuje zdrojový web appu, můžete používáme následujícího příkazu Powershellu zobrazíte informace úsek serveru zdroj webové aplikace (v tomto případě s názvem zdroj webappslot) stejným pro zdrojový web appu v prohlížeči:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Následující ukazuje, vytváření klonovat zdrojový web appu pro nové webové aplikace:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Konfigurace správce přenosu při kopírování v aplikaci ##

Vytvoření více oblastí webové aplikace a konfigurace Azure přenosy správce chcete provoz směrovat na tyto webové aplikace, je n důležitá funkce zajistit, že aplikace zákazníků vysoce k dispozici, když klonováním existující web appu máte možnost připojení k nový profil správce přenosy nebo již existujícího obou webových aplikací web apps – Všimněte si, že verze správce přenosy je podporována pouze správce prostředků Azure.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Vytvořit nový profil správce přenosu při kopírování v aplikaci ###

Scénář: Uživatel chtěli klonovat webovou aplikaci do jiné oblasti, při konfiguraci Správce prostředků Azure přenosy správce profilu, zahrnující obou webových aplikací web apps. Následující ukazuje, vytváření klonovat zdrojový web appu na nový web appu při konfiguraci nový profil přenosy správce:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Přidání nových klonovat Web Appu do existujícího profilu přenosy správce ###

Scénář: Už mít uživatel správce prostředků Azure přenosy správce profil, který by chtěl přidat obou webové aplikace jako koncové body. K tomu, potřebujeme nejprve získat existující přenosy profilu nadřízeného, bude potřebujeme id předplatného, název skupiny prostředků a stávající název profilu přenosy správce.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Po tom, co id přenosy správce, takto znázorňuje vytvoření klonovat zdrojový web appu na nový web appu při jejich přidávání nastavili do existujícího profilu přenosy správce:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Aktuální omezení ##

Tato funkce momentálně v náhledu, pracujeme na Přidat nové funkce v čase, v následujícím seznamu jsou známé omezení aktuální verzi aplikace klonováním:

- Nastavení automatického nejsou klonovat
- Nastavení zálohování plánu nejsou klonovat
- Nastavení VNET nejsou klonovat
- Aplikace přehledy nejsou nastavení automaticky na cílový web appu
- Nejsou klonovat snadno Auth nastavení
- Nejsou klonovat kudu rozšíření
- Tip: pravidla nejsou klonovat
- Nejsou klonovat obsah databáze


### <a name="references"></a>Odkazy ###
- [Azure správce prostředků základě příkazy Powershellu pro Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Web App klonováním portálu Azure](app-service-web-app-cloning-portal.md)
- [Obecnějším údajům web app v aplikaci služby Azure](web-sites-backup.md)
- [Azure podpory správce prostředků Azure přenosy správce (verze Preview)](../../articles/traffic-manager/traffic-manager-powershell-arm.md)
- [Úvod do prostředí aplikace služby](app-service-app-service-environment-intro.md)
- [Správce Azure Powershellu s Azure zdroje](../powershell-azure-resource-manager.md)
