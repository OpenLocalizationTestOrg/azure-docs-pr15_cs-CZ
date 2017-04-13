<properties
    pageTitle="Vytvoření serverové farmy Sharepointu | Microsoft Azure"
    description="Rychle vytvořte novou farmu SharePoint 2013 nebo SharePoint 2016 v Azure."
    services="virtual-machines-windows"
    documentationCenter=""
    authors="JoeDavies-MSFT"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="josephd"/>

# <a name="create-sharepoint-server-farms"></a>Vytvoření serverové farmy Sharepointu

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]klasický modelu.

## <a name="sharepoint-2013-farms"></a>Farmách serverů SharePoint 2013

Pomocí portálu webu Microsoft Azure marketplace můžete rychle vytvořit předkonfigurovaná farmách serverů SharePoint serveru 2013. To můžete uložit můžete spoustu času, když potřebujete farmě SharePoint základní nebo vysoká dostupnost pro vývojáře/testovacím prostředí nebo SharePoint serveru 2013 jsou vyhodnocení jako spolupráce řešení pro vaši organizaci.

> [AZURE.NOTE] **Serverové farmě SharePoint** položky v Azure Marketplace portálu Azure byla odebrána. Byla nahrazena položky **Farma SharePoint 2013 bez-HA** a **ZÁBRA farmě Sharepointu 2013** .

Základní farmě Sharepointu se skládá ze tří virtuálních počítačích v této konfiguraci.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/Non-HAFarm.png)

Pomocí této konfiguraci farmy zjednodušené nastavení pro vývoj aplikací služby SharePoint nebo vyzkoušení první SharePoint 2013.

Vytvoření základní farmě Sharepointu (tři server):

1. Klikněte na [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-nonha/).
2. Klikněte na **nasazení**.
3. V podokně **Farma SharePoint 2013 bez-HA** klikněte na **vytvořit**.
4. Určení nastavení na kroky podokna **Vytvořit farma SharePoint 2013 bez-HA** a klikněte na **vytvořit**.

Vysoké dostupnosti farmě Sharepointu se skládá z devět virtuálních počítačích v této konfiguraci.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/HAFarm.png)

Můžete tento konfigurací farmy k testování vyšší klientů zatížení, dostupnost externího webu služby SharePoint a SQL Server AlwaysOn dostupnost skupiny farmě služby SharePoint. Můžete také tuto konfiguraci aplikace pro vývoj pro SharePoint v prostředí vysoké dostupnosti.

Vytvoření farmě Sharepointu vysoké dostupnosti (devět server):

1. Klikněte na [zde](https://azure.microsoft.com/marketplace/partners/sharepoint2013/sharepoint2013farmsharepoint2013-ha/).
2. Klikněte na **nasazení**.
3. V podokně **HA farmě Sharepointu 2013** klikněte na **vytvořit**.
4. Určení nastavení na sedm kroky podokna **Vytvořit 2013 HA farmě Sharepointu** a klikněte na **vytvořit**.

> [AZURE.NOTE] **SharePoint 2013 není HA farmy** nebo **Farmě Sharepointu 2013 HA** nelze vytvořit s Azure bezplatnou zkušební verzi.

Portál Azure obě tyto farmy vytvoří v cloudu – pouze virtuální síti prostřednictvím stavu webu internetového. Existuje bez připojení k webu virtuální privátní sítě nebo ExpressRoute zpět k síti vaší organizace.

> [AZURE.NOTE] Když vytvoříte základní nebo pomocí portálu Azure farmách serverů SharePoint vysoké dostupnosti, nemůžete určit existující skupiny zdrojů. Informace k alternativním řešením toto omezení, vytvořte tyto farmy s Azure Powershellu. Další informace najdete v tématu [Vytvoření SharePoint 2013 zařízením nebo zkoušení farmách s Azure Powershellu](https://technet.microsoft.com/library/mt743093.aspx#powershell).

## <a name="sharepoint-2016-farms"></a>Farmách serverů SharePoint 2016

Naleznete v [tomto článku](https://technet.microsoft.com/library/mt723354.aspx) pokyny k vytváření následující jednoduchým serverová farma SharePoint serveru 2016.

![sharepointfarm](./media/virtual-machines-windows-sharepoint-farm/SP2016Farm.png)

## <a name="managing-the-sharepoint-farms"></a>Správa farmách serverů SharePoint

Můžete spravovat servery tyto farmy přes připojení ke vzdálené ploše. Další informace najdete v tématu [přihlášení k virtuální počítač](virtual-machines-windows-hero-tutorial.md#log-on-to-the-virtual-machine).

Z webu Centrální správa Sharepointu můžete nakonfigurovat osobních webů, aplikace SharePoint a další funkce. Další informace najdete v tématu [Konfigurace služby SharePoint](http://technet.microsoft.com/library/ee836142.aspx).

## <a name="next-steps"></a>Další kroky

- Seznamte se s další [Konfigurace služby SharePoint](https://technet.microsoft.com/library/dn635309.aspx) v Azure infrastruktury služby.
