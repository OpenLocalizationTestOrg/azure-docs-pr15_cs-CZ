<properties
    pageTitle="Identita synchronizace a duplikovat atribut odolnost proti chybám | Microsoft Azure"
    description="Nové chování postup v případě objekty s UPN nebo ProxyAddress konflikty při synchronizaci adresářů pomocí Azure AD Connect"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identita synchronizace a duplikovat atribut odolnost proti chybám
Duplicitní odolnost proti chybám atribut je funkce služby Azure Active Directory, jejichž odstraníte tření způsobená **UserPrincipalName** a **ProxyAddress** konflikty při spuštění jednu z nástroje pro synchronizaci společnosti Microsoft.

Tyto dva atributy jsou obvykle musí být jedinečný přes všechny **uživatele**, **skupinu**nebo **kontakt** objektů v dané klienta služby Azure Active Directory.

> [AZURE.NOTE] Pouze uživatelé mohou mít UPN.

Nové chování, který umožňuje tato funkce je v cloudu část kanálu synchronizace, proto klienta, která a relevantními značkami žádné synchronizace produktu společnosti Microsoft včetně Azure AD Connect, DirSync a MIM + spojnice. Obecný termín "synchronizačního klienta" slouží v tomto dokumentu představovat některou z těchto produktů.

## <a name="current-behavior"></a>Aktuální chování
Při pokusu o zřízení nového objektu s UPN nebo ProxyAddress hodnotou, který porušuje zásady toto omezení jedinečnost Azure Active Directory blokuje tento objekt z vzniku. Podobně když objektu je aktualizován-jedinečné UPN nebo ProxyAddress, aktualizace se nezdaří. Pokus o zajišťování nebo aktualizace opakované tak, že synchronizačního klienta při každé export obrázku a opakovaně selhává, dokud vyřeší konfliktu. Vygeneruje se při každém pokusu o e-mailu chybovou zprávu a chybu je zaznamenané synchronizačního klienta.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Chování s odolnost proti chybám duplicitní atribut
Místo úplně selhání zřízení nebo aktualizovat objekt s atributem duplicitní Azure Active Directory "izoluje" duplicitní atribut, který by porušovat omezení jedinečnosti. Pokud Tenhle atribut je nutný pro zřizování, jako je UserPrincipalName, přiřadí službu hodnotu zástupného symbolu. Formát tyto dočasné hodnoty  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>. onmicrosoft.com***".  
Pokud atribut nepožaduje, jako je **ProxyAddress**, Azure Active Directory jednoduše izoluje atribut konflikt a pokračovat vytváření objektu nebo aktualizace.

Po umístění do karantény atribut, nejsou odesílány informace o konfliktu ve stejné chybovou zprávu e-mailu použít starý chování. Ale tyto informace o je vidět jenom v chybové zprávě jednorázové, až karanténa se stane, ho není dál Zaprotokolují budoucí e-mailů. Taky, protože exportovat pro tento objekt obsahuje úspěšný, synchronizačního klienta nezaznamenává chybu a opakované pokusy o vytvořit / aktualizaci na pozdější synchronizaci cyklů.

Pro tuto funkci podporují přidala nový atribut třídy objektů uživatelů, skupin a kontaktů:  
**DirSyncProvisioningErrors**

Toto je atribut s více hodnotami, který se používá k ukládání konfliktní atributy, které by porušovat omezení jedinečnosti by měl být přidají obvyklým způsobem. Úlohy časovače na pozadí je povolená v Azure Active Directory, která poběží každou hodinu můžete vyhledávat duplicitní atribut konflikty, které byly nevyřešil, automaticky odstraní atributy dotyčného v karanténě.

### <a name="enabling-duplicate-attribute-resiliency"></a>Povolení odolnost proti chybám duplicitní atribut
Duplicitní atribut odolnosti bude nové výchozí chování přes všechny klienty služby Azure Active Directory. Bude na ve výchozím nastavení pro všechny klienty, které povolená synchronizace poprvé 22 srpen 2016 nebo novější. Klienti, které povolená synchronizace před tímto datem mají zapnutou po dávkách. Tento zavedení začne v září 2016 a e-mailové oznámení odešle každého klienta technické oznámení kontaktu s určité datum, kdy zapnout.

Jakmile má zapnutý odolnost proti chybám duplikujte atribut nedá zakázat.

Pokud chcete zjistit, pokud je funkce pro vašeho klienta, můžete to udělat tak, že si stáhnete nejnovější verzi modulu Azure Active Directory PowerShell a spuštění:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

Pokud chcete před Minipřekladač je zapnutý pro vašeho klienta včasným povolit funkci, stačí si stáhnete nejnovější verzi modulu Azure Active Directory PowerShell a spuštěním:

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>Identifikační objektů s DirSyncProvisioningErrors
Jak identifikovat objekty, které mají tyto chyby kvůli duplicitní vlastnost konfliktů, Azure Active Directory PowerShell a portálu pro správu Office 365 aktuálně dvěma způsoby. Existuje plány rozšířit další portálu základě vytváření sestav v budoucnu.

### <a name="azure-active-directory-powershell"></a>Prostředí PowerShell Azure Active Directory
Pro rutinách Powershellu v tomto tématu platí následující:

- Všechny tyto rutiny jsou malá a velká písmena.
- **– ErrorCategory PropertyConflict** musí vždy obsahovat. Neexistují žádné jiné druhy **ErrorCategory**, ale tento prodloužit v budoucnu.

Nejdřív začněte spuštěním **Připojit MsolService** a zadání přihlašovacích údajů pro správce klienta.

Potom pomocí následující rutiny a operátory zobrazit chyby různými způsoby:

1. [Zobrazit všechny](#see-all)

2. [Podle typu vlastnost](#by-property-type)

3. [Konfliktní hodnotou](#by-conflicting-value)

4. [Pomocí pole Hledat řetězec](#using-a-string-search)

5. [Řazení](#sorted)

6. [Omezené množství nebo všechny](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>Zobrazit všechny
Po připojení zobrazíte seznam obecné atribut zřizování chyb v klientovi spuštění:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

To bude výsledek třeba takto:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>Podle typu vlastnost
Chyby podle typu vlastnost zobrazíte přidání příznaku **Položka Název vlastnosti –** s argumentem **UserPrincipalName** nebo **ProxyAddresses** :

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Nebo

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Konfliktní hodnotou
Zobrazit chyby týkající se přidat určitou vlastnost **– Hodnota_vlastnosti** příznak (**- Položka Název vlastnosti** musí se používat i při přidávání tento příznak):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>Pomocí pole Hledat řetězec
Hledání obecných řetězec lze provést pomocí příznaku **- SearchString** . Lze použít nezávisle ze všech výše uvedených příznaky, s výjimkou **-ErrorCategory PropertyConflict**, který je vždy nutný:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Omezené množství nebo všechny
1. **MaxResults <Int> ** mohou sloužit k omezit dotaz určitý počet hodnot.

2. **Všechny** lze zajistit, aby že všechny výsledky načtením v případě, že existuje velký počet chyb.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Portálu pro správu Office 365

V Centru pro správu Office 365 můžete zobrazit chyby synchronizace adresářů. Sestava v portálu Office 365 se zobrazí pouze objekty **uživatelů** , které mají tyto chyby. Nezobrazuje informace o konfliktech mezi **skupinami** a **Kontakty**.


![Aktivní uživatelé] (./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "Aktivní uživatelé")

Další informace o tom, jak zobrazit chyby synchronizace adresářů v Centru pro správu Office 365 najdete v článku [identifikovat chyb synchronizace adresářů v Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).


### <a name="identity-synchronization-error-report"></a>Zprávu o chybách synchronizace identity
Při zpracovávání objekt s konfliktem duplicitní atribut s toto nové chování oznámení je součástí standardní e-mailovou zprávu o chybách synchronizace identit, odesílaný velkému technické oznámení kontaktování pro klienta. Je ale důležité změnit toto chování. V minulosti informace o konfliktu duplicitní atribut by být součástí každé následující chybové zprávy, dokud vyřešit konflikt. S toto nové chování oznamování chyb pro dané konflikt zobrazí, pouze jednou – v době, kdy konfliktní atribut je uložená v karanténě.

Tady je příklad vypadá e-mailového oznámení pro ProxyAddress konflikt:  
    ![Aktivní uživatelé](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>Řešení konfliktů
Poradce při potížích s taktik strategie a řešení těchto chyb neměli liší od způsobu duplicitní atribut chyby byly v minulosti. Jediný rozdíl je, že úloh časovače posunuje přes klienta na straně služby automaticky doplnit atribut dotyčného správné objekt po vyřešit konflikt.

Následující článek pojednává o různých strategie pro řešení problémů a řešení: [duplicitní nebo neplatné atributy brání synchronizaci adresářů v Office 365](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Známé problémy
Žádné z těchto známých problémů způsobí, že data ztrátě nebo služby snížení úrovně. Některé z nich jsou estetické, ostatní argumentem standardní "*před odolnost proti chybám*" duplicitní atribut vyvolání místo umístění do karantény atribut konflikt a jiné způsobí, že některé chyby vyžadovat další Ruční oprava zdola nahoru.

**Základní chování:**

1. Objekty s konfigurací konkrétní atribut nepřestanou objevovat chyby export namísto duplicitní atributy jsou uložená v karanténě.  
Příklad:

    na. Ve službě Active Directory s název UPN se vytvoří nový uživatel **Joe@contoso.com** a ProxyAddress**smtp:Joe@contoso.com**

    b. Vlastnosti objektu v konfliktu se změnami existující skupiny, kde je ProxyAddress **SMTP:Joe@contoso.com**.

    c. Při exportu **ProxyAddress konfliktu** se chyba místo s atributy konflikt uložená v karanténě. Operaci je při každé obrázku pozdější synchronizaci opakovat, jako by bylo před povolením funkci odolnosti.

2. Pokud dvěma skupinami se vytvářejí místního stejná adresa SMTP, jeden selže zřízení na prvním pokusu se standardní chyba **ProxyAddress** duplicitní. Však duplicitní hodnoty je správně v karanténě při další synchronizaci obrázku.

**Sestava portálu office**:

1. Podrobné chybová zpráva pro dva objekty v sadě konflikt Přípony se stejně. Tento údaj označuje, že obě měly, jejich Přípony změnit / uložená v karanténě, když ve skutečnosti pouze jeden z nich měli žádná data změnit.

2. Podrobné chybová zpráva konflikt UPN zobrazuje nepovedlo displayName pro uživatele, který byl jejich Přípony změnit/uložená v karanténě. Příklad:

    na. **Uživatel A** synchronizuje prvního s **UPN = User@contoso.com **.

    b. **Uživatel B** pokusu o synchronizovat přes další s **UPN = User@contoso.com **.

    c. **Uživatele B** Přípony se změní na **User1234@contoso.onmicrosoft.com** a **User@contoso.com** se přidá do **DirSyncProvisioningErrors**.

    d. Chybová zpráva **Uživatel B** uvádět už má **uživatel A** **User@contoso.com** jako UPN, ale zobrazuje vlastní displayName **Uživatel B** .



**Zprávu o chybách synchronizace identity**:

Odkaz na *Postup při řešení tohoto problému* je nesprávné:  
    ![Aktivní uživatelé](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

Odkazovat na [https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).


## <a name="see-also"></a>Viz taky

- [Synchronizace Azure AD Connect](active-directory-aadconnectsync-whatis.md)

- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)

- [Zjištění chyby synchronizace adresářů v Office 365](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
