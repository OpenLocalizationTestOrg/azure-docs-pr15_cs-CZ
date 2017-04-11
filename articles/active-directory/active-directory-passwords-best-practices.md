<properties
    pageTitle="Doporučené postupy: Azure AD heslo správy | Microsoft Azure"
    description="Doporučené postupy pro nasazení a použití, si přečtěte následující dokumentaci ukázkové koncových uživatelů a školení příručky pro správu hesel v Azure Active Directory."
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

# <a name="deploying-password-management-and-training-users-to-use-it"></a>Nasazení Správa hesel a školení uživatelé používat

> [AZURE.IMPORTANT] **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).

Po povolení pro vytvoření nového hesla, je dalším krokem, který budete muset udělat pomohou uživatelům pomocí služby ve vaší organizaci. K tomuto účelu musíte se přesvědčte se, zda uživatelé nakonfigurované používání služby správně, tak, že uživatelé mají školení budou muset být úspěšný při správě vlastní hesla. Tento článek vysvětluje, vám následující pojmy:

* [**Jak získat uživatelé nakonfigurován pro správu hesel**](#how-to-get-users-configured-for-password-reset)
  * [Co je účet nakonfigurovaný pro resetování hesla](#what-makes-an-account-configured)
  * [Způsoby, jak můžete k naplnění sami ověřování dat](#ways-to-populate-authentication-data)
* [**Nejlepší způsoby, jak přejít na heslo resetovat pro vaši organizaci**](#what-is-the-best-way-to-roll-out-password-reset-for-users)
  * [E mailové komunikace + ukázkové e-mailové komunikace](#email-based-rollout)
  * [Vytvoření vlastního portálu Správa vlastního hesla pro uživatele](#creating-your-own-password-portal)
  * [Jak používat vynucenou registrace k vynucení uživatelů k registraci na přihlásit](#using-enforced-registration)
  * [Jak nahrát ověření dat u uživatelských účtů](#uploading-data-yourself)
* [**Ukázkového uživatele a školicí materiály podpory (už brzo!)**](#sample-training-materials)

## <a name="how-to-get-users-configured-for-password-reset"></a>Jak získat uživatelé nakonfigurován pro resetování hesla
Tato část popisuje různé metody, které lze zajistit, že každý uživatel ve vaší organizaci můžete použít samoobslužné resetování efektivně v případě, že budou v žádném případě nezapomeňte své heslo hesla pro vás.

### <a name="what-makes-an-account-configured"></a>Co je účet nakonfigurovaný
Uživatel mohli použít resetování hesla, **všechny** tyto podmínky musí být splněná:

1.  V adresáři musí být povolený resetování hesla.  Zjistěte, jak povolit tak, že čtení [umožňují uživatelům jejich Azure AD hesel](active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords) nebo [umožňují uživatelům resetování nebo změna hesla AD](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) resetování hesla
2.  Uživatel musí mít licenci.
 - Pro uživatele cloudu uživatel musí mít **placené licence Office 365**, nebo **AAD základní** nebo přiřazenou **licenci AAD Premium** .
 - Pro místní uživatele (federované nebo hash synchronizovali), uživatel **musí mít licenci pro AAD Premium přiřazené**.
3.  Uživatel musí mít **minimální nastavení ověření dat definovaný** v souladu s aktuální zásad resetování hesla.
 - Ověření dat je považován za definované obsahuje-li odpovídající pole v adresáři ve správném formátu data.
 - Minimální nastavení ověření dat je definován na **aspoň jeden** povolené ověřování možnosti při konfiguraci zásad jednu bránu, nebo na **nejméně dvě** možnosti povolené ověřování Pokud zásadu dvou bránu nakonfigurovaný.
4.  Pokud uživatel používá místní účet, potom [Heslo zpětným](active-directory-passwords-getting-started.md#enable-users-to-reset-or-change-their-ad-passwords) musí být povolený a zapnutý

### <a name="ways-to-populate-authentication-data"></a>Způsoby, jak naplnění ověřování dat
Máte několik možností o tom, jak určit data pro uživatele ve vaší organizaci má být použita pro resetování hesla.

- Úpravy uživatelů v [Portálu pro správu Azure](https://manage.windowsazure.com) nebo [Portálu pro správu Office 365](https://portal.microsoftonline.com)
- Použití Azure AD Sync k synchronizaci vlastností uživatele do Azure AD z místní domény Active Directory
- Použijte Windows PowerShell pro úpravy vlastností uživatele pomocí [kroků uvedených v tomto poli](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).
- Povolit uživatelům vedení k portálu registraci na [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) zaregistrovat svá data
- Vyžadovat, aby uživatelé zaregistrujte heslo resetovat, pokud se přihlásit svým účtem Azure AD pomocí nastavení [**vyžadují uživatelům zaregistrovat se při přihlašování?**](active-directory-passwords-customize.md#require-users-to-register-when-signing-in) konfigurace možnost **Ano**.

Uživatelé nemusí zaregistrujte se k resetování hesla pro systému do práce.  Pokud máte existující mobilní telefon nebo office telefonních čísel v místním adresáři, můžete je synchronizovat v Azure AD a použijeme je pro automaticky resetování hesla.

Můžete taky přečíst další informace o [použití data tak, že heslo resetovat](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) a [jak můžete naplnit polí jednotlivé ověřování pomocí prostředí PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users).

## <a name="what-is-the-best-way-to-roll-out-password-reset-for-users"></a>Co je nejlepší způsob, jak zavedením resetování hesla pro uživatele?
Následují obecné zavedení kroky pro resetování hesla:

1.  Povolte pro vytvoření nového hesla v adresáři vašeho přejdete na kartu **Konfigurovat** na [Portálu Správa Azure](https://manage.windowsazure.com) a výběrem **Ano** pro možnost **Uživatelé povoleno pro resetování hesla** .
2.  Přiřaďte odpovídající licence pro každého uživatele, kterému chcete nabízejí heslo resetovat v tak, že přejdete na kartu **licence** na [Portálu Správa Azure](https://manage.windowsazure.com).
3.  Pokud chcete omezte heslo resetovat do skupiny uživatelů, kteří mají zavedením funkci pomalu určitou dobu nastavením **Omezení přístupu k resetování hesla** přepnout na **hodnotu Ano** a vyberte skupinu zabezpečení povolit pro resetování hesla (Poznámka: všechny tito uživatelé musí mít přiřazené licence).
4.  Řekněte uživatelům, můžete buď mu pošlete e-mailu, aby zaregistrovat, instrukce pro vytvoření nového hesla povolení nevynucují registraci na panelu aplikace access nebo odesláním odpovídající ověření dat pro tyto uživatele prostřednictvím DirSync, prostředí PowerShell nebo na [Portálu Správa Azure](https://manage.windowsazure.com).  Další informace o tom jsou uvedeny níže.
5.  Určitou dobu přečtěte si uživatelé registrace tak, že přejdete na kartu sestavy zobrazení sestavy [**Aktivity registrace resetování hesla**](active-directory-passwords-get-insights.md#view-password-reset-registration-activity) .
6.  Jakmile registrováno dobrý počet uživatelů, podívejte se na ně použít tak, že přejdete na kartu sestavy zobrazení sestavy [**Aktivity resetování hesla**](active-directory-passwords-get-insights.md#view-password-reset-activity) pro vytvoření nového hesla.

Existuje několik způsobů, jak informování uživatelů, můžete zaregistrujte si a používejte heslo resetovat ve vaší organizaci.  Jsou podrobně uvedeny níže.

### <a name="email-based-rollout"></a>E mailové komunikace
Možná je nejjednodušší informování uživatelů o zaregistrujte nebo použít heslo resetovat je tak, že je pošlete e-mailu je k tomu nevyzve instrukce.  Níže je šablonu, kterou můžete použít k tomuto.  Neváhejte nahradit barev a loga s podle vlastního výběru přizpůsobit je svým potřebám.

  ![][001]

Můžete si [Stáhnout šabloně e-mailu](http://1drv.ms/1xWFtQM).

### <a name="creating-your-own-password-portal"></a>Vytvoření portálu heslo
Jeden strategie vhodný pro větší zákazníky, kteří instalují možnosti správy heslo k vytvoření typu single "heslo portál", který uživatelům umožňuje spravovat všechno, co souvisí svoje hesla na jednom místě.  

V mnoha zákazníci největší zvolit vytvořit kořenové položky DNS jako https://passwords.contoso.com s odkazy na Azure AD heslo resetovat portál registrace portál resetování hesel a stránky změnit heslo.  Tímto způsobem v libovolné e-mailové komunikace nebo letáků, které odesíláte, můžete zahrnout jeden, snadno zapamatovatelné, adresu URL, která mohou uživatelé obsahujících podruhé Začínáme se službou.

Začínáme tady, jsme vytvořili jednoduché stránky, který používá nejnovější neodpovídá uživatelského rozhraní návrh vzorů a budou funkční na všechny prohlížeče a mobilní zařízení.

  ![][007]

Můžete si [Stáhnout šabloně webu](https://github.com/kenhoff/password-reset-page).  Doporučujeme přizpůsobení loga a barev potřeb vaší organizace.

### <a name="using-enforced-registration"></a>Použití vynucenou registrace
Pokud chcete uživatelům zaregistrujte heslo resetovat sami, můžete vynutit jim registraci při přihlášení na panelu aplikace access na [http://myapps.microsoft.com](http://myapps.microsoft.com).  Můžete tuto možnost povolíte z vašeho adresáře **Konfigurovat** karty povolením možnosti **Vyžadovat k registraci při přihlašování do panelu přístup uživatelů** .  

Můžete taky volitelně definovat, zda budou bude požádán, aby znovu zaregistrujte po konfigurovatelné časový úsek možnost **počet dní před uživatelé musí ověřit své o kontaktech** nenulovou hodnotu změnou. Další informace najdete v tématu [Přizpůsobení chování uživatelů a hesla správy](active-directory-passwords-customize.md#password-management-behavior) .

  ![][002]

Když povolíte tuto možnost, když uživatelé přihlašovat do panelu přístup, uvidí překryvné okno s upozorněním, že jim správce vyžaduje, aby ověřit své kontaktní informace. Ji může používat k obnovení hesla, pokud se stále ztratíte přístup k svému účtu.

  ![][003]

Kliknutím na **Ověřit teď** přiblíží k **registraci portál resetování hesla** v [http://aka.ms/ssprsetup](http://aka.ms/ssprsetup) a vyžaduje, aby zaregistrovat.  Registrace prostřednictvím tento způsob je možné klikněte na tlačítko **Storno** nebo zavřením okna zrušit, ale uživatelé upozorněni pokaždé, když budou pokud přihlášení není zaregistrovat.

  ![][004]

### <a name="uploading-data-yourself"></a>Odeslání dat
Pokud chcete nahrát ověřování dat sami, nemusí zaregistrovat uživatelů pro resetování před jejich hesla resetovat hesla.  Jak dlouho mají uživatelé ověřování dat definované na svůj účet, který splňuje heslo resetovat zásad, které jste definovali a potom tito uživatelé budou moci resetování hesla.

Zjistěte, jaké vlastnosti můžete nastavit přes připojení AAD nebo prostředí Windows PowerShell, najdete v článku [Jaká data používá resetování hesla](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset).

Ověřování dat prostřednictvím [Portálu pro správu Azure](https://manage.windowsazure.com) můžete odeslat pomocí následujících kroků:

1.  Přejděte do adresáře **Active Directory rozšíření** na [Portálu Správa Azure](https://manage.windowsazure.com).
2.  Klikněte na kartu **Uživatelé** .
3.  Vyberte uživatele, které vás zajímají ze seznamu.
4.  Na kartě první zjistíte **Alternativní e-mailu**, které mohou sloužit jako vlastnost Povolit resetování hesla.

    ![][005]

5.  Klikněte na kartu **Údajů o práci** .
6.  Na této stránce najdete **Telefon do kanceláře**, **mobilní telefon**, **Ověřování telefonu**a **Ověření e-mailu**.  Tyto vlastnosti můžete taky nastavit umožňuje povolit uživateli resetovat své heslo.

    ![][006]

Zobrazit, [Jaká data používá resetování hesla](active-directory-passwords-learn-more.md#what-data-is-used-by-password-reset) k najdete v článku jak každá z těchto vlastností můžete použít.

V tématu [Jak získat přístup ke heslo resetovat data pro svoje uživatele z prostředí PowerShell](active-directory-passwords-learn-more.md#how-to-access-password-reset-data-for-your-users) najdete v článku jak číst a nastavte tato data pomocí prostředí PowerShell.

## <a name="sample-training-materials"></a>Ukázka výukových materiálů
Pracujeme na ukázkové školicí materiály, které můžete použít pro organizaci IT a vaši uživatelé rychlé rychlejší přechod na nasazení a použití resetování hesla.  Zůstaňte nastavené!


<br/>
<br/>
<br/>

## <a name="links-to-password-reset-documentation"></a>Odkazy na heslo resetovat si přečtěte následující dokumentaci
Tady jsou odkazy na všech stránkách resetování hesla Azure AD si přečtěte následující dokumentaci:

* **Jste si tady, protože máte problémy s přihlášením?** Pokud ano, [tady je jak můžete změnit a resetování vlastního hesla](active-directory-passwords-update-your-own-password.md).
* [**Jak to funguje**](active-directory-passwords-how-it-works.md) – Další informace o šest jednotlivých součástí tuto službu a co každý znamená
* [**Začínáme**](active-directory-passwords-getting-started.md) – zjistěte, jak vám umožní uživatelům obnovit a změnit jejich cloudu a místní hesla
* [**Vlastní**](active-directory-passwords-customize.md) – zjistěte, jak přizpůsobit vzhled a chování a nastavení služby potřebám vaší organizace
* [**Dozvědět se víc**](active-directory-passwords-get-insights.md) – Další informace o naše integrované možnosti vytváření sestav
* [**Časté otázky**](active-directory-passwords-faq.md) – odpovědi na časté otázky
* [**Poradce při potížích**](active-directory-passwords-troubleshoot.md) – zjistěte, jak rychle vyřešit problémy se službou
* [**Další informace**](active-directory-passwords-learn-more.md) – přejděte důkladné do technické podrobnosti Princip služby



[001]: ./media/active-directory-passwords-best-practices/001.jpg "Image_001.jpg"
[002]: ./media/active-directory-passwords-best-practices/002.jpg "Image_002.jpg"
[003]: ./media/active-directory-passwords-best-practices/003.jpg "Image_003.jpg"
[004]: ./media/active-directory-passwords-best-practices/004.jpg "Image_004.jpg"
[005]: ./media/active-directory-passwords-best-practices/005.jpg "Image_005.jpg"
[006]: ./media/active-directory-passwords-best-practices/006.jpg "Image_006.jpg"
[007]: ./media/active-directory-passwords-best-practices/007.jpg "Image_007.jpg"
