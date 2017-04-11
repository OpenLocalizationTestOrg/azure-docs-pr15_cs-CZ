<properties
    pageTitle="Postup při konfiguraci služby Facebook ověřování aplikace aplikace služby"
    description="Zjistěte, jak nakonfigurovat Facebook ověřování pro aplikaci služby aplikace."
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>Jak nakonfigurovat aplikaci služby aplikace pomocí služby Facebook přihlášení

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

V tomto tématu se dozvíte, jak nakonfigurovat aplikaci služby Azure jako poskytovatele ověřování použít Facebook.

Postupujte podle kroků v tomto tématu, pokud nemáte účet služby Facebook, který má ověřenou e-mailovou adresu a číslo mobilního telefonu. Pokud chcete vytvořit nový účet služby Facebook, přejděte na [facebook.com].

## <a name="register"> </a>Registrovat aplikaci Facebook

1. Přihlaste se k [portálu Azure]a přejděte do aplikace. Zkopírujte **adresu URL**. To se používá ke konfiguraci aplikace Facebook.

2. V jiném okně prohlížeče přejděte na web [Služby Facebook vývojářů] a přihlaste se pomocí svých přihlašovacích údajů účtu Facebooku.

3. (Volitelné) Pokud jste to ještě už zaregistrovali, klikněte na **aplikace** > **zaregistrovat jako vývojář**, pak přijmout zásadu a postupujte podle kroků registrace.

4. Klikněte na **Moje aplikace** > **Přidat novou aplikaci** > **webu** > **Přeskočit a vytvořte ID aplikace**. 

5. Do pole **Zobrazovaný název**zadejte jedinečný název aplikace, zadejte **E-mailu kontaktu**, vyberte **kategorii** aplikaci, klikněte **Vytvoření ID aplikace** a dokončete kontrola zabezpečení. Tím přejdete na řídicí panel pro vývojáře pro novou aplikaci Facebook.

6. V části "Facebook přihlášení," klikněte na **Začínáme**. Přidání aplikace **Přesměrovat URI** **platné OAuth**přesměrovat URI a potom klikněte na **Uložit změny**. 

    > [AZURE.NOTE] Přesměrování URI je adresa URL aplikace přidaným s cestou _/.auth/login/facebook/callback_. Například `https://contoso.azurewebsites.net/.auth/login/facebook/callback`. Ujistěte se, že používáte schématu HTTPS.

6. V levém navigačním panelu klikněte na **Nastavení**. V poli **Tajná aplikace** klikněte na **Zobrazit**, zadejte svoje heslo požaduje a nakonec poznamenejte si hodnoty **ID aplikace** a **Aplikace tajná**. Pomocí těchto později nakonfigurovat aplikaci v Azure.

    > [AZURE.IMPORTANT] Tajná aplikace je důležité zabezpečení pověření. Sdílet tento tajná s kýmkoli nebo distribuce klientské aplikaci.

7. Facebookový účet, který byl použit k registraci aplikace je správce aplikace. V tomto okamžiku pouze správci můžou přihlásit do této aplikace. Ověření dalších účtů Facebook, klikněte na kartu **Revize aplikace** a povolení **zveřejnit < vaše aplikace název - >** obecné veřejně přístupné pomocí ověřování Facebook.

## <a name="secrets"> </a>Informace o přidání Facebooku do aplikace

1. Zpátky v [Azure portál]přejděte do aplikace. Klikněte na **Nastavení** > **ověřování / se tak mohli ověřovat**a ujistěte se **, zda je **Služba ověřování aplikace** **.

2. Klikněte na **Facebooku**, vložte do pole ID aplikace a aplikace tajná hodnoty, které jste získali dříve, pokud chcete povolit všechny obory potřeby tak, že aplikace a pak klikněte na **OK**.

    ![][0]

    Ve výchozím nastavení aplikace služba poskytuje ověřování, ale není omezit oprávnění přístup k rozhraní API a obsahem webu. Uživatelé musí povolit v aplikaci kódu.

3. (Volitelné) Pokud chcete omezit přístup k vašemu webu uživatelům jenom ověřené Facebook, nastavte **akce provedeny, pokud není ověřen požadavek** na **Facebook**. Tento postup vyžaduje, aby všechny žádosti o ověření a všechny žádosti o neověřených přesměrováni na Facebook pro ověřování.

4. Po dokončení konfigurace ověřování, klikněte na **Uložit**.

Teď jste připraveni pro účely ověření v aplikaci Facebook.

## <a name="related-content"> </a>Související obsah

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Vývojáři Facebooku]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[Facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure portálu]: https://portal.azure.com/
