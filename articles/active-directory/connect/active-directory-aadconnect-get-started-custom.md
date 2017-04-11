<properties
    pageTitle="Azure AD Connect: Vlastní instalace | Microsoft Azure"
    description="Tento dokument obsahuje podrobnosti požadovaných možností vlastní instalace pro Azure AD Connect. Pomocí těchto pokynů k instalaci služby Active Directory pomocí Azure AD Connect."
    services="active-directory"
    keywords="Co je Azure AD Connect, nainstalujte službu Active Directory, požadované součásti pro Azure AD"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"  
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="custom-installation-of-azure-ad-connect"></a>Vlastní instalace Azure AD Connect
Azure AD Connect **Vlastní nastavení** se používá, pokud chcete další možnosti instalace. Pracovní postup slouží Pokud máte víc strukturami nebo pokud chcete konfigurovat volitelné funkce nezahrnuté v Expresní instalace. Pracovní postup slouží ve všech případech, kdy možnost [**Expresní instalace**](active-directory-aadconnect-get-started-express.md) nesplňuje nasazení nebo topologie.

Než začnete instalaci Azure AD Connect, Nejdřív musíte mít [Stáhnout Azure AD Connect](http://go.microsoft.com/fwlink/?LinkId=615771) a dokončete předpokladem kroků v [Azure AD Connect: Hardware a požadavcích](../active-directory-aadconnect-prerequisites.md). Taky zkontrolujte, že nemusíte účtů podle popisu v [Azure AD Connect účtů a oprávnění](active-directory-aadconnect-accounts-permissions.md).

Vlastní nastavení neodpovídá topologii, třeba upgradovat DirSync najdete [související si přečtěte následující dokumentaci](#related-documentation) pro další scénáře.

## <a name="custom-settings-installation-of-azure-ad-connect"></a>Vlastní nastavení instalace Azure AD Connect

### <a name="express-settings"></a>Expresní nastavení
Na této stránce klikněte na **Přizpůsobit** zahájíte instalaci vlastní nastavení.

### <a name="install-required-components"></a>Nainstalovat požadované součásti
Při instalaci služby synchronizace části volitelné konfigurace můžete ponechat zaškrtnuté a Azure AD Connect nastaví všechno automaticky. Nastaví instanci SQL Server 2012 Express LocalDB, vytvářet příslušné skupiny a přiřaďte oprávnění. Pokud chcete změnit výchozí nastavení, můžete volitelná konfigurace slouží jednotlivé možnosti, které jsou dostupné v následující tabulce.

![Požadované součásti](./media/active-directory-aadconnect-get-started-custom/requiredcomponents.png)

Volitelné konfigurace  | Popis
------------- | -------------
Použití existujícího SQL serveru | Umožňuje zadat název SQL serveru a název instance. Vyberte tuto možnost, pokud už máte databázový server, který chcete použít. Zadejte název instance následovaný čárkou a port číslo v poli **Název Instance** , pokud SQL serveru, které neobsahují procházení povolené.
Použití existujícího účtu služby | Ve výchozím nastavení vytvoří Azure AD Connect účet místní služba pro synchronizaci služby používat. Heslo je automaticky generované a neznámý osobě instalaci Azure AD Connect. Pokud používáte vzdálený server SQL nebo používat na proxy server, který vyžaduje ověření, potřebujete službu účet v doméně a heslo. V těchto případech zadejte účet služby používat. Ujistěte se, že uživatel spouštějící instalace přidružení v jazyku SQL tak přihlašovací jméno pro účet služby dá vytvořit. Přečtěte si téma [Azure AD Connect účtů a oprávnění](active-directory-aadconnect-accounts-permissions.md#custom-settings-installation)
Zadejte vlastní synchronizace skupin | Ve výchozím nastavení Azure AD Connect vytvoří čtyři skupiny místních na server jsou nainstalované služby synchronizace. Jsou tyto skupiny: Správci skupiny, skupina operátory, procházet skupiny a skupině resetování hesla. Můžete zadat vlastní skupiny. Skupiny musí být místní na serveru a nebyla nalezena v doméně.

### <a name="user-sign-in"></a>Přihlašování uživatelů
Po instalaci požadované součásti, zobrazí se výzva vyberte uživatelé jednoho přihlašování způsobu. Následující tabulka obsahuje stručný popis dostupné možnosti. Úplné popis metody přihlášení najdete v tématu [přihlášení uživatele](../active-directory-aadconnect-user-signin.md).

![Přihlašovací uživatele](./media/active-directory-aadconnect-get-started-custom/usersignin.png)

Možnost přihlášení | Popis
------------- | -------------
Synchronizace hesel | Uživatelé můžou přihlásit ke cloudovým službám společnosti Microsoft, jako je Office 365 pomocí stejné heslo, které používají v místní síti. Hesla uživatele jsou synchronizovány Azure AD jako algoritmus hash hesla a dojde k ověření v cloudu. Další informace najdete v tématu [Synchronizace hesel](../active-directory-aadconnectsync-implement-password-synchronization.md) .
Federace se službou AD FS | Uživatelé můžou přihlásit ke cloudovým službám společnosti Microsoft, jako je Office 365 pomocí stejné heslo, které používají v místní síti.  Uživatelé přesměrováni do jejich místního nasazení služby AD FS instance se přihlásit a místní dojde k ověření.
Konfigurovat | Ani funkce nainstalovali a nakonfigurovali. Vyberte tuto možnost, pokud už máte 3 federačního serveru stran nebo jiné existující řešení na místě.

### <a name="connect-to-azure-ad"></a>Připojení k Azure AD
V části připojit služby Azure AD obrazovku zadejte účtu globálního správce a heslo. Pokud jste vybrali **federaci s AD FS** z předchozí stránky, není Přihlaste se pomocí účtu v doméně, že které chcete povolit pro federaci. Doporučení, je použití účtu v výchozí doménu **onmicrosoft.com** , kterou automaticky vytvořily služby Azure AD adresář.

Tento účet se používá pouze k vytvoření účtu služby Azure AD a není použit po dokončení průvodce.  
![Přihlašovací uživatele](./media/active-directory-aadconnect-get-started-custom/connectaad.png)

Pokud váš účet globální správce má MFA povoleno, pak potřebujete zadejte heslo do pole v rozevírací nabídce přihlašovací a vyplňte ověřovací kód MFA. Tím největší oříškem může být poskytující ověřovací kód nebo telefonního hovoru.  
![Uživatel přihlásit MFA](./media/active-directory-aadconnect-get-started-custom/connectaadmfa.png)

Účtu globálního správce mohou také obsahovat [Privilegovaných Správa identit](../active-directory-privileged-identity-management-getting-started.md) povolené.

Pokud dojít k chybě a máte problémy s připojením, pak v tématu [Poradce při potížích s připojením](../active-directory-aadconnect-troubleshoot-connectivity.md).

## <a name="pages-under-the-section-sync"></a>Stránky v části synchronizace

### <a name="connect-your-directories"></a>Připojení vaší adresáře
Připojení k vaší služba Active Directory Domain Azure AD Connect musí přihlašovacích údajů pro účet dostatečná oprávnění. Zadejte část doménu ve formátu NetBios nebo plně kvalifikovaný název domény, které je FABRIKAM\syncuser nebo fabrikam.com\syncuser. Tento účet může být běžného uživatelského účtu, protože pouze potřebuje výchozí oprávnění pro čtení. Však v závislosti na nefunguje, možná budete muset další oprávnění. Další informace najdete v tématu [Azure AD připojení účtů a oprávnění](../active-directory-aadconnect-accounts-permissions.md#create-the-ad-ds-account)

![Připojení adresáře](./media/active-directory-aadconnect-get-started-custom/connectdir.png)

### <a name="azure-ad-sign-in-configuration"></a>Azure AD přihlašovací konfigurace
Tato stránka umožňuje zkontrolovat UPN domén v místním služby AD DS, které byly ověřeny v Azure AD. Tato stránka umožňuje konfigurace atribut pro účely userPrincipalName.

![Neověřené domén](./media/active-directory-aadconnect-get-started-custom/aadsigninconfig.png)  
Prohlédněte si každá doména označené **Nepřidali** a **Ne ověření**. Zkontrolujte, že tyto domény, který používáte ověřili v Azure AD. Kliknutím na aktualizovat po ověření svojí domény. Další informace najdete v tématu [přidáním a ověřením domény](../active-directory-add-domain.md)

**UserPrincipalName** - atributu userPrincipalName je atribut uživatelé používat při přihlášení k Azure AD a Office 365. S doménami, nazývaný také-přípony UPN, by měl být ověřený v Azure AD před synchronizací uživatelů. Microsoft doporučuje zachovat výchozí atributu userPrincipalName. Pokud Tenhle atribut je směrovat a nelze ověřit, je možné vybrat jiný atribut. Můžete třeba vyberete e-mailu atribut blokování ID přihlášení. Pomocí jiného atributu userPrincipalName než se označuje jako **Alternativní ID**. Hodnota atributu alternativní ID postup standardu RFC822. Alternativní ID lze použít s synchronizaci hesel a federace.

>[AZURE.WARNING]
Použití alternativní ID není kompatibilní se všechny úlohami Office 365. Další informace najdete v příručce [Konfigurace alternativní ID přihlášení](https://technet.microsoft.com/library/dn659436.aspx).

### <a name="domain-and-ou-filtering"></a>Domény a OU filtrování
Ve výchozím nastavení jsou synchronizovány všech domén a organizačních jednotek. Pokud existují některé domény nebo organizačních jednotek nechcete synchronizovat s Azure AD, můžete zrušit výběr tyto domény a organizačních jednotek.  
![Filtrování DomainOU](./media/active-directory-aadconnect-get-started-custom/domainoufiltering.png) Tato stránka v průvodci je konfigurace domén filtrování. Další informace najdete v tématu [filtrování domén](../active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering).

Je také možné, že některé domény nejsou dostupné kvůli omezení brány firewall. Tyto domény jsou standardně a mít upozornění.  
![Nedostupné domén](./media/active-directory-aadconnect-get-started-custom/unreachable.png)  
Pokud se zobrazí upozornění, ujistěte se, že tyto domény jsou skutečně nedostupný a očekávané upozornění.

### <a name="uniquely-identifying-your-users"></a>Jednoznačně identifikační uživatelů
Odpovídající přes strukturami funkce umožňuje určit, jak uživatelům služby AD DS strukturami představují v Azure AD. Uživatel může buď být zobrazen pouze jednou mezi všechny strukturami nebo máte kombinací zapnutým a vypnutým účty. Uživatel může být také zobrazen jako kontakt v některých doménových.

![Jedinečný](./media/active-directory-aadconnect-get-started-custom/unique.png)

Nastavení | Popis
------------- | -------------
[Uživatelé pouze reprezentuje jednou mezi všechny strukturami](../active-directory-aadconnect-topologies.md#multiple-forests-separate-topologies) | Všichni uživatelé vytvářejí jako jednotlivé objekty v Azure AD. V metaverse nejsou spojeny objekty.
[Atribut pošty](../active-directory-aadconnect-topologies.md#multiple-forests-full-mesh-with-optional-galsync) | Tuto možnost spojí uživatelů a kontaktních atribut hromadné má stejné hodnoty v různých strukturami. Tuto možnost použijte, když kontakty byly vytvořeny pomocí GALSync.
[ObjectSID a msExchangeMasterAccountSID / atribut msRTCSIP-OriginatorSid](../active-directory-aadconnect-topologies.md#multiple-forests-account-resource-forest) | Tuto možnost spojí povolené uživatele ve struktuře účet s zakázané uživatele ve struktuře prostředků. V Exchange tuto konfiguraci jmenoval propojené poštovní schránky. Tuto možnost lze také pokud používáte Lync a Exchange není k dispozici ve struktuře zdroje.
sAMAccountName a MailNickName | Tuto možnost spojí atributy, které očekává se, že přihlašovací ID uživatele, můžete najít.
Určitý atribut | Tato možnost umožňuje vybrat vlastní atribut. **Omezení:** Zkontrolujte, že vyberte atribut, který už najdete v metaverse. Pokud vyberete vlastní atribut (ale ne v metaverse), nelze dokončit průvodce.

**Ukotvení zdroj** - sourceAnchor atribut je atribut, který je neměnný po dobu platnosti objektu uživatele. Je primární klíč propojení místní uživatele s uživatelem v Azure AD. Protože nemůžete změnit atribut, musí plánovat dobrý atribut používat. Aby pole mohlo sloužit je objectGUID. Tenhle atribut se nezmění, pokud je uživatelský účet se přesune mezi strukturami/domény. V prostředí více doménové kde přesouvat účty mezi strukturami, jiné musí být použit atribut, například atribut s číslo zaměstnance. Vyhněte se atributy, které chcete změnit, pokud osobu marries nebo změna přiřazení. Nelze použít atributy s @-sign, tak, aby e-mailu a userPrincipalName nelze použít. Atribut je také velká a malá písmena obsah přesouvaných objektu mezi strukturami, ujistěte se tedy chcete-li zachovat velká a malá písmena. Binární atributy jsou kódováním Base 64, ale další typy atribut zůstat v nešifrovaného stavu. Federace scénářů a některá Azure AD rozhraní Tenhle atribut je označovaná taky jako immutableID. Další informace o ukotvení zdroj najdete v [návrhu koncepty](../active-directory-aadconnect-design-concepts.md#sourceAnchor).

### <a name="sync-filtering-based-on-groups"></a>Synchronizace filtrování podle skupin
Filtrování u skupin funkce umožňuje synchronizovat pouze malou objektů pro pilotního nasazení. Tuto funkci lze použít, vytvořte skupinu k tomuto účelu na adresářová služba Active Directory. Potom dodejte uživatelé a skupiny, které se má synchronizovat Azure AD jako přímý členy. Později můžete přidávat a odebírat uživatele k této skupině ke správě seznamu objektů, které by měly tvořit Azure AD. Všechny objekty, které chcete synchronizovat musí být členem skupiny přímé. Uživatelé, skupiny, kontakty a počítače nebo zařízení musí být všechny přímé členy. Členství ve skupinách vnořené přetrvávají. Když přidáte skupinu při přidání člena, jen samotnou skupinu a nejsou jeho členy.

![Synchronizace filtrování](./media/active-directory-aadconnect-get-started-custom/filter2.png)

>[AZURE.WARNING]
Tato funkce je určená jenom k podpoře pilotní nasazení. Nepoužívejte ho v plnohodnotného nasazení.

V plnohodnotného nasazení má dělat třeba těžko udržovat jedinou skupinu u všech objektů na synchronizovat. Místo toho používejte jednu z metod v [Konfigurace filtrování](../active-directory-aadconnectsync-configure-filtering.md).

### <a name="optional-features"></a>Volitelné funkce
Tato obrazovka umožňuje vybrat volitelné funkce pro konkrétní scénáře.

![Volitelné funkce](./media/active-directory-aadconnect-get-started-custom/optional.png)

>[AZURE.WARNING]
Pokud máte aktuálně DirSync nebo Azure AD Sync aktivní, neaktivoval zpětným funkcí v Azure AD Connect.

Volitelné funkce | Popis
------------------- | -------------
Hybridní nasazení Exchange | Funkce hybridní nasazení Exchange umožňuje spoluvytváření existenci poštovních schránek Exchange obou místní a v Office 365. Azure AD Connect je synchronizace určité skupiny [atributy](../active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) z Azure AD zpátky do místního adresáře.
Azure AD aplikace a filtrování atributu | Povolením Azure AD aplikace a filtrování atribut lze přizpůsobit nastavení synchronizované atributy. Tato možnost přidá další dvoustránkový konfigurace vás průvodce. Další informace najdete v tématu [Azure AD aplikace a atribut filtrování](#azure-ad-app-and-attribute-filtering).
Synchronizace hesel | Pokud jste vybrali federace jako řešení přihlášení, můžete tuto možnost povolit. Synchronizace hesel můžete pak použít jako možnost zálohování. Další informace najdete v tématu [Synchronizace hesel](../active-directory-aadconnectsync-implement-password-synchronization.md).
Heslo zpětného zápisu | Povolením zpětným hesla, heslo změny, které pocházejí z Azure AD je zpět došlo k zápisu místního adresáře. Další informace najdete v tématu [Začínáme s Správa hesel](../active-directory-passwords-getting-started.md).
Skupina zpětného zápisu | Pokud používáte funkci **Skupin Office 365** , můžete si těchto skupin ve vaší místní Active Directory. Tato možnost je pouze k dispozici, pokud máte Exchange účastní na adresářová služba Active Directory. Další informace najdete v tématu [zpětným skupiny](../active-directory-aadconnect-feature-preview.md#group-writeback).
Zpětného zápisu zařízení | Umožňuje objekty zpětného zápisu zařízení v Azure AD ke službě Active Directory vaší místní podmíněného přístupu scénářích. Další informace najdete v tématu [Povolení zpětného zápisu zařízení v Azure AD Connect](../active-directory-aadconnect-feature-device-writeback.md).
Atribut synchronizaci adresářů rozšíření | Zapnutím synchronizace adresáře rozšíření atribut atributy určené synchronizované s Azure AD. Další informace najdete v článku [rozšíření Directory](../active-directory-aadconnectsync-feature-directory-extensions.md).

### <a name="azure-ad-app-and-attribute-filtering"></a>Azure AD aplikace a filtrování atribut
Pokud chcete omezit atributy, které budou synchronizovat s Azure AD, začněte tak, že vyberete služby, které používáte. Pokud uděláte změny konfigurace na této stránce, je nutné vybrat explicitně spuštěním Průvodce instalací nové služby.

![Volitelné funkce aplikace](./media/active-directory-aadconnect-get-started-custom/azureadapps2.png)

Založené na službě vybrané v předchozím kroku, tato stránka zobrazuje všechny atributy, které jsou synchronizovány. Tento seznam je tvořen kombinací typů objektů není synchronizovaný. Pokud existují některé konkrétní atributy, které potřebujete k synchronizaci, můžete zrušit zaškrtnutí položky atributy.

![Volitelné funkce atributy](./media/active-directory-aadconnect-get-started-custom/azureadattributes2.png)

>[AZURE.WARNING]
Odebrání atributy může být ovlivněné funkce. Osvědčené postupy a doporučeních najdete v článku [atributy synchronizovat](../active-directory-aadconnectsync-attributes-synchronized.md#attributes-to-synchronize).

### <a name="directory-extension-attribute-sync"></a>Atribut synchronizaci adresářů rozšíření
Schéma můžete rozšířit v Azure AD pomocí vlastní atributy přidané tak, že vaše organizace nebo jiné atributy ve službě Active Directory. Tuto funkci lze použít, vyberte **atribut synchronizace adresářů rozšíření** na stránce **Volitelné funkce** . Můžete vybrat další atributy pro synchronizaci na této stránce.

![Rozšíření Directory](./media/active-directory-aadconnect-get-started-custom/extension2.png)

Další informace najdete v článku [rozšíření Directory](../active-directory-aadconnectsync-feature-directory-extensions.md).

## <a name="configuring-federation-with-ad-fs"></a>Konfigurace federace se službou AD FS
Konfigurace služby AD FS s Azure AD Connect je velmi jednoduché pouhými několika kliknutími. Toto je potřeba před nastavením.

- Serveru Windows Server 2012 R2 pro federačního serveru s Vzdálená správa povolena
- Serveru Windows Server 2012 R2 pro webové aplikace Proxy server s Vzdálená správa povolena
- Certifikát SSL pro název federace služby, kterou chcete použít (například sts.contoso.com)

### <a name="ad-fs-configuration-pre-requisites"></a>AD FS konfigurace požadavků
Abyste mohli nakonfigurovat farmě služby AD FS pomocí Azure AD Connect, zajistěte, aby že WinRM je povolený na vzdálených serverech. Kromě toho projděte požadovaného porty uvedené v [tabulce 3 – Azure AD Connect a federační servery nebo WAP](../active-directory-aadconnect-ports.md#table-3---azure-ad-connect-and-federation-serverswap).

### <a name="create-a-new-ad-fs-farm-or-use-an-existing-ad-fs-farm"></a>Vytvořit novou službu AD FS farmu nebo použít existující farmě služby AD FS
Můžete použít existující farmě služby AD FS nebo můžete zvolit vytvořit nové farmě služby AD FS. Pokud budete chtít vytvořit novou, je nutné zadat certifikát SSL. Pokud certifikát SSL je chráněn heslem, zobrazí se výzva k zadání hesla.

![AD FS farmy](./media/active-directory-aadconnect-get-started-custom/adfs1.png)

Pokud se rozhodnete sdělit nám existující farmě služby AD FS, přejdete přímo na konfigurace vztah důvěryhodnosti mezi AD FS a Azure AD obrazovky.

### <a name="specify-the-ad-fs-servers"></a>Určení servery služby AD FS
Zadejte serverům, které chcete nainstalovat na AD FS. Můžete přidat jeden nebo více serverů podle kapacity plánování potřeb. Připojení všech serverů ke službě Active Directory před provedením této konfiguraci. Microsoft doporučuje instalaci jeden server služby AD FS test a pilotní nasazení. Potom přidejte a nasazení další servery potřebám měřítka spuštěním Azure AD Connect znovu po počáteční konfiguraci.

>[AZURE.NOTE]
Ujistěte se, že všechny servery jsou připojeny k doméně služby AD před provedením této konfiguraci.

![Servery AD FS](./media/active-directory-aadconnect-get-started-custom/adfs2.png)

### <a name="specify-the-web-application-proxy-servers"></a>Určení servery proxy serveru webové aplikace
Zadejte serverům, které chcete jako servery proxy serveru webové aplikace. Proxy serveru webové aplikace nasazenou v DMZ (extranetové webových) a podporuje ověřování žádosti o extranetové. Můžete přidat jeden nebo více serverů podle kapacity plánování potřeb. Microsoft doporučuje instalaci jeden Web aplikace proxy server test a pilotní nasazení. Potom přidejte a nasazení další servery potřebám měřítka spuštěním Azure AD Connect znovu po počáteční konfiguraci. Doporučujeme mít stejný počet proxy servery uspokojili ověřování z intranetu.

>[AZURE.NOTE]
<li> Nejsou-li účet, který použijete místní správce na servery služby AD FS, potom se zobrazí výzva k zadání přihlašovacích údajů správce.</li>
<li> Zajistěte, aby byl tam protokolu HTTP/HTTPS propojení mezi server Azure AD Connect a webové aplikace Proxy server před spuštěním tohoto kroku.</li>
<li> Ujistěte se, že je protokolu HTTP/HTTPS propojení mezi serveru webové aplikace a na server služby AD FS umožníte postupují požadavků na ověření.</li>

![V prohlížeči](./media/active-directory-aadconnect-get-started-custom/adfs3.png)

Zobrazí se výzva k zadání přihlašovacích údajů serveru webové aplikace můžete použít zabezpečené připojení na server služby AD FS. Tyto přihlašovací údaje musejí být místního správce na server služby AD FS.

![Proxy serveru](./media/active-directory-aadconnect-get-started-custom/adfs4.png)

### <a name="specify-the-service-account-for-the-ad-fs-service"></a>Zadejte účet služby pro službu AD FS
Služba AD FS vyžaduje účet služby domény ověřování uživatelů a vyhledávání informace o uživatelích ve službě Active Directory. Podporuje dva typy účtů služeb:

- **Skupina spravovat účet služby** – zavádí ve službě Active Directory Domain Services systému Windows Server 2012. Tento typ účtu poskytující služby, například služby AD FS, aniž by musel pravidelně aktualizovat heslo účtu jednoho účtu. Tuto možnost použijte, pokud už máte Windows Server 2012 domény řadiče v doméně, která patří servery služby AD FS.
- **Uživatelský účet domény** – tohoto typu účtu vyžaduje zadání hesla a pravidelně aktualizujte heslo, když se změní heslo nebo vyprší jejich platnost. Tuto možnost používejte pouze v případě, že nemusíte řadiče domény systému Windows Server 2012 v doméně, která patří servery služby AD FS.

Pokud jste vybrali skupiny spravovaných účtů služeb a tuto funkci používá nikdy ve službě Active Directory, zobrazí se výzva k zadání přihlašovacích údajů správce organizace. Těchto přihlašovacích údajů se používají k zahájení klíčové úložiště a povolit funkci ve službě Active Directory.

![Účet služby AD FS](./media/active-directory-aadconnect-get-started-custom/adfs5.png)

### <a name="select-the-azure-ad-domain-that-you-wish-to-federate"></a>Vyberte Azure AD doménu, kterou chcete vytvořit federaci
Toto nastavení slouží k nastavení federace vztah mezi AD FS a Azure AD. Konfiguruje služby AD FS tokenů zabezpečení problém Azure AD a konfiguruje Azure AD s informacemi o důvěryhodnosti tokenů z této konkrétní instanci služby AD FS. Tato stránka umožňuje nakonfigurujte jednu doménu na počáteční instalaci. Další domény můžete později nakonfigurovat spuštěním Azure AD Connect znovu.

![Azure AD Domain](./media/active-directory-aadconnect-get-started-custom/adfs6.png)

### <a name="verify-the-azure-ad-domain-selected-for-federation"></a>Ověření domény Azure AD pro federaci
Po výběru domény, kterou chcete být federované Azure AD Connect poskytuje potřebné informace můžete ověřit doménu neověřené. V tématu [Přidání a ověření domény](../active-directory-add-domain.md) informace o použití těchto informací.

![Azure AD Domain](./media/active-directory-aadconnect-get-started-custom/verifyfeddomain.png)

>[AZURE.NOTE]
Ověřte doménu ve fázi konfigurovat AD Connect se snaží. Pokud budete pokračovat konfigurace bez přidání potřebné záznamy DNS, Průvodce nedokáže dokončete konfiguraci.

## <a name="configure-and-verify-pages"></a>Konfigurace a ověřte stránky
Konfigurace dojde na této stránce.

>[AZURE.NOTE]
Před pokračováním instalace a konfigurace federace, ujistěte se, že jste nakonfigurovali [překlad pro federační servery](../active-directory-aadconnect-prerequisites.md#name-resolution-for-federation-servers).

![Připraveno ke konfiguraci](./media/active-directory-aadconnect-get-started-custom/readytoconfigure2.png)

### <a name="staging-mode"></a>Pracovní režimu
Je možné nastavit nový server synchronizace souběžně s přípravu režimu. Je podporována pouze můžete mít synchronizace serverů exportu do jednoho adresáře v cloudu. Ale pokud chcete přesunout z jiného serveru, například jeden pracovního nástroje DirSync, pak můžete povolit Azure AD Connect v pracovní režimu. Když povolíte, modul Synchronizace importovat a synchronizovat data jako normální, ale ho není exportovat všechno, co chcete Azure AD nebo AD. Heslo funkce zpětným synchronizace a heslo jsou zakázány během pracovní režimu.

![Pracovní režimu](./media/active-directory-aadconnect-get-started-custom/stagingmode.png)

V pracovní režimu, je možné proveďte požadované změny modul synchronizace a podívat, co je chcete exportovat. Po konfiguraci vypadá všechno dobře, znova spusťte Průvodce instalací a pracovní režim zakázat. Teď exportu dat Azure AD z tohoto serveru. Zkontrolujte, že zakázání dalších serveru ve stejnou dobu tak pouze jeden server aktivně export.

Další informace najdete v tématu [pracovní režim](../active-directory-aadconnectsync-operations.md#staging-mode).

### <a name="verify-your-federation-configuration"></a>Ověřte konfiguraci federace
Azure AD Connect ověřuje nastavení DNS za vás, když kliknete na tlačítko ověření.

![Dokončení](./media/active-directory-aadconnect-get-started-custom/completed.png)

![Ověření](./media/active-directory-aadconnect-get-started-custom/adfs7.png)

Kromě toho takto ověření:

- Ověřte, že se můžete přihlásit z prohlížeče z domény spojených počítače v síti intranet: připojení k https://myapps.microsoft.com a ověřte přihlášení s účtem přihlášený. Předdefinované účtu správce služby AD DS se nesynchronizuje a není možné použít pro ověření.
- Ověřte, že se můžete přihlásit ze zařízení v síti extranet. Na výchozí počítače nebo mobilním zařízení připojení k https://myapps.microsoft.com a zadejte svoje přihlašovací údaje.
- Ověření přihlašovací klienta RTF. Připojení k https://testconnectivity.microsoft.com, vyberte kartu **Office 365** a zvolte **Office 365 jednoho přihlašování Test**.

## <a name="next-steps"></a>Další kroky
Po dokončení instalace odhlaste se a přihlaste se znova k Windows před použitím Správce služby synchronizace nebo Editor pravidel synchronizace.

Teď, když máte nainstalovaný Azure AD Connect můžete [ověřit instalaci a přiřazovat licence](../active-directory-aadconnect-whats-next.md).

Další informace o těchto funkcích, které jsou povoleny k instalaci: [náhodné zabránit odstranění](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) a [Stavu připojení Azure AD](../active-directory-aadconnect-health-sync.md).

Další informace o tématech běžné: [Plánovač a jak spustit synchronizaci](../active-directory-aadconnectsync-feature-scheduler.md).

Další informace o [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Související si přečtěte následující dokumentaci

Téma |  
--------- | ---------
Přehled Azure AD Connect | [Integrace místních identit s Azure Active Directory](../active-directory-aadconnect.md)
Instalace pomocí nastavení Express | [Expresní instalace Azure AD Connect](active-directory-aadconnect-get-started-express.md)
Upgrade z DirSync | [Upgrade z synchronizační nástroj služby Azure AD (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Účty, které používají pro instalaci | [Další informace o Azure AD Connect účtů a oprávnění](active-directory-aadconnect-accounts-permissions.md)
