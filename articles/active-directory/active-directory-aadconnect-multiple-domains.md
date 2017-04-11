<properties
    pageTitle="Azure AD Connect víc domén"
    description="Tento dokument popisuje nastavení a konfiguraci víc domén nejvyšší úrovně s O365 a Azure AD."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>Podpora více domény pro federování s Azure AD
Následující dokumentaci obsahuje pokyny o tom, jak používat víc domén prvního řádu a dílčích domén s když federování s Office 365 nebo Azure AD domény.

## <a name="multiple-top-level-domain-support"></a>Podpora více doména nejvyšší úrovně
Federování víc domén prvního řádu s Azure AD vyžaduje některé další konfiguraci vyžadovanou není při federování s jeden doména nejvyšší úrovně.

Když doménu je federované se Azure AD, několik vlastnosti jsou nastaveny na doménu v Azure.  Důležité jednoho je IssuerUri.  Toto je identifikátor URI, který používá Azure AD pro identifikaci domény, kterému je přidružen tokenu.  Identifikátor URI nevyžaduje cokoli ale překládal musí být platná URI.  Ve výchozím nastavení Azure AD Nastaví tuto hodnotu identifikátor federace služby ve vaší místní AD FS konfigurace.

>[AZURE.NOTE]Identifikátor federace služby je URI, které jednoznačně identifikuje federace služby.  Federace služby je instancí AD FS, která funguje jako služba tokenů zabezpečení. 

Můžete zobrazit IssuerUri pomocí příkazu Powershellu `Get-MsolDomainFederationSettings - DomainName <your domain>`.

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

K potížím, když chcete přidat víc domén prvního řádu.  Například Řekněme, že máte nastavení federace mezi Azure AD a místního prostředí.  Pro tento dokument používám bmcontoso.com.  Teď můžu mít druhý, nejvyšší úrovně doménu přidali, bmfabrikam.com.

![Domény](./media/active-directory-multiple-domains/domains.png)

Když jsme pokusí o převedení naše domény bmfabrikam.com být federované, jsme dojít k chybě.  Důvodem je Azure AD má omezení, který neumožňuje vlastnost IssuerUri mít stejnou hodnotu pro víc než jednu doménu.  
  

![Federace chyby](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>Parametr SupportMultipleDomain

K řešení, potřebujete přidat různé IssuerUri, které lze provést pomocí `-SupportMultipleDomain` parametr.  Tento parametr používá pomocí těchto rutin:
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

Tento parametr zajišťuje Azure AD nakonfigurovat IssuerUri tak, aby je založena na název domény.  To bude jedinečný přes adresáře v Azure AD.  Použití parametru umožňuje příkazu Powershellu úspěšné dokončení.

![Federace chyby](./media/active-directory-multiple-domains/convert.png)

Hledání v rámci nastavení našeho nového bmfabrikam.com domény můžete najdete v těchto článcích:

![Federace chyby](./media/active-directory-multiple-domains/settings.png)

Všimněte si, že `-SupportMultipleDomain` nemění jiných koncové body pořád nakonfigurovaných tak, aby ukazovaly naší služby federace na adfs.bmcontoso.com.

Další věc, `-SupportMultipleDomain` znamená je, že zajišťuje, že služba AD FS systému zahrnuje správnou hodnotu Vystavitel tokeny vydané pro Azure AD. Je to tak, že doména uživatelů UPN toto nastavení jako domény v IssuerUri, tedy https://{upn přípona} / služby AD FS, služby a zabezpečení. 

Proto při ověřování Azure AD nebo Office 365, element IssuerUri v tokenu uživatele slouží k vyhledání domény v Azure AD.  Pokud nelze najít, že ověření se nepovede. 

Pokud uživatel UPN je například bsimon@bmcontoso.com, element IssuerUri otázky token AD FS nastaví se http://bmcontoso.com/adfs/services/trust. To bude shodovat s konfigurací Azure AD a bude ověření úspěšné.

Následující obrázek je pravidlo přizpůsobený deklarace implementující tento použití logických operátorů:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]Abyste mohli používat přepínačem - SupportMultipleDomain při pokusu o přidání nového nebo již převést přidání domény, musíte mít nastavení federované zabezpečení pro podporu původně.  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>Jak aktualizovat vztah důvěryhodnosti mezi AD FS a Azure AD
Pokud není nastaven federované vztah důvěryhodnosti mezi AD FS a instanci aplikace Azure AD, budete muset znovu vytvořit toto zabezpečení.  Důvodem je, když ho je původně nastavení bez `-SupportMultipleDomain` , IssuerUri je parametr nastaven s výchozí hodnotou.  V snímek dole můžete vidět, že že issueruri je nastavený na https://adfs.bmcontoso.com/adfs/services/trust.

Proto nyní, když úspěšně přidali novou doménu na portálu Azure AD a pokuste se převést pomocí `Convert-MsolDomaintoFederated -DomainName <your domain>`, jsme zobrazí následující chyba.

![Federace chyby](./media/active-directory-multiple-domains/trust1.png)

Pokud se pokusíte přidat `-SupportMultipleDomain` přepnout jsme se zobrazí chybová zpráva:

![Federace chyby](./media/active-directory-multiple-domains/trust2.png)

Jednoduše pokusu o spuštění `Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain` na původní doméně také způsobí chybu.

![Federace chyby](./media/active-directory-multiple-domains/trust3.png)

Přidání dalších domén prvního řádu pomocí následujících kroků.  Pokud jste doménu už přidali a nepoužili `-SupportMultipleDomain` parametru start s tímto postupem odeberete a aktualizace vaší původní doméně.  Pokud jste ještě nepřidali doména nejvyšší úrovně ještě můžete začít s kroků popisujících přidání domény pomocí prostředí PowerShell Azure AD Connect.

Pomocí následujících kroků odebrat zabezpečení Microsoft Online a aktualizace vaší původní doméně.

2.  Na server služby AD FS federační otevřít **AD FS Management.** 
2.  V levé části rozbalte **Vztahy důvěryhodnosti** a **Může stran vztah důvěryhodnosti**
3.  V pravé části odstranění položky **Identity platformu Microsoft Office 365** .
![Odebrání Microsoft Online](./media/active-directory-multiple-domains/trust4.png)
1.  Na počítač s nainstalovaným [Modul Azure Active Directory pro Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) spusťte následující: `$cred=Get-Credential`.  
2.  Zadejte uživatelské jméno a heslo globálního správce pro domain Azure AD, které jsou federování s
2.  V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`
4.  V prostředí PowerShell zadejte `Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`.  Toto je původní doméně.  Tak použití výše uvedených domén, ve kterých je:`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


Pomocí následujících kroků přidat nové nejvyšší úrovně domény pomocí prostředí PowerShell

1.  Na počítač s nainstalovaným [Modul Azure Active Directory pro Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx) spusťte následující: `$cred=Get-Credential`.  
2.  Zadejte uživatelské jméno a heslo globálního správce pro domain Azure AD, které jsou federování s
2.  V prostředí PowerShell zadejte`Connect-MsolService -Credential $cred`
3.  V prostředí PowerShell zadejte`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

Pomocí následujících kroků přidat nové doména nejvyšší úrovně pomocí Azure AD Connect.

1.  Spuštění Azure AD Connect ze stolního počítače nebo nabídky start
2.  Zvolte "Přidat další doménu Azure AD" ![přidat další domain Azure AD](./media/active-directory-multiple-domains/add1.png)
3.  Zadejte Azure AD a přihlašovací údaje k Active Directory
4.  Vyberte druhý doménu, kterou chcete konfigurovat pro federaci.
![Přidání dalších domain Azure AD](./media/active-directory-multiple-domains/add2.png)
5.  Klikněte na nainstalovat


### <a name="verify-the-new-top-level-domain"></a>Ověření seznamu nová doména nejvyšší úrovně
Pomocí příkazu Powershellu `Get-MsolDomainFederationSettings - DomainName <your domain>`můžete zobrazit aktualizované IssuerUri.  Následující snímek obrazovky zobrazuje federace nastavení byly aktualizovány naše původní http://bmcontoso.com/adfs/services/trust domény

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

A nastavila IssuerUri na naše novou doménu na https://bmfabrikam.com/adfs/services/trust

![Get-MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>Podpora pro dílčích domén
Po přidání domény, z důvodu domény způsobem Azure AD zpracování zdědí nastavení nadřazeného.  To znamená, že IssuerUri musí odpovídat nadřazených.

Ano, umožní přivítejte například, že se mají bmcontoso.com a pak přidejte corp.bmcontoso.com.  To znamená, že IssuerUri pro uživatele z corp.bmcontoso.com musí být **http://bmcontoso.com/adfs/services/trust.**  Však standardní pravidlo implementovaná nad pro Azure AD vygeneruje token s Vystavitel jako **http://corp.bmcontoso.com/adfs/services/trust.** které neodpovídají domény požadované hodnoty a ověření se nepovede.

### <a name="how-to-enable-support-for-sub-domains"></a>Jak povolit podpora dílčích domén
Pokud chcete tento problém vyřešit AD FS předávající stran zabezpečení pro Microsoft Online potřeba aktualizovat.  K tomuto účelu musíte nakonfigurovat vlastní deklarace pravidlo tak, aby ho odstraní vypnutí dílčích domén z přípony UPN pro uživatele při vytváření vlastní Vystavitel hodnoty. 

Následující tvrzení bude udělejte toto:

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

Pomocí následujících kroků přidat vlastní deklaraci identity pro podporu dílčích domén.

1.  Správa otevřít AD FS
2.  Klikněte pravým tlačítkem myši zabezpečení Microsoft Online RP a zvolte Upravit deklarace pravidel
3.  Vyberte třetí deklarace pravidlo a nahraďte ![upravit deklarace](./media/active-directory-multiple-domains/sub1.png)
4.  Nahraďte aktuální deklarace:
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    s
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![Nahrazení deklaraci identity](./media/active-directory-multiple-domains/sub2.png)
5.  Klikněte na Ok.  Klikněte na použít.  Klikněte na Ok.  Zavřete okno Správa AD FS.

