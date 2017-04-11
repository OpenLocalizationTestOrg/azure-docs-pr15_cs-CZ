<properties
    pageTitle="Aplikace služby rozhraní API aplikace metadat pro rozhraní API zjišťování a hodnocení kód generování | Microsoft Azure"
    description="Zjistěte, jak rozhraní API aplikace v aplikaci služby Azure pomocí metadat Swagger usnadnit rozhraní API zjišťování a hodnocení kód generování."
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>Aplikace služby rozhraní API aplikace metadat pro rozhraní API zjišťování a hodnocení kód generování 

Podpora pro metadata rozhraní API [Swagger 2.0](http://swagger.io/) je integrovaná v aplikaci služby rozhraní API aplikace. Nemusíte používat Swagger, ale pokud ji použijete, můžete využít rozhraní API aplikace funkcí, které snazší zjišťování a hodnocení spotřebu.   

## <a name="swagger-endpoint"></a>Swagger koncový bod

Můžete určit, které obsahuje Swagger 2.0 JSON metadat rozhraní API aplikace ve vlastnosti aplikace rozhraní API pro koncový bod. Koncový bod může být relativní základní adresu URL rozhraní API aplikace nebo absolutní adresa URL. Absolutní adresy URL přesunutím ukazatele myši vně rozhraní API aplikace. 

Řada podřízenými klientů (pro například generování kódu Visual Studiu a PowerApps "Přidat rozhraní API" toku), adresu URL, musí být veřejně přístupný (ne chráněný uživatele nebo ověření služby). To znamená, že pokud používáte aplikaci služby ověřování a chcete vystavit definici rozhraní API z v rámci samotnou aplikaci, musíte použít ověřování možnost, která umožňuje anonymní přenos kontaktovat vaše rozhraní API. Další informace najdete v tématu [a tak mohli ověřovat pro aplikaci služby rozhraní API aplikace](app-service-api-authentication.md).

### <a name="portal-blade"></a>Portál zásuvné

[Azure portál](https://portal.azure.com/) může vidět a změnit na zásuvné **Rozhraní API definice** adresa URL koncového bodu.

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Vlastnost Azure správce prostředků

Můžete taky nakonfigurovat URL definice rozhraní API aplikace rozhraní API pro pomocí [Průzkumníka zdroje](https://resources.azure.com/) nebo [Správce prostředků Azure šablony](../resource-group-authoring-templates.md) v nástrojích příkazového řádku například [Azure PowerShell](../powershell-install-configure.md) a [Azure rozhraní příkazového řádku](../xplat-cli-install.md). 

V **Průzkumníkovi zdroje**, přejděte na **předplatná > {předplatného} > resourceGroups > {skupiny zdrojů} > poskytovatelů > Microsoft.Web > weby > {webu} > config > web**, a zobrazí se `apiDefinition` vlastnosti:

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

Příklad šablony správce prostředků Azure, která nastavuje `apiDefinition` vlastnost, otevřete [soubor azuredeploy.json v ukázkové aplikaci seznam úkolů](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json). Najděte část šablonu, která vypadá jako ukázka JSON najdete ji výše.

### <a name="default-value"></a>Výchozí hodnota

Po vytvoření aplikace pro rozhraní API pomocí aplikace Visual Studio koncový bod definice rozhraní API automaticky nastavit základní adresu URL a rozhraní API aplikace `/swagger/docs/v1`. Toto je výchozí adresu URL používající balíček [Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet a bude předávat dynamicky generované Swagger metadat pro projekt rozhraní API webových ASP.NET. 

## <a name="code-generation"></a>Generování kódu

Jednou z výhod integrace Swagger do Azure rozhraní API aplikace je generování automatické kódu. Třídy vygenerovaných klientů snadněji psát kód, který volá rozhraní API aplikace.

Generovat kód klienta pro rozhraní API aplikace Visual Studio, nebo z příkazového řádku. Informace o tom, jak generovat třídy klientů ve Visual Studiu pro projekt rozhraní API webových ASP.NET najdete v tématu [Začínáme s aplikací rozhraní API a ASP.NET](app-service-api-dotnet-get-started.md#codegen). Informace o tom, jak to udělat z příkazového řádku pro všechny jazyky podporované najdete v tématu souboru readme [Azure/autorest](https://github.com/azure/autorest) úložiště na GitHub.com.
 
## <a name="next-steps"></a>Další kroky

Podrobný návod, provede vytváření nasazení a jinými aplikaci rozhraní API, najdete v článku [Začínáme s aplikací rozhraní API aplikace služby Azure](app-service-api-dotnet-get-started.md).

Pokud používáte Azure rozhraní API Management s aplikacemi jiných rozhraní API, můžete k importu svého rozhraní API na stránce pro správu rozhraní API Swagger metadat. Další informace najdete v článku [jak importovat definici rozhraní API s operacemi správy rozhraní API Azure](../api-management/api-management-howto-import-api.md). 
