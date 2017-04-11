<properties
    pageTitle="Aplikace služby rozhraní API aplikace – co se změnilo | Microsoft Azure"
    description="Zjistěte, co je nového pro rozhraní API aplikace v aplikaci služby Azure."
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>Aplikace služby rozhraní API aplikace - provedené změny

U události Connect() v listopadu 2015 číslo vylepšení v aplikaci služby Azure byly [ohlásí](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/). Vylepšení patří podkladového změny rozhraní API aplikace lépe zarovnáte Mobile, přičemž Web Apps, snížit počet koncept a zlepšit nasazení a modulu runtime výkon. Zahájení nového rozhraní API aplikace 30 listopad 2015, který vytvoříte pomocí portálu pro správu Azure nebo nejnovější nástrojů budou obsahovat tyto změny. Tento článek popisuje tyto změny, a taky jak přeinstalujte existující aplikace umožní využít výhod možností ovládání.

## <a name="feature-changes"></a>Funkce změny
Klíčové funkce rozhraní API aplikace – ověřování, CORS a rozhraní API metadata – byly přesunuté přímo do aplikace služby. Díky této změně funkce jsou dostupné na webový server, mobilní telefon a rozhraní API aplikace. Ve skutečnosti všechny tři sdílení stejného typu zdroje **Microsoft.Web/sites** správce prostředků. Brána rozhraní API aplikace je už není potřeba nebo nabízených pomocí rozhraní API aplikace. Také díky tomu snadněji používaly Azure rozhraní API správy od budou pouze jeden rozhraní API bránu pro správu.

![Přehled rozhraní API aplikace](./media/app-service-api-whats-changed/api-apps-overview.png)

Pravidlo hlavních aktualizacích rozhraní API aplikace je můžete přenést rozhraní API je v jazyce podle výběru.  Pokud vaše rozhraní API je nasadili jste jako v prohlížeči nebo mobilní aplikaci, nemáte přeinstalujte aplikace umožní využít výhod nových funkcí. Pokud je aktuálně k rozhraní API aplikace náhled, migrace pokyny jsou uvedeny níže.

### <a name="authentication"></a>Ověřování
Existující klíč rozhraní API aplikace, mobilní služby/aplikací Web Apps ověřování funkce a mít jednotné a jsou k dispozici v jedné zásuvné ověřování Azure aplikaci služby v portálu pro správu. Úvod do služby ověřování v aplikaci služby, najdete v článku [rozbalování aplikaci služby ověřování / se tak mohli ověřovat](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/).

Rozhraní API scénářích existuje celá řada relevantní nových možností:

- **Podpora služby Azure Active Directory přímo**bez kód klienta museli exchange token AAD pro token relace: klient zahrnout pouze tokeny AAD v záhlaví se tak mohli ověřovat podle nosný token specifikace. Také znamená to, že žádný SDK specifické pro aplikaci služby je potřeba na straně klienta nebo na serveru. 
- **Služba služby nebo access "Interní"**: Pokud máte démon obrázku nebo jiného klienta nutností přístup k rozhraní API bez nějakého rozhraní, můžete požádat o token pomocí jistinu služby AAD a předávání aplikaci služby pro ověřování s aplikací.
- **Povolení odloženo**: mnohé aplikace mají různé omezení přístupu pro různé části aplikace. Můžete chtít některé rozhraní API pro veřejně přístupná, zatímco jiné vyžadují přihlášení. Funkce původní ověřování a povolení byl vše nebo nic, se celý web vyžadujícího přihlášení. Tuto možnost stále existuje, ale můžete taky můžete povolit kód aplikace se vykreslují pomocí aplikace access rozhodnutí po aplikaci služby má ověření uživatele.
 
Další informace o nových funkcích ověřování najdete v článku [a tak mohli ověřovat pro rozhraní API aplikace v aplikaci služby Azure](app-service-api-authentication.md). Informace o tom, jak migrovat stávající rozhraní API aplikací z předchozích modelu rozhraní API aplikace pro novou skupinu, najdete v článku [migrace stávajícího rozhraní API aplikace](#migrating-existing-api-apps) dál v tomto článku.
 
### <a name="cors"></a>CORS
Místo oddělený středníkem **MS_CrossDomainOrigins** aplikace nastavení, je teď zásuvné v portálu pro správu Azure pro konfiguraci CORS. Můžete taky to je možné konfigurovat pomocí nástroje ATP Azure Powershellu, rozhraní příkazového řádku [Průzkumníka zdroje](https://resources.azure.com/)správce prostředků. Nastavte vlastnost **cors** na typ zdroje **Microsoft.Web/sites/config** pro vaše ** &lt;název webu&gt;/web** zdroje. Příklad:

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>Rozhraní API metadat
Definice zásuvné rozhraní API je k dispozici na webový server, mobilní telefon a rozhraní API aplikace. Na portálu Správa můžete určit relativní adresu url nebo absolutní adresa url přejdete na koncový bod této hosts Swagger 2.0 znázornění vaše rozhraní API. Můžete taky je možné konfigurovat pomocí nástrojů Správce prostředků. Nastavte vlastnost **apiDefinition** na typ zdroje **Microsoft.Web/sites/config** pro vaše ** &lt;název webu&gt;/web** zdroje. Příklad:

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

V současné době koncový bod metadat je třeba veřejně přístupný bez ověření mnoho pro klienty pro příjem (například Visual Studio rozhraní REST API klienta generování a PowerApps "Přidat rozhraní API" toku) ho zpracovat. To znamená, že pokud používáte aplikaci služby ověřování a chcete vystavit definici rozhraní API z v rámci samotnou aplikaci, musíte použít možnost odložit ověřování popsaným tak, aby směrování metadatům Swagger veřejné.

## <a name="management-portal"></a>Správa portálu
Výběr **Nový > Web + mobilní > rozhraní API aplikace** na portálu vytvoří rozhraní API aplikace odrážející nové možnosti popisované v následujícím článku. **Procházet > rozhraní API aplikace** zobrazí pouze tyto nové rozhraní API aplikace. Jakmile přejděte do aplikace pro rozhraní API zásuvné sdílí stejné rozložení a možností jako Web a mobilní aplikace. Pouze rozdíl se obsah rychlý úvod a řazení nastavení.

Existující rozhraní API aplikace (nebo Marketplace rozhraní API aplikace vytvořená z logických aplikace) s náhledem předchozí možnosti budou nadále zobrazovat v Návrháři logiky aplikace a při procházení všech zdrojů v skupina zdroje.

## <a name="visual-studio"></a>Visual Studio

Většina webových aplikací Web Apps nástroje pracovat s novou rozhraní API aplikace od sdílení stejného typu podkladového zdroje **Microsoft.Web/sites** . Visual Studio Azure nástroje, ale musí být upgradované verzi 2.8.1 nebo vyšší od poskytuje několik možnosti specifické pro rozhraní API. Stáhněte si SDK z [Azure stahování stránky](https://azure.microsoft.com/downloads/).

S racionalizace typů aplikace služeb, publikovat je také jednotný v části **Publikovat > Microsoft Azure aplikaci služby**:

![Rozhraní API aplikace publikování](./media/app-service-api-whats-changed/api-apps-publish.png)

Další informace o SDK 2.8.1 najdete v tématu oznámení [do blogu](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/).

Můžete taky můžete ručně importujete profilu publikování na portálu Správa povolit publikovat. Však Explorer cloudu, generování kódu a rozhraní API aplikace výběru/vytvoření vyžaduje SDK 2.8.1 nebo vyšší.

## <a name="migrating-existing-api-apps"></a>Migrace stávajícího rozhraní API aplikace
Vlastní rozhraní API po nasazení předchozí verze Preview rozhraní API aplikace, jsme požádat o přenést do nového modelu pro rozhraní API aplikace do 31 prosince, roku 2015. Vzhledem k tomu, jak starý i nový model jsou založeny na rozhraní API Web hostovaný v aplikaci služby, většinou existující kód můžete znovu použít.

### <a name="hosting-and-redeployment"></a>Hostování a nové nasazení
Postup pro opětovné nasazení jsou stejné jako nasazení všechny existující rozhraní API webových aplikací služby. Pomocí následujících kroků:

1. Vytvoření prázdného rozhraní API aplikace. To se teď dá na portálu s nový > rozhraní API aplikace ve Visual Studiu z publikovat nebo z nástrojů Správce prostředků. Použití nástrojů Správce prostředků nebo šablony, nastavte hodnotu **identifikovat** **rozhraní API** na typ zdroje **Microsoft.Web/sites** mít pomůcky pro začátečníky a nastavení v portálu pro správu orientovat na rozhraní API scénáře.
2. Připojení a nasazení projektu do aplikace prázdné rozhraní API použití libovolného příkazu pro nasazení mechanismy nepodporuje aplikaci služby. Přečtěte si [přečtěte následující dokumentaci pro aplikaci služby Azure nasazení](../app-service-web/web-sites-deploy.md) Další informace. 
  
### <a name="authentication"></a>Ověřování
Ověřování aplikace služby podpory stejné možnosti, které jsou dostupné pro předchozí modelu rozhraní API aplikace. Pokud používáte tokeny relace a vyžadovat SDK, použijte následující SDK klienta a serveru:

- Klient: [Azure mobilních klientů SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- Server: [Microsoft Azure mobilní aplikace .NET ověřování rozšíření](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

Pokud jste použili místo toho alfa aplikaci služby SDK, změněny jsou teď tyto:

- Klient: [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- Server: [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

Zejména se službou Azure Active Directory, ale bez aplikace specifických pro službu požaduje Pokud používáte AAD token přímo.

### <a name="internal-access"></a>Vnitřní přístup
Předchozí modelu rozhraní API aplikace zahrnuty úroveň předdefinované interní přístupu. Použití sady SDK to nutné pro podepisování žádosti o. Jak bylo popsáno dříve, s nový model aplikací rozhraní API AAD služby objekty lze použít jako alternativní služba Služba ověřování bez nutnosti aplikace specifických pro službu SDK. Přečtěte si další v [služby základní ověřování pro rozhraní API aplikace v aplikaci služby Azure](app-service-api-dotnet-service-principal-auth.md).

### <a name="discovery"></a>Zjišťování
Předchozí modelu rozhraní API aplikace dosáhl rozhraní API pro hledání dalších aplikací rozhraní API za běhu ve stejné skupině zdroje za stejné bráně. To je užitečné v architektury implementující microservice vzorků. Když to není podporována přímo, jsou k dispozici několik možností:

1. Použití rozhraní API Správce prostředků Azure pro zjišťování.
2. Umístěte Azure rozhraní API správy před vaší rozhraní API aplikace služby hostované. Azure správy rozhraní API slouží jako fasádou a zadejte stabilní externí vystaveného adresu url i v případě, že je vnitřní topologie změní.
3. Vytvářet vlastní zjišťování rozhraní API aplikace a jiných rozhraní API aplikace zaregistrovat v aplikaci zjišťování při spuštění.
4. Při nasazení naplní nastavení aplikace všech rozhraní API aplikace (a klienti) koncové body další rozhraní API aplikace. Toto je životaschopné nasazení šablony a od rozhraní API aplikace teď umožňují ovládací prvek adresy URL.

## <a name="using-api-apps-with-logic-apps"></a>Použití rozhraní API aplikace s aplikacemi jiných logiky

Nový model aplikací rozhraní API funguje dobře s [Aplikacemi jiných logiky verze schématu 2015. 08 01](../app-service-logic/app-service-logic-schema-2015-08-01.md).

## <a name="next-steps"></a>Další kroky

Další informace najdete v tématu články v [dokumentaci k rozhraní API aplikace oddíl](https://azure.microsoft.com/documentation/services/app-service/api/). Byly aktualizovány tak, aby odrážely nový model pro rozhraní API aplikace. Kromě toho posílání zpráv na fóra pro další informace a pokyny pro migraci:

- [Fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [Přetečení zásobníku](http://stackoverflow.com/questions/tagged/azure-api-apps)
