<properties 
    pageTitle="Ověření pomocí mobilního zapojení REST API – ruční nastavení"
    description="Popisuje, jak ručně nastavit ověřování pro mobilní zapojení REST API" 
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo"
    manager="erikre"
    editor=""/>

<tags
    ms.service="mobile-engagement"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="mobile-multiple"
    ms.workload="mobile" 
    ms.date="08/19/2016"
    ms.author="piyushjo"/>

# <a name="authenticate-with-mobile-engagement-rest-apis---manual-setup"></a>Ověření pomocí mobilního zapojení REST API – ruční nastavení

Toto je dodatku dokumentace [Ověřit Mobile zapojení REST API](mobile-engagement-api-authentication.md)na. Zkontrolujte, že si přečíst nejdříve, aby se zobrazilo kontextu. Tento scénář vystihuje alternativní možné provést jednorázové instalace pro nastavení ověřování pro mobilní zapojení ZBÝVAJÍCÍ rozhraní API, na portálu Azure. 

>[AZURE.NOTE] Podle pokynů uvedených níže jsou založené na této [příručce služby Active Directory](../resource-group-create-service-principal-portal.md) a přizpůsobený co je nutný pro ověřování pro rozhraní API zapojení Mobile. Takže vztahující se k němu chcete se dozvědět kroků podrobně. 

1. Přihlaste se ke svému účtu Azure pomocí [klasické portálu](https://manage.windowsazure.com/).

2. V levém podokně vyberte **Služby Active Directory** .

     ![Vyberte služby Active Directory][1]

3. Zvolte **Výchozí Active Directory** v portálu Azure. 

     ![Zvolte adresář][2]

    >[AZURE.IMPORTANT] Tento postup lze použít pouze v případě, že pracujete v výchozí Active Directory svůj účet a nebude fungovat, pokud je to ve službě Active Directory, kterou jste vytvořili ve vašem účtu. 

4. Zobrazení aplikace v adresáři, klikněte na **aplikace**.

     ![zobrazení aplikace][3]

5. Klikněte na **Přidat**. 

     ![Přidat aplikaci][4]

6. Klikněte na **Přidat aplikaci, které vyvíjí organizaci**

     ![nové aplikace][5]

6. Zadejte název aplikace a vyberte požadovaný typ aplikace jako **Rozhraní API webových aplikací a či nebo WEB** a klepněte na tlačítko Další.

     ![název aplikace][6]

7. Zadání všech formální adresy URL pro identifikátory **URI ID aplikace**a **Na PŘIHLAŠOVACÍ adresa URL** . Nejsou použity pro naše scénář a nejsou ověření adresy URL sami.  

     ![vlastnosti aplikace][7]

8. Na konci to budete mít aplikaci AAD nahraďte názvem, který jste dříve použili jako následující. Toto je váš **AD\_aplikace\_název** a poznamenejte si ji.  

     ![název aplikace][8]

9. Klikněte na název aplikace a klikněte na **Konfigurovat**.

     ![Konfigurace aplikace][9]

10. Poznamenejte si ID klienta, který bude sloužit jako **klienta\_ID** vaše rozhraní API volá. 

     ![Konfigurace aplikace][10]

11. Přejděte dolů do části **klíče** a přidejte klíč s dobou trvání nejlépe dva roky (vypršení platnosti) a klikněte na tlačítko **Uložit**. 

     ![Konfigurace aplikace][11]


12. Ihned zkopírujte hodnotu, která se zobrazuje pro klávesu tak, jak se zobrazí pouze teď a nebude uloženo tak, aby se někdy znovu nezobrazí. Pokud dojde ke ztrátě budete muset generovat nový klíč. To bude **CLIENT_SECRET** pro rozhraní API volání. 

     ![Konfigurace aplikace][12]

    >[AZURE.IMPORTANT] Tento klíč vyprší na konci doby trvání, který jste zadali, ujistěte se, že ho obnovit, když nastane čas jinak rozhraní API ověřování přestal fungovat. Můžete taky odstranit a znovu tento klíč, pokud si myslíte, že má ohroženo.
 
13. Klikněte na tlačítko **Zobrazit koncové body** nyní otevřete dialogové okno **Aplikace koncové body** . 

    ![][13]

14. V dialogovém okně koncové body aplikace zkopírujte **Koncový bod 2.0 TOKENU OAUTH**. 

    ![][14]

15. Tento koncový bod budete mít ve formuláři následující kde GUID v adrese URL je **TENANT_ID** tak poznamenejte si ho: 

        https://login.microsoftonline.com/<GUID>/oauth2/token

16. Teď můžeme pokračujte konfiguraci oprávnění této aplikace. U to bude mít otevřete [Azure portálu](https://portal.azure.com). 

17. Klikněte na **Skupiny zdrojů** a najděte skupinu zdroje **Mobile Engagement** .  

    ![][15]

18. Klikněte na skupina zdroje **Můžete zapojit Mobile** a přejděte na zásuvné **Nastavení** tady. 

    ![][16]

19. Klikněte na **uživatele** do zásuvné nastavení a potom klikněte na **Přidat** přidáte uživatele. 

    ![][17]

20. Klikněte na **Vybrat roli**

    ![][18]

21. Klikněte na **vlastníka**

    ![][19]

22. Vyhledejte název aplikace **AD\_aplikace\_název** do vyhledávacího pole. Se nezobrazí ve výchozím nastavení tady. Jakmile ho najdete, vyberte ho a klikněte na **Vybrat** v dolní části zásuvné. 

    ![][20]

23. Na zásuvné **Přidat Access** zobrazí jako **1 uživatel, 0 skupiny**. Klikněte na **OK** v této zásuvné potvrďte změny. 

    ![][21]

Teď jste dokončili požadovaná konfigurace AAD a jsou všechny nastavíte na volat rozhraní API. 

<!-- Images -->
[1]: ./media/mobile-engagement-api-authentication-manual/active-directory.png
[2]: ./media/mobile-engagement-api-authentication-manual/active-directory-details.png
[3]: ./media/mobile-engagement-api-authentication-manual/view-applications.png
[4]: ./media/mobile-engagement-api-authentication-manual/add-icon.png
[5]: ./media/mobile-engagement-api-authentication-manual/what-do-you-want-to-do.png
[6]: ./media/mobile-engagement-api-authentication-manual/tell-us-about-your-application.png
[7]: ./media/mobile-engagement-api-authentication-manual/app-properties.png
[8]: ./media/mobile-engagement-api-authentication-manual/aad-app.png
[9]: ./media/mobile-engagement-api-authentication-manual/configure-menu.png
[10]: ./media/mobile-engagement-api-authentication-manual/client-id.png
[11]: ./media/mobile-engagement-api-authentication-manual/client_secret.png
[12]: ./media/mobile-engagement-api-authentication-manual/keys.png
[13]: ./media/mobile-engagement-api-authentication-manual/view-endpoints.png
[14]: ./media/mobile-engagement-api-authentication-manual/app-endpoints.png
[15]: ./media/mobile-engagement-api-authentication-manual/resource-groups.png
[16]: ./media/mobile-engagement-api-authentication-manual/resource-groups-settings.png
[17]: ./media/mobile-engagement-api-authentication-manual/add-users.png
[18]: ./media/mobile-engagement-api-authentication-manual/add-role.png
[19]: ./media/mobile-engagement-api-authentication-manual/select-role.png
[20]: ./media/mobile-engagement-api-authentication-manual/add-user-select.png
[21]: ./media/mobile-engagement-api-authentication-manual/add-access-final.png



