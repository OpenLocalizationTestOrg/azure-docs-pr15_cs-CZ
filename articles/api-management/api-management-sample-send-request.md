<properties
    pageTitle="Pomocí služby správy rozhraní API pro generování požadavků HTTP"
    description="Naučte se používat žádostí a odpovědí zásady správy rozhraní API z vaší rozhraní API zavolat externí služby"
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


# <a name="using-external-services-from-the-azure-api-management-service"></a>Použití externích služeb z službu pro správu rozhraní API Azure

K dispozici v služby Azure rozhraní API správy zásad umí širokou škálu užitečné práce ryze na základě příchozí žádosti, odchozí odpovědi a konfigurace základní informace. Však je moct komunikovat s externími službami ze rozhraní API správy zásad se objeví mnoho příležitostí Další.

Jsme dříve viděli, jak jsme můžete pracovat s [služby Azure události Centrum pro protokolování, sledování a analýzy](api-management-log-to-eventhub-sample.md). V tomto článku jsme ukazují zásad, které umožní komunikovat s externími HTTP na základě služby. Tyto zásady lze použít pro spuštění vzdáleného události nebo k načtení informací, které se použijí k manipulaci s nimi původní žádostí a odpovědí určitým způsobem.

## <a name="send-one-way-request"></a>Odeslat jeden způsob požadavku
Možná je nejjednodušší externí interakce styl fire a zapomenete požadavku, který umožňuje externím služby oznámení o určitý druh důležité události. Používáme zásady řízení toku `choose` zjišťování jakýkoli druh podmínky, jsme zajímá a pak pokud splněny, jsme proveďte externí HTTP žádost o konverzaci pomocí zásad [odesílání jeden způsob požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest) . Může to být žádost o zasílání zpráv systému jako Hipchat nebo časová rezerva nebo Hromadná rozhraní API jako SendGrid nebo MailChimp nebo pro případy kritické podpory něco jako PagerDuty. Z těchto zpráv systémů mít jednoduchou HTTP rozhraní API můžete snadno vyvolat.

### <a name="alerting-with-slack"></a>Výstrahy s časová rezerva
Následující příklad ukazuje, jak odeslat zprávu o časové rezervy chatovací místnost, pokud HTTP odpověď stavový kód je větší než nebo rovno 500. 500 oblast chyba signalizuje potíže s naše back-end rozhraní API, klienta naše rozhraní API nerozpoznává sami. Obvykle vyžaduje určitý druh zásah v naší část.  

    <choose>
        <when condition="@(context.Response.StatusCode >= 500)">
          <send-one-way-request mode="new">
            <set-url>https://hooks.slack.com/services/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</set-url>
            <set-method>POST</set-method>
            <set-body>@{
                    return new JObject(
                            new JProperty("username","APIM Alert"),
                            new JProperty("icon_emoji", ":ghost:"),
                            new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                    context.Request.Method,
                                                    context.Request.Url.Path + context.Request.Url.QueryString,
                                                    context.Request.Url.Host,
                                                    context.Response.StatusCode,
                                                    context.Response.StatusReason,
                                                    context.User.Email
                                                    ))
                            ).ToString();
                }</set-body>
          </send-one-way-request>
        </when>
    </choose>

Časová rezerva obsahuje pojmu zavěšení příchozí web. Při konfiguraci zavěšení příchozí web, časová rezerva vygeneruje zvláštní adresy URL, které můžete dělat žádost jednoduchý příspěvek a k předání zprávy do časové rezervy kanálu. JSON textu, kterou jsme vytvořili vychází z formátu definované časová rezerva.

![Časová rezerva Web zavěšení](./media/api-management-sample-send-request/api-management-slack-webhook.png)

### <a name="is-fire-and-forget-good-enough"></a>Je fire a v žádném případě nezapomeňte dostatečně velký?
Při použití stylu fire a zapomenete žádosti o existují některé nevýhody. Pokud z nějakého důvodu selže žádost a pak selhání nebude možné vykázat. V této konkrétní situaci není oprávněné složitost s vedlejší Chyba při vytváření sestav systém a další náročnosti čekání na odpověď. Scénáře, kde je nutné zkontrolovat odpověď zásad [odesílání žádost](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest) je vhodnější.

## <a name="send-request"></a>Požadavek odeslání
`send-request` Zásada umožňuje používat externí služby funkce složité zpracování a vraťte se dat ke správě rozhraní API služeb, které se dá použít pro další zpracování zásad.

### <a name="authorizing-reference-tokens"></a>Povolení tokeny odkazů
Hlavní funkce správy rozhraní API chrání back-end zdroje. Pokud se tak mohli ověřovat server používané vaší rozhraní API vytvoří [JWT tokeny](http://jwt.io/) jako součást jeho OAuth2 tok, stejně jako [Azure Active Directory](../active-directory/active-directory-aadconnect.md) , můžete použít `validate-jwt` zásadu ověřte platnost token. Některé se tak mohli ověřovat servery vytvořit takzvaný [tokeny odkazů](http://leastprivilege.com/2015/11/25/reference-tokens-and-introspection/) , které nelze ověřit bez volání zpět na server se tak mohli ověřovat.

### <a name="standardized-introspection"></a>Standardizovaným introspection
V minulosti byl nijak standardizovaným ověřovat token odkazu pomocí serveru se tak mohli ověřovat. Ale naposledy navrhované standardní [RFC 7662](https://tools.ietf.org/html/rfc7662) byl publikován tak, že IETF, který definuje, jak můžete prostředků serveru ověřte platnost token.

### <a name="extracting-the-token"></a>Extrahování tokenu
Cílem prvního kroku je extrahovat tokenu z hlavičky se tak mohli ověřovat. Hodnotu záhlaví má být formátováno s `Bearer` se tak mohli ověřovat schéma, jednu mezeru a potom se tak mohli ověřovat token podle [RFC 6750](http://tools.ietf.org/html/rfc6750#section-2.1). Existuje bohužel případech, kdy se tak mohli ověřovat schéma vynechán. Chcete-li počítat při analýze, jsme rozdělení hodnotu záhlaví prázdné místo a vyberte posledního řetězce vracenou matici řetězců. Toto záhlaví chybně formátovaný se tak mohli ověřovat poskytuje řešení.

    <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

### <a name="making-the-validation-request"></a>Vytvoření žádosti o ověření
Jakmile máme token se tak mohli ověřovat provedeme ověření tokenu. RFC 7662 volá tento proces introspection a vyžaduje, aby se `POST` formulář HTML k introspection zdroje. Formulář HTML musí obsahovat aspoň pár klíč/hodnota s klávesou `token`. Tento požadavek na server se tak mohli ověřovat musí ověřit taky zajistit, že nelze přejít klientů vlečnou platné tokeny.

    <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
      <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
      <set-method>POST</set-method>
      <set-header name="Authorization" exists-action="override">
        <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
      </set-header>
      <set-header name="Content-Type" exists-action="override">
        <value>application/x-www-form-urlencoded</value>
      </set-header>
      <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
    </send-request>

### <a name="checking-the-response"></a>Kontrola odpověď
`response-variable-name` Atribut je použit abyste umožnili přístup k vrácené odpověď. Název definovaný v této vlastnosti mohou sloužit jako klíč do `context.Variables` slovník pro přístup k `IResponse` objektu.

Z objektu odpověď získáme textu a RFC 7622 víme, že odpověď musí být objekt JSON a musí obsahovat aspoň vlastnost s názvem `active` tedy logická hodnota. Když `active` je PRAVDA, pak tokenu považovány za platné.

### <a name="reporting-failure"></a>Chyba při vytváření sestav
Používáme `<choose>` zásad zjišťování Pokud tokenu není platná a pokud ano, vrátit 401 odpověď.

    <choose>
      <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
        <return-response response-variable-name="existing response variable">
          <set-status code="401" reason="Unauthorized" />
          <set-header name="WWW-Authenticate" exists-action="override">
            <value>Bearer error="invalid_token"</value>
          </set-header>
        </return-response>
      </when>
    </choose>

Podle [RFC 6750](https://tools.ietf.org/html/rfc6750#section-3) , který popisuje jak `bearer` bude použito tokenů, jsme také vracet `WWW-Authenticate` záhlaví s 401 odpověď. Ověření WWW má pokyn klienta o tom, jak vytvořit žádost o správně oprávnění. Z důvodu širokou škálu přístupy možné s OAuth2 framework je těžké komunikovat všechny potřebné informace. Existuje naštěstí úsilí probíhá na pomoc [klientů zjistěte, jak správně povolte žádosti o prostředků serveru](http://tools.ietf.org/html/draft-jones-oauth-discovery-00).

### <a name="final-solution"></a>Konečný řešení
Vložíte všechny části dohromady, doporučujeme vám následující zásady:

    <inbound>
      <!-- Extract Token from Authorization header parameter -->
      <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

      <!-- Send request to Token Server to validate token (see RFC 7662) -->
      <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
        <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
        <set-method>POST</set-method>
        <set-header name="Authorization" exists-action="override">
          <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
        </set-header>
        <set-header name="Content-Type" exists-action="override">
          <value>application/x-www-form-urlencoded</value>
        </set-header>
        <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
      </send-request>

      <choose>
            <!-- Check active property in response -->
            <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
                <!-- Return 401 Unauthorized with http-problem payload -->
                <return-response response-variable-name="existing response variable">
                    <set-status code="401" reason="Unauthorized" />
                    <set-header name="WWW-Authenticate" exists-action="override">
                        <value>Bearer error="invalid_token"</value>
                    </set-header>
                </return-response>
            </when>
        </choose>
      <base />
    </inbound>

Toto je jenom jedna z mnoha příkladů, jak `send-request` zásad mohou sloužit k integraci užitečné externí služby do proces žádosti o schůzku a odpovědi prochází službu pro správu rozhraní API.

## <a name="response-composition"></a>Složení odpověď
`send-request` Zásadu lze použít pro lepší žádost primární koncovému systému, jak jsme viděli v předchozím příkladu, nebo mohou sloužit jako úplný nahradit pro volání back-end. Pomocí tohoto postupu jsme mohli snadno vytvářet složené prostředky, které jsou agregované z několika různých systémů.

### <a name="building-a-dashboard"></a>Vytvoření řídicího panelu   
Někdy budete chtít mít možnost k zobrazování informací rozlohu ve více back-end systémů, třeba, řídit řídicího panelu. Klíčové ukazatele výkonu pocházet z všechny jiné zpět konce, ale chcete raději poskytovat přímý přístup k nim a bude hodní, všechny informace o mohou získat v jednom požadavku. Možná některé informace back-end vyžaduje některé rozdělení na řezy a kostky a nevelká sanitizing nejdřív! Možnost do mezipaměti složené zdroje mohly být užitečné snížit načíst back-end jako víte, že uživatelé mají zvykněte hammering klávesu F5 Pokud chcete zjistit, zda může změnit jejich underperforming metriky.    

### <a name="faking-the-resource"></a>Faking zdroje
Prvním krokem při vytváření naše řídicího panelu zdroje je konfigurace nová operace na portálu Správa rozhraní API aplikace publisher. To bude operace zástupný symbol používá ke konfiguraci zásadách složení vytvářet naše dynamické zdroje.

![Operace řídicího panelu](./media/api-management-sample-send-request/api-management-dashboard-operation.png)

### <a name="making-the-requests"></a>Vytvoření žádosti
Jednou `dashboard` vytvořil operace jsme konfigurace zásad pro operaci. 

![Operace řídicího panelu](./media/api-management-sample-send-request/api-management-dashboard-policy.png)

Cílem prvního kroku je extrahovat všechny parametry dotazu z příchozí žádosti tak, aby jsme můžete předávat na naše back-end. V tomto příkladu je naše řídicí panel zobrazující informace podle určitého časového období proto má `fromDate` a `toDate` parametr. Můžete použít `set-variable` zásadu extrahovat informace z požadavku na adresu URL.

    <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
    <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

Jakmile máme tyto informace jsme všech systémů back-end požádat. Každý požadavek vytvoří nová adresa URL informacemi parametr volání příslušným serveru a uloží odpověď v kontextu proměnné.

    <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
      <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
      <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

    <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
    <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
      <set-method>GET</set-method>
    </send-request>

Postup, který není ideální spustí, tyto požadavky. V nadcházející verzi jsme představení nové zásady s názvem `wait` , která vám umožní všechny tyto požadavky provést souběžně.

### <a name="responding"></a>Odpověď

K vytvoření složeného odpovědi používáme zásadu [Vrácená odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) . `set-body` Prvek výraz můžete použít k vytvoření nového `JObject` s všechny komponenty vyjádření vložené jako vlastnosti.

    <return-response response-variable-name="existing response variable">
      <set-status code="200" reason="OK" />
      <set-header name="Content-Type" exists-action="override">
        <value>application/json</value>
      </set-header>
      <set-body>
        @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                      new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                      new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                      new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                      ).ToString())
      </set-body>
    </return-response>

Celou smlouvu vypadá takto:

    <policies>
        <inbound>

      <set-variable name="fromDate" value="@(context.Request.Url.Query["fromDate"].Last())">
      <set-variable name="toDate" value="@(context.Request.Url.Query["toDate"].Last())">

        <send-request mode="new" response-variable-name="revenuedata" timeout="20" ignore-error="true">
          <set-url>@($"https://accounting.acme.com/salesdata?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="materialdata" timeout="20" ignore-error="true">
          <set-url>@($"https://inventory.acme.com/materiallevels?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="throughputdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <send-request mode="new" response-variable-name="accidentdata" timeout="20" ignore-error="true">
        <set-url>@($"https://production.acme.com/throughput?from={(string)context.Variables["fromDate"]}&to={(string)context.Variables["fromDate"]}")"</set-url>
          <set-method>GET</set-method>
        </send-request>

        <return-response response-variable-name="existing response variable">
          <set-status code="200" reason="OK" />
          <set-header name="Content-Type" exists-action="override">
            <value>application/json</value>
          </set-header>
          <set-body>
            @(new JObject(new JProperty("revenuedata",((IResponse)context.Variables["revenuedata"]).Body.As<JObject>()),
                          new JProperty("materialdata",((IResponse)context.Variables["materialdata"]).Body.As<JObject>()),
                          new JProperty("throughputdata",((IResponse)context.Variables["throughputdata"]).Body.As<JObject>()),
                          new JProperty("accidentdata",((IResponse)context.Variables["accidentdata"]).Body.As<JObject>())
                          ).ToString())
          </set-body>
        </return-response>
        </inbound>
        <backend>
            <base />
        </backend>
        <outbound>
            <base />
        </outbound>
    </policies>

V konfiguraci zástupného symbolu operace jsme konfigurace prostředků řídicího panelu v mezipaměti pro aspoň jednu hodinu vzhledem k tomu, že nám jasné, že přírodní data znamená, že i když je za hodinu vypršela platnost, že nebude dostatečně efektivní ke sdělení informací uživatelům.

## <a name="summary"></a>Souhrn
Azure služby správy rozhraní API nabízí flexibilní zásad, které se dají selektivně použít pro přenos HTTP a umožňuje složení back-end služeb. Jestli chcete vylepšit brány rozhraní API s upozornění funkce ověření, funkce ověření nebo vytvořit nové složené zdroje založené na více back-end services `send-request` a související zásady otevřít svět možnosti.

## <a name="watch-a-video-overview-of-these-policies"></a>Podívejte se na video přehled tyto zásady
Další informace o [odesílání jeden způsob požadavku](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendOneWayRequest), [Požadavek odeslání](https://msdn.microsoft.com/library/azure/dn894085.aspx#SendRequest)a [Vrácená odpověď](https://msdn.microsoft.com/library/azure/dn894085.aspx#ReturnResponse) zásady uvedené v tomto článku podívejte se prosím následující video.

> [AZURE.VIDEO send-request-and-return-response-policies]
