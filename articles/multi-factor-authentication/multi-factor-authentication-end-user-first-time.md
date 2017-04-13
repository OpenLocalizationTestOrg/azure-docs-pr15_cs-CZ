<properties
    pageTitle="Nastavení dvoustupňového ověření pro svůj pracovní nebo školní účet"
    description="Pokud vaše společnost nakonfiguruje Azure Vícefaktorové ověřování, zobrazí se zaregistrovat k dvoustupňového ověření. Zjistěte, jak nastavit. "
    services="multi-factor-authentication"
    keywords="jak používat azure adresáře active directory v cloudu, kurz služby active directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="pblachar"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="kgremban"/>

# <a name="set-up-my-account-for-two-step-verification"></a>Nastavení účtu pro dvoustupňového ověření

Dvoustupňového ověření představuje krok větší zabezpečení, které pomáhají chránit váš účet tím, že pro ostatní mohli v zalomit složitější. Pokud v tomto článku, kterou právě čtete, pravděpodobně máte e-mailu z vaší pracovní nebo školní správce o Vícefaktorové ověřování. Nebo možná pokusili přihlásit a máte zpráva s výzvou k ověření větší zabezpečení. Pokud je případě **že se nemůžete přihlásit, dokud nedokončíte proces zápisu automatického**.

Tento článek vám pomůže nastavit svůj **pracovní nebo školní účet**. Pokud chcete povolit dvoustupňového ověření vašim osobním účtem Microsoft, přečtěte si článek [o dvoustupňového ověření](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification).

## <a name="determine-how-you-will-use-multi-factor-authentication"></a>Určete, jak budou používat vícefaktorové ověřování

Dvoustupňového ověření pracuje s výzvou k dvěma datovými identifikace při přihlášení. Nejdřív požádáme pro vaše uživatelské jméno a heslo jako obvykle. Pak si kontaktů, do kterých patří telefonu, který víme a vy jste ověřujeme, že pokus o přihlášení byl legitimní.  

Začít s proces instalace, zkuste se přihlásit ke svému účtu, stejně jako obvykle. Pokud správce nakonfiguroval váš účet k dvoustupňového ověření, budete vyzváni k zahajte proces automatického zápisu. Začněte kliknutím na tento proces **nyní nastavit.**

![Nastavení](./media/multi-factor-authentication-end-user-first-time/first.png)

První otázky v procesu registrace je způsob vás kontaktovat. Podívejte se možnosti v tabulce, můžete přejít k postupu nastavení pro každou metodu odkazy.

| Způsob kontaktování | Popis |
| --- | --- |
[Mobilní aplikace](#use-a-mobile-app-as-the-contact-method) | - **Oznámení k ověření.** Tato možnost nabízí oznámení k aplikaci ověřovacích dat na smartphonu nebo tabletu. Zobrazit oznámení a, je-li legitimní, vyberte **Ověřit** v aplikaci. Pracovní nebo školní může vyžadovat zadat kód PIN, než je ověřit.<br>- **Použijte ověřovací kód.** V tomto režimu generuje aplikace ověřovacích dat ověřovací kód, který aktualizuje každých 30 sekund. Zadejte nejnovější aktualizaci ověřovací kód v rozhraní přihlášení.<br>Aplikace Microsoft Authenticator je k dispozici pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073). |
[Mobilní telefon volání nebo textu](#use-your-mobile-phone-as-the-contact-method) | - **Zavolat** umístí automatizovaných hlasových volání na telefonní číslo, které zadáte. Pokud chcete hovor přijmout a stiskněte klávesu # na klávesnici telefonu k ověření.<br>- **Textová zpráva** končí textovou zprávu obsahující ověřovacího kódu. Po zobrazení výzvy v textu odpověď na zprávu textu nebo zadejte ověřovací kód do rozhraní přihlášení k dispozici. |  
[Office telefonního hovoru](#use-your-office-phone-as-the-contact-method) | Umístí automatizovaných hlasových volání na telefonní číslo, které zadáte. Pokud chcete hovor přijmout a stiskne # na klávesnici telefonu k ověření. |

## <a name="use-a-mobile-app-as-the-contact-method"></a>Používání mobilní aplikace jako způsob kontaktování

Používání této metody vyžaduje instalaci aplikace ověřovacích dat na telefonu nebo tabletu. Kroky v tomto článku jsou založené na aplikaci Microsoft Authenticator, který je dostupný pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

1. **Mobilní aplikace** vyberte z rozevíracího seznamu.
2. Vyberte **zasílat oznámení ve formě k ověření** nebo **použít kód ověření**a potom vyberte **Nastavení**.

    ![Větší zabezpečení ověření obrazovky](./media/multi-factor-authentication-end-user-first-time-mobile-app/mobileapp.png)

3. Na telefonu nebo tabletu, otevřete aplikaci a vyberte **+** pro přidání nového účtu. (Na zařízeních s Androidem, vyberte tři tečky potom **Přidat účet**)
4. Určete, jestli chcete přidat pracovní nebo školní účet. Otevře se skener kódu QR na telefonu. Pokud kamera nefunguje správně, můžete vybrat ruční zadání údajů o vaší společnosti. Další informace najdete v tématu [Přidání účtu ručně](#add-an-account-manually).  
5. Skenování obrázek kódu QR, který se zobrazovala obrazovka pro konfiguraci mobilní aplikaci.  Vyberte **Hotovo** a zavřete okno kódu QR.  

    ![Obrazovka kódu QR](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

6. Po aktivaci dokončení v telefonu, vyberte **kontakt Já**.  Tento krok odešle oznámení nebo ověřovací kód do telefonu. Vyberte **ověření**.  
7. Pokud vaše společnost vyžaduje PIN kódu pro schvalování přihlašovací ověření, zadejte ho.

    ![Pole pro zadání PIN kódu](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

8. Po dokončení zadávání kódu PIN vyberte **Zavřít**. V tomto okamžiku ověření by měl být úspěšný.
9. Doporučujeme, abyste v případě, že ztratíte přístup k mobilní aplikace zadejte číslo svého mobilního telefonu. Zadejte zemi z rozevíracího seznamu a zadejte číslo svého mobilního telefonu v poli vedle názvu země. Vyberte **Další**.
10. V tomto okamžiku se výzva k nastavení hesla aplikace pro prohlížení aplikace třeba Outlook 2010 nebo starší nebo nativní e-mailové aplikace na zařízeních Apple. Je to proto některé aplikace nepodporují dvoustupňového ověření. Pokud nepoužíváte tyto aplikace, klikněte na tlačítko **Hotovo** a přeskočte zbývající kroky.
11. Pokud používáte tyto aplikace, kopírovat heslo aplikace k dispozici a vložte do aplikace místo běžná heslo. Stejné heslo aplikace můžete použít pro více aplikací. Další informace [Nápověda k aplikaci hesla].
12. Klikněte na tlačítko **Hotovo**.


### <a name="add-an-account-manually"></a>Ruční přidání účtu
Pokud chcete přidat účet v mobilní aplikaci ručně, místo použití QR reader, postupujte takto:

1. Vyberte tlačítko **Ruční zadání účtu** .  
2. Zadejte kód a adresu URL, která jsou k dispozici na stejné stránce zobrazující čárového kódu. Tyto informace o jsou uvedeny v polích **kód** a **adresu URL** v mobilní aplikaci.

    ![Nastavení](./media/multi-factor-authentication-end-user-first-time-mobile-app/barcode2.png)

3. Po aktivaci, vyberte **kontakt Já**. Tento krok odešle oznámení nebo ověřovací kód do telefonu. Vyberte **ověření**.

## <a name="use-your-mobile-phone-as-the-contact-method"></a>Použití mobilního telefonu jako způsob kontaktování

1. V rozevíracím seznamu vyberte **Ověřování telefon** .  

    ![Nastavení](./media/multi-factor-authentication-end-user-first-time-mobile-phone/phone.png)  

2. Z rozevíracího seznamu vyberte vaše země a zadejte číslo svého mobilního telefonu.
3. Výběr metody, kterou chcete použít s mobilním telefonu – text nebo hovoru.
4. Vyberte **kontakt mě** pro ověření vaší telefonní číslo. V závislosti na režim, který jste vybrali jsme odeslat text nebo volání. Postupujte podle pokynů na obrazovce a pak vyberte **ověření**.
5. V tomto okamžiku se výzva k nastavení hesla aplikace pro prohlížení aplikace třeba Outlook 2010 nebo starší nebo nativní e-mailové aplikace na zařízeních Apple. Je to proto některé aplikace nepodporují dvoustupňového ověření. Pokud nepoužíváte tyto aplikace, klikněte na tlačítko **Hotovo** a přeskočte zbývající kroky.
6. Pokud používáte tyto aplikace, kopírovat heslo aplikace k dispozici a vložte do aplikace místo běžná heslo. Stejné heslo aplikace můžete použít pro více aplikací. Další informace [Nápověda k aplikaci hesla].
7. Klikněte na tlačítko **Hotovo**.

## <a name="use-your-office-phone-as-the-contact-method"></a>Použijte telefon v kanceláři jako způsob kontaktování

1. Vyberte **Telefon do kanceláře** z rozevíracího seznamu  

    ![Nastavení](./media/multi-factor-authentication-end-user-first-time-office-phone/office.png)  

2. Pole telefonního čísla bude automaticky vložen kontaktních údajů o vaší společnosti. Pokud je číslo nepovedlo neobsahují žádnou hodnotu, požádejte správce, pokud chcete provést změny.
4. Vyberte **kontakt mě** pro ověření vaší telefonní číslo a název svého čísla. Postupujte podle pokynů na obrazovce a pak vyberte **ověření**.
5. V tomto okamžiku se výzva k nastavení hesla aplikace pro prohlížení aplikace třeba Outlook 2010 nebo starší nebo nativní e-mailové aplikace na zařízeních Apple. Je to proto některé aplikace nepodporují dvoustupňového ověření. Pokud nepoužíváte tyto aplikace, klikněte na tlačítko **Hotovo** a přeskočte zbývající kroky.
6. Pokud používáte tyto aplikace, kopírovat heslo aplikace k dispozici a vložte do aplikace místo běžná heslo. Stejné heslo aplikace můžete použít pro více aplikací. Další informace najdete v tématu [Co jsou aplikace hesla](multi-factor-authentication-end-user-app-passwords.md).
7. Klikněte na tlačítko **Hotovo**.

## <a name="next-steps"></a>Další kroky

- Změna preferovaného možnosti a [Spravovat svoje nastavení pro dvoustupňového ověření](multi-factor-authentication-end-user-manage-settings.md)
- Nastavení [hesla aplikace](multi-factor-authentication-end-user-app-passwords.md) k aplikacím nativní zařízení, které nepodporují dvoustupňového ověření.
- Podívejte se na [ověřovacích Microsoft dat aplikace](multi-factor-authentication-microsoft-authenticator.md) pro rychlé a zabezpečené ověřování i v případě, že nemáte službu buňky.
