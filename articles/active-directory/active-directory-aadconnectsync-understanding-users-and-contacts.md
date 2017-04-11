<properties
    pageTitle="Azure AD Connect synchronizace: Principy uživatelů a kontaktních | Microsoft Azure"
    description="Tento článek vysvětluje uživatelů a kontakty v Azure AD Connect synchronizovat."
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
    ms.date="10/10/2016"
    ms.author="markusvi;andkjell"/>


# <a name="azure-ad-connect-sync-understanding-users-and-contacts"></a>Azure AD Connect synchronizace: Principy uživatelů a kontakty

Existuje několik různých důvodů, proč byste měli více struktur služby Active Directory a existuje několik různých nasazení topologií. Běžné modely zahrnout nasazení účtu zdroje a doménových sync'ed GAL po spojení a získání. Ale i v případě, že jsou jen modelů, hybridní modely běžné stejně. Výchozí konfigurace v Azure AD Connect synchronizovat není předpokládá konkrétní model, ale podle toho, jak uživatel odpovídající byla vybrána v Průvodci instalací, můžete pozorovanými různých chování.

V tomto tématu projdeme chování výchozí konfigurace v některých topologií. Provedeme konfiguraci a editoru pravidla synchronizace mohou sloužit k podívejte se na konfiguraci.

Existuje několik obecné pravidla, která předpokládá konfigurace:

- Bez ohledu na jakém pořadí jsme import ze zdroje aktivní adresáře v konečném důsledku buďte vždy stejné.
- Aktivní účet vždy přispívali přihlašovací údaje, včetně **userPrincipalName** a **sourceAnchor**.
- Zakázaný účet bude přispívat userPrincipalName a sourceAnchor, že se nejedná propojené poštovní schránky, pokud není aktivní účet nalezen.
- Pro userPrincipalName a sourceAnchor se použijí nikdy účet s propojené poštovní schránky. Předpokládá se, že účet active najdete později.
- Objekt kontaktu může zřízení Azure AD jako kontakt nebo jako uživatel. Nevíte, skutečně dokud zpracovali struktur služby Active Directory všechny zdroje.

## <a name="contacts"></a>Kontakty

S kontakty, které představuje uživatele v jiné struktuře je běžné po spojení a získání kde je řešení GALSync přemostění dva nebo více strukturami Exchange. Objekt kontaktu je vždy spojující z prostor konektoru metaverse pomocí atribut pošta. Pokud už je objekt kontaktu nebo uživatele se stejnou adresou pošty, objekty jsou spojeny. Toto je nakonfigurováno v pravidlo **v z AD – připojit se ke kontaktu**. Je taky pravidlo s názvem **v z AD – kontakt běžné** atribut toku k metaverse atribut **sourceObjectType** s konstantou **kontaktu**. Toto pravidlo má příliš nízkou prioritou Pokud uživatelských objektů je připojen k stejný metaverse objekt a pak pravidla **v z AD – uživatele běžné** budou přispívat hodnotu uživatele do tohoto atributu. Při použití tohoto pravidla Tenhle atribut Pokud bude mít hodnoty kontakt Pokud připojil žádný uživatel a uživatel najít alespoň jeden uživatele.

Zřízení objektu Azure AD odchozího pravidla **na AAD – kontaktujte připojení** vytvoří objekt kontaktu když atribut metaverse **sourceObjectType** nastavené **kontaktovat**. Pokud Tenhle atribut je nastavený na **uživatele**, pak pravidlo **na AAD – připojit uživatel** vytvoří objektu uživatele místo.
Je možné, že objekt je propagované z kontaktu uživateli při importu a synchronizaci adresáře Active další zdroje.

Například v topologii GALSync jsme najdete objekty kontaktů pro všechny uživatele v druhé doménové jsme importu první doménové. To bude fáze nových kontaktů objektů AAD spojnice. Když budeme dál importovat a synchronizovat druhé doménové, jsme vyhledá reálných uživatelů a připojení k existující metaverse objekty. Potom Změníme odstranění kontaktů objektu v AAD a vytvořte objekt nového uživatele.

Pokud máte topologii kde uživatelů a tvaru kontakty, ujistěte se, vyberte podle uživatelů na atributu pošta v Průvodci instalací. Pokud vyberete jinou možnost, bude mít závislá konfigurace pořadí. Objekty kontaktů bude vždy na připojit ke atributů hromadné, ale objektů uživatele pouze připojí na atributu hromadné Pokud tato možnost je vybraný v Průvodci instalací. Můžete může potom nakonec znamenat dva různé objekty v metaverse se stejným atributem hromadné Pokud objekt kontaktu importovaného před objektu uživatele. Při exportu Azure AD se vyvolá chybu. Toto chování se o záměr a naznačují chybná data nebo aby nebyla topologii správně identifikovány v průběhu instalace.

## <a name="disabled-accounts"></a>Vypnutí účtů

Vypnutí účtů jsou synchronizovány i Azure AD. Vypnutí účtů jsou běžné představující zdrojů v Exchange, například konferenční místnosti. Výjimkou jsou uživatelé, kteří mají propojené poštovní schránky; jako výše uvedené tyto se nikdy zřízení účtu Azure AD.

Za předpokladu je, že pokud se nachází zakázaný uživatelský účet, a pak jsme nenajde jiný aktivní účet později a objekt zřízení Azure AD pomocí userPrincipalName a sourceAnchor najít. V případě, že jiný aktivní účet se připojit ke stejnému metaverse objektu, bude použit jeho userPrincipalName a sourceAnchor.

## <a name="changing-sourceanchor"></a>Změna sourceAnchor

Když objektu vyexportování Azure AD a není možné změnit sourceAnchor už. Když vyexportování objektu atributu metaverse **cloudSourceAnchor** nastavenou hodnotou **sourceAnchor** přijal Azure AD. Pokud **sourceAnchor** dojde ke změně, neshodují **cloudSourceAnchor**pravidlo **na AAD – uživatel připojit** vyvolá chyba **sourceAnchor atribut změnil**. V tomto případě konfiguraci nebo data musí být opravit tak, aby stejné sourceAnchor účastní metaverse znovu před objekt lze synchronizovat znovu.

## <a name="additional-resources"></a>Další zdroje informací

* [Azure AD připojení synchronizace: Přizpůsobení možností synchronizace](active-directory-aadconnectsync-whatis.md)
* [Integrace místních identit s Azure Active Directory](active-directory-aadconnect.md)
