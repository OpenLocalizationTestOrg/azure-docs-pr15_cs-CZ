<properties
    pageTitle="Azure na základě správce prostředků Powershellu příkazy pro Azure Web App | Microsoft Azure"
    description="Zjistěte, jak použít nové příkazy založené na Azure správce prostředků Powershellu ke správě Azure Web Apps."
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
    ms.date="09/29/2016"
    ms.author="aelnably"/>

# <a name="using-azure-resource-manager-based-powershell-to-manage-azure-web-apps"></a>Azure zdroje na základě správce Správa přes Azure Web Apps#

> [AZURE.SELECTOR]
- [Azure rozhraní příkazového řádku](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure Powershellu](app-service-web-app-azure-resource-manager-powershell.md)

S Microsoft Azure PowerShell verze 1.0.0 byly přidány nové příkazy, které umožnit uživatele pomocí příkazů na základě správce prostředků Azure Powershellu ke správě webových aplikací Web Apps.

Další informace o správě skupin zdrojů, najdete v článku [použití Azure pomocí Správce prostředků Azure](../powershell-azure-resource-manager.md). 

Další informace o úplný seznam parametrů a možnosti rutinách Powershellu najdete v tématu [Úplné Reference pro rutiny od založených na Web App Azure zdroje správce rutiny prostředí PowerShell](https://msdn.microsoft.com/library/mt619237.aspx)

## <a name="managing-app-service-plans"></a>Správa aplikací služby plány jednotného zasílání zpráv ##

### <a name="create-an-app-service-plan"></a>Vytvoření plánu aplikace služby ###
Pokud chcete vytvořit plán služeb aplikací, použijte rutinu **New-AzureRmAppServicePlan** .

Popis různých parametrů jsou následující:

-   **Název**: název plán služeb aplikací.
-   **Umístění**: služby plán umístění.
-   **ResourceGroupName**: Skupina zdroje, které zahrnuje tarif nově vytvořený aplikaci služby.
-   **Vrstvy**: požadované ceny osy (výchozí zdarma, je další karty jsou sdílené, Basic, Standard a Premium)
-   **WorkerSize**: velikost pracovníků (výchozí hodnota je menší, pokud parametr osy byla určena jako Basic, standardní nebo Premium. Další karty jsou střední a velikost velké.)
-   **NumberofWorkers**: počet zaměstnanců aplikaci služby plán (výchozí hodnota je 1). 

Příklad použití tuto rutinu:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -Tier Premium -WorkerSize Large -NumberofWorkers 10

### <a name="create-an-app-service-plan-in-an-app-service-environment"></a>Vytvoření plánu služba aplikací v prostředí aplikace služby ###
Pokud chcete vytvořit plán služeb aplikací v prostředí aplikace služby, příkazem stejný příkaz **Nový AzureRmAppServicePlan** se navíc parametry zadejte název pomocného mechanismu řízení a pomocného mechanismu řízení na název skupiny prostředků.

Příklad použití tuto rutinu:

    New-AzureRmAppServicePlan -Name ContosoAppServicePlan -Location "South Central US" -ResourceGroupName ContosoAzureResourceGroup -AseName constosoASE -AseResourceGroupName contosoASERG -Tier Premium -WorkerSize Large -NumberofWorkers 10

Další informace o aplikaci služby prostředí, zkontrolujte [Úvod do prostředí aplikace služby](app-service-app-service-environment-intro.md)

### <a name="list-existing-app-service-plans"></a>Stávající služba aplikace seznam plány jednotného zasílání zpráv ###

Stávající služba plány aplikace, použijte rutinu **Get-AzureRmAppServicePlan** .

Seznam všech plánů služby aplikace v rámci předplatného, můžete: 

    Get-AzureRmAppServicePlan

Seznam všech plánů služby aplikace v části určité skupiny zdrojů, můžete:

    Get-AzureRmAppServicePlan -ResourceGroupname ContosoAzureResourceGroup

Plán služeb určitou aplikaci, použijte:

    Get-AzureRmAppServicePlan -Name ContosoAppServicePlan


### <a name="configure-an-existing-app-service-plan"></a>Konfigurace existující aplikace služby plánování ###

Pokud chcete změnit nastavení pro existující plán služeb aplikací, použijte rutinu **Set-AzureRmAppServicePlan** . Můžete změnit úrovně, pracovní velikost a počet zaměstnanců 

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard -WorkerSize Medium -NumberofWorkers 9

#### <a name="scaling-an-app-service-plan"></a>Změna měřítka plán služeb aplikací ####

Změnit velikost existující aplikace služby plánu, můžete:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -NumberofWorkers 9

#### <a name="changing-the-worker-size-of-an-app-service-plan"></a>Změna velikosti pracovníka služby aplikace plánu ####

Změna velikosti pracovníků existující aplikace služby plánu, můžete:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -WorkerSize Medium

#### <a name="changing-the-tier-of-an-app-service-plan"></a>Změna osy plán služeb aplikací ####

Pokud chcete změnit úroveň existující aplikace služby plánu, použijte:

    Set-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Tier Standard

### <a name="delete-an-existing-app-service-plan"></a>Odstranění existující aplikace služby plánování ###

Pokud chcete odstranit existující plán služeb aplikací, všechny přiřazené webové aplikace muset přesunutý nebo odstraněný nejdřív. Potom použít rutinu **Odebrat AzureRmAppServicePlan** odstraníte plán služeb aplikací.

    Remove-AzureRmAppServicePlan -Name ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup

## <a name="managing-app-service-web-apps"></a>Správa aplikací služby Web Apps ##

### <a name="create-a-web-app"></a>Vytvoření webové aplikace ###

Pokud chcete vytvořit web appu, získáte pomocí rutiny **AzureRmWebApp nový** .

Popis různých parametrů jsou následující:

- **Název**: název pro web app.
- **AppServicePlan**: název plán služeb sloužící k hostování web appu.
- **ResourceGroupName**: pole Skupina zdroje, který je hostitelem plán služeb aplikací.
- **Umístění**: webové umístění aplikace.

Příklad použití tuto rutinu:

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"

### <a name="create-a-web-app-in-an-app-service-environment"></a>Vytvoření do webových aplikací v prostředí aplikace služby ###

Při vytváření webové aplikace v aplikaci služby prostředí řízení. Příkazem stejný **Nový AzureRmWebApp** se navíc parametry zadejte název pomocného mechanismu řízení a název skupiny zdroje, které patří pomocného mechanismu řízení.

    New-AzureRmWebApp -Name ContosoWebApp -AppServicePlan ContosoAppServicePlan -ResourceGroupName ContosoAzureResourceGroup -Location "South Central US"  -ASEName ContosoASEName -ASEResourceGroupName ContosoASEResourceGroupName

Další informace o aplikaci služby prostředí, zkontrolujte [Úvod do prostředí aplikace služby](app-service-app-service-environment-intro.md)

### <a name="delete-an-existing-web-app"></a>Odstranění existující Web Appu ###

Odstranit existující web appu můžete použít rutinu **Odebrat AzureRmWebApp** , budete muset zadat název web appu a název skupiny prostředků.

    Remove-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup


### <a name="list-existing-web-apps"></a>Seznam existující Web Apps ###

Seznam existující web aplikace, získáte pomocí rutiny **Get-AzureRmWebApp** .

Seznam všech webových aplikací v rámci předplatného, můžete:

    Get-AzureRmWebApp

Seznam všech aplikací pro web podle určité skupiny zdrojů, můžete:

    Get-AzureRmWebApp -ResourceGroupname ContosoAzureResourceGroup

Do konkrétních webových aplikací, použijte:

    Get-AzureRmWebApp -Name ContosoWebApp

### <a name="configure-an-existing-web-app"></a>Konfigurace existující Web Appu ###

Pokud chcete změnit nastavení a konfigurace pro existující web app, použijte rutinu **Set-AzureRmWebApp** . Úplný seznam parametrů zkontrolujte [rutina referenčního odkazu](https://msdn.microsoft.com/library/mt652487.aspx)

Příklad (1): umožňuje změnit připojovací řetězec tuto rutinu

    $connectionstrings = @{ ContosoConn1 = @{ Type = “MySql”; Value = “MySqlConn”}; ContosoConn2 = @{ Type = “SQLAzure”; Value = “SQLAzureConn”} }
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -ConnectionStrings $connectionstrings

Příklad (2): Přidání nebo změna nastavení aplikace

    $appsettings = @{appsetting1 = "appsetting1value"; appsetting2 = "appsetting2value"}
    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -AppSettings $appsettings


Příklad (3): nastavte web appu ke spouštění v režimu 64bitová verze

    Set-AzureRmWebApp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -Use32BitWorkerProcess $False

### <a name="change-the-state-of-an-existing-web-app"></a>Změna stavu existující Web Appu ###

#### <a name="restart-a-web-app"></a>Restartujte do webových aplikací ####

Restartujte do webových aplikací, je nutné zadat název a zdroje okruhem web appu.

    Restart-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Ukončení do webových aplikací ####

Ukončit do webových aplikací, je nutné zadat název a zdroje okruhem web appu.

    Stop-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Zahájení do webových aplikací ####

Pokud chcete začít do webových aplikací, je nutné zadat skupině název a zdroje z web appu.

    Start-AzureRmWebapp -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Správa publikování na webu aplikace profilů ###

Každý web appu má publikování profil, který slouží k publikování aplikací, několika operací můžete provádět na publikování profily.

#### <a name="get-publishing-profile"></a>Získání publikování profilu ####

Profil publikování pro web app, použijte:

    Get-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup -OutputFile .\publishingprofile.txt

Tento příkaz vrátí publikování profilu příkazového řádku jako dobře výstupní publikování profil do textového souboru.

#### <a name="reset-publishing-profile"></a>Obnovení publikování profilu ####

Jak resetovat publikování hesla FTP a webové nasazení pro web app, použití:

    Reset-AzureRmWebAppPublishingProfile -Name ContosoWebApp -ResourceGroupName ContosoAzureResourceGroup

### <a name="manage-web-app-certificates"></a>Správa certifikátů Web App ###

Informace o tom, jak spravovat certifikáty web app najdete v tématu [certifikáty SSL vazba pomocí prostředí PowerShell](app-service-web-app-powershell-ssl-binding.md)


### <a name="next-steps"></a>Další kroky ###
- Informace o podpoře Azure správce prostředků Powershellu najdete v tématu [pomocí prostředí PowerShell Azure pomocí Správce prostředků Azure.](../powershell-azure-resource-manager.md)
- Další informace o aplikaci služby prostředí, najdete v článku [Úvod do prostředí aplikace služeb.](app-service-app-service-environment-intro.md)
- Další informace o správě certifikáty SSL služby aplikace pomocí Powershellu najdete v tématu [certifikáty SSL vazba pomocí prostředí PowerShell.](app-service-web-app-powershell-ssl-binding.md)
- Další informace o úplný seznam rutiny prostředí PowerShell založené na Azure správce prostředků pro Azure Web Apps najdete v tématu [Reference pro rutiny Azure rutin webové aplikace Azure správce prostředků Powershellu.](https://msdn.microsoft.com/library/mt619237.aspx)
- - Další informace o správě aplikace služby pomocí rozhraní příkazového řádku najdete v tématu [Using Azure Resource Manager-Based XPlat rozhraní příkazového řádku pro Azure Web App.](app-service-web-app-azure-resource-manager-xplat-cli.md)
