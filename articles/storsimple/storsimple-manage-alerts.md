<properties 
   pageTitle="Zobrazení a správa StorSimple upozornění | Microsoft Azure"
   description="Popisuje StorSimple upozornění podmínky a závažnosti, jak nakonfigurovat oznámení a jak používat službu StorSimple správce ke správě upozornění."
   services="storsimple"
   documentationCenter="NA"
   authors="SharS"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="anbacker" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-storsimple-alerts"></a>Umožňuje zobrazovat a spravovat upozornění StorSimple službu StorSimple správce

## <a name="overview"></a>Základní informace

Na kartu **výstrahy** ve službě StorSimple správce poskytují způsob, jak můžete zkontrolovat a zrušte StorSimple související s zařízení upozornění na základě v reálném čase. Na kartě můžete problémy stavu StorSimple zařízení a celkové řešení Microsoft Azure StorSimple centrálně sledování.

Tento kurz popisuje běžné upozornění podmínky, úrovně upozornění závažnosti a postup při konfiguraci oznámení. Dále obsahuje upozornění stručný přehled tabulek, které umožňují rychle najít konkrétní upozornění a reagovat.

![Upozornění na stránku](./media/storsimple-manage-alerts/HCS_AlertsPage.png)

## <a name="common-alert-conditions"></a>Společné upozornění podmínky

Zařízení s StorSimple vygeneruje upozornění v odpovědi na řadu podmínky. Následují nejběžnější typy upozornění podmínek:

- **Problémy s hardwarem** – upozornění na tyto informace o stavu hardwaru. Budou se vám zobrazovala-li potřeba upgrady firmwaru, pokud rozhraní sítě má problémy nebo pokud se problém s některým z vašich jednotkách data.

- **Problémy s připojením** – tyto upozornění vyskytnout, když problémy s přenosem zpráv data. Komunikace problémů může dojít během převodu dat do a z Azure úložiště účtu nebo z důvodu nedostupnosti zařízení a služba StorSimple správce. Problémy s komunikace jsou uvedeny některé nejtěžší problém odstranit: vzhledem k tomu, že je tolik bodů selhání. Vždycky nejdřív zkontrolujte, zda připojení k síti přístup k Internetu k dispozici před pokračováním k pokročilejší Poradce při potížích. Pomoc s Poradce při potížích s projděte si téma [Poradce při potížích s rutinu testovat připojení](storsimple-troubleshoot-deployment.md).

- **Problémy s výkonem** – tyto upozornění dochází při systému není optimálně, například při velkém zatížení.

Kromě toho se může zobrazit upozornění týkající se zabezpečení, aktualizace nebo chyby úlohy.

## <a name="alert-severity-levels"></a>Úrovně upozornění závažnosti

Upozornění mají různé úrovně závažnosti, podle toho, vliv bude mít upozornění situaci a potřebné pro odpovědi na upozornění. Úrovně závažnosti jsou:

- **Kritický** – toto oznámení se odpověď podmínku, která má vliv na úspěšné výkonu systému. Akce je nezbytné k zajištění StorSimple není přerušení služby.

- **Varování** – tato situace může být za kritický, pokud není vyřešit. Měli byste prošetřit situaci a provádět žádnou akci povinné zrušte problém.

- **Informace o** – toto oznámení obsahuje informace, které mohou být užitečné při sledování a správa systému.

## <a name="configure-alert-settings"></a>Konfigurace nastavení oznámení

Můžete zvolit, jestli má být odesláno oznámení e-mailem upozornění podmínky pro jednotlivá pole StorSimple zařízení. Kromě toho můžete identifikujete ostatními příjemci oznámení zadáním jejich e-mailové adresy do pole **ostatním příjemcům e-mailu** , oddělené středníkem.

>[AZURE.NOTE] Můžete vytvořit maximálně 20 e-mailové adresy na zařízení.

Po povolení e-mailového oznámení pro zařízení členů seznamu oznámení, dostanou e-mailové zprávy důležité upozornění při každém výskytu. Odešle zprávy z *storsimple-alerts-noreply@mail.windowsazure.com* a bude popisují upozornění podmínku. Příjemci můžete kliknout na **Odhlásit odběr** sami odebrat ze seznamu e-mailových oznámení.

#### <a name="to-enable-email-notification-of-alerts-for-a-device"></a>Chcete-li povolit e-mailového oznámení upozornění pro zařízení

1. Přejděte na **zařízení** > **Konfigurovat** pro zařízení.

2. V části **Nastavení upozornění**nastavte takto:

    1. V poli **Odeslat e-mailového oznámení** vyberte **Ano**.

    2. V poli **Správci služeb e-mailu** vyberte **Ano** , pokud chcete mít má správce služby a všech dalších správců oznámení hlášení.

    3. V poli **ostatním příjemcům e-mailu** zadejte e-mailové adresy všech příjemců, kteří mají dostávat oznámení hlášení. Zadejte jména ve formátu *someone@somewhere.com*. Pomocí středníků oddělit e-mailové adresy. Můžete nakonfigurovat maximálně 20 e-mailové adresy na zařízení. 

        ![Konfigurace oznámení upozornění](./media/storsimple-manage-alerts/AlertNotify.png)

3. Odeslání zkušební e-mailového oznámení, klikněte na šipku vedle **Odeslání zkušební e-mailu**. Služba StorSimple správce zobrazí zprávy o stavu jako předává test oznámení. 

4. Pokud se zobrazí následující zpráva, klikněte na **OK**. 

    ![Upozornění na testování odeslané e-mailových oznámení](./media/storsimple-manage-alerts/HCS_AlertNotificationConfig3.png)

    >[AZURE.NOTE] Pokud není odeslat zprávu oznámení o zkušební, službu StorSimple správce zobrazí příslušnou zprávu. Klikněte na **OK**, počkejte pár minut a zkuste znovu odeslat zprávu oznámení o zkušební. 

## <a name="view-and-track-alerts"></a>Zobrazení a sledování upozornění

Řídicí panel služeb StorSimple správce obsahuje stručný přehled počet upozornění na vašich zařízeních uspořádány podle úrovně závažnosti.

![Řídicí panel upozornění](./media/storsimple-manage-alerts/admin_alerts_dashboard.png)

Kliknutím na tlačítko úroveň závažnosti otevřete kartu **výstrahy** . Výsledky obsahovat pouze upozornění, které odpovídají dané závažnosti úrovni.

![Upozornění na zprávu obor vymezený na typ oznámení](./media/storsimple-manage-alerts/admin_alerts_scoped.png)

Kliknutí na upozornění v seznamu umožňuje další podrobnosti pro upozornění, včetně posledního nahlášených upozornění, počet výskytů upozornění na zařízení a doporučené akci, kterou chcete vyřešit upozornění. Pokud je upozornění k hardwaru, zjistíte také komponentu hardwaru.

![Příklad oznámení na hardware](./media/storsimple-manage-alerts/admin_alerts_hardware.png)

Můžete zkopírovat podrobnosti výstrahy k textovému souboru v případě potřeby odeslat informace společnosti Microsoft Support. Po a až pak doporučení a vyřešit upozornění podmínka místní, byste měli zakázat upozornění ze zařízení výběrem upozornění na kartě **upozornění** a kliknutím na **Vymazat**. Zrušte několik upozornění, vyberte každý upozornění, klikněte na všechny sloupce kromě sloupci **upozornění** a potom klikněte na **Vymazat** po výběru všechna upozornění na Vymazat. Všimněte si, že některé upozornění vymažou automaticky při problém vyřeší nebo systému aktualizuje upozornění na nové informace.

Když kliknete na tlačítko **Vymazat**, máte možnost názor týkající se upozornění a kroky, které jste pořídili pomocí tento problém vyřešit. Některé události vymaže se systémem Pokud jiné události se při spuštění aktivují novými informacemi. V takovém případě zobrazí se tato zpráva.

![Zrušte zaškrtnutí políčka výstrahy zabezpečení](./media/storsimple-manage-alerts/admin_alerts_system_clear.png)

## <a name="sort-and-review-alerts"></a>Řazení a revize upozornění

Bude pravděpodobně efektivnější tak, aby můžete zkontrolovat a vymazat ve skupinách spuštění sestav na upozornění. Na kartě **upozornění** navíc můžete zobrazit až 250 upozornění. Pokud překročíte určeným počtem upozornění, zobrazí se ve výchozím zobrazení není všechna upozornění. Můžete kombinovat následující pole můžete přizpůsobit zobrazení upozornění:

- **Stav** – můžete zobrazit **aktivní** nebo **zúčtováno** upozornění. Aktivní upozornění se stále aktivují v počítači, zatímco nezaškrtnuté upozornění byly buď ručně zrušeno správcem nebo programově vymazat, protože systému aktualizaci upozornění podmínku novými informacemi.

- **Závažnosti** – můžete zobrazit upozornění na všechny úrovně závažnosti (kritické, upozornění, informace o) nebo jenom určité závažnosti, například jenom kritické upozornění.

- **Zdroj** – můžete zobrazit oznámení ze všech zdrojů nebo omezit upozornění na ty, které pocházet z službu nebo jeden nebo všechna zařízení.

- **Časový rozsah** – zadáním **od** a **do** data a časových razítek, můžete si může samostatně prohlížet upozornění časového období, která vás zajímá.

## <a name="alerts-quick-reference"></a>Stručný přehled upozornění

Následující tabulka uvádí některé z Microsoft Azure StorSimple upozornění, ke kterým může dojít, a dalších informací a doporučení-li k dispozici. Upozornění na zařízení StorSimple spadají do jedné z těchto kategorií:

- [Shluk připojení upozornění](#cloud-connectivity-alerts)

- [Upozornění na obrázku](#cluster-alerts)

- [Upozornění na obnovení havárie](#disaster-recovery-alerts)

- [Hardwarová upozornění](#hardware-alerts)

- [Výstrahy k chybám úloh](#job-failure-alerts)

- [Místně připnuté hlasitost upozornění](#locally-pinned-volume-alerts)

- [Upozornění sítě](#networking-alerts)

- [Upozornění na výkon](#performance-alerts)

- [Upozornění zabezpečení](#security-alerts)

- [Podpora balíčku upozornění](#support-package-alerts)

- [Upozornění na aktualizace](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Shluk připojení upozornění

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Připojení k <*cloudu pověření název*> nelze vytvořit.|Nelze se připojit k účtu úložiště.|Vypadá to, může být problém s připojením se svým zařízením. Spusťte `Test-HcsmConnection` rutina z rozhraní systému Windows PowerShell pro StorSimple na vašem zařízení identifikovat a vyřešit problém. Pokud nastavení je v pořádku, může být problém pomocí přihlašovacích údajů účtu úložiště pro které upozornění aktivované. V tomto případě použít `Test-HcsStorageAccountCredential` rutina zjistit, zda jsou problémy, které můžete vyřešit.<ul><li>Zkontrolujte nastavení sítě.</li><li>Zkontrolujte svoje přihlašovací údaje účtu úložiště.</li></ul>|
|Z vašeho zařízení jsme nedostali prezenční signál poslední <*číslo*> minut.|Nelze se připojit ke zařízení.|Vypadá to není problém s připojením se svým zařízením. Stiskněte klávesovou zkratku `Test-HcsmConnection` rutina z rozhraní systému Windows PowerShell pro StorSimple na vašem zařízení identifikovat a vyřešit problém nebo požádejte správce sítě.|

### <a name="storsimple-behavior-when-cloud-connectivity-fails"></a>Chování StorSimple selhání cloudu připojení

Co se stane, když připojení cloudu selže StorSimple zařízení aplikaci výrobní?

Když připojení cloudu nepovede na vašem zařízení výrobní StorSimple, pak v závislosti na vašem zařízení, následující může dojít: 

- **Pro místní data na vašem zařízení**: ve chvíli, bude žádná přerušení a čtení, zůstanou být podávané množství. Však a počet zbývající IOs zvyšuje překračuje limit, čtení by mohl začít nezdaří. 

    Podle objemu dat na vašem zařízení zápisy taky zůstanou dojít kvůli první několik hodin po narušení v cloudu připojení. Zápisy potom zpomalit a nakonec start selže, pokud přerušení propojení cloudu několik hodin. (Je dočasné úložiště v zařízení pro data, která má být vložena do cloudu. Tato oblast vyprázdnění při odeslání data. Pokud se nezdaří připojení data v této oblasti úložiště nebude posune do cloudu a vstupu a výstupu se nezdaří.)   

 
- **Data z cloudu**: chyb většina cloudu připojení, je vrácena chyba. Po obnovení připojení obnovit IOs, aniž by uživatel měl tak, aby objem online. Výjimečně může být potřeba vrátit hlasitost online z portálu Microsoft Azure klasické zásahu uživatele. 
 
- **K snímky mraků průběhu**: operaci opakovat klikejte na do 4-5 hodin a propojení není obnovena, se nepovede snímky cloudu.


### <a name="cluster-alerts"></a>Upozornění na obrázku

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Zařízení se nepodařilo přes <*název zařízení*>.|V režimu údržby je zařízení.|Zařízení se nepodařilo nad vzhledem k zadání nebo ukončení režimu údržby. To je v pořádku a není potřeba žádná akce. Poté, co jste potvrzeno toto oznámení, zrušte ho na stránce upozornění.|
|Zařízení se nepodařilo přes <*název zařízení*>.|Zařízení firmwaru nebo software byl aktualizovali.|Překlopení obrázku vzhledem k aktualizaci jste udělali. To je v pořádku a není potřeba žádná akce. Poté, co jste potvrzeno toto oznámení, zrušte ho na stránce upozornění.|
|Zařízení se nepodařilo přes <*název zařízení*>.|Řadiče byl vypnout nebo restartovat.|Zařízení se nezdařila nad aktivní řadiče byl vypnout nebo restartování správcem. Není nutná žádná akce. Poté, co jste potvrzeno toto oznámení, zrušte ho na stránce upozornění.|
|Zařízení se nepodařilo přes <*název zařízení*>.|Plánované překlopení.|Ověřte, že to byl plánované přepojení. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|
|Zařízení se nepodařilo přes <*název zařízení*>.|Neplánované převzetí.|StorSimple vytvořené pro automatické obnovení z neplánované převzetí služeb při selhání. Pokud se zobrazí velkého počtu tyto upozornění, kontaktujte Microsoft Support.|
|Zařízení se nepodařilo přes <*název zařízení*>.|Příčina jiných/neznámý.|Pokud se zobrazí velkého počtu tyto upozornění, kontaktujte Microsoft Support. Když problém vyřeší, zrušte zaškrtnutí políčka toto upozornění na stránce upozornění.|
|Služba kritické zařízení sestavy stavu jako neúspěšný.|Chyba služby DataPath. |Microsoft Support požádejte o pomoc.|
|Virtuální IP adresa síťové <*dat #*> Sestavy stavu, protože se nezdařila. |Příčina jiných/neznámý. |Někdy dočasné podmínky mohou způsobit následující upozornění. Pokud jde o případ, pak toto oznámení automaticky vymaže se po určité době. Pokud potíže potrvají, obraťte se na Microsoft Support.|
|Virtuální IP adresa síťové <*dat #*> Sestavy stavu, protože se nezdařila.|Název rozhraní: <*dat #*> IP adresu <IP address> nemůžete do online režimu, protože byl zjištěn duplicitní IP adresu v síti. |Přesvědčte se, že je duplicitní IP adresu odebráno ze sítě nebo překonfigurovat rozhraní s jinou IP adresu.|


### <a name="disaster-recovery-alerts"></a>Upozornění na obnovení havárie

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Využití nemůže obnovit všechny možnosti pro tuto službu. Zařízení konfigurační data jsou ve stavu nekonzistentní u některých zařízení.|Nekonzistence data po obnovení havárie.|Šifrovaná data ve službě se nesynchronizuje s, která zařízení. Povolte zařízení <*název zařízení*> ze Správce StorSimple spustit synchronizaci. Použití rozhraní systému Windows PowerShell pro StorSimple pro spuštění `Restore-HcsmEncryptedServiceData` na zařízení <*název zařízení*> rutina poskytující staré heslo jako vstupů pro tuto rutinu obnovit zabezpečovací profil. Spusťte `Invoke-HcsmServiceDataEncryptionKeyChange` rutina aktualizovat šifrovací klíč dat služby. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|


### <a name="hardware-alerts"></a>Hardwarová upozornění

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Hardwarové součásti <*ID součásti*> Sestavy stavu jako <*Stav*>.||Někdy dočasné podmínky mohou způsobit následující upozornění. Pokud ano, toto oznámení automaticky zrušeno za určitou dobu. Pokud potíže potrvají, obraťte se na Microsoft Support.|
|Trpný řadiče funkční.|Trpný (sekundární) zařízení nefunguje.|Zařízení je funkční, ale jednu řadiči nepracuje. Restartujte daného řadiče. Pokud problém nevyřeší, kontaktujte Microsoft Support.|

### <a name="job-failure-alerts"></a>Výstrahy k chybám úloh

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Zálohování <*ID skupiny hlasitost zdroje*> selhalo.|Zálohování úlohy se nezdařila.|Problémy s připojením může bránit zálohování úspěšném dokončení. Pokud jsou žádné problémy s připojením, může dosáhli maximálního počtu zálohy. Odstraňte všechny zálohy, které už nepotřebujete a opakujte. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|
|Klonovat <*záložní prvek zdroje ID*> <*cíl hlasitost pořadová čísla*> se nezdařila.|Klonovat úlohy se nezdařila.|Aktualizace seznamu zálohování potvrďte, že zálohování je pořád platná. Pokud platí zálohování, je možné, že problémy s připojením cloudu brání úspěšném dokončení operace klonovat. Pokud jsou žádné problémy s připojením, pravděpodobně jste dosáhli limit úložiště. Odstraňte všechny zálohy, které už nepotřebujete a opakujte. Poté, co jste provedli odpovídající akce tento problém vyřešit, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|
|Obnovení <*záložní prvek zdroje ID*> selhalo.|Obnovení úlohy se nezdařila.|Aktualizace seznamu zálohování potvrďte, že zálohování je pořád platná. Pokud platí zálohování, je možné, že problémy s připojením cloudu brání úspěšném dokončení operace obnovit. Pokud jsou žádné problémy s připojením, pravděpodobně jste dosáhli limit úložiště. Odstraňte všechny zálohy, které už nepotřebujete a opakujte. Poté, co jste provedli odpovídající akce tento problém vyřešit, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|

### <a name="locally-pinned-volume-alerts"></a>Místně připnuté hlasitost upozornění

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Nepodařilo se vytvořit objemu místní <*název svazku*>.| Vytvoření úloha hlasitost se nezdařila. <*Chybová zpráva odpovídající nezdařilo kódu*>.|Problémy s připojením může bránit úspěšném dokončení operace vytvoření místa. Místně připnuté svazky jsou thickly zřízenou a proces vytváření místo zahrnuje přesahu vrstvené svazky do cloudu. Pokud jsou žádné problémy s připojením, může vyčerpaly místní místo v zařízení. Určení, zda místo existuje v zařízení před opakování operaci.|
|Rozšíření místních hlasitost <*název svazku*> se nezdařila.|Úloha změny hlasitost selže kvůli <*chybová zpráva odpovídající nezdařilo kódu*>.|Problémy s připojením může bránit úspěšném dokončení operace rozšíření hlasitost. Místně připnuté svazky jsou thickly zřízenou a proces rozšíření existující prostor zahrnuje přesahu vrstvené svazky do cloudu. Pokud jsou žádné problémy s připojením, může vyčerpaly místní místo v zařízení. Určení, zda místo existuje v zařízení před opakování operaci.|
|Převod objemu <*název svazku*> se nezdařil.|Úlohy převodu hlasitost převést typ hlasitost z místně připnuté vrstveny selhalo.|Převod objemu z typu místně připnuté vrstveny nelze dokončit. Ujistěte se, že jsou žádné problémy s připojením zabránění operaci úspěšnému dokončení. Poradce při potížích s problémy s připojením v tématu [Poradce při potížích s rutinu Test HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Původní místně připnuté hlasitost teď byl dokument označen jako vrstvené hlasitost od některá data z místně připnuté hlasitost má uniknout do cloudu během převodu. Výsledné vrstvené hlasitost je pořád zabírá místní místo na zařízení, které nelze znovu použít pro budoucí místní svazky.<br>Připojení problémy, zrušte upozornění a převeďte svazku zpátky na původní typ místně připnuté hlasitost zajistit všechna data je k dispozici místně znovu.|
|Převod objemu <*název svazku*> se nezdařil.|Úlohy převodu hlasitost převést typ hlasitost z vrstveny místně připnuté se nezdařila.|Převod objemu z typu vrstveny k místně připnuté nelze dokončit. Ujistěte se, že jsou žádné problémy s připojením zabránění operaci úspěšnému dokončení. Poradce při potížích s problémy s připojením v tématu [Poradce při potížích s rutinu Test HcsmConnection](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-test-hcsmconnection-cmdlet).<br>Původní vrstvené hlasitost teď označen jako místně připnuté hlasitost jako součást procesu převodu má nadále dat umístěné v cloudu, zatímco thickly zřizování místo na zařízení pro tento hlasitost můžete už navrácená pro budoucí místní svazky.<br>Problémy připojení zrušte upozornění a převést svazku zpátky na původní typ vrstvené hlasitost zajistit místní místo thickly zřízení na zařízení můžete znovu použít.|
|Blíží spotřebu místním prostoru pro místní snímky <*název skupiny hlasitosti*>|Místní snímky záložní zásady může brzo dostatek místa a zruší Chcete-li předejít chyby zápisu Host (hostitel).|Časté místní snímky vedle konve vysoké dat v svazky přidružené k této skupině zásady zálohování způsobují místní místa na zařízení, které má být spotřebované množství rychle. Odstraňte místní snímky, které už nepotřebujete. Taky aktualizujte místní snímek plány u této zásady zálohování méně častých místní snímky a k snímky cloudu braly pravidelně. Pokud tyto akce nejsou dostali, místní prostoru pro tyto snímky může brzy vyčerpány a systém automaticky odstraní, aby zajištění hostitele zápisy úspěšně zpracovat.|
|Místní snímky <*název skupiny hlasitost*> byla ukončena jejich platnost.|Místní snímky <*název skupiny hlasitost*> byly ukončena jejich platnost a pak odstranit, protože byly překročení místní místo v zařízení.|Abyste měli jistotu, že to není opakování v budoucnu, zkontrolujte místní snímek plány u této zásady zálohování a odstranit místní snímky, které už nepotřebujete. Časté místní snímky vedle konve vysoké dat v svazky přidružené k této zásady zálohování skupiny může způsobit místní místa na zařízení, které má být spotřebované množství rychle.|
|Obnovení <*záložní prvek zdroje ID*> selhalo.|Obnovení úloha se nezdařila.|Pokud máte místní připnuté nebo kombinaci místně připnutých a vrstvené svazky v této zásady zálohování aktualizace seznamu zálohování potvrďte, že zálohování je pořád platná. Pokud platí zálohování, je možné, že problémy s připojením cloudu brání úspěšném dokončení operace obnovit. Místně připnuté svazky obnovených jako součást této skupině snímek nemají všech svých dat stáhnout do zařízení a pokud máte kombinaci vrstvené a místně připnuté svazky v této skupině snímku, nebudou synchronizace mezi sebou. K úspěšnému dokončení obnovení, prohlédněte svazky v této skupině offline v hostiteli a opakujte obnovit. Všimněte si, že budou ztraceny všechny změny, které byly provedeny během obnovení dat hlasitost.|

### <a name="networking-alerts"></a>Upozornění sítě

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Nelze zahájit StorSimple služeb.|Chyba DataPath |Pokud potíže potrvají, obraťte se na Microsoft Support.|
|Duplicitní IP adresu detekovaný pro "Data0".| |Systém zjistil konflikt IP adresu "10.0.0.1". Zdroje sítě "Data0" na zařízení *<device1>* je offline. Ujistěte se, že tuto IP adresu se nepoužívá jiný subjekt v této sítě. Poradce při potížích s problémům se sítí, přejděte na [Poradce při potížích s rutinu Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). Pro pomoc s vyřešením problému požádejte správce sítě. Pokud potíže potrvají, obraťte se na Microsoft Support. |
|Adresy IPv4 (nebo IPv6) "Data0" je offline.| |Zdroje sítě "Data0" s adresou IP "10.0.0.1." a délka "22" na zařízení předpony *<device1>* je offline. Zajištění provozní přepnout porty, ke kterým je připojen rozhraní. Poradce při potížích s problémům se sítí, přejděte na [Poradce při potížích s rutinu Get-NetAdapter](storsimple-troubleshoot-deployment.md#troubleshoot-with-the-get-netadapter-cmdlet). |
 

### <a name="performance-alerts"></a>Upozornění na výkon

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Načíst zařízení překročila <*mezní*>.|Nižší než očekávané doby odezvy.|Zařízení s sestavy využití při velkém zatížení vstupní a výstupní. To může způsobit zařízení nebudou fungovat stejně, jako je. Prohlédněte si úloh připojit do zařízení a zjistit, zda jsou k dispozici, který by mohl přesunuli do jiné zařízení nebo, které jsou už není potřeba.<br>Pokud chcete zjistit aktuální stav, přejděte na článek [použití službu StorSimple správce ke sledování zařízení](storsimple-monitor-device.md)|

### <a name="security-alerts"></a>Upozornění zabezpečení

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Microsoft Support relace zahájil.|Třetích stran získat přístup k relaci podpory.|Ověřte, zda že je povoleno tento přístup. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|
|<*Časový úsek*> vyprší platnost hesla pro <*prvek*>.|Blíží se vypršení platnosti hesla.|Změna hesla před vypršením jeho platnosti.|
|Informace o konfiguraci zabezpečení pro <*ID prvku*> chybí.||Svazky přidružené k tento kontejner hlasitost se nedají použít k replikovat StorSimple konfiguraci. Abyste měli jistotu, že vaše data se ukládají bezpečně, doporučujeme odstranit kontejneru hlasitost a svazky přidružené kontejneru hlasitost. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|
|<*číslo*> pokus o přihlášení se nepodařilo <*ID prvku*>.|Více neúspěšné pokusy o přihlášení.|Zařízení můžou být napadení nebo ověřený uživatel pokoušející se připojit pomocí nesprávného hesla.<ul><li>Kontaktujte oprávnění uživatelů a ověřte, že tyto pokusy byly legitimní zdrojem. Pokud budete pokračovat zobrazíte velkého množství neúspěšný pokus o přihlášení, zvažte zakázání vzdálená správa a jak kontaktovat správce sítě. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.</li><li>Kontrola snímek správce instance budou nakonfigurována správné heslo. Poté, co jste provedli odpovídající akce, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.</li></ul>Další informace přejděte k části [Změna hesla vypršela platnost zařízení](storsimple-snapshot-manager-manage-devices.md#change-an-expired-device-password).|
|Při změně šifrovací klíč dat služby došlo k jedné nebo více k chybám.||Došlo k chybám při změně šifrovací klíč dat služby došlo. Po vyřešili chybové podmínky spustit `Invoke-HcsmServiceDataEncryptionKeyChange` rutina z rozhraní systému Windows PowerShell pro StorSimple na vašem zařízení aktualizovat službu. Pokud problém přetrvává, kontaktujte podporu od Microsoftu. Po tento problém vyřešit, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|

### <a name="support-package-alerts"></a>Podpora balíčku upozornění

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Vytvoření balíčku podpory se nezdařila.|StorSimple nelze generovat balíček.|Opakujte. Pokud potíže potrvají, obraťte se na Microsoft Support. Po problém nevyřešil, zrušte zaškrtnutí políčka Tento upozornění na stránce upozornění.|

### <a name="update-alerts"></a>Upozornění na aktualizace

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Oprava hotfix.|Aktualizace softwaru/firmwaru dokončit.|Hotfix byl úspěšně nainstalovaný na zařízení.|
|Ruční aktualizace k dispozici.|Oznámení o dostupné aktualizace.|Pomocí rozhraní systému Windows PowerShell pro StorSimple na vašem zařízení nainstalovat tyto aktualizace. <br>Další informace přejděte na [Aktualizovat řady 8000 StorSimple zařízení](storsimple-update-device.md).|
|Nové aktualizacemi.|Oznámení o dostupné aktualizace.|Na stránce **správy** nebo pomocí rozhraní systému Windows PowerShell pro StorSimple na vašem zařízení, můžete nainstalovat tyto aktualizace. <br>Další informace přejděte na [Aktualizovat řady 8000 StorSimple zařízení](storsimple-update-device.md).|
|Nepodařilo se nainstalovat aktualizace.|Aktualizace neinstalovali úspěšně.|Systému se nepodařilo nainstalovat aktualizace. Na stránce **správy** nebo pomocí rozhraní systému Windows PowerShell pro StorSimple na vašem zařízení, můžete nainstalovat tyto aktualizace. Pokud potíže potrvají, obraťte se na Microsoft Support. <br>Další informace přejděte na [Aktualizovat řady 8000 StorSimple zařízení](storsimple-update-device.md).|
|Nelze automaticky zjišťovat nové aktualizace.|Automatická kontrola se nezdařila.|Můžete vyhledat o nových aktualizacích ze stránky **údržby** .|
|Nový agent WUA k dispozici.|Oznámení o dostupné aktualizace.|Stáhněte si nejnovější služba Windows Update Agent a nainstalujte si ho z rozhraní prostředí Windows PowerShell.|
|Verze součásti firmware <*ID součásti*> neshoduje s hardwarem.|Firmware aktualizací neinstalovali úspěšně.|Kontaktujte podporu od Microsoftu.|

## <a name="next-steps"></a>Další kroky

Další informace o [StorSimple chyby a řešení problémů s provozní zařízení](storsimple-troubleshoot-operational-device.md).

