<properties
   pageTitle="Nasazení virtuální spotřebiče dostupnost | Microsoft Azure"
   description="Jak nasadit sítě virtuální zařízení v dostupnost."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Nasazení řízení podnikových virtuální spotřebiče ve vysoké dostupnosti

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek shrnuje sadu postupy pro nasazení virtuální spotřebiče síť (NVAs) vysoce dostupné způsobem. Než budete pokračovat, ujistěte se, víte, jak [uživatelem definovaných tras (UDR)] [ udr-overview] a [Služba Vyrovnávání zatížení] [ lb-overview] práce v Azure.

Můžete různých NVAs k dispozici v Azure marketplace rozšířit použitelnost funkce Azure stejným způsobem jako používáte zařízení v místním datacentra. Následující obrázek znázorňuje nasazení výběru z [jedné NVA] [ nva-scenario] jako zařízení brány firewall. 

![[0]][0]

Ačkoli předchozí nasazení pracuje, zavádí selhání v jednom místě. Pokud virtuální zařízení selže, žádné přenosy přecházel. Tento problém vyřešit, budete muset používat více NVAs. Však také vyžadujícího další nastavení a zdroje se nemusí používat v závislosti na vašim požadavkům.

Můžete jednu z následujících způsobů nasazení vysoce dostupných prostředí NVA.

|Řešení|Výhody|Co byste měli zvážit|
|---|---|---|
|Průniku s virtuální spotřebiče vrstvy 7|Všechny uzly jsou aktivní|Vyžaduje NVA, který můžete ukončit připojení a používat SNAT<br/>Vyžaduje samostatnou sadu NVAs přenosů pocházející z Internetu a od Azure<br/>Lze použít pouze pro přenos pocházející mimo Azure|
|Výstupní průniku s virtuální spotřebiče vrstvy 7|Všechny uzly jsou aktivní<br/>Zpracovat přenosy pocházejí z Azure |Vyžaduje NVA, který můžete ukončit připojení a používat SNAT<br/>Vyžaduje samostatnou sadu NVAs přenosů pocházející z Internetu a od Azure|
|Přepnout PIP UDR|Jednu sadu NVAs pro všechny přenosy<br/>Můžete zpracování všech přenosů (bez omezení pravidla port)|Aktivní pasivní<br/>Vyžaduje překlopení obrázku|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Průniku s virtuální spotřebiče vrstvy 7
Sada NVAs za vyrovnávání zatížení Azure umožňuje poskytovat připojení k Azure pracovního vytížení za kratších seznamů porty na straně serveru (například HTTP a HTTPS). Na následujícím obrázku zvýrazní jak poskytnout dostupnost v tomto scénáři na úrovni NVA.

![[1]][1]

V tomto případě musí virtuální zařízení sítě použít ukončit všechna připojení a předejte podsítě pracovní zátěž. Pracovní zátěž virtuálních počítačích (VMs) odpovídají NVA že dostal žádost a tok bez problémů. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Výstupní průniku s virtuální spotřebiče vrstvy 7
Můžete rozšířit předchozí architektura a přidat další dvojice NVAs zpracovávání přenos pocházející z Azure uskutečněných jednotlivými NVAs, jak je znázorněno na následujícím obrázku:

![[2]][2]

V tomto scénáři všechny přenosy pocházející z Azure směrovány do interní řešení pro vyrovnávání zatížení, distribuuje zatížení mezi jinou sadu NVAs. Tyto NVAs směrovat přenosy na Internetu pomocí jejich jednotlivé veřejných IP adres. 

## <a name="pip-udr-switch"></a>Přepnout PIP UDR
Vytvoření více štosech NVA pomocí dvou NVAs v režimu aktivní pasivní se můžete vyhnout. V tomto scénáři můžete přepnout veřejnou IP adresu (PIP) a uživatelem definovaných tras (UDRs) po ukončení uzel aktivní.  

![[3]][3]

Tento scénář je podobný jeden scénář NVA. Jediný rozdíl je Pokud chcete přepnout mezi NVAs musí změnit PIP a UDRs. Tyto změny se teď dá ručně, nebo můžete také automatizovat. K automatizaci, můžete nasadit aplikaci na obou NVAs, která kontroluje stavu aktivní uzel. Když je aktivní uzel dolů, aplikace můžete změnit PIP a UDRs chcete vytvořit odkaz na pasivní uzel.

Je možné implementace toto řešení používat [ZooKeeper] [ zookeeper] démon na NVAs zpracovávání zvolení vodicí znak (rozhodováním, který uzel je aktivní uzel). Jakmile zvolených vodicího znaku volání rozhraní REST API Azure odebrat PIP z uzel a připojte ji k vedoucí. Měli změnit také UDRs tak, aby ukazovaly k IP adrese soukromé nové vodicí znak.

## <a name="next-steps"></a>Další kroky

- Zjistěte, jak [implementovat DMZ mezi Azure a místních datacentra] [ dmz-on-prem] pomocí NVAs vrstvy-7.
- Zjistěte, jak [implementovat DMZ mezi Azure a Internetu] [ dmz-internet] pomocí NVAs vrstvy-7.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Jeden NVA architektura"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Vrstvy 7 průniku"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Vrstvy 7 průniku a výstupním"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktivní pasivní obrázku"