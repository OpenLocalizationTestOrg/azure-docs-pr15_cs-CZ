<properties
    pageTitle="Postup při konfiguraci Twitter ověřování aplikace aplikace služby"
    description="Zjistěte, jak nakonfigurovat Twitteru ověřování pro aplikaci služby aplikace."
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>Jak nakonfigurovat aplikaci služby aplikace pomocí Twitter přihlášení

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

V tomto tématu se dozvíte, jak nakonfigurovat aplikaci služby Azure jako poskytovatele ověřování použít Twitter.

Postupujte podle kroků v tomto tématu, pokud nemáte účet Twitter, který má ověřenou e-mailovou adresu a telefonní číslo. Pokud chcete vytvořit nový účet Twitter, přejděte na <a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.

## <a name="register"> </a>Registrovat aplikaci Twitter


1. Přihlaste se k [portálu Azure]a přejděte do aplikace. Zkopírujte **adresu URL**. Tím se používá ke konfiguraci aplikace Twitter.

2. Přejděte na web [Twitter vývojáři] , přihlaste se pomocí svých přihlašovacích údajů účtu Twitter a klikněte na **Vytvořit novou aplikaci**.

3. Zadejte **název** a **Popis** pro novou aplikaci. Vložte do aplikace **adresy URL** pro hodnotu **webu** . O **Zpětné URL**vložte **Zpětné URL** , kterou jste si zkopírovali. Toto je přidán s cestou _/.auth/login/twitter/callback_brána mobilní aplikaci. Například `https://contoso.azurewebsites.net/.auth/login/twitter/callback`. Ujistěte se, že používáte schématu HTTPS.

3.  Dole na stránce přečtěte si a přijměte podmínky. Klikněte na **vytvořit aplikace Twitter**. To zaregistruje panelu aplikace zobrazující podrobnosti aplikace.

4. Klikněte na kartu **Nastavení** zaškrtněte **Povolit tuto aplikaci pro použití se přihlásili s Twitteru**, a poté klikněte na **Nastavení aktualizace**.

5. Vyberte kartu **klíče a tokeny přístupu** . Poznamenejte si hodnoty **Spotř (klíč rozhraní API)** a **příjemce tajná (tajná rozhraní API)**.

    > [AZURE.NOTE] Tajná příjemce je důležité zabezpečení pověření. Sdílet tento tajná s kýmkoli nebo distribuce s aplikací.


## <a name="secrets"> </a>Přidat Twitter informace o aplikaci

13. Zpátky v [Azure portál]přejděte do aplikace. Klikněte na **Nastavení**a potom **ověřování / se tak mohli ověřovat**.

14. Pokud ověření / se tak mohli ověřovat funkce není povolená, přepínač na **zapnuto**.

15. Klikněte na **Twitter**. Vložte do pole ID aplikace a aplikace tajná hodnoty, které jste získali dříve. Klikněte na **OK**.

    ![][1]

    Ve výchozím nastavení aplikace služba poskytuje ověřování, ale není omezit oprávnění přístup k rozhraní API a obsahem webu. Uživatelé musí povolit v aplikaci kódu.

17. (Volitelné) Pokud chcete omezit přístup k vašemu webu uživatelům jenom ověřené Twitter, nastavte **akce provedeny, pokud není ověřen požadavek** na **Twitter**. Tento postup vyžaduje, aby všechny žádosti o ověření a všechny žádosti o neověřených přesměrováni na Twitter pro ověřování.

17. Klikněte na **Uložit**.

Teď jste připraveni Twitter k ověření můžete použít v aplikaci.

## <a name="related-content"> </a>Související obsah

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Vývojáři Twitter]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure portálu]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
