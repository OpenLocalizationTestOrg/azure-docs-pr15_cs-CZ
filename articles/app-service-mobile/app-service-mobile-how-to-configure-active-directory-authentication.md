<properties
    pageTitle="Konfigurace ověřování Azure Active Directory pro aplikaci služby aplikace"
    description="Zjistěte, jak nakonfigurovat ověřování Azure Active Directory pro aplikaci služby aplikace."
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
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

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>Jak nakonfigurovat aplikaci služby aplikace pomocí služby Azure Active Directory přihlášení

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

V tomto tématu se dozvíte, jak nakonfigurovat aplikaci služby Azure jako poskytovatele ověřování použít Azure Active Directory.

## <a name="express"> </a>Konfigurace Azure Active Directory pomocí Expresní nastavení

13. V [Azure portálu]přejděte do aplikace. Klikněte na **Nastavení**a potom na položku **Ověření a povolení**.

14. Pokud ověření / se tak mohli ověřovat funkce není povolená, přepínač na **zapnuto**.

15. Klikněte na **Azure Active Directory**a potom klepněte na **Express** **Režim správy**.

16. Klikněte na **OK** zaregistrujte aplikace v Azure Active Directory. Tím vytvoříte novou registraci. Pokud chcete místo toho zvolit existující registrací, klikněte na tlačítko **Vybrat existující aplikace** a vyhledejte název dříve vytvořené registrace v rámci vašeho tenanta.
Klikněte na registrace ho vyberte a klikněte na **OK**. Klikněte na **OK** na zásuvné nastavení Azure Active Directory.

    ![][0]

    Ve výchozím nastavení aplikace služba poskytuje ověřování, ale není omezit oprávnění přístup k rozhraní API a obsahem webu. Uživatelé musí povolit v aplikaci kódu.

17. (Volitelné) Pokud chcete omezit přístup k vašemu webu uživatelům jenom ověřené Azure Active Directory, nastavte **akce provedeny, pokud není ověřen žádosti o** přihlášení **pomocí služby Azure Active Directory**. Tento postup vyžaduje, aby všechny žádosti o ověření a všechny žádosti o neověřených přesměrováni pro službu Azure Active Directory pro ověřování.

17. Klikněte na **Uložit**.

Nyní jsou připravené k použití služby Azure Active Directory pro ověřování v aplikaci.

## <a name="advanced"> </a>(Alternativní metody) ruční konfigurace Azure Active Directory pomocí upřesňujících nastavení
Můžete také zadat konfigurace nastavení ručně. Toto je nejlepší řešení, pokud se liší od klienta, se kterým se přihlásíte k Azure AAD klienta, který chcete použít. Dokončete konfiguraci je nutné nejprve vytvořit registrace v Azure Active Directory a potom je nutné zadat část Podrobnosti registrace aplikace služby.

### <a name="register"> </a>Registrovat aplikaci služby Azure Active Directory

1. Přihlaste se k [portálu Azure]a přejděte do aplikace. Zkopírujte **adresu URL**. To se používá ke konfiguraci aplikace služby Azure Active Directory.

3. Přihlaste se k [Azure klasické portál] a přejděte do **Služby Active Directory**.

    ![][2]

4. Vyberte svůj adresář a potom na kartu **aplikace** v horní. V dolní části vytvořit nové registraci aplikace klikněte na **Přidat** .

5. Klikněte na **Přidat aplikaci, které vyvíjí mé organizaci**.

6. V průvodci Přidat aplikace zadejte **název** aplikace a klikněte na typ **Rozhraní API webových aplikací a či nebo Web** . Klikněte na pokračovat.

7. Do pole **Adresa URL na znaménko** vložte adresu URL aplikace, kterou jste si zkopírovali. Do pole **Identifikátor URI ID aplikace** zadejte tuto adresu URL. Klikněte na pokračovat.

8. Po přidání aplikace klikněte na kartu **Konfigurovat** . Upravte **Adresu URL odpovědět** ve skupinovém rámečku **jednotného přihlašování** má být adresa URL aplikace přidaným s cestou _/.auth/login/aad/callback_. Například `https://contoso.azurewebsites.net/.auth/login/aad/callback`. Ujistěte se, že používáte schématu HTTPS.

    ![][3]

9. Klikněte na **Uložit**. Zkopírujte **Kód klienta** aplikace. Nakonfigurujte aplikace: pomocí tohoto příkazu později.

10. V dolním panelu příkazů klikněte na **Zobrazení koncové body**a potom zkopírujte adresu URL **Dokumentu metadat federace** a stáhnout tento dokument nebo přejděte v prohlížeči.

11. V kořenovém prvku **EntityDescriptor** by měl být atribut **ID entit** formuláře `https://sts.windows.net/` následovaný GUID konkrétní ke tenantovi (označované jako "ID klienta"). Zkopírujte tuto hodnotu: bude sloužit jako **Adresa URL vydavatel**. Nakonfigurujte aplikace: pomocí tohoto příkazu později.

### <a name="secrets"> </a>Aplikaci informace přidat Azure Active Directory

13. Zpátky v [Azure portál]přejděte do aplikace. Klikněte na **Nastavení**a potom na položku **Ověření a povolení**.

14. Pokud funkce ověřování a povolení není povolená, vypněte přepínač na **zapnuto**.

15. Klikněte na **Azure Active Directory**a potom klikněte na **Upřesnit** v části **Režim správy**. Vložte hodnotu ID klienta a Vystavitel adresu URL, kterou jste získali dříve. Klikněte na **OK**.

    ![][1]

    Ve výchozím nastavení aplikace služba poskytuje ověřování, ale není omezit oprávnění přístup k rozhraní API a obsahem webu. Uživatelé musí povolit v aplikaci kódu.

17. (Volitelné) Pokud chcete omezit přístup k vašemu webu uživatelům jenom ověřené Azure Active Directory, nastavte **akce provedeny, pokud není ověřen žádosti o** přihlášení **pomocí služby Azure Active Directory**. Tento postup vyžaduje, aby všechny žádosti o ověření a všechny žádosti o neověřených přesměrováni pro službu Azure Active Directory pro ověřování.

17. Klikněte na **Uložit**.

Nyní jsou připravené k použití služby Azure Active Directory pro ověřování v aplikaci.

## <a name="optional-configure-a-native-client-application"></a>(Volitelné) Konfigurace aplikace native client –

Azure Active Directory také umožňuje registrovat původní klienti poskytující větší kontrolu nad oprávnění mapování. Musíte to, pokud chcete provést přihlášení pomocí knihovny například **Active Directory Authentication Library**.

1. Přejděte do **Služby Active Directory** [Azure klasické portálu].

2. Vyberte svůj adresář a potom na kartu **aplikace** v horní. V dolní části vytvořit nové registraci aplikace klikněte na **Přidat** .

3. Klikněte na **Přidat aplikaci, které vyvíjí mé organizaci**.

4. V průvodci Přidat aplikace zadejte **název** aplikace a klikněte na typ **Nativní klientské aplikaci** . Klikněte na pokračovat.

5. V dialogovém okně **Přesměrování URI** zadejte webu _/.auth/login/done_ koncový bod, pomocí schématu HTTPS. Tato hodnota by měl být stejný _https://contoso.azurewebsites.net/.auth/login/done_. Při vytváření aplikace pro systém Windows, použijte funkci [balíček ID zabezpečení](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) jako identifikátor URI.

6. Po přidání nativní aplikaci klikněte na kartu **Konfigurovat** . Nalezení **ID klienta** a poznamenejte si tuto hodnotu.

7. Posun o stránku dolů do části **oprávnění na jiné aplikace** a klikněte na **Přidat aplikaci**.

8. Vyhledávání pro webovou aplikaci, kterou jste registrovali dříve a klikněte na ikonu se znaménkem plus. Potom klikněte na zkontrolovat zavřete dialogové okno. Pokud webové aplikace nelze najít, přejděte k jeho registraci a přidejte nová adresa URL odpovědět (například HTTP verzi aktuální adresy URL), klikněte na Uložit a pak opakujte tento postup: aplikace má zobrazit v seznamu.

9. Na nové položky, kterou jste právě přidali otevřete rozevírací nabídky **Delegované oprávnění** a vyberte **přístup (název_aplikace)**. Klepněte na tlačítko **Uložit**.

Teď jste nakonfigurovali native client – aplikace, které můžete získat přístup k aplikaci služby aplikací.

## <a name="related-content"> </a>Související obsah

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure portálu]: https://portal.azure.com/
[Azure klasický portálu]: https://manage.windowsazure.com/
[alternative method]:#advanced
