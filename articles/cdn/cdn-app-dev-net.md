<properties
    pageTitle="Začínáme s knihovnou Azure CDN pro .NET | Microsoft Azure"
    description="Informace o vytváření aplikací ke správě CDN Azure pomocí aplikace Visual Studio .NET."
    services="cdn"
    documentationCenter=".net"
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="casoper"/>

# <a name="get-started-with-azure-cdn-development"></a>Začínáme s vývojem Azure CDN

> [AZURE.SELECTOR]
- [Node.js](cdn-app-dev-node.md)
- [.NET](cdn-app-dev-net.md)

[Knihovna Azure CDN pro .NET](https://msdn.microsoft.com/library/mt657769.aspx) můžete použít k automatizaci vytváření a Správa profilů CDN a koncové body.  Tento kurz provede vytvoření jednoduchého .NET konzoly aplikace, která ukazuje některé z dostupných operací.  Tento kurz není určená k podrobně popisují všechny aspekty knihovnu Azure CDN pro .NET.

Potřebujete Visual Studio 2015 pro dokončení tohoto kurzu.  [Visual Studio komunity 2015](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) je zdarma stáhnout.

> [AZURE.TIP] [Dokončení projektu z tohoto kurzu](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c) je k dispozici ke stažení na webu MSDN.

[AZURE.INCLUDE [cdn-app-dev-prep](../../includes/cdn-app-dev-prep.md)]

## <a name="create-your-project-and-add-nuget-packages"></a>Vytvoření projektu a přidat Nuget balíčků

Teď můžeme jste vytvořili skupina zdroje pro naše CDN profily a naše Azure AD aplikace oprávnění ke správě profilů CDN a koncové body v dané skupině, můžeme začít vytvářet naše aplikace.

Ve Visual Studiu 2015, klikněte na **soubor**, **Nový** **projekt...** otevřete dialogové okno Nový projekt.  Rozšíření **Visual Basic**a potom vyberte v podokně na levé straně **okna** .  Klikněte na **Aplikace konzoly** v prostředním podokně.  Název projektu a potom klikněte na tlačítko **OK**.  

![Nový projekt](./media/cdn-app-dev-net/cdn-new-project.png)

Náš projektu bude používat některé Azure knihovny obsažené v Nuget balíčků.  Přidejte do něj můžou být do projektu.

1. Klikněte na nabídku **Nástroje** , **Správce balíčků Nuget**pak **Konzoly balíčku správce**.

    ![Správa Nuget balíčků](./media/cdn-app-dev-net/cdn-manage-nuget.png)

2. V konzole Správce balíčků spusťte následující příkaz a nainstalujte **Active Directory Authentication knihovny (ADAL)**:

    `Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory`

3. Spuštění podle následujících pokynů nainstalujte **Knihovnou správy CDN Azure**:

    `Install-Package Microsoft.Azure.Management.Cdn`

## <a name="directives-constants-main-method-and-helper-methods"></a>Směrnice, konstant, hlavní metoda a pomocné metody

Nastavíme základní struktura naše program napsaný.

1. Zpět na kartě Program.cs nahradit `using` směrnice nahoře následující:

    ```csharp
    using System;
    using System.Collections.Generic;
    using Microsoft.Azure.Management.Cdn;
    using Microsoft.Azure.Management.Cdn.Models;
    using Microsoft.Azure.Management.Resources;
    using Microsoft.Azure.Management.Resources.Models;
    using Microsoft.IdentityModel.Clients.ActiveDirectory;
    using Microsoft.Rest;
    ```

2. Je třeba definovat některé konstanty používané naše metody.  V `Program` třídy, ale předtím, než `Main` metody, přidejte následující text.  Nezapomeňte místo zástupné symboly, včetně ** &lt;lomenými závorkami&gt;**, s vlastními hodnotami v případě potřeby.

    ```csharp
    //Tenant app constants
    private const string clientID = "<YOUR CLIENT ID>";
    private const string clientSecret = "<YOUR CLIENT AUTHENTICATION KEY>"; //Only for service principals
    private const string authority = "https://login.microsoftonline.com/<YOUR TENANT ID>/<YOUR TENANT DOMAIN NAME>";

    //Application constants
    private const string subscriptionId = "<YOUR SUBSCRIPTION ID>";
    private const string profileName = "CdnConsoleApp";
    private const string endpointName = "<A UNIQUE NAME FOR YOUR CDN ENDPOINT>";
    private const string resourceGroupName = "CdnConsoleTutorial";
    private const string resourceLocation = "<YOUR PREFERRED AZURE LOCATION, SUCH AS Central US>";
    ```

3. Na úrovni třídy definujte tyto dvě proměnné.  Použijeme tyto později a zjistit, pokud naši stránku věnovanou profilu a koncový bod existovat.

    ```csharp
    static bool profileAlreadyExists = false;
    static bool endpointAlreadyExists = false;
    ```

4.  Nahrazení `Main` metoda takto:

    ```csharp
    static void Main(string[] args)
    {
        //Get a token
        AuthenticationResult authResult = GetAccessToken();

        // Create CDN client
        CdnManagementClient cdn = new CdnManagementClient(new TokenCredentials(authResult.AccessToken))
            { SubscriptionId = subscriptionId };

        ListProfilesAndEndpoints(cdn);

        // Create CDN Profile
        CreateCdnProfile(cdn);

        // Create CDN Endpoint
        CreateCdnEndpoint(cdn);
        
        Console.WriteLine();

        // Purge CDN Endpoint
        PromptPurgeCdnEndpoint(cdn);

        // Delete CDN Endpoint
        PromptDeleteCdnEndpoint(cdn);

        // Delete CDN Profile
        PromptDeleteCdnProfile(cdn);

        Console.WriteLine("Press Enter to end program.");
        Console.ReadLine();
    }
    ```

5. Některé naše jiné metody chodí výzvu s dotazy "Ano/Ne".  Přidání metodu trochu usnadníme tím, který:

    ```csharp
    private static bool PromptUser(string Question)
    {
        Console.Write(Question + " (Y/N): ");
        var response = Console.ReadKey();
        Console.WriteLine();
        if (response.Key == ConsoleKey.Y)
        {
            return true;
        }
        else if (response.Key == ConsoleKey.N)
        {
            return false;
        }
        else
        {
            // They pressed something other than Y or N.  Let's ask them again.
            return PromptUser(Question);
        }
    }
    ```

Teď základní struktura náš program napsali, by měl vytvoříme metody volat `Main` metody.

## <a name="authentication"></a>Ověřování

Abychom mohli použít knihovnou správy CDN Azure, potřebujeme ověření naše jistinu služby a získat token ověřování.  Tento způsob používá ADAL k načtení tokenu.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority); 
    ClientCredential credential = new ClientCredential(clientID, clientSecret);
    AuthenticationResult authResult = 
        authContext.AcquireTokenAsync("https://management.core.windows.net/", credential).Result;

    return authResult;
}
```

Pokud používáte ověřování jednotlivých uživatelů `GetAccessToken` metoda bude vypadat mírně odlišnou.

>[AZURE.IMPORTANT] Tento příklad kód použijte, pouze pokud jste zvolili možnost ověření jednotlivé uživatele místo služby základní.

```csharp
private static AuthenticationResult GetAccessToken()
{
    AuthenticationContext authContext = new AuthenticationContext(authority);
    AuthenticationResult authResult = authContext.AcquireTokenAsync("https://management.core.windows.net/",
        clientID, new Uri("http://<redirect URI>"), new PlatformParameters(PromptBehavior.RefreshSession)).Result;

    return authResult;
}
```

Nezapomeňte místo `<redirect URI>` s přesměrování jste zadali při registraci aplikace v Azure AD URI.

## <a name="list-cdn-profiles-and-endpoints"></a>Seznam CDN profily a koncové body

Teď můžeme začít provádět operace CDN.  První věc, kterou naše metoda je seznam všech profily a koncové body v naší pole Skupina zdroje a pokud najde shodu název profilu a koncový bod podle našeho konstanty, díky kterému poznámku, kterou na pozdější tak jsme není pokusu o vytvoření duplicitních položek.

```csharp
private static void ListProfilesAndEndpoints(CdnManagementClient cdn)
{
    // List all the CDN profiles in this resource group
    var profileList = cdn.Profiles.ListByResourceGroup(resourceGroupName);
    foreach (Profile p in profileList)
    {
        Console.WriteLine("CDN profile {0}", p.Name);
        if (p.Name.Equals(profileName, StringComparison.OrdinalIgnoreCase))
        {
            // Hey, that's the name of the CDN profile we want to create!
            profileAlreadyExists = true;
        }

        //List all the CDN endpoints on this CDN profile
        Console.WriteLine("Endpoints:");
        var endpointList = cdn.Endpoints.ListByProfile(p.Name, resourceGroupName);
        foreach (Endpoint e in endpointList)
        {
            Console.WriteLine("-{0} ({1})", e.Name, e.HostName);
            if (e.Name.Equals(endpointName, StringComparison.OrdinalIgnoreCase))
            {
                // The unique endpoint name already exists.
                endpointAlreadyExists = true;
            }
        }
        Console.WriteLine();
    }
}
```

## <a name="create-cdn-profiles-and-endpoints"></a>Vytvoření CDN profily a koncové body

Dále vytvoříme profilem.

```csharp
private static void CreateCdnProfile(CdnManagementClient cdn)
{
    if (profileAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating profile {0}.", profileName);
        ProfileCreateParameters profileParms =
            new ProfileCreateParameters() { Location = resourceLocation, Sku = new Sku(SkuName.StandardVerizon) };
        cdn.Profiles.Create(profileName, profileParms, resourceGroupName);
    }
}
```

Po vytvoření profilu vytvoříme koncový bod.

```csharp
private static void CreateCdnEndpoint(CdnManagementClient cdn)
{
    if (endpointAlreadyExists)
    {
        Console.WriteLine("Profile {0} already exists.", profileName);
    }
    else
    {
        Console.WriteLine("Creating endpoint {0} on profile {1}.", endpointName, profileName);
        EndpointCreateParameters endpointParms =
            new EndpointCreateParameters()
            {
                Origins = new List<DeepCreatedOrigin>() { new DeepCreatedOrigin("Contoso", "www.contoso.com") },
                IsHttpAllowed = true,
                IsHttpsAllowed = true,
                Location = resourceLocation
            };
        cdn.Endpoints.Create(endpointName, endpointParms, profileName, resourceGroupName);
    }
}
```

>[AZURE.NOTE] Z příkladu výše přiřadí koncový bod origin s názvem *Contoso* název hostitele `www.contoso.com`.  Je třeba změnit to tak, aby ukazovaly hostname vlastní origin.

## <a name="purge-an-endpoint"></a>Odstranění koncového bodu

Za předpokladu, že byl vytvořen koncový bod, je jeden běžné úkol, který jsme možná budete chtít v naší aplikaci provést vymazání obsahu v naší koncového bodu.

```csharp
private static void PromptPurgeCdnEndpoint(CdnManagementClient cdn)
{
    if (PromptUser(String.Format("Purge CDN endpoint {0}?", endpointName)))
    {
        Console.WriteLine("Purging endpoint. Please wait...");
        cdn.Endpoints.PurgeContent(endpointName, profileName, resourceGroupName, new List<string>() { "/*" });
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

>[AZURE.NOTE] V příkladu výše řetězec `/*` označuje, zda má být Vymazat všechno na kořenovou cestu koncového bodu.  Jedná se o ekvivalent kontrolu **Vymazat vše** v dialogovém okně "Vymazat" Azure portálu. V `CreateCdnProfile` metodu jsem vytvořil(a) naše profil jako profil aplikace **Azure CDN z Verizon** pomocí kódu `Sku = new Sku(SkuName.StandardVerizon)`, takže to bude úspěšně.  Však **Azure CDN z Akamai** profily nepodporují **Vymazat všechny**, takže pokud použití Akamai profilu pro účely tohoto návodu, potřeba zahrnout konkrétní cesty k vymazání.

## <a name="delete-cdn-profiles-and-endpoints"></a>Odstranění CDN profily a koncové body

Poslední metody odstraníte stránku věnovanou koncového bodu a profilu.

```csharp
private static void PromptDeleteCdnEndpoint(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN endpoint {0} on profile {1}?", endpointName, profileName)))
    {
        Console.WriteLine("Deleting endpoint. Please wait...");
        cdn.Endpoints.DeleteIfExists(endpointName, profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}

private static void PromptDeleteCdnProfile(CdnManagementClient cdn)
{
    if(PromptUser(String.Format("Delete CDN profile {0}?", profileName)))
    {
        Console.WriteLine("Deleting profile. Please wait...");
        cdn.Profiles.DeleteIfExists(profileName, resourceGroupName);
        Console.WriteLine("Done.");
        Console.WriteLine();
    }
}
```

## <a name="running-the-program"></a>Spuštění programu

Teď můžeme kompilaci a spustit program kliknutím na tlačítko **Start** ve Visual Studiu.

![Spuštění programu](./media/cdn-app-dev-net/cdn-program-running-1.png)

Řádku nad dosáhne program by měla se vraťte do skupiny zdroje na portálu Azure a najdete v článku vytvořené profil.

![Úspěch!](./media/cdn-app-dev-net/cdn-success.png)

Si ověřujeme potom výzvy ke spuštění zbytek programu.

![Dokončení programu](./media/cdn-app-dev-net/cdn-program-running-2.png)

## <a name="next-steps"></a>Další kroky

Chcete-li zobrazit hotového projektu z tohoto návodu, [Stáhněte si vzorku](https://code.msdn.microsoft.com/Azure-CDN-Management-1f2fba2c).

Pokud chcete najít další si přečtěte následující dokumentaci u této knihovny správy Azure CDN pro .NET, zobrazení [odkaz na webu MSDN](https://msdn.microsoft.com/library/mt657769.aspx).

Správa zdrojů CDN pomocí [prostředí PowerShell](./cdn-manage-powershell.md).


