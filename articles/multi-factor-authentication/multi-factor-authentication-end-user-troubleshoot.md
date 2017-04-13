<properties
    pageTitle="Poradce při potížích s dvoustupňového ověření | Microsoft Azure"
    description="V tomto dokumentu budou uživatelé na zadejte informace co dělat, pokud narazíte na problém s Azure Vícefaktorové ověřování."
    services="multi-factor-authentication"
    keywords = "vícefaktorové ověřování klienta, ověřování problém, ID korelace"
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
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="having-trouble-with-two-step-verification"></a>Máte potíže s dvoustupňového ověření

Tento článek popisuje některé problémy, které se můžete setkat v dvoustupňového ověření. Pokud problém, který máte není zahrnut je tady, přejděte prosím názor podrobné v oddílu komentáře tak, aby můžeme vylepšit.

## <a name="i-lost-my-phone-or-it-was-stolen"></a>Ztratilo telefonu nebo byl krádeže

Existují dva způsoby, jak vstoupit zpátky ke svému účtu. Prvním je Přihlaste se pomocí svého telefonního čísla alternativní ověřování, pokud jste nastavili jednu. Druhý je požádejte správce, aby smazat nastavení.

Pokud svůj telefon byl ztratili nebo krádeže, doporučujeme také, že máte správce resetování hesla aplikace a zrušte zaškrtnutí políčka některou zapamatuje zařízení. Není-li správce se, jak to provést, nasměrujte je na tento článek: [Správa uživatelů a zařízení](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords).


### <a name="use-an-alternate-phone-number"></a>Použití alternativní telefonní číslo

Pokud jste nastavili více možností ověření, včetně sekundární telefonní číslo nebo aplikace ověřovacích dat na jiném zařízení, můžete jeden z těchto článků přihlásit.

Přihlaste se pomocí alternativní telefonní číslo, postupujte takto:

1. Přihlaste se jako normálně.
2. Po zobrazení výzvy k další ověření vašeho účtu, vyberte **použít jiný ověřovací možnost**.

    ![Různé ověření](./media/multi-factor-authentication-end-user-manage/differentverification.png)

3. Vyberte telefonní číslo, které máte přístup.

    ![Alternativní telefonní](./media/multi-factor-authentication-end-user-manage/altphone2.png)

4. Až budete zase ve vašem účtu, [Spravovat nastavení](multi-factor-authentication-end-user-manage-settings.md) změnit telefonní číslo ověřování.

>[AZURE.IMPORTANT]
>Je důležité Konfigurace sekundárního ověřování telefonního čísla. -Li primární telefonní číslo a mobilní aplikace na stejné telefonu, budete potřebovat třetí možnost ztráty nebo krádeže telefonu.

### <a name="clear-your-settings"></a>Vymazání nastavení

Pokud jste nenakonfigurovali sekundární ověřování telefonního čísla, budete muset požádejte správce o pomoc. Požádejte vymažte nastavení tak, aby při příštím přihlášení, zobrazí se výzva k [nastavení svého účtu](multi-factor-authentication-end-user-first-time.md) znovu.


## <a name="i-am-not-receiving-a-text-or-call-on-my-phone"></a>Doručována text nebo volání v telefonu

Existuje několik důvodů, proč se mohou pokusíte přihlásit, ale nezobrazí na text nebo telefonního hovoru. Pokud jste úspěšně obdržíte texty nebo telefonní hovory na váš telefon v minulosti, je to pravděpodobně problém s poskytovatele telefonu není váš účet. Ujistěte se, že máte správné buňky signálu a pokud se pokoušíte přijímat textové zprávy zkontrolujte plánu telefonu a služby podpory textové zprávy.

Pokud jste počkat několik minut u textu nebo volání, je nejrychlejší způsob, jak dostat ke svému účtu zkusit jinou možnost.

1. Vyberte **použít jiný ověřovací možnost** na stránce, čeká na ověření.

    ![Různé ověření](./media/multi-factor-authentication-end-user-troubleshoot/diff_option.png)

2. Vyberte telefonní číslo nebo doručení metodu, který chcete použít.

    Pokud jste obdrželi více kódů ověření, jde použít pouze nejnovějšími daty z nich.

Pokud nemáte nakonfigurovaný ještě jeden způsob, požádejte správce a požádáte ho, aby smazat nastavení. Při příštím přihlášení, zobrazí se výzva k [Nastavení vícefaktorové ověřování](multi-factor-authentication-end-user-first-time.md) znovu.


Pokud máte často zpoždění kvůli signálu chybná buňky, doporučujeme, abyste že použití [aplikace Microsoft ověřovacích dat](multi-factor-authentication-microsoft-authenticator.md) na svého smartphonu. Aplikaci můžete generovat kódy náhodných zabezpečení, které používáte pro přihlášení a kódů nevyžadovat při tom všechny buňky signálu nebo připojení k Internetu.


## <a name="app-passwords-are-not-working"></a>Aplikace hesla nefungují

Nejdřív zkontrolujte, že jste zadali heslo aplikace správně.  Pokud se stále nefunguje zkuste přihlášením a [vytvořit nové aplikace](multi-factor-authentication-end-user-app-passwords.md).  Pokud to není práci, kontaktujte svého správce a jejich [Odstranění existující hesla aplikace](multi-factor-authentication-manage-users-and-devices.md#delete-users-existing-app-passwords) a pak vytvořte novou.

## <a name="i-didnt-find-an-answer-to-my-problem"></a>Odpověď na Můj problém neměli hledání

Pokud jste vyzkoušeli tyto možnosti řešení potíží, ale spuštěné na problémy, kontaktujte svého správce nebo osoba, která nastavení vícefaktorové ověřování za vás. Se soubory byste měli vám pomůže.

Taky můžete odeslat dotaz na [Azure AD fóra](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD) nebo [Kontaktujte podporu](https://support.microsoft.com/contactus) a jsme budete odpovídání na váš problém hned, jak můžeme.

Pokud budete kontaktovat podporu, obsahují následující informace:

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
- [Správa nastavení dvoustupňového ověření](multi-factor-authentication-end-user-manage-settings.md)  
- [Nejčastější dotazy týkající se aplikace Microsoft Authenticator](multi-factor-authentication-app-faq.md)
