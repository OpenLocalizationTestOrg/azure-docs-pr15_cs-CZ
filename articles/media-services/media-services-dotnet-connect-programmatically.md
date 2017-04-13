<properties 
    pageTitle="Připojení k účtu služby médií pomocí .NET" 
    description="Toto téma ukazuje, jak se připojit k Media Services uisng .NET." 
    services="media-services" 
    documentationCenter="" 
    authors="juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="09/26/2016"
    ms.author="juliako"/>


# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Připojení k účtu služby médií pomocí Media Services SDK pro .NET

> [AZURE.SELECTOR]
- [ZBÝVAJÍCÍ](media-services-rest-connect-programmatically.md)
- [.NET](media-services-dotnet-connect-programmatically.md)


Toto téma popisuje, jak získat programové připojení ke službě Microsoft Azure Media Services, když jsou programování s Media Services SDK pro .NET.


## <a name="connecting-to-media-services"></a>Připojit se ke službám médií

Programové připojení k Media Services musí nastavili účet Azure, nakonfigurované Media Services k tomuto účtu a pak nastavení Visual Studio projektu pro vývoj se Media Services SDK pro .NET. Další informace najdete v tématu Nastavení pro vývoj s Media Services SDK pro .NET.

Na konci procesu nastavení účtu Media Services získáte následující hodnoty požadované připojení. Pomocí těchto programové připojení Media Services.

- Název účtu Media Services.

- Svůj klíč účtu Media Services.

Tyto hodnoty najdete přejděte na Managment portál Azure, vyberte svůj účet služby média a klikněte na ikonu "**Správa KLÍČŮ**" v dolní části okna portálu. Klepnutím na ikonu vedle každé textové pole slouží ke kopírování hodnoty do schránky systému.


## <a name="creating-a-cloudmediacontext-instance"></a>Vytvoření CloudMediaContext Instance

Spuštění programování proti Media Services je potřeba vytvořit instanci **CloudMediaContext** , která představuje kontextu serveru. **CloudMediaContext** obsahuje odkazy na důležité kolekce včetně úlohy prostředky, soubory, zásady přístupu a Locator.

>[AZURE.NOTE] Třídy **CloudMediaContext** není podprocesů. Vytvoříte nové CloudMediaContext podle vlákna nebo sadu operací.


CloudMediaContext má pět konstruktor přetížení. Doporučujeme používat konstruktory, které přebírají **MediaServicesCredentials** jako parametr. Další informace najdete v tématu **Opakované použití tokeny služby Access ovládací prvek** , který následuje. 

Následující příklad využívá veřejný konstruktor CloudMediaContext(MediaServicesCredentials credentials):

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);
    
    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Opakované použití přístupových ovládací prvek Služba tokenů

Tato část popisuje znovu použít tokeny služba Řízení přístupu pomocí CloudMediaContext konstruktory, které přebírají MediaServicesCredentials jako parametr.


[Řízení přístupu Azure Active Directory](https://msdn.microsoft.com/library/hh147631.aspx) (označovaná taky jako služba Řízení přístupu nebo ACS) je cloudové služby, které poskytují snadný způsob, jak ověřování a ověřování uživatelů k získání přístupu k jejich webových aplikací. Microsoft Azure Media Services řídí přístup k jeho služeb přes OAuth Protocol (protokol), je to nutné ACS token. Media Services obdrží tokeny ACS ze serveru se tak mohli ověřovat.

Při vytváření s Media Services SDK, můžete není zacházet s tokeny, protože v SDK kódu správci je za vás. Však nechat SDK plně spravovat tokeny ACS vede k nepotřebných tokenu žádosti. Vyžádání tokeny dlouho a spotřebovává zdroje klienta a serveru. Navíc serveru ACS omezení žádosti Pokud sazbu je příliš velký. Další informace najdete v článku [Omezení služeb ACS](https://msdn.microsoft.com/library/gg185909.aspx) , limit je 30 požadavky za sekundu

Začnete s Media Services SDK verze 3.0.0.0, můžete znovu použít tokeny ACS. Sdílení tokeny ACS mezi více kontexty zapnout konstruktory **CloudMediaContext** , které přebírají **MediaServicesCredentials** jako parametr. Třídy MediaServicesCredentials zapouzdřuje pověření Media Services. Pokud je k dispozici ACS token a známé jeho platnosti, můžete vytvořit nové instance MediaServicesCredentials tokenu a předat konstruktoru CloudMediaContext. Všimněte si, že Media Services SDK automaticky obnoví tokeny pokaždé, když vyprší jejich platnost. Existují dva způsoby znovu použít ACS tokenů, jak je vidět v příkladech dál.

- Můžete mezipaměti **MediaServicesCredentials** objekt v paměti (například proměnnou statickou). Konstruktoru CloudMediaContext potom předejte režim cached objekt. Objekt MediaServicesCredentials obsahuje token ACS, který můžete znovu použít, pokud je pořád platná. Pokud token není platná, bude aktualizovat podle Media Services SDK pomocí přihlašovacích údajů předaných konstruktoru MediaServicesCredentials.

    Všimněte si, že objekt **MediaServicesCredentials** získá token platné po volání RefreshToken. **CloudMediaContext** volá metodu **RefreshToken** v konstruktoru. Pokud se chystáte uložit tokenu hodnoty do externí úložiště, zkontrolujte zkontrolujte, zda je hodnota TokenExpiration platná před uložením tokenu data. Pokud není platná, zavolejte RefreshToken před ukládání do mezipaměti.

        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        
        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }
        
        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

- Můžete taky mezipaměti řetězec AccessToken a TokenExpiration hodnoty. Hodnoty později lze vytvořit nový objekt MediaServicesCredentials s daty v mezipaměti tokenu.  Toto je užitečné především pro scénáře kde tokenu možné bezpečně sdílet mezi více procesů nebo počítačů.

    Následující fragmentů kódu zavolejte SaveTokenDataToExternalStorage GetTokenDataFromExternalStorage a UpdateTokenDataInExternalStorageIfNeeded metody, které nejsou v tomto příkladu definované. Můžete definovat těchto postupů ukládat, načíst a aktualizovat tokenu data v externí úložiště. 

        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
        
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
        
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
        
    Umožňuje vytvořit MediaServicesCredentials uložené tokenu hodnoty.


        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;
        
        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);
        
        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };
        
        CloudMediaContext context2 = new CloudMediaContext(credentials);

    V případě, že tokenu aktualizoval Media Services SDK aktualizujte tokenu kopii. 
    
        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }
        

- Pokud máte víc účtů Media Services (například zatížení sdílení účely nebo Geo distribuční) můžete mezipaměti objektů MediaServicesCredentials pomocí kolekci System.Collections.Concurrent.ConcurrentDictionary (kolekci ConcurrentDictionary představuje vláken kolekci klíč/dvojice, které můžete přistupovat pomocí více vláken souběžně). Můžete pak použijte metodu GetOrAdd k získání pověření v mezipaměti. 

        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();
        

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;
        
            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));
        
            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);
        
            return cloudMediaContext;
        }
        
## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Připojení k účtu služby Media nachází v oblasti severní Číně

Pokud váš účet je umístěn v oblasti severní Číně, použijte následující konstruktor:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Příklad:


    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>Ukládání připojení hodnot v konfiguraci

Je velmi doporučený postup vhodný k ukládání připojení hodnot, například jména a hesla, zejména citlivé hodnoty v konfiguraci. Je doporučený postup k šifrování citlivá konfigurační data. Šifrování celý konfiguračního souboru pomocí Windows soubor systému (ENCRYPTING System). Chcete-li povolit EFS na souboru, klikněte pravým tlačítkem myši na soubor, vyberte **Vlastnosti**a šifrování na kartě **Upřesnit** nastavení. Nebo můžete vytvořit vlastní řešení pro šifrování vybrané části konfiguračního souboru pomocí chráněné konfigurace. V části [šifrování konfigurace informace pomocí chráněné konfigurace](https://msdn.microsoft.com/library/53tyfkaw.aspx).

Následující konfiguračního souboru s hodnotami požadované připojení. Hodnoty v <appSettings> prvek jsou požadované hodnoty, které jste získali od procesu nastavení účtu Media Services.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


K načtení hodnot připojení z konfigurace, můžete použít třídu **ConfigurationManager** a potom přiřadit hodnoty polí v kódu:
    
    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



##<a name="media-services-learning-paths"></a>Media Services výukových postupů

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Poskytnutí zpětné vazby

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
