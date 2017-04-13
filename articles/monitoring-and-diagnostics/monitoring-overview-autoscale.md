<properties
    pageTitle="Základní informace o nastavení automatické měřítko ve virtuálních počítačích Microsoft Azure, cloudovými službami a webových aplikací Web Apps | Microsoft Azure"
    description="Základní informace o nastavení automatické měřítko v Microsoft Azure. Platí pro virtuálních počítačích, cloudovými službami a webových aplikací Web Apps."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Základní informace o nastavení automatické měřítko ve virtuálních počítačích Microsoft Azure, cloudovými službami a webových aplikací Web Apps

Tento článek popisuje co automatické Microsoft Azure měřítko, výhod a jak začít s ním pracovat.  

Automatické měřítko Azure Monitor platí jenom pro [Virtuální počítač měřítko sady](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Cloud Services](https://azure.microsoft.com/services/cloud-services/)a [Aplikaci služby – Web Apps](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure obsahuje dva způsoby automatické měřítko. Starší verzí systému automatické měřítko platí pro virtuálních počítačích (dostupnost sady). Tato funkce má omezenou podporu a doporučujeme migraci do sady měřítko OM podporu rychlejší a spolehlivější automatické měřítko. Odkaz na používání starší technologii je součástí tohoto článku.  


## <a name="what-is-autoscale"></a>Co je automatické měřítko?

Automatické měřítko umožňuje mít správné množství prostředků systém zpracovávání zatížení aplikace. Umožňuje přidávat materiály ke zpracování zvýšení načítání a také uložit peníze odebráním zdroje, které jsou umístěným nečinný. Určení minimální a maximální počet instancí spustit a přidat nebo odebrat VMs automaticky na základě sady pravidel. Problémy minimální něco v ní se aplikace vždy pracuje i bez zatížení. S maximální limity vaše náklady možné každou hodinu. Můžete změnit velikost automaticky mezi těmito dvěma mezními hodnotami pomocí pravidla, které vytvoříte.

 ![Vysvětlení nastavení automatické měřítko. Přidání a odebrání VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Pokud jsou splněny pravidlo podmínky, se při spuštění aktivují jedné nebo víc akcí automatické měřítko. Můžete přidat a odebrat VMs nebo dělat další věci. Konceptuální diagram níž znázorňuje tento proces.  

 ![Koncepční automatické měřítko vývojový Diagram](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Vysvětlení nastavení automatické měřítko obrázku
Následující vysvětlení platí pro použitelné části předchozím obrázku.   

### <a name="resource-metrics"></a>Metriky zdroje
Zdroje posílat metriky, které zpracovávají později pravidly. Metriky pocházet přes různé metody.
Sady měřítko OM používá telemetrickými daty z Azure diagnostiky agentů že telemetrie pro Web apps a cloudové služby je přímo z Azure infrastruktury. Některé často používané statistické zahrnout využití procesoru, využití paměti, počty vlákna, délka fronty a využití místa na disku. Seznam jaké telemetrickými daty, můžete použít najdete v článku [Automatické měřítko běžné metriky](insights-autoscale-common-metrics.md).

### <a name="time"></a>Čas
Pravidla založená na plán vycházejí UTC. Při nastavování pravidel musí správně nastavit časové pásmo.  

### <a name="rules"></a>Pravidla
Diagram zobrazuje jenom jedno pravidlo automatické měřítko, ale můžete mít řada z nich. V případě potřeby vaší situaci můžete vytvořit složité překrývající se pravidla.  Zahrnout typech pravidel  

 - **Na základě míru** – třeba jak to provést při využití procesoru je větší než 50 %.
 - **Založená na čase** – například aktivační událost webhook každé 8: 00 v Sobotou v dané časové pásmo.

Pravidla založená na míru změřit aplikace zatížení a přidávat nebo odebírat VMs založené na této načíst. Pravidla založená na plán umožňují měřítko najdete v článku vzorků čas ve vaší načítání a chcete změnit velikost před možné načítání zvýšení nebo snížení nastane.  


### <a name="actions-and-automation"></a>Akce a automatizace

Pravidla může vyvolat alespoň jeden typ akce.

- **Měřítko** – měřítko VMs či oddálení
- **E-mail** – odeslat e-mail k odběru správci, dalších správců nebo další e-mailovou adresu, kterou zadáte
- **Automatické prostřednictvím webhooks** - webhooks volání, která může vyvolat více komplexních akcí ve i mimo ni Azure. Uvnitř Azure můžete začít postupu runbook Azure automatizaci, funkce Azure nebo Azure logiky aplikace. Příklad 3 Adresa URL účastníky mimo Azure obsahovat služby, jako třeba časová rezerva a Twilio.


## <a name="autoscale-settings"></a>Nastavení automatické měřítko
Automatické měřítko použít následující terminologie a strukturu.

- **Nastavení automatické měřítko** čte modulem Automatické měřítko se rozhodnout, jestli chcete změnit velikost nahoru nebo dolů. Obsahuje jednu nebo více profily, informace o cílové zdrojů a nastavení oznámení.
    - **Automatické měřítko profil** je tvořen kombinací objemu nastavení sady pravidel aktivačními událostmi a měřítko akcí pro profil a opakování. Můžete nechat více profily, které umožňují starat o překrývající se odlišné požadavky.
        - **Nastavení kapacity** označuje, minimum, maximum a výchozích hodnot pro počet instancí. [příslušné místo použití fík 1]
        - **Pravidlo** obsahuje aktivační – metrických aktivační událost nebo aktivační času – a měřítko akci označující, zda by měly automatické měřítko měřítko nahoru nebo dolů po splněné toto pravidlo.
        - **Opakování** označuje, kdy umístění profil platnost automatické měřítko. Budete moci profily automatické jiné měřítko pro různou dny v týdnu, například.
- **Nastavení oznámení** Určuje, jaké oznámení by měla proběhnout při výskytu události automatické měřítko podle splňující kritéria některého nastavení automatické měřítko profily. Automatické měřítko můžete kontaktovat jeden nebo více e-mailové adresy nebo volat na jeden nebo více webhooks.

![Nastavení automatické Azure měřítko profilu a strukturu pravidla](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Úplný seznam polí, která dokáže nahradit a popisy najdete v rozhraní [REST API automatické měřítko](https://msdn.microsoft.com/library/dn931928.aspx).

Příklady kódu naleznete v tématu

* [Použití šablon správce prostředků pro OM měřítko sady Upřesnit konfiguraci automatické měřítko](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Automatické měřítko REST API](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Vodorovné a svislé měřítka

Automatické měřítko kombinace kláves rozšíří zdrojů v pouze škály vodorovně, který se zvýšením (",") nebo snížení ("") do pole počet OM instance.  Vodorovné ose měřítko, což je flexibilnější v cloudu situaci jako umožňuje spuštění potenciálně tisíce VMs zpracovávání načíst. Změna měřítka svislé je něco jiného. Pořád stejný počet VMs, ale kvůli kterému OM další ("nahoru") nebo méně ("dolů") výkonné. Power měří se v paměti rychlosti procesoru, místo na disku, atd.  Změna měřítka svislé má další omezení. Jsou závislé na dostupnost větší hardwaru, což se může lišit podle regionů a rychle narazí a horní mez. Změna měřítka svislé obvykle vyžaduje OM zastavit a zahájení. Další informace najdete v tématu [svisle měřítko Azure virtuálního počítače s Azure automatizaci](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Metody aplikace access
Můžete nastavit automatické měřítko prostřednictvím

- [Azure portálu](insights-how-to-scale.md)
- [Prostředí PowerShell](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Různé platformy rozhraní příkazového řádku (rozhraní příkazového řádku)](insights-cli-samples.md#autoscale )
- [Azure Monitor REST API](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Podporované služby pro automatické měřítko


| Služba                              | Schéma a dokumenty                                       |
|--------------------------------------|-----------------------------------------------------|
| Web Apps                             | [Změna měřítka Web Apps](insights-how-to-scale.md)              |
| Cloud Services                       | [Automatické měřítko do cloudové služby](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuálních počítačích: Classic           | [Změna velikosti sady dostupnost klasické virtuálního počítače](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuálních počítačích: Sady měřítko Windows| [Změna měřítka OM měřítko nastaví v systému Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtuálních počítačích: Nastaví Linux měřítko  | [Změna měřítka OM měřítko sad v Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuálních počítačích: Příklad Windows   | [Použití šablon správce prostředků pro OM měřítko sady Upřesnit konfiguraci automatické měřítko](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Další kroky

Další informace o nastavení automatické měřítko, používá návody automatické měřítko výše uvedených nebo naleznete v následujících zdrojích:

- [Azure Monitor automatické měřítko běžné metriky](insights-autoscale-common-metrics.md)
- [Doporučené postupy pro automatické měřítko Azure Monitor](insights-autoscale-best-practices.md)
- [Odeslání e-mailu a webhook oznámení pomocí automatické měřítko akce](insights-autoscale-to-webhook-email.md)
- [Automatické měřítko REST API](https://msdn.microsoft.com/library/dn931953.aspx)
- [Poradce při potížích virtuálního počítače měřítko sady nastavení automatické měřítko](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
