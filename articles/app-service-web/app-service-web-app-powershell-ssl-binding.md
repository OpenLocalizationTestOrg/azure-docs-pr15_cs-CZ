<properties
    pageTitle="Vazba certifikáty SSL pomocí prostředí PowerShell"
    description="Naučte se vytvořit vazbu certifikáty SSL do webové aplikace pomocí Powershellu."
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

# <a name="azure-app-service-ssl-certificate-binding-using-powershell"></a>Azure aplikace služby SSL certifikát vazba pomocí prostředí PowerShell #

Verze Microsoft Azure PowerShell verze 1.1.0 nové rutiny bylo přidáno, který by umožnit uživatele navázat existující nebo nové certifikáty SSL na stávající Web Appu.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

Další informace o použití Správce prostředků Azure na základě Azure PowerShell rutiny pro správu Web Apps najdete v [Správce prostředků Azure na základě příkazy Powershellu pro Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)

## <a name="uploading-and-binding-a-new-ssl-certificate"></a>Nahrávání a vytvoření vazby nový certifikát SSL ##

Scénář: Uživatel chcete navázat certifikátu SSL do jedné ze svých aplikací web apps.

Znalost zdroje název skupiny, který obsahuje webovou aplikaci, na název webové aplikace, cesta k souboru .pfx certifikát v počítači uživatele, heslo pro potvrzení a vlastní název hostitele, můžete používáme následujícího příkazu Powershellu této SSL vazby:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -CertificateFilePath PathToPfxFile -CertificatePassword PlainTextPwd -Name www.contoso.com

Nezapomeňte, že před přidáním vazbu SSL do webových aplikací, musí mít už nakonfigurovali název hostitele (vlastní domény). Pokud není nakonfigurován název hostitele a pak bude dojde k chybě při spuštění nové AzureRmWebAppSSLBinding neexistuje "hostname". Název hostitele můžete přidat přímo z portálu nebo pomocí Powershellu Azure. Následující úryvek prostředí PowerShell lze nakonfigurovat před spuštěním AzureRmWebAppSSLBinding nový název hostitele.   
  
    $webApp = Get-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup  
    $hostNames = $webApp.HostNames  
    $HostNames.Add("www.contoso.com")  
    Set-AzureRmWebApp -Name mytestapp -ResourceGroupName myresourcegroup -HostNames $HostNames   
  
Je důležité pochopit rutinu Set-AzureRmWebApp přepíše názvy hostitelů pro web app. Proto výše uvedený fragment kódu prostředí PowerShell připojení k existující seznam jmen hostitele pro web app.  

## <a name="uploading-and-binding-an-existing-ssl-certificate"></a>Nahrávání a vytvoření vazby stávající certifikát SSL ##

Scénář: Uživatel chcete navázat dříve nahraný certifikát SSL na jeden z jeho webových aplikací.

Bude seznam certifikátů už nahráli určité skupiny zdrojů pomocí následujícího příkazu

    Get-AzureRmWebAppCertificate -ResourceGroupName myresourcegroup

Všimněte si, že certifikáty jsou místní pro konkrétní umístění a pole Skupina zdroje muset znovu odeslat certifikát, pokud je nakonfigurované webové aplikace do jiného umístění a zdroje skupině další uživatele, který potřebné certifikátu 

Znalost zdroje název skupiny, který obsahuje webovou aplikaci, na název webové aplikace, miniaturu certifikátu a vlastní název hostitele, můžete používáme následujícího příkazu Powershellu této SSL vazby:

    New-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Thumbprint <certificate thumbprint> -Name www.contoso.com

## <a name="deleting-an-existing-ssl-binding"></a>Odstranění existující vazby SSL  ##

Scénář: Uživatel chcete odstranit existující vazbu SSL.

Znalost zdroje název skupiny, který obsahuje webovou aplikaci, na název webové aplikace a vlastní název hostitele, můžete používáme následujícího příkazu Powershellu odebrat tohoto SSL vazby:

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com

Poznámka: Pokud odebrané vazba SSL byla poslední vazbu s použitím tento certifikát v tomto umístění, ve výchozím nastavení certifikát se odstraní, pokud chcete zachovat certifikát, že aby certifikát může použít možnost DeleteCertificate uživatele

    Remove-AzureRmWebAppSSLBinding -ResourceGroupName myresourcegroup -WebAppName mytestapp -Name www.contoso.com -DeleteCertificate $false

### <a name="references"></a>Odkazy ###
- [Azure správce prostředků základě příkazy Powershellu pro Azure Web App](app-service-web-app-azure-resource-manager-powershell.md)
- [Úvod do prostředí aplikace služby](app-service-app-service-environment-intro.md)
- [Správce Azure Powershellu s Azure zdroje](../powershell-azure-resource-manager.md)
