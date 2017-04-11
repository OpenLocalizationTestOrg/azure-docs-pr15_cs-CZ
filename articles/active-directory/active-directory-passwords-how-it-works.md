<properties
    pageTitle="Jak to funguje: Azure AD heslo Management | Microsoft Azure"
    description="Další informace o různých součásti Azure AD heslo správy, včetně kde uživatele zaregistrovat, obnovit a změnit svoje hesla a pokud správce konfigurace, hlášení o a povolit správu z místní služby Active Directory hesla."
    services="active-directory"
    documentationCenter=""
    authors="asteen"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/12/2016"
    ms.author="asteen"/>

# <a name="how-password-management-works"></a>Jak funguje správa hesel

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

Správa hesel v Azure Active Directory se skládá ze několik logické součástí, které jsou popsané níže.  Klikněte na každý odkaz zobrazíte další informace o této součásti.

- [**Konfigurace portálu Správa hesla**](#password-management-configuration-portal) – můžete správcem různých aspektů správě hesla v jejich klienti tak, že přejdete na kartu konfigurovat jejich adresář na [Portálu Správa Azure](https://manage.windowsazure.com).
- [**Portál registrace uživatele**](#user-registration-portal) – mohou uživatelé registrovat pro tento web portálu resetování hesla.
- [**Portál resetování hesla uživatele**](#user-password-reset-portal) – uživatelé můžou resetovat vlastní hesla pomocí několika různých výzvy v souladu se zásadami resetovat heslo správce řídí
- [**Portál změnit heslo uživatele**](#user-password-change-portal) – uživatelů můžete kdykoli změnit svoje vlastní hesla, zadávání svoje staré heslo a výběrem nové heslo portálu tento web
- [**Sestavy správy heslo**](#password-management-reports) – správci můžete zobrazit a analyzovat heslo resetovat a registrace aktivitu na jejich klientovi tak, že přejdete do oddílu "Sestavy aktivit" Záložka "Sestavy" jejich adresáře na [Portálu Správa Azure](https://manage.windowsazure.com)
- [**Heslo zpětným součástí Azure AD Connect**](#password-writeback-component-of-azure-ad-connect) – správci můžete volitelně povolit funkci heslo zpětným při instalaci Azure AD Connect povolit správu federované nebo heslo synchronizaci uživatelských hesel z cloudu.

## <a name="password-management-configuration-portal"></a>Konfigurace portálu Správa hesla
Můžete nakonfigurovat zásad správy heslo pro určitých adresáři pomocí [Portálu Správa Azure](https://manage.windowsazure.com) tak, že přejdete do oddílu **Zásad resetování hesla uživatele** v adresáři **Konfigurovat** kartu.  Na této stránce konfigurace ovládáte mnoho aspektů způsob správy hesel ve vaší organizaci, včetně:

- Povolení nebo zakázání resetování hesla pro všechny uživatele v adresáři
- Nastavení čísla výzvy (jednu nebo dvě) uživatel musí absolvovat resetovat své heslo
- Nastavení konkrétní typy problémy, které chcete povolit uživatelům ve vaší organizaci z následujících možností:
 - Mobilní telefon (ověřovacího kódu přes text nebo hlasového hovoru)
 - Telefon do kanceláře (hlasového hovoru)
 - Alternativní e-mailu (ověřovacího kódu prostřednictvím e-mailu)
 - Dotazy zabezpečení (knowledge ověřování)
- Nastavení řadu otázek uživatele musíte zaregistrovat k použití otázky metodu ověření zabezpečení (viditelné jenom Pokud jsou povolené zabezpečení otázky)
- Nastavení řadu otázek uživatele zadejte během obnovit pomocí metody ověřování dotazy zabezpečení (viditelné jenom Pokud jsou povolené zabezpečení otázky)
- Použití předem konzervách, lokalizované, bezpečnostní otázky, které uživatel může rozhodnete sdělit nám při registraci heslo resetovat (viditelné jenom Pokud jsou povolené zabezpečení otázky)
- Definování vlastních zabezpečení otázky, které uživatel může rozhodnete sdělit nám při registraci heslo resetovat (viditelné jenom Pokud jsou povolené zabezpečení otázky)
- Uživatelích zaregistrovat k resetování když půjde do aplikace Access panely na [http://myapps.microsoft.com](http://myapps.microsoft.com)hesla.
- Požadování uživatelům potvrzení dříve registrované data po konfigurovatelné spočítá počet dnů uplyne (viditelné jenom Pokud je povolena vynucenou registrace)
- Poskytování vlastní technickou podporu e-mailu nebo adresu URL, která se zobrazí všem uživatelům v případě, že narazí na problém resetování hesla
- Povolení nebo zakázání funkci zpětným heslo (Pokud heslo zpětným má byly nasazeny pomocí AAD připojení)
- Zobrazení stavu agenta zpětným heslo (Pokud heslo zpětným má byly nasazeny pomocí AAD připojení)
- Povolení e-mailová oznámení pro uživatele při jejich resetování hesla (najdete v části **oznámení** na [Portálu Správa Azure](https://manage.windowsazure.com))
- Povolení e-mailová oznámení správcům při dalších správců resetovat své vlastní heslo (najdete v části **oznámení** na [Portálu Správa Azure](https://manage.windowsazure.com)
- Branding uživatelská hesla resetovat portálu a resetování hesla e-mailů s logem vaší organizace a název pomocí klienta branding funkce vlastního nastavení (najdete v části **Vlastnosti adresáře** na [Portálu Správa Azure](https://manage.windowsazure.com)

Další informace o konfiguraci Správa hesel ve vaší organizaci najdete v tématu [Začínáme: Správa hesel Azure AD](active-directory-passwords-getting-started.md).

##<a name="user-registration-portal"></a>Portál registrace uživatele
Před uživatelé budou moct používat resetování hesla, musí se jejich uživatelských účtů cloudu nastavit správné ověření dat, aby mohli projít požadovaný počet problémy při obnovení hesla definované správcem.  Správci můžete definovat ověřovací informace na své uživatele pomocí tlačítka Azure nebo Office web portály, DirSync / Azure AD Connect nebo prostředí Windows PowerShell.

Ale pokud se dají přednost uživatelé zaregistrovat svá data, taky nabízíme webovou stránku, která mohou uživatelé k zadejte tyto informace.  Tato stránka uživatelům umožňuje zadat ověřovací informace v souladu s heslo resetovat zásad, které máte ve své organizaci povolená.  Po ověření dat je uložená ve své cloudu uživatelského účtu se dá použít pro obnovení účtu později. To, co registrace portálu vypadá:

  ![][001]

Další informace najdete v tématu [Začínáme: Správa hesel Azure AD](active-directory-passwords-getting-started.md) a [Doporučené postupy: Správa hesel Azure AD](active-directory-passwords-best-practices.md).

##<a name="user-password-reset-portal"></a>Portál resetování hesla uživatele
Jakmile jste zapnuli automatický přístup samoobslužné resetování nastavení zásad resetování hesla samoobslužné vaší organizace a zajistit, aby vaši uživatelé mají příslušná data kontaktů v adresáři, uživatelé ve vaší organizaci budou moct resetování hesla automaticky z webové stránky, které používá pracovní nebo školní účet pro přihlašovací údaje (třeba [portal.microsoftonline.com](https://portal.microsoftonline.com)). Na stránkách jako jsou například uživatelům se zobrazí **nemáte přístup ke svému účtu?** odkaz.

  ![][002]

Po kliknutí na tento odkaz spustí portálu samoobslužné heslo resetovat.

  ![][003]

Další informace o tom, jak můžou uživatelé vlastní hesla resetovat najdete v tématu [Začínáme: Správa hesel Azure AD](active-directory-passwords-getting-started.md).

##<a name="user-password-change-portal"></a>Portál změnit heslo uživatele
Podle potřeby uživatelům měnit své vlastní hesla přehrávají tak tak, že na portálu změnit heslo kdykoli.  Uživatelé přístup k portálu změnit heslo prostřednictvím stránky profilu Panel přístup nebo kliknutím na odkaz "změnit heslo" z aplikací Office 365.  V případě po vypršení platnosti hesla, uživatelé budou také požádán, aby nezměníte automaticky při přihlašování.

  ![][004]

V obou případech povolil zpětným heslo a buď Federovaný uživatel nebo by synchronizaci hesel, tyto změny hesla se zpět došlo k zápisu na adresářová služba Active Directory. Tady je co vypadá portálu změnit heslo jako:

  ![][005]

Další informace o tom, jak uživatelé mohou měnit vlastní místní služby Active Directory hesla, najdete v článku [Začínáme: Správa hesel Azure AD](active-directory-passwords-getting-started.md).

##<a name="password-management-reports"></a>Sestavy správy hesel
Tak, že přejdete na kartu **sestavy** vyhledávání ve skupinovém rámečku **Protokoly aktivity** , zobrazí se dvě sestavy Správa hesel: **aktivity resetování hesla** a **Resetování hesel registrace aktivity**.  Pomocí těchto dvou sestav, můžete získat zobrazení uživatelů registraci a práce s nimi heslo resetovat ve vaší organizaci. Tady je tyto sestavy prohlédnout, jak na [Portálu Správa Azure](https://manage.windowsazure.com):

  ![][006]

Další informace najdete v tématu [získat další informace: sestavy správy Azure AD heslo](active-directory-passwords-get-insights.md).

##<a name="password-writeback-component-of-azure-ad-connect"></a>Heslo zpětným součástí Azure AD Connect
Pokud heslo uživatele ve vaší organizaci pocházejí z místního prostředí (buď přes federace nebo synchronizace hesel), můžete nainstalovat nejnovější verzi Azure AD Connect povolit aktualizace hesla přímo z cloudu.  To znamená, že když uživatelé v žádném případě nezapomeňte nebo chcete změnit své heslo AD, můžou dělat tak přímo z webu.  Tady se dozvíte, kde najdete zpětným heslo v Průvodci instalací Azure AD Connect:

  ![][007]

Další informace o Azure AD Connect najdete v tématu [Začínáme: Azure AD Connect](active-directory-aadconnect.md). Další informace o heslo zpětným najdete v tématu [Začínáme: Správa hesel Azure AD](active-directory-passwords-getting-started.md).


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Odkazy na heslo resetovat si přečtěte následující dokumentaci
Tady jsou odkazy na všech stránkách resetování hesla Azure AD si přečtěte následující dokumentaci:

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
* [**Začínáme**](active-directory-passwords-getting-started.md) – zjistěte, jak vám umožní uživatelům obnovit a změnit jejich cloudu a místní hesla
* [**Vlastní**](active-directory-passwords-customize.md) – zjistěte, jak přizpůsobit vzhled a chování a nastavení služby potřebám vaší organizace
* [**Doporučené postupy**](active-directory-passwords-best-practices.md) : Naučte se nasadit rychlé a efektivní práce s hesly ve vaší organizaci
* [**Dozvědět se víc**](active-directory-passwords-get-insights.md) – Další informace o naše integrované možnosti vytváření sestav
* [**Časté otázky**](active-directory-passwords-faq.md) – odpovědi na časté otázky
* [**Poradce při potížích**](active-directory-passwords-troubleshoot.md) – zjistěte, jak rychle vyřešit problémy se službou
* [**Další informace**](active-directory-passwords-learn-more.md) – přejděte důkladné do technické podrobnosti Princip služby



[001]: ./media/active-directory-passwords-how-it-works/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-how-it-works/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-how-it-works/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-how-it-works/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-how-it-works/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-how-it-works/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-how-it-works/007.jpg "Image_007.jpg"
