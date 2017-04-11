<properties 
    pageTitle="Postup při konfiguraci do cloudové služby (portál) | Microsoft Azure" 
    description="Zjistěte, jak nakonfigurovat cloudovým službám v Azure. Zjistěte, jak aktualizovat konfiguraci služby cloudu a konfigurace vzdálený přístup k roli instance. Tyto příklady používají portálu Azure." 
    services="cloud-services" 
    documentationCenter="" 
    authors="Thraka" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="cloud-services" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/11/2016"
    ms.author="adegeo"/>

# <a name="how-to-configure-cloud-services"></a>Postup při konfiguraci Cloudovým službám

> [AZURE.SELECTOR]
- [Azure portálu](cloud-services-how-to-configure-portal.md)
- [Azure klasické portálu](cloud-services-how-to-configure.md)

Konfigurace nejčastěji používaná nastavení do cloudové služby na portálu Azure. Nebo pokud se vám aktualizace souborů konfigurace přímo, stáhněte si soubor konfigurace služby aktualizovat a potom uložit doplněný soubor a aktualizovat nejnovějšími změnami konfigurace cloudové služby. Oběma způsoby konfigurace aktualizace jsou použiti na všechny instance role.

Můžete spravovat instancemi cloudové služby role nebo Vzdálená plocha do nich.

Azure pouze zajistit dostupnost služby 99.95 procenta během aktualizací konfigurace Pokud máte alespoň dvě instance role pro každý roli. Umožňující jednoho virtuálního počítače zpracovat žádosti klienta o době, kdy je aktualizován druhé. Další informace najdete v tématu [Smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

## <a name="change-a-cloud-service"></a>Změna do cloudové služby

Po otevření [Azure portál](https://portal.azure.com/), přejděte do cloudové služby. Tady je spravovat mnoho aspektů. 

![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)

**Nastavení** nebo **všechny** odkazy otevřou zásuvné **Nastavení** , kde můžete změnit **Vlastnosti**, změňte **Konfigurace**, Správa **certifikátů**, nastavení **upozornění pravidla**a správa **uživatelů** , kteří mají přístup ke cloudové služby.

![Zásuvné nastavení služby Azure cloudu](./media/cloud-services-how-to-configure-portal/cs-settings-blade.png)

>[AZURE.NOTE]
>Operační systém používaný ke cloudové službě nelze změnit pomocí **Azure portál**, můžete změnit pouze toto nastavení přes [Azure klasické portálu](http://manage.windowsazure.com/). Tento podrobný [tady](cloud-services-how-to-configure.md#update-a-cloud-service-configuration-file).

## <a name="monitoring"></a>Sledování

Oznámení můžete přidat ke cloudové službě. Klikněte na **Nastavení** > **Pravidel upozornění** > **upozornění na Přidat**. 

![](./media/cloud-services-how-to-configure-portal/cs-alerts.png)

Tady můžete nastavit upozornění. S **Mertic** rozevíracím seznamu můžete nastavit upozornění pro následující typy dat.

- Čtení disku
- Zápis disku
- V síti
- Sítě se
- Procento využití procesoru 

![](./media/cloud-services-how-to-configure-portal/cs-alert-item.png)

### <a name="configure-monitoring-from-a-metric-tile"></a>Konfigurace sledování z metrických vedle sebe

Místo použití **Nastavení** > **Upozornění pravidla**, kliknete na některou dlaždici metrických v části **Sledování** zásuvné **cloudové služby** .

![Cloudová služba sledování](./media/cloud-services-how-to-configure-portal/cs-monitoring.png)

Tady můžete přizpůsobení grafu použít s dlaždici, nebo můžete přidat pravidlo výstrahy.


## <a name="reboot-reimage-or-remote-desktop"></a>Restartujte počítač, reimage nebo Vzdálená plocha

V současné době nejde nakonfigurovat vzdálené plochy pomocí **Azure portálu**. Však můžete nastavit ho přes [Azure klasické portál](cloud-services-role-enable-remote-desktop.md), [Powershellu](cloud-services-role-enable-remote-desktop-powershell.md), nebo pomocí [Aplikace Visual Studio](../vs-azure-tools-remote-desktop-roles.md). 

Nejdřív klepněte na instance služby cloudu.

![Instanci služby cloudu](./media/cloud-services-how-to-configure-portal/cs-instance.png)

Z zásuvné, otevře uou můžete zahájit připojení ke vzdálené ploše vzdáleně Restartujte instanci a vzdáleně reimage (začínáte s obrázkem na svěže) instance.

![Tlačítka instanci služby cloudu](./media/cloud-services-how-to-configure-portal/cs-instance-buttons.png)



## <a name="reconfigure-your-cscfg"></a>Změna konfigurace vaší .cscfg

Budete muset znovu můžete nakonfigurovat cloudových služeb v souboru [Konfigurace služby (cscfg)](cloud-services-model-and-package.md#cscfg) . Nejdřív budete muset stáhnout soubor .cscfg, upravit a pak ho nahrajte.

1. Klikněte na ikonu **Nastavení** a propojení **všechna nastavení** na zásuvné **Nastavení** .

    ![Nastavení stránky](./media/cloud-services-how-to-configure-portal/cloud-service.png)

2. Klikněte na položku **Konfigurace** .

    ![Konfigurace zásuvné](./media/cloud-services-how-to-configure-portal/cs-settings-config.png)

3. Klikněte na tlačítko **Stáhnout** .

    ![Ke stažení](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-download.png)

4. Po aktualizaci service konfigurační soubor nahrát a nainstalovat aktualizace konfigurace:

    ![Nahrání](./media/cloud-services-how-to-configure-portal/cs-settings-config-panel-upload.png) 
    
5. Vyberte soubor .cscfg a klikněte na **OK**.

            
## <a name="next-steps"></a>Další kroky

* Zjistěte, jak [nasadit do cloudové služby](cloud-services-how-to-create-deploy-portal.md).
* Konfigurace [vlastní název domény](cloud-services-custom-domain-name-portal.md).
* [Spravovat Cloudová služba](cloud-services-how-to-manage-portal.md).
* Konfigurace [certifikáty ssl](cloud-services-configure-ssl-certificate-portal.md).