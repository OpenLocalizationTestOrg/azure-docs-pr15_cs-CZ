
<properties
    pageTitle="Pomocí stavu připojení Azure AD sync | Microsoft Azure"
    description="Tohle je stránka stavu připojení Azure AD, který bude diskutovat o tom, jak sledovat Azure AD Connect synchronizace."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Použití stavu připojení Azure AD pro synchronizaci
Následující dokumentaci si specifické pro sledování Azure AD Connect (synchronizace) s Azure AD stavu připojení.  Informace o sledování AD FS stavu připojení Azure AD najdete v článku [Použití Azure AD připojení stavu se službou AD FS](active-directory-aadconnect-health-adfs.md). Kromě toho informace týkající se sledování Active Directory Domain Services s stavu připojení Azure AD najdete v článku [Stavu připojení pomocí Azure AD pomocí služby AD DS](active-directory-aadconnect-health-adds.md).

![Azure AD Connect stav synchronizace](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Upozornění pro Azure AD připojení stav synchronizace
Oddílu Azure AD připojení stavu upozornění pro synchronizaci obsahuje seznam aktivní upozornění. Každý upozornění obsahuje příslušné informace, kroků řešení a odkazy na související si přečtěte následující dokumentaci. Výběrem aktivní nebo přeložena upozornění uvidíte nový zásuvné se další informace, jakož i kroky, kterými můžete vyřešit upozornění a odkazy na další dokumentaci. Můžete taky zobrazit historických dat na upozornění, které byly přeložit v minulosti.

Výběrem upozornění, které jste se dozvíte další informace, jakož i kroků může trvat přeložit upozornění a odkazy na další si přečtěte následující dokumentaci.

![Chyby synchronizace Azure AD Connect](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Omezené hodnocení upozornění
Pokud nepoužíváte Azure AD Connect je výchozí konfigurace (například pokud atribut filtrování se změnil z výchozí konfigurace vlastní konfiguraci), nebude agent stavu připojení Azure AD nahrát události chyb souvisejících s Azure AD Connect.

Toto omezení hodnocení upozornění službou. Zobrazí by se nápis označující tuto podmínku v portálu Azure v rámci služby.

![Azure AD Connect stav synchronizace](./media/active-directory-aadconnect-health-sync/banner.png)

Můžete změnit kliknutím na "Nastavení" a povolení agent stavu připojení Azure AD na protokoly chyb.

![Azure AD Connect stav synchronizace](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Přehled synchronizace
Správci často informací o časové náročnosti synchronizovat změny Azure AD a počtu změny probíhá. Tato funkce umožňuje snadný způsob, jak to vizualizovat, pomocí pod grafy:   

- Latence operací synchronizace
- Trend změn objektu

### <a name="sync-latency"></a>Latence synchronizace
Tato funkce umožňuje grafické trend latence operací synchronizace (import, export atd.) pro spojnice.  To umožňuje rychlý a snadný způsob, jak porozumět nejen latence operace (větší, pokud máte velký sadu změny, které se vyskytují), ale také způsob, jak zjistit odchylky v latence, které může vyžadovat další vyšetřování.

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Ve výchozím nastavení se zobrazuje jenom latence ' "exportu konektoru Azure AD.  Pokud chcete zjistit další operace u spojnice nebo zobrazíte operace z jiných spojovací čáry, klikněte pravým tlačítkem myši na graf, vyberte upravit graf nebo klikněte na tlačítko "Upravit latence grafu" a zvolte konkrétní operace a spojnice.

### <a name="sync-object-changes"></a>Synchronizace změn objektu
Tato funkce umožňuje grafické trend počet změny, které jsou vyhodnoceny a exportovat do Azure AD.  Snažíte shromáždění tyto informace z protokolů synchronizace je v současné době obtížně srozumitelný.  Graf vám nejen jednodušší způsob sledování počtu změny, které se vyskytnou ve vašem prostředí, ale taky vizuální zobrazení, které dochází k chybám.

![Latence synchronizace](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Objekt zprávu o chybách synchronizace úrovně (verze Preview)
Tato funkce umožňuje zprávu o chybách synchronizace, které může dojít při synchronizaci identity dat mezi systému Windows Server AD a Azure AD pomocí Azure AD Connect.

- Sestava obsahuje chyby zaznamenal synchronizačního klienta (Azure AD Connect verze 1.1.281.0 nebo novější)
- V posledním operace synchronizace na modul synchronizace obsahuje chyby, ke kterým došlo. ("Export" v Azure AD spojnice.)
- Azure AD připojení stavu agent pro synchronizaci musí mít odchozí připojení na požadované koncové body sestavy zahrnout nejnovější data. Podrobnosti najdete [v části požadavky](active-directory-aadconnect-health-agent-install.md#Requirements) .
- Sestava je **aktualizované po každých 30 minut** používat data odeslal agent stavu připojení Azure AD pro synchronizaci.
Poskytuje následující klíčové funkce

    - Třídění chyb
    - Seznam objektů, zobrazí se chyba podle kategorií
    - Všechna data o chybách na jednom místě
    - Sebe ve srovnání objektů s chybou kvůli konflikt
    - Stažení chybové zprávy jako CVS (už brzo)

### <a name="categorization-of-errors"></a>Třídění chyb
Sestava rozdělíte do kategorií existující chybami synchronizace v těchto kategorií:

| Kategorie | Popis |
| -------------- | ----------- |
| Duplicitní atribut | Chyby při Azure AD Connect pokusí vytvoření nebo aktualizace objekty s duplicitními hodnotami jeden nebo více atributů v Azure AD, které musí být jedinečný v klientovi, jako je proxyAddresses UserPrincipalName. |
| Neshoda dat | Chyby při selže měkkých POZVYHLEDAT při porovnávání objekty, které docházet k chybám synchronizace. |
| Chyba ověřování dat | Chyby kvůli neplatných dat, jako jsou třeba nepodporované znaky kritické atributy například UserPrincipalName, formátovat chyby neúspěšné ověření před napsané v Azure AD.|
| Velké atribut | Chyby při jeden nebo více atributů jsou větší než povolená velikost, délka nebo count.|
| Další | Všechny ostatní chyby, které nevyhovují výše uvedených kategorií. Na základě reakcí, bude tato kategorie rozdělen do sub kategorií.

![Sestava Souhrn chyby synchronizace](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![synchronizovat kategorie chyb sestav](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Seznam objektů, zobrazí se chyba podle kategorií
Procházení kategoriemi poskytne seznamu objektů s chybu v této kategorii.
![Seznam zpráv chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Podrobnosti o chybě
Sledování dat je k dispozici v zobrazení Podrobný pro každou chybu

- Identifikátory *AD objekt* souvisejících
- Identifikátory *Azure AD objekt* souvisejících (podle potřeby)
- Popis chyby a řešení
- Související články

![Podrobnosti sestavy chyb synchronizace](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Stažení chybové zprávy jako souboru CSV
Tato možnost je brzy k dispozici. Zůstaňte sestavenou další aktualizace.



## <a name="related-links"></a>Související odkazy
* [Odstraňování chyb při synchronizaci](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Duplikování odolnost proti chybám atribut](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure AD Connect stavu](active-directory-aadconnect-health.md)
* [Azure AD Connect instalaci agenta stavu](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect stavu operace](active-directory-aadconnect-health-operations.md)
* [Použití Azure AD stavu připojení se službou AD FS](active-directory-aadconnect-health-adfs.md)
* [Pomocí Azure AD stavu připojení se službou AD DS](active-directory-aadconnect-health-adds.md)
* [Azure AD Connect stavu časté otázky](active-directory-aadconnect-health-faq.md)
* [Azure AD Connect historie verzí stavu](active-directory-aadconnect-health-version-history.md)
