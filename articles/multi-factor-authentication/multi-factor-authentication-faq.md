<properties
    pageTitle="Nejčastější dotazy týkající se Azure Vícefaktorové ověřování"
    description="Obsahuje seznam často kladené otázky a odpovědi týkající se Azure Vícefaktorové ověřování. Vícefaktorové ověřování je způsob, jak ověření identity uživatele, které vyžaduje více než uživatelské jméno a heslo. Poskytuje další úroveň zabezpečení a přihlašování uživatelů a transakce."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="azure-multi-factor-authentication-faq"></a>Nejčastější dotazy týkající se Azure Vícefaktorové ověřování


Tento článek zodpovídá běžné otázky o Azure Vícefaktorové ověřování a používání služba Vícefaktorové ověřování, včetně dotazy týkající se billing modelu i použitelnost.

## <a name="general"></a>Obecné

**Otázka: jak Azure Multi-Factor Authentication serveru zpracovat uživatelská data?**

S Multi-Factor Authentication serverem uživatelská data uložená jenom na serverech místní. Žádná trvalý uživatelská data uložený v cloudu. Když uživatel provede dvoustupňového ověření, Multi-Factor ověřování serveru odešle data do cloudové služby Azure Vícefaktorové ověřování pro ověřování. Komunikace mezi Multi-Factor ověřování serveru a cloudovou službu Vícefaktorové ověřování používá přes port 443 odchozí Secure (Sockets Layer SSL) nebo zabezpečení TLS (Transport Layer).

Po ověření se odešle žádost o cloudové služby, údaje pro ověřování a použití sestavy. Datová pole součástí dvoustupňového ověření protokoly jsou následujícím způsobem:

- **Jedinečné ID** (buď jméno nebo místní Multi-Factor ověřování serveru ID uživatele)
- **Jméno a příjmení** (volitelné)
- **E-mailovou adresu** (volitelné)
- **Telefonní číslo** (při použití hlasový hovor nebo ověřování služby SMS)
- **Token zařízení** (při používání mobilní aplikace authentication)
- **Režim ověřování**
- **Výsledek ověřování**
- **Název serveru Vícefaktorové ověřování**
- **Vícefaktorové ověřování serveru IP**
- **Klienta IP** (Pokud je k dispozici)

Volitelná pole je možné konfigurovat na multi-Factor ověřování serveru.

Ověření výsledek (úspěch nebo odmítnutí) důvod, proč Pokud byl odepřen, je uložena s daty ověřování a je k dispozici v ověřování a použití sestavy.


## <a name="billing"></a>Fakturace

Podle pokynů na [stránce Multi-Factor Authentication ceny](https://azure.microsoft.com/pricing/details/multi-factor-authentication/)můžete zodpovězené většina fakturační otázky.

**Otázka: organizaci účtovaná za telefonní hovory nebo textové zprávy slouží k ověření svoje uživatele?**

Organizace nejsou účtovaná za jednotlivé telefonní hovory umístí nebo textové zprávy poslané na uživatelů prostřednictvím Azure Vícefaktorové ověřování. Vlastníci telefonní může platit pro telefonní hovory nebo textové zprávy, kterou dostanou, podle své osobní telefonní služby.

**Otázka: slouží modelu poplatků fakturační jednotlivé uživatele na základě počet uživatelů, kteří jsou nakonfigurovaný na používání Vícefaktorové ověřování nebo počet uživatelů, kteří provádění ověřování?**

Fakturace se podle počtu uživatelů nakonfigurovaný na používání Vícefaktorové ověřování.

**Otázka: Jak funguje fakturace Vícefaktorové ověřování?**

Při použití "za uživatele" nebo "za ověření" model, Azure MFA je zdroje založené na spotřebu. Všechny poplatky je faktura k předplatnému uživatele v organizaci Azure stejně jako virtuálních počítačích, weby, atd.

Při použití modelu licencí Azure Vícefaktorové ověřování licence koupili a pak se přiřadí uživatelům, stejně jako u Office 365 a další produkty předplatného.

**Otázka: existuje bezplatné verzi Azure Vícefaktorové ověřování pro správce?**

V některých případech Ano. Vícefaktorové ověřování pro správce Azure nabízí podmnožinu Azure MFA funkcí bez nákladů. Tuto nabídku platí pro členy skupiny Azure globální správci v instancí služby Azure Active Directory, které nebyly propojené poskytovateli Azure Vícefaktorové ověřování na základě spotřebu. Pomocí zprostředkovatele Vícefaktorové ověřování aktualizuje všechny správce a uživatele v adresáři, kteří jsou nakonfigurovaný na používání Vícefaktorové ověřování na plnou verzi Azure Vícefaktorové ověřování.

**Otázka: existuje bezplatné verzi Azure Vícefaktorové ověřování pro uživatele Office 365?**

V některých případech Ano. Vícefaktorové ověřování pro Office 365 nabízí podmnožinu Azure MFA funkcí bez nákladů. Tuto nabídku platí pro uživatele, kteří mají licenci pro Office 365 přiřazené, když poskytovatel Azure Vícefaktorové ověřování na základě spotřebu nebyla propojených odpovídající instanci služby Azure Active Directory. Pomocí zprostředkovatele Vícefaktorové ověřování aktualizuje všechny správce a uživatele v adresáři, kteří jsou nakonfigurovaný na používání Vícefaktorové ověřování plnou verzi Azure Vícefaktorové ověřování.

**Otázka: můžou organizaci přepínání mezi jednotlivé uživatele a za ověření spotřebu fakturační modely kdykoli?**

Vaše organizace zvolí fakturační modelu při vytváření zdroje. Není možné změnit fakturační modelu po zřízení zdroje. Vytvořením však jinému zdroji Vícefaktorové ověřování nahrazení původní. Nastavení a možnosti konfigurace nelze převést na nový zdroj.

**Otázka: můžou organizaci přepínání mezi spotřebu fakturace a modelu licencí kdykoli?**

Po přidání licencí na adresář, který už je zprostředkovatele Azure Vícefaktorové ověřování uživatelů na základě spotřebu fakturace je snížena o počet licencí vlastní. Pokud mají všichni uživatelé nakonfigurovaný na používání Vícefaktorové ověřování přiřazené licence, správce odstranit poskytovatele Azure Vícefaktorové ověřování.

Za ověření spotřebu fakturace s modelem licenci se nedají kombinovat. Propojeného zprostředkovatele Vícefaktorové ověřování za ověření do adresáře organizace fakturované pro všechny požadavky ověřování Vícefaktorové ověřování, bez ohledu na žádné licence vlastnictví.

**Otázka: organizaci má použít, a synchronizovat identit používat Azure Vícefaktorové ověřování?**

Organizace používá na základě spotřebu fakturační model, Azure Active Directory při není potřeba. Propojení zprostředkovatele Vícefaktorové ověřování adresář je nepovinný krok. Pokud vaše organizace není propojena s v adresáři, můžete nasadit Azure Multi-Factor ověřování serveru nebo Azure Multi-Factor Authentication SDK místní.

Azure Active Directory je nutný pro modelu licencí, protože přidat licence do adresáře při nákupu a jejich přiřazení k uživatelů v adresáři.


## <a name="usability"></a>Zvýšení použitelnosti

**Otázka: co uživatele? Pokud nemáte dostanete odpověď na telefon nebo telefon není k dispozici pro uživatele**

Pokud uživatel nakonfiguroval náhradní telefonní, jejich opakujte a vyberte tento telefon po zobrazení výzvy na přihlašovací stránce. Pokud uživatel nemá ještě jeden způsob nakonfigurovali, může správce organizace aktualizaci přiřazená uživatele primární telefonní číslo.


**Otázka: co správce dělat, když uživatel kontaktuje správce o účtu, který už má uživatel přístup?**

Správce obnovit uživatelský účet požádá, aby znovu projít procesu registrace. Další informace o [správě uživatelů a nastavení zařízení s Azure Vícefaktorové ověřování v cloudu](multi-factor-authentication-manage-users-and-devices.md).

**Otázka: co správce dělá případě ztráty nebo krádeže Telefon uživatele, který používá aplikaci hesla?**

Správce můžete odstranit všechny uživatele aplikace hesla abyste zabránili neoprávněnému přístupu. Náhradní zařízení po uživatel uživatel obnovit hesla. Další informace o [správě uživatelů a nastavení zařízení s Azure Vícefaktorové ověřování v cloudu](multi-factor-authentication-manage-users-and-devices.md).

**Otázka: jak postupovat, pokud uživatel nemůže Přihlaste se k prohlížení aplikace?**

Uživatel, který je nakonfigurovaný na používání Vícefaktorové ověřování vyžaduje aplikaci heslo pro přihlášení k některé aplikace jiné prohlížeče. Uživatel musí zrušte (odstranit) přihlašovací údaje, restartujte aplikaci a přihlaste se pomocí své uživatelské jméno a aplikace heslo.

Získáte další informace o vytváření aplikací hesla a další [Nápověda k aplikaci hesla](multi-factor-authentication-end-user-app-passwords.md).


>[AZURE.NOTE] Moderní ověřování pro klienty Office 2013
>
> Klienti Office 2013 (včetně aplikace Outlook) podporují nové ověřování protokoly. Můžete nakonfigurovat Office 2013 podporuje Vícefaktorové ověřování. Po konfiguraci Office 2013, aplikace hesla nejsou nutné pro klienty Office 2013. Další informace najdete v tématu [Office 2013 moderní ověřování veřejné náhled oznámení](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).

**Otázka: co uživatele? Pokud uživatel neobdrží textovou zprávu, nebo pokud uživatel odpovídá mezi dvěma stranami textové zprávy, ale ověření vyprší její časový limit**

Poskytovat z textu zprávy a přijetí odpovědi v mezi dvěma stranami SMS není možné zaručit, protože neexistují neovladatelném faktory, které by mohly ovlivnit spolehlivost služby. Tyto faktory patří země určení, carrier mobilního telefonu a: Zkontrolujte sílu signálu.

Uživatelé, kteří dojít nedaří problémy se spolehlivým přijímat textové zprávy by měly místo toho vyberte metodu mobilní aplikace nebo telefonního hovoru. Mobilní aplikaci můžete zasílat oznámení ve formě jak přes Wi-Fi připojení a mobilní síť. Mobilní aplikace kromě toho můžete generovat ověření kódy i v případě, že zařízení signalizuje žádné vůbec. Aplikace Microsoft Authenticator je k dispozici pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

Pokud je nutné použít textové zprávy, doporučujeme používat jednosměrné SMS spíše než mezi dvěma stranami SMS, pokud je to možné. Uživatelé z nabíhání globální poplatků za služby SMS v odpovědi textovou zprávu, která byla odeslána z jiné zemi jednosměrné SMS je spolehlivější.


**D: Mohu použít hardwarové tokeny s Azure Multi-Factor ověřování serveru?**

Pokud používáte Azure Multi-Factor Authentication Server, můžete importovat tokeny založená na čase, jednorázové heslo (TOTP) otevřít ověřování (MÍSTOPŘÍSEŽNÉM) třetích stran a používat je k dvoustupňového ověření.

Můžete použít ActiveIdentity klíčovými MÍSTOPŘÍSEŽNÉM TOTP tokeny Pokud tajné klíčové soubor umístit do souboru CSV a importovat Azure Multi-Factor Authentication Server. Tokeny MÍSTOPŘÍSEŽNÉM můžete použít s Active Directory Federation služby (služby AD FS), vzdálené vytáčených uživatele služby (RADIUS Authentication) při klientském počítači můžete zpracování odpovědí ověřovací kód programu přístup a ověřování pomocí formulářů informace o serveru Internetové informační.

Můžete importovat tokeny MÍSTOPŘÍSEŽNÉM TOTP třetí část s následujícími formáty:  
- Kontejner Portable symetrickou klíčů (PSKC)  
- Pokud soubor obsahuje pořadové číslo tajné klíč ve formátu Base 32 a časový interval souboru CSV  

**D: Mohu použít Server Azure Multi-Factor Authentication zajistit Terminálová?**

Ano, ale pokud používáte Windows serveru 2012 R2 nebo novější, pouze pomocí vzdálené plochy brány (RD).

Změny zabezpečení v systému Windows Server 2012 R2 se změnila tak, aby Azure Multi-Factor ověřování serveru připojuje k balíček zabezpečení místního úřadu zabezpečení (LSA) v systému Windows Server 2012 a dřívějších verzích. Verze Terminálové služby v systému Windows Server 2012 nebo starším můžete [zabezpečení aplikace pomocí ověřování Windows](multi-factor-authentication-get-started-server-windows.md#to-secure-an-application-with-windows-authentication-use-the-following-procedure). Pokud používáte Windows serveru 2012 R2, musíte RD brány.

**Otázka: Proč by uživatel přijímat hovory Vícefaktorové ověřování z anonymní volající po nastavení ID volajícího?**

Při volání Vícefaktorové ověřování jsou umístěná prostřednictvím veřejné telefonní síti, někdy jsou směrovány dopravci, který nepodporuje ID volajícího. Z toho důvodu ID volajícího není možné zaručit, i když je systémové Vícefaktorové ověřování vždy odešle.


## <a name="errors"></a>Chyby

**Otázka: co uživatelé dělat, když se zobrazí chybová zpráva "požadavek ověření není pro účet aktivované" při používání mobilní aplikace oznámení?**

Informujte k provedení těchto kroků odebrat svůj účet v mobilní aplikaci a pak ho zase přidejte:

1. Přejděte na [Azure portálu profil](https://account.activedirectory.windowsazure.com/profile/) a přihlaste se pomocí účtu organizace.
2. Vyberte **Další zabezpečení ověření**.
4. Odebrání existujícího účtu v mobilní aplikaci.
5. Klikněte na **Konfigurovat**a pak postupujte podle pokynů a překonfigurovat mobilní aplikaci.


**Otázka: co uživatelé dělat, když vidí 0x800434D4L chybové zprávy při přihlašování k aplikaci není prohlížeče?**

V současné době uživatele lze použít ověřování větší zabezpečení pouze aplikací a služeb, které má uživatel přístup prostřednictvím prohlížeče. Aplikace jiných prohlížeče (označuje také jako *klienta RTF aplikace*), které jsou instalovány v místním počítači, například prostředí Windows PowerShell nefunguje s účty, které vyžadují ověřování větší zabezpečení. V tomto případě uživatel může zobrazit aplikace chybovou zprávu 0x800434D4L.

Tento problém se mít zvláštní uživatelské účty pro správu související a jiných správců operací. Poštovní schránky mezi svého účtu správce a účtu – správce později, můžete propojit tak, aby Přihlaste se k aplikaci Outlook pomocí účtu správce. Podrobné informace o tomto zjistěte, jak [Správci umožnit otevřít a zobrazit obsah poštovní schránky uživatele](http://help.outlook.com/141/gg709759.aspx?sl=1).

## <a name="next-steps"></a>Další kroky

Pokud tady byste nenašli odpověď svou otázku, ponechte v komentářích v dolní části stránky. A tady jsou některé další možnosti pro získání nápovědy:


**Otázka: Jak můžu získat nápovědu ze služby Azure Vícefaktorové ověřování?**

- Prohledat [znalostní bázi Knowledge Base podpory společnosti Microsoft](https://www.microsoft.com/en-us/Search/result.aspx?form=mssupport&q=phonefactor&form=mssupport) pro řešení běžných problémů technické.

- Hledat a vyhledejte technické otázky a odpovědi od komunity nebo vlastní zeptejte se na [fórech Azure Active Directory](https://social.msdn.microsoft.com/Forums/azure/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

- Pokud jste zákazníkem starší verze PhoneFactor a máte nějaké dotazy nebo potřebujete pomoc s resetování hesla, otevřete případ podpory pomocí odkazu na [resetování hesla](mailto:phonefactorsupport@microsoft.com) .

- Obraťte se na odborné podporou [Azure Multi-Factor ověřování serveru (PhoneFactor)](https://support.microsoft.com/oas/default.aspx?prid=14947). Jak nás kontaktovat, je užitečné, pokud zahrnete tolik informace o problém co nejdřív. Informace, které lze zadat obsahuje stránky, kde jste viděli chybu, kód chyby, ID konkrétní relace a ID uživatele, kterému viděli chyby do schránky.
