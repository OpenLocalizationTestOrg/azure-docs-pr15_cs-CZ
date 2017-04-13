<properties 
   pageTitle="Místně StorSimple připnuté svazky nejčastější dotazy týkající se | Microsoft Azure"
   description="Najdete odpovědi na nejčastější dotazy týkající se StorSimple místně připnuté svazky."
   services="storsimple"
   documentationCenter="NA"
   authors="manuaery"
   manager="syadav"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/16/2016"
   ms.author="manuaery" />

# <a name="storsimple-locally-pinned-volumes-frequently-asked-questions-faq"></a>Místně StorSimple připnuté svazky: Nejčastější dotazy

## <a name="overview"></a>Základní informace

Následují otázek a odpovědí, které si můžete při vytvoření svazku StorSimple místně připnuté, převedení vrstvené hlasitost místně připnuté objem (nebo naopak), nebo zálohování a obnovení místně připnuté hlasitost.

Otázky a odpovědi jsou uspořádány do těchto kategorií

- Vytvoření místně připnuté svazku
- Zálohování místně připnuté
- Převedení vrstvené hlasitost místně připnuté hlasitosti
- Obnovení místně připnuté hlasitosti
- Převzetí místně připnuté hlasitosti

## <a name="questions-about-creating-a-locally-pinned-volume"></a>Dotazy týkající se vytváření místně připnuté hlasitosti

**OTÁZKA:** Co je maximální velikosti doručovaných místně připnuté svazku, který můžete vytvořit na zařízeních 8000 řady?

**A** můžete zřizujete místně připnuté svazky 8.5 TB nebo vrstvené svazky až 200 TB na 8100 zařízení. Větší 8600 zařízení, můžete vytvořit místně připnuté svazky 22,5 TB nebo vrstvené svazky až 500 TB.

**OTÁZKA:** Můžu naposledy zařízení 8100 upgradovaný na verzi aktualizace 2 a při pokusu o vytvoření místně připnuté svazku, maximální velikost dostupné je jenom 6 TB a ne 8.5 TB. Proč nelze vytvořit svazku 8.5 TB?

**A** můžete zřizujete místně připnuté svazky až 8.5 TB nebo vrstveny svazky až 200 TB na 8100 zařízení. Pokud vaše zařízení už má vrstveny svazky, pak místa pro vytvoření místně připnuté svazku bude úměrně nižší než maximální limit. Například pokud již byly vytvořeny 100 TB vrstvené svazky na 8100 zařízení (což je polovině vrstvené kapacita), potom maximální velikosti doručovaných místní svazku, který můžete vytvořit na 8100 zařízení odpovídajícím způsobem sníží 4 TB (přibližně poloviční maximální místně připnuté kapacity svazku).

Protože místní místo na zařízení se používá k hostování pracovní sada vrstvené svazky, je volné místo pro vytvoření místně připnuté svazku Pokud zařízení má vrstveny svazky nižší. Vytvoření místně připnuté svazku proporčně naopak snižuje volné místo pro vrstvené svazky. Následující tabulka shrnuje dostupné vrstvené kapacity na zařízeních 8100 a 8600 při vytvoření místně připnuté svazky.

|Místně připnuté svazky zřizování kapacity|K dispozici schopnost být k dispozici vrstvené svazky - 8100|K dispozici schopnost být k dispozici vrstvené svazky - 8600|
|-----|------|------|
|0 | 200 TB | 500 TB |
|1 TB | 176.5 TB | 477.8 TB|
|4 TB | 105.9 TB | 411.1 TB |
|8.5 TB | 0 TB | 311.1 TB|
|10 TB | NEDEF | 277.8 TB |
|15 TB | NEDEF | 166.7 TB |
|22.5 TB | NEDEF | 0 TB |


**OTÁZKA:** Proč je vytvoření místně připnuté svazku dlouhodobé operace? 

**ODPOVĚĎ:** Místně připnuté svazky jsou thickly zřízení. Pokud chcete udělat víc místa na místní úrovně zařízení, může být některá data z existující vrstvené svazky posune do cloudu během procesu zřizovací. A od to závisí na velikosti objemu zřízení, existující data na svém zařízení a dostupné šířky pásma do cloudu, dobu trvání k vytvoření místní svazku může být několik hodin.

**OTÁZKA:** Jak dlouho trvá k vytvoření místně připnuté svazku?

**ODPOVĚĎ:** Protože místně připnuté svazky jsou thickly zřízení, může některé existující data z vrstvené svazky posune do cloudu během procesu zřizovací. Proto čas potřebný k vytvoření místně připnuté svazku závisí na několika faktorech, včetně velikosti hlasitost, údajů o zařízení a dostupné šířky pásma. Na právě nainstalované zařízení, které má žádné svazky je čas vytvoření místně připnuté svazku asi 10 minut za terabajt data. Vytvoření místní svazky však může trvat několik hodin na základě faktorů vysvětlení nad na zařízení, které používá.

**OTÁZKA:** Chci vytvořit místně připnuté hlasitost. Nejdou nějaké doporučené postupy, potřebné je potřeba vědět?

**ODPOVĚĎ:** Místně připnuté svazky jsou vhodné pro úloh, které vyžadují místní záruky dat vždy a, jsou závislé na cloud čekacích dob. Při rozhodování, použití místní svazky některého z úloh, uvědomte si prosím z následujících akcí:

- Místně připnuté svazky jsou thickly zřízenou a vytvoření místních svazky ovlivňuje volné místo pro vrstvené svazky. Proto doporučujeme začínat menším svazky a škálování svůj požadavek zvýšením úložiště.

- Zřízení místní objemů je dlouhodobé operace, která může zahrnovat předání existujících dat z vrstvené svazky do cloudu. Jako výsledek může dojít ke snížení výkonu tyto objemů.

- Zřízení místní objemů je časově náročný operace. Pole Skutečná doba souvisejících závisí na několika faktorech: velikosti hlasitost Probíhá zřizování, údajů o zařízení a dostupné šířky pásma. Pokud jste nezálohovali existující svazky do cloudu, je vytvoření svazku pomaleji. Měli byste že pořizovat cloudu snímky existující svazky před zřízení místní hlasitost.
 
- Můžete převést existující vrstvené svazky místně připnuté svazky a přepočet zahrnuje zřizování místa na zařízení pro výsledné místně připnuté hlasitost (kromě uvedení dolů vrstvené dat [případné] z cloudu). Opět se jedná dlouhodobé operaci, která závisí na faktory, které jsme jste popsané výše. Měli byste, jak bude proces ještě pomaleji, pokud nejsou zálohovat existující svazky obecnějším údajům existující svazky před převodem. Zařízení s také výkon můžete zaznamenat, omezená během tohoto procesu.
    
Další informace o tom, jak [vytvořit místně připnuté hlasitosti](storsimple-manage-volumes-u2.md#add-a-volume)

**OTÁZKA:** Můžete vytvořit více místně připnuté svazky ve stejnou dobu?

**ODPOVĚĎ:** Ano, ale všechny úlohy vytváření a rozšíření místně připnuté hlasitost zpracování postupně.

Místně připnuté svazky jsou thickly zřízenou a vyžaduje vytvoření místní místa na zařízení (který může mít za následek existující data z vrstvené svazky přesunut do cloudu během procesu zřizovací). Proto pokud probíhá zřizování projekt, jiné úlohy vytvoření místní hlasitost bude ve frontě nedokončili této úlohy.

Podobně případně stávající místní hlasitost je rozbalení vrstvené hlasitost je převáděny na místně připnuté objem, pak vytvoření nové místní připnuté svazku je ve frontě až do dokončení předchozí úlohy. Rozbalení velikost místně připnuté svazku zahrnuje rozšíření existující místní prostoru pro tuto hlasitost. Převod z vrstvené k místně připnuté hlasitost také zahrnuje vytvoření místní místa pro výsledné místně připnuté hlasitost. V obou těchto operací, vytvoření nebo rozšíření místních místa typu long běží projektu.

Zobrazit tyto úlohy ve stránce **úlohy** služby Azure StorSimple správce. Úlohy, která aktivně zpracovávání automaticky projeví při aktualizaci průběhu zřizování místa. Zbývající práce místně připnuté hlasitost je označený jako spuštění, ale pozastavena daří plnit a jsou vybrány v pořadí, v jakém že jsou ve frontě.

**OTÁZKA:** Můžu odstranit místně připnuté hlasitost. Proč nevidím odvodněné místa v volné místo projeví při pokusu o vytvoření nového svazku? 

**ODPOVĚĎ:** Pokud byste odstranili místně připnuté objem, nemusí být místa pro nové svazky aktualizované okamžitě. Služba StorSimple správce aktualizuje místa na disku místní přibližně každou hodinu. Měli byste že čekat hodinu před pokusem o vytvoření nové hlasitost.

**OTÁZKA:** Podporují místně připnuté svazky v cloudu zařízení?

**ODPOVĚĎ:** Místně připnuté svazky nejsou podporovány v cloudu zařízení (8010 8020 zařízení a dříve označované jako StorSimple virtuální zařízení).

**OTÁZKA:** Můžete pomocí rutin prostředí PowerShell Azure vytváření a správa místně připnuté svazky? 

**ODPOVĚĎ:** Ne, nelze vytvořit místně připnuté svazky pomocí rutin prostředí PowerShell Azure (svazky, který vytvoříte pomocí prostředí PowerShell Azure vrstveny.). Měli byste taky nepoužívejte rutiny prostředí PowerShell Azure upravit jakékoli vlastnosti místně připnuté hlasitost, protože bude mít nežádoucí vliv na změny typu svazku na vrstvené.

## <a name="questions-about-backing-up-a-locally-pinned-volume"></a>Dotazy týkající se zálohování místně připnuté hlasitosti

**OTÁZKA:** Jsou místní snímky místně připnuté svazky podporované?

**ODPOVĚĎ:** Ano, může trvat místní snímky místně připnuté svazky. Však důrazně doporučujeme pravidelně zálohovat místně připnuté svazky se snímky cloudu zajistit, že vaše data se po zamknutí v případě selhání.

**OTÁZKA:** Jsou všechny pokyny pro správu místních snímků místně připnuté svazky?

**ODPOVĚĎ:** Časté místní snímky vedle vysoká rychlost konve data místně připnuté objemu může způsobit místní místa na tomto zařízení rychle spotřebované a mít za následek data z vrstvené svazky stisknuté do cloudu. Proto doporučujeme že minimalizovat počet místní snímky.

**OTÁZKA:** Můžu obdržel oznámení, že můj místní snímky místně připnuté svazky může být ukončena jejich platnost. Kdy lze to tak děje?

**ODPOVĚĎ:** Časté místní snímky vedle vysoká rychlost konve data místně připnuté objemu může způsobit místní místa na zařízení, které má být spotřebované množství rychle. Použijete-li místní úrovně zařízení velkém, rozšířené cloudu výpadku může způsobit zařízení stále úplné a příchozí zápisy hlasitost může mít za následek zneplatnění snímky (jak aktualizovat snímky odkázat na starší bloky data, která byla přepíšou existuje bez mezer). V takovém případě zápisu hlasitost zůstanou v být podávané množství, ale místní snímků může být neplatné. Neexistuje žádný vliv na stávající snímky cloudu.

Upozornění upozornění je s upozorněním, že tato situace můžete nastanou a zajistěte, aby že stejná adresa včas revizí místní snímky plány se méně častých místní snímky nebo odstraňování starších místní snímky, které jsou už není potřeba.

Pokud se zruší místní snímky, získáte informace upozornění oznamující, že místní snímků pro konkrétní zásady zálohování platnost vedle seznamu časová razítka místní snímků, které byly ukončena jejich platnost. Tyto snímky, budou odstraněny automaticky a již nebude moct prohlédnout na stránce **Zálohování katalogy** v portálu Azure klasické.

## <a name="questions-about-converting-a-tiered-volume-to-a-locally-pinned-volume"></a>Dotazy týkající se převedení vrstvené hlasitost místně připnuté hlasitosti

**OTÁZKA:** Některé pomalost na zařízení se mi pozorování při převodu vrstvené hlasitost místně připnuté hlasitost. Proč se to děje? 

**ODPOVĚĎ:** Proces převodu zahrnuje dva kroky: 

  1. Zřízení místa na zařízení brzy – k--převedou místně připnuté hlasitost.
  2. Stažení vrstvené dat z cloudu zajistit místní zaručuje.

Oba tyto kroky jsou dlouhé systém operacích, které jsou závislé na velikosti hlasitost převodu, údajů o zařízení a dostupné šířky pásma. Jak některá data z existující vrstvené svazky může přepadového do cloudu jako součást procesu zřizovací, zařízení výkon můžete zaznamenat, omezená během tohoto období. Kromě toho, může být převodu pomaleji pokud:

- Existující svazky byly nezálohovali do cloudu; abyste měli byste zálohovat svazky před zahájením převod.

- Byly použity zásady omezení šířky pásma, které můžou omezit dostupné šířky pásma do cloudu; Proto doporučujeme že máte vyhrazená 40 MB / nebo další připojení ke cloudu.

- Proces převodu může trvat několik hodin z důvodu několika faktorech vysvětlení nad; Proto doporučujeme provést operaci době není píků nebo na víkendu Chcete-li předejít dopad na konec spotřebitele.

Další informace o tom, jak [Převést vrstvené hlasitost místně připnuté objem](storsimple-manage-volumes-u2.md#change-the-volume-type)

**OTÁZKA:** Můžete zrušit operaci převodní hlasitost?

**ODPOVĚĎ:** Ne, nemůžete zrušit operaci převodní jednou, které iniciuje. Jak je uvedeno v předchozí otázce, uvědomte potenciální problémy s výkonem, může dojít během procesu a osvědčenými postupy výše uvedené při plánování vaší převod.

**OTÁZKA:** Co se stane Moje objem operaci převodní selže?

**ODPOVĚĎ:** Převod hlasitost může selhat kvůli problémy s připojením cloudu. Zařízení přestat převodu postupně po určitém počtu pokusech o snížilo vrstvené data z cloudu. V tomto případě typu svazku budou nadále hlasitost typ zdroje před převodu a:

- Upozornění na kritické zaokrouhleno na zobrazil upozornění při Chyba při převodu hlasitost. Další informace o [upozornění související s místně připnuté svazky](storsimple-manage-alerts.md#locally-pinned-volume-alerts)

- Pokud převádíte vrstvené místně připnuté objem, zůstanou hlasitost vykazovat vlastnosti vrstvené objemu jako dat může stále nacházejí v cloudu. Měli byste vyřešit problémy s připojením a zkuste operaci převodní.
 
- Podobně selhání převod místně připnuté vrstvené objem, přestože hlasitost budou označeny jako místně připnuté hlasitost, funkce jako vrstvené objem (protože dat může mít možno v cloudu). Však budou dál zaplnit místo na místní úrovně zařízení. Zde nebudou k dispozici pro jiné místně připnuté svazky. Měli byste, opakujte tuto operaci ověřit, že dokončení převodu hlasitost a místní místo na zařízení můžete znovu použít.

## <a name="questions-about-restoring-a-locally-pinned-volume"></a>Otázky týkající se obnovení místně připnuté hlasitosti

**OTÁZKA:** Jsou místně připnuté svazky obnovit okamžitě?

**ODPOVĚĎ:** Ano, místně připnuté svazky se obnoví okamžitě. Jakmile informace metadat pro hlasitost pocházejí z cloudu jako součást operace obnovení, hlasitost do online režimu a dají se otevřít hostitelem. Místní záruky svazku, data nebudou prezentovat tak, aby byla všechna data stažená z cloudu a byste mohli narazit však nižší výkon tyto objemů po celou dobu obnovit.

**OTÁZKA:** Jak dlouho trvá obnovit místně připnuté hlasitost?

**ODPOVĚĎ:** Místně připnuté svazky jsou obnovit okamžitě a do online režimu hned, jak informace metadat hlasitost je načtená z kontingenčního seznamu cloudu, zatímco data hlasitost nadále stáhnout na pozadí. Tento druhá část obnovení – začíná zpátky místního záruky hlasitost dat – je dlouhodobé operace a může trvat několik hodin všechna data, která chcete označit jako místní znovu. Čas potřebný k dokončení stejné závisí na několika faktorech, třeba velikost hlasitost obnovována nebo dostupné šířky pásma. Pokud odstranil původní svazku, který právě obnovována další doba trvání k vytvoření místní místa na zařízení jako součást operace obnovení.

**OTÁZKA:** Můžu potřeba obnovit svůj stávající místně připnuté hlasitost starší snímek (vzít při vrstveny hlasitost). Jestli se obnoví jako v tomto případě vrstveny?

**ODPOVĚĎ:** Ne, hlasitost obnoví jako místně připnuté hlasitost. Ačkoli snímek kalendářních dat na čas, kdy byla vrstveny hlasitost, při obnovení existující svazky StorSimple vždy používá typ hlasitosti na disku protože aktuálně existuje.

**OTÁZKA:** Můžu Moje místně připnuté hlasitost rozšířené naposledy, ale teď chcete obnovit dat na čas, kdy byla menší velikosti hlasitost. Zvětší obnovení aktuální hlasitost a bude potřeba rozšířit velikost hlasitost po dokončení obnovení?

**ODPOVĚĎ:** Ano, obnovení zvětší hlasitost a budete muset rozšířit velikost hlasitost po dokončení obnovení.

**OTÁZKA:** Můžete změnit typ svazku během obnovení?

**A.** Během obnovení Ne, nelze změnit typ hlasitost.

- Svazky, které jste odstranili se obnoví psaní uložené ve snímku.

- Existující svazky se obnoví podle jejich aktuální typ, bez ohledu na typ uložené ve snímku (viz předchozí dvě otázky).
 
**OTÁZKA:** Potřeba obnovit Moje místně připnuté objem, ale vybraných kontakty nesprávné čárky čas snímku. Můžete zrušit aktuální obnovení?

**ODPOVĚĎ:** Ano, můžete zrušit obnovení na přechod. Stav hlasitost se vrátit k stavu na začátku na obnovit. Zápis, provedené hlasitost při obnovení byla v průběhu však budou ztraceny.

**OTÁZKA:** Spustit operaci obnovení k některé z mé místně připnuté svazky a teď uvidím snímek v Moje rezervu katalog, který se nemusí recollect vytváření. Co je to použit pro?

**ODPOVĚĎ:** Jedná se dočasné snímek, který je vytvořený před obnovením a používá se pro vrácení zpět v případě obnovení se zruší nebo neúspěšné. Neodstraňujte tohoto snímku; To se automaticky odeberou po dokončení obnovení. Toto chování dochází, jestliže obnovení práce obsahuje pouze místně připnuté svazky nebo kombinaci místně připnuté a vrstvené svazky. Pokud úlohy obnovení obsahuje pouze vrstvené svazky, toto chování nedojde.

**OTÁZKA:** Můžete vytvořit kopii místně připnuté hlasitost?

**ODPOVĚĎ:** Ano, můžete si. Místně připnuté hlasitost bude však klonovat jako vrstvené hlasitost ve výchozím nastavení. Další informace o tom, jak [klonovat místně připnuté hlasitosti](storsimple-clone-volume-u2.md)

## <a name="questions-about-failing-over-a-locally-pinned-volume"></a>Dotazy týkající se při přechodu místně připnuté hlasitosti

**OTÁZKA:** Potřebuji selhání na svém zařízení na jiné zařízení fyzicky. Moje místně připnuté svazky se nezdaří delší než, případně místně připnuté vrstveny?

**ODPOVĚĎ:** V závislosti na verzi cílového zařízení místně připnuté svazky se nezdaří přes jako:

- Místně připnuté pokud cílové zařízení běží StorSimple 8000 aktualizace řady 2
- Vrstveny pokud cílové zařízení běží StorSimple 8000 řady aktualizovat 1.x
- Vrstveny pokud cílové zařízení je zařízení cloud (aktualizace verze 2 nebo aktualizovat 1.x)

Další informace o [převzetí a DR místně připnuté svazky mezi verzemi](storsimple-device-failover-disaster-recovery.md#device-failover-across-software-versions)

**OTÁZKA:** Místně připnuté svazky okamžitě obnoveny během obnovení havárie (DR)?

**ODPOVĚĎ:** Ano, místně připnuté svazky se obnoví okamžitě při selhání. Jakmile informace metadat pro hlasitost pocházejí z cloudu jako součást operace překlopení, hlasitost do online režimu na cílové zařízení a dají se otevřít hostitelem. Mezitím objemu dat, zůstanou stáhnout na pozadí a může dojít ke snížení výkonu tyto objemů dobu trvání záložní.

**OTÁZKA:** Můžu najdete v článku dokončeny převzetí úlohy, jak můžete sledovat průběh místně připnuté svazku, který právě obnovována na cílové zařízení?

**ODPOVĚĎ:** Během operace převzetí úlohy převzetí označena jako dokončit jednou všechny svazky v sadě převzetí byly okamžitě obnovit a do online režimu na cílové zařízení. Jedná se o svazky místně připnuté, které může failed však místní záruky data jenom bude k dispozici při stažená všechna data hlasitost. Můžete sledovat tento postup pro každou místní připnutý úkol, který se nezdařila úplně od začátku sledování odpovídající úloh obnovení vytvořené v rámci záložní. Tyto jednotlivých úloh obnovení pouze se vytvoří místně připnuté svazky.

**OTÁZKA:** Můžete změnit typ svazku při selhání?

**ODPOVĚĎ:** Ne, nemůžete změníte hlasitost typ během přepojení. Pokud se nedaří myši na jiné zařízení fyzicky, na kterém běží StorSimple 8000 řady aktualizace 2, svazky se nezdaří v závislosti na použitém typu objem uložené ve snímku. Když nedaří myši na jiné zařízení verzi, v nápovědě k otázce nad typu svazku po přepojení.

**OTÁZKA:** Můžete převzít kontejner hlasitost pomocí místně připnuté svazky do cloudu zařízení?

**ODPOVĚĎ:** Ano, můžete si. Místně připnuté svazky se nezdaří přes jako vrstvené svazky. Další informace o [převzetí a DR místně připnuté svazky mezi verzemi](storsimple-device-failover-disaster-recovery.md#considerations-for-device-failover)


