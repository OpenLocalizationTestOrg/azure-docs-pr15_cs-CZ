<properties
    pageTitle="Jak používat Twilio pro hlasové zprávy a zprávy SMS (PHP) | Microsoft Azure"
    description="Zjistěte, jak chcete někomu zavolat a odeslání zprávy SMS ke službě rozhraní API Twilio na Azure. Ukázky napsané v PHP."
    services=""
    documentationCenter="python"
    authors="devinrader"
    manager="twilio"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="python" 
    ms.topic="article"
    ms.date="02/19/2015"
    ms.author="MicrosoftHelp@twilio.com"/>





# <a name="how-to-use-twilio-for-voice-and-sms-capabilities-in-php"></a>Jak používat Twilio pro hlasovou a možnosti služby SMS PHP
Tato příručka ukazuje, jak provádět běžné úlohy programování pomocí rozhraní API Twilio služby Azure. Scénáře nichž se uplatní zahrnují uskutečnit telefonní hovor a posílání zpráv služby krátké zprávy (SMS). Další informace o Twilio a používání hlasové zprávy a zprávy SMS v aplikacích naleznete v části [Další kroky](#NextSteps) .

## <a id="WhatIs"></a>Co je Twilio?
Twilio je napájení budoucí komunikaci s obchodními, umožňuje vývojářům vložení hlasové VoIP a zpráv do aplikace. Budou při virtualizaci všechny potřebné v cloudu, globální prostředí vystavení prostřednictvím komunikace rozhraní API platformy Twilio infrastruktury. Aplikace se snadno vytvářet scalable. Využijte flexibilitu s platovou jako – můžete přejít ceny a využívat cloud spolehlivost.

**Twilio hlasu** umožňuje aplikace uskutečňovat a přijímat telefonní hovory. **Twilio** umožňuje aplikace odesílat a přijímat textové zprávy. **Klient Twilio** umožňuje pro hovory VoIP z telefonu, tabletu nebo prohlížeče a podporuje WebRTC.

## <a id="Pricing"></a>Twilio ceny a speciální nabídky

Azure zákazníkům [zvláštní nabídkou] 10 Twilio Dal po upgradu účtu Twilio. Tento Twilio platební lze použít pro všechny Twilio použití (10 Dal odpovídající až 1 000 zprávy SMS posílání a přijímání až 1000 příchozí hlasový minut, v závislosti na umístění cíle vaše telefonní číslo a zprávy nebo volání). Uplatnění tento Twilio platební a Začínáme na: [ahoy.twilio.com/azure].

Twilio je systému průběžného financování služba. Existuje žádné instalace sestavení poplatky a uzavření účtu kdykoli. Další informace najdete na [Twilio] [twilio_pricing].

## <a id="Concepts"></a>Koncepty
Rozhraní API Twilio je rozhraní API RESTful poskytující hlasové zprávy a funkce služby SMS pro aplikace. Knihoven klienta jsou k dispozici ve více jazycích; seznam najdete v tématu [Twilio rozhraní API knihovny] [twilio_libraries].

Klíčové aspekty rozhraní API Twilio jsou Twilio slovesa a Twilio Markup Language (TwiML).

### <a id="Verbs"></a>Twilio akce
Rozhraní API využívá Twilio slovesa; například ** &lt;Přivítejte&gt; ** slovesné pokyn Twilio zvukově prezentování zprávu na volání.

Následuje seznam Twilio akcí. Informace o dalších příkazů a možností prostřednictvím [Twilio Markup Language přečtěte následující dokumentaci pro] [http://www.twilio.com/docs/api/twiml].

* ** &lt;Dial&gt;**: připojí volající na jiný telefon.
* ** &lt;Shromáždění&gt;**: shromažďuje číslicemi zadaná na klávesnici telefonu.
* ** &lt;Zavěšení&gt;**: ukončí hovor.
* ** &lt;Přehrát&gt;**: přehrávání zvukového souboru.
* ** &lt;Pozastavit&gt;**: tiše čeká určitý počet sekund.
* ** &lt;Záznam&gt;**: zaznamenává hlasové volající a vrátí URL soubor, který obsahuje záznam.
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

Při registraci k účtu Twilio přijmete ID účtu a token ověřování. Jak je potřeba volat rozhraní API Twilio. Abyste zabránili neoprávněnému přístupu ke svému účtu, aby byly v ověřovací token bezpečí. ID účtu a ověření token je možné zobrazit na [stránce účtu Twilio] [twilio_account]v polích s popisky **účet ID zabezpečení** a **AUTH TOKENU**, respektive.

## <a id="create_app"></a>Vytvoření aplikace PHP
PHP aplikace, která používá službu Twilio a běží v Azure neliší od jakékoli jiné aplikace PHP, využívající službu Twilio. Během Twilio služby jsou založeny na ZBÝVAJÍCÍ a od PHP můžete volat několika způsoby, tento článek se zaměřuje na používání služeb Twilio s [knihovnou Twilio pro PHP z GitHub][twilio_php]. Další informace o používání knihovnu Twilio pro PHP, najdete v článku [http://readthedocs.org/docs/twilio-php/en/latest/index.html][twilio_lib_docs].

Podrobné pokyny k vytvoření a nasazení aplikace Twilio/PHP Azure jsou dostupné na to, [jak provádět Twilio pomocí telefonního hovoru v aplikaci PHP na Azure][howto_phonecall_php].

## <a id="configure_app"></a>Konfigurace aplikace pomocí knihoven Twilio
Můžete nakonfigurovat aplikaci pro účely knihovnu Twilio PHP dvěma způsoby:

1. Stáhnout knihovnu Twilio pro PHP z GitHub ([https://github.com/twilio/twilio-php][twilio_php]) a přidejte **Services** directory aplikaci.

    – NEBO –

2. Instalace knihovnu Twilio PHP jako balíček HRUŠNÍ. Možné nainstalovat pomocí následujících příkazů:

        $ pear channel-discover twilio.github.com/pear
        $ pear install twilio/Services_Twilio

Po instalaci knihovnu Twilio pro PHP můžete přidat výkaz **require_once** klikněte v horní části souborů PHP neodkazuje knihovny:

        require_once 'Services/Twilio.php';

Další informace najdete v tématu [https://github.com/twilio/twilio-php/blob/master/README.md][twilio_github_readme].

## <a id="howto_make_call"></a>Postup: odchozí hovor
Na následujícím obrázku je vytvoření odchozího hovoru pomocí třídy **Services_Twilio** . Tento kód taky používá webem Twilio-za předpokladu, že k vrácení odpověď Twilio Markup Language (TwiML). Nahradit hodnoty **od** a **do** telefonní čísla a zrušte zaškrtnutí ověření telefonní číslo **z** účtu Twilio před spuštěním kód.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";

    // The number of the phone initiating the the call.
    // (Must be previously validated with Twilio.)
    $from_number = "NNNNNNNNNNN";

    // The number of the phone receiving call.
    $to_number = "NNNNNNNNNNN";

    // Use the Twilio-provided site for the TwiML response.
    $url = "http://twimlets.com/message";

    // The phone message text.
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    //Make the call.
    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Jak je uvedeno, používá tento kód webem Twilio-za předpokladu, že k vrácení TwiML odpověď. Můžete místo toho použít vlastní webu poskytnout TwiML odpověď; Další informace najdete v tématu [jak poskytnout TwiML odpovědi z svůj vlastní webu](#howto_provide_twiml_responses).


- **Poznámka**: Poradce při potížích ověřovací certifikát SSL, najdete v článku [http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html][ssl_validation]


## <a id="howto_send_sms"></a>Postup: odeslání zprávy SMS
Na následujícím obrázku je způsob odeslání zprávy SMS pomocí **Services_Twilio** předmětu. Číslo **z** poskytuje společnost Twilio pro zkušební účty k odeslání zprávy SMS. Číslo **na** musí být ověřený účtu Twilio před spuštěním kód.

    // Include the Twilio PHP library.
    require_once 'Services/Twilio.php';

    // Library version.
    $version = "2010-04-01";

    // Set your account ID and authentication token.
    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";


    $from_number = "NNNNNNNNNNN"; // With trial account, texts can only be sent from your Twilio number.
    $to_number = "NNNNNNNNNNN";
    $message = "Hello world.";

    // Create the call client.
    $client = new Services_Twilio($sid, $token, $version);

    // Send the SMS message.
    try
    {
        $client->account->sms_messages->create($from_number, $to_number, $message);
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

## <a id="howto_provide_twiml_responses"></a>Postup: Zadejte TwiML odpovědi z vlastní webu
Pokud aplikace zahájí volání rozhraní API Twilio, Twilio odeslání žádosti o adresu URL, která se má vrátit TwiML odpověď. Výše uvedený příklad používá Twilio-za předpokladu, že adresa URL [http://twimlets.com/message][twimlet_message_url]. (Když TwiML je určen pro Twilio, můžete zobrazit it v prohlížeči. Například, klikněte na možnost [http://twimlets.com/message] [ twimlet_message_url] zobrazíte prázdné `<Response>` element. jiný příklad, klikněte na [http://twimlets.com/message?Message%5B0%5D=Hello%20World] [ twimlet_message_url_hello_world] zobrazíte `<Response>` prvek, který obsahuje `<Say>` element.)

Namísto na adrese URL poskytnutá Twilio, můžete vytvořit vlastní webu, který vrací odpovědi protokolu HTTP. Vytvoření webu na libovolný jazyk, který vrací odpovědi XML; Toto téma předpokládá, že použijete PHP vytvořit TwiML.

Na následující stránce PHP výsledkem TwiML odpověď, že **Vítáme** hovoru.

    <?php
        header("content-type: text/xml");
        echo "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n";
    ?>
    <Response>
        <Say>Hello world.</Say>
    </Response>

Jak vidíte v předchozím příkladu, odpověď TwiML je jednoduše dokumentu ve formátu XML. Knihovna Twilio pro PHP obsahuje třídy, které bude generovat TwiML za vás. Následující příklad vytváří odpovídající odpověď uvedené výše, ale používá **služby\_Twilio\_Twiml** třídy v knihovně Twilio PHP:

    require_once('Services/Twilio.php');

    $response = new Services_Twilio_Twiml();
    $response->say("Hello world.");
    print $response;

Další informace o TwiML najdete v tématu [https://www.twilio.com/docs/api/twiml][twiml_reference].

Až budete mít stránku PHP nastavena na poskytnout TwiML odpovědi, použijte adresu URL PHP stránky jako adresu URL předaným `Services_Twilio->account->calls->create` metody. Například pokud pracujete s webovou aplikací s názvem **MyTwiML** používaný Azure hostované služby a název stránky PHP **mytwiml.php**, může být předán adresu URL **Services_Twilio -> účet -> hovory -> vytvořit** uvedeno v následujícím příkladu:

    require_once 'Services/Twilio.php';

    $sid = "your_twilio_account_sid";
    $token = "your_twilio_authentication_token";
    $from_number = "NNNNNNNNNNN";
    $to_number = "NNNNNNNNNNN";
    $url = "http://<your_hosted_service>.cloudapp.net/MyTwiML/mytwiml.php";

    // The phone message text.
    $message = "Hello world.";

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    try
    {
        $call = $client->account->calls->create(
            $from_number,
            $to_number,
            $url.'?Message='.urlencode($message)
        );
    }
    catch (Exception $e)
    {
        echo 'Error: ' . $e->getMessage();
    }

Další informace o používání Twilio v Azure s PHP, přečtěte si, [jak provádět Twilio pomocí telefonního hovoru v aplikaci PHP na Azure][howto_phonecall_php].

## <a id="AdditionalServices"></a>Postup: pomocí dalších Twilio služby
Kromě příklady ukazuje tato část nabízí Twilio rozhraní API webových, která můžete využít další funkce Twilio Azure aplikace. Úplné podrobnosti najdete v [dokumentaci k rozhraní API Twilio] [twilio_api_documentation].

## <a id="NextSteps"></a>Další kroky
Teď, když jste se naučili základní informace o službě Twilio, tyto odkazy vedou na další informace:

* [Pokyny pro Twilio zabezpečení] [twilio_security_guidelines]
* [Vodítka Twilio postupy a příklad] [twilio_howtos]
* [Výukové programy pro rychlý úvod Twilio][twilio_quickstarts]
* [Twilio na GitHub] [twilio_on_github]
* [Obraťte se na podporu Twilio] [twilio_support]

[twilio_php]: https://github.com/twilio/twilio-php
[twilio_lib_docs]: http://readthedocs.org/docs/twilio-php/en/latest/index.html
[twilio_github_readme]: https://github.com/twilio/twilio-php/blob/master/README.md
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html

[howto_phonecall_php]: http://windowsazure.com/documentation/articles/partner-twilio-php-make-phone-call
[twimlet_message_url]: http://twimlets.com/message
[twimlet_message_url_hello_world]: http://twimlets.com/message?Message%5B0%5D=Hello%20World
[twiml_reference]: https://www.twilio.com/docs/api/twiml
[twilio_pricing]: http://www.twilio.com/pricing

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
