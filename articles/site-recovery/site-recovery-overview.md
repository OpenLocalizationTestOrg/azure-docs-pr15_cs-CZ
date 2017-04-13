<properties
    pageTitle="Co je obnovení webu? | Microsoft Azure"
    description="Přehled služby Azure obnovení webu a shrnuje scénáře nasazení."
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="cfreeman"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/13/2016"
    ms.author="raynew"/>

#  <a name="what-is-site-recovery"></a>Co je obnovení webu?

Vítá vás obnovení Azure webu! Tento článek obsahuje rychlý přehled službu obnovení webu a jak přispívá pro vaše podnikání.

Vaše organizace musí nepřerušený a strategii havárie obnovení (BCDR), která aplikace, pracovního vytížení a data bezpečných a k dispozici během zachovat plánované a neplánované prostoje a obnoví normální pracovní podmínky co nejdříve. Obnovení webu je Azure služba, která přispívá k této strategie.

Obnovení webu orchestrates replikace pracovní vytížení místní fyzické servery a virtuálních počítačích. Servery a VMs můžete replikovat z primární datacentra s cloudem (Azure), nebo vedlejší datacentra. Pokud výpadků dojde k primární webu, můžete převzít sekundární web chránit aplikace a úloh přístupných osobám s postižením k dispozici. Můžete být zpět na svém hlavním pracovišti při vrátí na normálně.

## <a name="site-recovery-in-the-azure-portal"></a>Obnovení webu Azure portálu

Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření a práce se zdroji. Správce prostředků Azure model a model klasické služby správy. Azure má dvě portály – i [Azure klasické portál](https://manage.windowsazure.com/) , který podporuje modelu klasické nasazení a [Azure portál](https://portal.azure.com) s podporou klasickou a modely správce prostředků.

- Obnovení webu je k dispozici v portálu klasické a portálu Azure.
- Na portálu Azure klasické podporují obnovení webu s modelem klasické služby správy.
- Na portálu Azure může podporovat klasické modelu nebo správce prostředků nasazení. 

Informace v tomto článku platí pro klasickou a Azure portálu nasazení. Rozdíly jsou uvedeny v případě potřeby.


## <a name="why-deploy-site-recovery"></a>Proč nasadit obnovení webu?

Tady je obnovení webu můžete provést pro svoji firmu:

- **Zjednodušení BCDR**– můžete zpracovat replikace, převzetí a obnovení více pracovního vytížení na jednom místě v portálu Azure. Obnovení webu orchestrates replikace a překlopení, ale není zachytit data aplikace nebo máte žádné informace.
- **Flexibilní replikace poskytovat**– pomocí obnovení webu můžete replikovat pracovní vytížení podporované VMs Hyper-V VMware VMs a fyzické servery systému Windows nebo Linux.
- **Testování replikace snadno provést**– obnovení webu poskytuje převzetí služeb při selhání test pro podporu havárie obnovení cvičení beze změny provozním prostředí.
- **Převzetí a obnovit**– spuštění plánované převzetí služeb při selhání pro očekávané výpadků se ztrátou dat nula nebo neplánované převzetí služeb při selhání s minimálními ke ztrátě (podle frekvence replikace) pro neočekávané havárie. Po selhání můžete navrácení primární weby. Obnovení webu poskytuje obnovení plánů, které mohou obsahovat skripty a Azure automatizaci sešity tak, aby je možné upravit převzetí a obnovení vícevrstvého aplikací.
- **Odstranění sekundární datacentra**– můžete replikovat pracovního vytížení Azure, namísto sekundární webu. Tím se omezí na náklady a složitosti zachování sekundární datacentra. Replikovanou ukládají se data v úložišti Azure s všechny odolnost, které obsahuje. VMs vzniká replikovanou daty, když dojde k selhání.
- **Integrace s existujících technologií BCDR**– obnovení webu lze integrovat s dalšími funkcemi BCDR. Můžete například obnovení webu chránit SQL Server backendovou podnikové úloh, včetně nativní podpory AlwaysOn serveru SQL, ke správě převzetí dostupnost skupiny.

## <a name="what-can-i-replicate"></a>Co lze replikovat?

Tady je přehled co replikovat pomocí obnovení webu.

![Místní do místního nasazení](./media/site-recovery-overview/asr-overview-graphic.png)

**REPLIKOVAT** | **REPLIKOVAT DO** 
---|---
Pracovní vytížení místní VMware VMs | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundární webu](site-recovery-vmware-to-vmware.md)
Pracovní vytížení VMs Hyper-V místním spravovaný v VMM mraky  | [Azure](site-recovery-vmm-to-azure.md)<br/><br/> [Sekundární webu](site-recovery-vmm-to-vmm.md) 
Pracovní zátěž spuštěná na místním Hyper-V VMs spravovaný v VMM mračnech s úložištěm SAN|  [Sekundární webu](site-recovery-vmm-san.md)
Pracovní vytížení VMs Hyper-V místním, bez VMM | [Azure](site-recovery-hyper-v-site-to-azure.md)
Pracovního vytížení umístěnými na serverech Windows/Linux fyzické místní | [Azure](site-recovery-vmware-to-azure-classic.md)<br/><br/> [Sekundární webu](site-recovery-vmware-to-vmware.md)


## <a name="what-workloads-can-i-protect"></a>K čemu úloh můžete chránit?

Obnovení webu umožňuje podporujících aplikaci BCDR, takže pracovního vytížení a aplikace nadále spuštěna jednotný způsob při výskytu výpadků. Obnovení webu poskytuje:

- **Aplikace konzistentních snímků**– počítačích replikovat s použitím aplikace konzistentní snímky aplikace jednoho nebo více osy. Kromě sběr dat disku, konzistenci aplikací snímky zachycení uvést všechna data v paměti a všechny transakce v procesu.
- **V blízkosti synchronní replikace**– obnovení webu poskytuje četnosti replikace je co nejnižší 30 sekund pro Hyper-V a nepřetržitý replikace VMware.
- **Plány flexibilní obnovy**– můžete vytvářet a přizpůsobovat plány obnovy s externími skripty a ruční akce. Integrace se službou Azure automatizaci runbooks umožňují obnovit celou aplikaci zásobníku jedním kliknutím.
- **Integrace se službou SQL Server AlwaysOn**– převzetí dostupnost skupiny můžete spravovat v plánech obnovení obnovení webu.
- **Automatizace knihovny**– bohaté knihovna Azure automatizaci poskytuje výrobní připravených specifické pro aplikaci skriptů, které si můžete stáhnout a integrovaný s obnovení webu.
- **Správa jednoduché sítě**– Správa sítě rozšířené obnovení webu a Azure zjednodušuje požadavky aplikace sítě, včetně rezervaci IP adresy, konfiguraci Vyrovnávání zatížení a integrace Azure přenosy správce pro efektivní síťový switchovers.


## <a name="next-steps"></a>Další kroky

- Další informace v [Jaké úloh obnovení webu chránit?](site-recovery-workload.md)
- Další informace o obnovení webu architektura [Jak funguje obnovení webu?](site-recovery-components.md)
 
