<properties 
    pageTitle="Používání e-mailové služby SendGrid (.NET) | Microsoft Azure" 
    description="Zjistěte, jak odeslat e-mailu s e-mailové služby SendGrid na Azure. Kód ukázky napsané v jazyce C# a pomocí rozhraní API .NET." 
    services="app-service\web" 
    documentationCenter=".net" 
    authors="thinkingserious" 
    manager="dwrede" 
    editor=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="dotnet" 
    ms.topic="article" 
    ms.date="01/14/2016" 
    ms.author="team-pi@sendgrid.com"/>





# <a name="how-to-send-email-using-sendgrid-with-azure"></a>Jak odeslat e-mailu pomocí SendGrid s Azure


## <a name="overview"></a>Základní informace

Tato příručka ukazuje, jak provádět běžné úlohy programování s SendGrid e-mailové služby Azure. Vzorky jsou napsané v jazyce C\#
a pomocí rozhraní API .NET. Scénáře nichž se uplatní zahrnují **vytváření e-mailu**, **odeslání e-mailu**, **Přidání přílohy**a **použitím filtrů**. Další informace o SendGrid a odesílání e-mailu naleznete v části [Další kroky][] .

## <a name="what-is-the-sendgrid-email-service"></a>Co je e-mailové služby SendGrid?

SendGrid je [cloudového e-mailová služba] , která poskytuje spolehlivé [doručování transakční e-mailů], škálovatelnost a v reálném čase analýzy spolu s flexibilní rozhraní API, která snadnou vlastní integrace. Obvyklé scénáře použití SendGrid patří:

-   Automatické odesílání oznámení pro zákazníky.
-   Správa distribučních seznamů pro odesílání zákazníci měsíční e letáků a speciální nabídky.
-   Shromažďování v reálném čase metriky pro činnosti, jako blokovaných e-mailu a rychlostí reakce zákazníka.
-   Generování sestav pomůže identifikovat trendy.
-   Přesměrování dotazy zákazníků.
-   Zpracovávání příchozích e-mailů.

Další informace najdete v tématu [https://sendgrid.com](https://sendgrid.com) nebo naše [C# knihovny][sendgrid csharp]

## <a name="create-a-sendgrid-account"></a>Vytvoření účtu SendGrid

[AZURE.INCLUDE [sendgrid-sign-up](../../includes/sendgrid-sign-up.md)]

## <a name="reference-the-sendgrid-net-class-library"></a>Odkaz knihovna tříd SendGrid .NET

[Balíček SendGrid NuGet](https://www.nuget.org/packages/Sendgrid) je nejjednodušší získat rozhraní API SendGrid a nakonfigurovat aplikaci všechny závislosti. NuGet je rozšíření Visual Studia zahrnutý v sadě Microsoft Visual Studio 2015, který usnadňuje nainstalovat a aktualizovat knihoven a nástroje. 

> [AZURE.NOTE] Pokud chcete nainstalovat NuGet, pokud jsou spuštěné verze dřívější než Visual Studio 2015 aplikace Visual Studio, navštěvujte blog o [http://www.nuget.org](http://www.nuget.org)a klikněte na tlačítko **Nainstalovat NuGet** .

K instalaci SendGrid NuGet balíčku aplikace, postupujte takto:

1.  Vytvoření nového projektu.

    ![Vytvoření nového projektu][create-new-project]

2.  Vyberte šablonu.

    ![Vyberte šablonu][select-a-template]

3.  V **Okně Průzkumník řešení**klikněte pravým tlačítkem na **odkazy**a potom klikněte na **Spravovat balíčků NuGet**.

4.  Vyhledejte **SendGrid** a vyberte položku **SendGrid** v seznamu výsledků.

    ![SendGrid NuGet balíčku][SendGrid-NuGet-package]

5.  Klikněte na tlačítko **nainstalovat** dokončete instalaci a potom zavřete dialogové okno.

Knihovna tříd na SendGrid .NET se nazývá **SendGridMail**. Obsahuje následující obory názvů:

-   **SendGridMail** pro vytváření grafů a práci s e-mailové zprávy.
-   **SendGridMail.Transport** k odesílání e-mailu pomocí protokolu **SMTP** nebo protokolu HTTP 1.1 s **Webu nebo ostatní**.

Přidat následující kód deklarace oboru názvů do horní části všech C\# soubor, ve kterém chcete programově přístup k e-mailové služby SendGrid.
**System.Net** a **System.Net.Mail** jsou .NET Framework obory názvů, které jsou však započítávány, protože jsou to například typů, které se běžně pomocí rozhraní API SendGrid.

    using System;
    using System.Net;
    using System.Net.Mail;
    using SendGrid;

## <a name="how-to-create-an-email"></a>Postup: vytvoření e-mailu

Pomocí objektu **SendGridMessage** -mailovou zprávu vytvoříte. Po vytvoření objekt zprávy můžete nastavit vlastnosti a metody, včetně odesílatele e-mailu, e-mailu příjemce a předmět a text e-mailu.

Následující příklad ukazuje, jak vytvořit objekt plně vyplněné e-mailu:

    // Create the email object first, then add the properties.
    var myMessage = new SendGridMessage();

    // Add the message properties.
    myMessage.From = new MailAddress("john@example.com");

    // Add multiple addresses to the To field.
    List<String> recipients = new List<String>
    {
        @"Jeff Smith <jeff@example.com>",
        @"Anna Lidman <anna@example.com>",
        @"Peter Saddow <peter@example.com>"
    };

    myMessage.AddTo(recipients);

    myMessage.Subject = "Testing the SendGrid Library";

    //Add the HTML and Text bodies
    myMessage.Html = "<p>Hello World!</p>";
    myMessage.Text = "Hello World plain text!";

Další informace o vlastnostech a způsobů podporovaný typ **SendGrid** najdete v článku [sendgrid csharp][] na GitHub.

## <a name="how-to-send-an-email"></a>Postup: odeslání e-mailu

Po vytvoření e-mailové zprávy, můžete ji odešlete pomocí rozhraní API webových poskytovanou SendGrid. Můžete také použít [. Na čistého integrované v knihovně](https://sendgrid.com/docs/Code_Examples/csharp.html).

Odeslání e-mailu vyžaduje zadat SendGrid rozhraní API klávesy nebo přihlašovací údaje účtu SendGrid (uživatelské jméno a heslo). Rozhraní API klíč je upřednostňovaná metoda. Pokud potřebujete zobrazit podrobnosti o tom, jak konfigurovat rozhraní API klíčů, navštivte naše si přečtěte následující [dokumentaci](https://sendgrid.com/docs/Classroom/Send/api_keys.html)

Kliknutí na KONFIGUROVAT a přidejte klíč/dvojice v části "nastavení aplikace" mohou obsahovat těchto přihlašovacích údajů prostřednictvím portálu Azure.

 ![Nastavení aplikace Azure][azure_app_settings]

 Potom se může k nim dostat takto: 
    
    var username = System.Environment.GetEnvironmentVariable("SENDGRID_USERNAME"); 
    var pswd = System.Environment.GetEnvironmentVariable("SENDGRID_PASSWORD");
    var apiKey = System.Environment.GetEnvironmentVariable("SENDGRID_APIKEY");

Pomocí přihlašovacích údajů:
    
    // Create network credentials to access your SendGrid account
    var username = "your_sendgrid_username";
    var pswd = "your_sendgrid_password";

    var credentials = new NetworkCredential(username, pswd);
    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

Pomocí rozhraní API klíče:

    var apiKey = "your_sendgrid_api_key";  
    // create a Web transport, using API Key
    var transportWeb = new Web(apiKey);


Následující příklady ukazují, jak odeslat zprávu pomocí rozhraní API webových.

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Create credentials, specifying your user name and password.
    var credentials = new NetworkCredential("username", "password");

    // Create an Web transport for sending email.
    var transportWeb = new Web(credentials);

    // Send the email, which returns an awaitable task.
    transportWeb.DeliverAsync(myMessage);

    // If developing a Console Application, use the following
    // transportWeb.DeliverAsync(mail).Wait();

## <a name="how-to-add-an-attachment"></a>Postup: Přidání přílohy

Přílohy můžete přidaný do zprávy volání metody **AddAttachment** a zadáním názvu a cesty soubor, který chcete připojit.
Můžete zahrnout více příloh tak, že zavoláte tato metoda jednou pro každý soubor se chcete připojit. Následující příklad ukazuje, přidáváním přílohy do zprávy:

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    myMessage.AddAttachment(@"C:\file1.txt");
    
Můžete také přidat přílohy z datového **proudu**. Zavoláním na oddělení stejným způsobem jako nad **AddAttachment**, ale předáním toku dat lze provést a název souboru se má zobrazit jako zprávu. V tomto případě se musíte přidat knihovnu System.IO.

    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    using (var attachmentFileStream = new FileStream(@"C:\file.txt", FileMode.Open))
    {
        myMessage.AddAttachment(attachmentFileStream, "My Cool File.txt");
    }


## <a name="how-to-use-apps-to-enable-footers-tracking-and-analytics"></a>Postup: pomocí aplikace povolit zápatí, sledování a technologie pro analýzu

SendGrid poskytuje funkce další e-mailu pomocí aplikace. To je nastavení, které můžete přidat e-mailové zprávě povolte určité funkce například klikněte na příkaz sledování, Google analytics, předplatné sledování, a tak dál. Úplný seznam aplikací najdete v tématu [Nastavení aplikace][].

Aplikace můžete použije **SendGrid** e-mailových zpráv pomocí metody implementovaná v rámci třídy **SendGrid** .

Následující příklady ukazují, zápatí a klikněte na tlačítko Sledování filtry:

### <a name="footer"></a>Zápatí

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Text = "Hello World!";

    // Add a footer to the message.
    myMessage.EnableFooter("PLAIN TEXT FOOTER", "<p><em>HTML FOOTER</em></p>");

### <a name="click-tracking"></a>Klikněte na tlačítko Sledování

    // Create the email object first, then add the properties.
    SendGridMessage myMessage = new SendGridMessage();
    myMessage.AddTo("anna@example.com");
    myMessage.From = new MailAddress("john@example.com", "John Smith");
    myMessage.Subject = "Testing the SendGrid Library";
    myMessage.Html = "<p><a href=\"http://www.example.com\">Hello World Link!</a></p>";
    myMessage.Text = "Hello World!";
    
    // true indicates that links in plain text portions of the email 
    // should also be overwritten for link tracking purposes. 
    myMessage.EnableClickTracking(true);

## <a name="how-to-use-additional-sendgrid-services"></a>Postup: pomocí dalších službách SendGrid

SendGrid nabízí rozhraní API a webhooks využívající můžete využít další funkce SendGrid Azure aplikace založené na webu. Úplné podrobnosti najdete v [dokumentaci k rozhraní API SendGrid][].

## <a name="next-steps"></a>Další kroky

Teď, když jste se naučili základy SendGrid e-mailové služby, tyto odkazy vedou na další informace.

*   SendGrid C\# knihovny repo: [sendgrid csharp][]
*   Rozhraní API SendGrid si přečtěte následující dokumentaci: <https://sendgrid.com/docs>
*   SendGrid zvláštní nabídkou pro zákazníky, Azure: [https://sendgrid.com](https://sendgrid.com)

  [Další kroky]: #next-steps
  [What is the SendGrid Email Service?]: #whatis
  [Create a SendGrid Account]: #createaccount
  [Reference the SendGrid .NET Class Library]: #reference
  [How to: Create an Email]: #createemail
  [How to: Send an Email]: #sendemail
  [How to: Add an Attachment]: #addattachment
  [How to: Use Filters to Enable Footers, Tracking, and Analytics]: #usefilters
  [How to: Use Additional SendGrid Services]: #useservices
  
  [special offer]: https://www.sendgrid.com/windowsazure.html
  
  [create-new-project]: ./media/sendgrid-dotnet-how-to-send-email/create_new_project.png
  [select-a-template]: ./media/sendgrid-dotnet-how-to-send-email/select_a_template.png
  [SendGrid-NuGet-package]: ./media/sendgrid-dotnet-how-to-send-email/sendgrid_nuget.png
  [azure_app_settings]: ./media/sendgrid-dotnet-how-to-send-email/app_settings.png
  [sendgrid csharp]: https://github.com/sendgrid/sendgrid-csharp
  [SMTP vs. Web API]: https://sendgrid.com/docs/Integrate/index.html
  [Nastavení aplikace]: https://sendgrid.com/docs/API_Reference/SMTP_API/apps.html
  [Rozhraní API SendGrid si přečtěte následující dokumentaci]: https://sendgrid.com/docs
  
  [cloudového e-mailové služby]: https://sendgrid.com/email-solutions
  [doručování transakční e-mailů]: https://sendgrid.com/transactional-email
 
