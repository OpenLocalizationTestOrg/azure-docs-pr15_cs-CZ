<properties 
    pageTitle="Přístup k rozhraní API služby Azure Mobile zapojení pomocí .NET SDK" 
    description="Popisuje, jak používat SDK Mobile zapojení .NET pro přístup k rozhraní API služby Azure Mobile můžete zapojit"        
    services="mobile-engagement" 
    documentationCenter="mobile" 
    authors="piyushjo" 
    manager="erikre" 
    editor="" />

<tags 
    ms.service="mobile-engagement" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="mobile-multiple" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="08/19/2016" 
    ms.author="piyushjo" />

#<a name="using-net-sdk-to-access-azure-mobile-engagement-service-apis"></a>Přístup k rozhraní API služby Azure Mobile zapojení pomocí .NET SDK

Azure zapojení Mobile zpřístupňuje sadu rozhraní API pro správu zařízení Reach/nabízených kampaní atd. Pokud chcete provést interakci s těchto rozhraní API, jsme také umožňují [Swagger souboru](https://github.com/Azure/azure-rest-api-specs/blob/master/arm-mobileengagement/2014-12-01/swagger/mobile-engagement.json) , můžete pomocí nástrojů generovat SDK pro preferovaný jazyk. Doporučujeme používat nástroj [AutoRest](https://github.com/Azure/AutoRest) generovat vaší SDK z našich Swagger souboru. 

Jsme vytvořili .NET SDK podobným způsobem, který umožňuje pracovat s těchto rozhraní API pomocí Obálka C# a nemusíte udělat tokenu vyjednávání ověřování a aktualizovat sami.  

Tento příklad prochází sadu kroky používat .NET SDK:

1. Nejprve budete muset nastavit ověřování pro vaše rozhraní API používají Azure Active Directory popsaných [v tomto poli](mobile-engagement-api-authentication.md#authentication). Na konci z těchto kroků byste měli mít platné **SubscriptionId** **TenantId**, **ApplicationId** a **tajná**. 

2. Použijeme jednoduché aplikace konzoly Windows prokázat práci s .NET SDK ve scénáři vytváření kampaně oznámení. Takto otevřete Visual Studiu a vytvořit **Konzoly aplikace**.   

3. Pak budete muset stáhnout .NET SDK, která je k dispozici jako **Knihovny Microsoft Azure zapojení správy** v galerii Nuget [tady](https://www.nuget.org/packages/Microsoft.Azure.Management.Engagement/).
Pokud instalujete Nuget z aplikace Visual Studio, je potřeba, abyste získali zaškrtnutí označené možnost **Zahrnout zkušební** při vyhledávání balíčku:

    ![][1]

4. V `Program.cs` souboru, přidejte následující obory názvů:

        using Microsoft.Rest.Azure.Authentication;
        using Microsoft.Azure.Management.Engagement;
        using Microsoft.Azure.Management.Engagement.Models;

5. Dále je třeba určit následující konstant, které použijeme pro ověřování a práce s aplikaci zapojení Mobile, ve kterém jsou vytváření kampaně oznámení:

        // For authentication
        const string TENANT_ID = "<Your TenantId>";
        const string CLIENT_ID = "<Your Application Id>";
        const string CLIENT_SECRET = "<Your Secret>";
        const string SUBSCRIPTION_ID = "<Your Subscription Id>";

        // This is the Azure Resource group concept for grouping together resources 
        //  see here: https://azure.microsoft.com/en-us/documentation/articles/resource-group-portal/
        const string RESOURCE_GROUP = "";

        // For Mobile Engagement operations
        // App Collection Name 
        const string APP_COLLECTION_NAME = "";
        // Application Resource Name - make sure you are using the one as specified in the Azure portal (NOT the App Name)
        const string APP_RESOURCE_NAME = "";

6. Definování `EngagementManagementClient` proměnnou, která použijeme volat metody SDK zapojení Mobile:

        static EngagementManagementClient engagementClient; 

7. Přidání podle následujících pokynů vašeho `Main` metodu:

        try
            {
                // Initialize the Engagement SDK to call out APIs. 
                InitEngagementClient().Wait();

                // Create a Reach campaign
                CreateCampaign().Wait();
            }
            catch (Exception ex)
            {
                Console.WriteLine(ex.InnerException.Message);
                throw ex;
            }

8. Definování následujícího postupu, který má na starosti inicializace `EngagementManagementClient` tak, že nejdřív ověřování a přidružení samotnou aplikaci zapojení Mobile, ve kterém chcete vytvořit kampaně oznámení:

        private static async Task InitEngagementClient()
        {
            var credentials = await ApplicationTokenProvider.LoginSilentAsync(TENANT_ID, CLIENT_ID, CLIENT_SECRET);
            engagementClient = new EngagementManagementClient(credentials) { SubscriptionId = SUBSCRIPTION_ID };
            
            // This is the Azure concept of ResourceGroup
            engagementClient.ResourceGroupName = RESOURCE_GROUP;

            // This is your Mobile Engagement App Collection & App Resource Name in which you create the campaign
            engagementClient.AppCollection = APP_COLLECTION_NAME;
            engagementClient.AppName = APP_RESOURCE_NAME;
        }

    > [AZURE.IMPORTANT] Všimněte si, že je potřeba použít **Název zdroje aplikace** nadefinovaná na portálu Správa Azure parametru název_aplikace. 

9. Nakonec definovat CreateCampaign metodu, která budou starat o použití dříve spuštění EngagementClient k vytvoření jednoduché **kdykoli** & **oznámení určené jen** kampaně názvem a zprávy: 

        private async static Task CreateCampaign()
        {
            //  Refer to the Announcement Campaign format from here - 
            //      https://msdn.microsoft.com/en-us/library/azure/mt683751.aspx
            // Make sure you are passing all the non-optional parameters
            Campaign parameters = new Campaign(
                name:"WelcomeCampaign",
                notificationTitle: "Welcome", 
                notificationMessage: "Thank you for installing the app!",
                type:"only_notif",
                deliveryTime:"any"
                );

            // Refer to the Campaign Kinds from here - https://msdn.microsoft.com/en-us/library/azure/mt683742.aspx
            CampaignStateResult result = 
                await engagementClient.Campaigns.CreateAsync(CampaignKinds.Announcements, parameters);
            Console.WriteLine("Campaign Id '{0}' was created successfully and it is in '{1}' state", result.Id, result.State);
        }

10. Spusťte aplikaci konzoly a zobrazí se následující na úspěšné vytvoření kampaně:

    **Id kampaně "159" byl vytvořen úspěšně a je ve stavu "koncept"**

<!-- Images. -->

[1]: ./media/mobile-engagement-dotnet-sdk-service-api/include-prerelease.png
