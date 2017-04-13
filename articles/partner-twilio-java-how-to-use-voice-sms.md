<properties 
    pageTitle="Jak používat Twilio pro hlasové zprávy a zprávy SMS (Java) | Microsoft Azure" 
    description="Zjistěte, jak chcete někomu zavolat a odeslání zprávy SMS ke službě rozhraní API Twilio na Azure. Ukázky napsané v Java." 
    services="" 
    documentationCenter="java" 
    authors="devinrader" 
    manager="twilio" 
    editor="mollybos"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="Java" 
    ms.topic="article" 
    ms.date="11/25/2014" 
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-java"></a>Jak používat Twilio pro hlasovou a služby SMS možnosti jazyka Java

Tato příručka ukazuje, jak provádět běžné úlohy programování pomocí rozhraní API Twilio služby Azure. Scénáře nichž se uplatní zahrnují uskutečnit telefonní hovor a posílání zpráv služby krátké zprávy (SMS). Další informace o Twilio a používání hlasové zprávy a zprávy SMS v aplikacích naleznete v části [Další kroky](#NextSteps) .

## <a id="WhatIs"></a>Co je Twilio?
Twilio je rozhraní API telefonních webové služby, které můžete použít existující web jazyky a dovedností k vytvoření hlasu a aplikací služby SMS. Twilio je služba jiných výrobců (ne Azure funkce a ne k produktu Microsoft).

**Twilio hlasu** umožňuje aplikace uskutečňovat a přijímat telefonní hovory. **Služby SMS Twilio** umožňuje aplikace uskutečňovat a přijímat zprávy SMS. Umožní **Twilio klient** aplikace povolit hlasové komunikace pomocí existujícího připojení k Internetu, včetně mobilního připojení.

## <a id="Pricing"></a>Twilio ceny a speciální nabídky
Informace o cenách Twilio je k dispozici [Cena za Twilio] [twilio_pricing]. Azure zákazníci obdrží [zvláštní nabídkou][special_offer]: bezplatné kreditních 1000 texty nebo 1000 příchozí minut. Registrace k této nabídce nebo získání dalších informací, navštivte [http://ahoy.twilio.com/azure][special_offer].  

## <a id="Concepts"></a>Koncepty
Rozhraní API Twilio je rozhraní API RESTful poskytující hlasové zprávy a funkce služby SMS pro aplikace. Knihoven klienta jsou k dispozici ve více jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny] [twilio_libraries].

Klíčové aspekty rozhraní API Twilio jsou Twilio slovesa a Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio akce
Rozhraní API využívá Twilio slovesa; například ** &lt;Přivítejte&gt; ** slovesné pokyn Twilio zvukově prezentování zprávu na volání. 

Následuje seznam Twilio akcí.

* ** &lt;Dial&gt;**: připojí volající na jiný telefon.
* ** &lt;Shromáždění&gt;**: shromažďuje číslicemi zadaná na klávesnici telefonu.
* ** &lt;Zavěšení&gt;**: ukončí hovor.
* ** &lt;Přehrát&gt;**: přehrávání zvukového souboru.
* ** &lt;Pozastavit&gt;**: tiše čeká určitý počet sekund.
* ** &lt;Záznam&gt;**: zaznamenává volajícího hlasu a vrátí URL soubor, který obsahuje záznam.
* ** &lt;Přesměrovat&gt;**: převede ovládací prvek volání nebo služby SMS TwiML na jinou adresu URL.
* ** &lt;Zamítnout&gt;**: odmítne příchozí volání na číslo Twilio bez fakturace můžete
* ** &lt;Přivítejte&gt;**: převede text na řeč, která je vytvořená Telefonuji.
* ** &lt;Služby Sms&gt;**: odešle zprávu služby SMS.

### <a id="TwiML"></a>TwiML
TwiML je sada založeném na jazyce XML pokynů podle Twilio akce, které informuje Twilio způsobu zpracování hovoru nebo SMS.

Jako příklad následující TwiML by převést **Vítáme** na řeč.

    <?xml version="1.0" encoding="UTF-8" ?>
    <Response>
       <Say>Hello World</Say>
    </Response>

Aplikace zavolá rozhraní API Twilio, parametrů rozhraní API při adresu URL, která vrací TwiML odpověď. Pro účely development můžete adresy URL poskytnutá Twilio poskytnout odpovědi TwiML používané aplikace. Může taky hostovat vlastní adresy URL k vytvoření odpovědi na TwiML a další možností je použití objektu **TwiMLResponse** .

Další informace o Twilio akcí, jejich atributy a TwiML najdete v tématu [TwiML] [twiml]. Další informace o rozhraní API Twilio, najdete v článku [Rozhraní API Twilio] [twilio_api].

## <a id="CreateAccount"></a>Vytvořit účet Twilio
Až budete chtít zřízení účtu Twilio, přihlásit na [Akci Twilio] [try_twilio]. Můžete začít s bezplatného účtu a později upgradovat, váš účet.

Při registraci k účtu Twilio dostanete ID účtu a token ověřování. Jak je potřeba volat rozhraní API Twilio. Abyste zabránili neoprávněnému přístupu ke svému účtu, aby byly v ověřovací token bezpečí. ID účtu a ověření token je možné zobrazit na [stránce účtu Twilio] [twilio_account]v polích s popisky **účet ID zabezpečení** a **AUTH TOKENU**, respektive.

## <a id="create_app"></a>Vytvoření aplikace Java
1. Získání Twilio JAR a přiřadit ji někomu cesty buildu Java a vaše WAR nasazení sestavení. Na [https://github.com/twilio/twilio-java][twilio_java], můžete stáhnout GitHub zdrojů a vytvořit vlastní SKLENICE nebo stáhnout předdefinovaných SKLENICE (s nebo bez závislosti).
2. Zajistit, aby vaše JDK **cacerts** keystore obsahuje Equifax zabezpečené certifikační autorita certifikátu s MD5 otisku 67:CB:9 D: C0:13:24:8A:82:9B:B2:17:1E:D1:1B:EC:D4 (pořadové číslo je 35:DE:F4:CF a otisku SHA1 D2:32:09:AD:23:D3:14:23:21:74:E4:0 D: 7F:9 D: 62:13:97:86:63:3A). Certifikační autorita (CA) certifikát pro [https://api.twilio.com] [ twilio_api_service] služba, která se nazývá při použití rozhraní API Twilio. Informace o zajištění vaší JDK **cacerts** keystore obsahuje správné certifikační najdete v článku [Přidání certifikátu do úložiště certifikátů CA Java][add_ca_cert].

Podrobné pokyny k používání knihovny klienta Twilio jazyka Java jsou dostupné na to, [jak provádět Twilio pomocí telefonního hovoru v aplikaci Java na Azure][howto_phonecall_java].

## <a id="configure_app"></a>Konfigurace aplikace pomocí knihoven Twilio
V rámci vašeho kódu můžete přidat **importovat** příkazy v horní části zdrojových souborů balíčků Twilio nebo třídy, kterou chcete použít v aplikaci. 

Pro Java zdrojové soubory:

    import com.twilio.*;
    import com.twilio.sdk.*;
    import com.twilio.sdk.resource.factory.*;
    import com.twilio.sdk.resource.instance.*;

Stránka serveru Java (JSP) zdrojové soubory:

    import="com.twilio.*"
    import="com.twilio.sdk.*"
    import="com.twilio.sdk.resource.factory.*"
    import="com.twilio.sdk.resource.instance.*"
Podle toho, jaký Twilio balíčků nebo třídy, kterou chcete použít může být vaše příkazy **import** jiné.

## <a id="howto_make_call"></a>Postup: odchozí hovor
Na následujícím obrázku je vytvoření odchozího hovoru pomocí třídy **CallFactory** . Tento kód taky používá webem Twilio-za předpokladu, že k vrácení odpověď Twilio Markup Language (TwiML). Nahradit hodnoty **od** a **do** telefonní čísla a zrušte zaškrtnutí ověření telefonní číslo **z** účtu Twilio před spuštěním kód.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the CallFactory.
    Account account = client.getAccount();

    // Use the Twilio-provided site for the TwiML response.
    String Url="http://twimlets.com/message";
    Url = Url + "?Message%5B0%5D=Hello%20World";

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN"); // Use your own value for the second parameter.
    params.put("To", "NNNNNNNNNN");   // Use your own value for the second parameter.
    params.put("Url", Url);

    // Create an instance of the CallFactory class.
    CallFactory callFactory = account.getCallFactory();

    // Make the call.
    Call call = callFactory.create(params);

Další informace o parametrech v metody **CallFactory.create** najdete v tématu [http://www.twilio.com/docs/api/rest/making-calls][twilio_rest_making_calls].

Jak je uvedeno, používá tento kód webem Twilio-za předpokladu, že k vrácení TwiML odpověď. Můžete místo toho použít vlastní webu poskytnout TwiML odpověď; Další informace najdete v tématu [jak poskytnout TwiML odpovědí v aplikaci Java na Azure](#howto_provide_twiml_responses).

## <a id="howto_send_sms"></a>Postup: odeslání zprávy SMS
Na následujícím obrázku je způsob odeslání zprávy SMS pomocí **SmsFactory** předmětu. **z** čísla, **4155992671**poskytuje společnost Twilio pro zkušební účty k odeslání zprávy SMS. Číslo **na** musí být ověřený účtu Twilio před spuštěním kód.

    // Use your account SID and authentication token instead
    // of the placeholders shown here.
    String accountSID = "your_twilio_account";
    String authToken = "your_twilio_authentication_token";

    // Create an instance of the Twilio client.
    TwilioRestClient client;
    client = new TwilioRestClient(accountSID, authToken);

    // Retrieve the account, used later to create an instance of the SmsFactory.
    Account account = client.getAccount();

    // Send an SMS message.
    MessageFactory messageFactory = account.getMessageFactory();
    
    List<NameValuePair> params = new ArrayList<NameValuePair>();
    params.add(new BasicNameValuePair("To", "+14159352345")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("From", "+14158141829")); // Replace with a valid phone number for your account.
    params.add(new BasicNameValuePair("Body", "Where's Wallace?"));
    
    Message sms = messageFactory.create(params);
        
Další informace o parametrech v metody **SmsFactory.create** najdete v tématu [http://www.twilio.com/docs/api/rest/sending-sms][twilio_rest_sending_sms].

## <a id="howto_provide_twiml_responses"></a>Postup: Zadejte TwiML odpovědi z vlastní webu
Když aplikace zahájí volání rozhraní API Twilio, například pomocí metody **CallFactory.create** Twilio odešle žádosti o adresu URL, která se má vrátit TwiML odpověď. Výše uvedený příklad používá Twilio-za předpokladu, že adresa URL [http://twimlets.com/message][twimlet_message_url]. (Když TwiML je určen pro webové služby, můžete zobrazit TwiML v prohlížeči. Například, klikněte na možnost [http://twimlets.com/message] [ twimlet_message_url] zobrazíte prázdné ** &lt;odpověď&gt; ** element. jiný příklad, klikněte na [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] zobrazíte ** &lt;odpověď&gt; ** prvek, který obsahuje ** &lt;Přivítejte&gt; ** elementu.)

Namísto na adrese URL poskytnutá Twilio, můžete vytvořit vlastní adresu URL webu, který vrací odpovědi protokolu HTTP. Vytvoření webu na libovolný jazyk, který vrací odpovědi protokolu HTTP; Toto téma předpokládá, že budete hostingu adresu URL na stránce JSP.

Na následující stránce JSP výsledkem TwiML odpověď, že **Vítáme** hovoru.

    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello World</Say>
    </Response>

Na následující stránce JSP výsledkem TwiML odpověď, která říká nějaký text, obsahuje několik pozastaví a uvádí informace o verzi rozhraní API Twilio a název Azure role.


    <%@ page contentType="text/xml" %>
    <Response> 
        <Say>Hello from Azure</Say>
        <Pause></Pause>
        <Say>The Twilio API version is <%= request.getParameter("ApiVersion") %>.</Say>
        <Say>The Azure role name is <%= System.getenv("RoleName") %>.</Say>
        <Pause></Pause>
        <Say>Good bye.</Say>
    </Response>

Parametr **ApiVersion** je k dispozici v žádosti o Twilio voice (není žádosti o služby SMS). K dispozici žádost parametrů Twilio hlasu a žádosti o služby SMS zobrazíte najdete v článku <https://www.twilio.com/docs/api/twiml/twilio_request> a <https://www.twilio.com/docs/api/twiml/sms/twilio_request>. Proměnná prostředí **RoleName** je k dispozici v rámci Azure nasazení. (Pokud chcete přidat vlastní proměnné tak fotek může být vybraných kontakty z **System.getenv**, naleznete v části prostředí proměnné na [Různé Role nastavení][misc_role_config_settings].)

Až budete mít stránku JSP nastavena na poskytnout TwiML odpovědi, použijte adresu URL JSP stránky jako adresu URL předaný metodu **CallFactory.create** . Například, pokud jste ještě webovou aplikaci s názvem MyTwiML používaný Azure hostované služby a název stránky JSP mytwiml.jsp, adresu URL můžete předávat **CallFactory.create** uvedeno v následující:

    // Place the call From, To and URL values into a hash map. 
    HashMap<String, String> params = new HashMap<String, String>();
    params.put("From", "NNNNNNNNNN");
    params.put("To", "NNNNNNNNNN");
    params.put("Url", "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.jsp");

    CallFactory callFactory = account.getCallFactory();
    Call call = callFactory.create(params);

Další možností pro porůznu TwiML spočívá ve využití **TwiMLResponse** předmětu, který je dostupný v okně **com.twilio.sdk.verbs** balíček.

Další informace o používání Twilio v Azure pomocí jazyka Java informace [o možnostech Twilio pomocí telefonního hovoru v aplikaci Java na Azure][howto_phonecall_java].

## <a id="AdditionalServices"></a>Postup: pomocí dalších Twilio služby
Kromě příklady ukazuje tato část nabízí Twilio rozhraní API webových, která můžete využít další funkce Twilio Azure aplikace. Úplné podrobnosti najdete v [dokumentaci k rozhraní API Twilio] [twilio_api_documentation].

## <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základní informace o službě Twilio, tyto odkazy vedou na další informace:

* [Pokyny pro Twilio zabezpečení] [twilio_security_guidelines]
* [Twilio postupy a příklady kódu] [twilio_howtos]
* [Výukové programy pro rychlý úvod Twilio][twilio_quickstarts] 
* [Twilio na GitHub] [twilio_on_github]
* [Obraťte se na podporu Twilio] [twilio_support]

[twilio_java]: https://github.com/twilio/twilio-java
[twilio_api_service]: https://api.twilio.com
[add_ca_cert]: java-add-certificate-ca-store.md
[howto_phonecall_java]: partner-twilio-java-phone-call-example.md
[misc_role_config_settings]: http://msdn.microsoft.com/library/windowsazure/hh690945.aspx
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twilio_rest_making_calls]: http://www.twilio.com/docs/api/rest/making-calls
[twilio_rest_sending_sms]: http://www.twilio.com/docs/api/rest/sending-sms
[twilio_pricing]: http://www.twilio.com/pricing
[special_offer]: http://ahoy.twilio.com/azure
[twilio_libraries]: https://www.twilio.com/docs/libraries
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api]: http://www.twilio.com/api
[try_twilio]: https://www.twilio.com/try-twilio
[twilio_account]:  https://www.twilio.com/user/account
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[twilio_api_documentation]: http://www.twilio.com/api
[twilio_security_guidelines]: http://www.twilio.com/docs/security
[twilio_howtos]: http://www.twilio.com/docs/howto
[twilio_on_github]: https://github.com/twilio
[twilio_support]: http://www.twilio.com/help/contact
[twilio_quickstarts]: http://www.twilio.com/docs/quickstart
