<properties
    pageTitle="Azure AD Connect: Uživatel Přihlaste se | Microsoft Azure"
    description="Azure AD Connect přihlašovací uživatele pro vlastní nastavení."
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
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD připojit uživatele přihlašovacích možnosti

Azure AD Connect umožňuje uživatelům přihlášení do cloudu a místní zdroje stejné heslem. Tento článek popisuje klíčové koncepty pro každý model identit snadněji zvolíte identitu, že kterou chcete použít pro svůj Přihlaste se k Azure AD.

Pokud už znáte model identit Azure AD a chcete se dozvědět víc o konkrétní metoda, klepněte pod na příslušné téma.

* [Synchronizace hesel](#password-synchronization)
* [Federované jednotné přihlašování (pomocí služby AD FS)](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>Výběr způsobu přihlášení uživatele
Pro většinu organizací jsou potřebovali jednoduše tak, aby uživateli přihlásit k Office 365, zdrojů, na základě SaaS aplikací nebo jiných Azure AD výchozí možnost synchronizace heslo doporučujeme.
V některých organizacích, ale máte konkrétní důvody pro používání federované přihlášení na možnost například služba AD FS.  Jedná se o:

- Vaše organizace už má služby AD FS nebo 3rd federace poskytovatele nasazený narozeninovou párty s motivy
- Zásady zabezpečení zakazuje synchronizace hash heslo do cloudu
- Vyžadovat, aby uživatelé zaznamenat bezproblémové jednotné přihlašování (bez dalších heslo vyzve) při přístupu k prostředkům cloudu z domény počítačích spojují podnikovou síť
- Vyžaduje některé určité možnosti, které obsahuje AD FS
    - Místní vícefaktorové ověřování pomocí zprostředkovatele nebo čipové karty (Další informace o poskytovatelích MFA třetí strany na AD FS ve Windows serveru 2012 R2)
    - Integrace funkce služby Active Directory například uzamčení měkké účtu nebo heslo a pracovních hodin zásady služby Active Directory
    - Podmíněné přístup k obojímu místních i cloudových zdrojů pomocí registrace zařízení, Azure AD spojení nebo zásady Intune MDM

### <a name="password-synchronization"></a>Synchronizace hesel
S synchronizace hesel hash uživatelská hesla se synchronizují z Active Directory vaší místní Azure AD.  Hesla se změně nebo resetování místně, nového hesla jsou synchronizovány okamžitě Azure AD tak, aby vaši uživatelé můžete používat stejné heslo pro cloudové zdroje jako místní.  Hesla se nikdy poslané na Azure AD ani uložené v Azure AD ve formátu prostého textu.
Synchronizace hesel můžete k povolení vlastní resetování hesla služby v Azure AD použít společně s zpětného zápisu heslo.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[Další informace o funkci Synchronizace hesel](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>Federace pomocí nového nebo existujícího služby AD FS ve Windows serveru 2012 R2 farmy
Federované poštu na vaši uživatelé můžete přihlásit k služby Azure AD na základě jejich místní hesla a v podnikové síti, aniž byste museli znova zadejte své heslo.  Možnost federace se službou AD FS umožňuje nasazení nové nebo určit existující AD FS ve Windows serveru 2012 R2 farmy.  Pokud se rozhodnete určit existující farmy, Azure AD Connect nakonfiguruje vztah důvěryhodnosti mezi farmy a Azure AD tak, aby uživatelé mohli přihlásit.

<center>![Cloud](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>Abyste mohli nasadit federaci s AD FS ve Windows serveru 2012 R2, budete potřebovat takto

Pokud nasazujete vytvořit farmu:

- Serveru Windows Server 2012 R2 federačního serveru.
- Serveru Windows Server 2012 R2 proxy serveru webové aplikace.
- soubor .pfx zamýšleného federace služby názvu, například fs.contoso.com s jeden certifikát SSL.

Pokud jsou nasazení vytvořit farmu nebo použití existující farmy:

- Pověření místního správce na federační servery.
- Pověření místního správce na kteroukoli pracovní skupinu (bez domény připojen) serverů, u kterých chcete nasadit role proxy serveru webové aplikace.
- Počítač, který spuštění průvodce musí být možné se připojit k jiných počítačů, na které chcete nainstalovat AD FS a proxy serveru webové aplikace pomocí Vzdálená správa systému Windows.

[Konfigurace jednotného přihlašování pomocí služby AD FS](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>Přihlaste se pomocí starší verze služby AD FS nebo třetí strany řešení

Pokud jste už nakonfigurovali symbol cloudu ve formátu starší verze služby AD FS (například služba AD FS 2.0) nebo poskytovatele federace řad třetích stran, můžete přeskočit přihlašovací konfigurace uživatele přes Azure AD Connect.  To vám umožní získat poslední synchronizace a další možnosti Azure AD Connect při pořád používání existujících řešení pro znaménko na.

[Seznam kompatibilních Azure AD federace třetích stran](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>Přihlašování uživatelů a hlavní jméno uživatele (UPN)

### <a name="understanding-user-principal-name"></a>Principy hlavního názvu uživatele

Ve službě Active Directory je výchozí přípony UPN DNS název domény, ve kterém vytvořen uživatelský účet. Ve většině případů to je název domény registrované jako podniková doména na Internetu. Však můžete přidat další přípony UPN pomocí a vztahy důvěryhodnosti služby Active Directory.

UPN uživatel je formátu username@domai. Například u služby active directory contoso.com Jan může uživatel Přípony john@contoso.com. Přípony uživatel vychází z RFC 822. I když UPN a e-mailu sdílet stejný formát, přínosu UPN pro uživatele může nebo nemusí být rovna e-mailovou adresu uživatele.

### <a name="user-principal-name-in-azure-ad"></a>Hlavní název uživatele v Azure AD

Azure AD Connect průvodce pomocí atributu userPrincipalName nebo umožňuje zadat atribut (vlastní instalace) se nemusí používat z místního jako hlavní název uživatele v Azure AD. Toto je hodnota, která se použije pro přihlášení k Azure AD. Pokud hodnota atributu hlavního názvu uživatele neodpovídá ověřenou doménou v Azure AD, pak Azure AD ho nahradíte s výchozím. onmicrosoft.com hodnotu.

Všechny adresáře v Azure Active Directory je součástí předdefinované doménou ve formuláři contoso.onmicrosoft.com, který vám umožní začít používat Azure nebo další služby od Microsoftu. Můžete zlepšit a zjednodušit přihlášení pomocí vlastních domén. Informace o vlastní doménu jména v Azure AD a ověření domény, přečtěte si [Přidat váš vlastní název domény pro službu Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure AD přihlašovací konfigurace

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure AD přihlašovací konfigurace s Azure AD Connect
Azure AD přihlášení závisí na tom, zda je možné podle příponu hlavního názvu uživatele uživatele synchronizovaný na jeden z těchto vlastních domén ověření v adresáři Azure AD Azure AD. Azure AD Connect poskytuje pomoc při konfiguraci Azure AD přihlašovací nastavení tak, aby uživatel přihlášení v cloudu je podobný místního prostředí. Azure AD Connect seznamy přípony UPN definované pro domény a pokusí se porovnat s vlastní doménou v Azure AD a pomůže vám odpovídající akce, kterou je potřeba vzít.
Na přihlašovací stránku Azure AD zobrazuje seznam se přípony UPN definované pro místní služby Active Directory a odpovídající stav před každou příponou. Hodnoty stavu lze níže:

| Stav | Popis | Není nutná akce |
|:----|:----|:----|
|Ověření| Azure AD Connect nalezené že odpovídající ověření domény v Azure AD. Všichni uživatelé pro tuto doménu můžete přihlásit pomocí svých přihlašovacích údajů místního| Není potřeba žádná akce |
|Neověřené| Azure AD Connect nebyly nalezeny odpovídající vlastní domény v Azure AD, ale není ověřit. Změní se přípony UPN uživatelů tuto doménu výchozí. onmicrosoft.com přípona po synchronizaci pokud doména není ověřená. | Ověření vlastní domény v Azure AD. [Víc se uč](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|Nepřidali| Azure AD Connect nelze najít odpovídající přípony UPN vlastní doménu. Přípony UPN pro uživatele z této domény se změní na výchozí. onmicrosoft.com, pokud není doménu přidali a verifeid v Azure.| Přidání a ověření vlastní domény odpovídající přípony UPN pro [Další informace](./active-directory-add-domain.md)|

Azure AD přihlašovací stránka seznamy suffix(s) UPN definované pro místní služby Active Directory a odpovídající vlastní domény v Azure AD s aktuálním stavem ověření. Vlastní instalace můžete teď vyberete atribut hlavního názvu uživatele na stránce **Azure AD přihlásit** .

![Azure AD přihlašovací stránka](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

Kliknete na tlačítko Aktualizovat znovu vzdáleně nejnovější stav vlastních domén z Azure AD.

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>Výběr atribut hlavního názvu uživatele v Azure AD

UserPrincipalName - atributu userPrincipalName je atribut budou uživatelé používat, když se přihlaste se k Azure AD a Office 365. S doménami, nazývaný také-přípony UPN, by měl být ověřený v Azure AD před synchronizací uživatelů. Důrazně doporučujeme zachovat výchozí atributu userPrincipalName. Pokud Tenhle atribut je směrovat a nelze ověřit je možné vybrat jiný atribut, například e-mailem jako atribut blokování ID přihlášení. Jedná se o jako alternativní ID. Hodnota atributu alternativní ID postup směrodatnou RFC822. Alternativní ID lze použít s heslem jednoho přihlašování (SSO) a federace jednotné přihlašování jako řešení přihlášení.

>[AZURE.NOTE] Použití alternativní ID není kompatibilní se všechny úlohami Office 365. Další informace získáte [Konfigurace alternativní ID přihlášení](https://technet.microsoft.com/library/dn659436.aspx).

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>Jiné vlastní domény států a neovlivní Azure přihlášení
Je důležité pochopit vztahy mezi stavy vlastní domény v adresáři Azure AD a přípony UPN definované místní. Dejte nám absolvovat různých možné Azure přihlašovací zkušenosti při nastavování sycnhronization pomocí Azure AD spojovacích.

Následující informace Předpokládejme, že nás zajímala contoso.com přípony UPN, které se používá v místním adresáři jako součást UPN, například user@contoso.com.

###### <a name="express-settings--password-synchronization"></a>Expresní nastavení / synchronizace hesel
| Stav         | Vliv na uživatele Azure přihlásit |
|:-------------:|:----------------------------------------|
| Nepřidali | V tomto případě žádná vlastní domény pro contoso.com někdo přidal v adresáři Azure AD. Uživatelé, kteří mají Přípony místních s příponou @contoso.com, nebudou moct pomocí svých místních hlavních názvů uživatelů Přihlaste se k Azure. Místo toho bude potřeba použít nové Přípony poskytovanou na ně Azure AD přidáním UPN pro výchozí Azure AD adresář. Například, pokud jsou synchronizaci uživatelům Azure AD adresáře azurecontoso.onmicrosoft.com pak uživatele místního user@contoso.com dostanou název UPNuser@azurecontoso.onmicrosoft.com|
| Neověřené | V tomto případě máme vlastní doménu contoso.com přidané v Azure AD adresáře, ale není ještě neověřili. Pokud budete pokračovat s synchronizaci uživatelé bez ověření domény, potom uživatelů bude přiřazeno nové Přípony tak, že Azure AD stejně jako v případě není přidali.|
| Ověření | V tomto případě máme contoso.com vlastní doménu už přidali a jejich ověření v Azure AD pro přípony UPN. Uživatelé budou moct používat své místní hlavní uživatelské jméno, například user@contoso.com, k přihlášení k Azure po synchronizované s Azure AD|

###### <a name="ad-fs-federation"></a>Federace AD FS
Nelze vytvořit federačním s výchozími. onmicrosoft.com v Azure AD nebo neověřený vlastní domény v Azure AD. Když spustíte Průvodce Azure AD Connect při výběru možnosti neověřený domény vytvořit federaci s potom Azure AD Connect zobrazí výběr z potřebné záznamy, které chcete vytvořit hostitelem vašeho serveru DNS pro doménu. Další informace najdete [tady](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation).

Pokud jste vybrali možnost odhlásit uživatele jako "Federaci s AD FS", musíte mít vlastní doménu, pokračujte vytváření federačním v Azure AD. Pro naše diskuse znamená to jsme by měl mít vlastní doménu contoso.com přidané v adresáři Azure AD.

| Stav         | Vliv na uživatele Azure přihlásit |
|:-------------:|:----------------------------------------|
| Nepřidali | V tomto případě Azure AD Connect nelze najít odpovídající vlastní doménu pro contoso.com přípony UPN v adresáři Azure AD. Budete muset přidat vlastní doménu contoso.com v případě potřeby uživatelům přihlášení pomocí služby AD FS s jejich Přípony místní jako user@contoso.com.|
| Neověřené | V tomto případě Azure AD Connect zobrazí výzvu s odpovídající podrobnosti o tom, jak ověření vaší domény na pozdější|
| Ověření | V takovém případě můžete přejít dále s konfigurací bez jakékoli další akce|  

## <a name="changing-user-sign-in-method"></a>Změna metody přihlášení uživatele

Můžete změnit přihlašovací metodu uživatele z federace k synchronizaci hesel pomocí avaialble úkoly v Azure AD Connect po počátečním nastavení Azure AD Connect pomocí průvodce. Znovu spustit Průvodce Azure AD Connect a zobrazí se seznamem úkolů, které lze provádět. Vyberte **změnit přihlášení uživatele aplikaci** ze seznamu úkolů. 

![Změna přihlašování uživatelů"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

Na další stránce se zobrazí výzva k zadání přihlašovací údaje pro Azure AD.

![Připojení k Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

Na stránce **přihlášení uživatele** vyberte **Synchronizace hesel**. Tím se změní v adresáři z federované spravovaných snímek.

![Připojení k Azure AD](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] Pokud vytváříte jenom dočasná přepínače synchronizace hesel, zkontrolujte **nepřevádí uživatelské účty**. Není kontrola na požadovanou možnost vede k převodu jednotlivých uživatelů k federované a může trvat několik hodin.
  
## <a name="next-steps"></a>Další kroky
Další informace o [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

Další informace o [Azure AD Connect: návrh koncepty](active-directory-aadconnect-design-concepts.md)


