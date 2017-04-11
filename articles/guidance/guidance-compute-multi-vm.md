<properties
   pageTitle="Spouštění více virtuálních | Vytvořte odkaz architektura | Microsoft Azure"
   description="Jak spustit několika instancích spuštěných OM na Azure škálovatelnost, odolnost proti chybám, správu a zabezpečení."
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>Spouštění více virtuálních na Azure škálovatelnost a dostupnosti 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek shrnuje sadu osvědčené postupy pro spuštění několika instancích spuštěných virtuálních počítačích (OM) kvůli zlepšení škálovatelnost, dostupnost, správu a zabezpečení.   

V tomto architektura pracovní zátěž rozvržena OM instance. Existuje jednu veřejnou IP adresu a internetový provoz úměrně VMs pomocí vyrovnávání zatížení. Tato architektura se dá použít pro jednoúrovňových aplikaci, třeba příslušnosti webových aplikací nebo úložiště obrázku. Je taky stavební blok pro aplikace N-osy. 

Tento článek je založena na [systém jediný OM na Azure][single vm]. Doporučení v tomto článku platí taky pro tuto architekturu.

## <a name="architecture-diagram"></a>Diagram architektury

VMs v Azure vyžadují podpůrné materiály sítí a úložiště.

> Dokument aplikace Visio, který obsahuje tento diagram architektury je možné stáhnout z [webu služby Stažení softwaru][visio-download]. Tento diagram je na "Jej – karta s víc OM." 

![[0]][0]

Architektura má tyto prvky: 

- **Dostupnost nastavení.** [Nastavte dostupnost] [ availability set] obsahuje VMs a je nutné pro podporu [dostupnost SLA Azure VMs][vm-sla].

- **VNet**. Každý OM v Azure nasazení do virtuální sítě (VNet), který je dál rozdělit na **podsítí**.

- **Vyrovnávání zatížení Azure.** [Služba Vyrovnávání zatížení] distribuuje příchozí žádosti Internet na OM instance sady dostupnosti. Vyrovnávání zatížení obsahuje několik související materiály:

  - **Veřejnou IP adresu.** Veřejnou IP adresu: není nutná pro vyrovnávání zatížení pro příjem internetový provoz.

  - **Front-end konfigurace.** Přiřadí veřejnou IP adresu Vyrovnávání zatížení.

  - **Fond back-end adres.** Stránka obsahuje síťová rozhraní (NIC) VMs, které budou dostávat příchozí přenos.

- **Načtení vyrovnávání pravidla.** Slouží k distribuci síťová komunikace mezi všechny VMs ve fondu back-end adres. 

- **Pravidla překladu síťových adres.** Použít provoz směrovat na konkrétní OM. Například povolit vzdálené plochy protokol (RDP) VMs, vytvořte pravidlo překladu adresu samostatnou síť pro každou OM. 

- **Síťová rozhraní (NIC)**. Každý OM má NIC připojení k síti.

- **Úložiště.** Účty úložiště podržte OM obrázky a další zdroje týkající se souborů, například dat diagnostiky OM zachycených Azure.

## <a name="recommendations"></a>Doporučení

Azure nabízí mnoho různých zdrojů a typů zdrojů, takže může být tento odkaz architektura zřízení mnoha různými způsoby. Uvádíme správce prostředků Azure šablonu nainstalovat architektura odkaz, který následuje doporučení níže uvedených pokynů. Pokud budete chtít vytvořit vlastní odkaz architektura, pokud máte specifické požadavky, doporučení nepodporuje by měl těmito doporučeními. 

### <a name="availability-set-recommendations"></a>Dostupnost nastavit doporučení

Musíte vytvořit aspoň dva VMs dostupnost nastaven pro podporu [dostupnost SLA Azure VMs][vm-sla]. Nezapomeňte, že vyrovnávání zatížení Azure také vyžaduje, aby Vyrovnávání zatížení VMs patří do stejné sady dostupnost.

### <a name="network-recommendations"></a>Doporučení sítě

Všechny VMs za vyrovnávání zatížení by měly umístěny ve stejné podsítě. Nezobrazují VMs přímo k Internetu funguje, ale udělit každý OM soukromých IP adres. Pomocí veřejné adresy IP Vyrovnávání zatížení připojují klienti.

### <a name="load-balancer-recommendations"></a>Doporučení Vyrovnávání zatížení

Přidání všech VMs v dostupnosti nastavit do fondu back-end adresu Vyrovnávání zatížení.

Definování pravidla Vyrovnávání zatížení přímé v síti na VMs. Například povolit přenosy protokolu HTTP, vytvořte pravidlo, které mapuje port 80 z front-end konfigurace port 80 na fondu back-end adres. Požadavky na port 80 veřejnou IP adresu přijaté Vyrovnávání zatížení je bude směrovat žádost s portem 80 k některé z nic z fondu back-end adresu.

Definujte pravidla překladu síťových adres provoz směrovat na konkrétní OM. Například povolit vytvoření RDP VMs samostatné překladu síťových adres pravidla pro každý OM. Každé pravidlo by měl mapovat různých port číslo portu 3389, výchozí port pro RDP. (Například port 50001 určenou "VM1," port 50002 "VM2," atd.) Přiřaďte pravidla překladu síťových adres nic na VMs. 

### <a name="storage-account-recommendations"></a>Doporučení účtu úložiště

Vytvoření účtů samostatné Azure úložiště pro každou OM podržte virtuální pevných discích (VHD), a zabránit tak zasažení vstupní a výstupní operace za druhý [limity (procesorů)] [ vm-disk-limits] u účtů úložiště. 

Vytvoření účtů úložiště pro diagnostické protokoly. Tento účet úložiště smí zobrazovat všechny VMs.

## <a name="scalability-considerations"></a>Co byste měli zvážit škálovatelnost:

Rozšiřování VMs v Azure dvěma způsoby: 

- Použijte při vyrovnávání zatížení k distribuci v síti přes sadu VMs. Zobrazit vytvořit další VMs a vložit je za vyrovnávání zatížení. 

- Použití [sady měřítko virtuálního počítače][vmss]. Sada měřítko obsahuje celá řada služby Transaction identickými VMs za vyrovnávání zatížení. OM měřítko nastaví neobsahovaly text podpory metrice výkonu. Jak zvyšuje zatížení VMs další VMs se automaticky přidají do Vyrovnávání zatížení. 

V následujících částech porovnání následujících dvou možností.

### <a name="load-balancer-without-vm-scale-sets"></a>Vyrovnávání zatížení bez OM měřítko sady

Vyrovnávání zatížení zabírá příchozí žádosti sítě a distribuuje mezi nic z fondu back-end adresu. Změnit velikost ve vodorovném směru, přidat další instance OM na sadu dostupnosti (nebo zrušit VMs Neomezovat). 

Předpokládejme například, že používáte na webový server. Přidejte do pravidla Vyrovnávání zatížení port 80 a/nebo port 443 (pro SSL). Pokud klient odešle žádost HTTP, Vyrovnávání zatížení vyskladnění back-end IP adresu pomocí [algoritmus hash] [ load balancer hashing] , který bude zahrnovat Zdrojová IP adresa. Tímto způsobem žádosti klienta o rozvržena všechny VMs. 

> [AZURE.TIP] Pokud přidáte novou OM dostupné nastavit, zkontrolujte, že vytvořit NIC OM a přidat NIC do fondu back-end adresu na Vyrovnávání zatížení. V opačném internetový provoz nebudou směrovány do nového OM.

Každý Azure předplatné omezuje výchozí na místě, včetně maximální počet VMs jednotlivých oblastech. Limit můžete zvýšit používá se zařazování žádost o podporu. Další informace najdete v tématu [Azure předplatné a omezení služby, kvót a omezení][subscription-limits].  

### <a name="vm-scale-sets"></a>Sady měřítko OM 

OM měřítko sady umožňují nasazovat a spravovat sady identickými VMs. Se všemi VMs konfigurovat stejná, OM měřítko sady bez předem zřizování VMs usnadňuje vytváření rozsáhlé služby zacílení velký výpočetním velký dat a kontejnerizovaná úloh nepodporuje automatické true měřítko. 

Další informace o sadách měřítko OM, najdete v článku [Přehled Nastaví měřítko virtuálního počítače][vmss].

Důležité informace o používání sad měřítko OM:

- Pokud potřebujete rychle rozšiřování VMs nebo třeba automatické měřítko, zvažte možnost sady měřítko. 

- V současné době měřítko sady nepodporují disků data. Možnosti ukládání dat jsou úložiště Azure souborů, jednotka OS, jednotka Temp nebo externí úložiště, jako je třeba úložiště Azure. 

- Všechny instance OM v rámci měřítka, které se nastavují automaticky patří stejnou sadu dostupnost s 5 poruch domén a 5 aktualizace domén.

- Ve výchozím nastavení měřítko sady používat "overprovisioning", což znamená sadu měřítko původně ustanovení další VMs než požádat o a potom odstraní navíc VMs. To zlepšuje celkové úspěšnost, když zřizujete VMs. 

- Doporučujeme, ale ne další a pak než 20 VMs za úložiště účet s overprovisioning povolenými nebo víc než 40 VMs s overprovisioning zakázán.  

- Můžete najít šablony správce prostředků pro nasazení měřítko nastaví v [Azure rychlý úvod šablony][vmss-quickstart].

- Existují dva základní způsoby, jak konfigurovat VMs používaný v sadě měřítko: vytvoření vlastního obrázku nebo rozšíření používá ke konfiguraci OM po máte k dispozici.

    - Sada měřítko založená na vlastní obrázek musí vytvořit všechny VHD disku OS v rámci jednoho účtu úložiště. 

    - S vlastní obrázky budete muset aktualizovat obrázku.

    - Rozšíření může trvat déle nově vytvořený OM na číselník nahoru.

Další informace najdete v článku [Navrhování OM měřítko sady pro měřítko][vmss-design].

> [AZURE.TIP]  Pokud chcete použít jakékoli automatické měřítko řešení, vyzkoušejte s zatížení úroveň výroby práce dobře předem. 

## <a name="availability-considerations"></a>Důležité informace o dostupnosti

Dostupnost sada zajišťuje aplikace více pružné plánované a neplánované údržbu události.

- Pokud Microsoft aktualizuje základní platformě někdy způsobí VMs restartování dojde k _plánovaná údržba_ . Azure něco v ní se VMs sady dostupnost nejsou všechny nerestartuje ve stejnou dobu, alespoň jedno bude k dispozici spuštěn a ostatní po restartu.

- _Neplánovanou údržbu_ se stane, když dojde k selhání hardwaru. Azure zajišťuje, že jsou VMs sady dostupnost zřízení přes víc regálů serveru. Díky snížit dopad selhání hardwaru sítě výpadků, power vyrušování a tak dále.

Další informace najdete v tématu [Správa dostupnosti virtuálních počítačích][availability set]. Následující video má i užitečný přehled sady dostupnost: [Jak můžu nastavit dostupnost měřítko VMs][availability set ch9]. 

> [AZURE.WARNING]  Zkontrolujte, že konfigurace dostupnosti nastavení, když zřizujete OM. V současné době nejde žádným způsobem přidáte dostupné po zřízení OM OM správce prostředků.

Vyrovnávání zatížení pomocí [sond stavu] sledovat dostupnost OM instance. Pokud zkušební nemají přístup k instanci v rámci časový limit, Vyrovnávání zatížení ukončí odesílání přenosy této v angličtině. Však Vyrovnávání zatížení, zůstanou probe a znovu k dispozici OM, Vyrovnávání zatížení dokončí odesílání přenosy této v angličtině.

Tady je pár doporučení týkající se stavu sond Vyrovnávání zatížení:

- Sond můžete otestovat HTTP nebo TCP. Pokud VMs spuštěním serveru HTTP (IIS nginx, Node.js aplikace a tak dál), vytvořte zkušební HTTP. V opačném případě vytvořte zkušební TCP.

- Pro zkušební HTTP zadejte cestu k koncový bod HTTP. Zkušební hledá HTTP 200 odpovědi z tohoto umístění. To může být cesta ke kořenové ("/") nebo koncový bod sledování stavu, implementující některé vlastní logiky pro kontrolu stavu aplikace. Koncový bod musí povolit anonymní požadavků HTTP.

- Zkušební se pošle z [známé] [ health-probe-ip] IP adresa 168.63.129.16. Zkontrolujte, že nechcete blokovat přenosy do nebo z této IP v libovolné zásady brány firewall nebo pravidla skupiny (NSG) zabezpečení sítě.

- Použití [stavu zkušební protokoly] [ health probe log] zobrazení stavu sond stavu. Povolte protokolování Azure portálu pro každou Vyrovnávání zatížení. Aby došlo k úložišti objektů Blob Azure jsou zápisu protokoly. Protokoly na back-end zobrazily kolik VMs nechodí v síti kvůli selhalo zkušební odpovědi.

## <a name="manageability-considerations"></a>Důležité informace o možnostech správy

S více VMs z něj stal důležité k automatizaci procesů, tak, aby byly spolehlivý a jestli ho opakující. Můžete použít [Azure automatizaci] [ azure-automation] k automatizaci nasazení opravy OS a dalších úkolů. Azure automatizaci je automatizaci služba, která běží na Azure a je založený na prostředí Windows PowerShell. Příklad automatizaci skripty jsou k dispozici v [Galerii postupu Runbook] na TechNetu.

## <a name="security-considerations"></a>Otázky bezpečnosti pro

Virtuální sítě jsou hranice izolace přenosy v Azure. VMs v jedné VNet nemůžete komunikovat ani přímo do VMs v různých VNet. VMs v rámci stejného VNet komunikovat, dokud nevytvoříte [skupiny zabezpečení sítě] [ nsg] (NSGs) omezení přenosy. Další informace najdete v tématu [cloudovým službám společnosti Microsoft a zabezpečení sítě][network-security].

Pro příchozí internetový provoz pravidla Vyrovnávání zatížení definovat, který přenos zobrazíte back-end. Však pravidla Vyrovnávání zatížení nepodporují povolení programů IP, takže pokud chcete povolených určitých veřejné IP adresy, přidat NSG do podsítě.

## <a name="solution-deployment"></a>Nasazení řešení

Nasazení pro implementující těmito doporučeními architekturu odkaz je dostupný na GitHub. Tento odkaz architektura obsahuje virtuální sítě (VNet), skupiny zabezpečení síti (NSG), Vyrovnávání zatížení a dvě virtuálních počítačích (VMs).

Architektura odkaz můžete nasadit buď s Windows nebo Linux VMs podle pokynů níže: 

1. Klikněte pravým tlačítkem myši na tlačítko a vyberte buď "Otevřít odkaz na nové kartě" nebo "Otevřít odkaz v novém okně":  
[![Nasazení Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. Jakmile otevře na odkaz v portálu Azure, musíte zadat hodnoty některá nastavení: 
    - Název **pole Skupina zdroje** je definován v souboru parametrů, takže vyberte možnost **Použít existující** a zadejte `ra-multi-vm-rg` do textového pole.
    - Vyberte oblast z **umístění** rozevíracím seznamu.
    - Úprava **Šablony kořenové Uri** nebo **Parametr kořenové Uri** textová pole.
    - Vyberte **Typ Os** v rozevíracím poli, **windows** a **linux**. 
    - Zkontrolujte podmínky a ujednání a potom klepnutím na zaškrtávací políčko **že můžu souhlasíte s podmínkami a ujednáními uvedené výše** .
    - Klikněte na tlačítko **koupit** .

3. Počkejte nasazení dokončete.

4. Soubory parametrů obsahuje pevně správce uživatelské jméno a heslo, a doporučuje okamžitě změnit obojí. Klikněte na OM s názvem `ra-multi-vm1` Azure portálu. Klikněte na **Resetovat heslo** v zásuvné **podpory + Poradce při potížích** . Vyberte **Resetovat heslo** do pole rozevíracího seznamu **režim** a potom vyberte nové **uživatelské jméno** a **heslo**. Klikněte na tlačítko **Aktualizovat** ukládat nové uživatelské jméno a heslo. Opakujte u OM s názvem `ra-multi-vm2`.

Informace o dalších způsobech nasazení tento odkaz architektura najdete v tématu v souboru readme v [pokyny jednoduchým OM] [ github-folder] GitHub složky. 

## <a name="next-steps"></a>Další kroky

Uvedení několik VMs za vyrovnávání zatížení je stavební blok pro vytvoření vícevrstvého architektury. Další informace najdete v tématu [Spuštění Windows VMs N-vrstvy architektury na Azure] [ n-tier-windows] a [Systémem Linux VMs N-vrstvy architektury na Azure][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[Stav sond]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[Vyrovnávání zatížení]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Galerie postupu Runbook]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "Architektury řešení s víc OM na Azure zahrnující dostupné sada se dvěma VMs a vyrovnávání zatížení"
