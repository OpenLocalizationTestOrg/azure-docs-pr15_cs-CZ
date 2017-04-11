<properties
    pageTitle="Ověřování uživatelů pro rozhraní API aplikace v aplikaci služby Azure | Microsoft Azure"
    description="Zjistěte, jak chránit aplikace rozhraní API aplikace služby Azure tím, že povolí přístup jenom pro ověřené uživatele."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="dotnet"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/30/2016"
    ms.author="rachelap"/>

# <a name="user-authentication-for-api-apps-in-azure-app-service"></a>Ověřování uživatelů pro rozhraní API aplikace v aplikaci služby Azure

## <a name="overview"></a>Základní informace

Tento článek ukazuje, jak chránit aplikace rozhraní API Azure tak, aby jenom ověřené uživatele můžete zavolejte ho. V článku se předpokládá, že jste přečetli jste si téma [Přehled ověřování aplikace služby Azure](../app-service/app-service-authentication-overview.md).

Se dozvíte:

* Jak nastavit poskytovatele ověřování s podrobnostmi pro službu Azure Active Directory (Azure AD).
* Informace o používání chráněných rozhraní API aplikace pomocí [Active Directory ověřování knihovny (ADAL) JavaScriptu](https://github.com/AzureAD/azure-activedirectory-library-for-js).

Tento článek obsahuje dva oddíly:

* V části [jak nakonfigurovat ověřování uživatele v aplikaci služby Azure](#authconfig) je popsáno obecně konfigurace ověření uživatele pro nějakou aplikaci rozhraní API a rovnoměrně z vnější platí pro všechny rámce nepodporuje aplikaci služby včetně .NET, Node.js a Java.

* Počínaje [pokračováním výukové programy pro .NET rozhraní API aplikace](#tutorialstart) části vodítka článek vás provede konfiguraci aplikace vzorku s .NET serverová a AngularJS přední konce tak, aby používala služby Azure Active Directory pro ověřování uživatelů. 

## <a id="authconfig"></a>Postup při konfiguraci ověřování uživatele v aplikaci služby Azure

Tato část obsahuje obecné pokyny, které se vztahují k aplikaci rozhraní API. Postup konkrétní ukázková aplikace pro .NET seznamu proveďte přejděte k [dalším výukové programy pro .NET Začínáme –](#tutorialstart).

1. [Azure portál](https://portal.azure.com/), přejděte na zásuvné **Nastavení** rozhraní API aplikace, kterou chcete chránit, najdete v části **funkce** a klikněte na **ověřování / se tak mohli ověřovat**.

    ![Azure portál ověřování a ověření](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. V **ověřování / se tak mohli ověřovat** zásuvné, klikněte **na**.

4. Vyberte jednu z možností v seznamu **akce provedeny, pokud není požadavek ověřit** rozevíracího seznamu.

    * Pokud chcete jenom ověřené hovory kontaktovat rozhraní API aplikace, vyberte jednu z možností **přihlášení …** . Tato možnost umožňuje chránit rozhraní API aplikace bez psaní jakýkoli kód, který běží v nich.

    * Pokud chcete všechna volání kontaktovat rozhraní API aplikace, vyberte **Povolit požadavek (žádná akce)**. Tato možnost umožňuje přímé neověřených volajícím s dotazem, jestli zprostředkovatelů ověřování systému. Pomocí této možnosti musíte kódu pro zpracování se tak mohli ověřovat.

    Další informace najdete v tématu [a tak mohli ověřovat pro rozhraní API aplikace v aplikaci služby Azure](app-service-api-authentication.md#multiple-protection-options).

5. Vyberte jeden nebo více **Zprostředkovatelů ověřování**.

    Obrázek znázorňuje možnosti, které vyžadují všechny volající ověřil Azure AD.
 
    ![Azure portálu zásuvné ověřování a ověření](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

    Pokud vyberete poskytovatele ověřování, zobrazí portálu zásuvné konfigurace poskytovatele. 

    Podrobné pokyny, které popisují, jak nakonfigurovat listy konfigurace poskytovatele ověřování najdete v článku [jak nakonfigurovat aplikaci služby aplikace pomocí služby Azure Active Directory přihlášení](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). (Přejde na odkaz na článek o Azure AD, ale samotný článek obsahuje karty, které odkazují na články pro ostatní zprostředkovatelé ověřování.)

7. Až skončíte s zásuvné konfigurace poskytovatele ověřování, klikněte na **OK**.

7. V **ověřování / se tak mohli ověřovat** zásuvné, klikněte na tlačítko **Uložit**.

Když to uděláte, aplikaci služby ověří všechny volání rozhraní API před dosáhla rozhraní API aplikace. Ověřovacích služeb práce pro všechny jazyky, které služba aplikace podporuje, včetně .NET, Node.js a Java stejné. 

Pro hovory ověřené rozhraní API, volající zahrnuje nosný token OAuth 2.0 zprostředkovatele ověřování se tak mohli ověřovat záhlaví požadavků HTTP. Tokenu lze získat pomocí zprostředkovatele ověřování SDK.

## <a id="tutorialstart"></a>Pokračování výukové programy pro .NET rozhraní API aplikace

Pokud sledujete Node.js nebo Java výukové programy pro rozhraní API aplikace, přejděte na další článek [služby základní ověřování pro rozhraní API aplikace](app-service-api-dotnet-service-principal-auth.md). 

Pokud sledují řadě kurzů .NET pro rozhraní API aplikace a jste již nainstalovali ukázková aplikace podle pokynů v [prvním](app-service-api-dotnet-get-started.md) a [druhým](app-service-api-cors-consume-javascript.md) kurzech, přejděte k části [Nastavení ověřování v aplikaci služby a Azure AD](#azureauth) .

Pokud chcete sledovat tento kurz bez opakovaného výukové programy pro první a druhé, proveďte následující kroky, které popisují, jak začít pomocí automatického procesu nasazení aplikace vzorku.

>[AZURE.NOTE] Podle těchto kroků dostanete k ze stejného počátečního bodu jako, pokud jste prvních dvou kurzy s jednou výjimkou: Visual Studio nebude už měli znát které webových aplikací nebo rozhraní API aplikace, který získá používaný jednotlivé projekty. To znamená, že nebudete mít přesné pokyny v tomto kurzu nasadit správné cílů. Pokud si nejste nevyhovuje zjistit, jak se to kroků nasazení si vlastní, je lepší sledování řadě kurzů z [první kurz](app-service-api-dotnet-get-started.md) než začněte u tento proces automatického nasazení.

1. Ujistěte se, že máte všechny požadavky uvedené v [první kurz](app-service-api-dotnet-get-started.md). Kromě předpoklady uvedené tyto výukové programy pro ověřování předpokládá, že jste pracovali s aplikací web apps a rozhraní API aplikace ve Visual Studiu a portálu Azure.

2. Klikněte na tlačítko **instalovat na Azure** v [souboru readme úložiště ukázka se seznamem úkolů](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/readme.md) pro nasazení aplikace rozhraní API a web app. Poznamenejte si skupiny Azure zdroje, které získá vytvořen, protože tímto způsobem můžete později vyhledat v prohlížeči a rozhraní API aplikace názvy.
 
3. Můžete si stáhnout nebo klonovat [úložiště ukázka se seznamem](https://github.com/Azure-Samples/app-service-api-dotnet-todo-list) získat kód, který budete spolupracujete místně ve Visual Studiu.

## <a id="azureauth"></a>Nastavení ověřování v aplikaci služby a Azure AD

Nyní máte aplikace spuštěné v aplikaci služby Azure bez nutnosti ověření uživatele. V této části Přidání ověřování můžete udělat následující úkoly:

* Nakonfigurujte aplikaci služby vyžadovat ověřování služby Azure Active Directory (Azure AD) pro volání střední úroveň rozhraní API aplikace.
* Vytvoření aplikace Azure AD.
* Konfigurace aplikace Azure AD odešlete nosný token po přihlášení AngularJS front-end. 

Pokud dostanete do potíží při výuková pokynů naleznete v části [Poradce při potížích](#troubleshooting) na konci kurzu. 
 
### <a name="configure-authentication-for-the-middle-tier-api-app"></a>Konfigurace ověřování rozhraní API aplikace střední vrstvy

1. [Azure portál](https://portal.azure.com/), přejděte na zásuvné **Nastavení** rozhraní API aplikace, kterou jste vytvořili ToDoListAPI projektu, najděte v části **funkce** a klikněte na **ověřování / se tak mohli ověřovat**.

    ![Azure portál ověřování a ověření](./media/app-service-api-dotnet-user-principal-auth/features.png)

3. V **ověřování / se tak mohli ověřovat** zásuvné, klepněte **na**.

4. V seznamu **akce provedeny, pokud není požadavek ověřit** vyberte **přihlásit pomocí účtu služby Azure Active Directory**.

    Tato možnost zajišťuje, že žádné žádosti o unauathenticated dosáhne rozhraní API aplikace. 

5. V části **Zprostředkovatelů ověřování**klikněte na **Azure Active Directory**.

    ![Azure portálu zásuvné ověřování a ověření](./media/app-service-api-dotnet-user-principal-auth/authblade.png)

6. V **Nastavení služby Azure Active Directory** zásuvné klikněte na **Express**

    ![Možnost Azure ověřování a povolení Express portálu](./media/app-service-api-dotnet-user-principal-auth/aadsettings.png)

    S parametrem **Express** aplikaci služby automaticky vytvořit aplikaci Azure AD Azure AD [klienta](https://msdn.microsoft.com/en-us/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant). 

    Nemusíte vytvoření klienta, protože každý účet Azure automaticky nějaký použitý.

7. Ve skupinovém rámečku **režim správy**Pokud není vybrána, klikněte na **Vytvořit novou aplikaci AD** a Všimněte si hodnotu, která je do textového pole **Vytvořit aplikace** ; budete vyhledat této AAD aplikace na portálu Azure klasické později.

    ![Nastavení Azure portálu Azure AD](./media/app-service-api-dotnet-user-principal-auth/aadsettings2.png)

    Azure automaticky vytvoří aplikace Azure AD určené názvem ve vašem klientovi Azure AD. Ve výchozím nastavení aplikace Azure AD se stejným názvem jako rozhraní API aplikace. Pokud chcete, můžete zadat jiný název.
 
7. Klikněte na **OK**.

7. V **ověřování / se tak mohli ověřovat** zásuvné, klikněte na tlačítko **Uložit**.

    ![Klikněte na tlačítko Uložit](./media/app-service-api-dotnet-user-principal-auth/authsave.png)

Pouze uživatelům Azure AD klienta teď můžete volat rozhraní API aplikace.

### <a name="optional-test-the-api-app"></a>Volitelné: Otestujte rozhraní API aplikace

1. V prohlížeči přejděte na adresu URL rozhraní API aplikace: v zásuvné **rozhraní API aplikace** na portálu Azure, klikněte na odkaz v části **Adresa URL**.  

    Budete přesměrováni na přihlašovací obrazovce, protože neověřené požadavky nejsou povolené kontaktovat rozhraní API aplikace.

    Pokud váš prohlížeč přejde na stránku "úspěšně vytvořeného", prohlížeči může být připojen – v takovém případě otevřete okno InPrivate nebo okno a přejít na adresu URL rozhraní API aplikace.

2. Přihlaste se pomocí přihlašovacích údajů pro uživatele ve vašem klientovi Azure AD.

    Pokud jste přihlášeni, "úspěšně vytvořené" stránce zobrazená v prohlížeči.

9. Zavřete prohlížeč.

### <a name="configure-the-azure-ad-application"></a>Konfigurace aplikace Azure AD

Při konfiguraci Azure AD ověřování vytvoří aplikaci služby Azure AD aplikace. Ve výchozím nastavení nové Azure AD o konfiguraci aplikace nosný token poskytovat URL rozhraní API aplikace. V této části nakonfigurovat aplikaci Azure AD poskytnout nosný token do AngularJS přední konce namísto přímo na střední úroveň rozhraní API aplikace. AngularJS front-end odešle tokenu do aplikace rozhraní API při volání rozhraní API aplikace.

>[AZURE.NOTE] Chcete-li zachovat procesu jako jednoduché možnou, tento kurz používá jeden Azure AD úroveň žádost o front-end a uprostřed rozhraní API aplikace. Další možností je používat dvě Azure AD aplikace. V takovém případě by potřeba udělit oprávnění aplikace Azure AD přední konce volání aplikace Azure AD střední úroveň. Tento přístup více aplikace by vám přesnější kontrolu nad oprávnění pro každou úroveň.

11. V [Azure klasické portál](https://manage.windowsazure.com/)přejděte na **Azure Active Directory**.

    Budete muset používat portál klasické, protože některá nastavení Azure Active Directory, které musíte mít přístup k ještě nejsou k dispozici v aktuálním Azure portálu.

12. Na kartě **adresář** klikněte na AAD klienta.

    ![Azure AD klasické portálu](./media/app-service-api-dotnet-user-principal-auth/selecttenant.png)

14. Klikněte na **aplikace > aplikace naší společnosti vlastní**a klikněte na značku zaškrtnutí.

    Budete taky muset aktualizovat stránku, abyste viděli novou aplikaci.

15. V seznamu aplikací klepněte na název ten, který Azure vytvoří když povolíte ověřování pro rozhraní API aplikace.

    ![Karta Azure AD aplikace](./media/app-service-api-dotnet-user-principal-auth/appstab.png)

16. Klikněte na **Konfigurovat**.

    ![Karta Azure AD konfigurovat](./media/app-service-api-dotnet-user-principal-auth/configure.png)

17. **Adresa URL přihlašování** nastavená na adresa URL pro webovou aplikaci AngularJS, bez koncových lomítko.

    Příklad: https://todolistangular.azurewebsites.net

    ![Adresa URL přihlašování](./media/app-service-api-dotnet-user-principal-auth/signonurlazure.png)

17. **Adresa URL odpovědět** nastavená na adresa URL pro webovou aplikaci, bez koncových lomítko.

    Příklad: https://todolistsangular.azurewebsites.net

16. Klikněte na **Uložit**.

    ![Adresa URL odpověď](./media/app-service-api-dotnet-user-principal-auth/replyurlazure.png)

15. V dolní části stránky klikněte na **manifestu spravovat > stáhnout manifestu**.

    ![Stažení manifestu](./media/app-service-api-dotnet-user-principal-auth/downloadmanifest.png)

17. Stáhněte si soubor na místo, kde se dají upravovat.

16. V seznamu stažený soubor vyhledejte `oauth2AllowImplicitFlow` vlastnost. Změna hodnoty této vlastnosti z `false` k `true`a potom soubor uložte.

    Toto nastavení je nutný pro access z aplikace jednostránkové JavaScript. Umožňuje nosnému tokenu Oauth 2.0 budou vráceny v část adresy URL.

16. Klikněte na **manifestu spravovat > nahrát manifestu**a nahrajte soubor, který jste změnili v předchozím kroku.

    ![Nahrání manifestu](./media/app-service-api-dotnet-user-principal-auth/uploadmanifest.png)

17. Zkopírujte hodnotu **ID klienta** a uložte jej někde, kterou si můžete z později.

## <a name="configure-the-todolistangular-project-to-use-authentication"></a>Konfigurace project ToDoListAngular ověřování

V této části Změna AngularJS front-end tak, aby používala Active Directory Authentication knihovny (ADAL) pro JS získat nosný token přihlášený uživatel z Azure AD. Kód, bude token v žádosti o HTTP odeslané do druhého osy, jak je znázorněno na následujícím obrázku. 

![Diagram ověřování uživatele](./media/app-service-api-dotnet-user-principal-auth/appdiagram.png)

Proveďte následující změny souborů v aplikaci project ToDoListAngular.

1. Otevřete soubor *index.html* .

2. Zrušte komentář řádky, které odkazují Active Directory ověřování knihovny (ADAL) JS skriptů.

        <script src="app/scripts/adal.js"></script>
        <script src="app/scripts/adal-angular.js"></script>

1. Otevřete soubor *app/scripts/app.js* .

2. Poznámky bloku kódu pro "ověřování bez" a zrušte komentář bloku kódu pro "ověřování s".

    Tato změna odkazuje zprostředkovatele ADAL JS ověřování a obsahuje hodnoty konfigurace k němu. V následujících krocích je nastavit hodnoty konfigurace pro rozhraní API aplikace a aplikace Azure AD.

8. Kód, který se nastaví `endpoints` proměnná, nastavit adresu URL rozhraní API adresu URL rozhraní API aplikace vytvořené ToDoListAPI projektu a nastavte ID aplikace Azure AD ID klienta, kterou jste zkopírovali z portálu Microsoft Azure klasické.

    Podobně jako v následujícím příkladu je kód.

        var endpoints = {
            "https://todolistapi0121.azurewebsites.net/": "1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3"
        };

9. Při volání `adalProvider.init`, nastavte `tenant` na název klienta a `clientId` stejnou hodnotu jste použili v předchozím kroku.

    Podobně jako v následujícím příkladu je kód.

        adalProvider.init(
            {
                instance: 'https://login.microsoftonline.com/', 
                tenant: 'contoso.onmicrosoft.com',
                clientId: '1cf55bc9-9ed8-4df31cf55bc9-9ed8-4df3',
                extraQueryParameter: 'nux=1',
                endpoints: endpoints
            },
            $httpProvider
            );

    Tyto změny u `app.js` určit, volající kód a označovaných rozhraní API budou ve stejné Azure AD aplikaci.

1. Otevřete soubor *app/scripts/homeCtrl.js* .

2. Poznámky bloku kódu pro "ověřování bez" a zrušte komentář bloku kódu pro "ověřování s".

1. Otevřete soubor *app/scripts/indexCtrl.js* .

2. Poznámky bloku kódu pro "ověřování bez" a zrušte komentář bloku kódu pro "ověřování s".

### <a name="deploy-the-todolistangular-project-to-azure"></a>Nasazení projektu ToDoListAngular Azure

8. V **Okně Průzkumník řešení**klikněte pravým tlačítkem ToDoListAngular projektu a potom klikněte na **Publikovat**.

9. Klikněte na **Publikovat**.

    Visual Studio nasazuje projektu a otevře prohlížeč základní adresu URL webové aplikace. Tím se zobrazí 403 chybovou stránku, která je normální při pokusu o přejít na rozhraní API webových základní adresu URL z prohlížeče.

    Pořád máte měnit rozhraní API aplikace střední úroveň chcete-li otestovat aplikace.

10. Zavřete prohlížeč.

## <a name="configure-the-todolistapi-project-to-use-authentication"></a>Konfigurace project ToDoListAPI ověřování

Aktuálně projektu ToDoListAPI odešle "*" jako `owner` hodnotu ToDoListDataAPI. V této části změňte kód odeslat ID přihlášený uživatel.

Proveďte následující změny v projektu ToDoListAPI.

1. Otevřete soubor *Controllers/ToDoListController.cs* a zrušte komentář řádku v obou metod akci, která nastavuje `owner` Azure AD `NameIdentifier` hodnotu deklarace. Příklad:

        owner = ((ClaimsIdentity)User.Identity).FindFirst(ClaimTypes.NameIdentifier).Value;

    **Důležité**: není zrušte komentář s kódem v `ToDoListDataAPI` metody. budete uděláte později výukové služby základní ověřování.

### <a name="deploy-the-todolistapi-project-to-azure"></a>Nasazení projektu ToDoListAPI Azure

8. V **Okně Průzkumník řešení**klikněte pravým tlačítkem ToDoListAPI projektu a potom klikněte na **Publikovat**.

9. Klikněte na **Publikovat**.

    Visual Studio nasadí projektu a otevře prohlížeč rozhraní API aplikace základní adresu URL.

10. Zavřete prohlížeč.

### <a name="test-the-application"></a>Testování aplikace

9. Přejděte na adresu URL webové aplikace **pomocí HTTPS, ne HTTP**.

8. Klikněte na kartu **Se seznamem úkolů** .

    Zobrazí se výzva k přihlášení.

9. Přihlaste se pomocí přihlašovací údaje uživatele ve vašem klientovi AAD.

10. Zobrazí se stránka **Se seznamem úkolů** .

    ![Seznam stránek](./media/app-service-api-dotnet-user-principal-auth/webappindex.png)

    Žádné položky k vyřízení zobrazují vzhledem k tomu, dokud nyní všechny byly vlastníka "*". Teď střední úroveň žádá položek pro přihlášeného uživatele a žádná byly vytvořeny ještě.

11. Přidání nové položky k vyřízení ověřte funkčnost aplikace.

12. V jiném okně prohlížeče, přejděte na adresu URL uživatelského rozhraní Swagger ToDoListDataAPI rozhraní API aplikace a klikněte na **seznam úkolů > získat**. Napište hvězdičku pro `owner` parametr a potom klikněte na **vyzkoušet si**.

    Odpověď ukazuje, že skutečné nových položek úkolů Azure AD ID uživatele ve vlastnosti vlastník.

    ![ID vlastníka JSON odpověď](./media/app-service-api-dotnet-user-principal-auth/todolistapiauth.png)


## <a name="building-the-projects-from-scratch"></a>Vytváření projektů úplně od začátku

Dva projekty rozhraní API webových vytvořená pomocí šablony **Azure rozhraní API aplikace** project a nahrazení výchozího hodnoty řadiče řadiči seznam úkolů. 

Informace o tom, jak vytvořit aplikaci jednostránkové AngularJS s 2 rozhraní API webových back-end najdete v tématu [ruce v laboratoři: sestavení jedné stránce aplikace (zabezpečené ověřování HESLA) pomocí rozhraní API webových ASP.NET a Angular.js](http://www.asp.net/web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs). Informace o tom, jak přidat Azure AD ověřovací kód najdete v následujících zdrojích:

* [Zabezpečení AngularJS jedné stránky aplikací s Azure AD](../active-directory/active-directory-devquickstarts-angular.md).
* [Úvodní informace o v1 ADAL JS](http://www.cloudidentity.com/blog/2015/02/19/introducing-adal-js-v1/)

## <a name="troubleshooting"></a>Řešení potíží

[AZURE.INCLUDE [troubleshooting](../../includes/app-service-api-auth-troubleshooting.md)]

* Ujistěte se, že jste Nezaměňujte ToDoListAPI (střední vrstvy) a ToDoListDataAPI (osy dat). Zkontrolujte například přidána ověřování do aplikace střední úroveň rozhraní API není osy dat. 
* Ujistěte se, že zdrojový kód AngularJS odkazuje střední úroveň rozhraní API aplikace adresu URL (ToDoListAPI, ne ToDoListDataAPI) a správné Azure AD klienta ID. 

## <a name="next-steps"></a>Další kroky

V tomto kurzu jste se naučili jak používat aplikaci služby ověřování aplikace rozhraní API pro a jak volat rozhraní API aplikace pomocí knihovnu ADAL JS. V dalším kurzu se dozvíte jak [zabezpečený přístup k rozhraní API aplikace služby service scénářích](app-service-api-dotnet-service-principal-auth.md).

