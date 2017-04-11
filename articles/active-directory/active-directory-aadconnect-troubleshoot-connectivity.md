<properties
    pageTitle="Azure AD Connect: Problémů s připojením | Microsoft Azure"
    description="Vysvětluje, jak vyřešit problémy s připojením s Azure AD Connect."
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
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Vyřešit problémy s připojením s Azure AD Connect
Tento článek vysvětluje, jak funguje propojení mezi Azure AD Connect a Azure AD a jak řešit problémy s připojením. Těmto problémům nejčastěji se zobrazí v prostředí s proxy server.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Vyřešit problémy s připojením v Průvodci instalací
Azure AD Connect používá moderní ověřování (pomocí ADAL library) pro ověřování. Průvodce instalací a modul Synchronizace správné vyžadují machine.config třeba správně nakonfigurovat od jedná se o .NET aplikace.

V tomto článku ukážeme, jak Fabrikam připojuje k Azure AD jeho proxy server. Proxy server je název fabrikamproxy a používá port 8080.

Potřebujeme nejdřív zkontrolujte, jestli je správně nakonfigurovaný [**machine.config**](active-directory-aadconnect-prerequisites.md#connectivity) .  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
V některých jiných výrobců blogy jsou popsány, že by měl být změny miiserver.exe.config místo. Tento soubor je však přepsat při každé upgradu sudé, že pokud to funguje během počáteční instalace systému přestanou fungovat při prvním upgradu. Proto doporučujeme místo toho aktualizovat machine.config.

Proxy serveru musí mít taky požadované URL otevřeli. Úřední seznam jsou popsány v [Office 365 URL a rozsahy IP adres ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2).

Z těchto v následující tabulce je absolutní minimum, abyste mohli připojit k Azure AD vůbec. Tento seznam neobsahuje žádné volitelné funkce, například hesla zpětným nebo Azure AD stavu připojení. Je popsána tady na pomoc při řešení potíží při počáteční konfiguraci.

ADRESA URL | Port | Popis
---- | ---- | ----
mscrl.microsoft.com | NASTAVIT INFORMACE HTTP/80 | Použít ke stahování CRL seznamů.
\*. verisign.com | NASTAVIT INFORMACE HTTP/80 | Použít ke stahování CRL seznamy.
\*. entrust.com | NASTAVIT INFORMACE HTTP/80 | Použít ke stahování CRL seznamy pro MFA.
\*. windows.net | PROTOKOL HTTPS A 443 | Používá se přihlásit k Azure AD.
Secure.aadcdn.microsoftonline p.com | PROTOKOL HTTPS A 443 | Použít pro MFA.
\*. microsoftonline.com | PROTOKOL HTTPS A 443 | Používá ke konfiguraci adresáři Azure AD a import a export dat.

## <a name="errors-in-the-wizard"></a>Chyby v Průvodci
Průvodce instalací používá dvěma různými kontexty zabezpečení. Na stránce **připojit k Azure AD** používá aktuálně přihlášený uživatel. Na stránce **Konfigurovat** je změna [účtu službou pro modul Synchronizace](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). Konfigurace proxy provedeme jsou globální k počítači, pokud se problém nepovedlo odstranit, tento problém se většina pravděpodobně objeví již na stránce **připojit k Azure AD** v průvodci.

Toto jsou nejčastější chyby, které se zobrazí v Průvodci instalací.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Průvodce instalací není nakonfigurovaný správně
Tato chyba se zobrazí, když samotný průvodce nelze dostanete proxy server.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Pokud se zobrazí, zkontrolujte [machine.config](active-directory-aadconnect-prerequisites.md#connectivity) správně nakonfigurované.
- Pokud, která vypadá v pořádku, postupujte podle [ověření proxy připojení](#verify-proxy-connectivity) zobrazíte, pokud je k dispozici mimo Průvodce i problém.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>Koncový bod MFA nemůže získat přístup na stránku
Tato chyba se zobrazí, pokud koncový bod **https://secure.aadcdn.microsoftonline-p.com** nemůže získat přístup na stránku a globální správce má MFA povolené.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Pokud se zobrazí, ověřte, že secure.aadcdn.microsoftonline koncového bodu-p.com někdo přidal do proxy server.

### <a name="the-password-cannot-be-verified"></a>Nelze ověřit heslo
Pokud je Průvodce instalací úspěšný při připojování ke Azure AD, ale heslo samotné nelze ověřit, uvidíte toto: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- Je heslo dočasné heslo a musí změnit? To je skutečně správné heslo? Zkuste se přihlásit k https://login.microsoftonline.com (na jiném počítači než server Azure AD Connect) a ověřte, zda že je možné použít účet.

### <a name="verify-proxy-connectivity"></a>Ověřte připojení proxy serveru
Ověření, pokud má server Azure AD Connect skutečné připojení protokolem Proxy a Internet používáme některé prostředí PowerShell zobrazíte, pokud je proxy server nebo ne povolení požadavky na web. Na příkazovém řádku prostředí PowerShell spusťte `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Doslova první volání slouží k https://login.microsoftonline.com a to budou fungovat i, ale identifikátor URI je rychlejší reagovat.)

Prostředí PowerShell bude konfigurace v souboru machine.config kontaktovat přes proxy server. Nastavení winhttp/netsh neměli mít vliv na těchto rutin.

Pokud nakonfigurovaný správně proxy server, by měla získat stav Úspěch: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Pokud se zobrazí **nelze se připojit k vzdálený server** zkuste prostředí PowerShell přímé zavolání bez použití proxy server nebo DNS není správná. Zkontrolujte, jestli že je správně nakonfigurovaný soubor **machine.config** .
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Pokud není správně nakonfigurovaný tak proxy server, jsme dojde k chybě: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Chyba | Text chybové zprávy | Komentář
---- | ---- | ---- |
403 | Zakázáno | Proxy server nebyl otevřen pro požadované adresy URL. Návštěvě konfigurací proxy a ujistěte se, otevřeli [adresy URL](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) .
407 | Povinné ověřování proxy serveru | Proxy serveru nutné přihlášení a žádná získali. Pokud proxy server vyžaduje ověření, zkontrolujte, že jste to konfigurovat soubor machine.config. Ujistěte se, že používáte domény účtů pro uživatele Průvodce stejně jako u účtu služby také.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Vzorek komunikace mezi Azure AD Connect a Azure AD
Pokud absolvování všech kroků nahoře a stále nemůžete připojit můžete v tomto okamžiku začít prohlížíte síťové protokoly. Tato část je dokumentace vzorek normální a úspěšné připojení. Je také seznam běžných herrings červené, které můžete ignorovat, pokud čtete síťové protokoly.

- Budou volání https://dc.services.visualstudio.com. Není nutné mít tuto otevřete v náhledové pro instalaci úspěšné a tyto můžete ignorovat.
- Zobrazí se, že služeb překládá názvy dns zobrazí seznam skutečné hosts být nsatc.net DNS názvu místa a další obory názvů za microsoftonline.com. Však nesmí být všechny žádosti o služby web na názvy skutečný server a nemáte přidat tyto proxy server.
- Koncové body adminwebservice a provisioningapi (viz níže v protokolech) koncové body zjišťování a použít ke zjištění skutečné koncový bod, který použít a bude se liší v závislosti na vaší oblasti.

### <a name="reference-proxy-logs"></a>Odkaz proxy protokoly
Tady je výpis z protokolu skutečné proxy a stránka Průvodce instalací ze kdy byla přijatá (mohly být odebrány duplicitní položky stejným koncový bod). Slouží jako odkaz pro vlastní proxy a síťové protokoly. Skutečné koncové body se můžou lišit ve vašem prostředí (zejména v *kurzívu*).

**Připojení k Azure AD**

Čas | ADRESA URL
--- | ---
1/11/2016 8:31 | Connect://Login.microsoftonline.com:443
1/11/2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:32 | připojení: / /*bba800 ukotvení*. microsoftonline.com:443
1/11/2016 8:32 | Connect://Login.microsoftonline.com:443
1/11/2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:33 | připojení: / /*bwsc02 předávací*. microsoftonline.com:443

**Konfigurace**

Čas | ADRESA URL
--- | ---
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11 nebo 2016 8:43 | připojení: / /*bba800 ukotvení*. microsoftonline.com:443
1/11/2016 8:43 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11/2016 8:44 | připojení: / /*bba900 ukotvení*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1/11 nebo 2016 8:44 | připojení: / /*bba800 ukotvení*. microsoftonline.com:443
1/11/2016 8:44 | Connect://Login.microsoftonline.com:443
1/11/2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
1/11/2016 8:46 | připojení: / /*bwsc02 předávací*. microsoftonline.com:443

**Počáteční synchronizace**

Čas | ADRESA URL
--- | ---
1/11/2016 8:48 | Connect://Login.Windows.NET:443
1/11/2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
1/11 nebo 2016 8:49 | připojení: / /*bba900 ukotvení*. microsoftonline.com:443
1/11/2016 8:49 | připojení: / /*bba800 ukotvení*. microsoftonline.com:443

## <a name="authentication-errors"></a>Chyby ověřování
Tato část popisuje chyby, které může být vrácen z ADAL (knihovny ověřování používané službou Azure AD Connect) a Powershellu. Chyba vysvětlení potřebné informace vyhledat v pochopit další kroky.

### <a name="invalid-grant"></a>Neplatný grant (udělit)
Neplatné uživatelské jméno a heslo. Další informace najdete v tématu [nelze ověřit heslo](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Neznámý uživatelský typ
Adresáře Azure AD nemůžete najít nebo vyřešit. Možná se pokusíte přihlásit pomocí uživatelského jména v doméně neověřený?

### <a name="user-realm-discovery-failed"></a>Zjišťování sféry uživatele se nezdařila.
Problémy s konfigurací sítě nebo proxy serveru. Nesmí být síti dosažení najdete v článku [Poradce při potížích s problémy s připojením v Průvodci instalací](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Vypršela platnost uživatelského hesla
Vypršela platnost přihlašovacích údajů. Změna hesla.

### <a name="authorizationfailure"></a>AuthorizationFailure
Neznámý problém.

### <a name="authentication-cancelled"></a>Ověřování zrušená
Uživatel ho zrušil ověřovací kód vícefaktorové ověřování (MFA).

### <a name="connecttomsonline"></a>ConnectToMSOnline
Ověření bylo úspěšné Azure AD PowerShell má problému s ověřování.

### <a name="azurerolemissing"></a>AzureRoleMissing
Ověření bylo úspěšné. Nejste globálního správce.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Ověření bylo úspěšné. Je povolená Správa privilegovaných identit a nejste aktuálně globálního správce. Další informace najdete v tématu [Privilegovaných správy identit](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Ověření bylo úspěšné. Nelze získat informace o společnosti z Azure AD.

### <a name="retrievedomains"></a>RetrieveDomains
Ověření bylo úspěšné. Nelze získat informace o doméně z Azure AD.

## <a name="troubleshooting-steps-for-previous-releases"></a>Návody na řešení potíží pro předchozí verze.
S verzemi počínaje číslem sestavení 1.1.105.0 (vydána únor 2016) se už nepoužívá Pomocníka pro přihlášení. V této části Konfigurace už měli povinná, ale bude k dispozici jako odkaz.

Jednotného přihlašování v Pomocník pracovat musí být nakonfigurováno winhttp. Lze provést pomocí [**příkazu netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Pomocník pro přihlášení není nakonfigurovaný správně
Tato chyba se zobrazí při Pomocníka pro přihlášení nelze kontaktovat proxy server nebo proxy server nepovoluje žádost.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Pokud se zobrazí, podívejte se na konfigurací proxy v [netsh](active-directory-aadconnect-prerequisites.md#connectivity) a ověřte, jestli že je správná.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Pokud, která vypadá v pořádku, postupujte podle [ověření proxy připojení](#verify-proxy-connectivity) zobrazíte, pokud je k dispozici mimo Průvodce i problém.

## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).
