<properties
    pageTitle="Jak někomu zavolat z Twilio (PHP) | Microsoft Azure"
    description="Zjistěte, jak chcete někomu zavolat a odeslání zprávy SMS ke službě rozhraní API Twilio na Azure. Vzorky jsou PHP aplikace."
    documentationCenter="php"
    services=""
    authors="devinrader"
    manager="twilio"
    editor="mollybos"/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="PHP"
    ms.topic="article"
    ms.date="11/25/2014"
    ms.author="microsofthelp@twilio.com"/>

# <a name="how-to-make-a-phone-call-using-twilio-in-a-php-application-on-azure"></a>Jak někomu zavolat pomocí Twilio aplikaci PHP na Azure

Následující příklad ukazuje, jak můžete Twilio uskutečnit hovor z webové stránky PHP použitý ve Azure. Výsledné aplikace se výzva k zadání hodnot telefonní hovor, jak je vidět na následující obrazovce snímek.

![Použití Twilio a PHP formuláře Azure hovoru][twilio_php]

Musíte použít kód v tomto tématu následujícím způsobem:

1. Získání účtu Twilio a ověřování tokenu. Začínáme s Twilio, jsou vyhodnoceny ceny za [http://www.twilio.com/pricing][twilio_pricing]. Můžou zaregistrovat ke zkušební verzi účtu na [https://www.twilio.com/try-twilio][try_twilio]. Informace o rozhraní API poskytovanou Twilio najdete v tématu [http://www.twilio.com/api][twilio_api].
2. Získání [Twilio v knihovně PHP](https://github.com/twilio/twilio-php) nebo když si nainstalujete jako balíček HRUŠNÍ. Další informace najdete v tématu [souboru readme](https://github.com/twilio/twilio-php/blob/master/README.md).
3. Instalace Azure SDK PHP. Přehled SDK a pokyny k instalaci naleznete v tématu [Nastavení SDK Azure pro PHP][setup_php_sdk].

## <a name="create-a-web-form-for-making-a-call"></a>Vytvoření webového formuláře pro volání

Následující kód HTML ukazuje, jak vytvořit web (**callform.html**), která vrací uživatelská data týkající se uplatňování hovoru:

    <html>
    <head>
        <title>Automated call form</title>
    </head>
    <body>
    <h1>Automated Call Form</h1>
    <p>Fill in all fields and click <b>Make this call</b>.</p>
    <form action="makecall.php" method="post">
    <table>
        <tr>
            <td>To:</td>
            <td><input type="text" size=50 name="callTo" value=""></td>
        </tr>
        <tr>
            <td>From:</td>
            <td><input type="text" size=50 name="callFrom" value=""></td>
        </tr>
        <tr>
            <td>Call message:</td>
            <td><input type="text" size=100 name="callText" value="Hello. This is the call text. Good bye." /></td>
        </tr>
        <tr>
            <td colspan=2><input type="submit" value="Make this call"></td>
        </tr>
    </table>
    </form>
    <br/>
    </body>
    </html>

## <a name="create-the-code-to-make-the-call"></a>Vytvoření kódu pro volání
Následující kód ukazuje, jak vytvořit web (**makecall.php**) nazývanou při odeslání formuláře zobrazením **callform.html**. Kód ukázáno v následujícím příkladu vytvoří zprávu volání a vygeneruje hovor. (Použijte svůj účet Twilio a ověřování token místo hodnot zástupný symbol přiřazené **$sid** a **$token** v následujícím kódu.)

    <html>
    <head><title>Making call...</title></head>
    <body>
    <p>Your call is being made.</p>

    <?php
    require_once 'Services/Twilio.php';

    $sid = "your_account_sid";
    $token = "your_authentication_token";

    $from_number = $_POST['callFrom']; // Calls must be made from a registered Twilio number.
    $to_number = $_POST['callTo'];
    $message = $_POST['callText'];

    $client = new Services_Twilio($sid, $token, "2010-04-01");

    $call = $client->account->calls->create(
        $from_number,
        $to_number,
        'http://twimlets.com/message?Message='.urlencode($message)
    );

    echo "Call status: ".$call->status."<br />";
    echo "URI resource: ".$call->uri."<br />";
    ?>
    </body>
    </html>

Kromě volání, zobrazuje **makecall.php** některé volání metadata (třeba ukazuje obrázek níže). Další informace o hovoru metadata najdete v tématu [https://www.twilio.com/docs/api/rest/call#instance-properties][twilio_call_properties].

![Použití Twilio a PHP odpověď Azure volání][twilio_php_response]

## <a name="run-the-application"></a>Spusťte aplikaci
Dalším krokem je pro nasazení aplikace na weby Azure. V následujících článcích obsahovat informace o vytváření webu a nasazení kódu libovolná, serveru FTP nebo WebMatrix (když není všechny informace v jednotlivých článku platí):

* [Umožňuje vytvořit web PHP MySQL Azure a nasazení pomocí libovolná][website-git]
* [Vytvoření webu Azure PHP MySQL a nasazení pomocí protokolu FTP][website-ftp]

## <a name="next-steps"></a>Další kroky
Tento kód získali zobrazit základní funkce pomocí Twilio v PHP na Azure. Než nasadíte Azure ve výrobním, můžete přidat další zpracování chyb nebo jiných funkcí. Příklad:

* Místo použití webového formuláře, můžete použít Azure úložiště objektů BLOB nebo databáze SQL ukládat telefonní čísla a hovoru textem. Informace o používání objektů BLOB Azure úložiště v PHP najdete v tématu [Použití Azure úložného prostoru s aplikacemi PHP][howto_blob_storage_php]. Informace o používání databáze SQL v PHP najdete v tématu [Použití databáze SQL s aplikacemi PHP][howto_sql_azure_php].
* Kód **makecall.php** používá Twilio-za předpokladu, že adresa URL ([http://twimlets.com/message][twimlet_message_url]) můžete zadat Twilio Markup Language (TwiML) odpověď informující Twilio jak pokračovat v hovoru. Například může obsahovat TwiML, je vrácená `<Say>` slovesné, jehož výsledkem text přehrát nahlas volání příjemci. Místo pomocí adresy URL poskytnutá Twilio, může vytvářet vlastní služby pro odpověď na požadavek na Twilio; Další informace najdete v tématu [jak Twilio použití hlasu a funkce služby SMS v PHP][howto_twilio_voice_sms_php]. Další informace o TwiML můžete najít na [http://www.twilio.com/docs/api/twiml][twiml]a další informace o `<Say>` a další akce Twilio, najdete na [http://www.twilio.com/docs/api/twiml/say][twilio_say].
* Přečíst pokyny pro Twilio zabezpečení na [https://www.twilio.com/docs/security][twilio_docs_security].

Další informace o Twilio najdete v tématu [https://www.twilio.com/docs][twilio_docs].

## <a name="see-also"></a>Viz taky
* [Jak používat Twilio pro hlasovou a možnosti služby SMS PHP](partner-twilio-php-how-to-use-voice-sms.md)

[twilio_pricing]: http://www.twilio.com/pricing
[try_twilio]: http://www.twilio.com/try-twilio
[twilio_api]: http://www.twilio.com/api
[verify_phone]: https://www.twilio.com/user/account/phone-numbers/verified#
[setup_php_sdk]: http://azurephp.interoperabilitybridges.com/articles/setup-the-windows-azure-sdk-for-php
[twimlet_message_url]: http://twimlets.com/message
[twiml]: http://www.twilio.com/docs/api/twiml
[twilio_api_service]: http://api.twilio.com
[build_php_azure_app]: http://azurephp.interoperabilitybridges.com/articles/build-and-deploy-a-windows-azure-php-application
[howto_twilio_voice_sms_php]: partner-twilio-php-how-to-use-voice-sms.md
[howto_blob_storage_php]: http://azure.microsoft.com/documentation/articles/storage-php-how-to-use-blobs/
[howto_sql_azure_php]: http://azure.microsoft.com/documentation/articles/sql-database-php-how-to-use/
[twilio_call_properties]: https://www.twilio.com/docs/api/rest/call#instance-properties
[twilio_docs_security]: http://www.twilio.com/docs/security
[twilio_docs]: http://www.twilio.com/docs
[twilio_say]: http://www.twilio.com/docs/api/twiml/say
[ssl_validation]: http://readthedocs.org/docs/twilio-php/en/latest/usage/rest.html
[twilio_php]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPCallForm.jpg
[twilio_php_response]: ./media/partner-twilio-php-make-phone-call/WA_TwilioPHPMakeCall.jpg
[website-git]: ./web-sites/web-sites-php-mysql-deploy-use-git.md
[website-ftp]: ./web-sites/web-sites-php-mysql-deploy-use-ftp.md
[twilio_php_github]: https://github.com/twilio/twilio-php
