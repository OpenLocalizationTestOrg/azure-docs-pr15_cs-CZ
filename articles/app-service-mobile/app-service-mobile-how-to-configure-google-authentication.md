<properties
    pageTitle="Postup při konfiguraci Google ověřování aplikace aplikace služby"
    description="Zjistěte, jak nastavit Google ověřování pro aplikaci služby aplikace."
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>Jak nakonfigurovat aplikaci služby aplikace pomocí služby Google přihlášení

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

V tomto tématu se dozvíte, jak nakonfigurovat aplikaci služby Azure jako poskytovatele ověřování použít Google.

Postupujte podle kroků v tomto tématu, pokud nemáte účet Google, který má ověřenou e-mailovou adresu. Pokud chcete vytvořit nový účet Google, přejděte na [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).

## <a name="register"> </a>Registrovat aplikaci Google

1. Přihlaste se k [portálu Azure]a přejděte do aplikace. Zkopírujte adresu **URL**, které můžete později nakonfigurovat aplikaci služby Google.

2. Přejděte na web [Google API](http://go.microsoft.com/fwlink/p/?LinkId=268303) , přihlaste se pomocí svých přihlašovacích údajů účtu Google, klikněte na **Vytvořit projekt**, zadejte **název projektu**, a pak klikněte na **vytvořit**.

3. V části **Sociální rozhraní API** klikněte na **Google + rozhraní API** a potom **Povolit**.

4. V levém navigačním panelu **pověření** > **OAuth souhlas obrazovky**, potom vyberte svoji **e-mailovou adresu**, zadejte **Název produktu**a klikněte na tlačítko **Uložit**.

5. Na kartě **přihlašovací údaje** klikněte na **vytvořit pověření** > **ID klienta OAuth**a pak vyberte **Webová aplikace**.

6. Vložte kterou jste si zkopírovali do **Oprávnění typů JavaScript**aplikaci služby **adresy URL** a potom vložte přesměrování URI do **Oprávnění přesměrovat URI**. Přesměrování URI je adresa URL aplikace přidaným s cestou _/.auth/login/google/callback_. Například `https://contoso.azurewebsites.net/.auth/login/google/callback`. Ujistěte se, že používáte schématu HTTPS. Klikněte na **vytvořit**.

7. Na další obrazovce si poznamenejte hodnoty ID klienta a tajná klienta.


    > [AZURE.IMPORTANT]
    Tajná klienta je důležité zabezpečení pověření. Sdílet tento tajná s kýmkoli nebo distribuce klientské aplikaci.


## <a name="secrets"> </a>Google přidat informace do aplikace

8. Zpátky v [Azure portál]přejděte do aplikace. Klikněte na **Nastavení**a potom **ověřování / se tak mohli ověřovat**.

9. Pokud ověření / se tak mohli ověřovat funkce není povolená, přepínač na **zapnuto**.

10. Klikněte na tlačítko **Google**. Vložte do pole ID aplikace a aplikace tajná hodnoty, které jste získali dříve a pokud chcete povolit obory, který vyžaduje vaše aplikace. Klikněte na **OK**.

    ![][1]

    Ve výchozím nastavení aplikace služba poskytuje ověřování, ale není omezit oprávnění přístup k rozhraní API a obsahem webu. Uživatelé musí povolit v aplikaci kódu.

17. (Volitelné) Pokud chcete omezit přístup k vašemu webu uživatelům jenom ověřené Google, nastavte **akce provedeny, pokud není ověřen požadavek** na **Google**. Tento postup vyžaduje, aby všechny žádosti o ověření a všechny žádosti o neověřených přesměrováni do Google pro ověřování.

12. Klikněte na **Uložit**.

Teď jste připraveni pro účely ověření do aplikace Google.

## <a name="related-content"> </a>Související obsah

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure portálu]: https://portal.azure.com/

