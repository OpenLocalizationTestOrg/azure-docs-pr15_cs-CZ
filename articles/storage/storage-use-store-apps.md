<properties
    pageTitle="Použití Azure úložiště v aplikacích pro Windows Store | Microsoft Azure"
    description="Naučte se vytvářet aplikace pro Windows Store, který používá úložiště objektů Blob Azure, fronty, tabulku nebo soubor."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>
    
# <a name="how-to-use-azure-storage-in-windows-store-apps"></a>Jak používat Azure úložiště v aplikacích pro Windows Store

## <a name="overview"></a>Základní informace

Tato příručka ukazuje, jak začít s vytvářením aplikace pro Windows Store, který využívá Azure úložiště.

## <a name="download-required-tools"></a>Stáhněte si požadované nástroje

- [Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx) usnadňuje vytváření ladění, localize, zabalit a nasazení aplikace pro Windows Store. Vyžaduje se Visual Studio 2012 nebo novější.
- [Azure úložiště klienta knihovny](https://www.nuget.org/packages/WindowsAzure.Storage) poskytuje knihovna tříd Windows Runtime pro práci s Azure úložiště.
- [WCF dat služby nástroje pro Windows Store aplikace](http://www.microsoft.com/download/details.aspx?id=30714) rozšiřuje přidat odkaz na službu s podporou OData klientské aplikace pro Windows Store ve Visual Studiu.

## <a name="develop-apps"></a>Můžete vyvíjet aplikace

### <a name="getting-ready"></a>Příprava

Vytvoření nového projektu pro Windows Store aplikace Visual Studio 2012 nebo novější:

![úložiště aplikace – úložiště sestavte projekt][store-apps-storage-vs-project]

Pravým tlačítkem myši na **odkazy**, kliknutím **Přidat odkaz**a poté přejděte na úložiště klienta knihovny pro Windows Runtime, který jste stáhli pak přidáte odkaz na knihovnu Azure úložiště klienta:

![úložiště přihlašovacích údajů aplikace – úložiště – zvolte knihovny][store-apps-storage-choose-library]

### <a name="using-the-library-with-the-blob-and-queue-services"></a>Používání knihovny služby objektů Blob a fronty

V tomto okamžiku je připravená k volání objektů Blob Azure a fronty služby aplikace. Přidejte následující příkazy **pomocí** tak, aby typy úložiště Azure můžete přímo odkazováno:

    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Auth;

Dále přidejte tlačítka na stránku. Přidejte následující kód jeho **klikněte na** událost nebo úprava metody obslužné rutiny události pomocí [asynchronní klíčového slova](http://msdn.microsoft.com/library/vstudio/hh156513.aspx):

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var blobClient = account.CreateCloudBlobClient();
    var container = blobClient.GetContainerReference("container1");
    await container.CreateIfNotExistsAsync();

Tento kód předpokládá, že máte proměnné dva řetězce, *název účtu* a *accountKey*. Představují název účtu úložiště a klíč účtu, který je přidružený k tomuto účtu.

Vytvoření a spuštění aplikace. Klikněte na tlačítko zkontrolujte, zda kontejner *container1* existuje ve vašem účtu a pak vytvořte není-li.

### <a name="using-the-library-with-the-table-service"></a>Používání knihovny se službou tabulky

Typy používané komunikovat se službou Azure tabulky, závisí na datové služby WCF knihovny aplikace pro Windows Store. Dále přidejte odkaz na požadované WCF knihovny pomocí konzole balíčku správce:

![úložiště aplikace--balíček – Správce úložiště][store-apps-storage-package-manager]

Použijte tento příkaz Správce čárky balíčků místo ve vašem počítači:

    Install-Package Microsoft.Data.OData.WindowsStore -Source "C:\Program Files (x86)\Microsoft WCF Data Services\5.0\bin\NuGet"

Tento příkaz automaticky přidá všechny požadované odkazy do projektu. Pokud nechcete používat konzolu Správce balíček, můžete přidat složku NuGet datové služby WCF na místním počítači do seznamu zdrojů balíčku a pak přidat odkaz v uživatelském rozhraní, podle popisu v [Správa NuGet balíčků pomocí dialogového okna](http://docs.nuget.org/docs/start-here/Managing-NuGet-Packages-Using-The-Dialog).

Když odkazujete balíček NuGet datové služby WCF změňte kód v případě **klikněte** na tlačítko:

    var credentials = new StorageCredentials(accountName, accountKey);
    var account = new CloudStorageAccount(credentials, true);
    var tableClient = account.CreateCloudTableClient();
    var table = tableClient.GetTableReference("table1");
    await table.CreateIfNotExistsAsync();

Tento kód zkontroluje, zda v tabulce s názvem *tabulka1* existuje ve vašem účtu a potom vytvoří není-li.

Můžete také přidat odkaz na Microsoft.WindowsAzure.Storage.Table.dll, která je dostupná ve stejném balíčku, který jste stáhli. Tato knihovna obsahuje další funkce, jako jsou založené na odraz serializace a obecné dotazy. Všimněte si, že tuto knihovnu nepodporuje JavaScript.



[store-apps-storage-vs-project]: ./media/storage-use-store-apps/store-apps-storage-vs-project.png
[store-apps-storage-choose-library]: ./media/storage-use-store-apps/store-apps-storage-choose-library.png
[store-apps-storage-package-manager]: ./media/storage-use-store-apps/store-apps-storage-package-manager.png
