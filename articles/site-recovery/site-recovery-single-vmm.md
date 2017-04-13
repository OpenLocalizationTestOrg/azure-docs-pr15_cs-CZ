
<properties
    pageTitle="Obnovení webu Azure: Replikovat Hyper-V virtuálních počítačích na jednom serveru VMM | Microsoft Azure"
    description="Tento článek popisuje, jak replikovat Hyper-V virtuálních počítačích, pokud máte jenom jeden VMM server."
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
    ms.date="08/24/2016"
    ms.author="raynew"/>

#  <a name="replicate-hyper-v-virtual-machines-on-a-single-vmm-server"></a>Replikovat Hyper-V virtuálních počítačích na jednom VMM serveru

V tomto článku se dozvíte, jak replikovat Hyper-V virtuálních počítačích umístěn na serveru hostitele Hyper-V v cloudu VMM pouze v případě obsahuje jeden server VMM nasazení.

Azure obsahuje dva různé [modelů nasazení](../resource-manager-deployment-model.md) pro vytváření grafů a práci s prostředky: Správce prostředků Azure a klasické. Azure má i dvou portály – Azure klasické portál, který podporuje modelu klasické nasazení a portálu Azure s podporou pro oba modely nasazení. Tento článek obsahuje pokyny k nastavení replikace v portálu Azure.


Pokud máte nějaké dotazy po přečtení v tomto článku, vystavit je v komentářích Disqus v dolní části tohoto článku nebo na [Fórum komunity služby Azure Recovery](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="overview"></a>Základní informace

Pokud chcete replikovat Hyper-V VMs nachází na Hyper-V hosts v VMM a máte jenom jednu VMM server, můžete to udělat [replikovat na Azure](site-recovery-vmm-to-azure.md), nebo mezi mračnech na jednom VMM serveru.

Doporučujeme, protože je třeba počet ruční a převzetí a obnovení bezproblémové po nejsou replikace mezi mračnech replikovat do Azure. Pokud budete chtít replikovat s použitím jenom server VMM, můžete udělat následující:

- Replikovat se serverem VMM jednoho samostatný
- Replikovat se serverem jednoho VMM nasazenou v natažené clusteru systému Windows


## <a name="replicate-across-sites-with-a-single-standalone-vmm-server"></a>Replikovat napříč weby s jednoho samostatný VMM server

![Samostatný virtuální VMM server](./media/site-recovery-single-vmm/single-vmm-standalone.png)

V tomto scénáři nasadit jeden server VMM jako virtuální počítač primární webu a replikovat tento OM na vedlejší webu pomocí účtu obnovení webu a pro Hyper-V otevřené.

1. Nastavte si VMM na OM Hyper-V. Měli byste že nainstalovat instanci systému SQL Server používané VMM na stejné OM si ušetřit čas. Pokud chcete použít dojde k instanci vzdálené SQL serveru a výpadku, je potřeba obnovit tuto instanci před můžete obnovit VMM.
2. Ujistěte, že VMM server má aspoň dva mračnech nakonfigurované. Jeden cloudu obsahuje VMs chcete replikovat a v cloudu slouží jako sekundární umístění. By měla být v cloudu, který obsahuje VMs chcete zamknout:

    - Nejméně jedna VMM skupiny hostitelů obsahující jeden nebo více serverů hostitele Hyper-V každé skupiny Host (hostitel).
    - Nejméně jeden Hyper-V virtuálního počítače na všech serverech hostitele Hyper-V.

3. Vytvoření trezoru služby Recovery generovat a stáhnout trezoru registračního klíče a zaregistrovat VMM server v trezoru. Při registraci instalace zprostředkovatele obnovení webů Azure na serveru VMM.
4. Nastavení jedné nebo více mračnech na OM VMM a přidejte hosts Hyper-V těchto mračnech.
3. Konfigurace nastavení ochrany pro shluky. Zadejte název jednoho serveru VMM jako zdrojový a cílové umístění. Konfigurace mapování sítě, mapujete OM síť pro cloudu s VMs chcete zamknout OM sítě pro replikace cloudu.
4. Povolte počáteční replikace VMs chcete zamknout v síti, protože obě mračnech umístěných na stejný server.
4. V konzole pro Hyper-V správce povolit Hyper-V otevřené na hostiteli Hyper-V obsahující VMM OM a replikace v OM. Zkontrolujte, že není přidáte OM VMM Oblaka, které jsou chráněny obnovení webu. Zajistíte tím, že nastavení Hyper-V otevřené přepsáno tak, že obnovení webu.
5. Pokud chcete vytvořit plán obnovení, zadejte pro zdrojový a cílové stejný server VMM.

Postupujte podle pokynů v [tomto článku](site-recovery-vmm-to-vmm.md) k vytváření trezoru, registrace serveru a nastavení ochrany.

### <a name="what-to-do-in-an-outage"></a>Co dělat výpadek

Pokud potřebujete pracovat na vedlejší webu dokončení výpadek, postupujte takto:

1.  V konzole pro Hyper-V správce sekundární webu spusťte neplánované převzetí překlopení OM VMM z primárního, sekundárního Web.
2.  Zkontrolujte, že OM VMM zařídit i na vedlejší webu.
3.  V trezoru služby Recovery spusťte neplánované převzetí překlopení pracovní zátěž VMs z primárního na vedlejší mračnech. Dokončete neplánované převzetí VMs potvrzení záložní nebo vyberte jiné obnovení bod podle potřeby.
4.  Po dokončení neplánované převzetí uživatelé přístup k pracovní zátěž zdroje na vedlejší webu.

Když primární webu funguje normálně znova, postupujte takto:

1.  V konzole pro Hyper-V správce povolení zpětné replikace VMM OM, spusťte replikace z sekundární na primární.
2.  V konzole pro Hyper-V správce spusťte plánované přepojení selhání zpět OM VMM primární Web. Potvrďte převzetí dokončit, protože se. Povolte obráceném replikace replikace VMM z primární sekundární.
3.  Obnovení služby trezoru povolte obráceném replikace pro pracovní zátěž VMs zahájíte replikace z sekundární na primární.
4.  V trezoru služby Recovery spusťte plánované přepojení selhání zpět pracovní zátěž VMs primární Web. Potvrďte převzetí dokončit, protože se. Povolte obráceném replikace replikace pracovní zátěž VMs z primární sekundární.



## <a name="replicate-across-sites-with-a-single-vmm-server-in-a-stretched-cluster"></a>Replikovat napříč weby s VMM serveru natažené obrázku

![Skupinový virtuálního serveru VMM](./media/site-recovery-single-vmm/single-vmm-cluster.png)

Místo nasazení serveru VMM samostatného jako OM, který zreplikuje na vedlejší webu, můžete zpřístupnit VMM vysoce nasazením jako OM v clusteru selhání systému Windows. To umožňuje odolnost pracovního vytížení a ochrana před selháním hardwaru. Nasazení s obnovení webu by měly být OM VMM nasazeny roztáhnout clusteru přes geograficky samostatné weby. Akce:

1. VMM nainstalovat na počítač virtuální v clusteru selhání systému Windows a vyberte možnost spustili serveru vysoce dostupné při instalaci.
2. Instanci systému SQL Server, používaná VMM by měl replikovat se skupinami dostupnost SQL Server AlwaysOn tak, aby bylo otevřené databázi na vedlejší webu.
3. Postupujte podle pokynů v [tomto článku](site-recovery-vmm-to-vmm.md) k vytváření trezoru, registrace serveru a nastavení ochrany. Musíte si zaregistrovat každý VMM server clusteru služby Recovery trezoru. K tomuto účelu instalace zprostředkovatele na aktivní uzel a zaregistrovat VMM serveru. Nainstalujete zprostředkovatele na uzlech.

### <a name="what-to-do-in-an-outage"></a>Co dělat výpadek

Když výpadek selhání VMM server a jeho odpovídající databáze SQL serveru a zobrazená po kliknutí na vedlejší webu.


## <a name="next-steps"></a>Další kroky

[Další informace](site-recovery-vmm-to-vmm.md) o podrobné obnovení webu nasazení VMM VMM replikace.
