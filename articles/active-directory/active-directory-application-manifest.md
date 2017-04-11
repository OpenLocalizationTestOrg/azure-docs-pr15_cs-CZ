<properties
   pageTitle="Principy Manifest aplikace Azure Active Directory | Microsoft Azure"
   description="Podrobné pokrytí manifestu aplikace služby Azure Active Directory, který představuje konfiguraci aplikace identity v klientovi Azure AD a slouží k usnadnění OAuth se tak mohli ověřovat, souhlas prostředí a další."
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="dkershaw;bryanla"/>

# <a name="understanding-the-azure-active-directory-application-manifest"></a>Principy manifest aplikace služby Azure Active Directory

Aplikace, které lze integrovat s Azure Active Directory (AD) musí být registrován s klientem Azure AD, poskytnutí konfigurace trvalý identity pro aplikaci. Konfigurace po projednání za běhu, povolení scénáře, které umožňují aplikaci externí a makléře ověřování/se tak mohli ověřovat pomocí Azure AD. Další informace o aplikaci modelu Azure AD, podívejte se na [přidávání, aktualizace a odstraňování aplikace] [ ADD-UPD-RMV-APP] článku.

## <a name="updating-an-applications-identity-configuration"></a>Aktualizace aplikace identity konfigurace

Ve skutečnosti řadu možností pro nejsou k dispozici aktualizace vlastností ke konfiguraci identity aplikace, které se liší funkce i stupňů obtížnosti, včetně následujících:

- ** [Azure klasické portálu] [ AZURE-CLASSIC-PORTAL] uživatelské rozhraní webu** umožňuje aktualizovat nejběžnější vlastností aplikace. To je nejrychlejší a aspoň chyby chybám tak, jak aktualizace vlastnosti aplikace, ale nezískáváte plný přístup ke všem vlastnostem, třeba dalších dvou způsobů.
- Místo, kam potřebujete aktualizovat vlastnosti, které nejsou přístupné Azure klasické portálu pokročilejší scénáře můžete změnit **manifest aplikace**. Toto je fokus v tomto článku která je podrobněji spuštění v další části.
- Je také možné **psát aplikace, která používá rozhraní [API grafu] [ GRAPH-API] ** aktualizovat aplikace, který náročný nejčastěji. Může to být atraktivních možnost i když, pokud jsou psaní software pro správu nebo potřebovali aktualizovat vlastnosti aplikace pravidelně automatické způsobem.

## <a name="using-the-application-manifest-to-update-an-applications-identity-configuration"></a>Použití manifestu aplikace aktualizovat konfigurace aplikace identity
Přes [Azure klasické portál][AZURE-CLASSIC-PORTAL], konfigurace identity aplikace můžete spravovat stažením a nahrávání JSON zařaďte znázornění, která se nazývá manifest aplikace. Žádné skutečné soubor uložený v adresáři. Manifest aplikace je pouze HTTP GET operaci Azure AD grafu rozhraní API aplikace entita a nahrávání operaci HTTP oprava entity aplikace.

V důsledku toho chcete porozumět tomu, jaký formát a vlastnosti manifestu aplikace, budete muset odkazovat grafu rozhraní API [aplikace entita] [ APPLICATION-ENTITY] si přečtěte následující dokumentaci. Příklady aktualizace, které lze provést, když nahrát manifest aplikace:

- Zveřejněné příslušným webového rozhraní API **Declare oprávnění oborů (oauth2Permissions)** . V tématu "Vystavení webového rozhraní API do jiných aplikací" aplikace [Integrace se službou Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] informace o implementaci zosobnění uživatele pomocí oauth2Permissions delegované oboru oprávnění. Jak jsme zmínili dříve, vlastnosti entity aplikace jsou popsány v grafu API [Entita a typ složitých] [ APPLICATION-ENTITY] referenční článek, včetně oauth2Permissions vlastnost, která je kolekce typu [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
- **Role aplikací Declare (appRoles) zveřejněné příslušným aplikace**. Vlastnost appRoles subjekt aplikace je sada typ [AppRole][APPLICATION-ENTITY-APP-ROLE]. V tématu [řízení přístupu v cloudu aplikací pomocí Azure AD na základě rolí] [ RBAC-CLOUD-APPS-AZUREAD] článek příklad implementaci.
- **Declare známé klientské aplikace (knownClientApplications)**, které umožňují logicky nevážou souhlas zadaný klient aplikace na zdroje/webového rozhraní API.
- **Žádost o Azure AD o vystavení skupinu, kterou převzetí členství** pro přihlášený uživatel (groupMembershipClaims).  To také je možné konfigurovat o vystavení tvrzení o členství role uživatele v adresáři. V tématu [Povolení aplikace cloudu pomocí skupin AD] [ AAD-GROUPS-FOR-AUTHORIZATION] článku příklad implementaci.
- **Povolit aplikaci pro podporu OAuth 2.0 implicitní udělit** toků (oauth2AllowImplicitFlow). Tento typ směrování grant (udělit) se používá s tímto JavaScript webové stránky nebo aplikací zabezpečené (jednoho ověřování HESLA stránky). Další informace o udělení implicitní se tak mohli ověřovat, najdete v článku [Principy implicitní OAuth2 udělit toku v Azure Active Directory][IMPLICIT-GRANT].
- **Povolit použití X509 certifikátů jako tajné klíč** (keyCredentials). Najdete v článku [Vytvoření aplikace služby a démon v Office 365] [ O365-SERVICE-DAEMON-APPS] a [Průvodce pro vývojáře auth pomocí rozhraní API Správce prostředků Azure] [ DEV-GUIDE-TO-AUTH-WITH-ARM] v článcích příklady implementace.
- **Přidat nové URI ID aplikace** aplikace (identifierURIs[]). Identifikátor URI aplikace používají jednoznačně identifikovat aplikaci v rámci jeho Azure AD klienta (nebo víc klientů Azure AD pro více klienta scénáře, kdy kvalifikované prostřednictvím ověření vlastní domény). Používají se při požadování oprávnění aplikace zdroje nebo získáním přístupový token aplikace zdroje. Když aktualizujete: Tento element stejná aktualizace volání na odpovídající služby jistinu servicePrincipalNames [] kolekce, které jsou umístěná v domácí klienta aplikace.

Manifest aplikace také poskytuje spolehlivých způsobů, jak sledovat stav registrace aplikace. Protože není k dispozici ve formátu JSON, můžete soubor, který představuje checked do zdrojového, spolu s aplikace zdrojového kódu.

## <a name="step-by-step-example"></a>Krok příklad krok
Nyní umožňuje, projděte si kroky potřebné k aktualizaci konfigurace aplikace identit prostřednictvím manifest aplikace. Jsme zvýrazní jednu z předchozích příkladech znázorňující, jak deklarovat nového oboru oprávnění u zdrojů aplikace:

1. Přejděte na [Azure klasický portálu] [ AZURE-CLASSIC-PORTAL] a přihlaste se pomocí účtu s oprávněními správce nebo vedlejšího Správce služby.

2. Po můžete jste ověřené, posuňte se dolů a vyberte Azure "služby Active Directory" rozšíření v levém navigačním panelu (1) a potom klikněte na registered Azure AD klienta, kde je aplikace (2).

    ![Vyberte Azure AD klienta][SELECT-AZURE-AD-TENANT]

3. Na stránce adresář se zobrazí, klikněte v horní části stránky zobrazíte seznam aplikací registraci v klientovi na "Aplikace" (1). Najděte aplikaci, kterou chcete aktualizovat v seznamu a klikněte na ni (2).

    ![Vyberte Azure AD klienta][SELECT-AZURE-AD-APP]

4. Teď, když vyberete hlavní stránky aplikace, Všimněte si funkci "Správa Manifest" v dolní části stránky (1). Pokud kliknete na odkaz, budete vyzváni k stáhnout nebo nahrát soubor JSON. Klikněte na "Stahování manifestu" (2). To bude třeba okamžitě dodržet potvrzovacím dialogovém okně Stažení s výzvou k potvrzení kliknutím na "Stáhnout Manifest" (3), a pak buď otevřít nebo uložit soubor místně (4).

    ![Správa manifest, stáhněte si možnost][MANAGE-MANIFEST-DOWNLOAD]

    ![Stažení manifestu][DOWNLOAD-MANIFEST]

5. V tomto příkladu jsme uložený soubor místně, abychom mohli otevřít v editoru, proveďte změny ve formátu JSON a opět uložte. Takto vypadá strukturu JSON v editoru Visual Studio JSON. Poznámku, že splňuje schématu [Aplikační entita] [ APPLICATION-ENTITY] jsme zmínili o tom:

    ![Aktualizace seznamu JSON][UPDATE-MANIFEST]

    Například za předpokladu, že chceme implementace/vystavení nové oprávnění s názvem "Employees.Read.All" na naše zdrojů aplikace (rozhraní API), jednoduše přidejte nové/druhé prvek ke kolekci oauth2Permissions ie:

        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow the application to access MyWebApplication on behalf of the signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to access MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow the application to have read-only access to all Employee data.",
        "adminConsentDisplayName": "Read-only access to Employee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow the application to have read-only access to your Employee data.",
        "userConsentDisplayName": "Read-only access to your Employee records",
        "value": "Employees.Read.All"
        }
        ],

    Položka musí být jedinečný a musíte tedy generovat nové globálně jedinečné ID (GUID) pro `"id"` vlastnost. V tomto případě protože jsme zadán `"type": "User"`, toto oprávnění lze souhlas pomocí libovolného účtu ověřování službou Azure AD tenant ve kterém je aplikace zdroje a rozhraní API registrováno. To uděluje klient aplikace oprávnění pro přístup k účtu jménem. Název řetězce popis a zobrazení jsou používány během souhlas a zobrazení na portálu Azure klasický.

6. Po dokončení aktualizace manifest, vraťte se na stránku aplikací Azure AD na portálu Azure klasické a klepněte na "Správa Manifest" funkci znovu (1). Ale tentokrát, vyberte možnost "Odeslat Manifest" (2). Podobně jako stahování můžete bude se vám znovu s druhý dialogového okna s výzvou k určení umístění souboru JSON. Klikněte na "Vyhledejte soubor..." (3) a potom vyberte soubor JSON (4) pomocí dialogového okna "Zvolte soubor k nahrání" a stiskněte klávesu "Otevřít". Jakmile dialogu nezmizí, vyberte na značku zaškrtnutí "OK" (5) a nahraje manifestu.  

    ![Správa manifest, nahrajte možnost][MANAGE-MANIFEST-UPLOAD]

    ![Nahrání seznamu JSON][UPLOAD-MANIFEST]

    ![Nahrání seznamu JSON – potvrzení][UPLOAD-MANIFEST-CONFIRM]

Teď je uložen manifest, můžete zobrazit registrované klientské aplikace přístup ke nové oprávnění je teď nově přidaná nad. Tentokrát není nutné můžete použít uživatelské rozhraní webu Azure klasické portál úpravy manifestu klientské aplikace:  

1. Nejdřív přejděte na stránku "Konfigurovat" klientské aplikace, ke kterému chcete přidat přístup k rozhraní API nové a klikněte na tlačítko "Přidat aplikaci".
2. Potom se zobrazí se seznam registrovaných zdroje aplikací (rozhraní API) v klientovi. Klepněte na znaménko plus / + symbol vedle názvu zdroje aplikace ho vyberte.  
3. Klepněte na značku zaškrtnutí v pravé dolní.
4. Když se vrátíte do části "Přidání aplikace" stránka konfiguraci vašeho klienta, zobrazí se nové aplikace zdroj v seznamu. Když najedete myší části "Delegovat oprávnění" napravo od řádku, zobrazí se rozevíracím seznamu zobrazit. Klikněte na seznam a potom vyberte Nová oprávnění, aby ho přidat do klienta požadovaný seznam oprávnění. Poznámka: Toto nové oprávnění budou uložené v klientské aplikaci identit konfiguraci ve vlastnosti kolekce "requiredResourceAccess".

![Oprávnění pro jiné aplikace][PERMS-TO-OTHER-APPS]

![Oprávnění pro jiné aplikace][PERMS-SELECT-APP]

![Oprávnění pro jiné aplikace][PERMS-SELECT-PERMS]

To je vše. Teď aplikace spuštěn svou novou konfiguraci identity.

## <a name="next-steps"></a>Další kroky
- Podrobné informace o vztah mezi objekty aplikace a služby jistinu aplikace, najdete v článku [aplikace a služby základní objektů v Azure AD][AAD-APP-OBJECTS].
- Přejděte na téma [Azure AD vývojář Glosář] [ AAD-DEVELOPER-GLOSSARY] definice některé základní pojmy vývojář Azure Active Directory (AD).

Stiskněte klávesovou zkratku DISQUS komentáře v části poskytnutí zpětné vazby a Pomozte nám vylepšit a přizpůsobovat naše obsah.

<!--Image references-->
[DOWNLOAD-MANIFEST]: ./media/active-directory-application-manifest/download-manifest.png
[MANAGE-MANIFEST-DOWNLOAD]: ./media/active-directory-application-manifest/manage-manifest-download.png
[MANAGE-MANIFEST-UPLOAD]: ./media/active-directory-application-manifest/manage-manifest-upload.png
[PERMS-SELECT-APP]: ./media/active-directory-application-manifest/portal-perms-select-app.png
[PERMS-SELECT-PERMS]: ./media/active-directory-application-manifest/portal-perms-select-perms.png
[PERMS-TO-OTHER-APPS]: ./media/active-directory-application-manifest/portal-perms-to-other-apps.png
[SELECT-AZURE-AD-APP]: ./media/active-directory-application-manifest/select-azure-ad-application.png
[SELECT-AZURE-AD-TENANT]: ./media/active-directory-application-manifest/select-azure-ad-tenant.png
[UPDATE-MANIFEST]: ./media/active-directory-application-manifest/update-manifest.png
[UPLOAD-MANIFEST]: ./media/active-directory-application-manifest/upload-manifest.png
[UPLOAD-MANIFEST-CONFIRM]: ./media/active-directory-application-manifest/upload-manifest-confirm.png

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-CLASSIC-PORTAL]: https://manage.windowsazure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

