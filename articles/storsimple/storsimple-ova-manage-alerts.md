<properties 
   pageTitle="Zobrazení a správa StorSimple virtuální pole upozornění | Microsoft Azure"
   description="Popisuje virtuální pole StorSimple upozornění podmínky a závažnosti a jak používat službu StorSimple správce ke správě upozornění."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="06/07/2016"
   ms.author="alkohli" />

# <a name="use-the-storsimple-manager-service-to-view-and-manage-alerts-for-the-storsimple-virtual-array"></a>K zobrazení a Správa oznámení pro pole virtuální StorSimple použijte službu StorSimple správce

## <a name="overview"></a>Základní informace

Na kartu **výstrahy** ve službě StorSimple správce poskytují způsob, jak můžete zkontrolovat a zrušte související s StorSimple virtuální pole upozornění na základě v reálném čase. Na kartě můžete problémy stavu virtuální matice StorSimple (označovaná taky jako StorSimple místní virtuální zařízení) a celkové řešení Microsoft Azure StorSimple centrálně sledování.

Tento kurz popisuje jak nakonfigurovat oznámení, společné upozornění podmínky, úrovně upozornění závažnosti a jak zobrazit a sledovat upozornění. Dále obsahuje upozornění stručný přehled tabulek, které umožňují rychle najít konkrétní upozornění a reagovat.

![Upozornění na stránku](./media/storsimple-ova-manage-alerts/alerts1.png)

## <a name="configure-alert-settings"></a>Konfigurace nastavení oznámení

Můžete zvolit, jestli má být odesláno oznámení e-mailem upozornění podmínky pro jednotlivá pole StorSimple virtuální zařízení. Kromě toho můžete identifikujete ostatními příjemci oznámení zadáním jejich e-mailové adresy do pole **ostatním příjemcům e-mailu** , oddělené středníkem.

>[AZURE.NOTE] Můžete vytvořit maximálně 20 e-mailové adresy za virtuální zařízení.

Po povolení e-mailového oznámení pro virtuální zařízení členů seznamu oznámení, dostanou e-mailové zprávy důležité upozornění při každém výskytu. Odešle zprávy z *storsimple-alerts-noreply@mail.windowsazure.com* a bude popisují upozornění podmínky. Příjemci můžete kliknout na **Odhlásit odběr** sami odebrat ze seznamu e-mailových oznámení.

#### <a name="to-enable-email-notification-of-alerts-for-a-virtual-device"></a>Chcete-li povolit e-mailového oznámení upozornění pro virtuální zařízení

1. Přejděte na **zařízení** > **Konfigurace** virtuální zařízení. Přejděte k části **Nastavení upozornění** .

    ![Nastavení upozornění](./media/storsimple-ova-manage-alerts/alerts2.png)

2. V části **Nastavení upozornění**nastavte takto:

    1. V poli **Odeslat e-mailového oznámení** vyberte **Ano**.

    2. V poli **Správci služeb e-mailu** vyberte **Ano** , pokud chcete mít má správce služby a všech dalších správců oznámení hlášení.

    3. V poli **ostatním příjemcům e-mailu** zadejte e-mailové adresy všech příjemců, kteří mají dostávat oznámení hlášení. Zadejte jména ve formátu *someone@somewhere.com*. Pomocí středníků oddělit e-mailové adresy. Můžete nakonfigurovat maximálně 20 e-mailové adresy za virtuální zařízení. 

        ![Konfigurace oznámení upozornění](./media/storsimple-ova-manage-alerts/alerts3.png)

3. V dolní části stránky klikněte na **Uložit** uložte konfiguraci.

4. Odeslání zkušební e-mailového oznámení, klikněte na šipku vedle **Odeslání zkušební e-mailu**. Služba StorSimple správce zobrazí zprávy o stavu jako předává test oznámení. 

5. Pokud se zobrazí následující zpráva, klikněte na **OK**. 

    ![Upozornění na testování odeslané e-mailových oznámení](./media/storsimple-ova-manage-alerts/alerts4.png)

    >[AZURE.NOTE] Pokud není odeslat zprávu oznámení o zkušební, službu StorSimple správce zobrazí příslušnou zprávu. Klikněte na **OK**, počkejte pár minut a zkuste znovu odeslat zprávu oznámení o zkušební. 

    Oznámení test bude vypadat podobně jako následující.

    ![Příklad test e-mailových upozornění](./media/storsimple-ova-manage-alerts/alerts5.png)

## <a name="common-alert-conditions"></a>Společné upozornění podmínky

Virtuální pole vaše StorSimple vygeneruje upozornění v odpovědi na řadu podmínky. Následují nejběžnější typy upozornění podmínek:

- **Problémy s připojením** – tyto upozornění vyskytnout, když problémy s přenosem zpráv data. Komunikace problémů může dojít během převodu dat do a z Azure úložiště účtu nebo z důvodu nedostupnosti virtuální zařízení a služba StorSimple správce. Problémy s komunikace jsou uvedeny některé nejtěžší problém odstranit: vzhledem k tomu, že je tolik bodů selhání. Vždycky nejdřív zkontrolujte, zda připojení k síti přístup k Internetu k dispozici před pokračováním k pokročilejší Poradce při potížích. Informace o porty a protokoly nastavení brány firewall přejděte do [pole virtuální StorSimple systémové požadavky](storsimple-ova-system-requirements.md). Pomoc s Poradce při potížích s projděte si téma [Poradce při potížích s rutinu testovat připojení](storsimple-troubleshoot-deployment.md).

- **Problémy s výkonem** – tyto upozornění dochází při systému není optimálně, například při velkém zatížení.

Kromě toho se může zobrazit upozornění týkající se zabezpečení, aktualizace nebo chyby úlohy.

## <a name="alert-severity-levels"></a>Úrovně upozornění závažnosti

Upozornění mají různé úrovně závažnosti, podle toho, vliv bude mít upozornění situaci a potřebné pro odpovědi na upozornění. Úrovně závažnosti jsou:

- **Pole Kritický** – toto oznámení je v odpovědi podmínce, která má vliv na úspěšné výkonu systému. Akce je nezbytné k zajištění StorSimple není přerušení služby.

- **Varování** – tato situace může být za kritický, pokud není vyřešit. Měli byste prošetřit situaci a provádět žádnou akci povinné zrušte problém.

- **Informace o** – toto oznámení obsahuje informace, které mohou být užitečné při sledování a správa systému.

## <a name="view-and-track-alerts"></a>Zobrazení a sledování upozornění

Řídicí panel služeb StorSimple správce obsahuje stručný přehled počet upozornění na virtuální zařízení, uspořádány podle úrovně závažnosti.

![Řídicí panel upozornění](./media/storsimple-ova-manage-alerts/alerts6.png)

Kliknutím na tlačítko úroveň závažnosti otevřete kartu **výstrahy** . Výsledky obsahovat pouze upozornění, které odpovídají dané závažnosti úrovni.

![Upozornění na zprávu obor vymezený na typ oznámení](./media/storsimple-ova-manage-alerts/alerts7.png)

Kliknutí na upozornění v seznamu umožňuje další podrobnosti pro upozornění, včetně posledního nahlášených upozornění, počet výskytů upozornění na zařízení a doporučené akci, kterou chcete vyřešit upozornění.

Můžete zkopírovat podrobnosti výstrahy k textovému souboru v případě potřeby odeslat informace společnosti Microsoft Support. Po a až pak doporučení a vyřešit upozornění podmínka místní byste měli zakázat upozornění od výběrem upozornění na kartě **upozornění** a kliknutím na **Vymazat**. Zrušte několik upozornění, vyberte každý upozornění, klikněte na všechny sloupce kromě sloupci **upozornění** a potom klikněte na **Vymazat** po výběru všechna upozornění na Vymazat. Všimněte si, že některé upozornění vymažou automaticky při problém vyřeší nebo systému aktualizuje upozornění na nové informace.

Když kliknete na tlačítko **Vymazat**, máte možnost názor týkající se upozornění a kroky, které jste pořídili pomocí tento problém vyřešit. 

![upozornění komentáře](./media/storsimple-ova-manage-alerts/clear-alert.png)

Klikněte na ikonu zaškrtnutí ![Ikona kontroly](./media/storsimple-ova-manage-alerts/check-icon.png) Uložte svůj komentář.

Některé události vymaže se systémem Pokud jiné události se při spuštění aktivují novými informacemi. V takovém případě zobrazí se tato zpráva.

![Zrušte zaškrtnutí políčka výstrahy zabezpečení](./media/storsimple-ova-manage-alerts/alerts8.png)

## <a name="sort-and-review-alerts"></a>Řazení a revize upozornění

Na kartě **upozornění** můžete zobrazit až 250 upozornění. Pokud překročíte určeným počtem upozornění, zobrazí se ve výchozím zobrazení není všechna upozornění. Můžete kombinovat následující pole můžete přizpůsobit zobrazení upozornění:

- **Stav** – můžete zobrazit **aktivní** nebo **zúčtováno** upozornění. Aktivní upozornění se stále aktivují v počítači, zatímco nezaškrtnuté upozornění byly buď ručně zrušeno správcem nebo programově vymazat, protože systému aktualizaci upozornění podmínka novými informacemi.

- **Závažnosti** – můžete zobrazit upozornění na všechny úrovně závažnosti (kritické, upozornění, informace o) nebo jenom určité závažnosti, například jenom kritické upozornění.

- **Zdroj** – můžete zobrazit oznámení ze všech zdrojů nebo omezit upozornění na ty, které jsou z jedné nebo všech virtuální zařízení či služby.

- **Časový rozsah** – zadáním **od** a **do** data a časových razítek, můžete si může samostatně prohlížet upozornění časového období, která vás zajímá.

## <a name="alerts-quick-reference"></a>Stručný přehled upozornění

Následující tabulka uvádí některé z Microsoft Azure StorSimple upozornění, ke kterým může dojít, a dalších informací a doporučení-li k dispozici. Virtuální zařízení upozornění StorSimple spadají do jedné z těchto kategorií:

- [Shluk připojení upozornění](#cloud-connectivity-alerts)

- [Konfigurace upozornění](#configuration-alerts)

- [Výstrahy k chybám úloh](#job-failure-alerts)

- [Upozornění na výkon](#performance-alerts)

- [Upozornění zabezpečení](#security-alerts)

- [Upozornění na aktualizace](#update-alerts)

### <a name="cloud-connectivity-alerts"></a>Shluk připojení upozornění

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Zařízení *<device name>* není připojený ke cloudu.|Pojmenovaná zařízení se nemůže připojit ke cloudu. |Nelze se připojit ke cloudu. Může to být kvůli jednu z následujících akcí:<ul><li>Může být problém s nastavení sítě na vašem zařízení.</li><li>Může být problém s přihlašovací údaje účtu úložiště.</li></ul>Další informace o řešení potíží s problémy s připojením přejděte na [místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md) zařízení.|


### <a name="configuration-alerts"></a>Konfigurace upozornění

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Místní konfigurace virtuální zařízení není podporován.|Snížení výkonu.|Aktuální konfigurace může způsobit snížení výkonu. Ujistěte se, že váš server splňuje požadavky na minimální konfigurace. Další informace přejděte na [StorSimple virtuální pole požadavky](storsimple-ova-system-requirements.md). 
|Jsou dost místa na disku zřizování na <*název zařízení*>.|Upozornění místa na disku.|Jsou dostatek místa na disku zřizování. Abyste uvolnili místo, zvažte přesunutí pracovního vytížení do jiného hlasitost nebo sdílet nebo odstraňovat data.

### <a name="job-failure-alerts"></a>Výstrahy k chybám úloh

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Zálohování <*název zařízení*> nelze dokončit.|Chyba při úlohy zálohování.|Nelze vytvořit zálohu. Zkuste jednu z následujících akcí:<ul><li>Problémy s připojením může bránit zálohování úspěšném dokončení. Ujistěte se, že nejsou žádné problémy s připojením. Další informace o řešení potíží s problémy s připojením přejděte na [místní web uživatelského rozhraní](storsimple-ova-web-ui-admin.md) pro virtuální zařízení.</li><li>Dosáhli limit úložiště k dispozici. Abyste uvolnili místo, zvažte možnost odstranění zálohy, které už nepotřebujete.</li></ul> Vyřešení problémů, zrušte na upozornění a opakujte.|
|Obnovení <*název zařízení*> nelze dokončit.|Obnovení selhání projektu.|Nelze obnovení ze zálohy. Zkuste jednu z následujících akcí:<ul><li>Zálohování seznamu nemusí být platná. Aktualizace seznamu a ověřte, jestli že je pořád platná.</li><li>Problémy s připojením může bránit úspěšném dokončení operace obnovit. Ujistěte se, že nejsou žádné problémy s připojením.</li><li>Dosáhli limit úložiště k dispozici. Abyste uvolnili místo, zvažte možnost odstranění zálohy, které už nepotřebujete.</li></ul>Řešení problémů, zrušte upozornění a opakujte obnovení.|
|Klonovat <*název zařízení*> nelze dokončit.|Klonovat selhání projektu.|Nevytvořila kopií. Zkuste jednu z následujících akcí:<ul><li>Zálohování seznamu nemusí být platná. Aktualizace seznamu a ověřte, jestli že je pořád platná.</li><li>Problémy s připojením může bránit úspěšném dokončení operace klonovat. Ujistěte se, že nejsou žádné problémy s připojením.</li><li>Dosáhli limit úložiště k dispozici. Abyste uvolnili místo, zvažte možnost odstranění zálohy, které už nepotřebujete.</li></ul>Vyřešení problémů, zrušte na upozornění a opakujte.|

### <a name="performance-alerts"></a>Upozornění na výkon

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Jste zaznamenali neočekávané zpožděním při převodu dat.|Převod pomalé data.|Omezení chyby při překročit škálovatelnost cílů služba úložiště. Služba úložiště dělá zajistit, že žádný jednoho klienta nebo klienta může používat službu na ostatní. Další informace o odstraňování potíží se váš účet Azure úložiště přejděte na [Monitor, Diagnostika a řešení potíží s úložišti tabulek Microsoft Azure](storage-monitoring-diagnosing-troubleshooting.md).
|Používáte zhoršeným místní rezervace místa na disku na <*název zařízení*>.|Čas pomalé odpověď.|vypočítá 10 % celkového zřizování velikost <*název zařízení*> je rezervován na místním zařízením a je teď málo na prázdné místo mezi rezervovaná. Pracovní zátěž na <*název zařízení*> vygenerovala vyšší sazbu konve nebo může provedení naposledy migrace velké množství dat. Výsledkem může být nižší výkon. Zkuste jednu z následujících akcí problém můžete vyřešit:<ul><li>Zvětšení šířky pásma cloudu na tomto zařízení.</li><li>Zmenšení nebo přesuňte pracovního vytížení do jiného hlasitost nebo sdílet.</li></ul>

### <a name="security-alerts"></a>Upozornění zabezpečení

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Ve dnech <*číslo*> vyprší platnost hesla pro <*název zařízení*>.|Heslo upozornění.| Do vypršení platnosti hesla < číslo < dnů. Zvažte možnost svoje heslo. Další informace přejděte k části [Změna hesla správce zařízení StorSimple virtuální pole](storsimple-ova-change-device-admin-password.md).

### <a name="update-alerts"></a>Upozornění na aktualizace

|Text upozornění|Události|Další informace a doporučené akce|
|:---|:---|:---|
|Nové aktualizace jsou k dispozici pro svoje zařízení.|Aktualizace pole virtuální StorSimple jsou k dispozici.|Instalovat aktualizace z stránky **údržby** .|
|Nelze vyhledat nových aktualizacích v blogu o <*název zařízení*>.|Aktualizujte selhání. |Došlo k chybě při instalaci nových aktualizací. Můžete ručně instalovat aktualizace. Pokud problém přetrvává, kontaktujte [Podporu od Microsoftu](storsimple-contact-microsoft-support.md).|


## <a name="next-steps"></a>Další kroky

- [Další informace o maticových virtuální StorSimple](storsimple-ova-overview.md).


