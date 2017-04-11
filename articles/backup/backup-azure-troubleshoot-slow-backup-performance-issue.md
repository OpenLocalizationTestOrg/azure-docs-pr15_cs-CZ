<properties
   pageTitle="Poradce při potížích s pomalé zálohování souborů a složek v Azure zálohování | Microsoft Azure"
   description="Obsahuje řešení problémů s pokyny k diagnostice způsobují problémy s výkonem Azure zálohování"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>Poradce při potížích s pomalé zálohování souborů a složek v Azure zálohování

Tento článek obsahuje řešení problémů s pokyny k diagnostice příčinu pomalý záložní souborů a složek při práci v Azure zálohování. Pokud použijete Azure záložní agent k obecnějším údajům soubory, záložní může trvat déle než obvykle. Toto zpoždění může být způsobeno některým z následujících akcí:

-   [Na počítači, ve kterém je zálohování nejsou kritické.](#cause1)
-   [Jiný proces nebo antivirový software narušuje procesu Azure zálohování.](#cause2)
-   [Agent zálohování běží na Azure virtuálního počítače (OM).](#cause3)  
-   [Máte zálohování velkého počtu (miliony) soubory.](#cause4)

Než začnete, řešení problémů, doporučujeme stáhněte a nainstalujte [nejnovější agent Azure zálohování](http://aka.ms/azurebackup_agent). Provedeme časté aktualizace agenta zálohování k řešení problémů různé funkce a přidejte zvýšení výkonu.

Také důrazně doporučujeme, abyste si prošli [Nejčastější dotazy týkající se služby Azure zálohování](backup-azure-backup-faq.md) aby zkontrolovala, jestli že se objeví některý z běžné problémy s konfigurací.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>Příčina: Kritické v počítači

Kritické body v počítači, který je zálohování může způsobovat zpoždění. Možnosti počítače načtení nebo zápisu dat na disk nebo dostupné šířky pásma poslat data přes síť, například mohou způsobit problémových míst.

Systém Windows nabízí předdefinované nástroj, který má s názvem [Sledování výkonu](https://technet.microsoft.com/magazine/2008.08.pulse.aspx) (Perfmon) zjišťování těchto problémů.

Tady jsou některé výkonnosti a oblastí, které mohou být užitečné při zjišťování problémových míst pro optimální zálohování.

| Čítač  | Stav  |
|---|---|
|Logické disku (fyzickou Disk) – % nečinnosti   | • 100 % nečinnosti 50 % nedělá = pořádku</br>• 49 % nečinnosti 20 % nedělá = upozornění nebo Monitor</br>• 19 % nečinnosti na 0 % nedělá = kritický nebo mimo specifikace|
|  Logické disku (fyzické Disk) – střední % Disku Sec čtení a zápis |  • 0,001 ms 0,015 ms = pořádku</br>• 0,015 ms 0,025 ms = upozornění nebo Monitor</br>• 0.026 ms nebo již = kritický nebo specifikace mimo|
|  Logické disku (fyzické Disk) – aktuální délka fronty disku (pro všechny instance) | 80 žádosti o víc než 6 minut |
| Paměť – fondu není stránkování bajtů|• Menší než 60 % fondu spotřebované množství = pořádku<br>• 61 až 80 % fondu spotřebované množství = upozornění nebo Monitor</br>• Vyšší než 80 % fondu spotřebované množství = kritický nebo mimo specifikace|
| Paměť – fondu stránkování bajty |• Menší než 60 % fondu spotřebované množství = pořádku</br>• 61 až 80 % fondu spotřebované množství = upozornění nebo Monitor</br>• Vyšší než 80 % fondu spotřebované množství = kritický nebo mimo specifikace|
| Paměť – k dispozici megabajtů| • 50 % bezplatná paměti nebo více = pořádku</br>• 25 % bezplatná paměti = monitoru</br>• 10 % bezplatné paměti = upozornění</br>• Méně než 100 MB nebo 5 % bezplatné paměti = kritický nebo mimo specifikace|
|Procesor pro platformu –\%času procesoru (všechny instance)|• Menší než 60 % spotřebované množství = pořádku</br>• 61 90 % spotřebované množství = Monitor nebo upozornění</br>• 91 až 100 % spotřebované množství = kritický|


> [AZURE.NOTE] Pokud zjistíte, že infrastruktury který, doporučujeme defragmentaci disků pravidelně pro lepší výkon.

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>Příčina: Jiného obrázku nebo antivirový software konflikt s Azure zálohování

Jsme viděli několika instancí místo, kam jiné procesy v systému Windows negativně ovlivnit výkonu procesu agent Azure zálohování. Například pokud používáte agenta Azure zálohování a jiným programem zálohování dat nebo pokud antivirový software se systémem s zámek na souborů, které chcete zálohovat, násobku zámky na soubory může způsobit konflikty. V takovém případě může selhat zálohování nebo projektu může trvat déle než obvykle.

Nejvhodnější v tomto scénáři je vypnout jinou záložní aplikaci a zkontrolujte, jestli záložní dobu Azure záložní agent změnil. Obvykle jak zajistit, aby se ve stejnou dobu nepoužívá více úlohy zálohování stačí zabráníte jejich vliv na sobě.

Virů doporučujeme vám vyloučit následující souborů a umístění.

- C:\Program Files\Microsoft Azure obnovení služby Agent\bin\cbengine.exe jako proces
- C:\Program Files\Microsoft Azure obnovení služby Agent\ složky
- Pomocná umístění (Pokud nepoužíváte standardní umístění)

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>Příčina: Zálohování agent spuštěna Azure virtuálního počítače

Pokud používáte agenta zálohování na virtuálního počítače, bude nižší než spustíte ho na fyzické počítač. Očekává se, že kvůli omezení procesorů.  Však můžete optimalizovat výkon přepnutím jednotek dat, které zálohujete k úložišti Premium Azure. Pracujeme na řešení tohoto problému a oprava bude k dispozici v budoucí verzi.

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>Příčina: Zálohování velkého počtu (miliony) souborů

Přesunutí velké množství dat bude trvat déle než přesunutí menší objemu dat. V některých případech záložní čas souvisí s nejen velikosti data, ale také počet souborů a složek. To platí zejména při zálohujete milióny malých souborů (několik bajty na několik kB).

Chování dochází, protože máte zálohování dat a přesunutím Azure, Azure současně programy souborů. V některých případech méně častých katalogu operace může trvat déle než obvykle.

Především vám mohou pomoci pochopit kritické místo a příslušným způsobem pracovat na další kroky:

- **Uživatelské rozhraní zobrazuje průběh pro přenos dat**. Data pořád přenášena. Šířka pásma nebo velikost dat může způsobovat zpoždění.

- **Uživatelské rozhraní není zobrazená průběh pro přenos dat**. Otevřít protokoly c:\ C:\Microsoft Azure obnovení služby Agent\Temp a potom zaškrtněte pro položku FileProvider::EndData v protokolech. Tento údaj označuje, že přenosu data dokončení a neděje operaci katalogu. Nerušit úlohy zálohování. Místo toho počkejte trochu již katalogu operace dokončete. Pokud potíže potrvají, obraťte se na [podporu Azure](https://portal.azure.com/#create/Microsoft.Support).
