<properties
    pageTitle="Azure AD Connect synchronizace: Principy deklarativní zřizování výrazů | Microsoft Azure"
    description="Tento článek vysvětluje deklarativní zřizovací výrazů."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/31/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-declarative-provisioning-expressions"></a>Azure AD Connect synchronizace: Principy deklarativní zřizování výrazů
Azure AD Connect synchronizace je založena na deklarativní zřizování nejdříve správce identit Forefront 2010. Umožňuje provádět obchodní logiky integrace naprosto bez nutnosti zkompilované kódu.

Důležitou součástí deklarativní zřizování je výraz jazyk používaný v atributu toků. Jazyk používaný tvoří podmnožinu Microsoft® Visual Basic for Applications (VBA). Tento jazyk používat v aplikaci Microsoft Office a uživatelé, kteří mají prostředí VBScript také rozpozná ho. Deklarativní zřizování výraz jazyka pouze pomocí funkcí a není na strukturovaného jazyk. Neexistují žádné metody nebo příkazy. Místo toho jsou vnořených funkcí když toku express program.

Další informace najdete v tématu [Vítá vás Visual Basic for přehled jazyk aplikace pro Office 2013](https://msdn.microsoft.com/library/gg264383.aspx).

Atributy jsou silného. Funkce přijímá pouze atributy správný typ. Je také malá a velká písmena. Funkce názvy a názvy atributu musí být správné velikosti písmen nebo vyvolá se chyba.

## <a name="language-definitions-and-identifiers"></a>Definice jazyk a identifikátory

- Funkce máte název a za ním uveďte argumenty v závorkách: názevfunkce (argument 1, argument N).
- Atributy jsou určeny hranatých závorkách: [Název_atributu]
- Parametry jsou označeny znak procent: název parametru %
- Řetězcové konstanty jsou uzavřeny v uvozovkách: třeba "Contoso" (Poznámka: musíte použít rovné "" a ne oblými "")
- Číselné hodnoty jsou bez nabídek a by měly být desetinné místo. Jsou hodnoty v šestnáctkové soustavě předponou & H. Například 98052 & HFF
- Logické hodnoty jsou vyjádřeny s konstanty: PRAVDA, NEPRAVDA.
- Předdefinované konstanty a literály jsou vyjádřeny se jeho jméno: hodnoty NULL, Line FEED, IgnoreThisFlow

### <a name="functions"></a>Funkce
Povolit možnost transformace hodnoty atributu deklarativní zřizování používá mnoho funkcí. Tyto funkce může být vnořeno, takže vzniknou jednu funkci předaný jiné funkci.

`Function1(Function2(Function3()))`

Úplný seznam funkcí najdete [informace o funkcích](active-directory-aadconnectsync-functions-reference.md).

### <a name="parameters"></a>Parametry
Parametr je definován spojnice nebo správce pomocí Powershellu. Parametry obvykle obsahují hodnoty, které se liší od systému, například název domény uživatele se nachází v. Atribut toků lze použít tyto parametry.

Konektor služby Active Directory podle následujících parametrů příchozí pravidla synchronizace:

| Název parametru | Komentář |
| --- | --- |
| Domain.Netbios | Formát NetBIOS domény aktuálně importovaný, například FABRIKAMSALES |
| Domain.FQDN | Plně kvalifikovaný název domény formát domény aktuálně importovaný, například sales.fabrikam.com |
| Domain.LDAP | Formát LDAP domény aktuálně importovaný, například Datacentrum = prodej, Datacentrum = fabrikam Datacentrum = modelu com |
| Forest.Netbios | NetBIOS formát názvu doménové aktuálně importovaný, například FABRIKAMCORP |
| Forest.FQDN | Plně kvalifikovaný název domény formát názvu doménové aktuálně importovaný, například fabrikam.com |
| Forest.LDAP | LDAP formát názvu doménové aktuálně importovaný, například Datacentrum = fabrikam Datacentrum = modelu com |

Jestli chcete systém poskytuje následující parametr, který se používá k získání identifikátor konektoru aktuálně spuštěných:  
`Connector.ID`

Tady je příklad, který naplní domény atribut metaverse s názvem netbios domény uživatele:  
`domain` <- `%Domain.Netbios%`

### <a name="operators"></a>Operátory
Můžete použít následující operátory:

- **Porovnání**: <, < =, <>, >, > =
- **Matematika**: +, -, \*, -
- **Řetězec**: & (zřetězení)
- **Logické**: & & (a), || (nebo)
- **Pořadí hodnocení**:)

Operátory jsou vyhodnocují zleva doprava a stejnou hodnocení prioritu. To znamená \* (násobek) není vyhodnocena před - (odčítání). 2\*(5 + 3) není stejný jako 2\*5 + 3. Hranaté závorky () slouží ke změně pořadí hodnocení, pokud není vhodné zleva doprava hodnocení pořadí.

## <a name="multi-valued-attributes"></a>Atributy s více hodnotami
Funkce mohou pracovat s jednou hodnotou i s více hodnotami atributy. S více hodnotami atributy funkci působí přes každou hodnotu a platí pro každou hodnotu stejnou funkci.

Příklad:  
`Trim([proxyAddresses])`Udělejte střih všechny hodnoty v atributu proxyAddress.  
`Word([proxyAddresses],1,"@") & "@contoso.com"`Pro každou hodnotu s @-sign, nahradit domény se @contoso.com.  
`IIF(InStr([proxyAddresses],"SIP:")=1,NULL,[proxyAddresses])`Najděte adresa SIP a odeberte ho z hodnoty.

## <a name="next-steps"></a>Další kroky

- Další informace o konfiguraci modelu v [Principy deklarativní zřizování](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
- Přečtěte si téma Jak deklarativní zřizování je použité mimo předdefinovaných v [Principy výchozí konfigurace](active-directory-aadconnectsync-understanding-default-configuration.md).
- Postup provádění změn na praktické pomocí deklarativní zřizování v [tom, jak změnit výchozí konfigurace](active-directory-aadconnectsync-change-the-configuration.md).

**Přehled témat**

- [Azure AD Connect synchronizace: princip a vlastní nastavení synchronizace](active-directory-aadconnectsync-whatis.md)
- [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)

**Odkazy na témata**

- [Azure AD Connect synchronizace: Přehled funkcí](active-directory-aadconnectsync-functions-reference.md)
