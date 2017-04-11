<properties
    pageTitle="Poradce při potížích s Proxy aplikace | Microsoft Azure"
    description="Popisuje, jak Poradce při potížích v Azure AD aplikace Proxy."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/19/2016"
    ms.author="kgremban"/>



# <a name="troubleshoot-application-proxy"></a>Poradce při potížích s Proxy aplikace

Pokud dojde k chybám při přístupu k publikované aplikace nebo publikování aplikací, projděte si následující možnosti zobrazíte Pokud Proxy aplikace Microsoft Azure AD funguje zařízení správně:

- Spusťte konzolu služby systému Windows a ověřte, zda je povolena službu **Microsoft AAD aplikace Proxy konektoru** a spuštění. Taky můžete podívat se na stránce Vlastnosti služeb Proxy aplikací jak je znázorněno na následujícím obrázku:  
  ![Snímek obrazovky okna vlastností spojnice Proxy aplikací AAD Microsoft](./media/active-directory-application-proxy-troubleshoot/connectorproperties.png)

- Otevřete Prohlížeč událostí a vyhledejte aplikaci Proxy spojnice události v **protokoly aplikací a služeb** > **Microsoft** > **AadApplicationProxy** > **spojnice** > **Správce**.
- V případě potřeby podrobnější protokoly jsou k dispozici zapnutím technologie pro analýzu a ladění protokoly a zapnutí protokolu aplikace Proxy spojnice relace.

## <a name="the-page-is-not-rendered-correctly"></a>Na stránce není správně vykreslovat

Pokud není slyšet konkrétní chybové zprávy, může pořád máte problémy s aplikací vykreslování nebo nebude fungovat správně. Tato situace může nastat, pokud jste publikovali článek cestu, ale aplikace vyžaduje obsah nacházející se mimo cesty.

Například pokud publikujete https://yourapp/app cestu, ale aplikace volá obrázky v https://yourapp/media, že nebude vykreslit. Ujistěte se, publikovat aplikace pomocí nejvyšší úrovně cestu potřebujete zahrnout všechny relevantní obsah. V tomto příkladu by bylo http://yourapp/.

Pokud změníte cestu k zahrnovat odkazované obsah, ale potřebujete uživatelům má na hlubší odkaz na cestě, najdete v blogovém příspěvku [nastavení správné odkaz pro aplikace Proxy aplikací v Azure AD panel přístup a Spouštěč aplikací Office 365](https://blogs.technet.microsoft.com/applicationproxyblog/2016/04/06/setting-the-right-link-for-application-proxy-applications-in-the-azure-ad-access-panel-and-office-365-app-launcher/).

## <a name="general-errors"></a>Obecné chyby

Chyba | Popis | Rozlišení
--- | --- | ---
Nemají přístup k této podnikové aplikace. Nemáte oprávnění pro přístup k této aplikaci. Ověření se nezdařilo. Zkontrolujte, že přiřazení uživatele s přístupem k této aplikaci. | Vám nemusí přiřazeny uživatele k této aplikaci. | Přejděte na kartu **aplikace** a potom v části **Uživatelé a skupiny**, přiřadit tohoto uživatele nebo skupiny uživatelů k této aplikaci.
Nemají přístup k této podnikové aplikace. Nemáte oprávnění pro přístup k této aplikaci. Ověření se nezdařilo. Zkontrolujte, jestli má uživatel licenci pro Azure Active Directory Premium nebo Basic. | Uživatelé může se zobrazit tato chyba při pokusu o přístup k aplikaci, kterou jste publikovali Pokud nebyly přiřazena explicitně s licencí Premium/Basic správcem účastníka. | Přejděte na kartu služby Active Directory **licence** účastníka a ujistěte se, že tento uživatel nebo skupina uživatelů přiřazená Premium nebo základní licenci.


## <a name="connector-troubleshooting"></a>Poradce při potížích s spojnice
Když se registrace nepovede během instalace konektoru průvodce, zobrazení důvod selhání dvěma způsoby. Podívejte se v protokolu událostí v části **aplikace a služby Logs\Microsoft\AadApplicationProxy\Connector\Admin**, nebo spuštěním následujícího příkazu prostředí Windows PowerShell.

    Get-EventLog application –source “Microsoft AAD Application Proxy Connector” –EntryType “Error” –Newest 1

| Chyba | Popis | Rozlišení |
| --- | --- | --- |
| Registrace spojnice se nezdařilo: Zkontrolujte, jestli je povolený Proxy aplikace na portálu Správa Azure a správně zadané služby Active Directory uživatelské jméno a heslo. Chyba: "jeden nebo více chybě." | Zavření okna registrace bez provádění přihlášení k Azure AD. | Znovu spustit Průvodce spojnici a zaregistrovat spojnice. |
| Registrace spojnice se nezdařilo: Zkontrolujte, jestli je povolený Proxy aplikace na portálu Správa Azure a správně zadané služby Active Directory uživatelské jméno a heslo. Chyba: "AADSTS50001: zdrojů `https://proxy.cloudwebappproxy.net /registerapp` je vypnutá,." | Je vypnutá, Proxy aplikace.  | Zkontrolujte, jestli že povolíte Proxy aplikace na portálu Azure klasické před pokusem o zaregistrovat spojnice. Další informace o povolení aplikace Proxy najdete v článku [Povolení Proxy aplikace služeb](active-directory-application-proxy-enable.md). |
| Registrace spojnice se nezdařilo: Zkontrolujte, jestli je povolený Proxy aplikace na portálu Správa Azure a správně zadané služby Active Directory uživatelské jméno a heslo. Chyba: "jeden nebo více chybě." | Pokud otevře okno registraci a okamžitě zavře bez umožňuje přihlásit, je nejspíš tato chyba se zobrazí. K této chybě dochází při síťové chybě v počítači. | Ujistěte se, že je možné se připojit z prohlížeče na veřejný web a jestli jsou porty otevřené podle [požadavky aplikace Proxy](active-directory-application-proxy-enable.md). |
| Registrace spojnice se nezdařilo: Přesvědčte se, zda je počítač připojený k Internetu. Chyba: "koncový bod naslouchají `https://connector.msappproxy.net :9090/register/RegisterConnector` , který může přijímat zprávy. Důvodem je často nesprávná adresa nebo akce SOAP. Pokud je k dispozici další informace najdete v článku InnerException,. " | Pokud přihlásit pomocí svého Azure AD uživatelské jméno a heslo, ale pak tato chybová zpráva, může být, že všechny porty nad 8081 blokovány. | Zkontrolujte, jestli jsou porty potřebné otevřené. Další informace najdete v článku [požadavky aplikace Proxy](active-directory-application-proxy-enable.md). |
| Vymazat chyby jsou uvedeny v okně registrace. Nelze pokračujte – jenom zavřete okno. | Zadali jste nesprávné uživatelské jméno a heslo. | Zkuste to znovu. |
| Registrace spojnice se nezdařilo: Zkontrolujte, jestli je povolený Proxy aplikace na portálu Správa Azure a správně zadané služby Active Directory uživatelské jméno a heslo. Chyba: "AADSTS50059: žádné klienta identifikační informace součástí buď žádost nebo předpokládané tak, že všechny zadané přihlašovací údaje a selže vyhledávání pomocí služby zásady URI. | Snažíte se přihlásit pomocí Account Microsoft a ne domény, která je součástí ID organizace na adresář, který se pokoušíte otevřít. | Ujistěte se, že správce zadejte část názvu domény jako klienta domény, například contoso.com při domain Azure AD Správce by měl být admin@contoso.com. |
| Načtení aktuální spuštění zásady pro spuštění skriptů Powershellu se nezdařila. | Pokud dojde k chybě instalace konektoru, zkontrolujte, že není zakázané zásad spouštění PowerShell. | Otevřete Editor zásad skupiny. Přejděte na **Počítač konfigurace** > **Šablony pro správu** > **Součásti systému Windows** > **Prostředí Windows PowerShell** a dvakrát klikněte na **Zapnout spuštění skriptu**. To můžete nastavit **Není nakonfigurováno** nebo **Povolit**. Pokud nastaven **povoleno**, ujistěte se, že v části Možnosti spuštění je nastavená zásada buď **místní povolit a vzdálené podepsané skriptů** nebo **Povolení všechny skripty**. |
| Spojnice se nepovedlo stáhnout konfiguraci. | Spojnice klienta certifikát, který slouží k ověření, kterým vypršela platnost. To taky může dojít, pokud máte konektoru nainstalovaný za na proxy server. V tomto případě spojnice nemůže získat přístup k Internetu a nebudou moct aplikace vzdáleným uživatelům. | Obnovení zabezpečení ručně pomocí `Register-AppProxyConnector` rutin v prostředí Windows PowerShell. Pokud vaše spojnice je za na proxy server, je potřeba udělit přístup k Internetu k účtům spojnice "síťové služby" a "místní systém." Můžete to provést udělení přístupu k proxy serveru nebo nastavení, aby používat proxy server. |
| Registrace spojnice se nezdařilo: Zkontrolujte, že jsou globálního správce služby Active Directory pro registraci spojnice. Chyba: "požadavek na byl odepřen." | Alias, který se pokoušíte přihlášení není správce tuto doménu. Na spojnici je vždy nainstalovaný na adresář, který vlastní doméně uživatele. | Ujistěte se, že správce snažíte se přihlásit jako globální oprávnění ke klientovi Azure AD.|


## <a name="kerberos-errors"></a>Protokol Kerberos chyby

| Chyba | Popis | Rozlišení |
| --- | --- | --- |
| Načtení aktuální spuštění zásady pro spuštění skriptů Powershellu se nezdařila. | Pokud dojde k chybě instalace konektoru, zkontrolujte, že není zakázané zásad spouštění PowerShell. | Otevřete Editor zásad skupiny. Přejděte na **Počítač konfigurace** > **Šablony pro správu** > **Součásti systému Windows** > **Prostředí Windows PowerShell** a dvakrát klikněte na **Zapnout spuštění skriptu**. To můžete nastavit **Není nakonfigurováno** nebo **Povolit**. Pokud nastaven **povoleno**, ujistěte se, že v části Možnosti spuštění je nastavená zásada buď **místní povolit a vzdálené podepsané skriptů** nebo **Povolení všechny skripty**. |
| 12008 - azure AD překročení maximální počet povolené pokusů ověřování protokolem Kerberos k serveru. | Tato událost mohou poukazovat nesprávná konfigurace mezi Azure AD a aplikační server back-end nebo nějaký problém v konfiguraci datum a čas oba počítače. | Back-end serveru k odmítnutí lístek Kerberos vytvořil Azure AD. Ověřte, že Azure AD a jsou správně nakonfigurované aplikační server back-end. Ujistěte se, že konfigurace datum a čas v Azure AD a aplikační server back-end jsou synchronizovány. |
| 13016 - azure AD nemůže získat lístek protokolu Kerberos jménem uživatele, protože je bez Přípony token okraje nebo soubor cookie přístup. | Máte potíže s nastavením služba tokenů zabezpečení. | Odstraňte Konfigurace nároku Přípony v Služba tokenů zabezpečení. |
| 13019 - azure AD nemůže získat lístek protokolu Kerberos jménem uživatele z důvodu obecné rozhraní API chybová zpráva. | Tato událost mohou poukazovat nesprávná konfigurace mezi Azure AD a server řadiče domény nebo problém s konfigurací datum a čas oba počítače. | Řadiče domény k odmítnutí lístek Kerberos vytvořil Azure AD. Ověření Azure AD a back-end aplikační server nakonfigurovaný správně, zejména konfigurace hlavního názvu služby. Zkontrolujte, že Azure AD je doména připojen k do stejné domény řadiče domény zajistit, že řadiče domény vytváří vztah důvěryhodnosti s Azure AD. Ujistěte se, že konfigurace datum a čas v Azure AD a jsou synchronizovány řadiče domény. |
| 13020 - azure AD nemůže získat lístek protokolu Kerberos jménem uživatele, protože není definované back-end serveru hlavního názvu služby. | Tato událost mohou poukazovat nesprávná konfigurace mezi Azure AD a server řadiče domény, nebo nějaký problém v konfiguraci datum a čas oba počítače. | Řadiče domény k odmítnutí lístek Kerberos vytvořil Azure AD. Ověření Azure AD a back-end aplikační server nakonfigurovaný správně, zejména konfigurace hlavního názvu služby. Zkontrolujte, že Azure AD je doména připojen k do stejné domény řadiče domény zajistit, že řadiče domény vytváří vztah důvěryhodnosti s Azure AD. Ujistěte se, že konfigurace datum a čas v Azure AD a jsou synchronizovány řadiče domény. |
| 13022 - azure AD nelze ověřuje uživatele, protože server back-end odpoví na pokusy o ověřování protokolem Kerberos s chybou HTTP 401. | Tato událost mohou poukazovat nesprávná konfigurace mezi Azure AD a aplikační server back-end nebo nějaký problém v konfiguraci datum a čas oba počítače. | Back-end serveru k odmítnutí lístek Kerberos vytvořil Azure AD. Ověřte, že Azure AD a jsou správně nakonfigurované aplikační server back-end. Ujistěte se, že konfigurace datum a čas v Azure AD a aplikační server back-end jsou synchronizovány. |
| Na webu nemůže zobrazit stránky. | Uživatel může se zobrazit tato chyba při pokusu o přístup k aplikaci, kterou jste publikovali, pokud jedná o aplikaci IWA. Hlavní definovaný název služby pro tuto aplikaci je pravděpodobně nesprávné. | K aplikacím IWA: Ujistěte se, jestli je správně nakonfigurovaný pro tuto aplikaci hlavního názvu služby. |
| Na webu nemůže zobrazit stránky. | Uživatel může se zobrazit tato chyba při pokusu o přístup k aplikaci, kterou jste publikovali, pokud jedná o aplikaci OWA. Příčin může být některým z následujících akcí: <br> -Definovaný hlavního názvu služby k této aplikaci není správná. <br> -Uživatele, kterému pokusu o přístup k aplikaci se svým účtem Microsoft, nikoli VELKÁ2 firemní účet přihlašování použít nebo uživatel je uživatel Host. <br> -Uživatele, kterému pokusu o přístup k aplikaci není správně definované pro tuto aplikaci na straně místní. | Kroky, které zredukují příslušným způsobem: <br> -Ověřte, jestli je správně nakonfigurovaný pro tuto aplikaci hlavního názvu služby. <br> – Zkontrolujte, jestli jste uživatel přihlásí pomocí své firemní účet, která odpovídá domény publikovaná aplikace. Uživatelé Microsoft Account a Host nelze získat přístup k aplikacím IWA. <br> – Zkontrolujte, že tento uživatel musí mít příslušná oprávnění definované pro tuto aplikaci back-end v počítači místní. |
| Nemají přístup k této podnikové aplikace. Nemáte oprávnění pro přístup k této aplikaci. Ověření se nezdařilo. Zkontrolujte, že přiřazení uživatele s přístupem k této aplikaci. | Uživatelé může se zobrazit tato chyba při pokusu o přístup k aplikaci, kterou jste publikovali pokud používá účtů Microsoft místo jejich firemní účet pro přihlášení. Tato chyba může objevit taky uživatele typu Host. | Uživatelé Account Microsoft a hostů nemůže získat přístup k aplikacím IWA. Ujistěte se uživatel přihlásí pomocí své firemní účet, která odpovídá domény publikovaná aplikace. |
| Tato podnikové aplikace nemají přístup k teď. Opakovaném pokusu … Konektor vypršel časový limit. | Uživatelé může se zobrazit tato chyba při pokusu o přístup k aplikaci, kterou jste publikovali, pokud nejsou správně definované pro tuto aplikaci na straně místní. | Ujistěte se, že uživatelé mají příslušná oprávnění definované pro tuto aplikaci back-end na místní počítač. |


## <a name="see-also"></a>Viz taky

- [Povolení aplikace Proxy služby Azure Active Directory](active-directory-application-proxy-enable.md)
- [Publikování aplikací s Proxy aplikace](active-directory-application-proxy-publish.md)
- [Povolení jednotné přihlašování](active-directory-application-proxy-sso-using-kcd.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-troubleshoot/connectorproperties.png
[2]: ./media/active-directory-application-proxy-troubleshoot/sessionlog.png
