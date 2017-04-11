<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - Definovat strategii zavádění identity hybridní | Microsoft Azure"
    description="Řízení přístupu podmíněné zkontroluje Azure Active Directory určité podmínky, které jste vybrali při ověřování uživatele a před umožněním přístup k aplikaci. Když jsou splněné tyto podmínky, uživatel ověření a povolený přístup k aplikaci."
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>


# <a name="define-a-hybrid-identity-adoption-strategy"></a>Definování strategii zavádění hybridní identity

V tomto úkolu definujete hybridní identity strategie pro vaše hybridní identity řešení požadavkům na firmy, které byly popisované v:

- [Určení potřeb podniku](active-directory-hybrid-identity-design-considerations-business-needs.md)
- [Určení požadavky synchronizace adresářů](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
- [Určit požadavky vícefaktorové ověřování](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="define-business-needs-strategy"></a>Definování obchodní potřeby strategii
První úkol adresy zjištění firmy organizace musí.  Může to být velmi obecných a obor nárůstu může dojít, pokud nejste opatrní.  Na začátek v jednoduchosti je krása ale nezapomeňte vždy plánování návrhu, který bude pokryly a usnadnění v budoucnosti změnit.  Bez ohledu na to, zda je jednoduchý návrh nebo velmi komplexní je Azure Active Directory, který podporuje Office 365, Microsoft Online Services a cloudu aplikace platformu Microsoft Identity.

## <a name="define-an-integration-strategy"></a>Definování efektivní strategii integrace
Společnost Microsoft nemá tři scénáře hlavní integrace, které jsou cloudové identitami synchronizované identitami a federovaný identitami.  Je třeba naplánovat na jednu z těchto integrace strategií přijetí.  Strategie, jaká jste se může lišit a rozhodnutí ve výběru některé může obsahovat, jaký typ uživatelské prostředí, které chcete zadat, máte některé infrastruktury už na místě a co je nákladů nejefektivnější.  
 
![](./media/hybrid-id-design-considerations/integration-scenarios.png)

Scénáře definované na obrázku nahoře je:

- **Identit cloudu**: Jedná se o identity, které jsou pouze v cloudu.  V případě Azure AD by se nacházejí v adresáři vašeho Azure AD.
- **Synchronized**: Jedná se o identity, které jsou v místním i schránkami v cloudu.  Pomocí Azure AD Connect tito uživatelé jsou vytvořené nebo spojené s existujícími Azure AD účty.  Algoritmus hash hesla uživatele se synchronizují z místního prostředí do cloudu na takzvanou algoritmus hash hesla.  Použití synchronizovat jeden výstrahou při, pokud je uživatel zakázané v místním prostředí, může trvat až 3 hodiny tohoto účtu stavu objevit v Azure AD.  Toto je kvůli časový interval synchronizace.
- **Federovaný**: tyto identity jsou oba místních i cloudových.  Pomocí Azure AD Connect tito uživatelé jsou vytvořené nebo spojené s existujícími Azure AD účty.  
 
>[AZURE.NOTE]
Další informace o synchronizaci další možnosti [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md).

V následující tabulce vám pomůže při určování výhody a nevýhody každé z následujících postupů:

| Strategie         | Výhody                                                                                                                                                                                                                                                  | Nevýhody                                                                                                                                                                                                                                                                                                                                                  |
|------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Shluk identity** | Snadnější správu pro malé organizace. <br> Žádná další hardware ne na místní potřeba nainstalovat<br>Snadno k dispozici, pokud uživatel opustí společnosti                                                                                                   | Uživatelé budou muset přihlásit při přístupu k úloh v cloudu <br> Hesla může nebo nemusí být stejný pro cloudu a místní identity                                                                                                                                                                                                                     |
| **Synchronizovat**     | Heslo místního bude ověření obou místních i cloudových adresáře <br>Snadněji spravovat malý, střední nebo velký organizacích <br>Uživatelé nemůžou mít jednotné přihlašování (SSO) pro některé zdroje informací <br> Microsoft upřednostňovaná metoda synchronizace <br> Snadněji spravovat | Někteří uživatelé můžou mít námitky přes média k synchronizaci svých adresářích s cloudu termín policejní určité společnosti                                                                                                                                                                                                                                                  |
| **Federované**        | Uživatelé nemůžou mít jednotné přihlašování (SSO) <br>Pokud uživatel ukončení nebo opuštění, účtu můžete okamžitě zakázáno a odvolat přístup<br> Podporuje rozšířené scénáře, které nelze provést se synchronizují                                           | Další kroky pro nastavení a konfiguraci <br> Vyšší údržby <br> Můžou vyžadovat další hardware pro infrastrukturu Služba tokenů zabezpečení <br> Můžou vyžadovat další hardware při instalaci federačního serveru. Další software je potřeba při použití služby AD FS <br> Vyžadovat rozsáhlé nastavení pro jednotné přihlašování <br> Pole kritický bod selhání-li federačním serveru dolů, uživatelé nebudou ověření |

### <a name="client-experience"></a>Možnosti klienta
Strategie, kterou používáte, bude určovat přihlášení uživatele.  V následujících tabulkách můžete s informacemi o jaké uživatelé očekávat jejich přihlášení jako.  Všimněte si, že ne všech poskytovatelů federovaných identit podporují jednotné přihlašování ve všech případech.

**Doména připojen a privátní síť aplikací**:
 

|                              | Synchronizované Identity      | Federované Identity                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webové prohlížeče                 | Ověřování pomocí formulářů | jednotného přihlášení zapnutý, někdy potřeba zadat ID organizace |
| Aplikace Outlook                      | Výzva k zadání přihlašovacích údajů     | Výzva k zadání přihlašovacích údajů                                       |
| Skype pro firmy (Lync)    | Výzva k zadání přihlašovacích údajů     | jednotného přihlašování na pro Lync, výzva přihlašovací údaje pro Exchange   |
| SkyDrive Pro                 | Výzva k zadání přihlašovacích údajů     | jednotné přihlašování                                               |
| Office Pro Plus předplatného | Výzva k zadání přihlašovacích údajů     | jednotné přihlašování                                               |

**Externích nebo nedůvěryhodné zdrojů**:

|                              | Synchronizované Identity      | Federované Identity                                           |
|------------------------------|----------------------------|--------------------------------------------------------------|
| Webové prohlížeče                 | Ověřování pomocí formulářů |  Ověřování pomocí formulářů |
| Skype pro firmy (Lyncu), Skydrive Pro předplatné Office v Outlooku| Výzva k zadání přihlašovacích údajů     | Výzva k zadání přihlašovacích údajů                                       |
| Exchange ActiveSync    | Výzva k zadání přihlašovacích údajů     | jednotného přihlašování na pro Lync, výzva přihlašovací údaje pro Exchange   |
| Mobilní aplikace                 | Výzva k zadání přihlašovacích údajů     | Výzva k zadání přihlašovacích údajů                                               |

Pokud jste usoudili z úkolu 1, jestli máte 3rd IdP nebo pokud je začátek poskytují federaci s Azure AD pomocí jednoho narozeninovou párty s motivy, musíte mějte na paměti následující podporovaných funkcí:
- Kteréhokoliv poskytovatele SAML 2.0, která je kompatibilní se standardem SP Lite profilu můžete podporuje ověřování Azure AD a přidružené aplikace
- Podporuje pasivní ověřování, což usnadňuje auth OWA, SPO atd.
- Exchange Online klientů můžete podporované prostřednictvím SAML 2.0 rozšířeného klienta profilu (ECP)

Musí být také přehled o jaké funkce nebudou k dispozici:

- Bez podpory WS – zabezpečení/federování jiných aktivní klientech přeruší
 - To znamená, že žádný klient Lyncu, Onedrivu klienta, předplatné Office, Office Mobile před Office 2016
- Přechod z Office pasivní ověřování umožní uživatelům podporují čistého SAML 2.0 IdPs, ale podpora bude stále na základě klienta klienta


>[AZURE.NOTE]
V aktuálním seznamu najdete v článku http://aka.ms/ssoproviders.

## <a name="define-synchronization-strategy"></a>Definování strategie synchronizace
V tomto úkolu definujete nástroje, které se použijí k synchronizaci organizace místních dat do cloudu a co byste měli použít topologie.  Protože většina organizací pomocí služby Active Directory, informace o použití Azure AD Connect k otázky podle výše uvedených v pár podrobností.  V prostředí, které neobsahují služby Active Directory najdete informace o pomocí FIM 2010 R2 nebo MIM 2016 plánování této strategie.  Však budoucích verzích Azure AD Connect bude podporovat LDAP adresáře, tak v závislosti na časovou osu, možná budete moct pomáhat tyto informace.

###<a name="synchronization-tools"></a>Nástroje pro synchronizaci
Let mít několik nástrojů synchronizace existoval a používá různých scénářích.  Je v současné době Azure AD Connect přejít na nástroj výběr pro všechny podporované scénáře.  AAD Sync DirSync jsou i nadále kolem a může být i prezentovat ve vašem prostředí nyní. 

>[AZURE.NOTE]
Nejnovější informace o podporovaných funkcí každý z nástrojů přečtěte si článek [Porovnání nástroje integrace adresářů](active-directory-hybrid-identity-design-considerations-tools-comparison.md) .  

### <a name="supported-topologies"></a>Podporovaných topologií
Při definování strategii synchronizace, musí být určeny topologie, který se používá. V závislosti na informace, která byla určena v kroku 2 můžete určit, které topologie je správné šablonu chcete použít. Jeden doménové jednoho Azure AD topologie je nejběžnější a se skládá z jednoho doménové služby Active Directory a jedna instance Azure AD.  Použitá ve většině scénáře a je očekávaná topologie při použití Express Azure AD připojení instalaci, jak je znázorněno na následujícím obrázku.
 
![](./media/hybrid-id-design-considerations/single-forest.png)Doménové jeden scénář je velmi běžné velkých a dokonce i malých organizacích mít víc strukturami, jak je vidět na obrázku 5.

>[AZURE.NOTE]
Další informace o různých místních a topologie Azure AD pomocí Azure AD Connect synchronizace, přečtěte si článek [topologie pro Azure AD Connect](active-directory-aadconnect-topologies.md).


![](./media/hybrid-id-design-considerations/multi-forest.png) 

Scénář více struktury

Pokud tento případ pak topologii více-forest-jednoduchá Azure AD považovat za pravdivosti následující položky:

- Uživatelé mají jenom 1 identity mezi všechny strukturami – jedinečně identifikační uživatelů v části to popisuje podrobněji.
- Uživatel se ověří na struktuře, ve kterém se nachází identitu
- Přípony a ukotvení zdroj (neměnný id) budou pocházet z této struktuře
- Všechny strukturami jsou přístupné Azure AD Connect – to znamená, že nemusí být domény připojeni a mohou být umístěny ve DMZ to usnadňuje to.
- Uživatelé mají jenom jedna poštovní schránka
- Struktura, který je hostitelem poštovní schránky uživatele má nejlepší kvalitu dat atributy viditelné v na Exchange globální seznam adres
- Pokud tam je žádné poštovní schránky na uživatele, pak všechny doménové lze přispívat tyto hodnoty
- Pokud máte propojené poštovní schránky, pak se rovněž jinému účtu v jiné struktuře použitá k přihlášení.

>[AZURE.NOTE]
Objekty, které jsou v obou místních i cloudových "připojeni" prostřednictvím jedinečný identifikátor. V rámci synchronizace adresářů tento jedinečný identifikátor se nazývá SourceAnchor. V souvislosti s jednotné přihlašování to se nazývá ImmutableId. [Návrh s koncepty Azure AD Connect](active-directory-aadconnect-design-concepts.md#sourceanchor) Další informace o používání SourceAnchor.

Pokud máte víc účtů aktivní nebo více než jedna poštovní schránka výše uvedených podmínek není, Azure AD Connect vyberte jednu a druhý ignorovat.  Pokud jste propojili poštovní schránky, ale jiného účtu, tyto účty nebude možné vyexportovat Azure AD a tento uživatel nesmí být členem skupiny.  Tím se liší od jak byla v minulosti pomocí DirSync a je záměrný pro lepší podpora scénářů více doménové. Situace více doménové zobrazený na následujícím obrázku.
 
![](./media/hybrid-id-design-considerations/multiforest-multipleAzureAD.png) 
 
**Více doménové více Azure AD scénářů**

Je vhodné mít jenom jeden adresář v Azure AD pro organizaci, ale je podporované ho relace 1:1 bude k dispozici až server Azure AD Connect synchronizace adresáře služby Azure AD.  Pro každý výskyt Azure AD budete potřebovat k instalaci aplikace Azure AD Connect.  Navíc Azure AD záměrné je izolace a uživatelé v výskyt Azure AD nebudou moct najdete v tématu uživatelů v jiné instanci.

Je možné a podporovaných se připojit k více adresářů Azure AD jednou instancí místní služby Active Directory, jak je znázorněno na následujícím obrázku:

![](./media/hybrid-id-design-considerations/single-forest-flitering.png) 
 

**Filtrování scénář jednoduchým struktury**

Abyste mohli provést následující musí být pravdivé:

- Azure AD Connect synchronizace serverech musí být nakonfigurované pro filtrování tak, aby každý mají sady vzájemně se vylučujících objektů.  To provést, například definování rozsahu každého serveru pro konkrétní doménu nebo organizační jednotku.
- DNS domény může registrované pouze v jednom Azure AD adresáře proto UPN uživatelů v místní AD používají samostatné obory názvů
- Uživatelé v výskyt Azure AD pouze budou moct najdete v tématu uživatelů z jejich instance.  Nebudou moct zobrazit uživatele v jiných případech
- Pouze jeden z adresáře Azure AD můžete povolit hybridní nasazení Exchange s místní AD
- Vzájemné výhradního práva se týká rovněž zpětného zápisu.  Díky některé zpětného zápisu funkce nejsou podporované v této topologii od těchto předpokládá jednoho místního konfigurace.  Tato volba zahrnuje:
 - Skupina zpětného zápisu s výchozí konfigurace
 - Zpětného zápisu zařízení


Mějte na paměti, že následující nepodporuje a neměli Vybraní jako implementace:

- Není podporována pro připojení k adresáři stejné Azure AD i v případě, že jsou nakonfigurované pro synchronizaci vzájemně se vylučujících nastavení objektu na více Azure AD Connect synchronizace serverech
- Není podporována synchronizace stejné uživateli více Azure AD adresářů. 
- Nepodporované je také k provedení změn Pokud chcete, aby uživatelé v jedné Azure AD zobrazovat jako kontaktů v jiném adresáři Azure AD v konfiguraci. 
- Je také nepodporované k úpravám Azure AD Connect synchronizace se připojit k více Azure AD adresářů.
- Azure AD adresáře jsou záměrné samostatný. Není podporovaná změna konfigurace synchronizace Azure AD Connect k načtení dat z jiného adresáře Azure AD při pokusu o vytvoření běžné a jednotné GAL mezi adresáře. Je taky nepodporuje export uživatelů mezi kontakty do jiného místní AD Azure AD Connect synchronizaci.


>[AZURE.NOTE]
Pokud vaše organizace omezí počítače v síti znemožňují připojit se k Internetu, tento článek obsahuje seznam koncové body (certifikátu IPv4 a IPv6 rozsahy adresy), měli byste zahrnout do svého odchozí seznamy a umožnit zóny Důvěryhodné servery aplikace Internet Explorer klienta počítače mohli úspěšně používat Office 365. Další informace naleznete v [Office 365 URL a rozsahy IP adres](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2?ui=en-US&rs=en-US&ad=US).

## <a name="define-multi-factor-authentication-strategy"></a>Definování strategie vícefaktorové ověřování
V tomto úkolu definujete strategie vícefaktorové ověřování používat.  Azure Vícefaktorové ověřování existují ve dvou různých verzí.  Jedna je cloudu a druhý místních na základě pomocí Azure MFA Server.  Na základě vyhodnocení nad se můžete zjistit řešení je ten správný strategie.  Použijte tuto tabulku, a zjistit, kterou možnost návrh nejlepší splnění požadavků zabezpečení vaší společnosti:

Možnosti Multi-Factor návrhu:

| Materiály k zabezpečení                                               | MFA v cloudu | MFA místní: |
|---------------------------------------------------------------|------------------|----------------|
| Aplikace Microsoft                                                | Ano              | Ano            |
| Aplikace SaaS v galerii aplikace                                  | Ano              | Ano            |
| Publikování aplikace služby IIS proxy server Azure AD aplikace         | Ano              | Ano            |
| Aplikace služby IIS nepublikované proxy server Azure AD aplikace | Ne               | Ano            |
| Vzdálený přístup jako VPN, RDG                                     | Ne               | Ano            |

I když pro strategie, může mít vyrovnané na řešení, potřebujete k vyhodnocení shora použít na umístění uživatele.  To může způsobit řešení chcete změnit.  Použijte tuto tabulku, usnadňující zjištění takto:

| Umístění uživatele                                                       | Preferovaný návrh                 |
|---------------------------------------------------------------------|-----------------------------------------|
| Azure Active Directory                                              | Více FactorAuthentication v cloudu |
| Azure AD a využití místního AD pomocí federace AD FS             | Obě                                    |
| Azure AD a místních AD pomocí Azure AD Connect žádné synchronizace hesel | Obě                                    |
| Azure AD a místních Azure AD Connect pomocí synchronizace hesel  | Obě                                    |
| Místní AD                                                      | Vícefaktorové ověřování serveru      |

>[AZURE.NOTE]
Měli byste taky zajistit, možnost vícefaktorové ověřování návrh, který jste vybrali podporuje funkce, které jsou potřeba pro návrh.  Další informace naleznete v [Zvolit řešení Multi-Factor zabezpečení za vás](../multi-factor-authentication-get-started.md#what-am-i-trying-to-secure).

## <a name="multi-factor-auth-provider"></a>Vícenásobné Auth poskytovatele
Vícefaktorové ověřování je k dispozici ve výchozím nastavení pro globální správce, kteří mají klienta služby Azure Active Directory. Ale pokud chcete rozšířit vícefaktorové ověřování pro všechny vaše uživatele a/nebo chcete pro globální správce možné využít funkce, jako jsou portálu Správa vlastních pozdravy a sestavy, potom kupte a konfigurace Multi-Factor Authentication poskytovatele.

>[AZURE.NOTE]
Měli byste taky zajistit, možnost vícefaktorové ověřování návrh, který jste vybrali podporuje funkce, které jsou potřeba pro návrh. 

##<a name="next-steps"></a>Další kroky
[Zjistit, požadavky ochrany dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)

## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
