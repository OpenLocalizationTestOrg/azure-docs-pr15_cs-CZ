<properties
    pageTitle="Azure na základě správce prostředků různé platformy příkaz nástroje pro Azure webové aplikace | Microsoft Azure"
    description="Naučte se pomocí nových nástrojů založené na Azure správce prostředků příkazového řádku různé platformy ke správě Azure Web Apps."
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

# <a name="using-azure-resource-manager-based-xplat-cli-for-azure-web-app"></a>Použití Azure zdroje na základě správce XPlat rozhraní příkazového řádku pro Azure Web App#

> [AZURE.SELECTOR]
- [Azure rozhraní příkazového řádku](app-service-web-app-azure-resource-manager-xplat-cli.md)
- [Azure Powershellu](app-service-web-app-azure-resource-manager-powershell.md)

Verze Microsoft Azure různé platformy příkazového řádku nástroje verze 0.10.5 byly přidány nové příkazy. Tyto příkazy umožnit uživatele pomocí příkazů na základě správce prostředků Azure Powershellu ke správě webových aplikací Web Apps.

Další informace o správě skupin zdrojů, najdete v článku [použití rozhraní příkazového řádku Azure ke správě Azure materiály a zdroje skupiny](../xplat-cli-azure-resource-manager.md). 


## <a name="managing-app-service-plans"></a>Správa aplikací služby plány jednotného zasílání zpráv ##

### <a name="create-an-app-service-plan"></a>Vytvoření plánu aplikace služby ###
Pokud chcete vytvořit plán služeb aplikací, příkazem **vytvořit azure appserviceplan** .

Popis různých parametrů jsou následující:

-   **– Skupina zdroje**: Skupina zdroje, které zahrnuje tarif nově vytvořený aplikaci služby.
-   **– název**: název plán služeb aplikací.
-   **– umístění**: umístění plán služeb aplikací.
-   **– osy**: požadované cenových sku (Možnosti jsou: F1 (zdarma). D1 (sdílená). B1 (základní malé), B2 (základní střední) a B3 (Basic velké). S1 (standardní malé), S2 (standardní střední) a S3 (standardní velké). P1 (Premium malá), P2 (Premium střední) a P3 (Premium velké).)
-   **– instance**: počet zaměstnanců aplikaci služby plán (výchozí hodnota je 1).

Příklad použití tuto rutinu:

    azure appserviceplan create --name ContosoAppServicePlan --location "South Central US" --resource-group ContosoAzureResourceGroup --sku P1 --instances 10

### <a name="list-existing-app-service-plans"></a>Stávající služba aplikace seznam plány jednotného zasílání zpráv ###

Seznam existující plán služeb aplikace, použijte příkaz **azure appserviceplan seznamu** .

Seznam všech plánů služby aplikace v části určité skupiny zdrojů, můžete:

    azure appserviceplan list --resource-group ContosoAzureResourceGroup

Plán služeb určitou aplikaci, použijte příkaz **azure appserviceplan zobrazení** :

    azure appserviceplan show --name ContosoAppServicePlan --resource-group southeastasia

### <a name="configure-an-existing-app-service-plan"></a>Konfigurace existující aplikace služby plánování ###

Pokud chcete změnit nastavení pro existující plán služeb aplikací, použijte příkaz **azure appserviceplan konfigurace** . Můžete změnit sku a počet zaměstnanců 

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1 --instances 9

#### <a name="scaling-an-app-service-plan"></a>Změna měřítka plán služeb aplikací ####

Změnit velikost existující aplikace služby plánu, můžete:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --instances 9

#### <a name="changing-the-sku-of-an-app-service-plan"></a>Změna SKU plán služeb aplikací ####

Změna sku existující aplikace služby plánu, můžete:

    azure appserviceplan config --nameContosoAppServicePlan --resource-group ContosoAzureResourceGroup --sku S1


### <a name="delete-an-existing-app-service-plan"></a>Odstranění existující aplikace služby plánování ###

Pokud chcete odstranit existující plán služeb aplikací, všechny přiřazené webové aplikace muset přesunutý nebo odstraněný nejdřív. Pak pomocí příkazu **Odstranit azure web appu** je že odstraníte plán služeb aplikací.

    azure appserviceplan delete --name ContosoAppServicePlan --resource-group southeastasia

## <a name="managing-app-service-web-apps"></a>Správa aplikací služby Web Apps ##

### <a name="create-a-web-app"></a>Vytvoření webové aplikace ###

Pokud chcete vytvořit web appu, příkazem **vytvořit azure web appu** .

Popis různých parametrů jsou následující:

- **– název**: název pro web app.
- **– plán**: název plán služeb sloužící k hostování web appu.
- **– Skupina zdroje**: pole Skupina zdroje, který je hostitelem plán služeb aplikací.
- **– umístění**: webové umístění aplikace.

Příklad použití tuto rutinu:

    azure webapp create --name ContosoWebApp --resource-group ContosoAzureResourceGroup --plan ContosoAppServicePlan --location "South Central US"

### <a name="delete-an-existing-web-app"></a>Odstranění existující Web Appu ###

Odstranit existující web appu můžete použít příkaz **Odstranit azure webové aplikace** , budete muset zadat název web appu a název skupiny prostředků.

    azure webapp delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="list-existing-web-apps"></a>Seznam existující Web Apps ###

Seznam existující web aplikace, použijte příkaz **seznamu azure web appu** .

Seznam všech aplikací pro web podle určité skupiny zdrojů, můžete:

    azure webapp list --resource-group ContosoAzureResourceGroup

Pokud chcete dostat do konkrétních webových aplikací, použijte příkaz **azure web appu zobrazit** .

    azure webapp show --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="configure-an-existing-web-app"></a>Konfigurace existující Web Appu ###

Pokud chcete změnit nastavení a konfigurace pro existující web app, příkazem **Nastavení konfigurace azure web appu** .

Příklad (1): Změna php verzi web appu 

    azure webapp config set --name ContosoWebApp --resource-group ContosoAzureResourceGroup --phpversion 5.6

Příklad (2): Přidání nebo změna nastavení aplikace

    webapp config appsettings set --name ContosoWebApp --resource-group ContosoAzureResourceGroup appsetting1=appsetting1value,appsetting2=appsetting2value

Pokud chcete vědět, co může být další úpravy nastavení změnit, použijte příkaz **azure web appu konfigurace set -h** .

### <a name="change-the-state-of-an-existing-web-app"></a>Změna stavu existující Web Appu ###

#### <a name="restart-a-web-app"></a>Restartujte do webových aplikací ####

Restartujte do webových aplikací, je nutné zadat název a zdroje okruhem web appu.

    azure webapp restart --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="stop-a-web-app"></a>Ukončení do webových aplikací ####

Ukončit do webových aplikací, je nutné zadat název a zdroje okruhem web appu.

    azure webapp stop --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="start-a-web-app"></a>Zahájení do webových aplikací ####

Pokud chcete začít do webových aplikací, je nutné zadat skupině název a zdroje z web appu.

    azure webapp start --name ContosoWebApp --resource-group ContosoAzureResourceGroup

### <a name="manage-web-app-publishing-profiles"></a>Správa publikování na webu aplikace profilů ###

Každý web appu má publikování profil, který slouží k publikování aplikací.

#### <a name="get-publishing-profile"></a>Získání publikování profilu ####

Profil publikování pro web app, použijte:

    azure webapp publishingprofile --name ContosoWebApp --resource-group ContosoAzureResourceGroup

Tento příkaz vrátí publikování profilu uživatelské jméno a heslo pro příkazového řádku.

### <a name="manage-web-app-hostnames"></a>Spravovat názvy hostitelů Web Appu ###

Ke správě hostname vazby pro webovou aplikaci, použijte příkaz **názvy hostitelů konfigurace azure web appu**  

#### <a name="list-hostname-bindings"></a>Seznam hostname vazby ####

Získání aktuální vazby hostname pro web app, můžete:

    azure webapp config hostnames list --name ContosoWebApp --resource-group ContosoAzureResourceGroup

#### <a name="add-hostname-bindings"></a>Přidat název hostitele vazby ####

Přidat název hostitele vazby k web appu, můžete:

    azure webapp config hostnames add --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

#### <a name="delete-hostname-bindings"></a>Odstranění hostname vazby ####

Hostname vazby, můžete odstranit:

    azure webapp config hostnames delete --name ContosoWebApp --resource-group ContosoAzureResourceGroup --hostname www.contoso.com

### <a name="next-steps"></a>Další kroky ###
- Informace o podpoře Azure správce prostředků rozhraní příkazového řádku najdete v tématu [Správa Azure materiály a zdroje skupiny pomocí rozhraní příkazového řádku Azure.](../xplat-cli-azure-resource-manager.md)
- Další informace o správě aplikace služby pomocí prostředí PowerShell najdete v tématu [Using Azure Resource Manager-Based Powershellu Správa Azure webových aplikací Web Apps.](app-service-web-app-azure-resource-manager-powershell.md)
