<properties
   pageTitle="Používání Data jezera analýzy Java SDK pro vývoj aplikací | Azure"
   description="Používání Azure Data jezera analýzy Java SDK pro vývoj aplikací"
   services="data-lake-analytics"
   documentationCenter=""
   authors="edmacauley"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="05/16/2016"
   ms.author="edmaca"/>

# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-java-sdk"></a>Kurz: Začínáme s Azure dat jezera technologie pro analýzu použití Java SDK

[AZURE.INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Naučte se používat Azure Data jezera analýzy Java SDK vytvořte účet Azure dat jezera a provádění základních operací, například při vytvoření složek, uložit a stáhnout datové soubory, odstranění účtu a práce s projekty. Další informace o jezera dat najdete v článku [Jezera analýzy dat Azure](data-lake-analytics-overview.md).

V tomto kurzu se dají aplikace konzoly Java, který obsahuje příklady běžných úkolů správy i testovací data vytvoření a odeslání úlohy.  Projít stejné kurz používání jiných podporovaných nástrojů, klikněte na karty v horní v této části.

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java Development Kit (JDK) 8 (pomocí jazyka Java verze 1.8).
* IntelliJ nebo jiné vhodné Java vývojové prostředí. Toto je nepovinný doporučené. Tyto kroky udělat pomocí IntelliJ.
* **Azure předplatného**. Viz [získání Azure bezplatnou zkušební verzi](https://azure.microsoft.com/pricing/free-trial/).
* **Povolení předplatného Azure** analýzy dat jezera veřejné (verze Preview). V tématu [pokyny](data-lake-analytics-get-started-portal.md#signup).
* Vytvoření aplikace Azure Active Directory (AAD) a získat své **ID klienta** **ID klienta**a **klíče**. Podrobnosti o aplikacích AAD a pokyny o tom, jak získat kód klienta najdete v článku [Vytvoření Active Directory aplikace a služby základní portálu](../resource-group-create-service-principal-portal.md). Identifikátor URI odpovědět a klíč také bude k dispozici z portálu až budete mít aplikaci vytvořili a generovaného klíče.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřit, pomocí služby Azure Active Directory?

Fragment kódu obsahuje kódy pro **interaktivním** ověřování, které aplikace obsahuje vlastní přihlašovací údaje.

Bude potřeba udělit oprávnění aplikace vytvoření zdrojů v Azure pro účely tohoto návodu pro práci. Je **důrazně doporučujeme** pouze umožnit oprávnění Přispěvatel této aplikace do skupiny nové nepoužívané a prázdné zdrojů v Azure předplatné pro účely tohoto kurzu.

## <a name="create-a-java-application"></a>Vytvoření aplikace Java

1. Otevřete IntelliJ a vytvořte nový projekt Java pomocí šablony **Aplikace příkazového řádku** .

2. Klikněte pravým tlačítkem myši na projektu v levé části obrazovky a klikněte na **Přidat Framework podporu**. Zvolte **Maven** a klikněte na **OK**.

3. Otevřete soubor nově vytvořený **"pom.xml"** a přidejte následující úryvek text mezi ** \</version >** značky a ** \</Projekt >** značku:

    Poznámka: Tento krok je dočasný, dokud nebude k dispozici v Maven Azure Data jezera analýzy SDK. Tento článek se aktualizují po SDK je k dispozici v Maven. Všechny budoucí aktualizace tento SDK budou dostupné prostřednictvím Maven.

        <repositories>
            <repository>
                <id>adx-snapshots</id>
                <name>Azure ADX Snapshots</name>
                <url>http://adxsnapshots.azurewebsites.net/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>oss-snapshots</id>
                <name>Open Source Snapshots</name>
                <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
                <layout>default</layout>
                <snapshots>
                    <enabled>true</enabled>
                    <updatePolicy>always</updatePolicy>
                </snapshots>
            </repository>
        </repositories>
        <dependencies>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-authentication</artifactId>
                <version>1.0.0-20160513.000802-24</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-client-runtime</artifactId>
                <version>1.0.0-20160513.000812-28</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.rest</groupId>
                <artifactId>client-runtime</artifactId>
                <version>1.0.0-20160513.000825-29</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-store</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
            <dependency>
                <groupId>com.microsoft.azure</groupId>
                <artifactId>azure-mgmt-datalake-analytics</artifactId>
                <version>1.0.0-SNAPSHOT</version>
            </dependency>
        </dependencies>


4. Přejděte na **soubor**, pak **Nastavení**a pak **Vytvoření**, **spuštění**, **nasazení**. Vyberte **Nástroje pro vytváření**, **Maven** **Import**. Zaškrtněte políčko **Maven importovat projekty automaticky**.

5. Otevřete **Main.java** a nahraďte stávající blok kódu následující kód. Také zadat hodnoty parametrů textem Schvalte dokument v kódu, například **localFolderPath** **_adlaAccountName** **_adlsAccountName** **_resourceGroupName** a nahraďte zástupné symboly pro **ID klienta**, **TAJNÁ klienta**, **ID klienta**a **ID PŘEDPLATNÉHO**.

    Tento kód prochází proces vytváření úložiště jezera dat a analýzy dat jezera účtů vytváření souborů v úložišti, spuštění úlohy, začíná stavu úlohy, stahování výstupu projektu a nakonec odstranění účtu.

    >[AZURE.NOTE] Známé problémy se službou Azure dat jezera aktuálně existuje.  Pokud aplikaci ukázka se přeruší nebo dojde k chybě, budete muset ručně odstranit úložiště jezera dat a analýzy dat jezera účty, které vytvoří skript.  Pokud znáte není portálu, průvodce [Správa Azure dat jezera analýzy pomocí portálu Azure](data-lake-analytics-manage-use-portal.md) vám pomůžou začít.


        package com.company;

        import com.microsoft.azure.CloudException;
        import com.microsoft.azure.credentials.ApplicationTokenCredentials;
        import com.microsoft.azure.management.datalake.store.*;
        import com.microsoft.azure.management.datalake.store.models.*;
        import com.microsoft.azure.management.datalake.analytics.*;
        import com.microsoft.azure.management.datalake.analytics.models.*;
        import com.microsoft.rest.credentials.ServiceClientCredentials;
        import java.io.*;
        import java.nio.charset.Charset;
        import java.nio.file.Files;
        import java.nio.file.Paths;
        import java.util.ArrayList;
        import java.util.UUID;
        import java.util.List;
        
        public class Main {
            private static String _adlsAccountName;
            private static String _adlaAccountName;
            private static String _resourceGroupName;
            private static String _location;
        
            private static String _tenantId;
            private static String _subId;
            private static String _clientId;
            private static String _clientSecret;
        
            private static DataLakeStoreAccountManagementClient _adlsClient;
            private static DataLakeStoreFileSystemManagementClient _adlsFileSystemClient;
            private static DataLakeAnalyticsAccountManagementClient _adlaClient;
            private static DataLakeAnalyticsJobManagementClient _adlaJobClient;
            private static DataLakeAnalyticsCatalogManagementClient _adlaCatalogClient;
        
            public static void main(String[] args) throws Exception {
                _adlsAccountName = "<DATA-LAKE-STORE-NAME>";
                _adlaAccountName = "<DATA-LAKE-ANALYTICS-NAME>";
                _resourceGroupName = "<RESOURCE-GROUP-NAME>";
                _location = "East US 2";
        
                _tenantId = "<TENANT-ID>";
                _subId =  "<SUBSCRIPTION-ID>";
                _clientId = "<CLIENT-ID>";
        
                _clientSecret = "<CLIENT-SECRET>"; // TODO: For production scenarios, we recommend that you replace this line with a more secure way of acquiring the application client secret, rather than hard-coding it in the source code.
        
                String localFolderPath = "C:\\local_path\\"; // TODO: Change this to any unused, new, empty folder on your local machine.
        
                // Authenticate
                ApplicationTokenCredentials creds = new ApplicationTokenCredentials(_clientId, _tenantId, _clientSecret, null);
                SetupClients(creds);
        
                // Create Data Lake Store and Analytics accounts
                WaitForNewline("Authenticated.", "Creating NEW accounts.");
                CreateAccounts();
                WaitForNewline("Accounts created.", "Displaying accounts.");
        
                // List Data Lake Store and Analytics accounts that this app can access
                System.out.println(String.format("All ADL Store accounts that this app can access in subscription %s:", _subId));
                List<DataLakeStoreAccount> adlsListResult = _adlsClient.getAccountOperations().list().getBody();
                for (DataLakeStoreAccount acct : adlsListResult) {
                    System.out.println(acct.getName());
                }
                System.out.println(String.format("All ADL Analytics accounts that this app can access in subscription %s:", _subId));
                List<DataLakeAnalyticsAccount> adlaListResult = _adlaClient.getAccountOperations().list().getBody();
                for (DataLakeAnalyticsAccount acct : adlaListResult) {
                    System.out.println(acct.getName());
                }
                WaitForNewline("Accounts displayed.", "Creating files.");
        
                // Create a file in Data Lake Store: input1.csv
                // TODO: these change order in the next patch
                byte[] bytesContents = "123,abc".getBytes();
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, "/input1.csv", bytesContents, true);
        
                WaitForNewline("File created.", "Submitting a job.");
        
                // Submit a job to Data Lake Analytics
                UUID jobId = SubmitJobByScript("@input =  EXTRACT Data string FROM \"/input1.csv\" USING Extractors.Csv(); OUTPUT @input TO @\"/output1.csv\" USING Outputters.Csv();", "testJob");
                WaitForNewline("Job submitted.", "Getting job status.");
        
                // Wait for job completion and output job status
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                System.out.println("Waiting for job completion.");
                WaitForJob(jobId);
                System.out.println(String.format("Job status: %s", GetJobStatus(jobId)));
                WaitForNewline("Job completed.", "Downloading job output.");
        
                // Download job output from Data Lake Store
                DownloadFile("/output1.csv", localFolderPath + "output1.csv");
                WaitForNewline("Job output downloaded.", "Deleting file.");
        
                // Delete file from Data Lake Store
                DeleteFile("/output1.csv");
                WaitForNewline("File deleted.", "Deleting account.");
        
                // Delete account
                _adlsClient.getAccountOperations().delete(_resourceGroupName, _adlsAccountName);
                _adlaClient.getAccountOperations().delete(_resourceGroupName, _adlaAccountName);
                WaitForNewline("Account deleted.", "DONE.");
            }
        
            //Set up clients
            public static void SetupClients(ServiceClientCredentials creds)
            {
                _adlsClient = new DataLakeStoreAccountManagementClientImpl(creds);
                _adlsFileSystemClient = new DataLakeStoreFileSystemManagementClientImpl(creds);
                _adlaClient = new DataLakeAnalyticsAccountManagementClientImpl(creds);
                _adlaJobClient = new DataLakeAnalyticsJobManagementClientImpl(creds);
                _adlaCatalogClient = new DataLakeAnalyticsCatalogManagementClientImpl(creds);
                _adlsClient.setSubscriptionId(_subId);
                _adlaClient.setSubscriptionId(_subId);
            }
        
            // Helper function to show status and wait for user input
            public static void WaitForNewline(String reason, String nextAction)
            {
                if (nextAction == null)
                    nextAction = "";
        
                System.out.println(reason + "\r\nPress ENTER to continue...");
                try{System.in.read();}
                catch(Exception e){}
        
                if (!nextAction.isEmpty())
                {
                    System.out.println(nextAction);
                }
            }
        
            // Create Data Lake Store and Analytics accounts
            public static void CreateAccounts() throws InterruptedException, CloudException, IOException {
                // Create ADLS account
                DataLakeStoreAccount adlsParameters = new DataLakeStoreAccount();
                adlsParameters.setLocation(_location);
        
                _adlsClient.getAccountOperations().create(_resourceGroupName, _adlsAccountName, adlsParameters);
        
                // Create ADLA account
                DataLakeStoreAccountInfo adlsInfo = new DataLakeStoreAccountInfo();
                adlsInfo.setName(_adlsAccountName);
        
                DataLakeStoreAccountInfoProperties adlsInfoProperties = new DataLakeStoreAccountInfoProperties();
                adlsInfo.setProperties(adlsInfoProperties);
        
                List<DataLakeStoreAccountInfo> adlsInfoList = new ArrayList<DataLakeStoreAccountInfo>();
                adlsInfoList.add(adlsInfo);
        
                DataLakeAnalyticsAccountProperties adlaProperties = new DataLakeAnalyticsAccountProperties();
                adlaProperties.setDataLakeStoreAccounts(adlsInfoList);
                adlaProperties.setDefaultDataLakeStoreAccount(_adlsAccountName);
        
                DataLakeAnalyticsAccount adlaParameters = new DataLakeAnalyticsAccount();
                adlaParameters.setLocation(_location);
                adlaParameters.setName(_adlaAccountName);
                adlaParameters.setProperties(adlaProperties);
        
                    /* If this line generates an error message like "The deep update for property 'DataLakeStoreAccounts' is not supported", please delete the ADLS and ADLA accounts via the portal and re-run your script. */
        
                _adlaClient.getAccountOperations().create(_resourceGroupName, _adlaAccountName, adlaParameters);
            }
        
            //todo: this changes in the next version of the API
            public static void CreateFile(String path, String contents, boolean force) throws IOException, CloudException {
                byte[] bytesContents = contents.getBytes();
        
                _adlsFileSystemClient.getFileSystemOperations().create(_adlsAccountName, path, bytesContents, force);
            }
        
            public static void DeleteFile(String filePath) throws IOException, CloudException {
                _adlsFileSystemClient.getFileSystemOperations().delete(filePath, _adlsAccountName);
            }
        
            // Download file
            public static void DownloadFile(String srcPath, String destPath) throws IOException, CloudException {
                InputStream stream = _adlsFileSystemClient.getFileSystemOperations().open(srcPath, _adlsAccountName).getBody();
        
                PrintWriter pWriter = new PrintWriter(destPath, Charset.defaultCharset().name());
        
                String fileContents = "";
                if (stream != null) {
                    Writer writer = new StringWriter();
        
                    char[] buffer = new char[1024];
                    try {
                        Reader reader = new BufferedReader(
                                new InputStreamReader(stream, "UTF-8"));
                        int n;
                        while ((n = reader.read(buffer)) != -1) {
                            writer.write(buffer, 0, n);
                        }
                    } finally {
                        stream.close();
                    }
                    fileContents =  writer.toString();
                }
        
                pWriter.println(fileContents);
                pWriter.close();
            }
        
            // Submit a U-SQL job by providing script contents.
            // Returns the job ID
            public static UUID SubmitJobByScript(String script, String jobName) throws IOException, CloudException {
                UUID jobId = java.util.UUID.randomUUID();
                USqlJobProperties properties = new USqlJobProperties();
                properties.setScript(script);
                JobInformation parameters = new JobInformation();
                parameters.setName(jobName);
                parameters.setJobId(jobId);
                parameters.setType(JobType.USQL);
                parameters.setProperties(properties);
        
                JobInformation jobInfo = _adlaJobClient.getJobOperations().create(_adlaAccountName, jobId, parameters).getBody();
        
                return jobId;
            }
        
            // Wait for job completion
            public static JobResult WaitForJob(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                while (jobInfo.getState() != JobState.ENDED)
                {
                    jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName,jobId).getBody();
                }
                return jobInfo.getResult();
            }
        
            // Get job status
            public static String GetJobStatus(UUID jobId) throws IOException, CloudException {
                JobInformation jobInfo = _adlaJobClient.getJobOperations().get(_adlaAccountName, jobId).getBody();
                return jobInfo.getState().toValue();
            }
        }

6. Podle pokynů na spustit a dokončete aplikace.


## <a name="see-also"></a>Viz taky

- Stejný kurz pomocí dalších nástrojů zobrazíte kliknutím na kartu voliče v horní části stránky.
- Složitější dotaz najdete v tématu [protokoly analyzovat webu pomocí analýzy jezera dat Azure](data-lake-analytics-analyze-weblogs.md).
- Začínáme s vývojem aplikací U SQL, najdete v článku [pomocí nástroje jezera datového Visual Studio skripty vyvíjet U-SQL](data-lake-analytics-data-lake-tools-get-started.md).
- Další U SQL najdete v tématu [Začínáme s jazykem Azure dat jezera analýzy U SQL](data-lake-analytics-u-sql-get-started.md)a [reference jazyka U SQL](http://go.microsoft.com/fwlink/?LinkId=691348).
- Úlohy správy najdete v článku [Správa Azure dat jezera analýzy pomocí portálu Azure](data-lake-analytics-manage-use-portal.md).
- Získejte přehled o jezera analýzy dat, najdete v článku [Přehled analýzy jezera dat Azure](data-lake-analytics-overview.md).
