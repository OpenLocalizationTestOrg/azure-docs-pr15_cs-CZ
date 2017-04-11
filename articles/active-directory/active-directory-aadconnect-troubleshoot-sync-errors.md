<properties
    pageTitle="Azure AD Connect: Odstraňování chyb při synchronizaci | Microsoft Azure"
    description="Vysvětluje, jak řešit problémy s chyby při synchronizaci s Azure AD Connect."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Odstraňování chyb při synchronizaci
Při synchronizaci identity dat z Windows Server služby Active Directory (AD DS) pro službu Azure Active Directory (Azure AD), může dojít k chybám. Tento článek obsahuje přehled různých typů chyby synchronizace, některé možné scénáře, které způsobují tyto chyby a potenciální možnosti opravy chyb. Tento článek obsahuje běžné chyby a nesmí zahrnovat všechny možné chyby.

 V tomto článku se předpokládá čtenáře se seznámíte s základní [Návrh koncepcí Azure AD a Azure AD Connect](active-directory-aadconnect-design-concepts.md).

V rámci nejnovější verze Azure AD Connect \(srpen 2016 nebo vyšší\), sestavy chyb synchronizace neexistuje [Azure portál](https://aka.ms/aadconnecthealth) jako součást Azure AD připojení stav synchronizace.


Zahájení 1 dne 2016 [Azure Active Directory duplicitní atribut odolnost proti chybám](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funkce bude k dispozici ve výchozím nastavení pro všechny *nové* Azure Active Directory klienti. Tato funkce bude automaticky k dispozici pro stávajících tenantů v nadcházejících měsících.

Azure AD Connect provádí 3 typy operací z pořád synchronizace adresářů: Import, synchronizace a Export. Chyby může proběhnout v všechny operace. Tento článek hlavně se zaměřuje na chyby při exportu Azure AD.

## <a name="errors-during-export-to-azure-ad"></a>Chyby při exportu Azure AD
Následující části popisuje různé typy chyby synchronizace, které může dojít během operace exportu Azure AD pomocí konektoru Azure AD. Tato spojnice může být označen formát názvu je "contoso. *onmicrosoft.com*".
Chyby při exportu Azure AD označení, že operace \(přidávat, aktualizovat, odstraňovat atd\) pokus o tak, že Azure AD Connect \(Sync Engine\) na Azure Active Directory se nezdařila.

![Přehled chyby exportu](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Chyby dat
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Popis
- Když Azure AD Connect \(sync engine\) pokyn Azure Active Directory, aby přidejte nebo aktualizujte objektů, Azure AD odpovídala příchozí objektu atributu **sourceAnchor** atribut **immutableId** objektů v Azure AD. Tento shody se nazývá **Pevné odpovídat**.
- Když Azure AD **nenalezne** libovolný objekt, který odpovídá **immutableId** atributem atribut **sourceAnchor** příchozí objektu před zřízení nového objektu, přejde zpět k použít atributy ProxyAddresses a UserPrincipalName najít shodu. Tento shody se nazývá **Měkké odpovídat**. Měkké odpovídají slouží podle objekty již v Azure AD (to jsou zdrojovými termíny v Azure AD) novými objekty právě přidali/aktualizace průběhu synchronizace, které představují stejný subjekt (uživatelů, skupin) místně.
- **InvalidSoftMatch** dojde k chybě pevné POZVYHLEDAT nenajde žádné odpovídající objekt **a** měkké odpovídal zjistí odpovídající objekt, ale tento objekt obsahuje jinou hodnotu *immutableId* než na příchozí objekt *SourceAnchor*, znázorňující, že odpovídající objekt byl sesynchronizovaná s jiným objektem z místní služby Active Directory.

Jinými slovy aby měkké porovnat práci, objekt měkkých porovnána s neměli mít každá hodnota parametru *immutableId*. Pokud libovolný objekt s *immutableId* sada se hodnotu selhání pevné POZVYHLEDAT ale splňující kritéria porovnávání měkkých že operace by mít za následek chybu InvalidSoftMatch synchronizace.

Azure Active Directory schématu neumožňuje dva nebo více objektů mít stejnou hodnotu následující atributy. \(Toto není vyčerpávající.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- Objektu

>[AZURE.NOTE] Funkci [odolnosti atribut duplikovat atribut Azure Active Directory](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) je také právě chránění jako výchozí chování Azure Active Directory.  Vytvořením Azure AD více pružné v způsobu zpracování duplicitních ProxyAddresses UserPrincipalName atributy a účastní na místní AD prostředí to bude snižte počet chyb synchronizace prezentací, které Azure AD Connect (i jiných klientech synchronizace). Tato funkce není oprava chyb duplikace. Takže data pořád potřebuje opravit. Ale umožňuje vytváření nových objektů, které jsou jinak blokovány v Azure AD zřízení kvůli duplicitní hodnoty. Taky toto sníží počet chyb synchronizace vráceny klientovi synchronizace.
Pokud je tato funkce je povoleno pro vašeho klienta, neuvidíte chyb synchronizace InvalidSoftMatch vidět během vytváření nových objektů.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Příklady scénářů pro InvalidSoftMatch
1. Dva nebo více objektů stejné hodnoty atributu ProxyAddresses existuje v místní služby Active Directory. Pouze jeden se zobrazuje v Azure AD zřízení.
2. Dva nebo více objektů se stejnou hodnotou userPrincipalName existuje v místní služby Active Directory. Pouze jeden se zobrazuje v Azure AD zřízení.
3. Objekt byl přidán do na místní služby Active Directory se stejnou hodnotu atributu ProxyAddresses jako existující objekt v Azure Active Directory. Objekt přidali místně není začíná zřízení služby Azure Active Directory.
4. Objekt byl přidán do místní služby Active Directory se stejnou hodnotu atributu userPrincipalName jako účet v Azure Active Directory. Objekt není začíná zřízení služby Azure Active Directory.
5. Synchronizované účet byl přesunut ze struktury A struktury B. Azure AD Connect (sync engine) používal ObjectGUID atribut pro výpočet SourceAnchor. Po přesunutí doménové SourceAnchor hodnotu jinou. Nový objekt (ze struktury B) se nezdařilo a synchronizuje ho se existující objekt v Azure AD.
6. Objekt synchronizované máte omylem odstranili z místní služby Active Directory a nový objekt byl vytvořený ve službě Active Directory pro stejný subjekt (například uživatel) aniž by odstranil účet služby Azure Active Directory. Nový účet selhává a synchronizuje ho se stávající Azure AD objekt.
7. Azure AD Connect byl odinstalovat a znova nainstalovat. Během opětovné instalace jiné atribut byla vybrána jako SourceAnchor. Všechny objekty, které bylo dřív synchronizovali ukončit synchronizovat s chybou InvalidSoftMatch.

#### <a name="example-case"></a>Příklad případ:
1. **Petr Novák** je synchronizované uživatele v Azure Active Directory z místní služby Active Directory *contoso.com*
2. Petr Novák **UserPrincipalName** není nastavena jako **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** je **SourceAnchor** vypočítány pomocí Azure AD Connect pomocí Petr Novák **objectGUID** z na prostory služby Active Directory, která je **immutableId** pro Petr Novák v Azure Active Directory.
4. Jan taky obsahuje následující hodnoty **atributu proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Nový uživatel, **Jan Taylora**je přidán do na místní služby Active Directory.
6. Jan Taylora **UserPrincipalName** není nastavena jako **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** je **sourceAnchor** vypočítány pomocí Azure AD Connect pomocí Jan Taylora **objectGUID** z na prostory služby Active Directory. Jan Taylora objekt obsahuje není synchronizované s Azure Active Directory ještě.
8. Jan Taylora obsahuje následující hodnoty atributu proxyAddresses
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. V průběhu synchronizace Azure AD Connect rozpozná sčítání Jan Taylora v místní služby Active Directory a požádejte Azure AD stejné změnu.
10. Azure AD nejdřív provede pevné POZVYHLEDAT. To znamená, bude hledat, pokud je rovno libovolný objekt s immutableId "abcdefghijkl0123456789 ==". Pevné shody se nepovede podle jiného objektu v Azure AD bude mít tento immutableId.
11. Azure AD pokusí měkkých POZVYHLEDAT Jan Taylora. To znamená bude hledat, pokud je rovno tři hodnoty, včetně libovolný objekt s proxyAddressessmtp:bob@contoso.com
12. Azure AD bude hledat objekt Petr Novák kritériím měkkých POZVYHLEDAT. Tento objekt obsahuje hodnotu immutableId, ale = "abcdefghijklmnopqrstuv ==". označující tento objekt byl synchronizovali z jiného objektu z místní služby Active Directory. Proto Azure AD nelze měkký porovnávání tyto objekty a bude mít za následek chybu **InvalidSoftMatch** synchronizace.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Jak opravit chybu InvalidSoftMatch
Nejběžnější důvod, proč chyby InvalidSoftMatch se dvěma objekty s jinou SourceAnchor \(immutableId\) mít stejnou hodnotu pro ProxyAddresses a/nebo UserPrincipalName atributy, které se používají při procesu shody měkkých na Azure AD. Pokud chcete vyřešit neplatné měkké POZVYHLEDAT

1.  Určení duplicitní proxyAddresses, userPrincipalName nebo jinou hodnotu atribut, který způsobuje chybu. Také zjistit, které dvě \(nebo více\) objekty se účastní konfliktu. Sestava vytvořené [Azure AD připojení stav synchronizace](https://aka.ms/aadchsyncerrors) vám může pomoct zjistit dvěma objekty.
2. Určení objektu by měl mít i nadále duplicitní hodnoty a který objekt neměli.
3. Odeberte duplicitní hodnoty z objektu, který není si měli daná hodnota. Všimněte si, že by měl uděláte změny v adresáři, kde je objekt pocházející z. V některých případech může potřebujete odstranit jeden z objektů v konfliktu.
4. Pokud jste změnili v na místní AD nechat Azure AD Connect synchronizovat změny.

Všimněte si, že zprávu o chybách synchronizace v rámci Azure AD připojení stav synchronizace se aktualizuje každých 30 minut a obsahuje chyb z nejnovější pokus o synchronizaci.

>[AZURE.NOTE] ImmutableId, podle definice neměňte v životnost objektu. Pokud Azure AD Connect nebyla nakonfigurována některé scénáře myslet ze seznamu, může skončily v situaci, kdy Azure AD Connect vypočítá jinou hodnotu SourceAnchor AD objektu, který představuje stejné entity (stejné uživatelské / / přidat kontakt do skupiny atd), která má existující Azure AD objekt, který chcete používat.

#### <a name="related-articles"></a>Související články
- [Duplicitní nebo neplatné atributy brání synchronizaci adresářů v Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Popis
Když Azure AD pokusí se měkké porovnat dvě objektů, je možné, že dva objekty různých "typ objektu" (například uživatele, skupinu, kontakt atd.) mají stejné hodnoty pro atributy slouží k provádění měkký POZVYHLEDAT. Duplikování tyto atributy není povolená v Azure AD, operace může způsobit chyby synchronizace "ObjectTypeMismatch".

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Příklady scénářů ObjectTypeMismatch chyby
- Skupiny zabezpečení pošty povolené se vytvoří v Office 365. Správce přidá nový uživatel nebo kontakt v místní nasazení AD (který se nesynchronizuje ještě Azure AD) se stejnou hodnotu atributu proxyAddresses jako skupiny Office 365.

#### <a name="example-case"></a>Příklad případu

1. Správce vytvoří novou skupinu zabezpečení pošty povolené v Office 365 pro oddělení daň a poskytuje e-mailovou adresu jako tax@contoso.com. To přiřadí atributu proxyAddresses pro tuto skupinu s hodnotou**smtp:tax@contoso.com**
2. Vytvoří nový uživatel spojí Contoso.com a účet se vytvoří pro uživatele místně s proxyAddress jako**smtp:tax@contoso.com**
3. Když Azure AD Connect synchronizuje nový uživatelský účet, se zobrazí chybová zpráva "ObjectTypeMismatch".

#### <a name="how-to-fix-objecttypemismatch-error"></a>Jak opravit chybu ObjectTypeMismatch
Nejběžnější důvod, proč chyby ObjectTypeMismatch je dva objekty jiný typ (uživatele, skupinu kontaktů atd.) mají stejnou hodnotu atributu proxyAddresses. Abyste mohli opravit ObjectTypeMismatch takto:

1.  Určení duplicitní proxyAddresses (nebo jiný atribut) hodnota, která způsobuje chybu. Také zjistit, které dvě \(nebo více\) objekty se účastní konfliktu. Sestava generovaný [Azure AD připojení stav synchronizace](https://aka.ms/aadchsyncerrors) vám může pomoct zjistit dvěma objekty.
2. Určení objektu by měl mít i nadále duplicitní hodnoty a který objekt neměli.
3. Odeberte duplicitní hodnoty z objektu, který není si měli daná hodnota. Všimněte si, že by měl uděláte změny v adresáři, kde je objekt pocházející z. V některých případech může potřebujete odstranit jeden z objektů v konfliktu.
4. Pokud jste změnili v na místní AD nechat Azure AD Connect synchronizovat změny. Zprávu o chybách synchronizace v rámci Azure AD připojení stav synchronizace aktualizaci každých 30 minut a obsahuje chyb z nejnovější pokus o synchronizaci.


## <a name="duplicate-attributes"></a>Duplicitní atributy
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Popis
Azure Active Directory schématu neumožňuje dva nebo více objektů mít stejnou hodnotu následující atributy. Je to, že každý objekt v Azure AD bude muset mít jedinečná hodnota tyto atributy k dané instance.

- ProxyAddresses
- UserPrincipalName

Pokud Azure AD Connect pokusí přidat nový objekt nebo aktualizovat existující objekt s hodnotou výše atributů již přiřazené k jinému objektu v Azure Active Directory, operace bude výsledkem chyba: "AttributeValueMustBeUnique" synchronizace.
#### <a name="possible-scenarios"></a>Možné scénáře:
1. Duplicitní hodnoty se přiřadí už synchronizované objekt, který v konfliktu s prací jiného synchronizované objektu.

#### <a name="example-case"></a>Příklad případ:
1. **Petr Novák** je synchronizované uživatele v Azure Active Directory z místní služby Active Directory contoso.com
2. Petr Novák **UserPrincipalName** místně není nastavena jako **bobs@contoso.com**.
3. Jan taky obsahuje následující hodnoty **atributu proxyAddresses** :
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Nový uživatel, **Jan Taylora**je přidán do na místní služby Active Directory.
5. Jan Taylora **UserPrincipalName** není nastavena jako **bobt@contoso.com**.
6. **Jan Taylora** má následující hodnoty atributu **ProxyAddresses** i. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Jan Taylora objekt je synchronizovaný s Azure AD úspěšně.
8. Správce rozhodli aktualizovat atributu **ProxyAddresses** Jan Taylora pomocí následující hodnoty: můžu. **smtp:bob@contoso.com**
9. Azure AD pokusí aktualizovat Jan Taylora objekt v Azure AD pomocí výše uvedená hodnota, ale, že se nezdaří jako že ProxyAddresses hodnota je už přiřazena Petr Novák, výsledkem je chyba "AttributeValueMustBeUnique".

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Jak opravit chybu AttributeValueMustBeUnique
Nejběžnější důvod, proč chyby AttributeValueMustBeUnique se dvěma objekty s jinou SourceAnchor \(immutableId\) mít stejnou hodnotu pro atributy ProxyAddresses a/nebo UserPrincipalName. Chcete-li opravit chybu AttributeValueMustBeUnique

1.  Určení duplicitní proxyAddresses, userPrincipalName nebo jinou hodnotu atribut, který způsobuje chybu. Také zjistit, které dvě \(nebo více\) objekty se účastní konfliktu. Sestava vytvořené [Azure AD připojení stav synchronizace](https://aka.ms/aadchsyncerrors) vám může pomoct zjistit dvěma objekty.
2. Určení objektu by měl mít i nadále duplicitní hodnoty a který objekt neměli.
3. Odeberte duplicitní hodnoty z objektu, který není si měli daná hodnota. Všimněte si, že by měl uděláte změny v adresáři, kde je objekt pocházející z. V některých případech může potřebujete odstranit jeden z objektů v konfliktu.
4. Pokud jste změnili v na místní AD nechat Azure AD Connect synchronizovat změny chyby k získání pevnou.

#### <a name="related-articles"></a>Související články
-[Duplicitní nebo neplatné atributy brání synchronizaci adresářů v Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Neúspěšná ověření dat
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Popis
Azure Active Directory vynucuje různých omezení samotná data před umožněním tato data do měly zapisovat do adresáře. Toto je zajistit, koncoví uživatelé získáte nejlepší možné prostředí při používání aplikace, které jsou závislé na tato data.
#### <a name="scenarios"></a>Scénáře
na. Hodnota atributu UserPrincipalName obsahuje neplatné znaky.
b. Atribut UserPrincipalName není podle požadovaný formát.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Jak opravit chybu IdentityDataValidationFailed

na. Zajistit, že atributu userPrincipalName obsahuje znaky a požadovaný formát.

#### <a name="related-articles"></a>Související články
- [Příprava zřizování uživatelů prostřednictvím synchronizace adresářů pro Office 365]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Popis
Toto je specifických případ, který má za následek chybu synchronizace **"DataValidationFailed"** při změně přípony UserPrincipalName uživatele z jedné federované domény do jiné federované domény.

#### <a name="scenarios"></a>Scénáře
Synchronizované uživateli přípony UserPrincipalName změnil z jednoho federované domény na další federované domény místně. Například *UserPrincipalName = bob@contoso.com * naposledy změnil na *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Příklad
1. Petr Novák, účet pro Contoso.com, se přidají jako nového uživatele ve službě Active Directory s UserPrincipalNamebob@contoso.com
2. Jan přesune do různých dělení Contoso.com s názvem Fabrikam.com a jeho UserPrincipalName se změní nabob@fabrikam.com
3. Contoso.com a fabrikam.com domény jsou federovaných doménách s Azure Active Directory.
4. Janův userPrincipalName aktualizováno a výsledkem chyba "DataValidationFailed" synchronizace.

#### <a name="how-to-fix"></a>Jak opravit
Pokud se přípona UserPrincipalName uživatele z bob@ **contoso.com** k bob@ **fabrikam.com**, kde jsou **contoso.com** a **fabrikam.com** **federovaných doménách**, udělejte pod postup k opravě chyb synchronizace

1. Aktualizace UserPrincipalName uživatele v Azure AD z bob@contoso.com k bob@contoso.onmicrosoft.com. Pomocí následujícího příkazu Powershellu s modul Azure AD Powershellu:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Počkejte, až další synchronizaci cyklus pokusu o synchronizaci. Synchronizace této času bude úspěšné a bude aktualizovat UserPrincipalName Jan k bob@fabrikam.com podle očekávání.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Popis
Pokud atribut přesahuje povolenou velikost, délka limit nebo limit počtu nastavil schématu služby Azure Active Directory, operace synchronizace bude výsledkem chyba synchronizace **LargeObject** nebo **ExceededAllowedLength** . Obvykle k této chybě dochází pro následující atributy

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Možné scénáře
1. Atribut userCertificate Janův je ukládání příliš mnoho certifikátů přiřazená Jan. Patří k nim starší s ukončenou platností certifikátů.
2. Janův thmubnailPhoto nastavit ve službě Active Directory je příliš velký synchronizovat v Azure AD.
3. Při automatické základního souboru atributu proxyAddresses ve službě Active Directory, máte přiřazenou objekt > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Jak opravit

1. Zajistěte, aby byl atribut způsobuje chybu v rámci povolené mezí.

## <a name="related-links"></a>Související odkazy
- [Vyhledejte objekty adresářové služby Active Directory v Centru pro správu služby Active Directory] (https://technet.microsoft.com/library/dd560661.aspx)
- [Jak se dotaz Azure Active Directory pro objekt pomocí Azure Active Directory PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)
