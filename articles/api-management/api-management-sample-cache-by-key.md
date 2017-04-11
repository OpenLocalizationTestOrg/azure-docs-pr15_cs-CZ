<properties
    pageTitle="Vlastní ukládání do mezipaměti Správa rozhraní API Azure"
    description="Zjistěte, jak do mezipaměti položky klíčem správy API Azure"
    services="api-management"
    documentationCenter=""
    authors="darrelmiller"
    manager="erikre"
    editor=""/>

<tags
    ms.service="api-management"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/25/2016"
    ms.author="darrmi"/>

# <a name="custom-caching-in-azure-api-management"></a>Vlastní ukládání do mezipaměti Správa rozhraní API Azure
Služba Azure rozhraní API Management má předdefinované podporu [ukládání do mezipaměti HTTP odpovědí](api-management-howto-cache.md) pomocí adresy URL zdroje jako klávesu. Klíč lze změnit tak, že žádosti o záhlaví pomocí `vary-by` vlastnosti. To je užitečné pro ukládání do mezipaměti celý odpovědi protokolu HTTP (označovaná taky jako znázornění), ale v některých případech je vhodné jenom mezipaměti část na číselné. Nové zásady [hodnota vyhledávání mezipaměti](https://msdn.microsoft.com/library/azure/dn894086.aspx#GetFromCacheByKey) a [hodnota mezipaměti úložiště](https://msdn.microsoft.com/library/azure/dn894086.aspx#StoreToCacheByKey) umožňují uložit a načíst libovolného použitelné části dat z definice zásad. Tato možnost taky přidá hodnotu zásady dříve zavedený [Požadavek odeslání](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) vzhledem k tomu, můžete teď mezipaměti odpovědi od externí služby.

## <a name="architecture"></a>Architektura  
Tak, aby při změně velikosti až několik jednotek se budou pořád dostat k stejná data v mezipaměti: využívá služba rozhraní API správy mezipaměti dat sdílené jednoho klienta. Při práci s více oblastí nasazení existují však nezávislé mezipaměti v každé z oblastí. Z toho důvodu je třeba nebude pracovat s mezipaměti jako datový úložiště, kde je jediný zdrojem některé důležité informace. Pokud jste a později rozhodnete umožní využít výhod nasazení více oblastí, pak zákazníci s uživateli, které cestovní může ztratíte přístup k této data uložená v mezipaměti.

## <a name="fragment-caching"></a>Fragment ukládání do mezipaměti
Existuje určitých případech, kdy odpovědi se vrací obsahují nějaká část dat, která je drahé k určení a ještě stále aktuální rozumné dobu. Jako příklad zvažte službu sestavují leteckou, které obsahuje informace týkající se letu rezervace, stav letech, atd. Pokud je uživatel členem airlines body programu, mají by také informace týkající se jejich aktuální stav a protokolem ujetých sečtenou za. Tyto informace související s uživatelem mohou být uložena v jiném systému, ale je vhodné zahrnout v odpovědích vrátí informace o stavu letu a rezervace. Lze to provést pomocí procesu s názvem ukládání do mezipaměti fragmentu. Primární znázornění může být vrácen ze zdrojového serveru s určitý druh token určujícími, kde má být vložen informace týkající se uživatele. 

Zvažte následující odpověď JSON z back-end rozhraní API.


    {
      "airline" : "Air Canada",
      "flightno" : "871",
      "status" : "ontime",
      "gate" : "B40",
      "terminal" : "2A",
      "userprofile" : "$userprofile$"
    }  

A vedlejší zdroje na `/userprofile/{userid}` , vypadá podobně jako,

     { "username" : "Bob Smith", "Status" : "Gold" }

Abyste mohli určit odpovídající uživatelské informace chcete zahrnout, potřebujeme určit, kdo je koncového uživatele. Tento postup je implementace závislá. Jako příklad používám `Subject` převzetí z `JWT` tokenu. 

    <set-variable
      name="enduserid"
      value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

Jsme uložen `enduserid` hodnotu do proměnné kontext pro pozdější použití. Dalším krokem je zjistit, zda má žádost o předchozí už získat informace o uživateli a uložené v mezipaměti. Pro tento používáme `cache-lookup-value` zásad.

      <cache-lookup-value
      key="@("userprofile-" + context.Variables["enduserid"])"
      variable-name="userprofile" />

Pokud je žádné položky v mezipaměti, které odpovídá hodnoty klíče, pak ne `userprofile` vytvoří kontextu proměnné. Ověření, zda úspěšné pomocí vyhledávání `choose` řízení toku zásad.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("userprofile"))">
            <!— If the userprofile context variable doesn’t exist, make an HTTP request to retrieve it.  -->
        </when>
    </choose>


Pokud `userprofile` neexistuje kontextu proměnné a pak bude potřeba nastavit žádost HTTP obnovit.

    <send-request
      mode="new"
      response-variable-name="userprofileresponse"
      timeout="10"
      ignore-error="true">

      <!-- Build a URL that points to the profile for the current end-user -->
      <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),
          (string)context.Variables["enduserid"]).AbsoluteUri)
      </set-url>
      <set-method>GET</set-method>
    </send-request>

Používáme `enduserid` vytvářet adresa URL pro zdroj profilu uživatele. Jakmile máme odpověď, jsme z vlastního textu mimo odpověď a uložte ji zpátky do proměnné kontextu.

    <set-variable
        name="userprofile"
        value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

Chcete-li předejít nám s opět tuto žádost HTTP u jednoho uživatele, kvůli kterému dalšího požadavku, jsme mohou být uloženy profilů uživatelů v mezipaměti.

    <cache-store-value
        key="@("userprofile-" + context.Variables["enduserid"])"
        value="@((string)context.Variables["userprofile"])" duration="100000" />

Doporučujeme uložit hodnotu do mezipaměti pomocí přesné stejný klíč, který jsme původně pokus o načtení s. Doba trvání, kterou jsme rozhodnete uložit hodnotu by podle toho, jaký informace změn a jak chybám uživatelé jsou často na aktuální informace. 

Je třeba si uvědomit, že načítání z mezipaměti je stále mimo proces, požadavek sítě a potenciálně můžete přidávat desítky milisekund žádost. Výhody pocházet při určování že informace profilů uživatelů trvá mnohem déle než kvůli museli databázové dotazy nebo souhrnné informace z více back konce.

Posledním kroku v procesu je pro informování vrácené odpovědí pomocí našich informace profilů uživatelů.

    <!—Update response body with user profile-->
    <find-and-replace
        from='"$userprofile$"'
        to="@((string)context.Variables["userprofile"])" />

Můžu se rozhodli uvozovky jako součást tokenu tak, že i když nahradit neobjevuje, odpověď byla pořád platná JSON. To bylo primárně usnadnění ladění.

Po sobě kombinovat všechny tyto kroky konečným výsledkem je zásadu, která vypadá jako následující.

     <policies>
        <inbound>
            <!-- How you determine user identity is application dependent -->
            <set-variable
              name="enduserid"
              value="@(context.Request.Headers.GetValueOrDefault("Authorization","").Split(' ')[1].AsJwt()?.Subject)" />

            <!--Look for userprofile for this user in the cache -->
            <cache-lookup-value
              key="@("userprofile-" + context.Variables["enduserid"])"
              variable-name="userprofile" />

            <!-- If we don’t find it in the cache, make a request for it and store it -->
            <choose>
                <when condition="@(!context.Variables.ContainsKey("userprofile"))">
                    <!—Make HTTP request to get user profile -->
                    <send-request
                      mode="new"
                      response-variable-name="userprofileresponse"
                      timeout="10"
                      ignore-error="true">

                       <!-- Build a URL that points to the profile for the current end-user -->
                        <set-url>@(new Uri(new Uri("https://apimairlineapi.azurewebsites.net/UserProfile/"),(string)context.Variables["enduserid"]).AbsoluteUri)</set-url>
                        <set-method>GET</set-method>
                    </send-request>

                    <!—Store response body in context variable -->
                    <set-variable
                      name="userprofile"
                      value="@(((IResponse)context.Variables["userprofileresponse"]).Body.As<string>())" />

                    <!—Store result in cache -->
                    <cache-store-value
                      key="@("userprofile-" + context.Variables["enduserid"])"
                      value="@((string)context.Variables["userprofile"])"
                      duration="100000" />
                </when>
            </choose>
            <base />
        </inbound>
        <outbound>
            <!—Update response body with user profile-->
            <find-and-replace
                  from='"$userprofile$"'
                  to="@((string)context.Variables["userprofile"])" />
            <base />
        </outbound>
     </policies>

Tento přístup mezipaměti primárně používají na webech kde HTML tvoří na straně serveru tak, aby lze vykreslit jako jednu stránku. Však ji může být také užitečné v rozhraní API kde klientů není klienta strana ukládání do mezipaměti HTTP nebo není žádoucí umístění, které odpovídá v klientském počítači.

Tento stejný druh ukládání do mezipaměti fragment také se teď dá na serverech back-end web pomocí Redis ukládání do mezipaměti serveru, ale pomocí služby správy rozhraní API k provedení této práce je užitečná, když režim cached neúplné pocházejí z jiné než primární odpovědi konců zpět.

## <a name="transparent-versioning"></a>Průhledný správy verzí
Je obvyklé více verzí jiné implementaci rozhraní API se plánuje podpora v daném čase. Je to možná pro podporu různých prostředích jako vývojáře test, výrobní, např, nebo může být na podporu starší verze aplikace rozhraní API pro rozhraní API spotřebitele migrovat do novější verzí. 

Jedním ze způsobů zpracování to místo vyžadující vývojářům klienta změnit adres URL z `/v1/customers` k `/v2/customers` je ukládat do data profilu příjemce jakou verzi systému rozhraní API aktuálně chtějí pomocí a volání adresu URL odpovídající back-end. Abyste mohli zjištění adresy URL správné back-end volání pro konkrétní klienta, je nezbytné k vytvoření dotazu některá data konfigurace. Podle těchto konfigurace dat do mezipaměti, jsme minimalizovat snížení výkonu to toto vyhledávání.

Cílem prvního kroku je k určení identifikátor používá ke konfiguraci požadovanou verzi. V tomto příkladu se rozhodli přidružit na verzi předplatného kód product key. 

        <set-variable name="clientid" value="@(context.Subscription.Key)" />

Proveďte jsme mezipaměti vyhledávání zobrazíte, pokud jsme už jste získali verzi požadované klienta.

        <cache-lookup-value
        key="@("clientversion-" + context.Variables["clientid"])"
        variable-name="clientversion" />

Potom jsme zkontrolujte Pokud nebyly nalezeny ho v mezipaměti.

    <choose>
        <when condition="@(!context.Variables.ContainsKey("clientversion"))">

Pokud nám neměli jsme go a načíst.

    <send-request
        mode="new"
        response-variable-name="clientconfiguresponse"
        timeout="10"
        ignore-error="true">
                <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                <set-method>GET</set-method>
    </send-request>

Extrahování textu odpovědi z odpověď.

    <set-variable
          name="clientversion"
          value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />

Uložte ho zpět do mezipaměti pro budoucí použití.

    <cache-store-value
          key="@("clientversion-" + context.Variables["clientid"])"
          value="@((string)context.Variables["clientversion"])"
          duration="100000" />

A nakonec aktualizovat adresu URL back-end vyberte verzi požadované klientem služby.

    <set-backend-service
          base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />

Zásady úplně vypadá takto.

     <inbound>
        <base />
        <set-variable name="clientid" value="@(context.Subscription.Key)" />
        <cache-lookup-value key="@("clientversion-" + context.Variables["clientid"])" variable-name="clientversion" />

        <!-- If we don’t find it in the cache, make a request for it and store it -->
        <choose>
            <when condition="@(!context.Variables.ContainsKey("clientversion"))">
                <send-request mode="new" response-variable-name="clientconfiguresponse" timeout="10" ignore-error="true">
                    <set-url>@(new Uri(new Uri(context.Api.ServiceUrl.ToString() + "api/ClientConfig/"),(string)context.Variables["clientid"]).AbsoluteUri)</set-url>
                    <set-method>GET</set-method>
                </send-request>
                <!-- Store response body in context variable -->
                <set-variable name="clientversion" value="@(((IResponse)context.Variables["clientconfiguresponse"]).Body.As<string>())" />
                <!-- Store result in cache -->
                <cache-store-value key="@("clientversion-" + context.Variables["clientid"])" value="@((string)context.Variables["clientversion"])" duration="100000" />
            </when>
        </choose>
        <set-backend-service base-url="@(context.Api.ServiceUrl.ToString() + "api/" + (string)context.Variables["clientversion"] + "/")" />
    </inbound>


Povolení rozhraní API spotřebitele transparentně určit, jakou verzi back-end přistupuje klientů bez nutnosti aktualizujte a přeinstalujte klientů je elegantní řešení prostředních mnoho pochybnosti rozhraní API správy verzí.

## <a name="tenant-isolation"></a>Izolace klienta

Některé společnosti v větší, více klienta nasazení vytvořit samostatné skupiny klienti na různých nasazení hardwaru back-end. To minimalizuje počet zákazníkům, kteří jsou ovlivněny hardwaru problém na back-end. Umožňuje také nové verze softwaru pro vrátit po krocích. Tato architektura back-end v ideálním případě by měly být průhledné API zákazníkům. To lze dosáhnout podobným způsobem taky na průhledné správy verzí, protože je založena na stejné techniky manipulace URL back-end pomocí stav konfigurace pro rozhraní API klíč.  

Místo vrací upřednostňované verzi rozhraní API pro každý klíč předplatného, by vrátíte identifikátor, které se vztahují ke klientovi ke skupině přiřazené hardwaru. Identifikátor URI, které mohou sloužit k vytváření odpovídající back-end adresy URL.

## <a name="summary"></a>Souhrn
Čitateli pro účely správy mezipaměti Azure API ukládání jakýkoli druh dat umožňuje efektivnější přístup ke konfiguraci datům, která může ovlivnit způsob zpracování příchozí žádosti. Jej lze také uložit fragmenty dat, které můžete rozšířit odpovědi vrácených back-end API.

## <a name="next-steps"></a>Další kroky
Zkontrolujte sdělte nám svůj názor v vlákna Disqus pro toto téma Pokud existují další scénáře, které mají povolenou tyto zásady nebo pokud jsou tu uvedené scénáře chcete dosáhnout, ale ne připadat jsou aktuálně možné.
