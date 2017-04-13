<properties
    pageTitle="programově sledovat úlohy v toku analýzy | Microsoft Azure"
    description="Zjistěte, jak programově sledování úlohy toku analýzy vytvořené pomocí rozhraní REST API, Azure SDK nebo Powershellu."
    keywords="sledování .net, sledovat úlohy, sledování aplikací"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>


# <a name="programmatically-create-a-stream-analytics-job-monitor"></a>Programové vytvoření monitoru úlohy toku analýzy
 Tento článek ukazuje, jak povolit sledování toku analýzy projektu. Ve výchozím nastavení monitorování nepovolili analýzy toku, které provádět úlohy vytvořený prostřednictvím rozhraní REST API, Azure SDK nebo Powershellu.  Můžete ručně povolit to na portálu Azure tak, že přejdete na stránku projektu Monitor a klikněte na tlačítko Povolit nebo tento proces automatizovat pomocí kroků uvedených v tomto článku. Sledování dat se zobrazí na kartě "Monitor" v portálu Azure pro analýzy toku práce.

![sledování práce karty úlohy](./media/stream-analytics-monitor-jobs/stream-analytics-monitor-jobs-tab.png)

## <a name="prerequisites"></a>Zjistit předpoklady pro
Než začnete v tomto článku, musíte mít takto:

- Visual Studio 2012 nebo 2013.
- Stáhněte a nainstalujte [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Existující úlohu toku analýzy, která potřebuje sledování povolené.

## <a name="setup-a-project"></a>Nastavení projektu

1.  Vytvoření aplikace Visual Studio C# .net konzoly.
2.  V konzole Správce balíčků následující příkazy k instalaci balíčků NuGet. První z nich je Azure toku analýzy Management .NET SDK. Druhý je Azure Monitor SDK, která se použijí k povolit sledování. Poslední je Azure Active Directory klienta, který bude sloužit k ověření.

    ```
    Install-Package Microsoft.Azure.Management.StreamAnalytics
    Install-Package Microsoft.Azure.Insights -Pre
    Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
    ```

3.  V následující části appSettings dodejte konfiguračního souboru.

    ```
    <appSettings>
        <!--CSM Prod related values-->
        <add key="ResourceGroupName" value="RESOURCE GROUP NAME" />
        <add key="JobName" value="YOUR JOB NAME" />
        <add key="StorageAccountName" value="YOUR STORAGE ACCOUNT"/>
        <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
        <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
        <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
        <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
        <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
        <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION ID" />
        <add key="ActiveDirectoryTenantId" value="YOUR TENANT ID" />
    </appSettings>
    ```
Nahradit hodnoty pro *SubscriptionId* a *ActiveDirectoryTenantId* Azure předplatné a klient ID. Spusťte následující rutinu Powershellu si mohli udělat tyto hodnoty:

    ```
    Get-AzureAccount
    ```
4.  Přidejte následující pomocí příkazů ve zdrojovém souboru (Program.cs) v projektu.

    ```
        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.Insights;
        using Microsoft.Azure.Management.Insights.Models;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
    ```
5.  Přidání metody ověřování pomocníka.

        public static string GetAuthorizationHeader()
            {
                AuthenticationResult result = null;
                var thread = new Thread(() =>
                {
                    try
                    {
                        var context = new AuthenticationContext(
                            ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] +
                            ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                        result = context.AcquireToken(
                            resource: ConfigurationManager.AppSettings["WindowsManagementUri"],
                            clientId: ConfigurationManager.AppSettings["AsaClientId"],
                            redirectUri: new Uri(ConfigurationManager.AppSettings["RedirectUri"]),
                            promptBehavior: PromptBehavior.Always);
                    }
                    catch (Exception threadEx)
                    {
                        Console.WriteLine(threadEx.Message);
                    }
                });

                thread.SetApartmentState(ApartmentState.STA);
                thread.Name = "AcquireTokenThread";
                thread.Start();
                thread.Join();

                if (result != null)
                {
                    return result.AccessToken;
                }

                throw new InvalidOperationException("Failed to acquire token");
        }

## <a name="create-management-clients"></a>Vytvoření Správa klientů
Následující kód vytvoří potřebné proměnných a správy klientů.

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    Uri resourceManagerUri = new
    Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

    // Create Stream Analytics and Insights management client
    StreamAnalyticsManagementClient streamAnalyticsClient = new
    StreamAnalyticsManagementClient(aadTokenCredentials, resourceManagerUri);
    InsightsManagementClient insightsClient = new
    InsightsManagementClient(aadTokenCredentials, resourceManagerUri);

## <a name="enable-monitoring-for-an-existing-stream-analytics-job"></a>Povolit sledování pro existující úlohu analýzy toku

Následující kód vám umožní sledování **existujícího** projektu toku analýzy. První část kódu provede požadavek GET Service analýz toku získat informace o určitý projekt toku analýzy. Použije vlastnost "Id" (načtená z kontingenčního seznamu požadavek GET) jako parametr metodu umístění v druhé polovině kód, který přesměruje umístění žádost o službu přehledy a umožňují sledovat pro danou úlohu toku analýzy.

> [AZURE.WARNING]
> Pokud jste zapnuli pro různé úlohy toku analýzy, portálu Azure nebo programově prostřednictvím sledování pod kód, **je vhodné zadat stejný název účtu úložiště, jako když jste zapnuli monitorování.**
>
> Propojené účtu úložiště k oblasti vytvořen toku analýzy práce v, nejsou speciálně k projektu samotné.
>
> Všechny analýzy toku úloha (a dalších Azure zdrojů) ve stejné oblasti sdílet tento účet úložiště pro uložení dat sledování. Pokud zadáte účet jiné úložiště, může dojít před nechtěným vedlejší efekty sledování jiných úlohy toku analýzy a/nebo další Azure zdroje.
>
> Název účtu úložiště slouží k nahrazení ```“<YOUR STORAGE ACCOUNT NAME>”``` níže by měl být úložiště účtu, který je ve stejném předplatném jako analýzy toku projektu můžete povolit sledování.

    // Get an existing Stream Analytics job
    JobGetParameters jobGetParameters = new JobGetParameters()
    {
        PropertiesToExpand = "inputs,transformation,outputs"
    };
    JobGetResponse jobGetResponse = streamAnalyticsClient.StreamingJobs.Get(resourceGroupName, streamAnalyticsJobName, jobGetParameters);

    // Enable monitoring
    ServiceDiagnosticSettingsPutParameters insightPutParameters = new ServiceDiagnosticSettingsPutParameters()
    {
            Properties = new ServiceDiagnosticSettings()
            {
                StorageAccountName = "<YOUR STORAGE ACCOUNT NAME>"
            }
    };
    insightsClient.ServiceDiagnosticSettingsOperations.Put(jobGetResponse.Job.Id, insightPutParameters);



## <a name="get-support"></a>Získání podpory
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Další kroky

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)
