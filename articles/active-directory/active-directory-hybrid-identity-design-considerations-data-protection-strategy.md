<properties
    pageTitle="Azure Active Directory hybridní identity navrhování - Definovat strategii ochranu dat | Microsoft Azure"
    description="Budete definovat strategii ochranu dat pro vaše hybridní identity řešení požadavkům firmy, které jste definovali."
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


# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>Definování strategie ochranu dat pro vaše hybridní identity řešení

V tomto úkolu definujete strategie ochranu dat pro vaše hybridní identity řešení požadavkům business, který jste definovali v:

- [Zjistit, požadavky ochrany dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
- [Určit požadavky na správu obsahu](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
- [Určit požadavky na řízení přístupu](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
- [Určit incidentu požadavky](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Definování možností ochrany dat
Způsobem popsaným byla v tématu [požadavky synchronizace adresářů určení](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), Microsoft Azure AD se můžou synchronizovat s vaší Active Directory Domain Services (AD DS) nachází místní. Tato integrace umožňuje organizacím využít Azure AD pro ověření přihlašovací údaje uživatele při pro přístup k podnikové prostředky. Lze to pro oba scénáře: data na ostatních místní i schránkami v cloudu.  Přístup k datům v Azure AD vyžaduje ověřování uživatelů prostřednictvím Služba tokenů zabezpečení (Služba tokenů zabezpečení).

Po ověření hlavní jméno uživatele (UPN) je pro čtení z ověřovací token a replikovanou oddíl a je určen kontejneru odpovídající doméně uživatele. Informace o uživatele existenci stav povoleno a rolí používá systému se tak mohli ověřovat a zjistit, zda požadovaný přístup na cílovém klientovi je oprávněn pro tohoto uživatele v této relaci. Některé oprávněnými akce (konkrétně vytvoření uživatele, resetovat hesla) vytvořit kontrolní záznam, který lze pomocí Správce tenanta: Správa dodržování předpisů úsilí nebo vyšetřování.

Přesunutí dat z místních datacentra do úložiště Azure přes připojení k Internetu nemusí být vždy možné kvůli dat svazku, dostupnost šířky pásma nebo Další informace. [Import nebo Export služba Azure úložiště](../storage/storage-import-export-service.md) poskytuje možnost hardwarové pro umístění/načítání velkých objemů dat v úložišti objektů blob. Umožňuje uživateli odeslat [zašifrovaných nástroje BitLocker](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) pevných discích přímo do Azure datacentra kterém cloudu operátory odešle obsah ke svému účtu úložiště, nebo je možné stáhnout Azure dat na vašich jednotkách vrátí. Pouze šifrované disků přijaté tohoto procesu (pomocí nástroje BitLocker generované službou samotné během instalace úlohy). Klíč BitLocker by vám měly Azure samostatně, tedy poskytující mimo pásma klíčové sdílení.

Protože data při přenosu šifrovaná může proběhnout v různých scénářích, je taky důležité vědět, že Microsoft Azure používá [virtuální sítě](https://azure.microsoft.com/documentation/services/virtual-network/) izolovat klienti přenosy od sebe, využívající míry například Host (hostitel) a hosta úroveň brány firewall, filtrování paketů IP, blokování portu a HTTPS koncové body. Většina Azure jeho vnitřní komunikace, včetně infrastruktury infrastrukturu a infrastruktury zákazníka (místní), jsou však také šifrované. Jiné důležitá funkce je komunikace v rámci Azure datacentrech; Microsoft spravuje sítí zajistit, že žádný můžete zosobnění nebo odposlouchávat IP adresu jiného. TLS/SSL se používá při přístupu k úložišti Azure nebo SQL databáze nebo při připojování ke Cloudovým službám. Správce zákazníka v tomto případě zodpovídá za získání certifikát TLS/SSL a nasazení jejich infrastruktury klienta. Data přenosy přesunutí mezi virtuálních počítačích ve stejném nasazení nebo mezi klienty v jednom nasazení prostřednictvím virtuální sítě Microsoft Azure můžete chráněných prostřednictvím šifrované komunikační protokoly například HTTPS SSL/TLS a ostatní.

Podle toho, jak zodpovězené otázky v [určení požadavky na ochranu dat](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)třeba moci určit, jak budete chtít ochrana dat a jak řešení identit hybridní vám pomůže na tomto. V tabulce jsou uvedeny možnosti podporované Azure, které jsou dostupné pro každý scénář ochranu dat.


| Možnosti ochrany dat         | V ostatních v cloudu | V ostatních místní | Při přenosu šifrovaná |
|---------------------------------|----------------------|---------------------|------------|
| Šifrování nástroje BitLocker jednotky      | X                    | X                   |            |
| Šifrování databáze SQL serveru | X                    | X                   |            |
| Šifrování OM OM             |                      |                     | X          |
| SSL/TLS.                         |                      |                     | X          |
| VIRTUÁLNÍ PRIVÁTNÍ SÍTĚ                             |                      |                     | X          |


>[AZURE.NOTE]
Přečtěte si [dodržování předpisů funkcí](https://azure.microsoft.com/support/trust-center/services/) na [Centrum zabezpečení aplikace Microsoft Azure](https://azure.microsoft.com/support/trust-center/) najdu Další informace o certifikáty, které každý Azure služba není kompatibilní s.
Protože vícevrstvé možnost používat proto možnosti pro ochranu dat, nejsou srovnání tyto možnosti pro tento úkol. Ujistěte se, že jsou využívání pro tyto stavy, které budou data k dispozici všechny možnosti.

## <a name="define-content-management-options"></a>Definujte možnosti správy obsahu
Jednou z výhod používání Azure AD pro správu infrastruktury identity hybridní je plně průhledné z perspektivu koncového uživatele procesu. Uživatel se pokusí přístup ke sdíleným prostředkům zdroj vyžaduje ověření, odešlete žádost o konverzaci ověřování Azure AD za účelem získání tokenu a přístup k prostředku bude mít uživatel. Pozadí bez zásahu uživatele se stane celého tohoto procesu. Je také možné udělit oprávnění pro [skupinu](active-directory-manage-groups.md#getting-started-with-access-management) uživatelů, aby mohli provádět některé běžné akce.

Organizacím, které jsou obvykle znepokojení o ochraně osobních údajů dat vyžadovat klasifikace dat pro jejich řešení. Pokud jejich aktuální místní infrastrukturu používá klasifikace data, je možné využít Azure AD jako hlavní úložiště pro identita uživatele. Běžné nástroj, že je použité místní data klasifikace bude jmenovat [Nástrojů klasifikace dat](https://msdn.microsoft.com/library/Hh204743.aspx) pro Windows Server 2012 R2. Tento nástroj pomáhají určit, klasifikaci a ochranu dat na serverech soubor v soukromé cloudu. Je také možné využít [Automatické klasifikace soubor](https://technet.microsoft.com/library/hh831672.aspx) v systému Windows Server 2012 k tomuto účelu.

Pokud vaše organizace nemá klasifikace dat na místě, ale je potřeba chránit citlivé informace bez přidání nového servery místní, můžete použít Microsoft [Azure Rights Management Service](https://technet.microsoft.com/library/JJ585026.aspx).  Azure RMS využívá šifrování, identit a zásady se tak mohli ověřovat zabezpečit soubory a e-mailu a funguje na několika zařízeních – telefonů, tabletů a počítačů. Protože se službou Azure RMS do cloudové služby, není potřeba provést explicitně vztahy důvěryhodnosti s jinými organizacemi před sdílením chráněného obsahu s nimi. Pokud už máte účet Office 365 nebo Azure AD adresáře, je automaticky podporovaná spolupráci v organizacích. Můžete synchronizovat taky jenom atributů adresáře službou Azure RMS potřebujete podporu běžné identity účtů místní služby Active Directory pomocí Azure Active synchronizační služba služby Directory (AAD Sync) nebo Azure AD Connect.

Důležitou součástí správy obsahu je pochopit přístupů které zdroje, tedy důležité pro řešení správy identit funkce bohaté protokolování. Azure AD poskytuje protokolu více než 30 dní, včetně:

- Změnami členství v roli (ex: uživatel přidaný do role globálního správce)
- Pověření aktualizace (ex: změny hesel)
- Domain management (ex: ověřováním vlastní doménu, odebrání domény)
- Přidání nebo odebrání aplikací
- Správa uživatelů (ex: přidáním, odebráním, aktualizace uživatele)
- Přidání nebo odebrání licence

>[AZURE.NOTE]
Přečtěte si [Microsoft Azure zabezpečení a správa protokolu auditování](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) dozvědět víc o protokolování funkce v Azure.
Podle toho, jak zodpovězené otázky v [určení správy obsahu požadavky](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)by měla určete, jak se má obsah spravován ve vašem hybridní identit řešení. Když všechny možnosti zveřejněným v tabulce 6 nejsou může integrace s Azure AD, je důležité definovat, které by byl vhodnější pro vaše potřeby firmy.

| Možnosti správy obsahu                                                               | Výhody                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Nevýhody                                                                                                                                                                                                                               |
|------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Centralizované místní (Active Directory Rights Management Server)                      | Oprávnění k úplnému řízení infrastruktura server zodpovědný za klasifikaci data <br> Předdefinované možnosti v systému Windows Server, není třeba pro předplatné a další licence <br> Lze integrovat s Azure AD ve scénáři hybridní <br> Podporuje informace o možnostech správy (k informacím IRM) přístupových práv ve službě Microsoft Online services například Exchange Online a Sharepointu Online, jakož i Office 365 <br> Podporuje místní serverové produkty společnosti Microsoft, třeba Exchange serveru, serveru SharePoint Server a servery soubor, se systémem Windows Server a soubor klasifikace infrastruktury (FCI). | Vyšší, údržbu (uchovávat nahoru s aktualizacemi, konfigurace a potenciální upgrady), od IT vlastníkem serveru <br> Vyžadovat infrastrukturu serveru místní<br> Doesn'tleverage Azure funkce nativně                                     |
| Centralizované v cloudu (Azure RMS)                                                     | Snadněji spravovat ve srovnání s místním řešení <br> Lze integrovat s AD DS ve scénáři hybridní <br>  Plně integrovaný s Azure AD <br> Nevyžaduje serveru místní abyste mohli nasadit služby <br> Podporuje místní Microsoft serverovými produkty například Exchange Server, Sharepointu, serveru a servery soubor, se systémem Windows Server a soubor klasifikace infrastruktury (FCI) <br> IT, může mít úplnou kontrolu nad jejich klienta klíč s možností BYOK.                                                                                    | Vaše organizace může mít cloudu předplatné, které podporuje RMS <br> Vaše organizace může mít adresáře služby Azure AD podporu ověřování uživatelů ve službě RMS                                                                                  |
| Hybridní (Azure RMS integrovaný s, místní Active Directory Rights Management Server) | Tento scénář sečteny výhody obojí centralizované místně a v cloudu.                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Vaše organizace může mít cloudu předplatné, které podporuje RMS <br> Adresář služby Azure AD podporu ověřování uživatelů ve službě RMS, musí mít vaše organizace <br> Vyžaduje připojení mezi Azure Cloudová služba a využití místního infrastruktury |

## <a name="define-access-control-options"></a>Definujte možnosti řízení přístupu
Využitím ověření se tak mohli ověřovat a řízení přístupu možnosti dostupné v Azure AD budete moct Povolit společnosti použijte k úložišti centrální identit zároveň uživatelům a partnerům jednotné přihlašování (SSO) jak je znázorněno na následujícím obrázku:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Centrální správa a plně integrace se službou ostatních adresářů

Azure Active Directory poskytuje jednotného přihlašování k tisícům SaaS aplikací a místní webových aplikací. Přečtěte si [seznamu Azure Active Directory federation funkce pro kompatibilitu: Zprostředkovatelé identit třetích stran, kteří mohou sloužit k implementace jednotného přihlašování](https://msdn.microsoft.com/library/azure/jj679342.aspx) článku podrobné informace o třetích stran jednotné přihlašování testované společnost Microsoft. Tato možnost umožňuje organizaci provádět různé B2B scénářů a zachovat přitom ovládacího prvku správy identit a přístup. Během B2B navrhování obrázku je ale důležité pochopit metodu ověření, která se použije partnerem a ověření, zda tento způsob je podporován Azure. Nyní jsou metody podporované Azure AD:

- Zabezpečení výraz Markup Language (SAML)
- OAuth
- Protokol Kerberos
- Tokeny
- Certifikáty


>[AZURE.NOTE] Přečtěte si [Azure Active Directory Authentication protokoly](https://msdn.microsoft.com/library/azure/dn151124.aspx) najdu Další informace o jednotlivých protokol a jeho funkcí v Azure.

Podpora Azure AD, mobilní podnikových aplikací pomocí prostředí stejné ověření snadno Mobile služby umožňující zaměstnanců a přihlaste se do svých mobilních aplikací pomocí svých přihlašovacích údajů podnikové služby Active Directory. Tato funkce je podporována Azure AD jako zprostředkovatelem identit služby Mobile společně s ostatní zprostředkovatelé identit jiní, které už podporujeme (včetně Accounts Microsoft, Facebook ID, Google ID a Twitter ID). Pokud aplikace v místním používá přihlašovací údaje uživatele c:\ společnosti AD DS, by měl být průhledné přístup partnerů a uživatelé pocházejících z cloudu. Spravovat řízení přístupu k podmíněné uživatele (cloudové) webových aplikací webového rozhraní API, cloudovým službám společnosti Microsoft, 3 aplikacích SaaS stran a nativní (mobilní) klientské aplikace a využívat výhod zabezpečení, auditování, vykazování všechno na jednom místě. Doporučujeme ale ověřit to v testovacím prostředí nebo s omezenou počtu uživatelů.


>[AZURE.TIP] je důležité zmínit Azure AD služby AD DS má nemá zásad skupiny. K jejímu vynucení zásad pro zařízení budete potřebovat řešení pro správu mobilních zařízení, jako je [Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).

Jakmile uživatele slouží k ověření Azure AD, je důležité vyhodnocení úroveň přístupu, aby uživatel měl ho. Úroveň přístupu, která bude mít uživatel přes zdroj se může lišit, i když Azure AD můžete přidat další vrstvu zabezpečení pomocí řízení přístupu k některé zdroje informací, můžete třeba taky mějte na paměti, že zdroje samotné mohou také obsahovat svůj vlastní seznam řízení přístupu samostatně, například řízení přístupu pro soubory uložené v souborovém serveru. Následující obrázek je uveden souhrn úrovně řízení přístupu, které máte ve scénáři hybridní:

![](./media/hybrid-id-design-considerations/accesscontrol.png)


Všechny interakce v diagramu u x obrázku představuje jeden přístup ovládací prvek scénář můžete vztahuje konsolidovaná Azure AD. Tady máte popis jednotlivých scénáře:

1.v podmíněné přístup k aplikacím, které jsou hostované místní: můžete registrovaných zařízení zásady přístupu pro aplikace, které jsou nakonfigurovány služby AD FS pomocí služby systému Windows Server 2012 R2. Další informace o nastavování podmíněného přístupu pro místní najdete v tématu [Nastavení místní podmíněné připojení přes Azure Active Directory registrace zařízení](active-directory-conditional-access-on-premises-setup.md).
2. přístup k ovládacímu prvku na portálu Správa Azure: Azure má taky možnost řízení přístupu k portálu Správa pomocí RBAC (Role pro řízení přístupu). Tato metoda umožňuje společnosti omezení počtu operacích, které jednotlivce můžete udělat, když má přístup k portálu Správa Azure. Pomocí RBAC k řízení přístupu k portálu správci IT ca přístup delegáta pomocí následujících přístupů řízení přístupu:

 - Přiřazení role skupina založená: můžete přiřadit přístup Azure AD skupin, které můžete synchronizují z místní služby Active Directory. Umožňuje využívat existující investice, které vaše organizace provedla nástrojů a procesů pro správu skupin. Můžete taky použít funkci delegované skupiny správy z Azure AD Premium.
 - Využití integrované role v Azure: můžete použít tři role – vlastník skupiny přispěvatelů a Reader, který je k zajištění uživatelům a skupinám oprávnění provádět pouze úkoly, kterou potřebují pro svou práci.
 - Podrobného přístupu k prostředkům: můžete přiřadit role uživatelům a skupinám u určitého předplatného, skupina zdroje nebo konkrétního zdroje Azure například na webu nebo v databázi. Tímto způsobem můžete zajistit, aby uživatelé měli přístup k všechny zdroje, které potřebují a bez přístupu k prostředkům, které není nutné spravovat.

>[AZURE.NOTE] [řízení přístupu na základě rolí v Azure](https://azure.microsoft.com/updates/role-based-access-control-in-azure-preview-portal/) najdu Další informace o této funkci přečtěte. Pro vývojáře, které jsou vytváření aplikací a chcete přizpůsobit řízení přístupu pro ně je také možné pro účely ověření Azure AD aplikace role. Přečtěte si tento [Web Appu RoleClaims DotNet příklad](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) o vytváření aplikace pro využití této funkce.

3 podmíněné přístupu pro aplikace Office 365 pomocí Microsoft Intune: správci IT můžete vytvořit zásady zařízení podmíněné přístupu k zabezpečení podnikové prostředky ve stejnou dobu povolení pracovníci pracující s informacemi na zařízení kompatibilní s přístup k službám. Další informace najdete v tématu [Podmíněné zásady přístupu zařízení pro služby Office 365](active-directory-conditional-access-device-policies.md).

4 podmíněné přístup k aplikacím Saas: [Tato funkce](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) umožňuje nastavit pravidla přístupu jednotlivé aplikace vícefaktorové ověřování a možnost zablokování přístupu pro uživatele není v síti důvěryhodné. Použít pravidla vícefaktorové ověřování pro všechny uživatele, které jsou přiřazené k aplikaci nebo jenom uživatelům v rámci určité skupiny zabezpečení. Uživatelé můžou vyloučené ze požadovaného vícefaktorové ověřování, pokud přistupují aplikace z IP adresy, která v uvnitř organizace síť.

Protože vícevrstvé možnost používat proto možností pro řízení přístupu, nejsou srovnání tyto možnosti pro tento úkol. Ujistěte se, že jsou využívání k dispozici pro každý scénář, který vyžaduje, abyste řízení přístupu k prostředkům všechny možnosti.

## <a name="define-incident-response-options"></a>Definujte možnosti incidentu odpověď
Azure AD pomůže IT identity potenciální zabezpečení rizikové v prostředí sledováním činnosti uživatelů, se dají IT využívat Azure AD přístup a použití sestavy možnost získat přehled o integrita a zabezpečení adresáře vaší organizace. Pomocí těchto informací správce IT můžete lépe zjistit, kde může být možné zabezpečení rizikové tak, aby dostatečně můžou plánovat pomoci tato rizika zmírnit.  [Předplatného Azure AD Premium](active-directory-get-started-premium.md) obsahuje sadu zabezpečení sestavy, které můžete povolit IT získat tyto informace. [Sestavy Azure AD](active-directory-view-access-usage-reports.md) jsou uspořádané do kategorií, jak je ukázáno v následujícím příkladu:

- **Sestavy odchylky**: obsahují přihlásit události, které jsme našli být neobvyklých. Naším cílem je přesně přehled o těchto aktivitě a umožňuje vám budete moci určit, zda je podezřelý události.
- **Integrované aplikace sestavy**: poskytuje podstatu používání cloudové aplikace ve vaší organizaci. Azure Active Directory umožňuje integraci s tisíce cloudové aplikace.
- **Chybové zprávy**: označení chyb, které se mohou vyskytnout, když zřizujete účtů externích aplikacích.
- **Specifické pro uživatele sestav**: zobrazení zařízení/znaménko aktivity dat pro konkrétní uživatele.
- **Protokoly aktivity**: obsahují záznam všechny ověřené události v rámci posledních 24 hodin, posledních 7 dnech nebo posledních 30 dní i změn aktivity skupin a heslo resetovat a registrace aktivity.

>[AZURE.TIP]
Další sestavy, které můžou pomoct taky týmu incidentu odpověď práci na případu je sestava [uživatele s prozrazený přihlašovací údaje](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) .  Tuhle sestavu plochy všechny nalezené shody mezi tyto seznamy prozrazený přihlašovací údaje a vašeho klienta.

Další důležité integrované v sestavách Azure AD něhož během vyšetřování incidentu odpověď a jsou:

- **Resetování hesla aktivity**: poskytnout podstatu jak aktivně používán v organizaci resetování hesla správce.
- **Resetování hesla registrace aktivity**: poskytuje přehledy, do kterých si uživatelé zaregistrovali jejich metod pro resetování hesla, a které metody jste vybrali.
- **Skupina aktivity**: poskytuje historii změn do skupiny (ex: uživatelé přidat nebo odebrat), které byly, které iniciuje na panelu aplikace Access.

Kromě základní vytváření sestav funkce k dispozici v Azure AD Premium, který lze využít během incidentu odpověď vyšetřování, IT může taky využijte sestavu auditování získávat informace:

- Změny v členství v roli (ex: uživatel přidaný do role globálního správce)
- Pověření aktualizace (ex: změny hesel)
- Domain management (ex: ověřováním vlastní doménu, odebrání domény)
- Přidání nebo odebrání aplikací
- Správa uživatelů (ex: přidáním, odebráním, aktualizace uživatele)
- Přidání nebo odebrání licence

Protože vícevrstvé možnost používat proto možnosti incidentu odpověď, nejsou srovnání tyto možnosti pro tento úkol. Ujistěte se, že jsou využívání všechny dostupné možnosti pro každý scénář, který vyžaduje, abyste využití vytváření sestav funkce Azure AD jako součást procesu incidentu vaší společnosti.

## <a name="next-steps"></a>Další kroky
[Určení úlohy správy identit hybridní](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)


## <a name="see-also"></a>Viz taky
[Přehled aspektech návrhu](active-directory-hybrid-identity-design-considerations-overview.md)
