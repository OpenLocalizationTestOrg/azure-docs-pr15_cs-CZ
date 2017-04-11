<properties
   pageTitle="Používání Data jezera úložiště Java SDK pro vývoj aplikací | Microsoft Azure"
   description="Používání Azure Data jezera úložiště Java SDK pro vývoj aplikací"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="get-started-with-azure-data-lake-store-using-java"></a>Začínáme s úložiště jezera dat Azure pomocí jazyka Java

> [AZURE.SELECTOR]
- [Portál](data-lake-store-get-started-portal.md)
- [Prostředí PowerShell](data-lake-store-get-started-powershell.md)
- [.NET SDK](data-lake-store-get-started-net-sdk.md)
- [Java SDK](data-lake-store-get-started-java-sdk.md)
- [ROZHRANÍ REST API](data-lake-store-get-started-rest-api.md)
- [Azure rozhraní příkazového řádku](data-lake-store-get-started-cli.md)
- [Node.js](data-lake-store-manage-use-nodejs.md)

Naučte se používat Azure Data jezera úložiště Java SDK k provádění základních operací, například při vytvoření složek, nahrávání a stahování datové soubory, atd. Další informace o jezera dat najdete v článku [Úložiště jezera dat Azure](data-lake-store-overview.md).

Přístup k rozhraní API SDK Java dokumenty, které pro Azure dat jezera úložiště přihlašovacích údajů v [Azure dat jezera úložiště Java API dokumenty](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Zjistit předpoklady pro

* Java Development Kit (JDK 7 nebo novější, pomocí jazyka Java verze 1.7 nebo vyšší)
* Úložiště jezera dat Azure účet. Postupujte podle pokynů na [Začínáme s úložiště jezera Azure dat na portálu Azure](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). Tento kurz používá Maven závislostí Tvůrce dotazů a projektu. Přestože je možné vytvářet bez použití systém sestavení jako Maven nebo Gradle, je jednodušší Správa závislostí tyto značky systémů.
* (Volitelné) A integrovaném vývojovém prostředí jako [IntelliJ MYŠLENCE](https://www.jetbrains.com/idea/download/) nebo [zatmění](https://www.eclipse.org/downloads/) apod.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Jak můžu ověřit, pomocí služby Azure Active Directory?

V tomto kurzu používáme tajná klientské aplikace Azure AD Načtěte token Azure Active Directory (-služeb ověřování). Vytvoření objektu jezera úložiště klienta provádět operace souborů a adresářů operace použijeme tento token. Pokyny k ověření s úložiště jezera dat Azure pomocí klienta tajná jsme takto nejvyšší úrovně:

1. Vytvořit webovou aplikaci Azure AD
2. Načtení ID klienta, tajná klienta a tokenu koncový bod pro webovou aplikaci Azure AD.
3. Konfigurace aplikace access pro webovou aplikaci Azure AD na úložišti jezera soubor nebo složka, kterou chcete přístup z Java aplikace, kterou vytváříte.

Pokyny k provedení těchto kroků najdete v článku [Vytvoření aplikace služby Active Directory](data-lake-store-authenticate-using-active-directory.md#create-an-active-directory-application).

Azure Active Directory poskytuje další možnosti i k načtení token. Můžete vybrat z několika různých ověřovacích mechanismy tak, aby vyhovovala nefunguje, například aplikace spuštěné v prohlížeči, aplikace distributed jako desktopové aplikace nebo aplikace server s místní nebo v Azure virtuálního počítače. Můžete také vybrat z různých typů přihlašovací údaje, jako jsou hesla, certifikáty, ověřování faktory 2 atd. Kromě toho Azure Active Directory vám umožní k synchronizaci místních uživatelů služby Active Directory s cloudu. Další informace najdete v tématu [Ověření scénáře pro službu Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md). 

## <a name="create-a-java-application"></a>Vytvoření aplikace Java

Kód vzorek k dispozici [na GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) walks vás provede procesem vytváření souborů v úložišti, zřetězení soubory, stahování souboru a odstranění některé soubory v úložišti. Tato část článek vás provede procesem hlavních částí kódu.

1. Vytvoření projektu Maven pomocí [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) z příkazového řádku nebo pomocí integrovaném vývojovém prostředí. Pokyny k vytváření Java projektu pomocí IntelliJ najdete v tématu [tady](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html). Pokyny k vytváření projektu pomocí zatmění najdete v tématu [tady](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm). 

2. Přidejte následující závislosti soubor **pom.xml** Maven. Přidejte následující úryvek text mezi ** \</version >** značky a ** \</Projekt >** značku:

        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.0.4-SNAPSHOT</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>

    První závislosti, je použít Data jezera úložiště SDK (`azure-datalake-store`) z úložiště maven. Druhá závislostí (`slf4j-nop`) je můžete určit, které framework protokolování použít k této aplikaci. Data jezera úložiště SDK používá fasády [slf4j](http://www.slf4j.org/) protokolování, které můžete vybrat z řady oblíbených protokolování rámce, jako je log4j, Java protokolování logback atd., nebo bez přihlášení. V tomto příkladu deaktivujeme protokolování, proto jsme použít **slf4j nop** vazbu. Použití dalších možností protokolování v aplikaci, najdete v článku [tady](http://www.slf4j.org/manual.html#projectDep).

### <a name="add-the-application-code"></a>Přidání kódu pro aplikace

Existují tři hlavní části kódu.

1. Získat token Azure Active Directory

2. Umožňuje vytvořit jezera úložiště klienta tokenu.

3. Umožňuje provádět operace jezera úložiště klienta.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Krok 1: Získání tokenu služby Azure Active Directory.

Data jezera úložiště SDK poskytuje pohodlný metody, které umožňují získat potřebné chcete mluvit na účtu v úložišti jezera tokenů zabezpečení. Však v SDK není pověřit použít pouze těchto postupů. Můžete použít jakýkoli jiný získání token stejně, jako je použití [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java)nebo vlastní kód.

Použití Data jezera úložiště SDK k získání token Active Directory webové aplikace jste dříve vytvořili, použijte statický metody v `AzureADAuthenticator` předmětu. Nahraďte **DOPLNĚNÍ UDĚLEJTE** skutečné hodnoty Azure Active Directory webové aplikace.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AzureADToken token = AzureADAuthenticator.getTokenUsingClientCreds(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Krok 2: Vytvoření objektu úložiště jezera dat Azure klienta (ADLStoreClient)

Vytvoření objektu [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/) vyžaduje, abyste zadejte název účtu úložiště jezera dat a vytvořeného v posledním kroku token Azure Active Directory. Všimněte si, že název účtu úložiště jezera dat musí být plně kvalifikovaný název domény. Například nahraďte **DOPLNĚNÍ TADY** nějak **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, token);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Krok 3: Použití ADLStoreClient k provádění operací, soubor a adresář

Následující kód obsahuje fragmenty příklad některých běžných operací. Můžete si může samostatně prohlížet celou [dat jezera úložiště Java SDK API dokumenty](https://azure.github.io/azure-data-lake-store-java/javadoc/) objekt **ADLStoreClient** zobrazíte dalších operací.
 
Všimněte si, že soubory jsou číst a zapisovat do pomocí standardní datové proudy Java. To znamená, že jste vrstvy některou Java datových proudů nad datových proudů úložiště jezera dat využívat standardních funkcí jazyka Java (například tisk datových proudů formátovaný výstup nebo jednotlivých datových proudů komprese nebo šifrování další funkce na horní, atd.).

    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Krok 4: Vytvoření a spuštění aplikace

1. Spustit z v rámci integrovaném vývojovém prostředí, vyhledejte a klepnutím na tlačítko **Spustit** . Pokud chcete spustit z Maven, použijte [spouštění: spouštění](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html).

2. K vytvoření samostatného sklenice, která lze spustit z příkazového řádku vytvářet sklenice se všechny závislostmi zahrnuté, pomocí modulu [Plug-in pro Maven sestavení](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). Pom.xml v [příkladu zdrojového kódu v github](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) má příklad toho, jak můžete to udělat.


## <a name="next-steps"></a>Další kroky

- [Zabezpečení dat v úložišti jezera dat](data-lake-store-secure-data.md)
- [Použití analýzy jezera Azure dat s jezera úložiště dat](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Pomocí úložiště jezera dat Azure HDInsight](data-lake-store-hdinsight-hadoop-use-portal.md)
