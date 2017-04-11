<properties
    pageTitle="Active Directory Federation Services Management a přizpůsobení s Azure AD Connect | Microsoft Azure"
    description="Správa AD FS s Azure AD Connect a přizpůsobení uživatelského AD FS přihlášení s Azure AD Connect a Powershellu."
    keywords="Služba AD FS, služby AD FS, služby AD FS Správa připojení AAD připojit, přihlásit, AD FS přizpůsobit, opravte zabezpečení O365, federace řídicí strany"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>Správa Active Directory Federation Services a přizpůsobení s Azure AD Connect

Tento článek podrobnosti úkoly související s Active Directory Federation Services (AD FS), kterou lze provést pomocí Microsoft Azure Active Directory připojení, iniciály ostatních běžné úlohy AD FS, které může být nutné pro dokončení konfigurace farmě služby AD FS.

| Téma | Co zahrnuje |
|:------|:-------------|
|**Správa AD FS**|
|[Oprava vztah důvěryhodnosti](#repairthetrust)| Oprava zabezpečení federaci s Office 365 |
|[Přidání server služby AD FS](#addadfsserver) | Rozbalení farmě služby AD FS s další server služby AD FS|
|[Přidat proxy server služby AD FS webové aplikace](#addwapserver) | Rozbalení farmě služby AD FS se serverem další WAP|
|[Přidání z federované domény](#addfeddomain)| Přidání z federované domény|
| **Přizpůsobení AD FS**|
|[Přidání vlastní firemní logo nebo obrázek](#customlogo)| Přizpůsobení přihlašovací stránku služby AD FS s loga společnosti a obrázku |
|[Přidání popisu přihlášení](#addsignindescription) | Přidání popisu přihlašovací stránka |
|[Úprava pravidla deklarace služby AD FS](#modclaims) | Změna služby AD FS nároky na různých situacích federace |

## <a name="ad-fs-management"></a>Správa AD FS

Azure AD Connect poskytuje nejrůznější věci související s AD FS, kterou lze provést pomocí Průvodce Azure AD Connect zásah minimální uživatele. Po dokončení instalace Azure AD Connect opětovným spuštěním Průvodce spuštěním průvodce znovu a provádět další úkoly.

### Oprava vztah důvěryhodnosti<a name=repairthetrust></a>

Azure AD Connect můžete vyhledat z aktuálního stavu zabezpečení služby AD FS a Azure Active Directory a odpovídající akcím opravit důvěřovat. Tyto kroky opravit Azure AD a zabezpečení služby AD FS.

1. Vyberte **opravit AAD a služby AD FS důvěřujete** ze seznamu dalších úkolů.
![Oprava AAD a služby AD FS zabezpečení](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. Na stránce **připojit k Azure AD** zadání přihlašovacích údajů globálního správce pro Azure AD a klikněte na tlačítko **Další**.
![Připojení k Azure AD](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. Na stránce **vzdáleného přístupu pověření** zadejte přihlašovací údaje správce domény.
![Přihlašovací údaje vzdáleného přístupu](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    Po klepnutí na tlačítko **Další**Azure AD Connect zkontrolovat stav certifikát a zobrazení všech problémů.

    ![Stát certifikátů](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **Připraveno ke konfiguraci** stránky se zobrazí v seznamu akce, které se provedou opravit důvěřovat.

    ![Připraveno ke konfiguraci](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. Klikněte na **nainstalovat** opravit důvěřovat.

>[AZURE.NOTE] Azure AD Connect se dají jenom oprava nebo act na certifikáty, které jsou automatickým podpisem. Azure AD Connect nemůže opravit certifikáty nezávislé certifikační autority.

### Přidání server služby AD FS<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD Connect vyžaduje soubor certifikátu PFX přidáte server služby AD FS. Tuto operaci proto lze provést jenom v případě, že jste nakonfigurovali farmě služby AD FS pomocí Azure AD Connect.

1. Vyberte **nasazení další federačního serveru** a klikněte na tlačítko **Další**.
![Další federačního serveru](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. Na stránce **připojit k Azure AD** zadejte svoje přihlašovací údaje globálního správce pro Azure AD a klikněte na tlačítko **Další**.
![Připojení k Azure AD](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. Zadejte doménu přihlašovací údaje správce.
![Přihlašovací údaje správce domény](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD Connect vyzve k zadání hesla PFX soubor, který jste zadali při konfiguraci farmě nové služby AD FS s Azure AD Connect. Klepněte na **Zadat heslo** k zadání hesla k souboru PFX.
![Heslo certifikátu](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![Určení certifikát SSL](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. Na stránce **Servery AD FS** zadejte název serveru nebo IP adresu, které budou přidány do farmě služby AD FS.
![Servery AD FS](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. Klikněte na tlačítko **Další** a absolvovat poslední stránka **Konfigurovat** . Po dokončení přidávání servery služby AD FS farmy Azure AD Connect bude vám nabídnuta možnost ověření propojení.
![Připraveno ke konfiguraci](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![Instalace dokončena](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### Přidání serveru proxy AD FS webové aplikace<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD Connect vyžaduje soubor certifikátu PFX přidat web aplikace proxy server. Proto budete moct tuto operaci jenom v případě, že jste nakonfigurovali farmě služby AD FS pomocí Azure AD Connect.

1. Vyberte **Nasazení Proxy webové aplikace** ze seznamu dostupných úkolů.
![Nasazení proxy serveru webové aplikace](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. Zadejte přihlašovací údaje Azure globálního správce.
![Připojení k Azure AD](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. Na stránce **certifikát SSL určit** zadejte heslo pro PFX soubor, který jste zadali při konfiguraci farmě služby AD FS s Azure AD Connect.
![Heslo certifikátu](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![Určení certifikát SSL](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. Přidání přidat jako webové aplikace proxy serveru. Protože proxy serveru webové aplikace nemusí být připojen k doméně, průvodce vyzve pro správu v části přihlašovací údaje k serveru přibývat.
![Přihlašovací údaje správce serveru](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. Na stránce **pověření zabezpečení Proxy** zadejte pověření ke konfiguraci proxy zabezpečení a přístup k primární server ve farmě služby AD FS.
![Přihlašovací údaje zabezpečení proxy serveru](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. Na stránce **Připraveno ke konfiguraci** Průvodce seznamem akcí, které se provedou.
![Připraveno ke konfiguraci](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. Klikněte na **nainstalovat** dokončete konfiguraci. Po dokončení konfigurace Průvodce vám nabídne možnost připojení k serverům ověřit. Klikněte na **Ověřit** zkontrolujte připojení.
![Instalace dokončena](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### Přidání z federované domény<a name=addfeddomain></a>

Není těžké si kvůli federovaní Azure AD pomocí Azure AD Connect přidat doménu. Azure AD Connect přidá domény pro federaci a upravuje deklarace pravidla, která správně zobrazit vystavitel, pokud máte víc domén federovaní Azure AD.

1. Pokud chcete přidat z federované domény, vyberte úkol **Přidat další domain Azure AD**.
![Další domain Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. Na další stránce průvodce zadejte pověření globálního správce pro Azure AD.
![Připojení k Azure AD](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. Na stránce **vzdáleného přístupu pověření** zadejte doménu přihlašovací údaje správce.
![Přihlašovací údaje vzdáleného přístupu](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. Na další stránce Průvodce poskytuje seznamu Azure AD domény které můžete vytvořit federaci místního adresáře. Ze seznamu vyberte doménu.
![Azure AD domain](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    Po zvolení příkazu doménu, Průvodce vám poskytne příslušné informace o další akce, které bude trvat průvodce a jejich dopad konfigurace. V některých případech při výběru možnosti doméně, která není ještě neověřili v Azure AD Průvodce vám poskytne informací o kontaktech můžete ověřit doménu. Další informace najdete v článku [Přidání vlastní název domény pro službu Azure Active Directory](active-directory-add-domain.md) .

5. Klikněte na **Další**a **Připraveno ke konfiguraci** stránky se zobrazí v seznamu akce, které provede Azure AD Connect. Klikněte na **nainstalovat** dokončete konfiguraci.
![Připraveno ke konfiguraci](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>Přizpůsobení AD FS

Následující části obsahují podrobné informace o některých běžných úloh, které může být vhodné provést přizpůsobování přihlašovací stránku služby AD FS.

### Přidání vlastní firemní logo nebo obrázek<a name=customlogo></a>

Logo společnosti, která se zobrazí na **přihlašovací** stránku, můžete změnit následující rutiny prostředí Windows PowerShell a syntaxe.

> [AZURE.NOTE] Doporučené rozměry pro logo jsou 260 × 35 @ 96 dpi s velikostí souboru není větší než 10 KB.

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] Parametr *Cílový_název* je povinný. Výchozí motiv nevydaném se službou AD FS jmenuje výchozí.


### Přidání popisu přihlášení<a name=addsignindescription></a>

Pokud chcete přidat popis přihlašovací stránka na **přihlašovací stránku**, pomocí následující rutiny prostředí Windows PowerShell a syntaxe.

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### Úprava pravidla deklarace AD FS<a name=modclaims></a>

Služba AD FS podporuje bohaté deklarace jazyk, který slouží k vytvoření pravidel pro vlastní deklarace. Další informace najdete v tématu [Role deklarace pravidlo jazyka](https://technet.microsoft.com/library/dd807118.aspx).

Následující oddíly popisují, jak můžete napsat vlastní pravidla pro některé scénáře, které se týkají pro Azure AD a federace AD FS.

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>Neměnný ID podmíněné podle hodnoty se účastní atribut

Azure AD Connect vám umožní určit atribut má být použit jako zdroj ukotvení při objekty se synchronizují Azure AD. Pokud není prázdné hodnoty v atributu vlastní, můžete chtít problémů deklaraci neměnný ID. Může například vyberte **ms-ds-consistencyguid** jako atribut ukotvení zdroj a chcete vystavit **ImmutableID** jako **ms-ds-consistencyguid** v případě, že atribut obsahuje hodnoty u ní. Pokud není žádná hodnota proti atribut, problémů **objectGuid** jako neměnný ID.  Můžete vytvořit sadu pravidel vlastní deklarace popsané v následující části.

**Pravidlo 1: Atributy dotazu**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

Na základě tohoto pravidla dotazu hodnoty **ms-ds-consistencyguid** a **objectGuid** pro uživatele ze služby Active Directory. Změňte název úložiště na název příslušné úložiště v AD FS nasazení. Taky změňte typ nároky na typ VELKÁ2 deklarací pro vaše federaci nadefinovaná **objectGuid** a **ms-ds-consistencyguid**.

Pomocí **Přidat** a není **problém**navíc nedávejte odchozí problém entity a hodnoty můžete použít jako pomocná hodnoty. Budete vydávat nárok na pozdější pravidlo po vytvoření hodnot, které chcete použít jako neměnný ID.

**Pravidlo 2: Zkontrolujte, pokud existuje ms-ds-consistencyguid pro uživatele**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

Pokud chcete toto pravidlo definuje dočasné příznaku jen **idflag** nastavený na **useguid** při žádné **ms-ds-concistencyguid** vyplněné pro uživatele. Logika za to je faktů AD FS neumožňuje prázdné deklarované. Tak přidáte deklarací http://contoso.com/ws/2016/02/identity/claims/objectguid a http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid pravidla 1, můžete skončí s **msdsconsistencyguid** převzetí pouze pokud je hodnota vyplněné pro uživatele. Pokud není vyplněné, AD FS uvidí, že bude obsahovat prázdnou hodnotu a vynechává okamžitě. Všechny objekty mají **objectGuid**, budou toto tvrzení vždy dostupné po provedení pravidla 1.

**Pravidlo 3: Vydávání ms-ds-consistencyguid jako neměnný ID, pokud je k dispozici**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

Toto je kontrolu implicitní **existují** . Pokud existuje hodnota pro deklaraci vydá, jako neměnný ID. V předchozím příkladu používá deklaraci **nameidentifier** . Budete muset změnit typ odpovídající deklarace neměnný ID ve vašem prostředí.

**Pravidlo 4: Zadávání objectGuid jako neměnný ID ms-ds-consistencyGuid není k dispozici**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

V tomto pravidlu jednoduše kontrolujete dočasné příznak **idflag**. Můžete rozhodnout, jestli vydávání deklaraci identity na základě jeho hodnoty.

> [AZURE.NOTE] Posloupnost následujícími pravidly je důležité.

#### <a name="sso-with-a-subdomain-upn"></a>Jednotné přihlašování s subdoménu UPN

Můžete přidat víc než jednu doménu federovaný pomocí Azure AD Connect způsobem popsaným v části [Přidat nové federované domény](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain). Deklarace musí změnit tak, aby ID Vystavitel odpovídá kořenovou doménu a ne subdoménu, protože federované kořenovou doménu také zahrnuje podsložky.

Ve výchozím nastavení deklarace pravidlo pro Vystavitel ID nastavena jako:

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Výchozí Vystavitel ID deklarace](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

Výchozí pravidlo jednoduše zabírá přípony UPN a použití v deklaraci ID vydavatel. Například Jan je uživatel v sub.contoso.com a contoso.com federovaní Azure AD. Slouží k vložení Jan john@sub.contoso.com uživatelské jméno při přihlášení k Azure AD a výchozí ID Vystavitel převzetí pravidlo ve službě AD FS zpracovává následujícím způsobem.

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**Hodnotu deklarace:** http://sub.contoso.com/adfs/services/trust/

Chcete, aby jenom kořenovou doménu v hodnota deklarace vystavitel, změňte pravidlo deklarace podle těchto věcí.

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>Další kroky

Další informace o [možnostech přihlašovací uživatelů](active-directory-aadconnect-user-signin.md).
