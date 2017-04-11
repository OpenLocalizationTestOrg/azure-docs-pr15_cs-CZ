
<properties 
    pageTitle="Úprava definice generované Swashbuckle rozhraní API" 
    description="Zjistěte, jak přizpůsobit Swagger rozhraní API definice generovaná Swashbuckle aplikace rozhraní API aplikace služby Azure." 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="bradygaster" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="dotnet" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/29/2016" 
    ms.author="rachelap"/>

# <a name="customize-swashbuckle-generated-api-definitions"></a>Úprava definice generované Swashbuckle rozhraní API 

## <a name="overview"></a>Základní informace

Tento článek vysvětluje, jak přizpůsobit Swashbuckle zpracovávání obvyklé scénáře, které můžete chtít změnit výchozí chování:

* Swashbuckle vygeneruje duplicitní operace identifikátory přetížení metod řadiče domény
* Swashbuckle předpokládá, že je platný pouze odpovědi z metody 200 HTTP (Ano). 
 
## <a name="customize-operation-identifier-generation"></a>Přizpůsobení operace identifikátor generování

Swashbuckle generuje Swagger operace identifikátory zřetězením název řadiče a pod názvem metody. Tento způsob vytvoří problém, pokud máte víc přetížení metodu: Swashbuckle vygeneruje duplicitní operace ID, které je neplatný Swagger JSON.

Například následující kód řadiče způsobí Swashbuckle ke generování tři ID Contact_Get operace.

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsincode.png)

![](./media/app-service-api-dotnet-swashbuckle-customize/multiplegetsinjson.png)

Problém můžete vyřešit ručně tím, že metody jedinečné názvy, například následující v tomto příkladu:

* Získání
* GetById
* GetPage

Alternativní je rozšířit Swashbuckle snažíme usnadnit jeho automaticky vytvořit jedinečný operace ID.

Podle těchto kroků ukazují, jak přizpůsobit Swashbuckle pomocí *SwaggerConfig.cs* soubor, který je součástí projektu šablonou projektu Visual Studio rozhraní API aplikace náhled.  Můžete také přizpůsobit Swashbuckle v rozhraní API webových projektu, který nastavení pro nasazení jako rozhraní API aplikace.

1. Vytvořit vlastní `IOperationFilter` implementaci 

    `IOperationFilter` Rozhraní představuje bod rozšiřitelnost pro Swashbuckle uživatelé, kteří chtějí přizpůsobit různé aspekty procesu Swagger metadata. Následující kód ukazuje jednu metodu změny chování generování id operací. Názvy parametrů kód připojí k název id operace.  

        using Swashbuckle.Swagger;
        using System.Web.Http.Description;
        
        namespace ContactsList
        {
            public class MultipleOperationsWithSameVerbFilter : IOperationFilter
            {
                public void Apply(
                    Operation operation,
                    SchemaRegistry schemaRegistry,
                    ApiDescription apiDescription)
                {
                    if (operation.parameters != null)
                    {
                        operation.operationId += "By";
                        foreach (var parm in operation.parameters)
                        {
                            operation.operationId += string.Format("{0}",parm.name);
                        }
                    }
                }
            }
        }

2. V souboru *App_Start\SwaggerConfig.cs* , zavolejte `OperationFilter` metoda způsobilo Swashbuckle s novým `IOperationFilter` implementaci.

        c.OperationFilter<MultipleOperationsWithSameVerbFilter>();

    ![](./media/app-service-api-dotnet-swashbuckle-customize/usefilter.png)

    Soubor *SwaggerConfig.cs* umístěná balíčkem Swashbuckle NuGet obsahuje mnoho přidán komentář mimo příklady body rozšíření. Chcete přidat víc komentářů nejsou vidět tady. 

    Po provedení této změny vaší `IOperationFilter` implementaci se používá a způsobí, že operace jedinečné ID generovat.
 
    ![](./media/app-service-api-dotnet-swashbuckle-customize/uniqueids.png)

<a id="multiple-response-codes" name="multiple-response-codes"></a>
    
## <a name="allow-response-codes-other-than-200"></a>Povolit odpověď kódy než 200

Ve výchozím nastavení Swashbuckle předpokládá, že odpověď 200 HTTP (Ano) *pouze* legitimní odpovědi z rozhraní API webových metody. V některých případech může chcete vrátit jiné kódy odpověď snadnějšímu klientovi na vysokoškolskou úroveň výjimku.  Například následující kód rozhraní API webových ukazuje scénáři, ve kterém je vhodné klienta přijmout 200 nebo 404 jako platná odpovědi.

    [ResponseType(typeof(Contact))]
    public HttpResponseMessage Get(int id)
    {
        var contacts = GetContacts();

        var requestedContact = contacts.FirstOrDefault(x => x.Id == id);

        if (requestedContact == null)
        {
            return Request.CreateResponse(HttpStatusCode.NotFound);
        }
        else
        {
            return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
        }
    }

V tomto scénáři Swagger generovaný Swashbuckle ve výchozím nastavení určuje jedinou legitimní HTTP stavový kód HTTP 200.

![](./media/app-service-api-dotnet-swashbuckle-customize/http-200-output-only.png)

Protože aplikace Visual Studio používá definici Swagger rozhraní API generovat kód pro klienta, vytvoří klientský kód, který vyvolá výjimku pro odpovědi než HTTP 200. Následující kód je z klienta C# vytvořený pro tento způsob rozhraní API webových vzorku.

    if (statusCode != HttpStatusCode.OK)
    {
        HttpOperationException<object> ex = new HttpOperationException<object>();
        ex.Request = httpRequest;
        ex.Response = httpResponse;
        ex.Body = null;
        if (shouldTrace)
        {
            ServiceClientTracing.Error(invocationId, ex);
        }
        throw ex;
    } 

Swashbuckle dvěma způsoby přizpůsobení seznam kódů očekávané HTTP odpověď generovaných ho, pomocí komentářů XML nebo `SwaggerResponse` atribut. Atribut je jednodušší, ale jenom je dostupná v Swashbuckle 5.1.5 nebo novější. Šablona Nový projekt náhled rozhraní API aplikace ve Visual Studiu 2013 obsahuje Swashbuckle verze 5.0.0, tak, aby je-li použít šablonu a nechcete, aby aktualizovat Swashbuckle vaše jediná možnost použití XML komentáře. 

### <a name="customize-expected-response-codes-using-xml-comments"></a>Přizpůsobení kódů očekávané odpovědí pomocí komentářů XML

Tímto způsobem můžete určit kódů odpovědí, pokud vaše Swashbuckle verze dřívější než 5.1.5.

1. Nejdřív přidejte nad metod, které chcete zadat HTTP odpověď kódy pro XML si přečtěte následující dokumentaci komentáře. Odběr vzorku rozhraní API webových akce nahoře a použití XML dokumentace k němu by vedla v kódu jako v následujícím příkladu. 

        /// <summary>
        /// Returns the specified contact.
        /// </summary>
        /// <param name="id">The ID of the contact.</param>
        /// <returns>A contact record with an HTTP 200, or null with an HTTP 404.</returns>
        /// <response code="200">OK</response>
        /// <response code="404">Not Found</response>
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();
        
            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
        
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

1. Přidání pokynů do souboru *SwaggerConfig.cs* směrování Swashbuckle nevyužívá XML si přečtěte následující dokumentaci soubor.

    * Otevřete *SwaggerConfig.cs* a vytvořte metodu ve třídě *SwaggerConfig* zadejte cestu k souboru XML si přečtěte následující dokumentaci. 

            private static string GetXmlCommentsPath()
            {
                return string.Format(@"{0}\XmlComments.xml", 
                    System.AppDomain.CurrentDomain.BaseDirectory);
            }

    * Přejděte dolů do souboru *SwaggerConfig.cs* se zobrazí řádek přidán komentář mimo kód podobný následující kopie obrazovky. 

        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-commented-out.png)
    
    * Zrušte komentář řádku umožňující zpracování během Swagger generování komentáře XML. 
    
        ![](./media/app-service-api-dotnet-swashbuckle-customize/xml-comments-uncommented.png)
    
1. Za účelem vytvoření souboru XML si přečtěte následující dokumentaci, přejděte do vlastnosti projektu a povolení souboru XML si přečtěte následující dokumentaci, jak je znázorněno níže snímek. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/enable-xml-documentation-file.png) 

Po provedení těchto kroků Swagger JSON generovaných Swashbuckle budou obsahovat kódy odpověď HTTP, které jste zadali v komentářích XML. Následující obrazovka ukazuje toto nové datové JSON. 

![](./media/app-service-api-dotnet-swashbuckle-customize/swagger-multiple-responses.png)

Při použití Visual Studio k opětovnému vytvoření kód klienta pro rozhraní REST API kód C# přijme HTTP OK i nebyl nalezen stavů bez vyvolá výjimku umožňující náročný kódu rozhodovat o tom, jak zpracovat vrácená null záznamu kontaktu. 

        if (statusCode != HttpStatusCode.OK && statusCode != HttpStatusCode.NotFound)
        {
            HttpOperationException<object> ex = new HttpOperationException<object>();
            ex.Request = httpRequest;
            ex.Response = httpResponse;
            ex.Body = null;
            if (shouldTrace)
            {
                ServiceClientTracing.Error(invocationId, ex);
            }
                throw ex;
        }

Kód Tato ukázka najdete v [tomto GitHub úložišti](https://github.com/Azure-Samples/app-service-api-dotnet-swashbuckle-swaggerresponse). Spolu s rozhraní API webových projekt označené poznámky si přečtěte následující dokumentaci XML je aplikace konzoly projekt, který obsahuje generované klienta pro toto rozhraní API. 

### <a name="customize-expected-response-codes-using-the-swaggerresponse-attribute"></a>Přizpůsobení kódů očekávané odpovědí pomocí atributu SwaggerResponse

Atribut [SwaggerResponse](https://github.com/domaindrivendev/Swashbuckle/blob/master/Swashbuckle.Core/Swagger/Annotations/SwaggerResponseAttribute.cs) je k dispozici v Swashbuckle 5.1.5 nebo novějším. V případě, že máte starší verze projektu, tento oddíl se spustí vysvětlíme, jak aktualizovat balíček Swashbuckle NuGet tak, aby bylo možné použít tento atribut.

1. V **Okně Průzkumník**projektu rozhraní API webových pravým tlačítkem klikněte na **Spravovat balíčků NuGet**. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/manage-nuget-packages.png)

1. Klikněte na tlačítko *Aktualizovat* u balíček NuGet *Swashbuckle* . 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/update-nuget-dialog.png)

1. Přidáte atributy *SwaggerResponse* metody rozhraní API webových akce, pro které chcete zadat platné kódy odpověď HTTP. 

        [SwaggerResponse(HttpStatusCode.OK)]
        [SwaggerResponse(HttpStatusCode.NotFound)]
        [ResponseType(typeof(Contact))]
        public HttpResponseMessage Get(int id)
        {
            var contacts = GetContacts();

            var requestedContact = contacts.FirstOrDefault(x => x.Id == id);
            if (requestedContact == null)
            {
                return Request.CreateResponse(HttpStatusCode.NotFound);
            }
            else
            {
                return Request.CreateResponse<Contact>(HttpStatusCode.OK, requestedContact);
            }
        }

2. Přidat `using` údajů pro atributu názvů:

        using Swashbuckle.Swagger.Annotations;
        
1. Přejděte na adresu URL */swagger/docs/v1* projektu a různých kódů odpovědí HTTP uvidí Swagger JSON. 

    ![](./media/app-service-api-dotnet-swashbuckle-customize/multiple-responses-post-attributes.png)

Kód Tato ukázka najdete v [tomto GitHub úložišti](https://github.com/Azure-Samples/API-Apps-DotNet-Swashbuckle-Customization-MultipleResponseCodes-With-Attributes). Spolu s rozhraní API webových projekt ozdobenou s atributem *SwaggerResponse* je aplikace konzoly projekt, který obsahuje generované klienta pro toto rozhraní API. 

## <a name="next-steps"></a>Další kroky

Tento článek ukazuje, jak přizpůsobit tak, jak Swashbuckle generuje operace ID a kódy platná odpověď. Další informace najdete v tématu [Swashbuckle na GitHub](https://github.com/domaindrivendev/Swashbuckle).
 
