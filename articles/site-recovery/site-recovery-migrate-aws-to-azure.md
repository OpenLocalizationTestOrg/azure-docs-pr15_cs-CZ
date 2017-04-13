<properties
    pageTitle="Migrace virtuálních počítačích Windows z Amazon webové služby Azure s obnovení webu | Microsoft Azure"
    description="Tento článek popisuje, jak migrovat virtuálních počítačích Windows službou Azure pomocí obnovení webu Azure v Amazon webové služby (AWA)."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="backup-recovery"
    ms.date="08/22/2016"
    ms.author="raynew"/>

#  <a name="migrate-windows-virtual-machines-in-amazon-web-services-aws-to-azure-with-azure-site-recovery"></a>Migrace virtuálních počítačích Windows v Amazon webové služby AWS Azure s obnovení webu Azure

## <a name="overview"></a>Základní informace

Vítá vás obnovení Azure webu. Tento článek použijte k migraci Windows instance spuštěna v AWS Azure s obnovení webu. Než začnete, Všimněte si, že:

- Azure obsahuje dva různé nasazení modely pro vytváření grafů a práci s prostředky: Správce prostředků Azure a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení. Toto jsou základní kroky migrace stejné jestli jste konfigurujete obnovení webu ve Správci zdrojů nebo klasickém. Ale pokyny uživatelského rozhraní a snímky obrazovek v tomto článku se vztahují k portálu Azure.
- **Nyní můžete jenom migrovat z AWS do Azure. Můžete převzít VMs z AWS Azure, ale můžete nelze navrácení je znovu. Neexistuje žádný probíhající replikace.**
- Migrace pokyny v tomto článku jsou založeny na pokyny k replikace fyzické počítače do Azure. V [replikovat VMs VMware nebo pole fyzicky serverů Azure](site-recovery-vmware-to-azure.md), které popisuje, jak replikovat fyzický server Azure portálu obsahuje odkazy na kroky.
- Pokud nastavujete obnovení webu na portálu klasické, postupujte podle podrobné pokyny v [tomto článku](site-recovery-vmware-to-azure-classic.md). **Měli byste už použít** pokyny v tomto [článku starší verze](site-recovery-vmware-to-azure-classic-legacy.md).

Vystavení komentáře nebo dotazy v dolní části tohoto článku nebo na [Fórum obnovení služby Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)


## <a name="prerequisites"></a>Zjistit předpoklady pro

Budete potřebovat pro toto nasazení

- **Konfigurace serveru**: OM místním systémem Windows Server 2012 R2, která funguje jako konfigurační server. Můžete nainstalovat jiné součásti obnovení webu (včetně proces serveru a předlohy na cílový) v tomto OM příliš. Další informace v [scénář architektura](site-recovery-vmware-to-azure.md#scenario-architecture) a [Konfigurace serveru požadavcích](site-recovery-vmware-to-azure.md#configuration-server-prerequisites).
- **EC2 OM instance**: instance s Windows, kterou chcete migrovat.

## <a name="deployment-steps"></a>Kroků nasazení

Tato část popisuje kroků nasazení v novém portálu Azure. V případě potřeby kroků nasazení pro obnovení webu na portálu klasického naleznete [v tomto článku](site-recovery-vmware-to-azure-classic.md).

1. [Vytvoření trezoru](site-recovery-vmware-to-azure.md#create-a-recovery-services-vault).
2. [Nasazení konfigurační server](site-recovery-vmware-to-azure.md#step-2-set-up-the-source-environment).
3. Po nasazení konfigurační server ověřte, zda můžete komunikovat s VMs, které chcete migrovat.
4. [Nastavení opakování](site-recovery-vmware-to-azure.md#step-4-set-up-replication-settings). Vytvoření zásad replikace a přiřaďte konfigurační server.
5. [Instalace služby nastavení mobilních zařízení](site-recovery-vmware-to-azure.md#step-6-replication-application). Každý OM, které chcete zamknout vyžaduje službu nastavení mobilních zařízení nainstalovaný. Tuto službu odešle data na server obrázku. Služba nastavení mobilních zařízení můžete být nainstalovány ručně nebo posune a automaticky serverem proces při zapnuté funkci ochranu OM. Pravidla brány firewall na EC2 instance, které chcete migrovat by měl nakonfigurovat tak, aby nabízených instalaci této služby. Skupina zabezpečení pro EC2 instance má následující pravidla:

    ![pravidla brány firewall](./media/site-recovery-migrate-aws-to-azure/migrate-firewall.png)

6. [Povolení replikace](site-recovery-vmware-to-azure.md#enable-replication). Povolte pro VMs, kterou chcete migrovat. Můžete zjistit EC2 instance pomocí soukromé IP adresy, které se dá dostat z konzoly EC2.
7. [Spuštění neplánované převzetí](site-recovery-failover.md#run-an-unplanned-failover). Po dokončení počáteční replikace můžete spustit neplánované převzetí z AWS Azure pro každou OM. Pokud chcete můžete vytvořit plán obnovy a spustit neplánované převzetí migrovat víc virtuálních počítačích z AWS Azure. [Další informace](site-recovery-create-recovery-plans.md) o různých plánech obnovení.

## <a name="next-steps"></a>Další kroky

Další informace o ostatních případech replikace v [Co je obnovení webu Azure?](site-recovery-overview.md)
