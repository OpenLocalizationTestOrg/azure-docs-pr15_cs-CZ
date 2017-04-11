<properties
    pageTitle="Základní informace o Azure Active Directory Domain Services | Microsoft Azure"
    description="Základní informace o Azure Active Directory Domain Services"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD Domain Services

## <a name="overview"></a>Základní informace
Azure infrastruktury služby umožňují nasadit širokou škálu výpočetních řešení aktivní způsobem. S Azure virtuálních počítačích nástroje můžete nasazovat téměř okamžitě a pouze platíte minuty. Používání podpora pro Windows, Linux, SQL Server, Oracle, IBM, SAP a BizTalk nástroje můžete nasazovat jakékoli pracovní zátěž, libovolný jazyk na téměř libovolném operační systém. Tyto výhody umožňují migrace starší verze nasazené místní Azure uložit na provozní náklady.

Klíčové aspekty migrace místní aplikací Azure zpracovává identity potřeb tyto aplikace. Adresář aplikací může spolehnout LDAP pro čtení nebo zápisu do podnikového adresáře nebo přes v ověřování systému Windows (ověřování protokolem Kerberos nebo NTLM) ověření koncoví uživatelé. Řádek z podnikových aplikací (LOB) se systémem Windows Server obvykle používají v počítačích domény připojen tak, aby se dá ovládat pomocí zásad skupiny. K aplikacím "výtah a shift" místní do cloudu tyto závislostí na infrastruktura podnikové identit se musí vyřešit.

Správci často hodit jednu z následujících způsobů splnění potřeb identity jejich nasazené v Azure:

- Připojení VPN k webu mezi pracovní vytížení v Azure infrastruktury služby a podnikového adresáře v místním nasazení.
- Rozšíření podnikové domény/doménové infrastruktury AD nastavením řadiče domény otevřené pomocí Azure virtuálních počítačích.
- Nasazení samostatné domény v Azure pomocí řadiče domény používaný jako Azure virtuálních počítačích.

Těchto postupů žádné vysoké náklady a správy režijních. Nasazení řadiče domény pomocí virtuálních počítačích v Azure musí správci. Dále budou muset Správa zabezpečení, oprava, sledovat, zálohování a Poradce při potížích s těmito virtuálních počítačích. Závislost na připojení VPN k místního adresáře způsobí, že pracovního vytížení nasazenou v Azure být ohroženo poruch přechodná sítě nebo výpadků. Tyto sítě výpadků zase mít za následek nižší provozu a snížené spolehlivosti pro tyto aplikace.

Jsme navržený Azure AD Domain služby chcete použít jako alternativu snadnější.


## <a name="introducing-azure-ad-domain-services"></a>Úvodní informace o Azure AD Domain Services
Azure AD Domain Services obsahuje spravovaný domény služeb, jako je připojení k doméně, ověřování protokolem Kerberos/NTLM zásady, LDAP, skupiny, které jsou plně kompatibilní se službou Active Directory pro Windows Server. Můžou využívat služby tyto domény bez nutnosti nasazení, Správa a oprava řadiče domény v cloudu. Azure AD Domain Services lze integrovat s stávajícímu klientovi Azure AD a tím umožnit, aby je uživatelé mohli přihlásit pomocí svých přihlašovacích údajů podnikové. Kromě toho můžete existující skupiny a uživatelských účtů zabezpečit přístup ke zdrojům, aby byla zajištěna zjednodušují "výtah a směna" místních zdrojů infrastruktury služby Azure.

Funkce Azure AD Domain Services bezproblémově pracuje bez ohledu na to, zda je vašeho klienta Azure AD jen cloudu nebo synchronizované s místní služba Active Directory.

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>Azure AD Domain Services pro jen cloudu organizace
Jen cloudu Azure AD klienta (někdy označovány jako "tenantů spravovaných"), které neobsahují žádné stopu identity místní. Jinými slovy jsou všechny nativní v cloudu – to znamená vytváří a spravuje v Azure AD uživatelské účty, jejich hesla a členství ve skupině. Podívejte na na Contoso je jen cloudu Azure AD klienta. Jak je znázorněno na následujícím obrázku, správce společnosti Contoso nakonfiguroval virtuální síťové infrastruktury služby Azure. Aplikace a pracovního vytížení serverů jsou používaný v Azure virtuálních počítačích tento virtuální síť. Vzhledem k tomu Contoso je jen cloudu klienta, všechny identity uživatele, své přihlašovací údaje a členství ve skupinách vytváří a spravuje v Azure AD.

![Azure AD Domain služby základní informace](./media/active-directory-domain-services-overview/aadds-overview.png)

Společnosti Contoso správce IT můžete povolit služby Azure AD Domain pro jejich Azure AD klienta a ho zpřístupnit služby domény v této virtuální sítě. Poté služby Domain Azure AD předpisy spravované domény a díky kterému je dostupný v virtuální sítě. Všechny uživatelské účty členství ve skupinách a přihlašovací údaje uživatele k dispozici v klientovi Azure AD společnosti Contoso jsou také k dispozici v této nově vytvořený domény. Tato funkce umožňuje uživatelům v organizaci Přihlaste se k domény pomocí své firemní přihlašovací údaje – například při vzdáleném připojení k doméně počítačích prostřednictvím vzdálené plochy. Správci možné zajistit přístup ke zdrojům v doméně pomocí existujícího členství ve skupině. Nasazené na virtuálních počítačích virtuální síti se systémem můžete pomocí funkcí, jako je připojení k doméně LDAP pro čtení, LDAP vazbu, NTLM a Kerberos ověřování a zásad skupiny.

Několik důležité aspekty spravované domény, máte k dispozici službou Azure AD Domain Services jsou následujícím způsobem:

- Společnosti Contoso správce IT není nutné spravovat, oprava nebo sledovat tuto doménu nebo některé řadiče domény pro tuto doménu spravovaný.
- Je potřeba spravovat AD replikace u této domény. Uživatelské účty, členství ve skupinách a přihlašovací údaje z klienta Azure AD společnosti Contoso jsou automaticky k dispozici v rámci této spravované domény.
- Protože domény spravuje Azure AD Domain Services, Contoso společnosti správce IT nemá oprávnění správce domény nebo podnikový správce v této doméně.


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>Azure AD Domain Services pro hybridní organizace
Organizace s hybridním IT infrastrukturu používat kombinaci cloudu a místní zdroje. Tyto organizace synchronizovat identity informace z místního adresáře jejich Azure AD klienta. Jak hybridní organizace vypadat migrovat víc jejich místní aplikací do cloudu, zejména starší adresář aplikací, může být užitečné k nim Azure AD Domain Services.

Litware Corporation má nasazené [Azure AD Connect](../active-directory/active-directory-aadconnect.md)synchronizujete identity informace z místního adresáře jejich Azure AD klienta. Identita informace, které se synchronizuje obsahuje uživatelské účty, jejich hash přihlašovací údaje pro ověřování (synchronizaci hesel) a členství ve skupinách.

> [AZURE.NOTE] **Synchronizace hesel je povinné pro hybridní organizace použití služby Azure AD domény**. Tento požadavek je, protože jsou přístupové údaje uživatelů v seznamu spravované doména poskytovanou Azure AD Domain Services ověření tito uživatelé prostřednictvím NTLM nebo Kerberos metody ověřování.

![Azure AD Domain služby společnosti Litware](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

Na předchozím obrázku ukazuje použití Azure AD Domain Services organizace s hybridním IT infrastrukturu, například Litware Corporation. Aplikace a server úloh, které vyžadují doméně služby společnosti Litware jsou nasazenou v virtuální síťové infrastruktury služby Azure. Na litware správce IT můžete povolit služby Azure AD Domain pro jejich Azure AD klienta a ho zpřístupnit spravovaných doménu v tomto virtuální sítě. Protože Litware je organizace s hybridním IT infrastrukturu, uživatelských účtů, skupiny a přihlašovací údaje jsou synchronizovány jejich Azure AD klienta z místního adresáře. Tato funkce umožňuje uživatelům se přihlásit k domény pomocí své firemní přihlašovací údaje – třeba když vzdáleně připojení k počítači připojené k doméně prostřednictvím vzdálené plochy. Správci možné zajistit přístup ke zdrojům v doméně pomocí existujícího členství ve skupině. Nasazené na virtuálních počítačích virtuální síti se systémem můžete pomocí funkcí, jako je připojení k doméně LDAP pro čtení, LDAP vazbu, NTLM a Kerberos ověřování a zásad skupiny.

Několik důležité aspekty spravované domény, máte k dispozici službou Azure AD Domain Services jsou následujícím způsobem:

- Spravované doména je samostatné domény. Není rozšíření Litware na místní domény.
- Na litware správce IT není nutné spravovat, opravu, nebo sledovat řadiče domény pro tuto doménu spravovaný.
- Je potřeba spravovat replikace AD pro tuto doménu. Uživatelské účty, členství ve skupinách a přihlašovací údaje z místního adresáře společnosti Litware jsou synchronizovány Azure AD pomocí Azure AD Connect. Tyto uživatelských účtů členství ve skupinách a přihlašovací údaje jsou automaticky k dispozici v rámci spravované domény.
- Protože domény spravuje Azure AD Domain Services, Litware společnosti správce IT nemá oprávnění správce domény nebo podnikový správce v této doméně.


## <a name="benefits"></a>Výhody
Se službou Azure AD Domain Services můžete využívat následující výhody:

-   **Jednoduché** – splníte identity potřeb virtuálních počítačích používaný služby Azure infrastruktury několika kliknutími jednoduché. Můžete nemusíte nasazovat a spravovat infrastruktura identit v Azure nebo nastavení připojení zpátky na infrastrukturu místních identit.

-   **Integrované** – Azure AD Domain Services je integrováno s Azure AD klienta. Teď můžete Azure AD jako adresář integrovaná cloudové organizace, která caters potřebám moderní aplikace a tradiční adresář aplikací.

-   **Kompatibilní** – Azure AD Domain Services jsou založeny na osvědčené enterprise testu infrastruktury služby Active Directory pro Windows Server. Aplikace můžete proto spolehnout na vyšší úroveň kompatibility s funkcí služby Active Directory pro Windows Server. Ne všechny funkce dostupné v systému Windows Server AD jsou aktuálně dostupné ve službách Azure AD domény. Dostupné funkce jsou však kompatibilní s odpovídající systému Windows Server AD funkcí, u kterých spolehnout ve vaší místní infrastruktury. Funkce spojení LDAP Kerberos, NTLM, zásad skupiny a domény tvoří zdokonaleným nabízení, které byly testováno a kontrast přes různých verzí systému Windows Server.

-   **Efektivní** – služby Azure AD doménu, se můžete vyhnout infrastrukturu a Správa zatížení, který bude přidružený správy identit infrastruktury podporuje tradiční adresář aplikací. Můžete přesouvat aplikací služby Azure infrastruktury a využívat větší úspor na provozní náklady.
