<properties
    pageTitle="Správa .NET SDK pro analýzy toku | Microsoft Azure"
    description="Začínáme s toku analýzy Management .NET SDK. Zjistěte, jak se dají vytvořit a spustit analýzy úlohy: vytvoření projektu, vstupy, výstupy a transformace."
    keywords=".NET SDK, analýzy rozhraní API"
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


# <a name="management-net-sdk-set-up-and-run-analytics-jobs-using-the-azure-stream-analytics-api-for-net"></a>Správa .NET SDK: Nastavení a spuštění analýzy úlohy pomocí rozhraní API Azure toku Analytics pro .NET

Zjistěte, jak nastavit spuštění analýzy úlohy pomocí rozhraní API toku Analytics pro .NET pomocí .NET SDK správy. Nastavení projektu, vytvořte vstupní a výstupní zdrojů, transformace a zahájení a ukončení úloh. Pro analýzy projekty můžete přenášet data z úložiště objektů Blob nebo rozbočovači události.

V tématu [Přehled správy si přečtěte následující dokumentaci pro rozhraní API toku Analytics pro .NET](https://msdn.microsoft.com/library/azure/dn889315.aspx).

Azure analýzy toku je plně spravovaných služba poskytuje zpracování nízkou latencí vysoce dostupné, scalable, složité události nad přenos dat v cloudu. Technologie pro analýzu toku umožňuje zákazníci zřídit streamování úlohy a analyzujte data datových proudů a umožňuje, aby jednotka poblíž v reálném čase analýzy.  


## <a name="prerequisites"></a>Zjistit předpoklady pro
Než začnete v tomto článku, musíte mít takto:

- Instalace aplikace Visual Studio 2012 nebo 2013.
- Stáhněte a nainstalujte [Azure.NET SDK](https://azure.microsoft.com/downloads/).
- Vytvoření skupiny zdroje Azure předplatné. Následujícím obrázku je ukázkový skript Powershellu Azure. Azure PowerShell informace najdete v tématu [instalace a konfigurace prostředí PowerShell Azure](../powershell-install-configure.md);  


        # Log in to your Azure account
        Add-AzureAccount

        # Select the Azure subscription you want to use to create the resource group
        Select-AzureSubscription -SubscriptionName <subscription name>

            # If Stream Analytics has not been registered to the subscription, remove the remark symbol (#) to run the Register-AzureRMProvider cmdlet to register the provider namespace
            #Register-AzureRMProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>


-   Nastavení vstupní zdroj a cílový výstup používat. Další pokyny najdete v článku [Přidání vstupy](stream-analytics-add-inputs.md) nastavit ukázková vstupní a [Přidání výstupy](stream-analytics-add-outputs.md) nastavit ukázkový výstup.


## <a name="set-up-a-project"></a>Nastavení projektu

Vytvořit analýzy úlohy pomocí rozhraní API toku Analytics pro .NET, musíte nejdřív nastavit projektu.

1. Vytvoření aplikace Visual Studio C# .NET konzoly.
2. V konzole Správce balíčků následující příkazy k instalaci balíčků NuGet. První z nich je Azure toku analýzy Management .NET SDK. Druhý je Azure Active Directory klienta, který bude sloužit k ověření.

        Install-Package Microsoft.Azure.Management.StreamAnalytics
        Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory

4. Přidejte následující části **appSettings** konfiguračního souboru:

        <appSettings>
          <!--CSM Prod related values-->
          <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
          <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
          <add key="WindowsManagementUri" value="https://management.core.windows.net/" />
          <add key="AsaClientId" value="1950a258-227b-4e31-a9cf-717495945fc2" />
          <add key="RedirectUri" value="urn:ietf:wg:oauth:2.0:oob" />
          <add key="SubscriptionId" value="YOUR AZURE SUBSCRIPTION" />
          <add key="ActiveDirectoryTenantId" value="YOU TENANT ID" />
        </appSettings>


    Nahradit hodnoty pro **SubscriptionId** a **ActiveDirectoryTenantId** Azure předplatné a klient ID. Spusťte následující rutinu Powershellu Azure si mohli udělat tyto hodnoty:

        Get-AzureAccount

5. Přidejte následující příkazy **pomocí** ve zdrojovém souboru (Program.cs) v projektu:

        using System;
        using System.Configuration;
        using System.Threading;
        using Microsoft.Azure;
        using Microsoft.Azure.Management.StreamAnalytics;
        using Microsoft.Azure.Management.StreamAnalytics.Models;
        using Microsoft.IdentityModel.Clients.ActiveDirectory;

6.  Přidání metody ověřování helper:

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


## <a name="create-a-stream-analytics-management-client"></a>Vytvořit klienta řízení toku analýzy

Objekt **StreamAnalyticsManagementClient** umožňuje spravovat úkoly a součásti úlohy, například zadání vstupních hodnot, výstup a transformace.

Na začátek metodu **hlavní** přidáte následující kód:

    string resourceGroupName = "<YOUR AZURE RESOURCE GROUP NAME>";
    string streamAnalyticsJobName = "<YOUR STREAM ANALYTICS JOB NAME>";
    string streamAnalyticsInputName = "<YOUR JOB INPUT NAME>";
    string streamAnalyticsOutputName = "<YOUR JOB OUTPUT NAME>";
    string streamAnalyticsTransformationName = "<YOUR JOB TRANSFORMATION NAME>";

    // Get authentication token
    TokenCloudCredentials aadTokenCredentials =
        new TokenCloudCredentials(
            ConfigurationManager.AppSettings["SubscriptionId"],
            GetAuthorizationHeader());

    // Create Stream Analytics management client
    StreamAnalyticsManagementClient client = new StreamAnalyticsManagementClient(aadTokenCredentials);

Hodnota proměnné **resourceGroupName** by měl být stejný jako název skupiny prostředků vytvořené nebo vybraných kontakty v základní kroky.

K automatizaci poměr prezentace pověření vytvoření projektu, podívejte se do [ověřování služby jistinu pomocí Správce prostředků Azure](../resource-group-authenticate-service-principal.md).

V dalších částech tohoto článku se předpokládá, že tento kód je na začátku metodu **hlavní** .

## <a name="create-a-stream-analytics-job"></a>Vytvoření úlohy toku analýzy

Následující kód vytvoří toku analýzy úlohy v části Skupina zdroje, který jste definovali. Přidáte vstup, výstup a transformace úlohy později.

    // Create a Stream Analytics job
    JobCreateOrUpdateParameters jobCreateParameters = new JobCreateOrUpdateParameters()
    {
        Job = new Job()
        {
            Name = streamAnalyticsJobName,
            Location = "<LOCATION>",
            Properties = new JobProperties()
            {
                EventsOutOfOrderPolicy = EventsOutOfOrderPolicy.Adjust,
                Sku = new Sku()
                {
                    Name = "Standard"
                }
            }
        }
    };

    JobCreateOrUpdateResponse jobCreateResponse = client.StreamingJobs.CreateOrUpdate(resourceGroupName, jobCreateParameters);


## <a name="create-a-stream-analytics-input-source"></a>Vytvoření pramenu vstupní toku analýzy

Následující kód vytvoří vstupní zdroj toku analýzy objektů blob vstupní zdroj typem a serializace CSV. Pokud chcete vytvořit zdrojem vstupní centrální událostí, použijte místo **BlobStreamInputDataSource** **EventHubStreamInputDataSource** . Podobně můžete přizpůsobit serializace typ vstupní zdroj.

    // Create a Stream Analytics input source
    InputCreateOrUpdateParameters jobInputCreateParameters = new InputCreateOrUpdateParameters()
    {
        Input = new Input()
        {
            Name = streamAnalyticsInputName,
            Properties = new StreamInputProperties()
            {
                Serialization = new CsvSerialization
                {
                    Properties = new CsvSerializationProperties
                    {
                        Encoding = "UTF8",
                        FieldDelimiter = ","
                    }
                },
                DataSource = new BlobStreamInputDataSource
                {
                    Properties = new BlobStreamInputDataSourceProperties
                    {
                        StorageAccounts = new StorageAccount[]
                        {
                            new StorageAccount()
                            {
                                AccountName = "<YOUR STORAGE ACCOUNT NAME>",
                                AccountKey = "<YOUR STORAGE ACCOUNT KEY>"
                            }
                        },
                        Container = "samples",
                        PathPattern = ""
                    }
                }
            }
        }
    };

    InputCreateOrUpdateResponse inputCreateResponse =
        client.Inputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobInputCreateParameters);

Zadávání zdrojů z úložiště objektů Blob nebo rozbočovači událostí je se stejným konkrétní úlohu. Použít stejné vstupní zdroj u různých projektů, musíte volat metodu znovu a zadejte jiný název úlohy.


## <a name="test-a-stream-analytics-input-source"></a>Testování zdroje vstupní toku analýzy

Metodu **TestConnection** testuje, zda je možné se připojit k vstupní zdroj, jakož i další aspekty specifické pro typ vstupní zdroje úlohy toku analýzy. Například v objektů blob vstupní zdroj, který jste vytvořili v předchozím kroku, metodu bude zkontrolujte, že název účtu úložiště a pár klíče slouží k připojení k účtu úložiště i zkontrolujte, zda zadaného kontejneru existuje.

    // Test input source connection
    DataSourceTestConnectionResponse inputTestResponse =
        client.Inputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsInputName);

## <a name="create-a-stream-analytics-output-target"></a>Vytvoření cílový výstup toku analýzy

Vytváření cílový výstup je velmi podobné vytváření analýz toku vstupní zdroje. Jako vstupní zdrojů je výstup cílů stejným konkrétní práci. Pokud chcete použít stejné cílový výstup pro různé úlohy, musíte volat metodu znovu a zadejte jiný název úlohy.

Následující kód vytvoří cílový výstup (databáze Azure SQL). Můžete přizpůsobit cílový výstup datový typ a/nebo typ serializace.

    // Create a Stream Analytics output target
    OutputCreateOrUpdateParameters jobOutputCreateParameters = new OutputCreateOrUpdateParameters()
    {
        Output = new Output()
        {
            Name = streamAnalyticsOutputName,
            Properties = new OutputProperties()
            {
                DataSource = new SqlAzureOutputDataSource()
                {
                    Properties = new SqlAzureOutputDataSourceProperties()
                    {
                        Server = "<YOUR DATABASE SERVER NAME>",
                        Database = "<YOUR DATABASE NAME>",
                        User = "<YOUR DATABASE LOGIN>",
                        Password = "<YOUR DATABASE LOGIN PASSWORD>",
                        Table = "<YOUR DATABASE TABLE NAME>"
                    }
                }
            }
        }
    };

    OutputCreateOrUpdateResponse outputCreateResponse =
        client.Outputs.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, jobOutputCreateParameters);

## <a name="test-a-stream-analytics-output-target"></a>Testování cílový výstup toku analýzy

Cílový výstup toku analýzy má i metody **TestConnection** testování připojení.

    // Test output target connection
    DataSourceTestConnectionResponse outputTestResponse =
        client.Outputs.TestConnection(resourceGroupName, streamAnalyticsJobName, streamAnalyticsOutputName);

## <a name="create-a-stream-analytics-transformation"></a>Vytvořte transformaci toku analýzy

Následující kód vytvoří transformace toku analýzy s dotazem "vyberte * ze vstupní" a určuje přidělit datové proudy jednu jednotku pro danou úlohu toku analýzy. Další informace o úpravách streamování jednotky najdete v článku [úlohy měřítko Azure toku analýzy](stream-analytics-scale-jobs.md).


    // Create a Stream Analytics transformation
    TransformationCreateOrUpdateParameters transformationCreateParameters = new TransformationCreateOrUpdateParameters()
    {
        Transformation = new Transformation()
        {
            Name = streamAnalyticsTransformationName,
            Properties = new TransformationProperties()
            {
                StreamingUnits = 1,
                Query = "select * from Input"
            }
        }
    };

    var transformationCreateResp =
        client.Transformations.CreateOrUpdate(resourceGroupName, streamAnalyticsJobName, transformationCreateParameters);

Jako vstupní a výstupní transformace také stejným pro určitý projekt analýzy toku, který byl vytvořený v části.

## <a name="start-a-stream-analytics-job"></a>Spuštění úlohy toku analýzy
Po vytvoření úlohy toku technologie pro analýzu a jeho input(s) output(s) a transformace, můžete začít úlohy tak, že zavoláte metoda **Start** .

Následující ukázkový kód spuštění analýzy toku úlohu s čas začátku vlastní výstup nastavit 12 prosinec 2012 12:12:12 UTC:

    // Start a Stream Analytics job
    JobStartParameters jobStartParameters = new JobStartParameters
    {
        OutputStartMode = OutputStartMode.CustomTime,
        OutputStartTime = new DateTime(2012, 12, 12, 0, 0, 0, DateTimeKind.Utc)
    };

    LongRunningOperationResponse jobStartResponse = client.StreamingJobs.Start(resourceGroupName, streamAnalyticsJobName, jobStartParameters);



## <a name="stop-a-stream-analytics-job"></a>Ukončit úlohu toku analýzy
Ukončení spuštěná úloha toku analýzy tak, že zavoláte metoda **Stop** .

    // Stop a Stream Analytics job
    LongRunningOperationResponse jobStopResponse = client.StreamingJobs.Stop(resourceGroupName, streamAnalyticsJobName);

## <a name="delete-a-stream-analytics-job"></a>Odstranění úlohy toku analýzy
Metoda **Delete** odstraníte úlohy a také základní dílčí zdrojů, včetně input(s) output(s) a transformace úlohy.

    // Delete a Stream Analytics job
    LongRunningOperationResponse jobDeleteResponse = client.StreamingJobs.Delete(resourceGroupName, streamAnalyticsJobName);


## <a name="get-support"></a>Získání podpory
Další pomoc Vyzkoušejte naše [Fórum komunity Azure toku analýzy](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).


## <a name="next-steps"></a>Další kroky

Jste seznámení se základy používání .NET SDK k vytvoření a spuštění analýzy úloh. Další informace najdete v těchto článcích:

- [Úvod do technologie pro analýzu Azure toku](stream-analytics-introduction.md)
- [Začínáme s používáním analýzy toku Azure](stream-analytics-get-started.md)
- [Změna velikosti úlohy Azure toku analýzy](stream-analytics-scale-jobs.md)
- [Azure toku analýzy Management .NET SDK](https://msdn.microsoft.com/library/azure/dn889315.aspx).
- [Odkaz na Azure toku analýzy dotaz jazyka](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure toku analýzy správy REST API odkaz](https://msdn.microsoft.com/library/azure/dn835031.aspx)


<!--Image references-->
[5]: ./media/markdown-template-for-new-articles/octocats.png
[6]: ./media/markdown-template-for-new-articles/pretty49.png
[7]: ./media/markdown-template-for-new-articles/channel-9.png


<!--Link references-->
[azure.blob.storage]: http://azure.microsoft.com/documentation/services/storage/
[azure.blob.storage.use]: http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/

[azure.event.hubs]: http://azure.microsoft.com/services/event-hubs/
[azure.event.hubs.developer.guide]: http://msdn.microsoft.com/library/azure/dn789972.aspx

[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.forum]: http://go.microsoft.com/fwlink/?LinkId=512151

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.developer.guide]: stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
