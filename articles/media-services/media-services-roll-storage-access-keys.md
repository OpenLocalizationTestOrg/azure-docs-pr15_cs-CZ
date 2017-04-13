<properties 
    pageTitle="Aktualizace Media Services po pohybu přístupových kláves z verze úložiště | Microsoft Azure" 
    description="Tento článek vám pokyny k aktualizaci Media Services po postupných přístupových kláves z verze úložiště." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako"
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016" 
    ms.author="milangada;cenkdin;juliako"/>

#<a name="update-media-services-after-rolling-storage-access-keys"></a>Aktualizace Media Services po postupných úložiště přístupových kláves

Když vytvoříte nový účet Azure Media Services, zobrazí se výzva, vyberte účet Azure úložiště, který se používá k ukládání obsahu média. Všimněte si, že můžete [přidat víc účtů úložiště](meda-services-managing-multiple-storage-accounts.md) ke svému účtu Media Services.

Po vytvoření nového účtu úložiště Azure vygeneruje dva 512-bit úložiště přístupové klávesy, které slouží k ověření přístup ke svému účtu úložiště. Jak připojení k úložišti lépe zabezpečit, doporučuje se pravidelně obnovit a jeho otočení přístupová klávesa úložiště. Dva přístupové klávesy (primárních a sekundárních) slouží k zachování připojení k účtu úložiště pomocí jednoho přístupové klávesy, zatímco obnovit přístupová klávesa povolit. Tento postup se nazývá také "postupné přístupových kláves z verze".

Media Services závisí na úložiště klíč poskytnuté. Konkrétně Locator, které se používají můžete vysílat datovými proudy nebo stahování svém majetku závisí na přístupová klávesa zadaný úložiště. Po vytvoření účtu AMS převezme závislost přístupová klávesa primární úložiště ve výchozím nastavení ale jako uživatel, můžete aktualizovat klíč úložiště, který má AMS. Je třeba jestli chcete, aby Media Services vědět, které klíč, který používá podle kroků popsaných v tomto tématu. Také při pohybu úložiště přístupové klávesy, budete muset zkontrolujte, že aktualizace vaší Locator tak nebudou nijak narušené v datových proudů službě (Tento krok je také popsaná v tématu).

>[AZURE.NOTE]Pokud máte víc účtů úložiště, by provést tento postup se každý účet úložiště.
>
>Před provedením kroků popsaných v tomto tématu na účet výrobní, zkontrolujte, že test na účet před výroby.


## <a name="step-1-regenerate-secondary-storage-access-key"></a>Krok 1: Obnovit sekundárním úložiště přístupová klávesa

Začněte s obnovení klíč sekundárním úložiště. Ve výchozím nastavení sekundárního klíče nepoužívá Media Services.  Informace o tom, jak vrátit klíče úložiště najdete v tématu [jak: zobrazení, kopírovat a regenerovat úložiště přístupové klávesy](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).
  
##<a id="step2"></a>Krok 2: Aktualizace Media Services se pomocí klávesy nové sekundárním úložiště

Aktualizujte Media Services používat přístupová klávesa sekundárním úložiště. Můžete jednu z následujících dvou způsobů synchronizace klávesu regenerované úložiště s Media Services.

- Používat portál Azure: vyhledejte název a klíč hodnot, přejděte na portál Azure a vyberte svůj účet. Nastavení okno na pravé straně. V okně Nastavení vyberte klíče. Podle toho, který chcete použít pro Media Services k synchronizaci s klíč úložiště vyberte synchronizovat primárního klíče nebo vedlejší klíčové tlačítko synchronizovat. V tomto případě pomocí klávesy sekundární.

- Použití REST API pro správu Media Services.

Následující příklad ukazuje, jak vytvářet https://endpoint/**subscriptionId/služby/mediaservices/účty nebo*název účtu*/StorageAccounts/*storageAccountName*/Key žádost za účelem synchronizace klávesu zadaný úložiště služby média. V tomto případě bude použita hodnota klíčové sekundárním úložiště. Další informace najdete v tématu [jak: rozhraní REST API použití médií služby správy](http://msdn.microsoft.com/library/azure/dn167656.aspx).
    
    public void UpdateMediaServicesWithStorageAccountKey(string mediaServicesAccount, string storageAccountName, string storageAccountKey)
    {
        var clientCert = GetCertificate(CertThumbprint);
        
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(string.Format("{0}/{1}/services/mediaservices/Accounts/{2}/StorageAccounts/{3}/Key",
        Endpoint, SubscriptionId, mediaServicesAccount, storageAccountName));
        request.Method = "PUT";
        request.ContentType = "application/json; charset=utf-8";
        request.Headers.Add("x-ms-version", "2011-10-01");
        request.Headers.Add("Accept-Encoding: gzip, deflate");
        request.ClientCertificates.Add(clientCert);
        
        
        using (var streamWriter = new StreamWriter(request.GetRequestStream()))
        {
            streamWriter.Write("\"");
            streamWriter.Write(storageAccountKey);
            streamWriter.Write("\"");
            streamWriter.Flush();
        }
        
        using (var response = (HttpWebResponse)request.GetResponse())
        {
            string jsonResponse;
            Stream receiveStream = response.GetResponseStream();
            Encoding encode = Encoding.GetEncoding("utf-8");
            if (receiveStream != null)
            {
                var readStream = new StreamReader(receiveStream, encode);
                jsonResponse = readStream.ReadToEnd();
            }
        }
    }

Po dokončení tohoto kroku aktualizujte existující Locator (jejichž závislost na původní klíč úložiště), jak je vidět v následujícím kroku.

>[AZURE.NOTE]Až 30 minut před provedením všechny operace s Media Services (například vytváření nových Locator), aby se žádný vliv na čekající úlohy.

##<a name="step-3-update-locators"></a>Krok 3: Aktualizace Locator

>[AZURE.NOTE]Když postupných úložiště přístupové klávesy, budete muset zkontrolujte, že aktualizace stávající Locator, není k dispozici žádná přerušení služby datových proudů.

Počkejte nejméně 30 minut po synchronizaci nový klíč úložiště s AMS. Pak můžete znovu vytvořit OnDemand Locator tak využijte závislost na klávesu zadaný úložiště a udržovat existující adresu URL.

Všimněte si, že když aktualizujete (nebo obnovil) locator přidružení zabezpečení, adresa URL bude kdykoli změnit.

>[AZURE.NOTE] Abyste měli jistotu, že zachovat stávající adresy URL OnDemand Locator, budete muset odstranit existující locator a vytvořte novou se stejným ID.

.NET příklad ukazuje, jak obnovil locator se stejným ID.

soukromé statické ILocator RecreateLocator(CloudMediaContext context, ILocator locator) {/ / vlastnosti existující locator uložit.
var materiálů = locator. Materiály; var accessPolicy = locator. AccessPolicy; var locatorId = locator. ID; var startDate = locator. Čas spuštění; var locatorType = locator. Typ; var locatorName = locator. Název.

Odstraňte staré locator.
URL. Delete();

Pokud (locator. ExpirationDateTime < = DateTime.UtcNow) {nové výjimku (String.Format ("nelze znovu vytvořit locator Id = {0} protože jeho dobu platnosti locator se z minulých chyb", locator. ID.)) }
    
        // Create new locator using saved properties.
        var newLocator = context.Locators.CreateLocator(
            locatorId,
            locatorType,
            asset,
            accessPolicy,
            startDate,
            locatorName);
    
    
    
        return newLocator;
    }


##<a name="step-5-regenerate--primary-storage-access-key"></a>Krok 5: Obnovit primární přístupová klávesa

Obnovit přístupová klávesa primární úložiště. Informace o tom, jak vrátit klíče úložiště najdete v tématu [jak: zobrazení, kopírovat a regenerovat úložiště přístupové klávesy](../storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys).

##<a name="step-6-update-media-services-to-use-the-new-primary-storage-key"></a>Krok 6: Aktualizace Media Services použít nový primární klíč
    
Použijte stejný postup podle pokynů v [kroku 2](media-services-roll-storage-access-keys.md#step2) pouze v tomto případě synchronizovat novou přístupová klávesa primární úložiště s účtem Media Services.

>[AZURE.NOTE]Až 30 minut před provedením všechny operace s Media Services (například vytváření nových Locator), aby se žádný vliv na čekající úlohy.

##<a name="step-7-update-locators"></a>Krok 7: Aktualizace Locator  

Po 30 minutách můžete obnovit OnDemand Locator tak využijte závislost na nový primární klíč a udržovat existující adresu URL.

Použijte stejný postup podle pokynů v [kroku 3](media-services-roll-storage-access-keys.md#step-3-update-locators).


##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



###<a name="acknowledgments"></a>Potvrzení 

Rádi potvrdit sledování lidí dobré, kdo přispěla k vytvoření tohoto dokumentu: Cenk Dingiloglu Milán Gada Seva Titov.
