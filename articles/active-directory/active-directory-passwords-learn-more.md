<properties
    pageTitle="Další informace: Správa Azure AD hesel | Microsoft Azure"
    description="Rozšířené témat na Správa Azure AD hesel, včetně hesla zpětným fungování, zabezpečení zpětným hesla, jak resetovat heslo portálu funguje a jaká data používá resetování hesla."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="learn-more-about-password-management"></a>Další informace o správě hesel

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

Pokud jste již nainstalovali Správa hesel nebo jsou jenom hledáte další informace o nitty že krupičnaté jak to funguje před nasazením, tento oddíl vám nabídne užitečný přehled technické Principy služby technické. Budeme se zabývat těmito oblastmi takto:

* [**Přehled zpětným heslo**](#password-writeback-overview)
  - [Jak funguje zpětným hesla](#how-password-writeback-works)
  - [Scénáře podporované pro heslo zpětného zápisu](#scenarios-supported-for-password-writeback)
  - [Model zabezpečení heslo zpětného zápisu](#password-writeback-security-model)
* [**Jak obnovit heslo portálu práce?**](#how-does-the-password-reset-portal-work)
  - [Jaká data používá resetování hesla?](#what-data-is-used-by-password-reset)
  - [Jak získat přístup ke heslo resetovat dat pro vaše uživatele](#how-to-access-password-reset-data-for-your-users)

## <a name="password-writeback-overview"></a>Přehled zpětným heslo
Heslo zpětným je komponentu [Azure Active Directory připojení](active-directory-aadconnect.md) , která lze povolit a používat aktuální předplatitelé Azure Active Directory Premium. Další informace najdete v tématu [Azure Active Directory edice](active-directory-editions.md).

Heslo zpětným umožňuje konfigurace klienta cloudu napsat hesly zpět místní služby Active Directory.  Vyloučí můžete ušetřil práci se dají vytvořit a spravovat řešení složitých místního samoobslužné resetování a poskytuje cloudové pohodlně pro uživatele o resetování hesla místní místo, kde jsou.  Přečtěte si o některé klíčové funkce zpětným heslo:

- **Nula zpoždění názory.**  Heslo zpětným je synchronní operace.  Uživatelé se okamžitě oznámení, pokud své heslo nesplňuje zásad nebo nebylo možné obnovit nebo změně jakéhokoli důvodu.
- **Resetování hesel pro uživatele pomocí služby AD FS nebo jinými technologiemi federace podporuje.**  S heslo zpětným dlouhou, jak se synchronizují federované uživatelských účtů do vašeho tenanta Azure AD budou moct spravovat své místní AD hesel z cloudu.
- **Podporuje resetování hesel pro uživatele používající hash synchronizaci hesel.** Když službu resetovat heslo zjistí synchronizované uživatelského účtu aktivované řešení hash synchronizaci hesel, obnovení tento účet místní a heslo cloudu současně.
- **Změna hesla z panel přístup a Office 365 podporuje.**  Když federované nebo synchronizaci hesel by uživatelé přijde změnit svoje ukončenou platností nebo konec platnosti hesla, jsme budete odepsat tyto hesla místního prostředí AD.
- **Podporuje hesla zápis zpět, pokud správce resetování přímo z** [**Portál Správa azure**](https://manage.windowsazure.com).  Pokaždé, když obnovíte správce, který by hesla uživatele v [Portálu pro správu Azure](https://manage.windowsazure.com), pokud je federované se nebo synchronizaci hesel, budete nastavení heslo, které správce vybere na svůj místní AD, stejně.  Momentálně není podporována v portálu pro správu Office.
- **Vynutí vaší místní zásady pro hesla AD.**  Když uživatel obnoví své heslo, jsme zkontrolujte, jestli splňuje vaše místní zásady před použitím tohoto adresáře služby Active Directory.  Platí to i historie složitosti, věku, filtrů hesel a další omezení heslo, které jste definovali ve vaší místní AD.
- **Nevyžaduje všechny příchozí pravidla brány firewall.**  Heslo zpětným používá Bus služby Azure relay jako základní komunikace kanálu, což znamená, že není nutné otevírat všechny příchozí portů brány firewall pro tuto funkci používat.
- **Není podporována u uživatelských účtů, které jsou v zamknutém skupiny služby Active Directory vaší místní.** Další informace o chráněné skupiny najdete v článku [chráněné účty a skupiny ve službě Active Directory](https://technet.microsoft.com/library/dn535499.aspx).

### <a name="how-password-writeback-works"></a>Jak funguje zpětným heslo
Heslo zpětným má tři hlavní součásti:

- Heslo resetovat cloudové služby (Toto je také integrovaný stránky změnit heslo Azure AD)
- Předávací Bus služby Azure specifické pro klienta
- Resetování hesla místní koncový bod

Odpovídají společně podle popisu v následujícím obrázku:

  ![][001]

Pokud heslo nebo federované hash synchronizovat by uživatel přejde na resetování nebo změna své heslo v cloudu, mít tyto důsledky:

1.  Jsme zkontrolujte má uživatel jaký druh heslo.  Pokud vidíme, že hesla se spravuje místně, pak můžeme zajistit službu zpětným zařídit i.  Pokud ano, doporučujeme, aby uživatel pokračovat, pokud není, jsme dát uživatelům, které nelze tady resetovat své heslo.
2.  Potom uživateli předá bran odpovídající ověřování a dosáhne obrazovce heslo resetovat.
3.  Uživatel vybere nové heslo a potvrdí ho.
4.  Po kliknutí na tlačítko Odeslat, jsou šifrovány heslo ve formátu prostého textu symetrickou klíčem, který byl vytvořen během instalace zpětného zápisu.
5.  Po šifrování heslo, jsme zahrnout do data, která se odešlou kanálem HTTPS do vašeho klienta konkrétní službu bus relay (který jsme taky pro vás vytvořil při nastavování zpětným).  Tento předávací chráněn náhodně generovaným heslo, které zná pouze místní instalace.
6.  Po zprávu dosáhne bus služby, koncový bod resetovat heslo automaticky obnoví a uvidí, že má žádost obnovit čeká na vyřízení.
7.  Služba pak vyhledá uživatele předmětné atributem cloudu ukotvení.  Pro toto vyhledávání úspěšné objektu uživatele musí existovat v prostoru spojnice AD, musí být propojené s odpovídající objekt více hodnot a musí být propojené s odpovídající objekt AAD spojnice. Nakonec, aby synchronizace najít tento uživatelský účet, propojení AD spojnice objekt více hodnot může mít pravidlo synchronizace `Microsoft.InfromADUserAccountEnabled.xxx` na odkaz.  Není nutná, protože při volání pochází cloudu, používá atribut cloudAnchor vyhledat objekt místo spojnice AAD sync engine a potom následující odkaz na objekt více hodnot a zpátky k objektu AD následuje odkaz. Vzhledem k tomu může existovat více AD objektů (s více doménové) pro jednoho uživatele, závisí na modulu synchronizace `Microsoft.InfromADUserAccountEnabled.xxx` odkaz vybrat správná.
8.  Když našli uživatelský účet jsme pokus o resetování hesla přímo ve struktuře odpovídající AD.
9.  Pokud je operace nastavit heslo úspěšná, jsme dát uživatelům upraven své heslo a můžete přejít na Šťastné cestě.
10. Pokud dojde k chybě operace nastavit heslo, jsme vrátí chybu uživateli a zpřístupněte je to zkuste znova.  Operace může nepovede, protože nefungoval službu, protože hesla budou vybrané nesplňuje zásady organizace, protože nepovedlo se nám najít uživatele v místním AD nebo jiné číslo z důvodů.  Můžeme mít na určitou zprávu v mnoha případech a dát uživatelům co můžou dělat tento problém vyřešit.

### <a name="scenarios-supported-for-password-writeback"></a>Scénáře podporované pro heslo zpětného zápisu
Následující tabulka popisuje scénáře, které jsou podporovány pro verzích naše možnost synchronizace.  Obecně doporučujeme nainstalovat nejnovější verzi [Azure AD Connect](active-directory-aadconnect.md#install-azure-ad-connect) , pokud chcete použít zpětným heslo.

  ![][002]

### <a name="password-writeback-security-model"></a>Model zabezpečení heslo zpětného zápisu
Heslo zpětným je služba vysoce zabezpečeném a robustní.  Chcete-li zajistěte, aby že se po zamknutí informacím, jsme povolit model vrstveny 4 zabezpečení, který je popsáno níže.

- **Konkrétní relay service bus klienta** – při nastavování služby, nastavení relay bus specifické pro klienta služby, který je chráněn náhodně generovaným silné heslo, které Microsoft nikdy má přístup k.
- **Uzamknout dolů neposkytuje silné heslo šifrovací klíč** – po vytvoření service bus relay, vytvoříme silných symetrickou klíč, který jsme šifrování databáze pomocí hesla jako vystupuje lince.  Tento klíč jsou umístěná pouze ve vaší společnosti tajné úložiště přihlašovacích údajů v cloudu, který je intenzivně uzamknout auditování, stejně jako jakékoli jiné heslo v adresáři.
- **Standardní TLS odvětví** – po resetování hesla nebo změna operace dojde k v cloudu, jsme prohlédněte ve formátu prostého textu hesla a šifrování s veřejným klíčem.  Společnost Microsoft pak plop, do HTTPS zprávy, která se pošle šifrované kanálem pomocí certifikáty SSL společnosti Microsoft k vaší relay service bus.  Po příchodu zprávy do služby Bus místní agent se obnoví, ověří služby Bus pomocí silné heslo, které bylo dřív vygenerovalo, vyzvedne zašifrované zprávy, dešifruje pomocí privátním klíčem jsme vzniku a pokusech po nastavení hesla prostřednictvím rozhraní API AD DS SetPassword.  Tento krok je co umožňuje nám k jejímu vynucení vaší místní heslo zásady služby Active Directory (složitosti, věku, historie, filtry atd.) v cloudu.
- **Zásady vypršení platnosti zprávy** – nakonec, pokud z nějakého důvodu zprávy je umístěn v služby Bus vzhledem k tomu místní služba dolů, bude vypršel časový limit a odebrat po několika minutách zvýšit zabezpečení te000130432 můžete dál.

## <a name="how-does-the-password-reset-portal-work"></a>Jak obnovit heslo portálu práce?
Když uživatel přejde do portálu resetování hesel, pracovního postupu je vykazuje postavu lze zjistit, s uživatelským účtem je platný, jaké organizace, která patří uživatelů, kde se spravuje heslo uživatele a zda je uživatel licencován můžete pomocí funkce.  Další kroky zobrazíte další informace o logiku za stránce Resetovat heslo.

1.  Kliknutí na nemáte přístup ke spojení účtu ani přejde přímo do [https://passwordreset.microsoftonline.com](https://passwordreset.microsoftonline.com).
2.  Uživatel zadá id uživatele a předá test captcha.
3.  Azure AD ověřuje, zda je uživatel mít tuto funkci lze použít následujícím způsobem:
    - Zkontroluje, jestli má uživatel tuto funkci povolit a licenci Azure AD přiřazené.
        - Pokud uživatel nemá tuto funkci povolit nebo přiřazenou licenci, uživatel požádán kontaktovat přihlásil správce resetovat své heslo.
    - Zkontroluje, jestli má uživatel vpravo ověřovací údaje definované na svého účtu v souladu se zásadami správce.
        - Vyžaduje-li zásad jedinou výzvu, pak je je zajistit, jestli má uživatel příslušná data definované pro alespoň jedno z výzvy povolit tak, že správce zásad.
          - Pokud uživatel není nakonfigurován, by měl uživatel kontaktovat přihlásil správce resetovat své heslo.
        - Pokud zásadu vyžaduje dva úkoly, pak je je zajistit, jestli má uživatel příslušná data definované pro aspoň dva úkoly povolit tak, že správce zásad.
          - Pokud není nakonfigurován uživateli, je jsme uživatel doporučeno kontaktovat přihlásil správce resetovat své heslo.
    - Zkontroluje, zda heslo uživatele spravují místně (federované nebo synchronizaci hesel hash obsahoval).
       - Pokud zpětným nasazeném a hesla se spravuje místně, uživatel může pokračujte ověřování a resetovat své heslo.
       - Pokud není zpětným nasazeném a hesla se spravuje místně, uživatel požádán kontaktovat přihlásil správce resetovat své heslo.
4.  Pokud je určen, že uživatel je si úspěšně resetovat své heslo, uživatel řídit procesem obnovit.

Další informace o tom, jak nasazení zpětným heslo na [Začínáme: Správa hesel Azure AD](active-directory-passwords-getting-started.md).

### <a name="what-data-is-used-by-password-reset"></a>Jaká data používá resetování hesla?
V následující tabulce osnovy kde a jak tato data se použije při obnovení hesla a umožňuje vám pomůžou rozhodnout se, jaké možnosti ověřování vhodných pro vaši organizaci. Tato tabulka ukazuje také formátování požadavky pro případy, kdy zadání dat jménem uživatele ze vstupní cesty, které ověřují tato data.

> [AZURE.NOTE] Telefon do kanceláře na portálu registrace nezobrazí, protože uživatelé můžou momentálně není úprava tuto vlastnost v adresáři.

<table>
          <tbody><tr>
            <td>
              <p>
                <strong>Způsob kontaktování název</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Údaj, který Azure Active Directory</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Použité / nastavit místo, kam?</strong>
              </p>
            </td>
            <td>
              <p>
                <strong>Požadavky na formát</strong>
              </p>
            </td>
          </tr>
          <tr>
            <td>
              <p>Telefon do kanceláře</p>
            </td>
            <td>
              <p>TelefonníČíslo</p>
              <p>například Set-MsolUser - UserPrincipalName JWarner@contoso.com - TelefonníČíslo "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Použít:</p>
              <p>Portál resetování hesla</p>
              <p>Settable z:</p>
              <p>TelefonníČíslo je nastavit z prostředí PowerShell, DirSync, portálu Správa Azure a portál správy Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (například 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Nutné zadat směrové číslo země<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Nutné zadat směrové číslo oblasti (v případě potřeby)<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Nutné zadat kód + před země<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Musí mít je mezera mezi směrové číslo země a zbytek číslo<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Rozšíření nejsou podporována, pokud máte všechny příponami zadanými jsme odstraní ho od čísla před odesláním hovoru.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Mobilní telefon</p>
            </td>
            <td>
              <p>AuthenticationPhone</p>
              <p>NEBO</p>
              <p>Mobilní telefon</p>
              <p>(Ověřování telefonu se používá při prezentování dat, jinak to přejde poli mobilní telefon).</p>
              <p>například Set-MsolUser - UserPrincipalName JWarner@contoso.com mobilní telefon – "+ 1 1234567890 x 1234"</p>
            </td>
            <td>
              <p>Použít:</p>
              <p>Portál resetování hesla</p>
              <p>Registrace portálu</p>
              <p>Settable z: </p>
              <p>AuthenticationPhone je nastavit z portálu registrace resetovat heslo nebo MFA registrace portál.</p>
              <p>Mobilní telefon se nastavit z prostředí PowerShell, DirSync, portálu Správa Azure a portál správy Office</p>
            </td>
            <td>
              <p>+ ccc xxxyyyzzzz (například 1234567890 + 1)</p>
              <ul>
                <li class="unordered">
Musíte zadat směrové číslo země.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Musíte zadat směrové číslo (pokud to jde).<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Musí mít poskytovat + před země kód.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Musí mít je mezera mezi směrové číslo země a zbytek číslo.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
Rozšíření nejsou podporována, pokud máte všechny rozšíření zadali, jsme ignorujte ji při odesílání hovoru.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Alternativní e-mailu</p>
            </td>
            <td>
              <p>AuthenticationEmail</p>
              <p>NEBO</p>
              <p>AlternateEmailAddresses [0] </p>
              <p>(Ověření e-mailu se používá při prezentování dat, jinak to přejde do pole alternativní E-mail).</p>
              <p>Poznámka: pole alternativní e-mailovou není zadán jako matice řetězců v adresáři.  V této matici používáme první položku.</p>
              <p>například Set-MsolUser - UserPrincipalName JWarner@contoso.com - AlternateEmailAddresses"email@live.com"</p>
            </td>
            <td>
              <p>Použít:</p>
              <p>Portál resetování hesla</p>
              <p>Registrace portálu</p>
              <p>Settable z: </p>
              <p>AuthenticationEmail je nastavit z portálu registrace resetování hesla nebo MFA registrace portál.</p>
              <p>AlternateEmail je nastavitelné z prostředí PowerShell, portálu pro správu Azure a portál správy Office</p>
            </td>
            <td>
              <p>
                <a href="mailto:user@domain.com">user@domain.com</a>nebo甲斐@黒川.日本</p>
              <ul>
                <li class="unordered">
E-mailů postupujte podle standardní formátování jako za.<br><br></li>
              </ul>
              <ul>
                <li class="unordered">
E-mailů Unicode jsou podporovány.<br><br></li>
              </ul>
            </td>
          </tr>
          <tr>
            <td>
              <p>Zabezpečení otázek a odpovědí</p>
            </td>
            <td>
              <p>Chcete-li změnit přímo v adresáři není k dispozici.</p>
            </td>
            <td>
              <p>Použít:</p>
              <p>Portál resetování hesla</p>
              <p>Registrace portálu </p>
              <p>Settable z: </p>
              <p>Jediný způsob, jak nastavit dotazy zabezpečení je prostřednictvím portálu pro správu Azure.</p>
              <p>Jediný způsob, jak nastavit odpovědi na otázky zabezpečení pro daného uživatele je portálu registrace.</p>
            </td>
            <td>
              <p>Dotazy zabezpečení obsahovat maximálně 200 znaků a min 3 znaky</p>
              <p>Odpovědi mají max 40 znaků a min 3 znaky</p>
            </td>
          </tr>
        </tbody></table>

###<a name="how-to-access-password-reset-data-for-your-users"></a>Jak získat přístup ke heslo resetovat dat pro vaše uživatele
####<a name="data-settable-via-synchronization"></a>Nastavit prostřednictvím synchronizace dat
Tato pole můžou být synchronizované z místního:

* Mobilní telefon
* Telefon do kanceláře

####<a name="data-settable-with-azure-ad-powershell"></a>Nastavit s Azure AD PowerShell dat
Tato pole jsou dostupné s Azure AD PowerShell a rozhraní API grafu:

* Alternativní e-mailu
* Mobilní telefon
* Telefon do kanceláře
* Ověřování telefonu
* Ověření e-mailu

####<a name="data-settable-with-registration-ui-only"></a>Nastavit s registrací uživatelského rozhraní pouze data
Následující pole jsou pouze přístupných registrace SSPR uživatelského rozhraní (https://aka.ms/ssprsetup):

* Zabezpečení otázek a odpovědí

####<a name="what-happens-when-a-user-registers"></a>Co se stane, když uživatel zaregistruje?
Když uživatel zaregistruje, stránce registrace bude **vždy** nastavit následující pole:

* Ověřování telefonu
* Ověření e-mailu
* Zabezpečení otázek a odpovědí

Pokud jste zadali hodnotu pro **mobilní telefon** nebo **Alternativní e-mailu**, uživatelé můžete hned začít používat můžou být o resetování hesla, i když nebyly registrované pro službu.  Kromě toho uživatelé budou při registraci poprvé, najdete v článku jsou tyto hodnoty a je upravit, pokud chtějí.  Však po úspěšném registraci, tyto hodnoty budou zachovat v polích **Telefonu ověřování** a **Ověření e-mailu** v tomto pořadí.

To může být užitečné způsob, jak zrušit blokování velkého množství uživatelé můžou používat SSPR zároveň umožňuje uživatelům ověřit tyto informace prostřednictvím procesu registrace.

####<a name="setting-password-reset-data-with-powershell"></a>Nastavení hesla resetovat dat pomocí prostředí PowerShell
Můžete nastavit hodnoty do těchto polí s Azure AD Powershellu.

* Alternativní e-mailu
* Mobilní telefon
* Telefon do kanceláře

Nejdřív se musíte nejdřív si [Stáhněte a nainstalujte modul Azure AD Powershellu](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Až budete mít nainstalovaný, můžete postupujte podle návodu pro nastavení každého pole.

#####<a name="alternate-email"></a>Alternativní e-mailu
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -AlternateEmailAddresses @("email@domain.com")
```

#####<a name="mobile-phone"></a>Mobilní telefon
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -MobilePhone "+1 1234567890"
```

#####<a name="office-phone"></a>Telefon do kanceláře
```
Connect-MsolService
Set-MsolUser -UserPrincipalName user@domain.com -PhoneNumber "+1 1234567890"
```

####<a name="reading-password-reset-data-with-powershell"></a>Resetování hesla čtení dat pomocí prostředí PowerShell
Přečtěte si hodnoty do těchto polí s Azure AD Powershellu.

* Alternativní e-mailu
* Mobilní telefon
* Telefon do kanceláře
* Ověřování telefonu
* Ověření e-mailu

Nejdřív se musíte nejdřív si [Stáhněte a nainstalujte modul Azure AD Powershellu](https://msdn.microsoft.com/library/azure/jj151815.aspx#bkmk_installmodule).  Až budete mít nainstalovaný, můžete postupujte podle návodu pro nastavení každého pole.

#####<a name="alternate-email"></a>Alternativní e-mailu
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select AlternateEmailAddresses
```

#####<a name="mobile-phone"></a>Mobilní telefon
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select MobilePhone
```

#####<a name="office-phone"></a>Telefon do kanceláře
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select PhoneNumber
```

#####<a name="authentication-phone"></a>Ověřování telefonu
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select PhoneNumber
```

#####<a name="authentication-email"></a>Ověření e-mailu
```
Connect-MsolService
Get-MsolUser -UserPrincipalName user@domain.com | select -Expand StrongAuthenticationUserDetails | select Email
```

## <a name="links-to-password-reset-documentation"></a>Odkazy na heslo resetovat si přečtěte následující dokumentaci
Tady jsou odkazy na všech stránkách resetování hesla Azure AD si přečtěte následující dokumentaci:

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
* [**Jak to funguje**](active-directory-passwords-how-it-works.md) – Další informace o šest jednotlivých součástí tuto službu a co každý znamená
* [**Začínáme**](active-directory-passwords-getting-started.md) – zjistěte, jak vám umožní uživatelům obnovit a změnit jejich cloudu a místní hesla
* [**Vlastní**](active-directory-passwords-customize.md) – zjistěte, jak přizpůsobit vzhled a chování a nastavení služby potřebám vaší organizace
* [**Doporučené postupy**](active-directory-passwords-best-practices.md) : Naučte se nasadit rychlé a efektivní práce s hesly ve vaší organizaci
* [**Dozvědět se víc**](active-directory-passwords-get-insights.md) – Další informace o naše integrované možnosti vytváření sestav
* [**Nejčastější dotazy týkající se**](active-directory-passwords-faq.md) -odpovědi na časté otázky
* [**Poradce při potížích**](active-directory-passwords-troubleshoot.md) – zjistěte, jak rychle vyřešit problémy se službou



[001]: ./media/active-directory-passwords-learn-more/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-learn-more/002.jpg "Image_002.jpg"
