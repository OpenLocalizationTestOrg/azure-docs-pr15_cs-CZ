<properties
    pageTitle="Azure AD Connect synchronizace: synchronizace služby UI | Microsoft Azure"
    description="Vysvětlení informací na kartě spojnic ve Správci služby synchronizace pro Azure AD Connect."
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

![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

Karta spojnic slouží ke správě všechny systémy, se kterými modul Synchronizace spojení.

## <a name="connector-actions"></a>Spojnice akce

Akce | Komentář
--- | ---
Vytvoření | Nepoužívejte. Připojit se k další AD strukturami, použijte Průvodce instalací.
Vlastnosti | Použít pro doménu a OU filtrování.
[Odstranění](#delete) | Slouží k buď odstranit data do pole spojnice nebo připojení k strukturu odstranit.
[Konfigurace profilů](#configure-run-profiles) | Všechny domény kromě filtrování, nic konfigurace tady. Tato akce umožňuje zobrazit už nakonfigurované profilů.
Spuštění | Slouží ke spuštění jednorázové spustit profilu.
Stop | Ukončí spojnice aktuálně spuštěných profilu.
Export spojnice | Nepoužívejte.
Import spojnice | Nepoužívejte.
Aktualizace spojnice | Nepoužívejte.
Aktualizace schématu | Aktualizuje schématu uložené v mezipaměti. Je přednost před použijte možnost v Průvodci instalací, od které taky aktualizace synchronizovat pravidla.
[Vyhledávací spojnici místa](#search-connector-space) | Slouží k vyhledání objektů a sledovat [objektu a jeho dat prostřednictvím systému](#follow-an-object-and-its-data-through-the-system).

### <a name="delete"></a>Odstranění
Chcete v akci odstranění se používá pro dvě různé věci.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

**Odstranit pouze prostor konektoru** odebere všechna data, ale zachovat konfiguraci.

**Odstranění spojnice a prostor konektoru** odebere data a konfigurace. Tato možnost se používá při připojení k strukturu už nechcete.

Obě možnosti synchronizaci všech objektů a aktualizace metaverse objektů. Tato akce je dlouhodobé operace.

### <a name="configure-run-profiles"></a>Konfigurace profilů
Tato možnost umožňuje zobrazit profilů nakonfigurovány spojnice.

![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a>Vyhledávací spojnici místa
Akce hledání spojnice místo je užitečný pro vyhledání objektů a řešení problémů s.

![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

Začněte tak, že vyberete **rozsah**. Můžete si ji vyhledat podle dat (RDN DN, ukotvení dílčí strom), nebo státní objektu (všechny ostatní možnosti).  
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)  
Pokud neuděláte například dílčí stromu vyhledávání, se zobrazí všechny objekty v jedné OU.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png) z této tabulky můžete vybrat objektu, vyberte **Vlastnosti**a [postupujte podle ho](#follow-an-object-and-its-data-through-the-system) ze zdrojového prostoru spojnice prostřednictvím metaverse a prostor konektoru cílové.

## <a name="follow-an-object-and-its-data-through-the-system"></a>Postupujte podle objektu a jeho data celého systému
Při řešení potíží s daty, podle objekt ze zdrojového spojnice prostoru metaverse a byste do cíle prostor konektoru je klíčové postup pochopit, proč dat, které neobsahují očekávané hodnoty.

### <a name="connector-space-object-properties"></a>Vlastnosti objektu místo spojnice
**Import**  
Při otevření objektu cs existuje několik karet nahoře. Na kartě **importovat** zobrazena data, která je připraven po operaci importu.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/csimport.png) **Stará hodnota** ukazuje, co je obsažené v systému a **Nová hodnota** co byla přijata ze zdrojového systému a dosud nepoužil. V tomto případě vzhledem k tomu dojde k chybě synchronizace, změny se neprojeví.

**Chyba**  
Chybová stránka je viditelné pouze pokud máte potíže s objektem. Zobrazit podrobnosti na stránce Operace Další informace o tom, jak [Poradce při potížích s chybami synchronizace](active-directory-aadconnectsync-service-manager-ui-operations.md#troubleshoot-errors-in-operations-tab).

**Souvisejících zdrojích**  
Na kartě souvisejících zdrojích zobrazena souvislost objekt místo spojnice k objektu metaverse. Uvidíte importu spojnice poslední změnu z připojeného systému a která pravidla použitý k vyplnění dat v metaverse.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/cslineage.png) ve sloupci **Akce** uvidíte je jedno pravidlo synchronizace **vstupní** akce **poskytování**. Určující, dokud tento objekt místo spojnice je k dispozici, metaverse objekt zůstane. Pokud v seznamu pravidel synchronizace místo toho se pravidlo synchronizace s směr **odchozí** a **poskytování**, znamená to, že tento objekt je při odstranění objektu metaverse odstraněny.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/cslineageout.png) se také zobrazí ve sloupci **PasswordSync** , že oboru konektor příchozí pošty do ní můžete přidávat změny heslo od jedno pravidlo synchronizace má hodnotu **True**. Toto heslo se přemístí Azure AD pomocí odchozího pravidla.

Na kartě souvisejících zdrojích jste dostali na metaverse kliknutím na [Vlastnosti objektu Metaverse](#metaverse-object-properties).

V dolní části všechny karty jsou dvě tlačítka: **Náhled** a **protokolu**.

**Náhled**  
Náhled stránky slouží k synchronizaci jeden jeden objekt. Je užitečné, pokud odstraňujete některá pravidla synchronizace zákazníka a chcete si prohlédnout výsledek změny na jeden objekt. Můžete vybrat mezi **Úplné synchronizaci** a **Delta synchronizace**. Také můžete vybrat mezi **Generovat náhled**, který pouze sleduje změny v paměti a **Potvrďte náhledu**, které z fází všechny se změní na cílovém spojnice mezery.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/preview1.png) můžete zkontrolovat a pravidlo, které se použije pro určitý atribut toku objektu.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/preview2.png)

**Log**  
Na stránce protokol slouží k zobrazení stavu synchronizace heslo a historie Další informace najdete v tématu [Synchronizace hesel Poradce při potížích](active-directory-aadconnectsync-implement-password-synchronization.md#troubleshoot-password-synchronization) .

### <a name="metaverse-object-properties"></a>Vlastnosti objektu Metaverse
**Atributy**  
Na kartě atributy můžete zobrazit hodnoty a které spojnice přispěla ho.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/mvattributes.png)
**spojnic**  
Karta spojnic zobrazuje všechny mezery spojnice, které mají formátovanými jako na objekt.
![Správce služby synchronizace](./media/active-directory-aadconnectsync-service-manager-ui/mvconnectors.png) na této kartě lze také přejděte na [objekt místo spojnice](#connector-space-object-properties).

## <a name="next-steps"></a>Další kroky
Další informace o konfiguraci [Azure AD Connect synchronizovat](active-directory-aadconnectsync-whatis.md) .

Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
