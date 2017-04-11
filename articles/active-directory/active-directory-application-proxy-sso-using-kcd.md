<properties
    pageTitle="Jednotné přihlašování k aplikaci Proxy | Microsoft Azure"
    description="Popisuje, jak poskytnout jednotné přihlašování pomocí proxy server Azure AD aplikace."
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
    ms.date="08/10/2016"
    ms.author="kgremban"/>


# <a name="single-sign-on-with-application-proxy"></a>Jednotné přihlašování k aplikaci Proxy

Jednotné přihlašování je důležitou součástí proxy server Azure AD aplikace. Poskytuje nejlepší možnosti uživatele pomocí následujícího postupu:

1. Uživatel přihlásí ke cloudu  
2. Všechny ověření zabezpečení dojít v cloudu (předběžné ověření)  
3. Po odeslání žádosti pro místní aplikaci zosobňuje konektoru aplikace Proxy uživatele. Aplikaci back-end předpokládá, že toto je běžný uživatel pocházejících z zařízení doméně.

![Diagram přístup z koncového uživatele pomocí aplikace Proxy k podnikové síti](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_diagram.png)

Proxy server Azure AD aplikace vám pomůže zjistit jednotné přihlašování (SSO) setkat i v případě pro uživatele. Publikování aplikací jednotné přihlašování ve službě postupujte podle následujících pokynů:


## <a name="sso-for-on-prem-iwa-apps-using-kcd-with-application-proxy"></a>Jednotné přihlašování k aplikacím IWA místní KCD pomocí aplikace Proxy
Můžete povolit jednotného přihlašování k aplikacím pomocí integrovaného ověřování systému Windows (IWA) tím, že aplikace Proxy spojnic oprávnění ve službě Active Directory zosobnění uživatelů a odesílat a přijímat tokeny nim jejich jménem.


### <a name="network-diagram"></a>Diagram sítě

Tento diagram vysvětluje tok, když uživatel pokusí o přístup k místní aplikace, která používá IWA.

![Microsoft AAD ověřování vývojový diagram](./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png)

1. Uživatel zadá adresu URL pro přístup k aplikaci místní prostřednictvím aplikace Proxy.
2. Proxy aplikace přesměruje požadavek ověřování služby Azure AD preauthenticate. V tomto okamžiku Azure AD slouží k použití použít ověřování a zásady se tak mohli ověřovat, například vícefaktorové ověřování. Pokud ověření uživatele Azure AD vytvoří token a odešle uživateli.
3. Uživatel předá tokenu Proxy aplikace.
4. Aplikace Proxy ověří tokenu získá uživatel hlavní název (UPN) z něho a ke spojnici připojit přes obousměrně ověřené zabezpečeného kanálu pošle žádost, s UPN a služby základní název (hlavní název služby).
5. Spojnice provádí vyjednávání delegování omezena pouze pomocí protokolu Kerberos (KCD) s místní AD zosobnění uživatele získat tokenu protokolu Kerberos na aplikace.
6. Služby Active Directory odešle tokenu protokolu Kerberos aplikace ke spojnici připojit.
7. Spojnice rozešle původní žádost o aplikační server pomocí protokolu Kerberos tokenu přijaté od AD.
8. Aplikace odešle odpověď na spojnici, kterou pak obnoven Proxy aplikace a nakonec uživateli.

### <a name="prerequisites"></a>Zjistit předpoklady pro

Než začnete s jednotným Přihlašováním pro aplikace Proxy, zkontrolujte, jestli že už se dá se následující nastavení a konfigurace prostředí:

- Vaše aplikace, jako je Web služby SharePoint apps se nastavit, aby používal integrované ověřování systému Windows. Další informace najdete v článku [Povolení podpory pro ověřování protokolem Kerberos](https://technet.microsoft.com/library/dd759186.aspx)nebo SharePoint najdete v článku [Plánování ověřování protokolem Kerberos v Sharepointu 2013](https://technet.microsoft.com/library/ee806870.aspx).

- Vaše aplikace mají názvy hlavní služby.

- Serveru spojnici a serveru aplikaci jsou domény připojen a část stejné domény nebo důvěryhodné domény. Další informace o připojení k doméně najdete v článku [připojení počítače k doméně](https://technet.microsoft.com/library/dd807102.aspx).

- Serveru konektoru má přístup pro čtení TokenGroupsGlobalAndUniversal pro uživatele. Toto je výchozí nastavení, které můžou mít vliv zabezpečení hardening prostředí. Získáte další nápovědu v [KB2009157](https://support.microsoft.com/en-us/kb/2009157).

### <a name="active-directory-configuration"></a>Konfigurace služby Active Directory

Konfigurace služby Active Directory se liší podle toho, zda na spojnici Proxy aplikací a publikované serveru jsou ve stejné domény nebo ne.

#### <a name="connector-and-published-server-in-the-same-domain"></a>Spojnice a publikované serveru ve stejné doméně

1. Ve službě Active Directory, přejděte na **Nástroje** > **uživatelů a počítačů**.
2. Vyberte spojnici serveru.
3. Klikněte pravým tlačítkem myši a vyberte **Vlastnosti** > **delegování**.
4. Vyberte **zabezpečení počítače delegování určeným službám** a v části **služeb, ke kterým tento účet Visio svádět k zahrnutí delegovaných pověření**, zadejte hodnotu pro hlavního názvu služby identit aplikační server.
5. Díky konektoru aplikace Proxy zosobnění uživatelů ve službě Active Directory proti definované v seznamu aplikací.

![Snímek obrazovky okna vlastností SVR spojnice](./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg)

#### <a name="connector-and-published-server-in-different-domains"></a>Spojnice a publikované serveru v různých domén

1. Seznam požadavky pro práci s KCD přes domény najdete v tématu [Kerberos vynucené delegování přes domény](https://technet.microsoft.com/library/hh831477.aspx).
2. V systému Windows 2012 R2, použít `principalsallowedtodelegateto` vlastnost na serveru spojnice povolit aplikaci Proxy delegovat spojnice serveru, kde je publikován server `sharepointserviceaccount` a delegování server je `connectormachineaccount`.

        $connector= Get-ADComputer -Identity connectormachineaccount -server dc.connectordomain.com

        Set-ADComputer -Identity sharepointserviceaccount -PrincipalsAllowedToDelegateToAccount $connector

        Get-ADComputer sharepointserviceaccount -Properties PrincipalsAllowedToDelegateToAccount


>[AZURE.NOTE] `sharepointserviceaccount`může být účtu SPS počítače nebo účet služby, pod kterým je spuštěný fondu aplikací SPS.


### <a name="azure-classic-portal-configuration"></a>Konfigurace Azure klasické portálu

1. Publikování aplikace podle pokynů v [aplikacích publikovat s Proxy aplikace](active-directory-application-proxy-publish.md). Je nutné vybrat jako **Způsob používající** **Azure Active Directory** .
2. Až se aplikace zobrazí v seznamu aplikací, vyberte ho a klikněte na **Konfigurovat**.
3. V části **Vlastnosti**nastavení **Interního metody ověřování** **integrované**ověřování systému Windows.  
  ![Konfigurace rozšířené aplikace](./media/active-directory-application-proxy-sso-using-kcd/cwap_auth2.png)  
4. Zadejte **Interní hlavního názvu aplikace služby** aplikační server. V tomto příkladu je hlavního názvu služby pro naše publikovaná aplikace http/lob.contoso.com.  

>[AZURE.IMPORTANT] Pokud vaše místních hlavních názvů uživatelů a Přípony v Azure Active Directory nejsou stejné, budete muset konfigurace [delegované přihlašovací identitu](#delegated-login-identity) popořádku pro používající pracovat.

|  |  |
| --- | --- |
| Vnitřní ověřování | Pokud používáte Azure AD pro používající, můžete nastavit interní ověřování uživatelům výhody jednotné přihlašování (SSO) k této aplikaci. <br><br> **Integrované ověřování systému Windows** (IWA) vyberte, pokud vaše aplikace používá IWA a můžete nakonfigurovat pomocí protokolu Kerberos omezena pouze delegování (KCD) povolit jednotné přihlašování k této aplikaci. Aplikace, které využívají IWA musí být nakonfigurované používání KCD, jinak Proxy aplikace nebude možné publikovat tyto aplikace. <br><br> Pokud aplikace nepoužívá IWA, vyberte **žádné** . |
| Interní aplikace hlavního názvu služby | Toto je název jistinu služby (hlavní název služby) interní aplikace nakonfigurovaná v místní Azure AD. Hlavní název služby používá Proxy konektoru aplikace vzdáleně Kerberos tokeny pro aplikaci pomocí KCD. |


## <a name="sso-for-non-windows-apps"></a>Jednotné přihlašování k aplikacím systému Windows
Tok delegování Kerberos v Azure AD aplikace Proxy, spustí Azure AD ověřuje uživatele v cloudu. Jakmile žádost přijde místní Proxy konektoru aplikace Azure AD problémy lístek protokolu Kerberos jménem uživatele tak, že interakce s místní služby Active Directory. Tento proces se označuje jako Kerberos omezena pouze delegování (KCD). Ve fázi další žádost o odeslaný do back-end aplikaci tento lístek Kerberos. Existuje celá řada protokolů, které definují odeslání žádosti. Většina serverů – Windows očekávat potvrdit/SPNego teď podporovaný na proxy server Azure AD aplikace.

![Jednotné přihlašování non-Windows diagramu](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_nonwindows_diagram.png)

### <a name="delegated-login-identity"></a>Delegovaná přihlašovací identitu
Delegovaná přihlašovací identitu umožňuje zpracovat dva různé přihlášení scénáře:

- Windows jiné aplikace, které obvykle získat identita uživatele ve formuláři uživatelské jméno nebo název účtu SAM, ne e-mailovou adresu (username@domain).
- Alternativní přihlašovací konfigurace kde UPN v Azure AD a Přípony v na adresářová služba Active Directory se liší.

U aplikace Proxy můžete vybrat které identity používat k získání lístek Kerberos. Toto nastavení je na aplikace. Některé z těchto možností jsou vhodné pro systémy, které nepřijímá e-mailovou adresu formát, ostatní jsou sice pro alternativní přihlašovací.

![Snímek obrazovky parametr identity delegované přihlášení](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)

Když je použita delegované přihlašovací identitu, nemusí být hodnota jedinečné pro všechny domény nebo strukturami ve vaší organizaci. Tento problém se můžete vyhnout publikováním tyto aplikace dvakrát pomocí dvou různých skupin spojnice. Vzhledem k tomu, že každá aplikace má jiný uživatel cílovou skupinu, se můžete připojit jeho konektory na jinou doménu.


## <a name="working-with-sso-when-on-premises-and-cloud-identities-are-not-identical"></a>Práce s jednotným Přihlašováním při místním prostředím a cloudu identity nejsou stejné
Pokud jinak nakonfigurován, Proxy aplikace předpokládá, že uživatelé mají stejné identity v cloudu a místní. Můžete nakonfigurovat, pro každou aplikaci, která identita bude použito při provádění jednotného přihlašování.  

Tato možnost umožňuje mnoho organizacím, které mají různé cloudu a místní identit třeba jednotné přihlašování z cloudu pro místní aplikace bez nutnosti uživatelé můžou zadat jinou uživatelská jména a hesla. Jedná se o organizace, která:

- Máte víc domén interně (joe@us.contoso.com, joe@eu.contoso.com) a jednu doménu v cloudu (joe@contoso.com).

- Máte název domény směrovat interně (joe@contoso.usa) a právní v cloudu.

- Nepoužívejte názvy domén interně (Jan)

- Použití různých aliasů místní i schránkami v cloudu. Například joe-johns@contoso.coma.joej@contoso.com  

Také vám pomůže s aplikací, které nepřijímá adresy ve formě e-mailovou adresu, která je velmi běžné situace pro servery systému Windows back-end.


### <a name="setting-sso-for-different-cloud-and-on-prem-identities"></a>Nastavení jednotného přihlašování pro různé cloudu a místní identity

1. Konfigurace nastavení Azure AD Connect hlavní identita budou e-mailovou adresu (pošta). Důvodem je jako součást procesu přizpůsobit změnou pole **Uživatelské jméno zásady** v nastavení synchronizace. Poznámka: aby tato nastavení také určit, jak uživatelé přihlásit k Office 365, Windows10 zařízení a ostatní aplikace, které využívají Azure AD jako své identity úložiště.  
  ![Určení uživatelů – snímek obrazovky rozevíracího seznamu hlavního názvu uživatele](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_connect_settings.png)  
2. V dialogovém okně Nastavení konfigurace aplikace na aplikaci, kterou chcete upravit vyberte **Delegované přihlašovací identitu** mají být použity:
  - Uživatelské jméno zásady:joe@contoso.com  
  - Princip alternativní uživatelské jméno:joed@contoso.local  
  - Uživatelské jméno součástí zásady uživatelské jméno: Jana  
  - Uživatelské jméno součástí alternativní uživatelské jméno zásada: joed  
  - Název účtu SAM místní: v závislosti místní domény řadiče domény konfigurace

  ![Snímek obrazovky rozevírací nabídka delegované přihlášení identity](./media/active-directory-application-proxy-sso-using-kcd/app_proxy_sso_diff_id_upn.png)  

### <a name="troubleshooting-sso-for-different-identities"></a>Poradce při potížích s jednotného přihlašování pro různé identity
Pokud dojde k chybě při přihlašování zobrazí v protokolu spojnice počítač událostí způsobem popsaným v tématu [Poradce při potížích](active-directory-application-proxy-troubleshoot.md).
Ale v některých případech úspěšně pošle žádost aplikaci back-end během této aplikace odpoví v různých dalších odpovědí HTTP. Poradce při potížích s takovýchto případech by měly začít porovnáním číslo události 24029 v počítači spojnice v protokolu událostí relace Proxy aplikace. Identita uživatele, která byla použita pro delegování se zobrazí v poli "uživatel" v rámci podrobností o události. Zapnout protokol relace, vyberte **Zobrazit analytický protokoly o ladění a** v případě, že prohlížeč zobrazení nabídky.


## <a name="see-also"></a>Viz taky

- [Publikování aplikací s Proxy aplikace](active-directory-application-proxy-publish.md)
- [Řešení potíží s aplikací Proxy](active-directory-application-proxy-troubleshoot.md)
- [Práce s aplikací deklarací](active-directory-application-proxy-claims-aware-apps.md)
- [Povolení podmíněné přístupu](active-directory-application-proxy-conditional-access.md)

Nejnovější příspěvky a aktualizace najdete v článku [aplikace Proxy blogu](http://blogs.technet.com/b/applicationproxyblog/)


<!--Image references-->
[1]: ./media/active-directory-application-proxy-sso-using-kcd/AuthDiagram.png
[2]: ./media/active-directory-application-proxy-sso-using-kcd/Properties.jpg
