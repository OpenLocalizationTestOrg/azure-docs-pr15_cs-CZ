<properties
   pageTitle="Azure AD Connect synchronizace: zabránit náhodné odstraníte | Microsoft Azure"
   description="Toto téma popisuje funkce náhodné odstraníte (zabránění náhodné odstranění) zabránit v Azure AD Connect."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/01/2016"
   ms.author="billmath"/>

# <a name="azure-ad-connect-sync-prevent-accidental-deletes"></a>Azure AD Connect synchronizace: zabránit náhodné odstranění
Toto téma popisuje funkce náhodné odstraníte (zabránění náhodné odstranění) zabránit v Azure AD Connect.

Při instalaci Azure AD Connect, kvůli kterým náhodné odstraníte je standardně zapnutá a není neumožňuje export s víc než 500 odstraní. Tato funkce slouží k ochraně před nechtěným konfigurace změny a do svého místního adresáře, které by mohly ovlivnit většího počtu uživatelů a jiných objektů.

Obvyklé scénáře až uvidíte mnoho odstraníte patří:

- [Filtrování](active-directory-aadconnectsync-configure-filtering.md) , kde nevybrané celý [OU](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) nebo [doména](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) se změní.
- Budou odstraněny všechny objekty v s organizační Jednotkou.
- Organizační jednotka je přejmenovat tak, aby všechny objekty v něm jsou považovány za pro synchronizaci mimo rozsah.

Pomocí prostředí PowerShell bude možné měnit nebo rovná hodnotě 500 objektů pomocí `Enable-ADSyncExportDeletionThreshold`. Nastavte tuto hodnotu přizpůsobí velikost vaší organizace. Protože plánovači synchronizace získáte každých 30 minut hodnota je počet odstraníte považovat za 30 minut.

Pokud jsou moc odstraníte připravené k exportu do Azure AD, ukončí exportovat a obdržíte e-mail takto:

![Zabránit náhodné odstraníte e-mailu](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/email.png)

> *Hello (technická kontakt). V (čas) služby synchronizace Identity zjištěná překročení počet vymazání mezní nakonfigurované odstranění (názvu organizace). K odstranění v této Identity synchronizace byly odeslány (číslo) celkového počtu objektů. To splňuje nebo překračuje mezní hodnota nakonfigurované odstranění objektů (číslo). Potřebujeme vás o poskytnutí potvrzení, že by měly být odstranění zpracovány nemůžeme pokračovat. Přečtěte si téma bránící náhodné odstranění Další informace o chybě uvedené v této e-mailové zprávě.*

Můžete zobrazit stav `stopped-deletion-threshold-exceeded` při prohlížení ve **Správci synchronizační služby** UI profilu exportovat.
![Zabránit náhodné odstraníte synchronizace služby UI](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/syncservicemanager.png)

Jestli to neočekávané, zkoumat a udělali jaké nápravné kroky. Pokud chcete zobrazit objektů, které se chystáte odstranit, postupujte takto:

1. Spuštěním **služby synchronizace** z nabídky Start.
2. Přejděte ke **spojnicím**.
3. Vyberte spojnici s typem **Azure Active Directory**.
4. V části **Akce** vpravo vyberte **Vyhledávací spojnici místa**.
5. V místní nabídce klikněte v části **obor**vyberte **Odpojení od** a vyberte čas v minulosti. Klikněte na **Hledat**. Tato stránka obsahuje zobrazení všech objektů na odstranit. Po kliknutí na všechny požadované položky, můžete získat další informace o objektu. Klikněte na tlačítko **Nastavení sloupce** přidat další atributy, které nemusí být vidět v mřížce.

![Vyhledávací spojnici místa](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/searchcs.png)

Podle potřeby všechny odstraníte jsou, pak udělejte toto:

1. Dočasně vypnout tuto ochranu a ponechat tyto odstraníte absolvovat, spusťte rutinu Powershellu: `Disable-ADSyncExportDeletionThreshold`. Zadejte účet Azure AD globální správce a heslo.
![Přihlašovací údaje](./media/active-directory-aadconnectsync-feature-prevent-accidental-deletes/credentials.png)
2. S Azure Active Directory Connector stále vybrán vyberte akce **Spustit** a vyberte **Export**.
3. Pokud chcete znova zamknout, spusťte rutiny prostředí PowerShell: `Enable-ADSyncExportDeletionThreshold`.

## <a name="next-steps"></a>Další kroky

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
