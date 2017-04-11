<properties
    pageTitle="Co je funkce Samoobslužné registrace Azure? | Microsoft Azure"
    description="Přehled samoobslužné registrace Azure, jak spravovat proces zápisu a jak převzít řízení názvu domény DNS."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Co je funkce Samoobslužné registrace Azure?

Toto téma vysvětluje postup samoobslužné registrace a řešení převzít řízení názvu domény DNS.  

## <a name="why-use-self-service-signup"></a>Proč používat samoobslužné registrace?

- Získejte zákazníků služeb, které mají být rychlejší.
- Vytvoření nabídky k e mailové služby.
- Vytvoření e mailové registrace toků rychle umožnit uživatelům vytvářet identit pomocí své práce snadno zapamatovatelné e-mailových aliasů.
- Nespravované Azure adresářů půjde do spravovaných adresářů později a znovu použít pro další služby.

## <a name="terms-and-definitions"></a>Pojmy a definice

+ **Samoobslužné registrace**: Toto je způsob uživatel zaregistruje ke cloudové službě a má podle identitu v Azure Active Directory (Azure AD) je automaticky vytvoří jejich e-mailové domény.
+ **Adresář nespravované Azure**: je to adresář, kde je vytvořena tato identita. Nespravované služby directory je adresář, který má žádný globální správce.
+ **Ověření e-mailu uživatelů**: Toto je typ uživatelský účet v Azure AD. Uživatel, který má identitu automaticky vytvoří po registraci k nabídce samoobslužné se označuje jako uživatel ověření e-mailu. Uživatel aplikace ověření e-mailu je členem adresář označený creationmethod běžná = EmailVerified.

## <a name="user-experience"></a>Uživatelské prostředí

Například Řekněme, že uživatele, jehož e-mailu Dan@BellowsCollege.com obdrží citlivé soubory prostřednictvím e-mailu. Soubory, které mají zamknutí tak, že Azure Rights Management (Azure RMS). Ale Dan v organizaci, vlnovce vysokoškolský nebyla registraci službou Azure RMS ani má nasazené Active Directory RMS. V tomto případě Dan můžou zaregistrovat ke neváhejte RMS pro jednotlivce za účelem čtení chráněné soubory.

Pokud Dan je první uživatele s e-mailovou adresu z BellowsCollege.com se zaregistrovat k této samoobslužné nabízí, pak nespravované adresáře se vytvoří BellowsCollege.com v Azure AD. Pokud ostatní uživatelé z domény BellowsCollege.com registraci tuto nabídku nebo podobné samoobslužné nabízení, mají uživatelé také ověření e-mailu uživatelských účtů vytvořených v adresáři stejné nespravované v Azure.

## <a name="admin-experience"></a>Možnosti správy

Správce, který vlastní název domény DNS nespravované Azure adresáře můžete převzít řízení nebo sloučit v adresáři po ověřením vlastnictví. Možnosti správy podrobněji popsán v následujících částech, ale tady je souhrn:

- Když převzít řízení nespravované Azure directory stanete jednoduše globální správce nespravované adresář. Toto je někdy se jí říká interní převzetí.
- Při sloučení nespravované Azure adresáře, přidejte název domény DNS nespravované adresáře do adresáře spravovaných Azure a se vytvoří mapování uživatelé zdrojů tak uživatelé můžou dál přístup ke službám bez přerušení. Toto je někdy se jí říká externí převzetí.

## <a name="what-gets-created-in-azure-active-directory"></a>Co se vytvářejí v Azure Active Directory?

#### <a name="directory"></a>adresář

- Adresář služby Azure Active Directory pro doménu se vytvoří, jeden adresář na domény.
- V adresáři Azure AD má žádný globální správce.

#### <a name="users"></a>Uživatelé

- U každého uživatele, kteří zaregistruje je vytvořen objekt uživatele v adresáři Azure AD.
- Každý uživatel objekt je označený jako externí.
- Každý uživatel je uveden přístup ke službě se zaregistrovali.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Jak žádat samoobslužné Azure AD adresáře pro doménu vlastním?

Převzetí samoobslužné Azure AD adresáře ověřením domény. Ověření domény ukáže, že jste vlastníkem domény tak, že vytvoříte záznamy DNS.

Postup převzetí DNS adresáře Azure AD dvěma způsoby:

- vnitřní převzetí (Správce zjistí nespravované Azure adresář a chce přeměna spravovaný adresář)
- externí převzetí (správce pokusí přidat novou doménu na jejich spravovaný Azure adresář)

Nejspíš vás bude zajímat ověření, že vlastníte doménu, protože nespravované služby directory jsou převzetí po provedení samoobslužné registrace nebo může být přidání novou doménu na existující spravovaných adresář. Například máte doménu s názvem contoso.com a chcete přidat novou doménu s názvem contoso.co.uk nebo contoso.uk.

## <a name="what-is-domain-takeover"></a>Co je převzetí doménu?  

Tato část popisuje, jak ověřit, že jste vlastníkem domény

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Co je ověření domény a proč slouží?

K provedení operace na adresář, Azure AD vyžaduje ověření vlastnictví domény DNS.  Ověření domény můžete převzít adresář a buď podporovat adresáři samoobslužné spravovaný adresář nebo sloučit adresáři samoobslužné do existující spravovaných adresář.

## <a name="examples-of-domain-validation"></a>Příklady ověření domény

Postup převzetí DNS adresáře dvěma způsoby:

+ vnitřní převzetí (například správce zjistí samoobslužných funkcí, nespravované adresář a chce přeměna spravovaný adresář)
+ externí převzetí (například jako správce pokusí přidat novou doménu na spravovaný adresář)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Vnitřní převzetí – zvýšení úrovně samoobslužných funkcí, nespravované adresář s spravovaný adresář

Po provedení interní převzetí adresáři převeden z adresáře nespravované do spravovaný adresář. Musíte udělat ověření název domény DNS, kde můžete vytvářet záznamu TXT nebo MX záznam v zóny DNS. Akce:

+ Ověří, že jste vlastníkem domény
+ Umožňuje spravovat v adresáři
+ Díky globální správce v adresáři

Řekněme, že správce IT z vlnovce vysokoškolské zjistí, že uživatelé ze školy zaregistroval samoobslužné nabídky. Jako registrovaných vlastníka DNS názvu BellowsCollege.com, správce IT můžete ověřit vlastnictví názvu DNS v Azure a potom převzít řízení nespravované adresář. V adresáři pak bude spravovaný adresář a správce IT přiřazenou roli globálního správce pro BellowsCollege.com adresář.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externí převzetí - sloučit adresář samoobslužné do existujícího spravovaných adresáře

V externí převzetí již máte spravovaný adresář a chcete všem uživatelům a skupinám z adresáře nespravované ke spravované adresáře, nikoli vlastní dvě oddělit adresáře.

Jako správce spravovaný adresář přidání domény a tuto doménu se stane s mít nespravované adresář s ním spojené.

Například Řekněme, že jste správce IT a již máte spravovaný adresář Contoso.com, název domény, které máte zaregistrované pro vaši organizaci. Zjistíte, že uživatelé z vaší organizace provedli samoobslužné registrace nahoru pro nabídky pomocí e-mailovou doménou user@contoso.co.uk, tedy jiný název domény, který vaše organizace vlastní. Tito uživatelé aktuálně mají účty v nespravovaném adresář pro contoso.co.uk.

Nechcete, aby ke správě dvou samostatných adresáře, takže sloučit adresáři nespravované contoso.co.uk do adresáře existující IT spravovaných pro contoso.com.

Externí převzetí spočívá v ověření procesu stejné DNS jako interní převzetí.  Přičemž rozdíl: uživatele a služby jsou znovu namapovat k adresáři IT spravovat.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Co je vlivu fungoval externí převzetí?

S externím převzetí se vytvoří mapování uživatelé zdrojů tak, aby uživatelé můžou dál přístup ke službám bez přerušení. Mnoha aplikacích, včetně RMS pro jednotlivce, zpracujte mapování uživatelé zdroje dobře a uživatelé můžou dál zobrazíte tyto služby beze změny. Pokud aplikace není efektivně zpracovat mapování uživatelé zdrojů, může být externí převzetí explicitně blokována uživatelům špatné prostředí.

#### <a name="directory-takeover-support-by-service"></a>převzetí podpory adresářů prostřednictvím služby

Momentálně podporují těchto služeb převzetí:

- RMS


Následující služby bude brzy bude k dispozici podpora převzetí:

- PowerBI

Následující co nedělat a vyžadují pro migraci uživatelská data po externí převzetí akce další správce.

- SharePoint nebo OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Jak provádět převzetí název domény DNS

Máte několik možností, jak provádět ověření domény (a převzetí Chcete-li):

1.  Portál Správa Azure

    Převzetí se aktivuje způsobem přidání domény.  Pokud v adresáři již existuje u domény, budete mít možnost provádět externí převzetí.

    Přihlaste se k portálu Azure pomocí svých přihlašovacích údajů.  Přejděte do existující adresář a potom na **Přidat doménu**.

2.  Office 365

    Možnosti na stránce [Spravovat domény](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) v Office 365 umožňuje pracovat se svými doménami a DNS záznamy. Přečtěte si téma [ověření domény v Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Prostředí Windows PowerShell

    Následující kroky nutné provést ověření pomocí prostředí Windows PowerShell.

    Krok    |   Rutina používat
    ------- | -------------
    Vytvoření objektu přihlašovacích údajů | Get-přihlašovacích údajů
    Připojení k Azure AD | Připojení MsolService
    Získejte seznam domén   | Get-MsolDomain
    Vytvářet složité  | Get-MsolDomainVerificationDns
    Vytvoření záznamu DNS   | Akce na DNS server
    Ověření tím největší oříškem    | Potvrďte MsolEmailVerifiedDomain

Příklad:

1. Připojení k Azure AD pomocí přihlašovacích údajů použitých při odpovídání na nabídky samoobslužné: import modul MSOnline $msolcred = get pověření připojení msolservice-pověření $msolcred

2. Zobrazte seznam domén:

    Get-MsolDomain

3. Spusťte rutinu Get-MsolDomainVerificationDns vytvářet složité:

    Get-MsolDomainVerificationDns – název_domény *your_domain_name* – DnsTxtRecord režimu

    Příklad:

    Get-MsolDomainVerificationDns – název_domény contoso.com – DnsTxtRecord režimu

4. Zkopírujte hodnotu (ověřovací kód) vrácený tento příkaz.

    Příklad:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. V vaše veřejné názvů DNS vytvořte záznam txt DNS, který obsahuje hodnotu, kterou jste zkopírovali v předchozím kroku.

    Název pro tento záznam je název nadřazené domény, tak je-li vytvořit tento záznam o prostředku pomocí roli DNS ze systému Windows Server, ponechejte vložit prázdný a jenom název záznamu hodnotu do textového pole

6. Spusťte rutinu potvrdit MsolDomain ověřit na výzvu:

    Potvrďte MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Příklad:

    Potvrďte MsolEmailVerifiedDomain - DomainName contoso.com

Úspěšné ověřovací kód se vrátíte do příkazového řádku bez chybu.

## <a name="how-do-i-control-self-service-settings"></a>Jak nastavit samoobslužné nastavení?

Správci mají dvou ovládacích prvcích typu samoobslužné dnes. Můžete ovládat:

- Zda uživatelé mohou připojit v adresáři prostřednictvím e-mailu.
- Zda uživatelé můžete licenci sami u aplikací a služeb.


### <a name="how-can-i-control-these-capabilities"></a>Jak může nastavit tyto funkce?

Správce může konfigurovat pomocí těchto parametrů rutinu Set-MsolCompanySettings Azure AD tyto možnosti:

+ **AllowEmailVerifiedUsers** Určuje, zda uživatele můžete vytvořit nebo připojení nespravované adresář. Pokud tento parametr má argument $false, účastnit se žádné ověření e-mailu uživatelů v adresáři.
+ **AllowAdHocSubscriptions** mezery nastavuje možnost pro uživatele k provedení samoobslužné registrace nahoru. Pokud tento parametr má argument $false, bez uživatelé mohou provádět samoobslužné registrace.


### <a name="how-do-the-controls-work-together"></a>Jak ovládacích prvků spolupracují?

Tyto dva parametry lze ve spojení definovat přesnější kontrolu nad samoobslužné registrace nahoru. Například následující příkaz bude uživatelům umožnit provádění samoobslužné registrace, pouze pokud tito uživatelé už máte nastavený účet v Azure AD (to znamená, uživatelé, kteří jsou potřeba účet ověření e-mailu, který chcete vytvořit nelze provést samoobslužné registrace nahoru):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Následující vývojový diagram vysvětluje různé kombinace těchto parametrů a výsledná podmínky adresář a samoobslužné registrace nahoru.

![][1]

Další informace a příklady použití těchto parametrů najdete v článku [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Viz taky

-  [Instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md)

-  [Azure Powershellu](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Reference pro rutiny Azure](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
