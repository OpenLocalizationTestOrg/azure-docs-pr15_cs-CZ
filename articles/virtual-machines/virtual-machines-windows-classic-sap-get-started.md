<properties
   pageTitle="Používání SAP na virtuálních počítačích Windows | Microsoft Azure"
   description="Vymazat o používání SAP na virtuálních počítačích Windows (VMs) v Microsoft Azure"
   services="virtual-machines-windows,virtual-network,storage"
   documentationCenter="saponazure"
   authors="MSSedusch"
   manager="timlt"
   editor=""
   tags="azure-service-management"
   keywords=""/>
<tags
   ms.service="virtual-machines-windows"
   ms.devlang="NA"
   ms.topic="campaign-page"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="na"
   ms.date="10/04/2016"
   ms.author="sedusch"/>

# <a name="using-sap-on-windows-virtual-machines-in-azure"></a>Používání SAP na virtuálních počítačích Windows v Azure

Cloud Computing je často používaný termínů, které získává více důležitost v rámci odvětví informačních malé společností až velkých a mezinárodních společnosti. Microsoft Azure je platformu cloudové služby od Microsoftu, která nabízí celou dokumentech nové možnosti. Teď zákazníci se rychle zřízení a zrušte zřízení aplikace jako cloudové služby, takže nejsou omezené pro technické záležitosti nebo rozpočtování omezení. Místo investovat čas a rozpočtové do hardwaru infrastruktury, společnosti zaměřit na aplikaci, obchodní procesy a jeho výhody pro zákazníky a uživatele.

Microsoft virtuálních počítačích Microsoft Azure nabízí komplexní infrastruktury jako platformu služby (IaaS). SAP NetWeaver na základě aplikace jsou podporovány na virtuálních počítačích Azure (IaaS). Dokumenty White Paper dole popisují, jak plánování a implementaci aplikací SAP NetWeaver založené na virtuálních počítačích Windows v Azure. Můžete taky implementace aplikací SAP NetWeaver založené na [virtuálních počítačích Linux](virtual-machines-linux-classic-sap-get-started.md).

[AZURE.INCLUDE [virtual-machines-common-classic-sap-get-started](../../includes/virtual-machines-common-classic-sap-get-started.md)]

## <a name="sap-netweaver-on-azure---ha"></a>SAP NetWeaver z Azure - HA

Název: SAP NetWeaver na Azure - clusterů SAP ASC/SCS instance pomocí Windows Server překlopení obrázku na Azure s s DataKeeper

Shrnutí: "Tento dokument se dozvíte, jak používat s DataKeeper nastavit vysoce dostupné konfigurace SAP ASC/SCS v Azure. SAP chrání jejich součástí selhání jako SAP ASC/SCS nebo Enqueue replikace služby s konfigurací systému Windows Server překlopení obrázku, které vyžadují sdílených disků v jednom místě. Tyto součásti SAP jsou důležité pro funkci SAP systému. Proto funkce vysoké dostupnosti potřebuje umístění na místě, abyste měli jistotu, že tyto součásti můžete udržovat selhání serveru nebo OM jako hotový s konfigurací clusteru systému Windows pro Hyper-V prostředí a čistý počítač. K srpna 2015 Azure na sobě neposkytuje sdílených disků, které by bylo nutné pro Windows na základě vysoce dostupné konfigurace potřebných pro tyto důležité součásti SAP. Pomocí tohoto produktu DataKeeper tak, že SIOS však můžete konfigurace systému Windows Server překlopení obrázku v případě potřeby pro SAP ASC/SCS založená na platformu Azure IaaS. Tento dokument v kroku fázích popisuje, jak nainstalovat konfigurace systému Windows Server překlopení obrázku s sdílených disku poskytovanou s Datakeeper v Azure. Papír vysvětluje podrobnosti v konfigurace na straně Azure, Windows a SAP, které konfigurace dostupnost práce optimální způsobem. Papír doplňuje si přečtěte následující dokumentaci instalace SAP a SAP poznámky, které představují primární prostředky pro zařízení a nasazení softwaru SAP na danou platformách.

Aktualizované: Srpen 2015

[Stáhnout příručku](http://go.microsoft.com/fwlink/?LinkId=613056)
