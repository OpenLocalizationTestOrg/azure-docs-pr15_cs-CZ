<properties 
    pageTitle="Používání e-mailové služby SendGrid (PHP) | Microsoft Azure" 
    description="Zjistěte, jak odeslat e-mailu s e-mailové služby SendGrid na Azure. Ukázky napsané v PHP." 
    documentationCenter="php" 
    services="" 
    manager="sendgrid" 
    editor="mollybos" 
    authors="thinkingserious"/>

<tags 
    ms.service="multiple" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="PHP" 
    ms.topic="article" 
    ms.date="10/30/2014" 
    ms.author="elmer.thomas@sendgrid.com; erika.berkland@sendgrid.com; vibhork; matt.bernier@sendgrid.com"/>
# <a name="how-to-use-the-sendgrid-email-service-from-php"></a>Jak používat službu SendGrid e-mailu z PHP

Tato příručka ukazuje, jak provádět běžné úlohy programování s SendGrid e-mailové služby Azure. Vzorky jsou napsané v PHP.
Scénáře nichž se uplatní zahrnují **vytváření e-mailu**, **odeslání e-mailu**a **Přidání přílohy**. Další informace o SendGrid a odesílání e-mailu naleznete v části [Další kroky](#next-steps) .

## <a name="what-is-the-sendgrid-email-service"></a>Co je e-mailové služby SendGrid?

SendGrid je [cloudového e-mailová služba] , která poskytuje spolehlivé [doručování transakční e-mailů], škálovatelnost a v reálném čase analýzy spolu s flexibilní rozhraní API, která snadnou vlastní integrace. Obvyklé scénáře použití SendGrid patří:

-   Automatické odesílání oznámení zákazníkům
-   Správa distribučních seznamů pro odesílání zákazníci měsíční e letáků a speciální nabídky
-   Shromažďování v reálném čase metriky pro činnosti, jako blokovaných e-mailu a rychlostí reakce zákazníka
-   Generování sestav k identifikaci trendů
-   Přesměrování dotazy zákazníků
- E-mailová oznámení z aplikace

Další informace najdete v tématu [https://sendgrid.com][].

## <a name="create-a-sendgrid-account"></a>Vytvoření účtu SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../includes/sendgrid-sign-up.md)]

## <a name="using-sendgrid-from-your-php-application"></a>Použití SendGrid z aplikace PHP

Použití SendGrid v aplikaci Azure PHP vyžaduje žádné zvláštní konfigurace nebo kódování. Protože SendGrid je služba, k němu přesně stejným způsobem z aplikace cloudu jako ho můžete z místního aplikace.

## <a name="how-to-send-an-email"></a>Postup: odeslání e-mailu

Můžete poslat e-mailu pomocí SMTP nebo rozhraní API webových poskytovanou SendGrid.

### <a name="smtp-api"></a>ROZHRANÍ API SMTP

Pokud chcete poslat e-mailu pomocí rozhraní API SendGrid SMTP, pomocí *Swift poštovní*, knihovnu na základě součásti pro odesílání e-mailů z PHP aplikací. Knihovna *Swift poštovní* si můžete stáhnout z [http://swiftmailer.org/download][] v5.3.0 (použijte [autora] nainstalovat Swift poštovní). Odeslání e-mailu s knihovnou zahrnuje vytvoření instancí <span class="auto-style2">Swift\_SmtpTransport</span>, <span class="auto-style2">Swift\_poštovní</span>, a <span class="auto-style2">Swift\_zprávy</span> tříd nastavení příslušné vlastnosti a volání <span class="auto-style2">Swift\_Mailer::send</span> metody.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */ 
     $text = "Hi!\nHow are you?\n";
     $html = "<html>
           <head></head>
           <body>
               <p>Hi!<br>
                   How are you?<br>
               </p>
           </body>
           </html>";
     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     // Email recipients
     $to = array(
           'john@contoso.com'=>'Destination 1 Name',
           'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';

     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);

     // Create a message (subject)
     $message = new Swift_Message($subject);

     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
         // This will let us know how many users received this message
         echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
         echo "Something went wrong - ";
         print_r($failures);
     }

### <a name="web-api"></a>Webového rozhraní API

Odeslání e-mailu pomocí rozhraní API webových SendGrid pomocí PHP jeho [otáčení (funkce)][] .

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD'; 

     $params = array(
          'api_user' => $user,
          'api_key' => $pass,
          'to' => 'john@contoso.com',
          'subject' => 'testing from curl',
          'html' => 'testing body',
          'text' => 'testing body',
          'from' => 'anna@contoso.com',
       );
       
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

Rozhraní API webových společnosti SendGrid je velmi podobné rozhraní REST API, když není opravdu rozhraní API RESTful vzhledem k tomu, ve většině hovory, jak získat a příspěvek slovesa mohou sloužit jako synonyma.

## <a name="how-to-add-an-attachment"></a>Postup: Přidání přílohy

### <a name="smtp-api"></a>ROZHRANÍ API SMTP

Odeslání přílohy pomocí rozhraní API SMTP vyžaduje jeden další řádek kódu příklad skriptu pro odesílání e-mailu pomocí poštovní Swift.

    <?php
     include_once "vendor/autoload.php";
     /*
      * Create the body of the message (a plain-text and an HTML version).
      * $text is your plain-text email
      * $html is your html version of the email
      * If the reciever is able to view html emails then only the html
      * email will be displayed
      */
     $text = "Hi!\nHow are you?\n";
      $html = "<html>
          <head></head>
          <body>
             <p>Hi!<br>
                How are you?<br>
             </p>
          </body>
          </html>";

     // This is your From email address
     $from = array('someone@example.com' => 'Name To Appear');
     
     // Email recipients
     $to = array(
          'john@contoso.com'=>'Destination 1 Name',
          'anna@contoso.com'=>'Destination 2 Name'
     );
     // Email subject
     $subject = 'Example PHP Email';
     
     // Login credentials
     $username = 'yoursendgridusername';
     $password = 'yourpassword';
     
     // Setup Swift mailer parameters
     $transport = Swift_SmtpTransport::newInstance('smtp.sendgrid.net', 587);
     $transport->setUsername($username);
     $transport->setPassword($password);
     $swift = Swift_Mailer::newInstance($transport);
     
     // Create a message (subject)
     $message = new Swift_Message($subject);
     
     // attach the body of the email
     $message->setFrom($from);
     $message->setBody($html, 'text/html');
     $message->setTo($to);
     $message->addPart($text, 'text/plain');
     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName("file_name"));
     
     // send message 
     if ($recipients = $swift->send($message, $failures))
     {
          // This will let us know how many users received this message
          echo 'Message sent out to '.$recipients.' users';
     }
     // something went wrong =(
     else
     {
          echo "Something went wrong - ";
          print_r($failures);
     }

Na další řádek kódu vypadá takto:

     $message->attach(Swift_Attachment::fromPath("path\to\file")->setFileName('file_name'));

Tento řádek kódu metodu připojit vyzývá <span class="auto-style2">Swift\_zprávy</span> objektu a bude použit statickou metodu <span class="auto-style2">fromPath</span> <span class="auto-style2">Swift\_přílohu</span> třídy zaujmout a připojení souboru ke zprávě.

### <a name="web-api"></a>Webového rozhraní API

Odeslání přílohy pomocí rozhraní API webových je velmi podobný odesílání e-mailů pomocí rozhraní API webových. Však platí, že v následujícím příkladu, musí obsahovat pole parametru: Tento element:

    'files['.$fileName.']' => '@'.$filePath.'/'.$fileName

Příklad:

    <?php

     $url = 'https://api.sendgrid.com/';
     $user = 'USERNAME';
     $pass = 'PASSWORD';
     
     $fileName = 'myfile';
     $filePath = dirname(__FILE__);

     $params = array(
         'api_user' => $user,
         'api_key' => $pass,
         'to' =>'john@contoso.com',
         'subject' => 'test of file sends',
         'html' => '<p> the HTML </p>',
         'text' => 'the plain text',
         'from' => 'anna@contoso.com',
         'files['.$fileName.']' => '@'.$filePath.'/'.$fileName
     );
     
     print_r($params);
     
     $request = $url.'api/mail.send.json';
     
     // Generate curl request
     $session = curl_init($request);
     
     // Tell curl to use HTTP POST
     curl_setopt ($session, CURLOPT_POST, true);
     
     // Tell curl that this is the body of the POST
     curl_setopt ($session, CURLOPT_POSTFIELDS, $params);
     
     // Tell curl not to return headers, but do return the response
     curl_setopt($session, CURLOPT_HEADER, false);
     curl_setopt($session, CURLOPT_RETURNTRANSFER, true);
     
     // obtain response
     $response = curl_exec($session);
     curl_close($session);
     
     // print everything out
     print_r($response);

## <a name="how-to-use-filters-to-enable-footers-tracking-and-analytics"></a>Postup: pomocí filtrů povolit zápatí, sledování a technologie pro analýzu

SendGrid poskytuje funkce dalších e-mailových použitím "filtry". To je nastavení, které můžete přidat e-mailové zprávě povolte určité funkce například povolení klikněte na sledování, Google analytics, předplatné sledování a tak dál.

Filtry můžete použít ke zprávě tak, že pomocí filtrů vlastnosti. Jednotlivé filtry nastavil hash obsahující nastavení specifické pro filtr. Následující příklad umožňuje Filtr zápatí a určuje textovou zprávu, která budou přidána do dolní části e-mailu.
V tomto příkladu použijeme [sendgrid php knihovny].
Použijte [autora] nainstalovat knihovny:
    
    php composer.phar require sendgrid/sendgrid 2.1.1

Příklad:    

    <?php
     /*
      * This example is used for sendgrid-php V2.1.1 (https://github.com/sendgrid/sendgrid-php/tree/v2.1.1)
      */
     include "vendor/autoload.php";

     $email = new SendGrid\Email();
     // The list of addresses this message will be sent to
     // [This list is used for sending multiple emails using just ONE request to SendGrid]
     $toList = array('john@contoso.com', 'anna@contoso.com');

     // Specify the names of the recipients
     $nameList = array('Name 1', 'Name 2');

     // Used as an example of variable substitution
     $timeList = array('4 PM', '5 PM');

     // Set all of the above variables
     $email->setTos($toList);
     $email->addSubstitution('-name-', $nameList);
     $email->addSubstitution('-time-', $timeList);

     // Specify that this is an initial contact message
     $email->addCategory("initial");

     // You can optionally setup individual filters here, in this example, we have 
     // enabled the footer filter
     $email->addFilter('footer', 'enable', 1);
     $email->addFilter('footer', "text/plain", "Thank you for your business");
     $email->addFilter('footer', "text/html", "Thank you for your business");

     // The subject of your email
     $subject = 'Example SendGrid Email';

     // Where is this message coming from. For example, this message can be from 
     // support@yourcompany.com, info@yourcompany.com
     $from = 'someone@example.com';

     // If you do not specify a sender list above, you can specifiy the user here. If 
     // a sender list IS specified above, this email address becomes irrelevant.
     $to = 'john@contoso.com';

     # Create the body of the message (a plain-text and an HTML version). 
     # text is your plain-text email 
     # html is your html version of the email
     # if the receiver is able to view html emails then only the html
     # email will be displayed

     /*
      * Note the variable substitution here =)
      */
     $text = "
     Hello -name-,
     Thank you for your interest in our products. We have set up an appointment to call you at -time- EST to discuss your needs in more detail.
     Regards,
     Fred";

     $html = "
     <html> 
     <head></head>
     <body>
     <p>Hello -name-,<br>
     Thank you for your interest in our products. We have set up an appointment
     to call you at -time- EST to discuss your needs in more detail.

     Regards,

     Fred<br>
     </p>
     </body>
     </html>";

     // set subject
     $email->setSubject($subject);

     // attach the body of the email
     $email->setFrom($from);
     $email->setHtml($html);
     $email->addTo($to);
     $email->setText($text);

     // Your SendGrid account credentials
     $username = 'sendgridusername@yourdomain.com';
     $password = 'example';

     // Create SendGrid object
     $sendgrid = new SendGrid($username, $password);

     // send message
     $response = $sendgrid->send($email);

     print_r($response);

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy SendGrid e-mailové služby, tyto odkazy vedou na další informace.

-   SendGrid si přečtěte následující dokumentaci: <https://sendgrid.com/docs>
-   Knihovna SendGrid PHP: <https://github.com/sendgrid/sendgrid-php>
-   SendGrid zvláštní nabídkou pro zákazníky, Azure: <https://sendgrid.com/windowsazure.html>

Další informace najdete v tématu taky [Středisko pro vývojáře PHP](/develop/php/).


  [https://sendgrid.com]: https://sendgrid.com
  [https://sendgrid.com/transactional-email/pricing]: https://sendgrid.com/transactional-email/pricing
  [special offer]: https://www.sendgrid.com/windowsazure.html
  [Packaging and Deploying PHP Applications for Azure]: http://msdn.microsoft.com/library/windowsazure/hh674499(v=VS.103).aspx
  [http://swiftmailer.org/download]: http://swiftmailer.org/download
  [otočení (funkce)]: http://php.net/curl
  [cloudového e-mailové služby]: https://sendgrid.com/email-solutions
  [doručování transakční e-mailů]: https://sendgrid.com/transactional-email
  [sendgrid php knihovny]: https://github.com/sendgrid/sendgrid-php/tree/v2.1.1
  [Autora]: https://getcomposer.org/download/
