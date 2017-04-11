<properties
    pageTitle="Azure AD Connect synchronizace: Přehled funkcí | Microsoft Azure"
    description="Odkaz deklarativní zřizovací výrazů v Azure AD Connect synchronizovat."
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
    ms.date="08/23/2016"
    ms.author="andkjell;markvi"/>


# <a name="azure-ad-connect-sync-functions-reference"></a>Azure AD Connect synchronizace: Přehled funkcí

V Azure AD Connect funkce slouží k práci s hodnotě atributu při synchronizaci.  
Syntaxe funkce vyjádřena pomocí v tomto formátu:  
`<output type> FunctionName(<input type> <position name>, ..)`

Pokud funkce přetížený a přijme více syntaxí, jsou uvedené všechny platné syntaxe.  
Funkce jsou silného a jejich ověření, že typ předaný shody dokumentovaného typu.  
Pokud požadovaný typ neodpovídá, vyvolá se chyba.

Jaké jsou vyjádřeny pomocí následující syntaxe:

- **Koš** – binární.
- **logická hodnota** – logická hodnota
- **dt** – UTC kalendářních dat/časů
- **výčet** – výčet známé konstant
- **exp** – výraz, který se má vyhodnocena jako logická hodnota
- **mvbin** – seznamem binární
- **mvstr** – seznamem řetězec
- **mvref** – odkaz seznamem
- **num** – číselné
- **ref** – přehled
- **funkce str** – řetězec
- **var** – hodnotu typu variant (téměř) jiný typ
- **Zrušit** – nevrací hodnotu

Funkce s typy **mvbin**, **mvstr**a **mvref** pouze pracovat s více hodnotami atributy. Funkce s **Koš** **str**a **ref** pracovat na atributy jedinou hodnotu i s více hodnotami.

## <a name="functions-reference"></a>Přehled funkcí

Seznam funkcí | | | | |  
--------- | --------- | --------- | --------- | --------- | ---------
**Převod** |  
[Funkce CBool](#cbool) | [Funkce CDate](#cdate) | [CGuid](#cguid) | [ConvertFromBase64](#convertfrombase64)
[ConvertToBase64](#converttobase64) | [ConvertFromUTF8Hex](#convertfromutf8hex) | [ConvertToUTF8Hex](#converttoutf8hex) | [CNum](#cnum)
[CRef](#cref) | [Funkce CStr](#cstr) | [StringFromGuid](#StringFromGuid) | [StringFromSid](#stringfromsid)
**Data / času** |  
[Funkce DateAdd](#dateadd) | [DateFromNum](#datefromnum) | [FormatDateTime](#formatdatetime) | [Nyní](#now)
[NumFromDate](#numfromdate) |  
**Adresář** |  
[DNComponent](#dncomponent) | [DNComponentRev](#dncomponentrev) | [EscapeDNComponent](#escapedncomponent)
**Hodnocení** |  
[IsBitSet](#isbitset) | [Funkce IsDate](#isdate) | [IsEmpty](#isempty) | [IsGuid](#isguid)
[Funkce IsNull](#isnull) | [IsNullOrEmpty](#isnullorempty) | [Funkce IsNumeric](#isnumeric) | [IsPresent](#ispresent) |
[IsString](#isstring) |  
**Matematické** |  
[BitAnd](#bitand) | [BitOr](#bitor) | [RandomNum](#randomnum)
**S více hodnotami** |  
[Obsahuje](#contains) | [Počet](#count) | [Položky](#item) | [ItemOrNull](#itemornull)
[Spojení](#join) | [RemoveDuplicates](#removeduplicates) | [Rozdělení](#split) |
**Tok programu** |  
[Chyba](#error) | [FUNKCE IIF](#iif)  | [Přepínač](#switch)
**Text** |  
[IDENTIFIKÁTOR GUID](#guid) | [Funkce InStr](#instr) | [InStrRev](#instrrev) | [Funkce LCase](#lcase)
[Vlevo](#left) | [Funkce LEN](#len) | [Funkce LTrim](#ltrim) | [Funkce MID](#mid)
[PadLeft](#padleft) | [PadRight](#padright) | [PCase](#pcase) | [Nahrazení](#replace)
[ReplaceChars](#replacechars) | [Vpravo](#right) | [Funkce RTrim](#rtrim) | [Funkce Trim](#trim)
[Funkce UCase](#ucase) | [Aplikace Word](#word)

----------
### <a name="bitand"></a>BitAnd

**Popis:**  
Funkce BitAnd nastaví zadaný počet podle hodnoty.

**Syntaxe:**  
`num BitAnd(num value1, num value2)`

- Hodnota1; hodnota2: Číselná hodnota, která by měla být logickým společně

**Poznámky:**  
Tato funkce převede obou parametrů na zápis v binární soustavě a nastaví trochu:

- 0 - li jednu nebo obě odpovídající bity *masky* a *příznak* rovnají 0
- 1 – Pokud jsou obě odpovídající posunuto 1.

Jinými slovy vrátí hodnotu 0 ve všech případech kromě při odpovídající mají bity obou parametrů hodnotu 1.

**Příklad:**  
`BitAnd(&HF, &HF7)`  
Vrátí hodnotu 7, protože šestnáctkové "F" a "F7" jsou vyhodnoceny jako tuto hodnotu.

----------
### <a name="bitor"></a>BitOr

**Popis:**  
Funkce BitOr nastaví zadaný počet podle hodnoty.

**Syntaxe:**  
`num BitOr(num value1, num value2)`

- Hodnota1; hodnota2: Číselná hodnota, která by měla být sjednocením (OR) společně

**Poznámky:**  
Tato funkce převede obou parametrů na zápis v binární soustavě a nastaví trochu 1, pokud jeden nebo oba odpovídající bity masky a příznak hodnotu 1 a 0 jsou-li obě odpovídající posunuto 0. Jinými slovy vrátí 1 ve všech případech kromě kde odpovídající bity obou parametrů rovnají 0.

----------
### <a name="cbool"></a>Funkce CBool

**Popis:**  
Funkce CBool vrátí logickou hodnotu na základě vyhodnocené výrazu

**Syntaxe:**  
`bool CBool(exp Expression)`

**Poznámky:**  
Pokud výraz vyhodnocen jako nenulová hodnota, funkce CBool vrátí hodnotu PRAVDA, a pak, jinak se ještě vrátí hodnotu False.

**Příklad:**  
`CBool([attrib1] = [attrib2])`  

Vrátí hodnotu PRAVDA, pokud obou atributy mít stejnou hodnotu.

----------
### <a name="cdate"></a>Funkce CDate

**Popis:**  
Funkce CDate vrátí data a času UTC řetězce. Data a času není typu nativní atribut synchronní, ale používá některé funkce.

**Syntaxe:**  
`dt CDate(str value)`

- Hodnota: Řetězec s datum, čas a volitelně časového pásma

**Poznámky:**  
Vrácený řetězec je vždy UTC.

**Příklad:**  
`CDate([employeeStartTime])`  
Vrátí DateTime podle zaměstnance spuštění

`CDate("2013-01-10 4:00 PM -8")`  
Vrátí datum a čas představující "2013-01-11 12:00 dop."

----------
### <a name="cguid"></a>CGuid

**Popis:**  
Funkce CGuid převede číselné řetězce identifikátor GUID jeho zápis v binární soustavě.

**Syntaxe:**  
`bin CGuid(str GUID)`

- Řetězec formátovanou v tomto vzoru: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

----------
### <a name="contains"></a>Obsahuje

**Popis:**  
Funkce Contains vyhledá řetězce uvnitř atribut s více hodnotami

**Syntaxe:**  
`num Contains (mvstring attribute, str search)`– malá a velká písmena  
`num Contains (mvstring attribute, str search, enum Casetype)`  
`num Contains (mvref attribute, str search)`– malá a velká písmena

- Atribut: s více hodnotami atribut hledání.
- hledání: řetězec v atributu najít.
- Casetype: CaseInsensitive nebo CaseSensitive.

Vrátí index v atributu s více hodnotami nalezených řetězec. Pokud tento řetězec není nalezen, je vrácená 0.

**Poznámky:**  
Hledání s více hodnotami řetězec atributů najde podřetězec v hodnoty.  
Odkaz atributů prohledávaných řetězce musí přesně shodovat hodnotu považuje za shodu.

**Příklad:**  
`IIF(Contains([proxyAddresses],"SMTP:")>0,[proxyAddresses],Error("No primary SMTP address found."))`  
Pokud atributu proxyAddresses má primární e-mailovou adresu (určený malá "SMTP:"), pak se vraťte atribut proxyAddress, jinak vrátí chybu.

----------
### <a name="convertfrombase64"></a>ConvertFromBase64

**Popis:**  
Funkce ConvertFromBase64 převede hodnoty zadané ve formátu Base 64 zakódovaný na běžná řetězec.

**Syntaxe:**  
`str ConvertFromBase64(str source)`-předpokládá pro kódování Unicode  
`str ConvertFromBase64(str source, enum Encoding)`

- zdroje: kódováním Base 64 řetězec  
- Kódování: Unicode, ASCII, UTF-8

**Příklad**  
`ConvertFromBase64("SABlAGwAbABvACAAdwBvAHIAbABkACEA")`  
`ConvertFromBase64("SGVsbG8gd29ybGQh", UTF8)`

Vraťte se oba příklady "*Vítáme!*"

----------
### <a name="convertfromutf8hex"></a>ConvertFromUTF8Hex

**Popis:**  
Funkce ConvertFromUTF8Hex převede zadanou hodnotu UTF8 Hex zakódovaný řetězec.

**Syntaxe:**  
`str ConvertFromUTF8Hex(str source)`

- zdroje: UTF8 2bajtové zakódovaný palčivost

**Poznámky:**  
Rozdíl mezi této funkce a ConvertFromBase64([],UTF8), výsledkem je vhodné pro atribut DN.  
Tento formát je Azure Active Directory používat jako název domény.

**Příklad:**  
`ConvertFromUTF8Hex("48656C6C6F20776F726C6421")`  
Vrátí hodnotu "*Vítáme!*"

----------
### <a name="converttobase64"></a>ConvertToBase64

**Popis:**  
Funkce ConvertToBase64 převede řetězec na řetězec ve formátu Base 64.  
Převede hodnoty v matici s celými čísly vyjádření odpovídající řetězec, který kódovaný pomocí základu-64 číslic.

**Syntaxe:**  
`str ConvertToBase64(str source)`

**Příklad:**  
`ConvertToBase64("Hello world!")`  
Vrátí "SABlAGwAbABvACAAdwBvAHIAbABkACEA"

----------
### <a name="converttoutf8hex"></a>ConvertToUTF8Hex

**Popis:**  
Funkce ConvertToUTF8Hex převede řetězec na hodnotu UTF8 Hex kódování.

**Syntaxe:**  
`str ConvertToUTF8Hex(str source)`

**Poznámky:**  
Výstupní formát tato funkce slouží Azure Active Directory jako formát atributu DN.

**Příklad:**  
`ConvertToUTF8Hex("Hello world!")`  
Vrátí 48656C6C6F20776F726C6421

----------
### <a name="count"></a>Počet

**Popis:**  
Funkce Count vrátí počet prvků v atribut s více hodnotami

**Syntaxe:**  
`num Count(mvstr attribute)`

----------
### <a name="cnum"></a>CNum

**Popis:**  
Funkce CNum zabírá řetězce a vrátí číselný typ dat.

**Syntaxe:**  
`num CNum(str value)`

----------
### <a name="cref"></a>CRef

**Popis:**  
Převede řetězec atribut odkaz

**Syntaxe:**  
`ref CRef(str value)`

**Příklad:**  
`CRef("CN=LC Services,CN=Microsoft,CN=lcspool01,CN=Pools,CN=RTC Service," & %Forest.LDAP%)`

----------
### <a name="cstr"></a>Funkce CStr

**Popis:**  
Funkce CStr převede na datový typ string.

**Syntaxe:**  
`str CStr(num value)`  
`str CStr(ref value)`  
`str CStr(bool value)`  

- hodnota: může být číselnou hodnotu, odkaz atribut nebo logická hodnota.

**Příklad:**  
`CStr([dn])`  
Může vrátit "cn = Jana, datacentrum = contoso, datacentrum = com"

----------
### <a name="dateadd"></a>Funkce DateAdd

**Popis:**  
Vrátí datum obsahující datum, ke kterému je přidán zadaný časový interval.

**Syntaxe:**  
`dt DateAdd(str interval, num value, dt date)`

- interval: řetězcový výraz, který je časové období chcete přidat. Řetězec musí mít jednu z následujících hodnot:
 - yyyy rok
 - q čtvrtletí
 - m měsíc
 - y den roku
 - d den
 - w den v týdnu
 - ww týden
 - h hodinu
 - n min
 - s druhé
- hodnota: počet jednotek, který chcete přidat. Může být (Chcete-li získat data v budoucnosti) obojím (Chcete-li získat datum v minulosti).
- datum: datum a čas představující datum, ke kterému je přidán interval.

**Příklad:**  
`DateAdd("m", 3, CDate("2001-01-01"))`  
Přidá 3 měsíce a vrátí DateTime představující "2001 04 01".

----------
### <a name="datefromnum"></a>DateFromNum

**Popis:**  
Funkce DateFromNum převede hodnotu na AD datum formátování k typu datum a čas.

**Syntaxe:**  
`dt DateFromNum(num value)`

**Příklad:**  
`DateFromNum([lastLogonTimestamp])`  
`DateFromNum(129699324000000000)`  
Vrátí datum a čas, které představuje 2012 01 01 23:00:00

----------
### <a name="dncomponent"></a>DNComponent

**Popis:**  
Funkce DNComponent vrátí hodnotu určené součásti DN přejdete zleva.

**Syntaxe:**  
`str DNComponent(ref dn, num ComponentNumber)`

- rozlišující název: atribut odkaz při interpretaci miniaplikace
- ComponentNumber: Součástí DN vrátíte

**Příklad:**  
`DNComponent([dn],1)`  
Pokud je dn "cn = Jana, ou =...," vrátí Jana

----------
### <a name="dncomponentrev"></a>DNComponentRev

**Popis:**  
Funkce DNComponentRev vrátí hodnotu určené součásti DN přecházet od doprava (end).

**Syntaxe:**  
`str DNComponentRev(ref dn, num ComponentNumber)`  
`str DNComponentRev(ref dn, num ComponentNumber, enum Options)`

- rozlišující název: atribut odkaz při interpretaci miniaplikace
- ComponentNumber - součástí DN vrátíte
- Možnosti: Datacentrum – ignorovat všechny komponenty se "datacentrum ="

**Příklad:**  
Pokud dn "cn = Jana, ou = Plzeň, ou = GA, ou = US, datacentrum = contoso, datacentrum = com" potom  
`DNComponentRev([dn],3)`  
`DNComponentRev([dn],1,"DC")`  
Jak vrátit US.

----------
### <a name="error"></a>Chyba

**Popis:**  
Vlastní chybu vrátit se používá chybovou funkci.

**Syntaxe:**  
`void Error(str ErrorMessage)`

**Příklad:**  
`IIF(IsPresent([accountName]),[accountName],Error("AccountName is required"))`  
Pokud atribut účtu se nezobrazí, vyvolá chybu na požadovaném objektu.

----------
### <a name="escapedncomponent"></a>EscapeDNComponent

**Popis:**  
Funkce EscapeDNComponent zabírá jednou součástí názvu domény a řídící, aby představoval v LDAP.

**Syntaxe:**  
`str EscapeDNComponent(str value)`

**Příklad:**  
`EscapeDNComponent("cn=" & [displayName]) & "," & %ForestLDAP%)`  
Zajišťuje, aby že objekt lze vytvořit adresář LDAP i v případě atribut displayName obsahuje znaky, které je nutné uvést v LDAP.

----------
### <a name="formatdatetime"></a>FormatDateTime

**Popis:**  
Funkce FormatDateTime slouží k formátování pomocí zadaného formátu data a času na řetězec

**Syntaxe:**  
`str FormatDateTime(dt value, str format)`

- hodnota: hodnota ve formátu DateTime
- formát: typu string představující formátu chcete převést.

**Poznámky:**  
Možné hodnoty pro formátu najdete tady: [Formáty data a času definované uživatelem (funkce Format)](http://msdn2.microsoft.com/library/73ctwf33\(VS.90\).aspx)

**Příklad:**  

`FormatDateTime(CDate("12/25/2007"),"yyyy-mm-dd")`  
Výsledkem "2007 – 12-25".

`FormatDateTime(DateFromNum([pwdLastSet]),"yyyyMMddHHmmss.0Z")`  
Můžete mít za následek "20140905081453.0Z"

----------
### <a name="guid"></a>IDENTIFIKÁTOR GUID

**Popis:**  
Funkce GUID vygeneruje nové náhodné GUID

**Syntaxe:**  
`str GUID()`

----------
### <a name="iif"></a>FUNKCE IIF

**Popis:**  
Funkce IIF vrátí jednu ze sady možných hodnot na základě zadané podmínky.

**Syntaxe:**  
`var IIF(exp condition, var valueIfTrue, var valueIfFalse)`

- Podmínka: Libovolná hodnota nebo výraz, který může být vyhodnocen jako true nebo false.
- valueIfTrue: Pokud podmínka vyhodnotí jako PRAVDA, vrácená hodnota.
- valueIfFalse: Pokud podmínka vyhodnotí jako NEPRAVDA, vrácená hodnota.

**Příklad:**  
`IIF([employeeType]="Intern","t-" & [alias],[alias])`  
 Pokud je uživatel učně, vrátí alias uživatele s "t-" přidána na začátek ho ještě vrátí alias uživatele je.

----------
### <a name="instr"></a>Funkce InStr

**Popis:**  
Funkce InStr najde prvního výskytu podřetězec v řetězci

**Syntaxe:**  

`num InStr(str stringcheck, str stringmatch)`  
`num InStr(str stringcheck, str stringmatch, num start)`  
`num InStr(str stringcheck, str stringmatch, num start , enum compare)`

- prohledávaný_řetězec: řetězec, který má prohledávat
- hledaný_řetězec: řetězec, který má být nalezena
- spuštění: počáteční pozice zobrazíte podřetězec
- porovnání: vbTextCompare nebo vbBinaryCompare

**Poznámky:**  
Vrátí pozici kde podřetězec nebyla nalezena nebo 0, pokud nebyl nalezen.

**Příklad:**  
`InStr("The quick brown fox","quick")`  
Hodnoty 5

`InStr("repEated","e",3,vbBinaryCompare)`  
Vyhodnotí až 7

----------
### <a name="instrrev"></a>InStrRev

**Popis:**  
Funkce InStrRev najde poslední výskyt podřetězec v řetězci

**Syntaxe:**  
`num InstrRev(str stringcheck, str stringmatch)`  
`num InstrRev(str stringcheck, str stringmatch, num start)`  
`num InstrRev(str stringcheck, str stringmatch, num start, enum compare)`

- prohledávaný_řetězec: řetězec, který má prohledávat
- hledaný_řetězec: řetězec, který má být nalezena
- spuštění: počáteční pozice zobrazíte podřetězec
- porovnání: vbTextCompare nebo vbBinaryCompare

**Poznámky:**  
Vrátí pozici kde podřetězec nebyla nalezena nebo 0, pokud nebyl nalezen.

**Příklad:**  
`InStrRev("abbcdbbbef","bb")`  
Vrátí hodnotu 7

----------
### <a name="isbitset"></a>IsBitSet

**Popis:**  
Funkce IsBitSet testů Pokud tento bit je nastavený nebo ne

**Syntaxe:**  
`bool IsBitSet(num value, num flag)`

- hodnota: číselnou hodnotu, která je evaluated.flag: číselnou hodnotu, která obsahuje bit má být vyhodnocen

**Příklad:**  
`IsBitSet(&HF,4)`  
Vrátí hodnotu PRAVDA, protože v šestnáctkovou hodnotu "F" je nastaven bit "4"

----------
### <a name="isdate"></a>Funkce IsDate

**Popis:**  
Pokud může být výraz vyhodnotí jako typu datum a čas, pak Funkce IsDate vrací logickou hodnotu PRAVDA.

**Syntaxe:**  
`bool IsDate(var Expression)`

**Poznámky:**  
Slouží k určení Pokud CDate() může být úspěšný.

----------
### <a name="isempty"></a>IsEmpty

**Popis:**  
Pokud atribut existuje v CS nebo více hodnot, ale je vyhodnocen jako prázdný řetězec, pak funkce IsEmpty vrací logickou hodnotu PRAVDA.

**Syntaxe:**  
`bool IsEmpty(var Expression)`

----------
### <a name="isguid"></a>IsGuid

**Popis:**  
Pokud řetězec může převést na identifikátor GUID, pak funkce IsGuid vyhodnocen jako true.

**Syntaxe:**  
`bool IsGuid(str GUID)`

**Poznámky:**  
Identifikátor GUID je definována následujícím řetězec sledovat jednu z těchto vzory: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx nebo {xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}

Slouží k určení Pokud CGuid() může být úspěšný.

**Příklad:**  
`IIF(IsGuid([strAttribute]),CGuid([strAttribute]),NULL)`  
Pokud StrAttribute formátem GUID, vrátit zápis v binární soustavě, jinak vrátí Null.

----------
### <a name="isnull"></a>Funkce IsNull

**Popis:**  
Pokud je výraz vyhodnocen jako hodnota Null, vrátí funkce IsNull PRAVDA.

**Syntaxe:**  
`bool IsNull(var Expression)`

**Poznámky:**  
Atribut je hodnota Null vyjádřeného absence atribut.

**Příklad:**  
`IsNull([displayName])`  
Vrátí hodnotu PRAVDA, pokud atribut není k dispozici v CS nebo více hodnot.

----------
### <a name="isnullorempty"></a>IsNullOrEmpty

**Popis:**  
Pokud je výraz null nebo prázdný řetězec, pak IsNullOrEmpty funkce vrátí hodnotu true.

**Syntaxe:**  
`bool IsNullOrEmpty(var Expression)`

**Poznámky:**  
Pro atribut tím byste jsou vyhodnoceny jako PRAVDA Pokud atribut chybí nebo existuje, ale je prázdný řetězec.  
Inverzní funkce jmenuje IsPresent.

**Příklad:**  
`IsNullOrEmpty([displayName])`  
Vrátí hodnotu PRAVDA, pokud atribut není k dispozici nebo se prázdný řetězec v CS nebo více hodnot.

----------
### <a name="isnumeric"></a>Funkce IsNumeric

**Popis:**  
Funkce IsNumeric vrátí hodnotu typu Boolean označující, zda může být výraz vyhodnocen jako typu číslo.

**Syntaxe:**  
`bool IsNumeric(var Expression)`

**Poznámky:**  
Slouží k určení Pokud CNum() může být úspěšný analyzovat výraz.

----------
### <a name="isstring"></a>IsString

**Popis:**  
Pokud může být výraz vyhodnocen jako řetězec napíšete, pak IsString funkce vrací logickou hodnotu PRAVDA.

**Syntaxe:**  
`bool IsString(var expression)`

**Poznámky:**  
Slouží k určení Pokud CStr() může být úspěšný analyzovat výraz.

----------
### <a name="ispresent"></a>IsPresent

**Popis:**  
Pokud je výraz vyhodnocen řetězcem, který není Null ani není prázdný, vrátí funkce IsPresent true.

**Syntaxe:**  
`bool IsPresent(var expression)`

**Poznámky:**  
Inverzní funkce jmenuje IsNullOrEmpty.

**Příklad:**  
`Switch(IsPresent([directManager]),[directManager], IsPresent([skiplevelManager]),[skiplevelManager], IsPresent([director]),[director])`

----------
### <a name="item"></a>Položky

**Popis:**  
Funkce předmět vrátí jednu položku z s více hodnotami/atributem řetězce.

**Syntaxe:**  
`var Item(mvstr attribute, num index)`

- Atribut: s více hodnotami atribut
- index: indexu položky s více hodnotami řetězce.

**Poznámky:**  
Funkce předmět je užitečná společně s funkcí obsahuje vzhledem k tomu, že tato funkce vrátí index k nějaké položce v atributu s více hodnotami.

Pokud index je mimo rozsah vyvolá chybu.

**Příklad:**  
`Mid(Item([proxyAddress],Contains([proxyAddress], "SMTP:")),6)`  
Vrátí primární e-mailovou adresu.

----------
### <a name="itemornull"></a>ItemOrNull

**Popis:**  
Funkce ItemOrNull vrací jednu položku z s více hodnotami/atributem řetězce.

**Syntaxe:**  
`var ItemOrNull(mvstr attribute, num index)`

- Atribut: s více hodnotami atribut
- index: indexu položky s více hodnotami řetězce.

**Poznámky:**  
ItemOrNull funkce je užitečná společně s funkcí obsahuje, protože tato funkce vrátí index k nějaké položce v atributu s více hodnotami.

Pokud index je mimo rozsah, vrátí hodnotu Null.

----------
### <a name="join"></a>Spojení

**Popis:**  
Funkce Join přijímá řetězec s více hodnotami a vrátí hodnotu typu string jedinou hodnotu s zadaný oddělovač vložen mezi jednotlivými položkami.

**Syntaxe:**  
`str Join(mvstr attribute)`  
`str Join(mvstr attribute, str Delimiter)`

- Atribut: s více hodnotami atribut obsahující řetězců, které mají být připojen.
- oddělovač: jakýkoli řetězec slouží k oddělení dílčích řetězců vrácený řetězec. Pokud není zadáno, znak mezery ("") slouží. Je-li oddělovač řetězec nulové délky ("") nebo nic, bez oddělovače spojeny všechny položky v seznamu.

**Poznámky:**  
Mezi funkce spojení a rozdělení je dostupná. Funkce Join trvá maticových řetězce a připojené účastníky pomocí oddělovačem, aby se vrátil jeden řetězec. Funkce rozdělení přijímá řetězec a odděluje na oddělovač vrátíte maticových řetězce. Klíčové rozdíl je však spojení můžete zřetězení řetězců s jakýkoli řetězec oddělovač, rozdělení pouze samostatné řetězců pomocí jednotlivý znak oddělovače.

**Příklad:**  
`Join([proxyAddresses],",")`  
Může vrátit:"SMTP:john.doe@contoso.com,smtp:jd@contoso.com"

----------
### <a name="lcase"></a>Funkce LCase

**Popis:**  
Funkce LCase převede všechny znaky v řetězci na malá písmena.

**Syntaxe:**  
`str LCase(str value)`

**Příklad:**  
`LCase("TeSt")`  
Vrátí "test".

----------
### <a name="left"></a>Vlevo

**Popis:**  
Funkce Left vrátí zadaný počet znaků z levé části řetězec.

**Syntaxe:**  
`str Left(str string, num NumChars)`

- řetězec: řetězec, který má vrátit znaky z
- NumChars: číslo určující počet znaků vrátíte od začátku (vlevo) řetězec

**Poznámky:**  
Řetězec obsahující první numChars znaků v řetězci:

- Pokud numChars = 0, vrátí prázdný řetězec.
- Pokud numChars < 0, vrátí vstupní řetězec.
- Pokud je řetězec null, vrátí prázdný řetězec.

Pokud řetězec obsahuje méně znaků, než číslo v souladu s numChars, bude vrácena řetězec identickými řetězec (který se obsahující všechny znaky v parametru 1).

**Příklad:**  
`Left("John Doe", 3)`  
Vrátí "Joh".

----------
### <a name="len"></a>Funkce LEN

**Popis:**  
Funkce DÉLKA vrátí počet znaků v řetězci.

**Syntaxe:**  
`num Len(str value)`

**Příklad:**  
`Len("John Doe")`  
Vrátí 8

----------
### <a name="ltrim"></a>Funkce LTrim

**Popis:**  
Funkce LTrim odebere počátečních mezer v řetězci.

**Syntaxe:**  
`str LTrim(str value)`

**Příklad:**  
`LTrim(" Test ")`  
Vrátí hodnotu "Test"

----------
### <a name="mid"></a>Funkce MID

**Popis:**  
Funkce část vrátí zadaný počet znaků od zadané pozice v řetězci.

**Syntaxe:**  
`str Mid(str string, num start, num NumChars)`

- řetězec: řetězec, který má vrátit znaky z
- spuštění: číslo určující počáteční umístění v řetězci vrátit znaky z
- NumChars: číslo určující počet znaků k vrácení z umístění v řetězci

**Poznámky:**  
Vrátí numChars znaky od pozice start v řetězci.  
Řetězec obsahující znaky numChars od pozice začátku řetězce:

- Pokud numChars = 0, vrátí prázdný řetězec.
- Pokud numChars < 0, vrátí vstupní řetězec.
- Pokud start > délka řetězce, vrátí vstupní řetězec.
- Pokud start < = 0, vrátí vstupní řetězec.
- Pokud je řetězec null, vrátí prázdný řetězec.

Pokud nejsou numChar znaky zbývající v řetězci na obrazovce start pozici tolik největšímu jsou vráceny znaky.

**Příklad:**  
`Mid("John Doe", 3, 5)`  
Vrátí "kola hn".

`Mid("John Doe", 6, 999)`  
Vrátí "Karásek"

----------
### <a name="now"></a>Nyní

**Popis:**  
Funkce nyní vrátí DateTime určující aktuální datum a čas podle systémového data a času v počítači.

**Syntaxe:**  
`dt Now()`

----------
### <a name="numfromdate"></a>NumFromDate

**Popis:**  
Funkce NumFromDate vrací datum ve formátu data v AD.

**Syntaxe:**  
`num NumFromDate(dt value)`

**Příklad:**  
`NumFromDate(CDate("2012-01-01 23:00:00"))`  
Vrátí 129699324000000000

----------
### <a name="padleft"></a>PadLeft

**Popis:**  
PadLeft funkce vlevo – překladače řetězec zadané délky znaku zadaného odsazení.

**Syntaxe:**  
`str PadLeft(str string, num length, str padCharacter)`

- řetězec: řetězec na plocha.
- Délka: typu integer představující požadovaný délku řetězce.
- padCharacter: řetězec obsahující znak použití jako znak číselník

**Poznámky:**

- Pokud se délka řetězce je menší než délka, pak padCharacter opakovaně připojen k začátku (vlevo) řetězce, dokud má délku rovna délce.
- PadCharacter může být znak mezery, ale nesmí být hodnotu null.
- Pokud se délka řetězce je rovna nebo větší než délka, bude vrácena řetězec beze změny.
- Pokud má řetězec znaků větší než nebo rovno délka, je vrácena řetězec identickými řetězec.
- Pokud se délka řetězce je menší než délka, bude vrácena nový řetězec požadovaná délka obsahující řetězec doplněné s padCharacter.
- Pokud je řetězec null, vrátí funkce prázdný řetězec.

**Příklad:**  
`PadLeft("User", 10, "0")`  
Vrátí "000000User".

----------
### <a name="padright"></a>PadRight

**Popis:**  
PadRight funkce vpravo – překladače řetězec zadané délky znaku zadaného odsazení.

**Syntaxe:**  
`str PadRight(str string, num length, str padCharacter)`

- řetězec: řetězec na plocha.
- Délka: typu integer představující požadovaný délku řetězce.
- padCharacter: řetězec obsahující znak použití jako znak číselník

**Poznámky:**

- Pokud délka řetězce je menší než délka, pak padCharacter opakovaně připojen na konec (vpravo) řetězec dokud má délku rovna délce.
- padCharacter může být znak mezery, ale nesmí být hodnotu null.
- Pokud se délka řetězce je rovna nebo větší než délka, bude vrácena řetězec beze změny.
- Pokud má řetězec znaků větší než nebo rovno délka, je vrácena řetězec identickými řetězec.
- Pokud se délka řetězce je menší než délka, bude vrácena nový řetězec požadované délku obsahující řetězec doplněné s padCharacter.
- Pokud je řetězec null, vrátí funkce prázdný řetězec.

**Příklad:**  
`PadRight("User", 10, "0")`  
Vrátí "User000000".

----------
### <a name="pcase"></a>PCase

**Popis:**  
Funkce PCase převede první písmeno každého slova mezerou v řetězci na velká písmena a všechny ostatní znaky budou převedeny na malá písmena.

**Syntaxe:**  
`String PCase(string)`

**Poznámky:**

- Tato funkce momentálně neposkytuje správné velikosti písmen převést slovo, které je úplně velká, jako je zkratka.

**Příklad:**  
`PCase("TEsT")`  
Vrátí "Test".

`PCase(LCase("TEST"))`  
Vrátí hodnotu "Test"

----------
### <a name="randomnum"></a>RandomNum

**Popis:**  
Funkce RandomNum Vrátí náhodné číslo mezi určeného časového intervalu.

**Syntaxe:**  
`num RandomNum(num start, num end)`

- spuštění: číslo určující dolní mez náhodné hodnoty pro generování
- ukončení: číslo určující horní mez náhodné hodnoty pro generování

**Příklad:**  
`Random(100,999)`  
Vrátit zpět 734.

----------
### <a name="removeduplicates"></a>RemoveDuplicates

**Popis:**  
Funkce RemoveDuplicates přijímá řetězec s více hodnotami a ujistěte se, že každá hodnota je jedinečný.

**Syntaxe:**  
`mvstr RemoveDuplicates(mvstr attribute)`

**Příklad:**  
`RemoveDuplicates([proxyAddresses])`  
Vrátí upravený proxyAddress atribut, které mohly být odebrány všechny duplicitní hodnoty.

----------
### <a name="replace"></a>Nahrazení

**Popis:**  
Funkce Nahradit nahradí všechny výskyty řetězci na znaky s jiným řetězcem.

**Syntaxe:**  
`str Replace(str string, str OldValue, str NewValue)`

- řetězec: nahrazení hodnot v řetězci.
- OldValue: Řetězec, který chcete vyhledat a nahradit.
- Nová_hodnota: Řetězec, který má nahradit.

**Poznámky:**  
Funkce rozpozná následující zvláštní zástupných názvů:

- \n – nový řádek
- \r – konce řádku
- \t – karta

**Příklad:**  
`Replace([address],"\r\n",", ")`  
Nahradí Line FEED čárky a mezery a může vést k "Jedné Microsoft způsobem, Redmond, WA, USA"

----------
### <a name="replacechars"></a>ReplaceChars

**Popis:**  
Funkce ReplaceChars nahradí všechny výskyty znaků v řetězci ReplacePattern nalezena.

**Syntaxe:**  
`str ReplaceChars(str string, str ReplacePattern)`

- řetězec: nahradit znaky v řetězci.
- ReplacePattern: řetězec obsahující slovníku se znaky nahradit.

Formát je {zdroj1}: {target1}, {zdroj2}: {target2}, {zdrojN}, {targetN} Pokud je zdrojem znaku najít a cílového webu řetězec, který má nahradit.

**Poznámky:**

- Funkce trvá každý výskyt definovaný zdrojů a nahradí cíle.
- Zdroj musí být jeden znak (unicode).
- Zdroj nemůže být prázdné nebo delší než jeden znak (při analýze).
- Cíl může obsahovat více znaků, například ö:oe, β:ss.
- Cíl může být prázdný označující, že znak má odebrána.
- Zdroje se rozlišují malá a musí být přesné shody.
- (Čárka) a: (dvojtečka) jsou vyhrazené znaky a nelze nahradit pomocí této funkce.
- Mezery a dalších bílé znaků v řetězci ReplacePattern jsou ignorovány.

**Příklad:**  
`%ReplaceString% = ’:,Å:A,Ä:A,Ö:O,å:a,ä:a,ö,o`

`ReplaceChars("Räksmörgås",%ReplaceString%)`  
Vrátí Raksmorgas

`ReplaceChars("O’Neil",%ReplaceString%)`  
Vrátí hodnotu "ONeil", jednoho popisky je definován má být odebrán.

----------
### <a name="right"></a>Vpravo

**Popis:**  
Funkce ZPRAVA vrátí zadaný počet znaků zprava (end) řetězce.

**Syntaxe:**  
`str Right(str string, num NumChars)`

- řetězec: řetězec, který má vrátit znaky z
- NumChars: číslo určující počet znaků se vrátíte na konci (vpravo) řetězce

**Poznámky:**  
Na poslední pozici řetězce jsou vráceny NumChars znaky.

Řetězec obsahující poslední numChars znaků v řetězci:

- Pokud numChars = 0, vrátí prázdný řetězec.
- Pokud numChars < 0, vrátí vstupní řetězec.
- Pokud je řetězec null, vrátí prázdný řetězec.

Pokud řetězec obsahuje méně znaků, než číslo v souladu s NumChars, bude vrácena řetězec identickými řetězec.

**Příklad:**  
`Right("John Doe", 3)`  
Vrátí "Karásek".

----------
### <a name="rtrim"></a>Funkce RTrim

**Popis:**  
Funkce RTrim odebere koncových mezer v řetězci.

**Syntaxe:**  
`str RTrim(str value)`

**Příklad:**  
`RTrim(" Test ")`  
Vrátí "Test".

----------
### <a name="split"></a>Rozdělení

**Popis:**  
Funkce rozdělit řetězec oddělené pomocí oddělovače a chce řetězec s více hodnotami.

**Syntaxe:**  
`mvstr Split(str value, str delimiter)`  
`mvstr Split(str value, str delimiter, num limit)`

- hodnota: řetězec s oddělovač k oddělení.
- oddělovač: znak má být použit jako oddělovač.
- limit: maximální počet hodnot, které se můžete vrátit.

**Příklad:**  
`Split("SMTP:john.doe@contoso.com,smtp:jd@contoso.com",",")`  
Vrátí řetězec s více hodnotami s 2 prvky užitečné pro atribut proxyAddress.

----------
### <a name="stringfromguid"></a>StringFromGuid

**Popis:**  
Funkce StringFromGuid zabírá binární GUID a převede ho na řetězce

**Syntaxe:**  
`str StringFromGuid(bin GUID)`

----------
### <a name="stringfromsid"></a>StringFromSid

**Popis:**  
Funkce StringFromSid převede pole bajt obsahuje identifikátor zabezpečení řetězec.

**Syntaxe:**  
`str StringFromSid(bin ObjectSID)`  

----------
### <a name="switch"></a>Přepínač

**Popis:**  
Funkce Switch slouží k vrací jedinou hodnotu na základě vyhodnocené podmínek.

**Syntaxe:**  
`var Switch(exp expr1, var value1[, exp expr2, var value … [, exp expr, var valueN]])`

- výraz: Chcete-li zjistit výraz typu Variant.
- hodnota: hodnota, která má být vrácena, pokud odpovídající výraz vyhodnotí jako PRAVDA.

**Poznámky:**  
V seznamu argumentů funkce Přepnout je tvořeno dvojice hodnot search_column výrazy a hodnoty. Výrazy jsou vyhodnoceny zleva doprava a vrátí hodnotu přidružený k prvnímu výrazu, který jsou vyhodnoceny jako PRAVDA. Pokud části nejsou párovaný správně, dojde k chybě běhu.

Například pokud Výraz1 používá hodnota PRAVDA, vrátí přepnout hodnota1. Pokud výraz-1 je NEPRAVDA, ale výraz-2 je PRAVDA, vrátí přepínač hodnota 2 atd.

Vrátí přepnout není nic pokud:

- Žádná z výrazů jsou PRAVDA.
- První výraz True obsahuje odpovídající hodnotu Null.

Přepnout vyhodnotí všech výrazů, i když vrátí jenom jedna z nich. Z tohoto důvodu mají zjišťovat nežádoucí vedlejší účinky. Například pokud hodnocení výraz jazyka má za následek chybu dělení nulou, dojde k chybě.

Hodnota lze také chybovou funkci, která vrátí řetězec vlastní.

**Příklad:**  
`Switch([city] = "London", "English", [city] = "Rome", "Italian", [city] = "Paris", "French", True, Error("Unknown city"))`  
Vrátí jazyk používaný v některých hlavní měst, jinak vrátí chybu.

----------
### <a name="trim"></a>Funkce Trim

**Popis:**  
Funkce PROČISTIT odstraní počáteční a koncové prázdné znaky z řetězce.

**Syntaxe:**  
`str Trim(str value)`  

**Příklad:**  
`Trim(" Test ")`  
Vrátí "Test".

`Trim([proxyAddresses])`  
Odstraní počáteční a koncové mezery pro jednotlivé hodnoty v atributu proxyAddress.

----------
### <a name="ucase"></a>Funkce UCase

**Popis:**  
Funkce UCase převede všechny znaky v řetězci na velká písmena.

**Syntaxe:**  
`str UCase(str string)`

**Příklad:**  
`UCase("TeSt")`  
Vrátí "TEST".

----------
### <a name="word"></a>Aplikace Word

**Popis:**  
Word vrátí slova obsažená v řetězci na základě parametrů popisující oddělovače použití a word číslo a vrátí.

**Syntaxe:**  
`str Word(str string, num WordNumber, str delimiters)`

- řetězec: řetězec vrátíte slova.
- WordNumber: měly vrátit číslo identifikační číslo, které word.
- oddělovače: typu string představující delimiter(s) použitý k identifikaci slova

**Poznámky:**  
Každý řetězec znaků v řetězci odděleni tu znaky v oddělovače jsou označeny jako slova:

- Pokud číslo < 1, vrátí prázdný řetězec.
- Pokud je řetězec null, vrátí funkce prázdný řetězec.

Pokud řetězec obsahuje nižší než počet slov nebo řetězec neobsahuje žádné slova označenými oddělovače, vrátí se prázdný řetězec.

**Příklad:**  
`Word("The quick brown fox",3," ")`  
Vrátí hodnotu "hnědé"

`Word("This,string!has&many separators",3,",!&#")`  
Vrátí "je"

## <a name="additional-resources"></a>Další zdroje informací

* [Principy deklarativní zřizovací výrazů](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md)
* [Azure AD připojení synchronizace: Přizpůsobení možností synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
