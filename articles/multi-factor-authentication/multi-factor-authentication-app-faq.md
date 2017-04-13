<properties
    pageTitle="Aplikace Microsoft ověřovacích dat – nejčastější dotazy"
    description="Obsahuje seznam často kladené otázky a odpovědi týkající se aplikace Microsoft Authentication a Azure Vícefaktorové ověřování."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar, librown"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator-application-faq"></a>Nejčastější dotazy týkající se aplikace Microsoft Authenticator

V aplikaci Microsoft Authenticator nahradit aplikaci ověřovacích Azure dat a doporučené aplikace použijete Azure Vícefaktorové ověřování. Tato aplikace je k dispozici pro Windows Phone, Android a iOS.

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

- **Co se stalo s ověřovacích dat Azure, Multi-Factor Auth a aplikací účet Microsoft?**

    V aplikaci Microsoft Authenticator slouží k nahrazení vzájemně tyto aplikace. Upgrade na Microsoft Authenticator Azure ověřovacích dat. Používáte Multi-Factor Auth nebo aplikací účtu Microsoft, můžete nainstalovat Microsoft Authenticator a opětovné přidání účtů. Ujistěte se, že dokončíte přidávání účty pro nové aplikace před odstraněním staré.

- **Už používám aplikaci Microsoft Authenticator pro ověření kódy. Jak se dá přepnout jedním kliknutím nabízená oznámení?**  

    Schválení přihlásit pomocí nabízená oznámení je dostupný jenom u účtů Microsoft, ale ne pro účty třetí strany jako Google nebo Facebooku. Pracovní nebo školní účet Microsoft a vaše organizace můžete vypnout tuto možnost, i když.

    Pokud používat účet Microsoft pro svůj osobní účet a chcete přepnout na nabízená oznámení, musíte přidat svůj účet znova. Znovu zaregistrujte zařízení pomocí svého účtu a nastavení nabízených oznámení.  

    Pokud váš účet nemá dvoustupňového ověření povoleno, přečtěte si článek [o dvoustupňového ověření](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) rozhodnout, pokud je pro vás nejlepší.  

- **Pokud se vám nebudou moct používat jedním kliknutím nabízená oznámení na Iphonu nebo Ipadu?**  

    Tato funkce je v beta až do konce srpen, až bude veřejně dostupný u účtů Microsoft. Pokud se chcete připojit náš beta program, odeslat e-mailů na msauthenticator@microsoft.com. Zahrnout křestního jména, příjmení a Apple ID do e-mailu.  

- **Jedním kliknutím nabízená oznámení fungují u účtů společnosti Microsoft**  

    Ne, nabízená oznámení fungují jen pro účty Microsoft a účty adresářové služby Azure Active Directory. Pokud váš pracovní nebo školní používá Azure AD účty, jejich může tuto funkci zakázat.  

- **Můžu obnovit zařízení ze zálohy a chybějící nebo nefunguje kódy Můj účet. Co se přihodilo?**  

    Z bezpečnostních důvodů doporučujeme není účty obnovení systému pomocí zálohy aplikace. Obnovení aplikace IOS ze zálohy, účtů stále zobrazují ale nefungují k příjmu přihlašovací ověření nebo generovat zabezpečení kódy. Po obnovení aplikace odstranit účty a znovu přidat.

- **Objevila se nové zařízení. Jak odebrat aplikaci Microsoft Authenticator ze starého zařízení a přesuňte do nového?**

    Přidání aplikace Microsoft Authenticator nové zařízení neodstraní automaticky na jiných zařízeních. Pokud chcete spravovat, která zařízení jsou nakonfigurovány pro váš účet, na webu stejné, který slouží ke správě dvoustupňového ověření a zvolte odebrat staré aplikace.

    Osobní účet Microsoft a je tento web stránku [účet zabezpečení](https://account.microsoft.com/security) . Pracovní nebo školní účet Microsoft a může být tento web [Moje aplikace](https://myapps.microsoft.com) nebo vlastní portál, který vaše organizace nastavila.

## <a name="contact-us"></a>Kontaktujte nás

Pokud tady nebyl zodpovězen svou otázku, napište komentář v dolní části stránky. Nebo [Kontaktujte podporu](https://support.microsoft.com/contactus) a můžeme si odpovězte na váš problém hned, jak můžeme.

Kontaktujte podporu uveďte co nejvíc tyto informace jako můžete provést tyto akce:

- **ID uživatele** – co je e-mailovou adresu, kterou jste se přihlašujete?
- **Obecný popis chyby** – k čemu přesně dochází k chybě měli jste najdete v článku?  Pokud jste udělali žádná chybová zpráva, popisují neočekávané chování, které jste si všimli podrobně.
- **Stránka** – jaké stránky umístěné když jste viděli chyby do schránky (uveďte adresu URL)?
- **Kód chyby** – kód chyby, které dostáváte.
- **ID relace** – id konkrétní relace, které dostáváte.
- **ID korelace** – jaký byl kód id korelace přihlášení vygenerované při uživatel viděli chyby do schránky.
- **Časové razítko** – jaký byl přesné datum a čas viděli chyby do schránky (včetně časové pásmo)?

Velká část těchto informací se nachází na přihlašovací stránku. Až si ověřte vaše přihlašovací v čase, vyberte **Zobrazit podrobnosti**.

![Podrobnosti o chybách přihlášení](./media/multi-factor-authentication-end-user-troubleshoot/view_details.png)

Vložení těchto informací pomáhá nás vyřešit váš problém co nejdříve.

## <a name="related-topics"></a>Příbuzná témata

- [Nejčastější dotazy týkající se Azure Vícefaktorové ověřování](multi-factor-authentication-faq.md)  
- [O dvoustupňového ověření](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification) pro účty Microsoft
- [Máte potíže s dvoustupňového ověření?](multi-factor-authentication-end-user-troubleshoot.md)
