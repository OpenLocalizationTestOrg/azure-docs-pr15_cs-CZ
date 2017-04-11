<properties
    pageTitle="Akce: v případě Azure služby narušení, které mají vliv Azure Cloud Services | Microsoft Azure"
    description="Přečtěte si postup v případě přerušení služby Azure, které mají vliv Azure Cloud Services."
    services="cloud-services"
    documentationCenter=""
    authors="kmouss"
    manager="drewm"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="cloud-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="05/16/2016"
    ms.author="kmouss;aglick"/>

#<a name="what-to-do-in-the-event-of-an-azure-service-disruption-that-impacts-azure-cloud-services"></a>Akce: v případě Azure služby narušení, které mají vliv Azure Cloud Services

Společnost Microsoft pracujeme pevně abyste měli jistotu, že služeb společnosti Microsoft jsou vždy dostupné a když je potřebujete. Přinutí naše ovlivnit někdy vliv na to, nám způsoby, které způsobují přerušení poskytování služeb neplánované.

Microsoft poskytuje smlouva úrovni služeb (SLA) pro své služby jako potvrzení provozu a připojení. SLA pro jednotlivé služby Azure najdete v [Azure smlouvy o úrovni služeb](https://azure.microsoft.com/support/legal/sla/).

Azure už má mnoho předdefinované platformy, které podporují vysoce dostupných aplikací. Další informace o těchto služeb přečtěte si [havárie využití a dostupnost pro Azure aplikace](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Tento článek popisuje obnovení scénáři platí havárie případě, že celé oblasti dojde výpadku kvůli hlavní přírodní katastrofě nebo přerušení poskytování služeb široké. Toto jsou méně častých výskytů, ale je nutné připravit možnost, že je výpadku celé oblasti. Pokud celá oblast dojde k přerušení služby, místně záložní kopie dat dočasně bude k dispozici. Pokud jste povolili geo replikace, tři dalších kopií objektů BLOB Azure úložiště a tabulek jsou uložené v jiné oblasti. V případě výpadku dokončení místní nebo selhání, ve kterém oblasti primární je znovu mapuje Azure, všechny položky DNS k oblasti replikovat geo.

>[AZURE.NOTE]Mějte na paměti, že nemáte žádné publikum nemůže ovládat tento proces a proběhne pouze při přerušení poskytování služeb nevyžádaného datacentra. Z toho důvodu musí taky přes v jiných specifické pro aplikaci záložní strategie dosažení nejvyšší úrovně dostupnosti. Další informace naleznete v části informace o [strategie dat pro obnovení katastrofě](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md#DSDR). Pokud chcete mít možnost ovlivňovat vlastní překlopení, můžete chtít zvažte použití [geo nadbytečné úložiště přístup pro čtení (Vzdálená pomoc GRS)](../storage/storage-redundancy.md#read-access-geo-redundant-storage), která vytvoří kopii jen pro čtení dat v jiné oblasti.

Můžete dělat tyto méně častých výskytů, nabízíme v případě přerušení služby celé oblasti, kde je vaše aplikace Azure OM nasazena následující pokyny pro Azure virtuálních počítačích (VMs).

##<a name="option-1-wait-for-recovery"></a>Možnost 1: Počkejte obnovení
V tomto případě není nutná žádná akce na druhé straně. Víte, že Azure týmy pracují pečlivě obnovíte dostupnost služby. Zobrazí se aktuální stav služby na naše [Řídicí panel stavu služby Azure](https://azure.microsoft.com/status/).

>[AZURE.NOTE]Toto je nejlepší možností, pokud zákazník nenainstaloval obnovení webu Azure nebo vedlejší nasazení v jiné oblasti.

Pro zákazníky, kteří mají okamžitý přístup k jejich nasazeném cloudové služby jsou k dispozici následující možnosti.

>[AZURE.NOTE]Mějte na paměti, že tyto možnosti mít možnost ztrátu dat.     

##<a name="option-2-re-deploy-your-cloud-service-configuration-to-a-new-region"></a>Možnost 2: Znovu nasaďte konfigurace cloudové služby na novou oblast

Pokud jste ještě do původního kódu, můžete jednoduše jenom znovu nasadit aplikaci, přidružená konfigurace a přidružené prostředky do nového cloudové služby v novou oblast.  

Podrobnější informace o tom, jak vytvořit a nasazení aplikace cloudové služby, najdete v tématu [Vytvoření a nasazení do cloudové služby](./cloud-services-how-to-create-deploy-portal.md).

V závislosti na zdrojů dat aplikace budete muset zkontrolovat obnovení postupy pro zdroj dat aplikace.
  * Zdroje dat Azure úložiště najdete v článku [úložišti Azure replikace](../storage/storage-redundancy.md#read-access-geo-redundant-storage) informace o možnosti, které jsou k dispozici založené na modelu replikace zvolte aplikace.
  * Databáze SQL zdrojů, přečtěte si [Přehled: cloudu firmy kontinuitu databáze havárie obnovení a s databáze SQL](../sql-database/sql-database-business-continuity.md) informace o možnosti, které jsou k dispozici založen na vybraném replikace modelu aplikace.

##<a name="option-3-use-a-backup-deployment-through-azure-traffic-manager"></a>Možnost 3: Použití záložní nasazení prostřednictvím správce dopravy Azure
Tato možnost předpokládá, že jste vytvořili už vaše aplikace řešení s využitím místního havárie na paměti. Můžete použít tuto možnost, pokud už máte nasazení aplikace sekundární cloudové služby, které pracuje v jiné oblasti a připojení přes Správce kanálu přenosy. V tomto případě kontrola stavu sekundární nasazení. Pokud je v pořádku, můžete přesměrujete přenosy na ho pomocí Azure přenosy správce. Pomocí této strategie můžete využít výhod metodu směrování přenosu a konfigurace pořadí převzetí v Azure přenosy správce. Další informace najdete v tématu [jak nakonfigurovat nastavení přenosy správce](../traffic-manager/traffic-manager-overview.md#how-to-configure-traffic-manager-settings).

![Vyrovnávání Azure Cloud Services různých oblastí pomocí Správce přenosy Azure](./media/cloud-services-disaster-recovery-guidance/using-azure-traffic-manager.png)

##<a name="next-steps"></a>Další kroky

Další informace o tom, jak implementovat havárie využití a dostupnost strategie najdete v tématu [obnovení havárie a dostupnost pro Azure aplikace](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md).

Se dají podrobné technické informace o platformy cloudové možnosti, najdete v článku [příručku Azure odolnost proti chybám](../resiliency/resiliency-technical-guidance.md).

Pokud pokyny není zřejmé, nebo pokud budete chtít Microsoft provádět operace vaším jménem kontaktujte [Zákaznickou podporu](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
