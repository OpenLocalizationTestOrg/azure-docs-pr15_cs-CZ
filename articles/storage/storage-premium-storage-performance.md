<properties
    pageTitle="Azure Premium úložiště: Návrh výkon | Microsoft Azure"
    description="Navrhněte výkonné aplikací pomocí Azure Premium úložiště. Úložiště Premium nabízí podpora výkonných maximum, minimum latence disku můžu/O-značnou pracovní vytížení na virtuálních počítačích Azure."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn" />

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>

# <a name="azure-premium-storage-design-for-high-performance"></a>Azure Premium úložiště: Návrh vysoký výkon

## <a name="overview"></a>Základní informace  
Tento článek obsahuje pokyny k vytváření výkonných aplikací pomocí Azure Premium úložiště. Můžete pomocí pokynů uvedených v tomto dokumentu s výkonem doporučené postupy pro technologie používané aplikace. Ke znázornění pokyny, jsme použili SQL Server spuštěna skladování Premium jako příklad v tomto dokumentu.

Během jsme řešení výkonu scénářů vrstvy úložiště v tomto článku, musíte optimalizace vrstvě aplikací. Například pokud jste hostitelem farmě služby SharePoint na Azure Premium úložiště, můžete SQL Server příklady v tomto článku optimalizovat databázovém serveru. Optimalizace navíc webový server Sharepointovou farmou a aplikační server získat většina výkonu.

Tento článek vám pomůže odpovědí sledovat běžné otázky o optimalizaci výkonu aplikace v úložišti Premium Azure

-   Jak se dá měření výkonu aplikace?  
-   Proč nejsou zobrazené očekávané vysoký výkon?  
-   Faktory, které ovlivnit výkon aplikace úložný prostor Premium?  
-   Jak následujících skutečností ovlivnit výkon aplikace úložný prostor Premium?  
-   Jak lze abyste optimalizovali procesorů, šířka pásma a latence?  

Uvádíme tyto pokyny pro Premium úložiště, protože pracovního vytížení spuštěna skladování Premium jsou vysoce citlivé výkonu. V případě potřeby uvádíme příklady. Můžete také použít některé z těchto pokynů aplikacím na IaaS VMs s disků standardní úložiště.

Než začnete, pokud začínáte k základnímu úložišti Premium, nejprve si přečtěte téma [Premium úložiště: výkonné úložiště pro Azure virtuálního počítače úloh](storage-premium-storage.md) článek a [škálovatelnost úložiště Premium Azure a výkonu cílů](storage-scalability-targets.md#premium-storage-accounts).

## <a name="application-performance-indicators"></a>Ukazatele výkonu aplikace  
Vyhodnocení zda aplikace funguje dobře nebo ne pomocí zamlouvá ukazatele výkonu, jak rychle aplikace je zpracování požadavku na uživatele, množství zpracovávaných dat aplikace zpracování na žádost o, kolik požadavky je zpracování aplikací v určitém období dobu, jak dlouho má uživatel čekat na získání odpověď po odeslání jejich žádosti. Technické termíny pro těchto ukazatelů výkonu jsou procesorů, výkon nebo šířka pásma a latence.

V této části probereme běžné ukazatele výkonu v rámci Premium úložiště. V části následující požadavky aplikace shromažďování se dozvíte, jak změřit těchto ukazatelů výkonu aplikace. Dále v optimalizaci výkonu aplikace se dozvíte o faktory, které ovlivňují těchto ukazatelů výkonu a doporučení pro optimalizaci nimi.

## <a name="iops"></a>PROCESORŮ  
Procesorů je zadané číslo požadavků, které aplikace odesílá disků úložiště v jedné sekundy. Vstupní a výstupní operace může číst nebo psát, sekvenční nebo náhodné. OLTP aplikace, jako je online maloobchod webu nutné zpracovat žádosti o mnoha souběžné uživatele ihned. Jsou vložení dotazů koncových uživatelů a aktualizovat náročné databázové transakce, které musí rychle aplikace. Proto OLTP vyžadují, velmi vysoké procesorů. Tyto aplikace zpracovat miliony požadavků na malých a náhodné vstupů/výstupů. Pokud máte aplikací, je třeba navrhnout aplikační infrastruktury optimalizovat pro procesorů. V části novější *Optimalizaci výkonu aplikace*probereme tato podrobně všechny faktory, které je třeba uvážit získat vysoké procesorů.

Pokud připojíte premium úložiště disk na vysoké měřítko OM, Azure ustanovení za vás zaručené počet procesorů podle specifikaci disku. Například P30 disk ustanovení 5000 procesorů. Každý vysoké měřítko OM velikost má i konkrétní limit procesorů, který můžete udržovat. Standardní OM GS5 má například 80 000 procesorů omezit.

## <a name="throughput"></a>Výkon  
Výkon nebo šířku pásma Internetu je množství dat, které aplikace odesílá disků úložiště v určeného časového intervalu. Pokud aplikace provádí vstupní a výstupní operace s velké velikosti vstupu a výstupu jednotku, vyžaduje vysoký výkon. Datový sklad aplikací často o vystavení prohledávání náročné přístup k větší části dat v čase a operací běžně provádí operace hromadně. Jinými slovy například vyžadují vyšší výkon. Pokud máte aplikací, je třeba navrhnout svoji infrastrukturu optimalizovat výkon. V následující části probereme tato podrobně faktory musí ladění docílit.

Pokud připojíte premium úložiště disk na vysoké měřítko OM, Azure ustanovení výkon podle tohoto disku specifikace. Například P30 disk ustanovení 200 MB za druhý disk výkon. Každý vysoké měřítko OM velikost má také jako konkrétní výkon, což maximální počet můžete udržovat. Například standardní OM GS5 obsahuje maximální výkon 2 000 MB sekundu.

Existuje vztah mezi výkonu a procesorů, jak je znázorněno níže uvedeného vzorce.

![](media/storage-premium-storage-performance/image1.png)

Proto je důležité určit optimální výkon procesorů hodnoty a vyžadovaných aplikací. Při pokusu o jednu optimalizace druhý taky získá vliv. V další části, *Optimalizaci výkonu aplikace*, bude probereme tato témata v další informace o optimalizaci procesorů a výkon.

## <a name="latency"></a>Latence  
Latence je údaj o čase trvá aplikace přijmout žádost o jednu, je předávat disků úložiště a odpověď odesílala klienta. Toto je důležité měření výkonu aplikace kromě procesorů a výkon. Latence disk premium úložiště je dobu potřebnou k načtení informací o žádost a komunikaci zpátky do okna aplikace. Úložiště Premium poskytuje konzistentní zhoršeným čekacích dob. Pokud povolíte ukládání do mezipaměti na premium úložiště discích hostitele jen pro čtení, získáte mnohem nižší čtení latence. Bude probereme tato diskové mezipaměti podrobněji v další části na *Optimalizaci výkonu aplikace*.

Když jsou optimalizace aplikace zobrazíte vyšší procesorů a výkon, bude to mít vliv latence aplikace. Po ladění výkonu aplikace, vždycky vyhodnocení latence aplikace, chcete-li předejít neočekávané vysokou latencí chování.

## <a name="gather-application-performance-requirements"></a>Shromáždění požadavky na výkon aplikace  
Cílem prvního kroku v návrhu vysoký výkon spuštěné v úložišti Premium Azure je znát požadavky na výkon aplikace. Po shromáždit funkční požadavky, abyste optimalizovali aplikace dosáhnout nejčastěji optimální výkon.

V předchozí části jsme vysvětlení společných ukazatelů výkonu procesorů, výkon a latence. Je nutné určit, které z těchto ukazatelů výkonu je považován za kritický aplikaci předvádění požadované uživatelského prostředí. Například vysoké procesorů nejdůležitějším informacím aplikacím OLTP zpracování miliony transakce do druhého. Vzhledem k tomu vysoký výkon kritické datový sklad aplikací, zpracování velkých objemů dat do druhého. Velmi nízký latence je důležité pro aplikace v reálném čase jako živé video streamování weby.

Pak změřte maximální výkon požadavky aplikace v celém životnosti. Kontrolní seznam ukázkové pod použijte jako začátek. Záznam požadavky maximální výkonu během normální, Špička a která pracovní zátěž obdobími. Díky identifikaci požadavky pro všechny úrovně zatížení, budou moct zjistit celkový výkon požadavku aplikace. Například normální pracovní zátěž obchodování webu budou transakce, které slouží během většina dnů v roce. Pole Špička pracovní zátěž webu budou transakce, které slouží během svátcích nebo jinak prodej události. Pracovní zátěž Špička je obvykle zkušených po omezenou dobu, ale můžete požádat aplikace zobrazit dva nebo víc času normální operace. Zjistěte, 50 procent, 90 percentilu a 99 percentilu požadavky. Díky odfiltrovat odlehlé všechny hodnoty v požadavky na výkon a se můžete zaměřit na optimalizace pro nesprávné hodnoty.

**Kontrolní seznam požadavků výkonu aplikace**

| **Požadavky na výkon** | **50 procent** | **90 percentil** | **99 percentil** |
|---|---|---|---|
| Max. Transakce za sekundu | | | |
| Operace čtení %            | | | |
| Operace zápisu %           | | | |
| Náhodné operace %          | | | |
| Sekvenční operace %      | | | |
| Velikost žádost o vstupů/výstupů              | | | |
| Průměrná výkon           | | | |
| Max. Výkon              | | | |
| Min. Latence                 | | | |
| Průměrnou latenci              | | | |
| Max. PROCESOR                     | | | |
| Průměr CPU                  | | | |
| Max. Paměti                  | | | |
| Průměrná paměti               | | | |
| Hloubka fronty                  | | | |

>**Poznámka:**  
>Měli byste zvážit měřítka číselným podle očekávaných budoucí růst aplikace. Je dobré plán pro růst předem, protože může to být víc. Chcete-li změnit infrastrukturu pro zvýšení výkonu později.

Pokud máte existující aplikace a chcete přesunout k základnímu úložišti Premium, nejdřív vytvořte kontrolní seznam nad existující aplikace. Potom vytváření prototypů aplikace na Premium úložiště a návrhu aplikace podle pokynů podle *Optimalizaci výkonu aplikace* v další části tohoto dokumentu. Následující části popisuje nástroje, které můžete použít k získání měření výkonu.

Vytvoření kontrolního seznamu podobný aplikace pro typ.. Pomocí nástroje Benchmarking můžete simulovat pracovního vytížení a měření výkonu v aplikaci prototypů. Na [Benchmarking](#benchmarking) Další informace naleznete v části. Provedením, můžete určit, zda Premium úložiště můžete odpovídat nebo překročí vašim požadavkům výkonu aplikace. Pak můžete používat stejný pokyny výrobní aplikace.

### <a name="counters-to-measure-application-performance-requirements"></a>Čítače měření výkonu požadavky aplikace  
Nejlepší způsob, jak měřit požadavky na výkon aplikace, je použití nástroje pro sledování výkonu poskytovanou operačního systému serveru. Můžete PerfMon pro Windows a iostat Linux. Tyto nástroje zachytit čítače odpovídající každé opatření vysvětlení v oddílu výše. Hodnoty tyto čítače musí zachytit při spuštění aplikace jeho normální, Špička a která úloh.

Čítačů výkonu jsou k dispozici pro procesor paměti a logické disku a každého fyzického disku serveru. Při používání disků úložiště premium se virtuálního počítače fyzické čítače pro každý premium úložiště disk a logické čítače pro každou vytvořil disků úložiště premium. Je nutné zachytit hodnoty disků hostujících pracovní zátěž aplikace. Pokud je na jednom mapování mezi logické a fyzické disků, můžete vytvořit odkaz fyzické čítače; v opačném případě v nápovědě k logické čítače. Na Linux příkaz iostat generuje sestavy využití procesoru a místo na disku. Sestava využití disku obsahuje statistiky za oddíl nebo pole fyzicky zařízení. Pokud máte databázový server s jeho dat a protokolů na samostatném discích, tato data pro obě disků shromažďovat. Pod tabulkou popisuje čítače disků, procesoru a paměti:

| Čítač | Popis | PerfMon | Iostat |
|---|---|---|---|
| **Procesorů nebo transakce za sekundu** | Počet požadavky v/v znalostní báze úložiště sekundu. | Čtení disku/s <br> Zápisy na disk/s | TPS <br> r/s <br> m/s |
| **Disk čtení a zápis** | % čtení a zápis operací na disku. | % Času čtení disku <br> Čas zápisu disku % | r/s <br> m/s |
| **Výkon** | Množství dat přečíst ze nebo na disk sekundu. | Čtení bajty/s disku <br> Zápis disku bajtů/sec | kB_read/s <br> kB_wrtn/s |
| **Latence** | Celkový čas na dokončení zadání vstupu a výstupu disku. | Průměrná disku sec/přečtené <br> Průměrná disku sec i zápis | očekávat <br> svctm |
| **Velikost vstupů/výstupů** | Velikost vstupu a výstupu žádosti věcí disků úložiště. | Průměrná disku bajtů/přečtené <br> Průměrná disku bajtů i zápis | avgrq sz. |
| **Hloubka fronty** | Počet vstupu a zbývající výstupu žádostí o čekání číst formuláře nebo zapisovat na disk úložiště. | Aktuální délka fronty disku | avgqu sz. |
| **Max. Paměti** | Velikost paměti potřebné ke spuštění aplikace hladce | % Potvrzeného bajtů používán | Použití vmstat |
| **Max. PROCESOR** | Částka procesoru potřebné ke spuštění aplikace hladce | Čas procesoru % | % util |

Další informace o [iostat](http://linuxcommand.org/man_pages/iostat1.html) a [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).


## <a name="optimizing-application-performance"></a>Optimalizace výkonu aplikace  
Hlavní faktory, které ovlivňují výkonu z aplikace spuštěna skladování Premium jsou přírodní z/v žádosti o, OM velikost písma, velikosti disku, počet disků diskové mezipaměti Multithreading a hloubka fronty. Některé z následujících skutečností můžete ovládat pomocí knoflíky poskytovanou systému. Možnost změnit velikost vstupů/výstupů a hloubka fronty přímo nemusí být většiny aplikací. Například pokud používáte SQL serveru, nemůžete si vybrat vstupu a výstupu velikost a fronty hloubku. SQL Server zvolí optimální vstupu a výstupu velikost fronty název hloubkové hodnoty a získat většina výkonu. Je důležité pochopit efekty obou typů koeficientů na výkon aplikace tak, aby zřizujete vhodné prostředky potřebám výkonu.

V této části naleznete kontrolní seznam požadavků aplikace, kterou jste vytvořili, jak identifikovat, kolik potřebujete pro optimalizaci výkonu aplikace. Založeno na tomto, bude moct zjistit faktory, které v této části budete muset ladění. Účastnit efekty každý faktor na výkon aplikace, spouštět benchmarking nástrojů na nastavení aplikace. Přečtěte si část [Benchmarking](#Benchmarking) na konci tohoto článku postup používat běžné benchmarking nástroje v systémech Windows a Linux VMs.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimalizace procesorů, výkon a latence – rychlý přehled  
Následující tabulka obsahuje souhrn všech faktory výkonu a postup optimalizovat procesorů, výkon a latence. V částech sledovat tento souhrn bude popsány jednotlivé faktor je název hloubkové mnohem víc.

| | **PROCESORŮ** | **Výkon** | **Latence** |
|---|---|---|---|
| **Příklad scénáře** | Aplikace Enterprise OLTP vyžadující velmi vysoké transakce za druhý sazba.                                                                                                                                 | Data organizace skladování aplikace zpracování velkých objemů dat | U aplikace v reálném čase vyžadující rychlé odpovědi na žádosti o uživatele, jako je online herní. |
| Faktory výkonu  | | | |
| **Velikost vstupů/výstupů** | Menší velikost vstupů/výstupů dává vyšší procesorů.                                                                                                                                                                           | Větší velikost vstupů/výstupů k dává vyšší výkon. | |
| **Velikost OM** | Použijte velikost OM, která nabízí procesorů větší než požadovanou aplikaci. Přečtěte si téma OM velikosti a jejich procesorů limity. | Pomocí OM velikost výkon limit větší než požadovanou aplikaci. Přečtěte si téma OM velikosti a jejich výkon limity. | Použijte velikost OM nabídky Velikost mezní hodnoty větší než požadovanou aplikaci. Přečtěte si téma OM velikosti a jejich omezení. |
| **Velikost disku** | Použijte velikost disku, který nabízí procesorů větší než požadovanou aplikaci. V tématu velikosti disku a jejich procesorů limity. | Pomocí disku velikost výkon limit větší než požadovanou aplikaci. V tématu velikosti disku a jejich výkon limity. | Použijte velikost disku nabídky Velikost mezní hodnoty větší než požadovanou aplikaci. V tématu velikosti disku a jejich omezení. |
| **OM a omezení rozsahu disku** | Omezení počtu procesorů velikosti OM zvolili musí být větší než celkové procesorů úsilím disků úložiště premium připojené k němu. | Výkon limit velikosti OM zvolili by měla být větší než celkový výkon úsilím disků úložiště premium připojené k němu. | Měřítko limity velikosti OM zvolili musí být větší než celkové měřítko limity úložiště disků připojených prémií. |
| **Diskové mezipaměti** | Povolte mezipaměť jen pro čtení na premium úložiště discích s hodně operace čtení získat vyšší procesorů pro čtení. | | Povolte mezipaměť jen pro čtení na premium úložiště discích s připravení hodně operacemi získat velmi nízký čtení čekacích dob. |
| **Prokládání disku** | Použití několika disků a prokládanou je získat kombinované vyšší procesorů a výkon mezní hodnoty. Všimněte si, že kombinované limit na OM by měla být vyšší než kombinované limity připojených prémií discích. | |
| **Velikost pruhu** | Menší velikost prokládání náhodné malé vstupu a výstupu vzorek v aplikacích OLTP zobrazovat. Například pro aplikaci SQL Server OLTP použijte velikost prokládání 64 kB. | Větší velikost prokládání sekvenční velké vstupu a výstupu vzorek v aplikacích datový sklad zobrazovat. Například pomocí velikost prokládání 256KB pro SQL Server Data warehouse aplikace. | |
| **Multithreading** | Použití multithreading nabízená vyšší počet požadavků k základnímu úložišti Premium, vede k vyšší procesorů a výkon. Například na serveru SQL Server nastavte hodnotu Vysoká MAXDOP přidělit více procesorů SQL Server. | |
| **Hloubka fronty** | Větší hloubka fronty dává vyšší procesorů. | Větší hloubka fronty dává vyšší výkon. | Menší hloubka fronty dává nižší čekacích dob. |

## <a name="nature-of-io-requests"></a>Povahy požadavků na vstupu a výstupu  
Žádost o konverzaci vstupu a výstupu je jednotka vstupní a výstupní operace, které budete provádět aplikace. Identifikace přírodní požadavků na vstupu a výstupu, náhodné nebo postupné, pro čtení a zápis, malý nebo velký, vám pomůže určit požadavky na výkon aplikace. Je důležité pochopit přírodní žádosti o vstupu a výstupu, aby správná rozhodnutí při návrhu infrastrukturu aplikace.

Velikost vstupů/výstupů je jedním z důležitější faktorů. Počet vstupů/výstupů je velikost žádost o vstupní a výstupní operace generováno aplikací. Velikost vstupů/výstupů má významný dopad na výkon zejména na procesorů a šířky pásma, který může aplikace dosáhnout. Následující vzorec ukazuje vztah mezi procesorů, velikost vstupů/výstupů a šířky pásma/výkon.  
    ![](media/storage-premium-storage-performance/image1.png)

Některé aplikace umožňuje změnit jejich velikost vstupů/výstupů během některé aplikace nepodporují. Třeba SQL Server určuje optimální velikost vstupů/výstupů samotné a není uživatelům poskytnout všechny knoflíky ho změnit. Na druhé straně Oracle poskytuje parametr s názvem [DB\_BLOKOVAT\_velikost](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) pomocí které můžete konfigurovat vstupu a výstupu žádost o velikosti databáze.

Pokud používáte aplikaci, kterou nelze změnit velikost vstupů/výstupů, řiďte se pokyny v tomto článku k optimalizovat výkon klíčový ukazatel výkonu nejrelevantnější aplikaci. Například

-   Aplikace OLTP vygeneruje miliony požadavků na malých a náhodné vstupů/výstupů. Ke zpracování tyto typu vstupu a výstupu požadavky, je nutné vytvořit infrastrukturu aplikace získat vyšší procesorů.  
-   Datové skladování aplikace vygeneruje velkých a postupné žádosti o vstupu a výstupu. Ke zpracování tyto typu vstupu a výstupu požadavky, je nutné vytvořit infrastrukturu aplikace získat větší šířku pásma nebo výkon.

Pokud používáte aplikaci, která umožňuje změnit velikost vstupů/výstupů, použijte toto pravidlo pro velikost vstupů/výstupů kromě další pokyny výkonu

-   Menší velikost vstupů/výstupů získat vyšší procesorů. Příklad 8 znalostní bázi Knowledge Base OLTP aplikace.  
-   Větší velikost vstupů/výstupů získat vyšší šířky pásma/výkon. Příklad 1 024 znalostní bázi Knowledge Base datový sklad aplikace.

Tady je příklad na způsob výpočtu procesorů a výkon/šířky pásma pro aplikaci. Zvažte aplikace přes P30 disku. Maximální hodnota, kterou můžete dosáhnout procesorů a výkon P30 disk šířky pásma je 5000 procesorů a 200 MB sekundu v tomto pořadí. Teď Pokud vaše aplikace vyžaduje maximální procesorů z disku P30 a používáte zmenšené vstupu a výstupu jako 8 KB, výsledné šířka pásma bude moct získat je 40 MB sekundu. Ale pokud aplikace vyžaduje maximální výkon/šířky pásma z disku P30 a používáte větší velikost vstupů/výstupů jako 1 024 KB, výsledný procesorů budou menší, 200 procesorů. Proto ladění velikost vstupů/výstupů tak, aby ho musí vyhovovat požadavkům procesorů nebo výkon/pásma obou aplikace. Tabulka obsahuje souhrn různě vstupu a výstupu a jejich odpovídající procesorů a výkon P30 disku.

| **Aplikaci požadavkem** | **Velikost vstupu a výstupu** | **PROCESORŮ** | **Výkon a šířky pásma** |
|-----------------------------|--------------|----------|--------------------------|
| Maximální počet procesorů                    | 8 ZNALOSTNÍ BÁZI KNOWLEDGE BASE         | 5 000    | 40 MB za sekundu         |
| Max výkon              | 1 024 ZNALOSTNÍ BÁZI KNOWLEDGE BASE      | 200      | 200 MB za sekundu        |
| Max výkon + vysoké procesorů  | 64 KB        | 3,200    | 200 MB za sekundu        |
| Maximální počet procesorů + vysoký výkon  | 32 ZNALOSTNÍ BÁZI KNOWLEDGE BASE        | 5 000    | 160 MB za sekundu        |

Procesorů a šířku pásma vyšší než maximální hodnota disk úložiště jednoho premium, použijte více disků premium rozložené společně. Například pruh dvou P30 discích sekundu vás zajímají kombinované procesorů v 10 000 procesorů nebo kombinované výkon 400 MB. Se dozvíte v další části, je nutné použít velikost OM, která podporuje kombinované disku procesorů a výkon.

>**Poznámka:**  
>Jak zvýšit procesorů nebo výkon druhý taky zvyšuje, zkontrolujte, že že není klikněte na výkon nebo omezení procesorů disku nebo OM při zvětšení kterýkoli z nich.

Účastnit efekty velikost vstupů/výstupů na výkon aplikace, můžete spustit benchmarking nástroje na OM a discích. Vytvoření více zkušební jízdy a pomocí různých velikost vstupů/výstupů pro každé spuštění zobrazit dopad. Přečtěte si část [Benchmarking](#Benchmarking) na konci tohoto článku Další podrobnosti.

## <a name="high-scale-vm-sizes"></a>Vysoký měřítko OM velikosti  
Při spuštění návrhu aplikaci první věcí, které se reprodukujte, zvolte OM hostovat aplikace. Premium úložiště je součástí vysoké OM měřítko formátů mohlo by umožnit spuštění aplikace vyžadující vyšší výpočetního výkonu a vysoký výkon vstupu a výstupu místní disk. Tyto VMs poskytovat rychlejší procesorů, vyšší poměr paměti core a Solid-State jednotky (SSD) na místní disk. Vysoký měřítko VMs podpůrné Premium úložiště příklady řad Pošta, DSv2 a GS VMs.

Vysoký VMs měřítko jsou dostupné v různých velikostech s rozdílný počet procesoru jádra paměti, OS a velikost dočasné disku. Každou OM velikost má i maximální počet disků dat, které můžete připojit angličtině. Proto velikost zvolené OM ovlivní, kolik zpracování paměti, a kapacitu úložiště je k dispozici pro aplikace. Také ovlivňuje jej a úložiště nákladů. V následující tabulce jsou specifikace nejdelší OM v řadě Pošta, DSv2 řady a GS řadě:

| Velikost OM | Procesor vzorky | Paměti | Velikostí disků OM | Max. disků dat | Velikost mezipaměti | PROCESORŮ | Omezení šířky pásma/v mezipaměti |
|---|---|---|---|---|---|---|---|
| Standard_DS14 | 16 | 112 GB | OPERAČNÍ SYSTÉM = 1023 GB <br> Místní SSD = 224 GB | 32 | 576 GB | VÍCE NEŽ 50 000 PROCESORŮ <br> 512 MB za sekundu | 4 000 procesorů a MB 33 za sekundu |
| Standard_GS5 | 32 | 448 GB | OPERAČNÍ SYSTÉM = 1023 GB <br> Místní SSD = 896 GB | 64 | 4224 GB | 80 000 PROCESORŮ <br> 2 000 MB za sekundu | 5 000 procesorů a 50 MB za sekundu |

Pokud chcete zobrazit úplný seznam všech dostupných velikostí Azure OM, v nápovědě k [Windows OM velikostí](../virtual-machines/virtual-machines-windows-sizes.md) nebo [velikosti OM Linux](../virtual-machines/virtual-machines-linux-sizes.md). Výběr velikosti OM, můžete schůzky a změnit velikost vašim požadavkům výkonu požadovanou aplikaci. Kromě toho vzít v úvahu následující důležité při výběru OM velikosti.


*Omezení rozsahu*  
Maximální limity procesorů podle OM a disku jsou rozdílnými a nezávisle na sobě. Ujistěte se, že aplikace dohání procesorů v rámci OM, jakož i disků premium připojené k němu. Výkon aplikace jinak, budou mít omezení.

Jako příklad předpokládejme, že aplikaci požadavkem je maximálně 4 000 procesorů. K dosažení tohoto zřizujete disk P30 na DS1 OM. P30 disk přináší až 5 000 procesorů. OM DS1 je ale omezený na 3,200 procesorů. Proto výkon aplikace bude ovlivněna limit OM na 3,200 procesorů a bude výkon. Chcete-li této situaci předejít, zvolte velikost OM a místo na disku, která budou oba požadavkům aplikace.

*Pole náklady operace*  
V mnoha případech je možné, že celkové náklady operace pomocí Premium úložiště je nižší než pomocí standardní úložiště.

Například zvažte aplikaci vyžadující 16 000 procesorů. Abyste dosáhli tento výkonu, budete potřebovat standardní\_D14 Azure IaaS OM, které je možné maximální procesorů 16,000 pomocí 32 disků standardní úložiště 1TB. Každý disk standardní úložiště 1TB můžete dosáhnout maximálně 500 procesorů. Odhad nákladů tento OM měsíčně budou $1,570. Měsíční náklady 32 disků standardní úložiště bude $1,638. Odhad nákladů měsíční budou $3,208.

Ale pokud hostované stejné aplikaci úložný prostor Premium byste potřebovali zmenšené OM a méně úložiště disk premium, čímž se zmenší celkové náklady. Standardní\_DS13 OM můžete zahájit požadovaného 16 000 procesorů čtyři P30 discích. OM DS13 má maximální procesorů 25,600 a každý P30 disk maximální procesorů 5 000. Celkově této konfiguraci dosáhnete 5 000 x 4 = 20 000 procesorů. Odhad nákladů tento OM měsíčně budou $1,003. Měsíční náklady čtyř P30 premium úložiště disků budou $544.34. Odhad nákladů měsíční budou $1,544.

Tabulka obsahuje souhrn rozdělení nákladů tento scénář pro standardních a Premium úložiště.

| | **Standardní** | **Premium** |
|---|---|---|
| **Náklady OM za měsíc** | $1,570.58 (standardní\_D14)   | $1,003.66 (standardní\_DS13) |
| **Pole náklady disků za měsíc** | 1,638.40 (32 x 1 TB disků) | 544.34 (4 x P30 disků) |
| **Celkové náklady za měsíc**  | $3,208.98 | $1,544.34 |

*Linux Distros*  

S Azure Premium úložiště dostanete stejné úrovni výkonu VMs systém Windows a Linux. Podporujeme mnoho provedeních Linux distros a zobrazit úplný seznam [tady](../virtual-machines/virtual-machines-linux-endorsed-distros.md). Je důležité mít na paměti, že různých distros vhodnější pro různé typy úloh. Zobrazí se různých úrovní výkonu v závislosti na distro spuštěné na svůj pracovní zátěž. Testování distros Linux s aplikací a zvolte ten, který je nejvhodnější.


Jestliže systém Linux s úložištěm Premium, zkontrolujte, zda nejnovější aktualizace týkající se požadované ovladače zajistit vysoký výkon.

## <a name="premium-storage-disk-sizes"></a>Disk velikosti úložiště Premium  
Azure Premium úložiště aktuálně nabízí tři velikosti disku. Velikost pro všechny disku má omezení na jiné měřítko pro procesorů, šířka pásma a úložiště. Volbou vpravo velikost úložiště disku Premium podle požadavky aplikace a vysoký měřítko OM velikost. Následující tabulka ukazuje tři disků velikosti a jejich funkcí.

| **Typ disku**       | **P10**           | **P20**           | **P30**           |
|---------------------|-------------------|-------------------|-------------------|
| Velikost disku           | 128 OS           | 512 OS           | 1 024 OS (1 TB)   |
| Procesorů na disku       | 500               | 2300              | 5000              |
| Výkon na disku | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu |

Kolik disků zvolíte, závisí na disku velikost zvolené. Můžete použít jeden disk P30 nebo více disků P10 plnit požadovanou aplikaci. Promítnutí do uvedených pod ním při rozhodování důležité informace o účtu.

*Omezení rozsahu (procesorů a výkon)*  
Procesorů a výkon limity velikosti disku každý Premium je rozdílnými a nezávislé omezení rozsahu OM. Ujistěte se, že celkem procesorů a výkon z disků v určených mezích měřítko zvolené OM velikosti.

Pokud například aplikaci požadavkem je do velikosti 250 MB/sec výkon a používáte OM DS4 se jeden P30 disk. OM DS4 můžete zobrazit až 256 MB/sec výkon. Jeden disk P30 má však výkon maximálně 200 MB/sec. Aplikace se v důsledku toho omezena pouze na 200 MB/sec kvůli limit na disku. Chcete-li odstranit toto omezení, zřízení víc než jednu disků dat bude v angličtině.

>**Poznámka:**  
>Čtení hodit mezipaměti nejsou součástí disku procesorů a výkon, tedy nevztahuje se na ně omezení disku. Mezipaměť má samostatné procesorů a výkon limit jednoho OM.
>
>Například původně čtení a zápis jsou 60MB/sec a 40MB/sec v tomto pořadí. V průběhu času mezipaměti warms a slouží další apod načte z mezipaměti. Pak můžete získat zápisu vyšší výkon z disku.

*Počet disků*  
Určení počtu disků, které budete potřebovat podle hodnocení požadavky aplikace. Každou OM velikost taky má omezení počtu disků, které můžete připojit angličtině. Obvykle je to dvakrát počet jádra. Ujistěte se, že velikost OM, jaká jste podporují počet disků potřeby.

Mějte na paměti, disků Premium úložiště mít vyšší výkon možnosti ve srovnání s disků standardní úložiště. Proto migrujete aplikace z Azure IaaS OM pomocí standardní úložiště k základnímu úložišti Premium, bude pravděpodobně potřebnosti méně disků premium dosáhnout stejné nebo vyšší výkon aplikace.

## <a name="disk-caching"></a>Diskové mezipaměti  
Vysoký VMs měřítko, které využívají úložišti Premium Azure mít vícevrstvého technologii ukládání do mezipaměti s názvem BlobCache. BlobCache používá kombinace RAM virtuálního počítače a místní SSD pro ukládání do mezipaměti. Trvalý disků Premium úložiště a místních discích OM neexistuje mezipaměti. Ve výchozím nastavení toto nastavení mezipaměti nastavenou pro čtení i zápis disků OS a jen pro čtení u dat disků hostitelem Premium úložiště. S diskové mezipaměti zapnuta disků Premium úložiště, můžete dosáhnout vysoké měřítko VMs velmi vysoké úrovně výkonu, které překročení podkladového výkon disku.

>[AZURE.WARNING] Změna nastavení mezipaměti Azure disku odpojí a znovu připojí cílový disk. Pokud je operační systém disku, OM restartovat. Ukončete všechny aplikace a služby, které možná ovlivněny tento narušení před změnou nastavení diskové mezipaměti.

Další informace o tom, jak BlobCache funguje, najdete pod odkazy uvnitř [Úložišti Premium Azure](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) blogu.

Je třeba povolit mezipaměť na správné nastavení discích. Zda má povolení diskové mezipaměti na disku premium nebo nesmí být závislé na vzorek pracovní zátěž disku bude zpracování. Tabulka ukazuje výchozí nastavení mezipaměti pro OS a Data discích.

| **Typ disku** | **Výchozí nastavení mezipaměti** |
|---|---|
| OS disk | Pouze |
| Disk dat | Žádná |

Nastavení mezipaměti disku doporučený disků dat jsou následující

| **Nastavení mezipaměti na disku** | **Doporučení ohledně kdy použít toto nastavení** |
|---|---|
| Žádná | Konfigurace hostitele mezipaměti jako žádná jen zapsat a zapsat těžké disků. |
| Jen pro čtení | Konfigurace hostitele mezipaměti jako jen pro čtení pro disků jen pro čtení a pro čtení i zápis. |
| Pouze | Konfigurace hostitele mezipaměti jako pouze jenom v případě, že aplikace správně úchyty pro psaní data uložená v mezipaměti trvalý disků v případě potřeby. |

*Jen pro čtení*  
Pomocí konfigurace ukládání do mezipaměti dat Premium úložiště disků jen pro čtení, můžete dosáhnout zhoršeným latence pro čtení a získat velmi vysoké procesorů pro čtení a výkon aplikace. Toto je splatná ze dvou důvodů

1.  Čtení provést v mezipaměti, která je na paměti OM a místní SSD, jsou mnohem rychlejší než čtení z disku dat, který je v úložišti objektů blob Azure.  
2.  Úložiště Premium nepočítá však čtení podávané množství z mezipaměti, směrem k disku procesorů, výkon. Aplikace je proto možné k dosažení vyšší součet procesorů a výkon.

*Pouze*  
Ve výchozím nastavení disků OS má pouze povoleno ukládání do mezipaměti. Nedávno přidaly podpora pouze mezipaměti dat disků stejně. Pokud používáte pouze ukládání do mezipaměti, musíte mít správné způsob, jak zápisu dat z mezipaměti trvalý disků. SQL Server například pracuje s zápisu dat uložených v mezipaměti disků trvalý úložiště na vlastní. Pomocí aplikace, která zpracovává uchování požadovaná data pouze mezipaměti může vést k ztrátou dat, pokud dojde k chybě OM.

Jako příklad, můžete použít tyto pokyny k SQL serveru spuštěna skladování Premium takto,

1.  Konfigurace "Jen pro čtení" mezipaměti na premium úložiště discích hostingu datové soubory.  
    na.  Čas SQL Server dotazu vzhledem k tomu datové stránky jsou mnohem rychleji načte z mezipaměti ve srovnání s přímo z dat disků je rychlý napsáno z mezipaměti malá písmena.  
    b.  Čtení slouží z mezipaměti, znamená, že není k dispozici z premium dat disků další výkon. SQL serveru můžete použít toto další výkon při načítání dalších dat stránek a dalších operací, jako je zálohování a obnovení dávkové zatížení a znovu sestaví index.  
2.  Konfigurace "Žádná" mezipaměti na premium úložiště discích hostingu souborů protokolu.  
    na.  Protokoly neobsahuje primárně zápisu těžké operace. Proto že výhod mezipaměť jen pro čtení.

## <a name="disk-striping"></a>Prokládání disku  
Když vysoké měřítka, které OM je připojen s několika disků trvalý úložiště premium, můžete disků společně rozložené agregovat jejich procesorů šířky pásma a kapacitu úložiště.

Ve Windows můžete úložiště mezery pruh disků společně. Do fondu musíte nakonfigurovat jeden sloupec pro každý disk. Celkový výkon prokládaného svazku jinak, může být nižší než byste čekali, kvůli lichý rozdělení provozu v discích.

Důležité: Pomocí Správce serveru uživatelského rozhraní, můžete nastavit celkový počet sloupců až 8 pro prokládané hlasitost. Při programovém připojování více než 8 disků, pomocí prostředí PowerShell můžete vytvářet hlasitost. Pomocí prostředí PowerShell, můžete nastavit počet sloupců rovno počet disků. Například, pokud existují 16 disků v sadě jednoho pruh; určení 16 sloupců v parametru *NumberOfColumns* rutiny prostředí PowerShell *New-VirtualDisk* .


Na Linux použijte nástroj MDADM pruh disků společně. Podrobný postup na prokládání disků na Linux v příručce [Konfigurace Software RAID na Linux](../virtual-machines/virtual-machines-linux-configure-raid.md).


*Velikost pruhu*  
Je důležité konfigurace v prokládání disků velikost pruhu. Velikost prokládání nebo velikost bloku je nejmenší bloku data, která aplikace můžete řešit jenom na prokládané hlasitost. Velikost pruhu, které jste nakonfigurovali závisí na typu aplikace a jeho žádost o vzorku. Pokud se rozhodnete velikost nepovedlo prokládání, může vést k chybné vstupu a výstupu zarovnání, které vedou k jejich funkčnost bude omezena výkon aplikace.

Například pokud žádost vstupu a výstupu generované aplikace je větší než velikost prokládání disku, úložiště systém zapíše ho přes hranice jednotku pruh na víc než jeden disk. Pokud je potřeba přístup k datům, bude mít hledání ve víc jednotky pruh dokončete zadání. Kumulativní účinek tohoto chování může vést k snížení úrovně který nabízí podstatně vyšší výkon. Na druhé straně je-li vstupu a výstupu žádost o velikost menší než velikost prokládání a je náhodné, požadavky vstupu a výstupu můžou nakupit na stejném disku kritickým a nakonec snížení ZVYŠUJÍ výkon.


V závislosti na typu pracovního vytížení aplikace pracuje, zvolte velikost odpovídající pruhu. Náhodné malé požadavky na vstupu a výstupu použijte menší velikost pruhu. Vzhledem k tomu, velké sekvenčních vstupů/výstupů žádosti o dosáhnete větší velikost pruhu. Zjistěte, doporučení velikost pruh aplikace, že používáte úložný prostor Premium. Pro systém SQL Server nakonfigurujte velikost prokládání 64KB OLTP pracovního vytížení a 256KB skladovými úloh data. V tématu [výkon doporučené postupy pro systém SQL Server na Azure VMs](../virtual-machines/virtual-machines-windows-sql-performance.md#disks-and-performance-considerations) Další informace.


>**Poznámka:**  
>Můžete společně prokládanou maximálně 64 disků úložiště premium na základě série GS OM a 32 disků úložiště premium na základě série DS OM.

## <a name="multi-threading"></a>Multithreading využívaný  
Azure vytvořila platformy Premium úložiště je datových paralelní. Proto aplikace přepočet ve více vláknech dosahuje daleko vyšší výkon než jedním podprocesem aplikace. Přepočet ve více vláknech aplikace rozdělí svých úkolů mezi více vláken a zvýšení efektivity spuštění využitím OM a disku zdroje na maximum.

Například aplikace pracuje v jedné core OM pomocí dvou podprocesů, procesoru můžete přepínat mezi dvěma vláknech dosáhnout efektivity. Během jeden podproces čeká na v/v dokončete disku, procesoru můžete přepnout na jiné vlákna. Tímto způsobem lze provést dvěma vláknech více než jednoho vlákna by. Pokud OM má víc než jeden základní, dál zmenšení pracovního času od každého core můžete provádět úkoly souběžně.

Stát, že nebudete moci měnit způsob, jakým aplikace standardní implementuje jednoho podprocesu nebo Multithreading využívaný. SQL Server je třeba schopen zpracování více procesorů a více základní. SQL Server rozhodne za jakých podmínek jej využít jednu nebo více vláknech zpracuje dotazu. Může spouštění dotazů a vytváření indexů pomocí multithreading využívaný. Pro dotaz, který zahrnuje připojení rozsáhlých tabulek a řazení dat před návratu do uživatele bude SQL serveru pravděpodobně používat více vláken. Uživatele však nelze určit, zda SQL Server spustí dotaz pomocí podproces jednoho nebo více vláken.

Existuje nastavení, která mohou měnit k ovlivnění to multithreading využívaný nebo paralelní zpracování aplikace. V případě serveru SQL Server je například maximální stupeň paralelismu konfigurace. Toto nastavení používají s názvem MAXDOP, umožňuje nastavit maximální počet procesorů, které SQL serveru můžete použít při paralelní zpracování. Konfigurace MAXDOP pro jednotlivé dotazy nebo operace indexování. Toto je skutečným, pokud chcete provést vyrovnání zdroje systému kritické aplikace výkonu.

Dejme tomu, že aplikace pomocí serveru SQL Server probíhá velké dotazu a operaci indexování ve stejnou dobu. Předpokládejme, že byste chtěli index operaci, která má být více performant ve srovnání s velké dotazu. V takovém případě můžete nastavit MAXDOP přínosu index operaci, která má být vyšší než hodnota MAXDOP pro dotaz. Tímto způsobem, SQL Server dosahuje více procesorů, které můžete využít pro operaci indexování ve srovnání s počet procesorů, které můžete určit velké dotazu. Nezapomeňte, že nebudete řídit počet podprocesů serveru SQL Server bude používat pro každou akci. Můžete určit maximální počet procesorů vyhrazené pro multithreading využívaný.

Další informace o [Stupňů paralelismus](https://technet.microsoft.com/library/ms188611.aspx) na serveru SQL Server. Přečtěte si tato nastavení, které ovlivňují multithreading využívaný v aplikaci a jejich konfigurace pro optimalizaci výkonu.

## <a name="queue-depth"></a>Hloubka fronty  
Název hloubkové fronty nebo délka fronty nebo velikost fronty je počet vstupů/výstupů požadavků čekajících v systému. Hodnota hloubka fronty určuje, kolik operací v/v aplikaci můžete zarovnat, které disků úložiště zpracování. Ovlivní všechny indikátory výkonu tři aplikace, které jsme popisované v tomto článku jako procesorů, výkon a latence.

Fronta hloubku a multithreading využívaný jsou úzce související. Hodnota hloubka fronty znamená, kolik multithreading využívaný can dosáhnout aplikace. U velkých hloubka fronty se aplikace může provést další operace souběžně jinými slovy, multithreading využívaný. Pokud hloubka fronty je malý, i když aplikace je přepočet ve více vláknech, nebude mít dost žádosti o zarovnaná souběžné spuštění.

Obvykle vypnout shelf aplikací nepovolují změnit hloubka fronty protože pokud nastavené nesprávně udělá další poškození než co je dobré. Aplikace se nastavit správné hodnoty název hloubkové fronty získat optimální výkon. Je důležité pochopit tohoto konceptu tak, aby se aplikace můžete vyřešit problémy s výkonem. Můžete sledovat taky efekty hloubka fronty spuštěním benchmarking nástroje v počítači.

Některé aplikace zadejte nastavení k ovlivnění hloubka fronty. Například MAXDOP (Maximální stupeň paralelismu) nastavení na serveru SQL Server vysvětlení v předchozí části. MAXDOP je způsob, jak ovlivnit hloubka fronty a multithreading využívaný, i když se nezmění přímo hodnotu hloubka fronty SQL serveru.

*Hloubka vysoké fronty*  
Název hloubkové vysoké fronty zarovnán další operace na disku. Disk zná fronty předem na požadavek na další. Disk proto můžete naplánovat operace předem a zpracovat optimální postupně. Protože aplikace odesílá více požadavků na disk, můžete disk zpracovat další paralelní IOs. Nakonec aplikace bude moct dosažení vyšší procesorů. Protože aplikace zpracování žádostí o další celkový výkon aplikace taky zvyšuje.

Obvykle aplikace můžete dosáhnout maximální výkon s 8-16 + zbývající IOs za připojený disk. Pokud hloubka fronty, aplikace není dost IOs předání do systému a menší velikost zpracuje v dané období. Jinými slovy nižší výkon.

Například v SQL Server, nastavení hodnoty MAXDOP dotazu na "4" informuje SQL serveru, ho můžete až se čtyřmi jádra spuštění dotazu. SQL Server určí, jaký je nejlepší hodnoty název hloubkové fronty a počet jejích vzorky pro spuštění dotazu.

*Hloubka optimální fronty*  
Hodnoty název hloubkové velmi vysoké fronty má i její nevýhody. Pokud je hodnota název hloubkové fronty moc vysoké, aplikace se bude snažit k řízení velmi vysoké procesorů. Pokud má aplikace trvalý disků s dostatečné zřizování procesorů, to negativně ovlivnit čekacích dob aplikace. Následující vzorec zobrazuje relaci mezi procesorů, zpoždění a hloubka fronty.  
    ![](media/storage-premium-storage-performance/image6.png)

Hloubka fronty neměli konfigurovat na libovolnou hodnotu Vysoká, ale optimální hodnotu, která přináší dost procesorů aplikace beze změny čekacích dob. Pokud aplikace latence je třeba 1 milisekund, hloubka fronty potřebné k dosažení 5 000 procesorů je například, QD = 5000 × 0,001 = 5.

*Co je výplň fronty prokládaného svazku*  
Prokládaného svazku udržujte hloubka dostatečně vysoké fronty tak, aby každý disk má hloubka fronty Špička jednotlivě. Například, zvažte aplikaci, která posune hloubka fronty čísla 2 a v pruh jsou 4 discích. Dva požadavky vstupu a výstupu budou přesměrovány na dvou discích a zbývající dva disků bude nečinný. Proto nakonfigurujte hloubka fronty tak, aby se všechny disků může být zaneprázdněn. Následující vzorec ukazuje, jak určit hloubka fronty prokládané svazky.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Omezení  
Azure Premium úložiště ustanovení počet procesorů a výkon v závislosti na velikosti OM a velikosti disku, které zvolíte. Pokaždé, když aplikace pokusí o řízení procesorů nebo výkon nad tyto limity co OM nebo disk můžete zpracovat, bude omezení úložiště Premium ho. To manifesty ve formě výkon v aplikaci. To znamenat vyšší latence, nižší výkon nebo snižovat procesorů. Pokud není omezení úložiště Premium, aplikace selhat úplně tak, že vyšší než co jsou schopné dosáhnout jeho zdroje. Ano Chcete-li předejít problémům s výkonem z důvodu omezení, vždy zřízení dostatečné prostředky pro aplikace. Vzít v úvahu, co jsme popisované v částech velikosti disku výše a OM velikosti. Porovnávání je nejlepší způsob, jak zjistit, jaké prostředky, budete muset aplikaci hostovat.

## <a name="benchmarking"></a>Porovnávání  
Porovnávání je proces simulace různých pracovního vytížení na aplikace a měření výkonu aplikace pro každou pracovní zátěž. Pomocí postupu v předchozí části, jste shromáždili požadavky výkonu aplikace. Spuštěním benchmarking nástroje na VMs hostitelské služby aplikace můžete zjistit úrovně výkonu, které aplikace můžete dosáhnout s úložištěm Premium. V této části jsme umožňují příklady porovnávání standardní OM DS14 zřízení s disků Azure Premium úložiště.

Použili jsme běžné benchmarking nástroje Iometer a FIO, Windows a Linux v tomto pořadí. Tyto nástroje spustit více vláken simulace výrobní jako pracovního vytížení a měření výkonu systému. Pomocí nástrojů můžete konfigurovat taky parametrů, jako je blok velikost a fronty název hloubkové, které obvykle se nedá změnit aplikace. To vám větší flexibilitu při řízení maximální výkon na vysoké měřítku OM zřízení s premium disků pro různé typy úloh aplikace. Další informace o jednotlivých vzorová analytická pomůcka najdete [Iometer](http://www.iometer.org/) a [FIO](http://freecode.com/projects/fio).

Ke sledování na příklady dole, vytvořte standardní OM DS14 a připojte 11 disků Premium úložiště bude v angličtině. 11 disků konfiguraci 10 disků od hostitele osobních ukládání do mezipaměti jako "Žádná" a prokládanou do hlasitost s názvem NoCacheWrites. Konfigurace hostitele ukládání do mezipaměti jako "Jen pro čtení" na zbývající disk a vytvořte hlasitost s názvem čtení z mezipaměti pomocí tohoto disku. Pomocí tohoto nastavení, bude moct najdete v článku maximální výkon pro čtení a zápis ze standardní OM DS14. Podrobnosti o vytváření OM DS14 s disků premium klikněte na [vytvořit a použít účet Premium úložiště pro data disk virtuálního počítače](storage-premium-storage.md#create-and-use-a-premium-storage-account-for-a-virtual-machine-data-disk).

*Zahřívání mezipaměti*  
Disk od hostitele osobních jen pro čtení ukládání do mezipaměti budou moct dát procesorů vyšší než limit na disku. Aby tento maximální rychlost čtení z mezipaměti host, nejdřív se musíte teplé vytvoří mezipaměť tohoto disku. Zajistíte tím, že IOs pro čtení, které vzorová analytická pomůcka bude jednotka svazku čtení z mezipaměti skutečně přístupy do mezipaměti a nikoli disk přímo. Výsledkem mezipaměti přístupů další procesorů z jednoho mezipaměti povolený disku.

>**Důležité:**  
>Před spuštěním porovnávání, pokaždé, když po restartování OM musí teplé nahoru mezipaměti.

#### <a name="iometer"></a>Iometer   
[Stáhněte si nástroj Iometer](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) v OM.

*Testovací soubor*  
Iometer používá testovací soubor, který je uložený na svazku, na které se spustí benchmarking test. Jednotky čtení a data zapisuje na tento soubor test změřit disku procesorů a výkon. Iometer vytvoří tento soubor test, pokud jste to ještě jeden. Vytvoření souboru test 200GB s názvem iobw.tst objemů čtení z mezipaměti a NoCacheWrites.

*Specifikace aplikace Access*  
Specifikace, požádat o velikost vstupů/výstupů, % pro čtení i zápis, % náhodné/sekvenční se konfigurují na kartu "Specifikace aplikace Access" v Iometer. Vytvoření specifikace aplikace access pro jednotlivá pole scénáře píše níže. Vytvořte specifikace aplikace access s "Uložit" s odpovídajícím názvem jako – RandomWrites\_8 kb RandomReads\_8 kB. Vyberte odpovídajícího specifikaci při spuštění scénář testování.

Níže je znázorněn příklad specifikace aplikace access maximální scénář psaní procesorů  
    ![](media/storage-premium-storage-performance/image8.png)

*Specifikace Test maximální procesorů*  
Prokázat maximální procesorů, použijte žádost o zmenšené. Použijte 8K žádosti o velikosti a vytvořte specifikace pro náhodné zapíše a čtení.

| Specifikace aplikace Access | Žádost o velikosti | Náhodné % | Čtení % |
|----------------------|--------------|----------|--------|
| RandomWrites\_8 kb     | 8K           | 100      | 0      |
| RandomReads\_8 kb      | 8K           | 100      | 100    |

*Specifikace Test maximální výkon*  
Prokázat maximální výkon, použijte větší velikost žádost. Žádost o velikosti 64 kB a vytvořením specifikace pro náhodné zapíše a čtení.

| Specifikace aplikace Access | Žádost o velikosti | Náhodné % | Čtení % |
|----------------------|--------------|----------|--------|
| RandomWrites\_64 kB    | 64 KB          | 100      | 0      |
| RandomReads\_64 kB     | 64 KB          | 100      | 100    |

*Spuštěním Iometer*  
Postupujte podle pokynů níže teplé nahoru mezipaměti

1.  Vytvoření dvou specifikace aplikace access s hodnotami ukázáno v následujícím příkladu,

  	| Jméno              | Žádost o velikosti | Náhodné % | Čtení % |
  	|-------------------|--------------|----------|--------|
  	| RandomWrites\_1MB | 1MB          | 100      | 0      |
  	| RandomReads\_1MB  | 1MB          | 100      | 100    |

2.  Spusťte test Iometer inicializace diskové mezipaměti pomocí následujících parametrů. Tři pracovní vlákna určenou hlasitost cílové a hloubka fronty 128. Nastavte dobu trvání "Běhu" testu na 2hrs na kartě "Testování nastavení".

  	| Scénář              | Cíl hlasitosti | Jméno              | Doba trvání |
  	|-----------------------|---------------|-------------------|----------|
  	| Inicializace diskové mezipaměti | Čtení z mezipaměti    | RandomWrites\_1MB | 2hrs     |

3.  Spusťte test Iometer oteplovat diskové mezipaměti pomocí následujících parametrů. Tři pracovní vlákna určenou hlasitost cílové a hloubka fronty 128. Nastavte dobu trvání "Běhu" testu na 2hrs na kartě "Testování nastavení".

  	| Scénář           | Cíl hlasitosti | Jméno             | Doba trvání |
  	|--------------------|---------------|------------------|----------|
  	| Teplé nahoru diskové mezipaměti | Čtení z mezipaměti    | RandomReads\_1MB | 2hrs     |

Po diskové mezipaměti je provozní teplotu, pokračujte scénáře testování vypsané dole. Testu Iometer použijte pro **každou** cílové nejméně tři pracovní vlákna. Pro každý podproces vyberte cílový hlasitost, nastavit fronty hloubku a vyberte jednu z uložené test specifikace uvedené v následující tabulce, spusťte odpovídající scénář testování. Uvádí také očekávané výsledky pro procesorů a výkon při spuštění tyto testy. Pro všechny scénáře menší velikost vstupů/výstupů 8 kB a hloubka vysoké fronty 128 slouží.

| Scénář testování      | Cíl hlasitosti | Jméno              | Výsledek       |
|--------------------|---------------|-------------------|--------------|
| Max. Přečtěte si procesorů     | Čtení z mezipaměti    | RandomWrites\_8 kb  | VÍCE NEŽ 50 000 PROCESORŮ  |
| Max. Psaní procesorů    | NoCacheWrites | RandomReads\_8 kb   | 64 000 PROCESORŮ  |
| Max. Kombinované procesorů | Čtení z mezipaměti    | RandomWrites\_8 kb  | 100 000 PROCESORŮ |
|                    | NoCacheWrites | RandomReads\_8 kb   |              |
| Max. Přečtěte si MB/sec   | Čtení z mezipaměti    | RandomWrites\_64 kB | 524 MB/sec   |
| Max. Psaní MB/sec  | NoCacheWrites | RandomReads\_64 kB  | 524 MB/sec   |
| Kombinované MB/sec    | Čtení z mezipaměti    | RandomWrites\_64 kB | 1 000 MB/sec  |
|                    | NoCacheWrites | RandomReads\_64 kB  |              |

Tady jsou výsledky pro kombinované procesorů výkon příklady použití snímků obrazovek Iometer testu.

*Kombinované čtení a zápis maximální procesorů*  
![](media/storage-premium-storage-performance/image9.png)

*Kombinované čtení a zápis maximální výkon*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO  
FIO je Oblíbené nástroj k základnímu úložišti srovnávacích na Linux VMs. Má flexibilitu vyberte vstupu a výstupu různě, sekvenční nebo náhodné čtení a data zapisuje. Založí pracovních podprocesů nebo procesů k provedení určité operací. Můžete určit typ operací každý podproces musíte udělat používat soubory projektu. Jsme vytvořili jeden soubor úlohy za scénář ukázáno v příkladech dál. Můžete změnit specifikace v těchto úlohy souborů a testu výkonnosti různých úloh spuštěna skladování Premium. V příkladech používáme standardní OM 14 DS spuštění **se systémem Ubuntu**. Před spuštěním testů typovou úlohou použijte stejné nastavení podle na začátku [Benchmarking oddíl](#Benchmarking) a teplé až mezipaměti.

Než začnete, [Stáhnout FIO](https://github.com/axboe/fio) a nainstalovat na počítač virtuální.

Spusťte tento příkaz pro systémem Ubuntu,

        apt-get install fio

Použijeme čtyři pracovních podprocesů pro řízení zápisu a čtyři pracovní vlákna pro řidiče k jízdě operace čtení na discích. Zápis pracovníků bude řízením přenosy na objem "nocache", který má 10 disků s mezipamětí nastavený na hodnotu "Žádný". Čtení zaměstnanců bude řízením přenosy na objem "readcache", který má 1 disk sadou mezipaměti na "Jen pro čtení".

*Maximální zápisu procesorů*  
Vytvoření souboru projektu s následující specifikace získat maximální procesorů psát. Pojmenujte ho "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Poznámka: sledovat základních věcí, které jsou v souladu s pokyny pro návrh popisované v předchozí části. Tyto údaje jsou základní jednotka maximální procesorů  
-   Hloubka vysoké fronty 256 znaků.  
-   Velikost malé bloku 8 kB.  
-   Provádění náhodné zápisy více vláken.

Spusťte tento příkaz a spusťte tak test FIO 30 sekund  

    sudo fio --runtime 30 fiowrite.ini

V průběhu test, budete moct zobrazit počet psaní procesorů OM a doručování Premium disků. Jak je uvedeno v následující ukázce OM DS14 poskytuje jeho maximální zápisu procesorů maximálně 50 000 procesorů.  
    ![](media/storage-premium-storage-performance/image11.png)

*Maximální čtení procesorů*  
Vytvoření souboru projektu s následující specifikace získat maximální procesorů pro čtení. Pojmenujte ho "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Poznámka: sledovat základních věcí, které jsou v souladu s pokyny pro návrh popisované v předchozí části. Tyto údaje jsou základní jednotka maximální procesorů

-   Hloubka vysoké fronty 256 znaků.  
-   Velikost malé bloku 8 kB.  
-   Provádění náhodné zápisy více vláken.

Spusťte tento příkaz a spusťte tak test FIO 30 sekund

    sudo fio --runtime 30 fioread.ini

V průběhu test, budete moct najdete v článku číst počet procesorů OM a doručování Premium discích. Jak je uvedeno v následující ukázce OM DS14 poskytuje více než 64 000 procesorů pro čtení. Toto je kombinace disk a výkonu mezipaměti.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximální pro čtení a zápis procesorů*  
Vytvoření souboru projektu s tímto specifikace získat maximální kombinované pro čtení a zápis procesorů. Pojmenujte ho "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Poznámka: sledovat základních věcí, které jsou v souladu s pokyny pro návrh popisované v předchozí části. Tyto údaje jsou základní jednotka maximální procesorů

-   Hloubka vysoké fronty 128.  
-   Velikost malý blok 4KB.  
-   Více vláken provádění náhodné čte a zapisuje.

Spusťte tento příkaz a spusťte tak test FIO 30 sekund

    sudo fio --runtime 30 fioreadwrite.ini

V průběhu test, bude moct zobrazit počet kombinované pro čtení a zápis procesorů OM a doručování Premium discích. Jak je uvedeno v následující ukázce OM DS14 poskytuje víc než 100 000 kombinované číst a psát procesorů. Toto je kombinace disk a výkonu mezipaměti.  
    ![](media/storage-premium-storage-performance/image13.png)

*Nejvyšší možná výkon*  
Získat maximální kombinovaných pro čtení a zápis výkon, použijte větší velikost bloku a hloubka velké fronty s více vláken provádění čtení a zápis. Můžete blokovat velikosti 64 kB a hloubka fronty 128.

## <a name="next-steps"></a>Další kroky  

Další informace o úložišti Premium Azure:

- [Úložiště Premium: Výkonné úložiště pro pracovního vytížení Azure virtuálního počítače](storage-premium-storage.md)  

Pro uživatele systému SQL Server, přečtěte si články o doporučených postupech výkonu pro systém SQL Server:

- [Výkon doporučené postupy pro systém SQL Server ve virtuálních počítačích Azure](../virtual-machines/virtual-machines-windows-sql-performance.md)
- [Azure Premium úložiště poskytuje nejvyšší výkon pro systém SQL Server v Azure OM](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx) 
