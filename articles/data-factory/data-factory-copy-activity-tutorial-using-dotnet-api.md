<properties 
    pageTitle="Kurz: Vytváření kanálů se kopie aktivity pomocí rozhraní API .NET | Microsoft Azure" 
    description="V tomto kurzu vytvoříte Azure Data Factory kanálu aktivitu kopírovat pomocí rozhraní API .NET." 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="10/27/2016" 
    ms.author="spelluru"/>

# <a name="tutorial-create-a-pipeline-with-copy-activity-using-net-api"></a>Kurz: Vytváření kanálů se kopie aktivity pomocí rozhraní API .NET
> [AZURE.SELECTOR]
- [Přehled a požadavky](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
- [Kopírování Průvodce](data-factory-copy-data-wizard-tutorial.md)
- [Azure portálu](data-factory-copy-activity-tutorial-using-azure-portal.md)
- [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
- [Prostředí PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
- [Azure správce prostředků šablony](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
- [ROZHRANÍ REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
- [ROZHRANÍ API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)


Tento kurz se dozvíte, jak vytvořit a sledovat factory Azure dat pomocí rozhraní API .NET. Kanálu v factory dat slouží ke kopírování dat z úložiště objektů Blob Azure k databázi SQL Azure aktivitu na Kopírovat.

Aktivity kopírovat provede přesun dat v Azure Data Factory. Aktivity používá technologii globálně dostupná služba, která můžete přesouvat data mezi různých úložiště dat zabezpečené spolehlivý a scalable způsobem. Článek [Aktivity přesun dat](data-factory-data-movement-activities.md) najdete v článku podrobné informace o aktivitě kopírovat.   

> [AZURE.NOTE] 
> Tento článek se nezabývá všechny rozhraní API .NET Factory Data. Přečtěte si článek [dat Factory .NET rozhraní API](https://msdn.microsoft.com/library/mt415893.aspx) podrobnosti o Data Factory .NET SDK. 

## <a name="prerequisites"></a>Zjistit předpoklady pro
- Projděte [Přehled a předpoklady](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) získejte přehled o kurzu a postupujte podle pokynů **předpoklad** . 
- Visual Studio 2012 nebo 2013 nebo 2015
- Stažení a instalace [Azure.NET SDK](http://azure.microsoft.com/downloads/)
- Azure Powershellu. Postupujte podle pokynů v článku [Jak nainstalovat a nakonfigurovat Azure PowerShell](../powershell-install-configure.md) nainstalovat prostředí PowerShell Azure ve vašem počítači. Pomocí prostředí PowerShell Azure vytvořit aplikaci služby Azure Active Directory.

### <a name="create-an-application-in-azure-active-directory"></a>Vytvoření aplikace služby Azure Active Directory
Vytvoření aplikace služby Azure Active Directory, vytvořit hlavní název služby pro aplikaci a přiřadit ji někomu role **Přispěvatele Factory Data** .  

1. Spusťte **Powershellu**. 
1. Spusťte tento příkaz a zadejte uživatelské jméno a heslo, které používáte pro přihlášení k portálu Azure.
    
        Login-AzureRmAccount   
2. Spusťte tento příkaz Zobrazit všechna předplatná pro tento účet.

        Get-AzureRmSubscription 
3. Spusťte tento příkaz vyberte předplatné, ke kterým chcete pracovat s. Nahrazení ** &lt;NameOfAzureSubscription** &gt; s názvem předplatného Azure. 

        Get-AzureRmSubscription -SubscriptionName <NameOfAzureSubscription> | Set-AzureRmContext

    > [AZURE.IMPORTANT] Poznamenejte si **SubscriptionId** a **TenantId** z výstupu tento příkaz. 
4. Vytvoření skupiny Azure zdroje s názvem **ADFTutorialResourceGroup** spuštěním následujícího příkazu v Powershellu.  

        New-AzureRmResourceGroup -Name ADFTutorialResourceGroup  -Location "West US"

    Pokud již existuje skupina zdroje, můžete určit, zda k aktualizaci ho (Y) nebo uchovat jako (N). 

    Pokud používáte jiné skupině zdrojů, budete muset nesou název této skupiny zdrojů místo ADFTutorialResourceGroup v tomto kurzu.
5. Vytvoření aplikace služby Azure Active Directory. 

        $azureAdApplication = New-AzureRmADApplication -DisplayName "ADFCopyTutotiralApp" -HomePage "https://www.contoso.org" -IdentifierUris "https://www.adfcopytutorialapp.org/example" -Password "Pass@word1"

    Pokud se zobrazí chybová zpráva, zadat jinou adresu URL a znovu spusťte příkaz. 

        Another object with the same value for property identifierUris already exists.

6. Vytvoření AD jistinu služby. 

        New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

7. Přidání služby jistinu do role **Přispěvatele Factory Data** . 

        New-AzureRmRoleAssignment -RoleDefinitionName "Data Factory Contributor" -ServicePrincipalName $azureAdApplication.ApplicationId.Guid

8. Získání ID aplikace.

        $azureAdApplication

    Poznamenejte si ID aplikace (**applicationID** z výstupu).

Měli byste mít následujících čtyř hodnot z těchto kroků: 

- ID klienta
- ID předplatného
- ID aplikace 
- Heslo (podle prvního příkazu)   

## <a name="walkthrough"></a>Návod
1. Pomocí aplikace Visual Studio 2012/2013/2015 můžete vytvořte aplikace konzoly C# .NET.
    1. Spuštění **aplikace Visual Studio** 2012/2013/2015.
    2. Klikněte na **soubor**, přejděte na **Nový**a klikněte na **projekt**.
    3. Rozbalení **šablony**a vyberte **Visual Basic**. V tomto návodu použijete C#, ale můžete použít libovolný jazyk .NET.
    4. Ze seznamu typů projektů na pravé straně zvolte **Aplikace konzoly** .
    5. Zadejte název **DataFactoryAPITestApp** .
    6. Vyberte **C:\ADFGetStarted** umístění.
    7. Klikněte na **OK** vytvořte projekt.
2. Klikněte na **Nástroje**, přejděte na **Správce Nuget balíčku**a klikněte na **Správce balíčků konzoly**.
3.  V dialogovém okně **Správce balíčků konzoly**proveďte následující kroky: 
    1.  Spusťte tento příkaz Data Factory balíček nainstalovat:`Install-Package Microsoft.Azure.Management.DataFactories`       
    2.  Spusťte tento příkaz nainstalovat Azure Active Directory (použít rozhraní API Active Directory v kódu):`Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.19.208020213`
6. V následující části **appSetttings** dodejte **konfiguračního** souboru. Toto nastavení slouží metodou helper: **GetAuthorizationHeader**. 

    Nahrazení hodnot pro ** &lt;ID aplikace&gt;**, ** &lt;heslo&gt;**, ** &lt;ID předplatného&gt;**, a ** &lt;klient ID&gt; ** s vlastními hodnotami. 

        <?xml version="1.0" encoding="utf-8" ?>
        <configuration>
            <startup> 
                <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5.2" />
            </startup>
            <appSettings>
                <add key="ActiveDirectoryEndpoint" value="https://login.windows.net/" />
                <add key="ResourceManagerEndpoint" value="https://management.azure.com/" />
                <add key="WindowsManagementUri" value="https://management.core.windows.net/" />

                <add key="ApplicationId" value="your application ID" />
                <add key="Password" value="Password you used while creating the AAD application" />
                <add key="SubscriptionId" value= "Subscription ID" />
                <add key="ActiveDirectoryTenantId" value="Tenant ID" />
            </appSettings>
        </configuration>
6. Přidejte následující příkazy **pomocí** ve zdrojovém souboru (Program.cs) v projektu.

        using System.Threading;
        using System.Configuration;
        using System.Collections.ObjectModel;
        
        using Microsoft.Azure.Management.DataFactories;
        using Microsoft.Azure.Management.DataFactories.Models;
        using Microsoft.Azure.Management.DataFactories.Common.Models;
        
        using Microsoft.IdentityModel.Clients.ActiveDirectory;
        using Microsoft.Azure;
6. Přidejte následující kód, který vytváří instanci třídy **DataPipelineManagementClient** metody **hlavní** . Tento objekt použijete k vytváření factory dat, propojené služby, vstupní a výstupní datové sady a potrubí. Sledování výseče datovou sadu běhu taky pomocí tohoto objektu.    

            // create data factory management client
            string resourceGroupName = "ADFTutorialResourceGroup";
            string dataFactoryName = "APITutorialFactory";

            TokenCloudCredentials aadTokenCredentials =
                new TokenCloudCredentials(
                    ConfigurationManager.AppSettings["SubscriptionId"],
                    GetAuthorizationHeader());

            Uri resourceManagerUri = new Uri(ConfigurationManager.AppSettings["ResourceManagerEndpoint"]);

            DataFactoryManagementClient client = new DataFactoryManagementClient(aadTokenCredentials, resourceManagerUri);

    > [AZURE.IMPORTANT] 
    > Nahraďte hodnotu **resourceGroupName** na název skupiny Azure zdroje. 
    > 
    > Aktualizujte název factory dat (**dataFactoryName**) být jedinečné. Název data factory musí být jedinečné. Pojmenování pravidla pro Data Factory artefakty naleznete v tématu [Data Factory - pojmenování pravidla](data-factory-naming-rules.md) . 

7. Přidejte následující kód, který vytvoří **dat factory** metody **hlavní** .

            // create a data factory
            Console.WriteLine("Creating a data factory");
            client.DataFactories.CreateOrUpdate(resourceGroupName,
                new DataFactoryCreateOrUpdateParameters()
                {
                    DataFactory = new DataFactory()
                    {
                        Name = dataFactoryName,
                        Location = "westus",
                        Properties = new DataFactoryProperties() { }
                    }
                }
            );

8. Přidejte následující kód, který vytvoří **úložišti Azure propojené služby** metodu **hlavní** . 

    > [AZURE.IMPORTANT] Nahraďte **storageaccountname** a **accountkey** název a klíč účtu Azure úložiště. 

            // create a linked service for input data store: Azure Storage
            Console.WriteLine("Creating Azure Storage linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureStorageLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureStorageLinkedService("DefaultEndpointsProtocol=https;AccountName=<storageaccountname>;AccountKey=<accountkey>")
                        )
                    }
                }
            );
9. Přidejte následující kód, který vytvoří **Azure SQL propojené služby** metodu **hlavní** .
 
    > [AZURE.IMPORTANT] Nahraďte **webu název serveru**, **název databáze**, **uživatelské jméno**a **heslo** názvy Azure SQL serveru, databázi, uživatele, a heslo.  

            // create a linked service for output data store: Azure SQL Database
            Console.WriteLine("Creating Azure SQL Database linked service");
            client.LinkedServices.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new LinkedServiceCreateOrUpdateParameters()
                {
                    LinkedService = new LinkedService()
                    {
                        Name = "AzureSqlLinkedService",
                        Properties = new LinkedServiceProperties
                        (
                            new AzureSqlDatabaseLinkedService("Data Source=tcp:<servername>.database.windows.net,1433;Initial Catalog=<databasename>;User ID=<username>;Password=<password>;Integrated Security=False;Encrypt=True;Connect Timeout=30")
                        )
                    }
                }
            );
10. Přidejte následující kód, který vytvoří metody **hlavní** **vstupní a výstupní datové sady** . 

            // create input and output datasets
            Console.WriteLine("Creating input and output datasets");
            string Dataset_Source = "DatasetBlobSource";
            string Dataset_Destination = "DatasetAzureSqlDestination";

            Console.WriteLine("Creating input dataset of type: Azure Blob");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,


            new DatasetCreateOrUpdateParameters()
            {
                Dataset = new Dataset()
                {
                    Name = Dataset_Source,
                    Properties = new DatasetProperties()
                    {
                        Structure = new List<DataElement>()
                        {
                            new DataElement() { Name = "FirstName", Type = "String" },
                            new DataElement() { Name = "LastName", Type = "String" }
                        },
                        LinkedServiceName = "AzureStorageLinkedService",
                        TypeProperties = new AzureBlobDataset()
                        {
                            FolderPath = "adftutorial/",
                            FileName = "emp.txt"
                        },
                        External = true,
                        Availability = new Availability()
                        {
                            Frequency = SchedulePeriod.Hour,
                            Interval = 1,
                        },

                        Policy = new Policy()
                        {
                            Validation = new ValidationPolicy()
                            {
                                MinimumRows = 1
                            }
                        }
                    }
                }
            });


            Console.WriteLine("Creating output dataset of type: Azure SQL");
            client.Datasets.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new DatasetCreateOrUpdateParameters()
                {
                    Dataset = new Dataset()
                    {
                        Name = Dataset_Destination,
                        Properties = new DatasetProperties()
                        {
                            Structure = new List<DataElement>()
                            {
                                new DataElement() { Name = "FirstName", Type = "String" },
                                new DataElement() { Name = "LastName", Type = "String" }
                            },
                            LinkedServiceName = "AzureSqlLinkedService",
                            TypeProperties = new AzureSqlTableDataset()
                            {
                                TableName = "emp"
                            },

                            Availability = new Availability()
                            {
                                Frequency = SchedulePeriod.Hour,
                                Interval = 1,
                            },
                        }
                    }
                });

11. Přidejte následující kód se **vytvoří a aktivuje potrubí** metody **hlavní** . Tento kanál má **CopyActivity** trvá **BlobSource** jako zdroj a **BlobSink** jako jímka.

            // create a pipeline
            Console.WriteLine("Creating a pipeline");
            DateTime PipelineActivePeriodStartTime = new DateTime(2016, 8, 9, 0, 0, 0, 0, DateTimeKind.Utc);
            DateTime PipelineActivePeriodEndTime = PipelineActivePeriodStartTime.AddMinutes(60);
            string PipelineName = "ADFTutorialPipeline";

            client.Pipelines.CreateOrUpdate(resourceGroupName, dataFactoryName,
                new PipelineCreateOrUpdateParameters()
                {
                    Pipeline = new Pipeline()
                    {
                        Name = PipelineName,
                        Properties = new PipelineProperties()
                        {
                            Description = "Demo Pipeline for data transfer between blobs",

                    // Initial value for pipeline's active period. With this, you won't need to set slice status
                    Start = PipelineActivePeriodStartTime,
                            End = PipelineActivePeriodEndTime,

                            Activities = new List<Activity>()
                            {
                                new Activity()
                                {
                                    Name = "BlobToAzureSql",
                                    Inputs = new List<ActivityInput>()
                                    {
                                        new ActivityInput() {
                                            Name = Dataset_Source
                                        }
                                    },
                                    Outputs = new List<ActivityOutput>()
                                    {
                                        new ActivityOutput()
                                        {
                                            Name = Dataset_Destination
                                        }
                                    },
                                    TypeProperties = new CopyActivity()
                                    {
                                        Source = new BlobSource(),
                                        Sink = new BlobSink()
                                        {
                                            WriteBatchSize = 10000,
                                            WriteBatchTimeout = TimeSpan.FromMinutes(10)
                                        }
                                    }
                                }
                            },
                        }
                    }
                }); 

12. Přidejte následující kód metody **hlavní** stav výseč výstup datové sady. Existuje pouze výsečí v tomto příkladu by měly.   
 
            // Pulling status within a timeout threshold
            DateTime start = DateTime.Now;
            bool done = false;

            while (DateTime.Now - start < TimeSpan.FromMinutes(5) && !done)
            {
                Console.WriteLine("Pulling the slice status");
                // wait before the next status check
                Thread.Sleep(1000 * 12);

                var datalistResponse = client.DataSlices.List(resourceGroupName, dataFactoryName, Dataset_Destination,
                    new DataSliceListParameters()
                    {
                        DataSliceRangeStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString(),
                        DataSliceRangeEndTime = PipelineActivePeriodEndTime.ConvertToISO8601DateTimeString()
                    });

                foreach (DataSlice slice in datalistResponse.DataSlices)
                {
                    if (slice.State == DataSliceState.Failed || slice.State == DataSliceState.Ready)
                    {
                        Console.WriteLine("Slice execution is done with status: {0}", slice.State);
                        done = true;
                        break;
                    }
                    else
                    {
                        Console.WriteLine("Slice status is: {0}", slice.State);
                    }
                }
            }

14. Přidejte následující kód provádět získat podrobnosti řezu dat metody **hlavní** .

            Console.WriteLine("Getting run details of a data slice");

            // give it a few minutes for the output slice to be ready
            Console.WriteLine("\nGive it a few minutes for the output slice to be ready and press any key.");
            Console.ReadKey();

            var datasliceRunListResponse = client.DataSliceRuns.List(
                    resourceGroupName,
                    dataFactoryName,
                    Dataset_Destination,
                    new DataSliceRunListParameters()
                    {
                        DataSliceStartTime = PipelineActivePeriodStartTime.ConvertToISO8601DateTimeString()
                    }
                );

            foreach (DataSliceRun run in datasliceRunListResponse.DataSliceRuns)
            {
                Console.WriteLine("Status: \t\t{0}", run.Status);
                Console.WriteLine("DataSliceStart: \t{0}", run.DataSliceStart);
                Console.WriteLine("DataSliceEnd: \t\t{0}", run.DataSliceEnd);
                Console.WriteLine("ActivityId: \t\t{0}", run.ActivityName);
                Console.WriteLine("ProcessingStartTime: \t{0}", run.ProcessingStartTime);
                Console.WriteLine("ProcessingEndTime: \t{0}", run.ProcessingEndTime);
                Console.WriteLine("ErrorMessage: \t{0}", run.ErrorMessage);
            }

            Console.WriteLine("\nPress any key to exit.");
            Console.ReadKey();

13. Přidejte následující helper metodou metodou **hlavní** třídy **aplikace** .  
 
        public static string GetAuthorizationHeader()
        {
            AuthenticationResult result = null;
            var thread = new Thread(() =>
            {
                try
                {
                    var context = new AuthenticationContext(ConfigurationManager.AppSettings["ActiveDirectoryEndpoint"] + ConfigurationManager.AppSettings["ActiveDirectoryTenantId"]);

                    ClientCredential credential = new ClientCredential(ConfigurationManager.AppSettings["ApplicationId"], ConfigurationManager.AppSettings["Password"]);
                    result = context.AcquireToken(resource: ConfigurationManager.AppSettings["WindowsManagementUri"], clientCredential: credential);
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


15. V okně Průzkumník rozbalte project (**DataFactoryAPITestApp**), klikněte pravým tlačítkem na **odkazy**a klikněte na **Přidat odkaz**. Zaškrtněte políčko pro sestavení "**System.Configuration**" a klikněte na **OK**. 
16. Vytvoření aplikace konzoly. V nabídce klikněte na **vytvořit** a klikněte na tlačítko **Sestavit řešení**. 
16. Ověřte, že je alespoň jeden soubor v kontejneru **adftutorial** v úložišti objektů blob Azure. Pokud ne, vytvořte **Emp.txt** soubor v poznámkovém bloku s následující obsah a uložte do kontejneru adftutorial.

        John, Doe
        Jane, Doe
     
17. Spuštění vzorku kliknutím **ladění** -> **Spustit ladění** v nabídce. Pokud se zobrazí, **začíná spustit podrobnosti výseč**, počkejte pár minut a stiskněte klávesu **ENTER**. 
18. Použití portálu Azure k ověření dat factory **APITutorialFactory** se vytvoří pomocí následující artefakty: 
    - Propojené služby: **LinkedService_AzureStorage** 
    - Datová sada: **DatasetBlobSource** a **DatasetBlobDestination**.
    - **PipelineBlobSample** kanálem k odesílání zpráv: 
18. Ověřte, že v záznamech dva zaměstnance jsou v tabulce "**emp**" v zadané databázi Azure SQL.

## <a name="next-steps"></a>Další kroky

- Prostudujte článek [Aktivity pohyb dat](data-factory-data-movement-activities.md) , který obsahuje podrobné informace o aktivity kopie použité v tomto kurzu.
- Přečtěte si článek [dat Factory .NET rozhraní API](https://msdn.microsoft.com/library/mt415893.aspx) podrobnosti o Data Factory .NET SDK. Tento článek se nezabývá všechny rozhraní API .NET Factory Data. 

 
