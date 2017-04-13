<properties 
    pageTitle="Konfigurace připojovací řetězec k základnímu úložišti Azure | Microsoft Azure"
    description="Konfigurace připojovací řetězec pro účet Azure úložiště. Připojovací řetězec obsahuje informace potřebné k ověření přístup k účtu úložiště z aplikace za běhu."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Konfigurace Azure úložiště připojovací řetězec

## <a name="overview"></a>Základní informace

Připojovací řetězec obsahuje ověřovací informace potřebné k přístupu k datům v Azure úložiště účtu z aplikace za běhu. Můžete nakonfigurovat připojovací řetězec pro:

- Připojení k emulátoru Azure úložiště.
- Přístup k účtu úložiště v Azure.
- Přístup k zadané zdroje v Azure prostřednictvím sdílený přístup k podpisu (přidružení zabezpečení).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Ukládání připojovacího řetězce

Aplikace se potřebují dostat k připojovací řetězec za běhu pro ověření žádosti k základnímu úložišti Azure. Máte několik různých možností pro ukládání připojovací řetězec:

- Pro spuštění aplikace, na ploše nebo na zařízení, mohou být uloženy připojovací řetězec do `app.config `soubor nebo `web.config` soubor. Do oblasti **AppSettings** přidáte připojovací řetězec.
- Aplikace spuštěné v Azure cloudové služby můžete v [souboru schématu (.cscfg) konfigurace služby Azure](https://msdn.microsoft.com/library/ee758710.aspx)uložit připojovací řetězec. Přidejte připojovací řetězec do oddílu **ConfigurationSettings** konfiguračního souboru služby.
- Můžete taky připojovací řetězec přímo v kódu. Většině případů ale doporučujeme uložit konfigurační řetězec konfiguračního souboru.

Ukládání připojovací řetězec v souboru konfigurace snadno aktualizovat připojovací řetězec můžete přepínat mezi emulátoru úložiště a účet Azure úložiště v cloudu. Potřebujete upravit připojovací řetězec tak, aby ukazovaly na cílové prostředí.

Můžete třída [Microsoft Azure Správce konfigurace](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) pro přístup k připojovací řetězec za běhu bez ohledu na to, kde je aplikace spuštěna.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Vytvoření připojovacího řetězce na emulátoru úložiště

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Další informace o emulátoru úložiště naleznete v tématu [použití emulátor Azure úložiště pro vývoj a testování](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Vytvoření připojovacího řetězce s klientem Azure úložiště

Pokud chcete vytvořit připojovací řetězec pro váš účet Azure úložiště, použijte formát připojovacího řetězce dole. Určit, jestli se chcete připojit k účtu úložiště až HTTPS (doporučeno) nebo HTTP, nahraďte `myAccountName` s název účtu úložiště a nahradit `myAccountKey` s svůj klíč účtu aplikace access:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Například připojovací řetězec bude vypadat podobně jako následující ukázkové připojovací řetězec:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure úložiště podporuje HTTP a HTTPS v připojovacím řetězci; však pomocí HTTPS důrazně doporučujeme.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Vytvoření připojovacího řetězce podpisem sdílený přístup

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Vytvoření připojovacího řetězce koncový bod explicitní úložiště

Výslovně zadaný koncové body služby v připojovacím řetězci místo použití výchozí koncové body. Pokud chcete vytvořit připojovací řetězec, který určuje explicitní koncový bod, zadejte koncový bod dokončení služby pro každou službu, včetně specifikaci protocol (protokol HTTPS v (doporučeno) nebo HTTP), v tomto formátu:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Jeden scénář místo, kam chcete zadat explicitní koncový bod je, pokud jste přiřadili koncový bod úložiště objektů Blob vlastní doménu. V takovém případě můžete zadat vlastní koncový bod pro úložiště objektů Blob v připojovacím řetězci a volitelně určete výchozí koncové body jiné služby, pokud je aplikace používá.

Tady jsou příklady platný připojovací řetězec určující explicitní koncový bod pro službu objektů Blob:

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Koncový bod hodnotu, která je uvedená v připojovacím řetězci slouží k vytvoření žádosti o URI ke službě objektů Blob a určuje formuláře žádné URI, které vracejí do kódu.

Všimněte si, že pokud se rozhodnete sdělit vynechat koncový bod služby z připojovací řetězec, pak nebudete moct používat tuto připojovací řetězec pro přístup k datům v této službě v kódu.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Vytvoření připojovacího řetězce s příponou koncový bod

Pokud chcete vytvořit připojovací řetězec pro služba úložiště v oblasti nebo instancí pomocí různých koncový bod přípony, jako je třeba Čína Azure nebo zásady správného řízení Azure, použijte následující formát připojovacího řetězce. Označuje, zda chcete připojení k účtu úložiště prostřednictvím protokolu HTTP nebo HTTPS, nahraďte `myAccountName` název účtu úložiště nahradit `myAccountKey` s přístup klíč účtu a nahradit `mySuffix` s příponou URI:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Například připojovací řetězec by měla vypadat podobně jako následující připojovací řetězec:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Analýza připojovacího řetězce

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Další kroky

- [Použití emulátoru Azure úložiště pro vývoj a testování](storage-use-emulator.md)
- [Průzkumník Azure úložiště](storage-explorers.md)
- [Používání podpisů sdílený přístup (přidružení zabezpečení)](storage-dotnet-shared-access-signature-part-1.md)
