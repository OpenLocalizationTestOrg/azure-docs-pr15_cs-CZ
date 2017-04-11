<properties
    pageTitle="Postup při konfiguraci Account Microsoft ověřování aplikace aplikace služby"
    description="Zjistěte, jak nakonfigurovat Account Microsoft ověřování pro aplikaci služby aplikace."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>Jak nakonfigurovat aplikaci služby aplikace pomocí přihlašování pomocí zvláštního Account společnosti Microsoft

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

V tomto tématu se dozvíte, jak nakonfigurovat aplikaci služby Azure používat Account Microsoft jako poskytovatele ověřování. 

## <a name="register-microsoft-account"> </a>Zaregistrovat aplikaci pomocí účtu Microsoft

1. Přihlaste se k [portálu Azure]a přejděte do aplikace. Zkopírujte vaše **Adresa URL**, které později používá ke konfiguraci aplikace pomocí Account Microsoft.

2. Přejděte na stránku [Moje aplikace] na webu Microsoft Account Developer Center a se přihlaste pomocí účtu Microsoft, pokud je to nutné.

3. Klikněte na **Přidat aplikaci**, potom zadejte název aplikace a klikněte na **vytvořit aplikaci**.

4. Poznamenejte si **ID aplikace**, jak budete ho později potřebovat. 

5. V části "Platformy," klikněte na **Přidat platformy** a vyberte "Web".

6. V části "URI přesměrování" zadat koncový bod pro aplikace a potom klikněte na **Uložit**. 
 
    >[AZURE.NOTE]Přesměrování URI je adresa URL aplikace přidaným s cestou _/.auth/login/microsoftaccount/callback_. Například `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.   
    >Ujistěte se, že používáte schématu HTTPS.

7. V části "Aplikace tajemství," klikněte na **Generovat nové heslo**. Poznamenejte si hodnota, která se zobrazí. Po opuštění stránky, nezobrazí se znovu.


    > [AZURE.IMPORTANT] Heslo, je důležité zabezpečení pověření. Sdílet s kýmkoli heslo nebo distribuce klientské aplikaci.

## <a name="secrets"> </a>Aplikaci aplikaci služby informace přidat účet Microsoft

1. Zpátky v [Azure portál], přejděte do aplikace, klikněte na **Nastavení** > **ověřování / se tak mohli ověřovat**.

2. Pokud ověření / se tak mohli ověřovat funkce není povolená, přepněte **na**.

3. Klikněte na **Účet Microsoft**. Vložte do pole ID aplikace a heslo hodnoty, které jste získali dříve a pokud chcete povolit oborů, který vyžaduje vaše aplikace. Klikněte na **OK**.

    ![][1]

    Ve výchozím nastavení aplikace služba poskytuje ověřování, ale není omezit oprávnění přístup k rozhraní API a obsahem webu. Uživatelé musí povolit v aplikaci kódu.

4. (Volitelné) Pokud chcete omezit přístup k vašemu webu uživatelům jenom ověření pomocí účtu Microsoft, nastavte **akce provedeny, pokud není požadavek ověřit** s **Účtem Microsoft**. Tento postup vyžaduje, aby všechny žádosti o ověření a všechny žádosti o neověřených přesměrováni na účet Microsoft pro ověřování.

5. Klikněte na **Uložit**.

Teď jste připraveni pro účely ověření v aplikaci Microsoft Account.

## <a name="related-content"> </a>Související obsah

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Moje aplikace]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure portálu]: https://portal.azure.com/
