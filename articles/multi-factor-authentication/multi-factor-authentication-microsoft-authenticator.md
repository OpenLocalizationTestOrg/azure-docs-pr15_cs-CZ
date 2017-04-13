<properties
    pageTitle="Aplikace Microsoft Authenticator pro mobilní telefony | Microsoft Azure"
    description="Zjistěte, jak upgradovat na nejnovější verzi ověřovacích dat Azure."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtland"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/22/2016"
    ms.author="kgremban"/>

# <a name="microsoft-authenticator"></a>Microsoft ověřovacích dat

V aplikaci Microsoft Authenticator poskytuje další úroveň zabezpečení v Azure účtu (například bsimon@contoso.onmicrosoft.com), vaší místní pracovním účtem (třeba bsimon@contoso.com), nebo účet Microsoft (například bsimon@outlook.com).

Aplikace funguje jedním ze dvou způsobů:

- **Oznámení**. Aplikaci dávají zabránili neoprávněnému přístupu na účty a zastavit podvodné transakce stisknutím oznámení do svého smartphonu nebo tabletu. Jednoduše zobrazit oznámení a je-li legitimní, vyberte **ověření**. Jinak můžete vybrat **Odepřít**. Informace o zakázání oznámení najdete v tématu Jak používat funkci podvod sestavy a odepřít pro Vícefaktorové ověřování.

- **Hesla se ověřovací kód**. Aplikace mohou sloužit jako software token generovat kód ověření OAuth. Zadejte kód poskytované aplikace na přihlašovací obrazovce spolu s uživatelské jméno a heslo, po zobrazení výzvy. Ověřovací kód obsahuje druhý formulář ověřování.

Verze aplikace Microsoft Authenticator se má nahradit starou aplikaci ověřovacích dat Azure.  Aplikaci ověřovacích Azure dat bude pokračovat v práci, ale pokud se rozhodnete přesunout do nové aplikace Microsoft Authenticator, v tomto článku vám mohou pomoci.  

## <a name="install-the-app"></a>Instalace aplikace

Aplikace Microsoft Authenticator je k dispozici pro [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072)a [IOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-to-the-app"></a>Přidání účtů v aplikaci

Pro každý účet, který chcete přidat do aplikace Microsoft Authenticator použijte jeden z následujících postupů.

### <a name="add-an-account-to-the-app-by-using-the-qr-code-scanner"></a>Přidání účtu do aplikace pomocí skeneru kódu QR

1. Přejděte na obrazovku nastavení ověření zabezpečení.  Informace o tom, jak dostat na této obrazovce najdete v tématu [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md).

2. Zaškrtněte políčko **Konfigurovat**.

    ![Tlačítko konfigurace na obrazovce nastavení ověření zabezpečení](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Tím se vyvolá obrazovka s kódu QR na něj.

    ![Obrazovky, poskytuje kódu QR](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Otevřete aplikaci ověřovacích dat Microsoft. Na obrazovce **účty** vyberte **+**a pak zadejte, kterou chcete přidat pracovní nebo školní účet.

    ![Na obrazovce účty se symbolem plus](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Obrazovka pro zadání pracovního nebo školního účtu](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. Skenování kódu QR pomocí fotoaparátu a pak vyberte **Hotovo** a zavřete okno kódu QR.

    ![Obrazovka pro skenování kódu QR](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

    Pokud kamera nefunguje správně, můžete zadat kódu QR a adresu URL ručně. Další informace najdete v tématu [Přidání účtu do aplikace ručně](#add-an-account-to-the-app-manually).

5. Počkejte, dokud aktivaci účtu. Po dokončení aktivací vyberte **kontakt Já**.  To, odešle oznámení nebo ověřovací kód telefonu.  Vyberte **ověření**.

    ![Obrazovka vyberete ověření se přihlásit](./media/multi-factor-authentication-end-user-first-time-mobile-app/verify.png)

6. Pokud vaše společnost vyžaduje PIN kódu pro schvalování přihlašovací ověření, zadejte ho.

    ![Pole pro zadání PIN kódu](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan3.png)

7. Po dokončení zadávání kódu PIN vyberte **Zavřít**. V tomto okamžiku ověření by měl být úspěšný.
8. Doporučujeme zadejte číslo svého mobilního telefonu v případě, že ztratíte přístup k aplikaci. Zadejte zemi z rozevíracího seznamu a zadejte číslo svého mobilního telefonu v poli vedle názvu země. Vyberte **Další**.
9. V tomto okamžiku jste nastavili způsob kontaktování. Teď je čas nastavit aplikace hesel pro prohlížení aplikací, jako je Outlook 2010 nebo starší. Pokud nepoužíváte tyto aplikace, vyberte **Hotovo**. Jinak pokračujte dalším krokem.

    ![Obrazovka pro vytváření heslo k aplikaci](./media/multi-factor-authentication-end-user-first-time-mobile-app/step4.png)

10. Pokud používáte aplikace není prohlížeče, zkopírujte heslo zadané aplikace a vložte heslo do aplikace. Postup na jednotlivých aplikací, jako je Outlook i Lync najdete v článku jak změnit heslo v e-mailu aplikace heslo a jak si změnit heslo aplikaci a aplikace heslo.
11. Vyberte **Hotovo**.

Teď byste měli vidět nový účet v dialogovém okně **účty** .

![Účty obrazovky](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-manually"></a>Ruční přidání účtu do aplikace

1. Přejděte na obrazovku nastavení ověření zabezpečení.  Informace o tom, jak dostat na této obrazovce najdete v tématu [Změna nastavení zabezpečení](multi-factor-authentication-end-user-manage-settings.md).

2. Zaškrtněte políčko **Konfigurovat**.

    ![Tlačítko konfigurace na obrazovce nastavení ověření zabezpečení](./media/multi-factor-authentication-azure-authenticator/azureauthe.png)

    Tím se vyvolá obrazovka s kódu QR na něj.  Poznámka: kód a adresu URL.

    ![Obrazovky, poskytuje kódu QR a adresy URL](./media/multi-factor-authentication-azure-authenticator/barcode2.png)

3. Otevřete aplikaci ověřovacích dat Microsoft. Na obrazovce **účty** vyberte **+**a pak zadejte, kterou chcete přidat pracovní nebo školní účet.

    ![Na obrazovce účty se symbolem plus](./media/multi-factor-authentication-azure-authenticator/addaccount3.png)

    ![Obrazovka pro zadání pracovního nebo školního účtu](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan.png)

4. V skener vyberte **ručně zadat kód**.

    ![Obrazovka pro skenování kódu QR](./media/multi-factor-authentication-end-user-first-time-mobile-app/scan2.png)

5. Zadejte kód a adresu URL do příslušných polí v aplikaci.

    ![Obrazovka pro zadávání kódu a adresu URL](./media/multi-factor-authentication-azure-authenticator/manual.png)

    ![Obrazovka pro zadávání kódu a adresu URL](./media/multi-factor-authentication-end-user-first-time-mobile-app/addaccount2.png)

6. Počkejte, dokud aktivaci účtu. Po aktivaci dokončení, vyberte **kontakt Já**. To, odešle oznámení nebo ověřovací kód telefonu. Vyberte **ověření**.

Teď byste měli vidět nový účet v dialogovém okně **účty** .

![Účty obrazovky](./media/multi-factor-authentication-azure-authenticator/accounts.png)

### <a name="add-an-account-to-the-app-by-using-touch-id"></a>Přidání účtu do aplikace pomocí ID dotykového ovládání

Aplikace Microsoft Authenticator na iOS podporuje dotykové ovládání ID.  Azure Vícefaktorové ověřování umožňuje organizacím vyžadovat kód PIN pro zařízení. S dotykovým ovládáním ID uživatele iOS není potřeba zadat kód PIN. Můžete místo toho skenování jejich otisku a vyberte **schválení**.

Nastavte si dotykové ovládání ID s Microsoft Authenticator je velmi jednoduché. Dokončení výzvu normální ověřování kódem PIN. Pokud vaše zařízení podporuje dotykové ovládání ID, Microsoft Authenticator nastaví ho automaticky tohoto účtu.

![Ověření nastavení ID dotykového ovládání](./media/multi-factor-authentication-azure-authenticator/touchid1.png)

Od místa dále, pokud budete vyzváni k ověření vaší přihlásit, vyberte přijaté nabízená oznámení a prohledávání prstu místo přímého zadávání svůj PIN kód.

![Nabízená oznámení](./media/multi-factor-authentication-azure-authenticator/touchid2.png)

## <a name="uninstall-the-old-azure-authentication-app"></a>Odinstalujte starou aplikaci ověřování Azure

Po přidání všech účtů na nová aplikace se Odinstalujte starou aplikaci z telefonu.

## <a name="delete-an-account"></a>Odstranění účtu

Odebrání účtu v aplikaci Microsoft Authenticator, vyberte účet a potom vyberte **Odstranit**.

![Tlačítko pro odstranění](./media/multi-factor-authentication-azure-authenticator/remove.png)
