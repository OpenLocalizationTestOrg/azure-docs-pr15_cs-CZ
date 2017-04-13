<properties
   pageTitle="Vytvoření struktury služby Azure clusterů v systému Windows Server a Linux | Microsoft Azure"
   description="Služba struktury clusterů spustit v systému Windows Server a Linux, což znamená, že byste měli nasazení a hostitelské služby struktury kdekoli aplikace můžete spustit Windows Server a Linux."
   services="service-fabric"
   documentationCenter=".net"
   authors="Chackdan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="chackdan"/>

# <a name="create-service-fabric-clusters-on-windows-server-or-linux"></a>Vytvoření struktury služby clusterů ve Windows Server a v Linuxu

Azure struktury služby umožňují vytvářet služby struktury clusterů na VMs nebo počítače se systémem Windows Server a Linux. Znamená to, že budete moci nasazení a spouštění aplikací služby struktury v jakémkoli prostředí, kde máte sadu Windows Server a Linux propojených počítačích, budete ho místní, Microsoft Azure nebo s kteréhokoliv poskytovatele cloudu.

##<a name="create-service-fabric-clusters-on-azure"></a>Vytvoření struktury služby clusterů na Azure

Vytvoření clusteru na Azure probíhá buď přes šablonu Model prostředků nebo portálu Azure. Přečtěte si [vytvořit cluster služby struktury pomocí šablony správce prostředků](service-fabric-cluster-creation-via-arm.md) nebo [Vytvoření struktury služby clusteru z portálu Microsoft Azure](service-fabric-cluster-creation-via-portal.md) Další informace.

## <a name="supported-operating-systems-for-clusters-on-azure"></a>Podporované operační systémy pro clusterů na Azure

Je možné vytvořit clusterů na VMs s těmito operační systémy:

* Windows serveru 2012 R2
* Windows serveru 2016 (po se ohlásí obecně k dispozici)
* Linux systémem Ubuntu 16.04 (ve veřejných verze preview) 


##<a name="create-service-fabric-standalone-clusters-on-premise-or-with-any-cloud-provider"></a>Vytvoření služby struktury samostatného clusterů místní nebo s kteréhokoliv poskytovatele cloudu

Služba struktury poskytuje instalační balíček pro potřeba vytvářet samostatný služby struktury clusterů místní nebo u kteréhokoli poskytovatele cloudu

Další informace o nastavení samostatnou službu struktury clusterů v systému Windows Server přečtěte si [vytváření clusteru struktury služby systému Windows Server](service-fabric-cluster-creation-for-windows-server.md)

### <a name="any-cloud-deployments-vs-on-premises-deployments"></a>Některé cloudu nasazení porovnání v místním nasazení
Proces vytváření clusteru služby struktury místní je podobný proces vytváření clusteru na libovolnou cloudu podle svého výběru sadou VMs. První kroky zřízení VMs upravuje cloudu poskytovatele nebo v místním prostředí, které používáte. Až budete mít sadu VMs s podporou mezi nimi připojení k síti, potom návodu pro nastavení balíčku služby struktury upravte nastavení clusteru a spusťte vytváření clusteru a skripty pro správu jsou stejné. To zajišťuje, že znalosti a zkušenosti provozní a Správa služby struktury clusterů převoditelné když budete chtít cílové novou hostitelské prostředí.

### <a name="benefits-of-creating-standalone-service-fabric-clusters"></a>K čemu je vytvoření samostatného služby struktury clusterů
* Máte možnost zvolit kteréhokoliv poskytovatele cloudu hostovat svůj cluster.
* Služby struktury aplikace, jednou napsali, lze spustit ve více hostitelské prostředí s minimálními žádné změny.
* Znalost vytváření aplikací struktury služba přenáší z jednoho hostitelské prostředí do druhého.
* Provozní zkušenosti spuštění a správě služby struktury clusterů mělo přes z jednoho prostředí do druhého.
* Obecných zákazníka reach neomezené hostitelské prostředí omezení.
* Další úroveň ochrany proti rozšířených výpadků a spolehlivosti existuje, protože můžete přesunout služby do jiného prostředí pro nasazení Pokud dat centra nebo cloudového poskytovatele má přerušení spojení.

## <a name="supported-operating-systems-for-standalone-clusters"></a>Podporované operační systémy pro samostatné clusterů
Je možné vytvářet clusterů na VMs nebo počítačích s těmito operační systémy:

* Windows serveru 2012 R2
* Windows serveru 2016 (po se ohlásí obecně k dispozici)
* Linux (už brzo)

## <a name="advantages-of-service-fabric-clusters-on-azure-over-standalone-service-fabric-clusters-created-on-premises"></a>Výhody služby struktury clusterů na Azure přes samostatná služby struktury clusterů vytvořili místní

Spuštěna clusterů struktury služby Azure poskytuje možnost výhody místní, takže pokud nemáte konkrétní úrazovém kde spustíte clusterů, pak doporučujeme můžete spustit na Azure. Na Azure nabízíme integrace s jinými Azure funkce a služby, které provádí operace a řízení clusteru jednodušší a spolehlivější.

* **Azure portálu:** Azure portál usnadňuje vytváření a Správa clusterů.

* **Azure správce prostředků:** Použití Správce prostředků Azure umožňuje snadno správu všech zdrojů používaných clusteru jako celek a zjednodušuje sledování náklady a fakturace.
* **Služba struktury obrázku jako zdroj Azure** Služby struktury obrázku je ARM zdroje, abyste mohli ho stejným způsobem dalších zdrojů ARM v Azure.
* **Integrace se službou Azure infrastruktury** Souřadnice struktury služby základní Azure infrastrukturu pro OS sítě a jiné inovace zlepšit dostupnost a spolehlivost aplikací.  
* **Diagnostiky:** Na Azure poskytneme integrace Azure diagnostiky a technologie pro analýzu protokolu.
* **Automatické měřítko:** U clusterů na Azure nabízíme předdefinované funkce Automatické měřítko kvůli měřítko sad virtuálního počítače. V místním prostředím a jiných prostředích cloudu budete muset vytvořit svou vlastní měřítko automatického funkce nebo měřítko ručně pomocí rozhraní API, která poskytuje struktury služby pro změnu velikosti clusterů.

## <a name="next-steps"></a>Další kroky
Vytvoření clusteru na VMs nebo počítače se systémem Windows serveru: [Vytvoření obrázku struktury služby systému Windows Server](service-fabric-cluster-creation-for-windows-server.md)

Vytvoření clusteru na VMs nebo počítače se systémem Linux: [Služba struktury na Linux](service-fabric-linux-overview.md)
