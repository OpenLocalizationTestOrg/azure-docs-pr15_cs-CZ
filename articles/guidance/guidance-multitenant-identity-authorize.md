<properties
   pageTitle="Povolení aplikace víceklientské | Microsoft Azure"
   description="Jak provést se tak mohli ověřovat víceklientské aplikace"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/02/2016"
   ms.author="mwasson"/>

# <a name="role-based-and-resource-based-authorization-in-multitenant-applications"></a>Na základě rolí a na základě zdroje se tak mohli ověřovat víceklientské aplikace

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Tento článek je [součástí řady]. Je také úplné [Ukázková aplikace] , který doprovází řady.

Náš [implementaci odkaz] je aplikace ASP.NET Core 1.0. V tomto článku podíváme na dva hlavní způsoby se tak mohli ověřovat, použití ověření rozhraní API podle ASP.NET Core 1.0.

-   **Ověřování na základě rolí**. Ověřování na základě rolí se uživateli přiřadí akce. Některé akce například vyžadovat role správce.
-   **Ověřování na základě zdroje**. Povolení akce podle určitého zdroje. Například pro každý zdroj dostupné některé vlastníky. Vlastník můžete odstranit prostředku. ostatní uživatelé nemůžou.

Typické aplikace budou využívat kombinaci obou. Například odstranit zdroj, musí být uživatel může zdroje vlastník _nebo_ správce.


## <a name="role-based-authorization"></a>Ověřování na základě rolí

[Průzkumy Tailspin] [ Tailspin] aplikace definuje následující role:

- Správce. Může provádět všechny operace CRUD na průzkum, které patří k tomuto klientovi.
- Poznámkové bloky pro školy. Můžete vytvářet nové průzkumy
- Čtečka. Může číst zjišťování, které patří k tomuto klientovi

Role vztahovat na _uživatele_ aplikace. V aplikaci průzkumy je uživatel správce, poznámkové bloky pro školy nebo Readeru.

Informace o tom, jak definovat a spravovat role najdete v tématu [role aplikací].

Bez ohledu na to, jak spravovat role bude vypadat podobně jako kódu se tak mohli ověřovat. Základní ASP.NET 1.0 představuje odběru s názvem [zásady se tak mohli ověřovat][policies]. Pomocí této funkce v kódu definovat zásady autorizace a pak použijte tyto zásady řadiče akce. Zásady je oddělené od správce.

### <a name="create-policies"></a>Vytvořit zásady

K definování zásad, nejprve vytvořit třídu implementující `IAuthorizationRequirement`. Je nejjednodušší jsou odvozeny od `AuthorizationHandler`. V `Handle` způsob, zkontrolujte důležité claim(s).

Tady je příklad z aplikace Tailspin průzkumy:

```csharp
public class SurveyCreatorRequirement : AuthorizationHandler<SurveyCreatorRequirement>, IAuthorizationRequirement
{
    protected override void Handle(AuthorizationContext context, SurveyCreatorRequirement requirement)
    {
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin) ||
            context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            context.Succeed(requirement);
        }
    }
}
```

> [AZURE.NOTE] V tématu [SurveyCreatorRequirement.cs]

Tato třída definuje požadavku na uživatele k vytvoření nového průzkumu. Uživatel musí být v roli SurveyAdmin nebo SurveyCreator.

Ve svojí třídě spuštění definujte pojmenovanou zásad, který obsahuje jeden nebo více požadavků. Pokud je potřeba několik věcí, uživatel musí splňovat _každý_ požadavek k tomu. Následující kód definuje dvě zásady:

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy(PolicyNames.RequireSurveyCreator,
        policy =>
        {
            policy.AddRequirements(new SurveyCreatorRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });

    options.AddPolicy(PolicyNames.RequireSurveyAdmin,
        policy =>
        {
            policy.AddRequirements(new SurveyAdminRequirement());
            policy.AddAuthenticationSchemes(CookieAuthenticationDefaults.AuthenticationScheme);
        });
});
```

> [AZURE.NOTE] V tématu [Startup.cs]

Tento kód taky Nastaví schéma ověřování, který říká ASP.NET které ověřování middleware by měla běžet neúspěšné ověření. V tomto případě jsme zadat middleware ověřování souborů cookie, protože middleware ověřování souborů cookie ale můžete je přesměrovávat uživatele na stránku "Zakázáno". Nastavení umístění stránky zakázáno v AccessDeniedPath možnost, aby soubor cookie middleware; v tématu [Konfigurace middleware ověřování].

### <a name="authorize-controller-actions"></a>Povolte řadiče akce

Nakonec, abyste autorizovali akce v MVC řadiče, nastavit zásady `Authorize` atribut:

```csharp
[Authorize(Policy = "SurveyCreatorRequirement")]
public IActionResult Create()
{
    // ...
}
```

Ve starších verzích ASP.NET nastavíte vlastnost **role** v atributu:

```csharp
// old way
[Authorize(Roles = "SurveyCreator")]

```

Toto je stále podporováno v ASP.NET Core 1.0, ale má některé nevýhody ve srovnání se tak mohli ověřovat zásady:

-   Předpokládá typ konkrétní deklarace. Zásady můžete vyhledat libovolný typ deklarace. Role jsou jenom typ deklarace.
-   Název role je pevně do atribut. Zásady logika se tak mohli ověřovat je všechny na jednom místě, snadnější aktualizace nebo dokonce načíst z nastavení.
-   Zásady Povolit složitější rozhodnutí o ověření (například stáří > = 21), který není vyjádřit podle členství jednoduché role.

## <a name="resource-based-authorization"></a>Povolení zdroje na základě

_Zdroje na základě povolení_ dochází při každém povolení závisí na konkrétní prostředek, který bude to mít vliv na operací. V aplikaci Tailspin průzkumy má každé průzkumu některé vlastníky a přispěvatelům nulové přiřazení.

-   Vlastník může číst, aktualizovat, odstranit, publikovat a zrušit publikování průzkumu.
-   Vlastník můžete přiřadit přispěvatelům průzkumu.
-   Přispěvatelům můžete číst a aktualizovat průzkumu.

"Vlastník" a "Přispěvatel" nejsou role aplikací; jsou uložené na průzkum v databázi aplikace. Pokud chcete zkontrolovat, zda uživatel může odstranit průzkumu, například aplikaci zkontroluje, jestli uživatel je vlastník průzkumu.

V ASP.NET Core 1.0 implementujte ověřování na základě zdroje vyplývající z **AuthorizationHandler** a přepsání způsob **zpracování** .

```csharp
public class SurveyAuthorizationHandler : AuthorizationHandler<OperationAuthorizationRequirement, Survey>
{
     protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
    {
    }
}
```

Všimněte si, že tento třídy silného pro objekty průzkumu.  Zaregistrujte si předmětu DI při spuštění:

```csharp
services.AddSingleton<IAuthorizationHandler>(factory =>
{
    return new SurveyAuthorizationHandler();
});
```

Ke kontrole se tak mohli ověřovat pomocí rozhraní **IAuthorizationService** , která vložíte do svého zařízení. Následující kód zkontroluje, zda může uživatel může číst průzkumu:

```csharp
if (await _authorizationService.AuthorizeAsync(User, survey, Operations.Read) == false)
{
    return new HttpStatusCodeResult(403);
}
```

Protože jsme předat `Survey` objektu, vyvolá volání `SurveyAuthorizationHandler`.

V kódu se tak mohli ověřovat je dobrým přístupem agregační všechny uživatele na základě rolí a na základě zdroje oprávnění a potom zaškrtněte agregace nastavit proti požadované operace.
Tady je příklad z aplikace průzkumy. Aplikace definuje několik typů oprávnění:

- Správce
- Skupiny přispěvatelů
- Poznámkové bloky pro školy
- Vlastník
- Čtečky

Aplikace taky definuje sadu s průzkumy možné operace:

- Vytvoření
- Pro čtení
- Aktualizace
- Odstranění
- Publikování
- Unpublsh

Následující kód vytvoří seznam oprávnění pro konkrétní uživatele a průzkumu. Všimněte si, že tento kód sleduje uživatele aplikace role i pole Vlastník/přispěvatelů průzkumu.

```csharp
protected override void Handle(AuthorizationContext context, OperationAuthorizationRequirement operation, Survey resource)
{
    var permissions = new List<UserPermissionType>();
    string userTenantId = context.User.GetTenantIdValue();
    int userId = ClaimsPrincipalExtensions.GetUserKey(context.User);
    string user = context.User.GetUserName();

    if (resource.TenantId == userTenantId)
    {
        // Admin can do anything, as long as the resource belongs to the admin's tenant.
        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyAdmin))
        {
            context.Succeed(operation);
            return;
        }

        if (context.User.HasClaim(ClaimTypes.Role, Roles.SurveyCreator))
        {
            permissions.Add(UserPermissionType.Creator);
        }
        else
        {
            permissions.Add(UserPermissionType.Reader);
        }

        if (resource.OwnerId == userId)
        {
            permissions.Add(UserPermissionType.Owner);
        }
    }
    if (resource.Contributors != null && resource.Contributors.Any(x => x.UserId == userId))
    {
        permissions.Add(UserPermissionType.Contributor);
    }
    if (ValidateUserPermissions[operation](permissions))
    {
        context.Succeed(operation);
    }
}
```

> [AZURE.NOTE] V tématu [SurveyAuthorizationHandler.cs].

V aplikaci více klienta musíte se ujistit, že oprávnění není "proniknout" k datům jiného klienta. V aplikaci průzkumy oprávnění Přispěvatel povolené mezi klienty &mdash; přiřazení uživatele z jiného klienta jako contriubutor. Další oprávnění typy jsou omezeny na zdroje, které patří klienta tohoto uživatele. K jejímu vynucení tohoto požadavku, kód kontroluje ID klienta před udělit oprávnění. ( `TenantId` Polí jako přiřazená po vytvoření průzkumu.)

Dalším krokem je ke kontrole operace (číst, aktualizovat, odstranit atd.) proti oprávnění. Aplikaci průzkumy implementuje tento krok pomocí vyhledávací tabulky funkcí:

```csharp
static readonly Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>> ValidateUserPermissions
    = new Dictionary<OperationAuthorizationRequirement, Func<List<UserPermissionType>, bool>>

    {
        { Operations.Create, x => x.Contains(UserPermissionType.Creator) },

        { Operations.Read, x => x.Contains(UserPermissionType.Creator) ||
                                x.Contains(UserPermissionType.Reader) ||
                                x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Update, x => x.Contains(UserPermissionType.Contributor) ||
                                x.Contains(UserPermissionType.Owner) },

        { Operations.Delete, x => x.Contains(UserPermissionType.Owner) },

        { Operations.Publish, x => x.Contains(UserPermissionType.Owner) },

        { Operations.UnPublish, x => x.Contains(UserPermissionType.Owner) }
    };
```


## <a name="next-steps"></a>Další kroky

- Přečtěte si další článek v této řadě: [zabezpečení back-end webového rozhraní API víceklientské aplikace][web-api]
- Další informace o povolení zdroje na základě ASP.NET 1.0 jádra, najdete v článku [Ověřování na základě zdroje][rbac].

<!-- Links -->
[Tailspin]: guidance-multitenant-identity-tailspin.md
[součástí řady]: guidance-multitenant-identity.md
[Role aplikací]: guidance-multitenant-identity-app-roles.md
[policies]: https://docs.asp.net/en/latest/security/authorization/policies.html
[rbac]: https://docs.asp.net/en/latest/security/authorization/resourcebased.html
[implementace odkaz]: guidance-multitenant-identity-tailspin.md
[SurveyCreatorRequirement.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyCreatorRequirement.cs
[Startup.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Web/Startup.cs
[Konfigurace ověřování middleware]: guidance-multitenant-identity-authenticate.md#configuring-the-authentication-middleware
[SurveyAuthorizationHandler.cs]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps/blob/master/src/Tailspin.Surveys.Security/Policy/SurveyAuthorizationHandler.cs
[Ukázková aplikace]: https://github.com/Azure-Samples/guidance-identity-management-for-multitenant-apps
[web-api]: guidance-multitenant-identity-web-api.md
