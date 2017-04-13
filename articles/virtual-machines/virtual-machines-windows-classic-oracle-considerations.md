<properties
pageTitle="Důležité informace týkající se používání obrázků Oracle OM | Microsoft Azure"
description="Informace o podporovaných konfigurací a omezení pro Oracle OM v systému Windows Server v Azure před nasazením."
services="virtual-machines-windows"
documentationCenter=""
manager="timlt"
authors="rickstercdn"
tags="azure-service-management"/>

<tags
ms.service="virtual-machines-windows"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="vm-windows"
ms.workload="infrastructure-services"
ms.date="09/06/2016"
ms.author="rclaus" />

#<a name="miscellaneous-considerations-for-oracle-virtual-machine-images"></a>Různé aspekty pro obrázky s virtuálního počítače Oracle



Tento článek popisuje, co byste měli zvážit pro Oracle virtuálních počítačích v Azure, které jsou založeny na obrázky software Oracle poskytovanou Oracle.  

-  Obrázky virtuálního počítače databáze Oracle
-  Obrázky virtuálního počítače WebLogic Server Oracle
-  Oracle JDK virtuálního počítače obrázky

##<a name="oracle-database-virtual-machine-images"></a>Obrázky virtuálního počítače databáze Oracle
### <a name="clustering-rac-is-not-supported"></a>Není podporovaná clusterů (certifikátů)

Azure aktuálně nepodporuje Oracle skutečné aplikace clusterů (certifikátů) databáze Oracle. Je možné jenom samostatného instancí databáze Oracle. Je to proto Azure aktuálně nepodporuje virtuální sdílení disku způsobem pro čtení i zápis mezi několika instancích spuštěných virtuálního počítače. Nepodporuje také vícesměrové UDP.

### <a name="no-static-internal-ip"></a>Žádné statické IP interní

Azure přiřadí každý virtuální počítač interní IP adresu. Pokud virtuální počítač není součástí virtuální sítě, IP adresu virtuálního počítače je dynamické a po restartování virtuálního počítače se může změnit. Protože databáze Oracle očekává statické IP adresu to může způsobit problémy. Chcete-li předejít problém, můžete do ní přidat virtuálního počítače k síti virtuální Azure. Další informace najdete v článku [Virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) a [vytvořit virtuální sítě v Azure](../virtual-network/virtual-networks-create-vnet-arm-pportal.md) .

### <a name="attached-disk-configuration-options"></a>Možnosti konfigurace připojený disk

Datové soubory můžete umístit na disk operačního systému virtuální počítač nebo na připojených disků, nazývaný také dat discích. Připojených disků může nabídnout lepší výkon a velikost flexibilitu než disk operačního systému. Disk operačního systému může být vhodnější jenom u databází ve skupinovém rámečku 10 gigabajtů (GB).

Připojených disků spolehnout služba úložiště objektů Blob Azure. Každý disk je schopen teoretický maximálně přibližně 500 vstupní a výstupní operace sekundu (procesorů). Výkon připojených disků nemusí být optimální původně a po určité době "vypálit se změnami" přibližně 60 90 minut nepřetržitý operace může výrazně zlepšení výkonu procesorů. Pokud disk následně nečinný, může do jiného vypálit doby nepřetržitý operace zmírnit procesorů výkonu. Stručně řečeno další aktivní a disku, více pravděpodobně je optimální výkon procesorů přístup.

I když je nejjednodušší připojit jeden disk do virtuálního počítače a ukládat soubory databáze na disku, tento přístup je také nejčastěji omezující z hlediska výkonu. Pokud používáte více připojených disků, rozšířit dat z databáze mezi nimi a potom použijte Oracle automatické ukládání Management (ASM) místo toho můžete vylepšit často efektivní procesorů výkonu. Další informace najdete v tématu [Přehled Oracle automatické ukládání](http://www.oracle.com/technetwork/database/index-100339.html) . Přestože je možné použít prokládání více disků na úrovni operační systém, tento přístup se nedoporučuje, protože není známo, aby se zvýšil výkon procesorů.

Vezměte v úvahu dva různé přístupy pro připojení více disků založené na tom, jestli chcete nastavit jejich priority výkon operace čtení nebo do nich psát operace pro databázi:

- **Oracle ASM na samostatném** je nejspíš za následek lepší výkon operace zapsat, ale nejhorší procesorů pro operace čtení ve srovnání s přístup pomocí disková pole. Následující obrázek znázorňuje logicky tohoto ujednání.  
    ![](media/virtual-machines-windows-classic-oracle-considerations/image2.png)

>[AZURE.IMPORTANT] Vyhodnocení poměr zápisu výkon a výkon operace čtení na jednotlivé případy. Pokud použijete tuto se může lišit skutečné výsledky.

### <a name="high-availability-and-disaster-recovery-considerations"></a>Vysoký dostupnost a havárie obnovení co byste měli zvážit

Při použití databáze Oracle v Azure virtuálních počítačích, jste zodpovědní pro provádění vysoké dostupnosti a havárie obnovení řešení blokovat všechny výpadek služeb. Zodpovídáte taky pro zálohování dat a aplikace.

Vysoké dostupnosti a havárie obnovení Oracle databáze Enterprise Edition (bez certifikátů) v Azure lze dosáhnout pomocí dvou databází ve dvou samostatných virtuálních počítačích [stráž dat aktivní stráž dat](http://www.oracle.com/technetwork/articles/oem/dataguardoverview-083155.html)nebo [Oracle Golden bránu](http://www.oracle.com/technetwork/middleware/goldengate). Obě virtuálních počítačích by měl být na stejnou [Cloudová služba](virtual-machines-linux-classic-connect-vms.md) a stejné [virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) zajistit, že se můžou dostat k sobě přes soukromé trvalý IP adresu.  Kromě toho doporučujeme, abyste umístěte virtuálních počítačích stejné [Nastavení dostupnost](virtual-machines-windows-manage-availability.md) umožňují Azure, že je umístíte do samostatných poruch domén a upgrade domény. Pouze virtuálních počítačích ve stejném cloudové služby může účastnit stejnou sadu dostupnosti. Každý počítač virtuální musíte mít alespoň na úrovni 2 GB paměti a 5 místa na disku.

S stráž dat Oracle dostupnost můžete dosáhnout s hlavní databází ve jednoho virtuálního počítače, sekundární (úsporném) databáze v jiném počítači virtuální a jednosměrné replikace nastavení mezi nimi. Výsledek je přístup pro čtení na kopii databáze. S Oracle GoldenGate můžete nakonfigurovat obousměrné replikace mezi dvěma databází. Informace o nastavení vysoké dostupnosti řešení pro databáze pomocí těchto nástrojů, najdete v článku [Aktivní stráž dat](http://www.oracle.com/technetwork/database/features/availability/data-guard-documentation-152848.html) a [GoldenGate](http://docs.oracle.com/goldengate/1212/gg-winux/index.html) přečtěte následující dokumentaci webu Oracle. Pokud budete potřebovat pro čtení i zápis přístup na kopii databáze, můžete použít [Aktivní stráž dat Oracle](http://www.oracle.com/uk/products/database/options/active-data-guard/overview/index.html).

##<a name="oracle-weblogic-server-virtual-machine-images"></a>Obrázky virtuálního počítače WebLogic Server Oracle

-  **Clusterů podporuje Enterprise Edition pouze.** Mají licenci k používání WebLogic clusterů jenom při použití Enterprise Edition WebLogic serveru. Nepoužívejte clusteru WebLogic Server Standard Edition.

-  **Časové limity připojení:** Pokud aplikace závisí na připojení k veřejné koncové body jiné Azure cloudové službě (například služby osy databáze), mohou Azure zavřete tyto odpojený po čtyři minutách nečinnosti. Může to mít vliv funkcí a aplikací na sdružení připojení, protože připojení, která jsou neaktivní pro více toto omezení zůstaly už není platná. Pokud to blokování automaticky otevíraných aplikace, zvažte povolení "udržování" logiky na sdružení připojení.

    Pokud koncový bod je *vnitřní* nasazení služby Azure cloud (například samostatný počítač virtuální databáze ve *stejném* cloudovou službu jako WebLogic virtuálních počítačích), potom připojení je přímé nevyužívají Vyrovnávání zatížení Azure a tedy nepodléhá časový limit připojení.

-  **UDP multicast nepodporuje.** Která UDP Azure podporuje, ale ne vysílání nebo vysílání. WebLogic Server je moct jsou závislé na možnostech unicast Azure UDP. Pro nejlepší výsledky na UDP unicast, doporučujeme velikost obrázku WebLogic nacházet statické, nebo nacházet s více než 10 spravované servery součástí clusteru.

-  **WebLogic Server očekává veřejných a privátních porty shodovat T3 přístupu (například při použití JavaBeans organizace).** Zvažte scénáři vícevrstvého, kde je aplikace služby vrstvy (EJB) výpočetnímu clusteru WebLogic serveru složený ze dvou nebo více spravovaných serverů v cloudové službě s názvem **SLWLS**. Na úrovni klienta je v jiné cloudové službě aplikaci jednoduché Java voláním EJB ve vrstvě služby. Protože je nutné vrstvy služby Vyrovnávání zatížení, veřejné koncový bod Vyrovnávání zatížení musí vytvořit virtuálních počítačích v clusteru WebLogic serveru. Pokud privátní číslo portu zadané pro tento se liší od veřejné port (například 7006:7008), zobrazí následující chyba:

        [java] javax.naming.CommunicationException [Root exception is java.net.ConnectException: t3://example.cloudapp.net:7006:

        Bootstrap to: example.cloudapp.net/138.91.142.178:7006' over: 't3' got an error or timed out]

    Je to proto pro všechny vzdáleného přístupu T3 WebLogic serveru očekává port Vyrovnávání zatížení a port serveru WebLogic spravovaných stejný. Ve výše uvedeném případě klienta přístupu port 7006 (port Vyrovnávání zatížení) a je spravovaný server přijímá na 7008 (soukromé port). Toto omezení platí pouze pro T3 přístup, ne HTTP.

    Pokud chcete tomuhle problému předejít, použijte jeden z následujících postupů:

    -  Použijte stejný čísla portů soukromé a veřejné pro koncové body rozloženy snaží T3 přístup.

    -  Patří následující parametr JVM při spuštění WebLogic serveru:

            -Dweblogic.rjvm.enableprotocolswitch=true

Související informace najdete v článku znalostní bázi Knowledge Base článek **860340.1** na <http://support.oracle.com>.

-  **Dynamické clusterů a vyrovnávání zatížení omezení.** Předpokládejme, že budete chtít používat dynamické obrázku v WebLogic serveru a vystavit pomocí jediné, veřejné Vyrovnávání zatížení koncového bodu v Azure. Lze to provést dlouhou, jak používat několik pevné port pro všechny spravované servery (přiřazeno není dynamicky z oblasti) a nespouštět více spravovaných serverů než počítačích sledování správce (to znamená víc než jednu spravovaných server na virtuálních počítačů). Pokud je konfiguraci výsledkem další servery WebLogic spouštěn než virtuálních počítačích (to znamená, kde více instancí WebLogic serveru sdílet stejném počítači virtuální), a pak ho není možné víc než jednu z těchto instancí servery WebLogic serveru se vytvořit vazbu s dané číslo portu – ostatní členové této virtuálního počítače se nezdaří.

    Na druhé straně Pokud konfigurace serveru pro správu automaticky přiřadit jedinečné číslo spravovaných serverů Vyrovnávání zatížení není možné protože Azure podporuje mapování z jednoho veřejné port více soukromé porty, jako by být nutné pro tuto konfiguraci.

-  **Více instancí Weblogic Server na virtuálních počítač.** V závislosti na vaší nasazení požadavky zvažte možnost spuštění více instancí WebLogic serveru ve stejném počítači virtuální, pokud virtuálního počítače je dostatečně velký k tomu. Například na počítač virtuální středně velké, který obsahuje dvě jádra, může můžete spustit dvě instance WebLogic serveru. Ale Všimněte si, že pořád Doporučujeme vyhnout se zavedení jednoho možnostem selhání do architekturu, která bude v případě, když jste použili jediným virtuálního počítače, na kterém běží několika instancích WebLogic serveru. Použití aspoň dva virtuálních počítačích může být lepší přístup a každý z těchto virtuálních počítačích může spusťte několika instancích WebLogic serveru. Každá z těchto případů WebLogic serveru stále nemusí být součástí stejného obrázku. Osobní zprávu, ale je momentálně není možné použít Azure Vyrovnávání zatížení koncové body, které jsou zveřejněné příslušným takové nasazení serveru WebLogic ve stejném počítači virtuální, protože Vyrovnávání zatížení Azure vyžaduje vyrovnávání zatížení servery, které rozdělí mezi jedinečné virtuálních počítačích.

##<a name="oracle-jdk-virtual-machine-images"></a>Oracle JDK virtuálního počítače obrázky

-  **JDK 6 a 7 nejnovější aktualizace.** Během doporučujeme používat nejnovější veřejné, podporovanou verzi Java (aktuálně jazyka Java 8) Azure taky zpřístupníte JDK 6 a 7 obrázky. To je určená pro starší verze aplikace, které nejsou zatím připravená k upgradu na JDK 8. Během aktualizace předchozí JDK obrázky mohou být už není k dispozici veřejnosti, zadaných partnerství Microsoft s Oracle, JDK 6 a 7 obrázky poskytovanou Azure mají obsahovat novější není veřejné aktualizace poskytovaná běžným způsobem tak, že Oracle jenom vybranou skupinu podporované zákazníci Oracle. Nové verze obrázků JDK budou dostupné průběhu času s aktualizovanou verzích JDK 6 a 7.

    Dostupné v tomto JDK 6 a 7 obrázky virtuálních počítačích a obrázky odvozeno z nich JDK lze použít pouze v rámci Azure.

-  **64-bit JDK.** Oracle WebLogic Server virtuálního počítače obrázky a obrázky virtuálního počítače Oracle JDK poskytovanou Azure obsahovat 64bitové verze systému Windows Server a JDK.

##<a name="additional-resources"></a>Další zdroje informací
[Oracle virtuálního počítače obrázky Azure](virtual-machines-linux-classic-oracle-images.md)
