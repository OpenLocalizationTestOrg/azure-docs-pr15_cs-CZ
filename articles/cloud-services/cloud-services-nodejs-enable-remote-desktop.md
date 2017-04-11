<properties 
    pageTitle="Povolení ke vzdálené ploše pro cloudovým službám (Node.js)" 
    description="Zjistěte, jak chcete povolit přístup ke vzdálené ploše pro virtuálních počítačích hostingu Azure Node.js aplikace." 
    services="cloud-services" 
    documentationCenter="nodejs" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="nodejs" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="enabling-remote-desktop-in-azure"></a>Povolení vzdálené plochy v Azure

Vzdálené plochy umožňuje přístup k ploše role instanci spuštěné v Azure. Připojení ke vzdálené ploše můžete konfigurovat virtuální počítač nebo Poradce při potížích s aplikací.

> [AZURE.NOTE] Tento článek se týká aplikacím Node.js hostované jako cloudové služby Azure.


## <a name="prerequisites"></a>Zjistit předpoklady pro

- Instalace a konfigurace [Prostředí Powershell Azure](../powershell-install-configure.md).
- Nasazení aplikace od Node.js Azure cloudové služby. Další informace najdete v tématu [Vytvoření a nasazení aplikace Node.js cloudové služby Azure](cloud-services-nodejs-develop-deploy-app.md).


## <a name="step-1-use-azure-powershell-to-configure-the-service-for-remote-desktop-access"></a>Krok 1: Použití Azure Powershellu ke konfiguraci služby pro přístup ke vzdálené ploše

Použít Vzdálená plocha, musíte aktualizovat definice Azure služby a konfigurace uživatelské jméno, heslo a certifikát. 

Proveďte následující kroky z počítače, který obsahuje zdrojové soubory aplikace.

1. **Prostředí Windows PowerShell** spusťte jako správce. (V **Nabídce Start** nebo **Obrazovce Start**, vyhledejte **Prostředí Windows PowerShell**.)

2.  Přejděte na adresář, který obsahuje definice služby (.csdef) a soubory služby konfigurace (.cscfg).

3. Zadejte následující rutinu Powershellu:

        Enable-AzureServiceProjectRemoteDesktop

4. Do příkazového řádku zadejte uživatelské jméno a heslo.

    ![Povolit azureserviceprojectremotedesktop][enable-rdp]

3.  Zadejte následující rutinu Powershellu publikovat změny:

        Publish-AzureServiceProject

    ![publikování azureserviceproject][publish-project]

## <a name="step-2-connect-to-the-role-instance"></a>Krok 2: Připojení k instanci rolí

Po publikování definici service update můžete připojit k instanci rolí.

1.  [Azure klasické portál]vyberte **Cloudovým službám** a pak vyberte služby.

    ![Azure klasické portálu][cloud-services]

2.  Klikněte na **případy**a potom na **výrobní** nebo **pracovní** zobrazíte instancí služby. Vyberte instanci a pak klikněte na **Připojit** v dolní části stránky.

    ![Na stránce instance][3]

2.  Po klepnutí na tlačítko **Připojit**webového prohlížeče vás vyzve k uložení souboru RDP. Otevřete tento soubor. (Například pokud používáte Internet Explorer, klikněte na **Otevřít**.)

    ![Objeví se výzva k otevření nebo uložení souboru RDP][4]

3.  Při dalším otevření souboru, zobrazí se následující výzvy zabezpečení:

    ![Zobrazení výzvy zabezpečení systému Windows][5]

4.  Klikněte na **Připojit**, a bude zobrazeno upozornění zabezpečení k zadání přihlašovacích údajů pro přístup k instanci systému. Zadání hesla vytvořeného v [kroku 1] [krok 1: Konfigurace služby pro přístup ke vzdálené ploše pomocí prostředí PowerShell Azure] a klikněte na **OK**.

    ![výzva uživatelské jméno a heslo.][6]

Po vytvoření připojení k připojení ke vzdálené ploše zobrazí plocha instance v Azure. 

![Relací vzdálené plochy][7]

## <a name="step-3-configure-the-service-to-disable-remote-desktop-access"></a>Krok 3: Konfigurace služby zakázat přístup ke vzdálené ploše 

Pokud už vyžadují připojení ke vzdálené ploše na instance role v cloudu, zakážete přístup ke vzdálené ploše pomocí [Prostředí PowerShell Azure].

1.  Zadejte následující rutinu Powershellu:

        Disable-AzureServiceProjectRemoteDesktop

2.  Zadejte následující rutinu Powershellu publikovat změny:

        Publish-AzureServiceProject

## <a name="additional-resources"></a>Další zdroje informací

- [Vzdálený přístup k roli instancí v Azure] 
- [Pomocí vzdálené plochy s Azure role]
- [Středisko pro vývojáře Node.js](/develop/nodejs/)

  [Azure Powershellu]: http://go.microsoft.com/?linkid=9790229&clcid=0x409

[Azure klasické portálu]: http://manage.windowsazure.com
[publish-project]: ./media/cloud-services-nodejs-enable-remote-desktop/publish-rdp.png
[enable-rdp]: ./media/cloud-services-nodejs-enable-remote-desktop/enable-rdp.png
[cloud-services]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-services-remote.png
[3]: ./media/cloud-services-nodejs-enable-remote-desktop/cloud-service-instance.png
[4]: ./media/cloud-services-nodejs-enable-remote-desktop/rdp-open.png
[5]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-12.png
[6]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-13.png
[7]: ./media/cloud-services-nodejs-enable-remote-desktop/remote-desktop-14.png
  
[Vzdálený přístup k instancí Role v Azure]: http://msdn.microsoft.com/library/windowsazure/hh124107.aspx
[Pomocí vzdálené plochy s Azure role]: http://msdn.microsoft.com/library/windowsazure/gg443832.aspx
 