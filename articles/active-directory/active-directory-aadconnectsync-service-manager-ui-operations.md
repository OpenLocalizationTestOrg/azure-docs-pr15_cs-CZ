<properties
    pageTitle="Azure AD Connect synchronizace: synchronizace služby UI | Microsoft Azure"
    description="Vysvětlení informací na kartě operace v okně Správce služeb synchronizace pro Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="billmath"/>


# <a name="azure-ad-connect-sync-synchronization-service-manager"></a>Azure AD Connect synchronizace: Správce služby synchronizace

[Operace](active-directory-aadconnectsync-service-manager-ui-operations.md) | [Spojnice](active-directory-aadconnectsync-service-manager-ui-connectors.md) | [Návrhář Metaverse](active-directory-aadconnectsync-service-manager-ui-mvdesigner.md) | [Hledání Metaverse](active-directory-aadconnectsync-service-manager-ui-mvsearch.md)
--- | --- | --- | ---

![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/operations.png)

Kartu operace zobrazuje výsledky posledních operace. Tato karta je klíčová k pochopení a řešení potíží.

## <a name="understand-the-information-visible-in-the-operations-tab"></a>Pochopit informace zobrazené na kartě operace
Horní polovině zobrazí všechny se spustí v chronické pořadí. Ve výchozím nastavení protokol operace obsahuje informace o posledních sedmi dnů, ale toto nastavení můžete změnit pomocí [Plánovač](active-directory-aadconnectsync-feature-scheduler.md). Chcete-li vyhledat spustit, který nezobrazuje stav úspěch. Můžete změnit řazení kliknutím záhlaví.

Ve sloupci **Stav** je nejdůležitější informace a zobrazí nejčastěji špatných problém ke spuštění. Tady je rychlý přehled nejběžnější stavy v pořadí priorit prozkoumat (kde * označení několik možných chybové řetězce).

Stav | Komentář
--- | ---
zastaveno-* | Spustit nelze dokončit. Pokud například vzdálený systém není k dispozici a nelze kontaktovat.
Přerušili limit chyby | Obsahuje víc než 5 000 chyby. Spustit automaticky zastavil kvůli velký počet chyb.
dokončení -\*-chyby | Spustit dokončený, ale došlo k chybám (méně než 5 000), které je třeba se zabývat.
dokončení -\*-upozornění | Spustit dokončený, ale některé dat není v očekávané stavu. Pokud máte chyby, tato zpráva je obvykle jenom příznakem. Dokud vyřešili chyby neměli prozkoumejte upozornění.
Úspěch | Bez problémů.

Po výběru řádku dolní aktualizace a zobrazit podrobnosti o spuštění. Na levém okraji dolní bude pravděpodobně nutné seznam sdělují **kroku #**. Tento seznam se zobrazí, pouze pokud máte víc domén v doménové kde každá doména představované kroku. Název domény najdete v části nadpis **oddílu**. V části **Statistiky synchronizace**najdete další informace o počtu změny, které byly zpracovat. Klikněte na odkazy na zobrazte seznam změněné objekty. Pokud máte objekty s chybami, tyto chyby zobrazit v části **Chyb synchronizace**.

## <a name="troubleshoot-errors-in-operations-tab"></a>Poradce při potížích ve kartu operace
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/errorsync.png)  
Když budete mít chyby, objekt v chyby a vlastní chyba jsou odkazy, které poskytuje další informace.

Začněte tím, že kliknete na řetězce chyb (**synchronizace pravidlo – chyby – funkce spouštěný** na obrázku). Nejdřív se zobrazí základní informace o objektu. Pokud chcete zobrazit skutečnou chybovou, klikněte na tlačítko **Zásobníku**. Trasování obsahuje informace o úrovni ladění chyby.

**TIP:** Můžete klikněte pravým tlačítkem myši do pole **volání zásobníku informace** , zvolte **Vybrat vše**a **Kopírovat**. Můžete pak zkopírujte zásobníku a podívejte se na chybu v seznamu Oblíbené editor, například programu Poznámkový blok.

- Pokud je chyba z **SyncRulesEngine**, pak zásobníku volání informace nejdřív obsahuje seznam všech atributů na požadovaném objektu. Přejděte dolů, dokud se nezobrazí záhlaví **InnerException = >**.  
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/errorinnerexception.png)  
Čára po zobrazuje chybu. Na obrázku nahoře chyba je z vlastní Fabrikam pravidlo synchronizace vytvořili.

Pokud vlastní chyba neposkytuje dost informací, bude času můžete visiové samotná data. Klikněte na odkaz s identifikátor objektu a [postupujte podle objektu a jeho data celém systému](active-directory-aadconnectsync-service-manager-ui-connectors.md#follow-an-object-and-its-data-through-the-system).

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
